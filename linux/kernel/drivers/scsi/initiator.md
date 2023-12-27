```c
scsi_transport_iscsi
iscsi_tcp
libiscsi
libiscsi_tcp

scsi_transport_iscsi.c
  iscsi_transport_init
    struct netlink_kernel_cfg cfg = {
      .groups = 1,
      .input  = iscsi_if_rx,
    };
    netlink_kernel_create(&init_net, NETLINK_ISCSI, &cfg)

  iscsi_if_rx
    iscsi_if_recv_msg(skb)
      struct iscsi_internal *priv = iscsi_if_transport_lookup(iscsi_ptr(ev->transport_handle))
      transport = priv->iscsi_transport // iscsi_transport
      switch (nlh->nlmsg_type) {
        case ISCSI_UEVENT_CREATE_SESSION:
          iscsi_if_create_session(priv)
            session = transport->create_session
        case ISCSI_UEVENT_CREATE_CONN:
          iscsi_if_create_conn(transport)
            session = iscsi_session_lookup(ev->u.c_conn.sid)
            conn = transport->create_conn(session, ev->u.c_conn.cid)
        case ISCSI_UEVENT_BIND_CONN:
          session = iscsi_session_lookup(ev->u.b_conn.sid)
          conn = iscsi_conn_lookup(ev->u.b_conn.sid, ev->u.b_conn.cid)
          transport->bind_conn(session, conn)

  iscsi_register_transport(struct iscsi_transport *tt)
    struct iscsi_internal *priv = kzalloc(sizeof(*priv), GFP_KERNEL)
    priv->iscsi_transport = tt // iscsi_transport, &iscsi_sw_tcp_transport
    priv->t.user_scan = iscsi_user_scan // /sys/devices/platform/hostX/scsi_host/hostX/scan
    priv->dev.class = &iscsi_transport_class // /sys/class/iscsi_transport
    dev_set_name(&priv->dev, "%s", tt->name)
    device_register(&priv->dev) // /sys/class/iscsi_transport/tcp
    transport_container_register(&priv->t.host_attrs) // /sys/class/iscsi_host
    transport_container_register(&priv->conn_cont) // /sys/class/iscsi_connection
    transport_container_register(&priv->session_cont) // /sys/class/iscsi_session

  iscsi_alloc_session(struct Scsi_Host *shost, struct iscsi_transport *iscsit, int dd_size)
    session = kzalloc(sizeof(*session) + dd_size, GFP_KERNEL)
    session->transport = iscsit // iscsi_transport
    device_initialize(&session->dev)

  iscsi_add_session(struct iscsi_cls_session *session, unsigned int target_id)
    session->sid = atomic_add_return(1, &iscsi_session_nr)
    dev_set_name(&session->dev, "session%u", session->sid);
    device_add(&session->dev)

  iscsi_create_conn(struct iscsi_cls_session *session, int dd_size, uint32_t cid)
    iscsi_transport *transport = session->transport
    iscsi_cls_conn *conn = kzalloc(sizeof(*conn) + dd_size, GFP_KERNEL)
    conn->transport = transport;
    conn->cid = cid;
    dev_set_name(&conn->dev, "connection%d:%u", session->sid, cid)
    device_register(&conn->dev)
      device_initialize(dev)
      device_add(dev)
    transport_register_device(&conn->dev)

hosts.c
  scsi_host_alloc(struct scsi_host_template *sht, int privsize)
    shost = kzalloc(sizeof(struct Scsi_Host) + privsize, gfp_mask)
    shost->host_no = atomic_inc_return(&scsi_host_next_hn) - 1
    shost->hostt = sht
    device_initialize(&shost->shost_gendev)
    dev_set_name(&shost->shost_gendev, "host%d", shost->host_no)
    device_initialize(&shost->shost_dev)
    dev_set_name(&shost->shost_dev, "host%d", shost->host_no)
    shost->ehandler = kthread_run(scsi_error_handler, shost)
    scsi_proc_hostdir_add(shost->hostt)

iscsi_tcp.c
  iscsi_sw_tcp_init
    iscsi_sw_tcp_scsi_transport = iscsi_register_transport(iscsi_sw_tcp_transport)

  iscsi_sw_tcp_session_create
    shost = iscsi_host_alloc(&iscsi_sw_tcp_sht)
    shost->transportt = iscsi_sw_tcp_scsi_transport // scsi_transport_template
    iscsi_host_add(shost, NULL)
    iscsi_session_setup(&iscsi_sw_tcp_transport, shost)

  iscsi_sw_tcp_conn_create(struct iscsi_cls_session *cls_session, uint32_t conn_idx)
    struct iscsi_cls_conn *cls_conn = iscsi_tcp_conn_setup(cls_session, sizeof(*tcp_sw_conn), conn_idx)

  static struct scsi_host_template iscsi_sw_tcp_sht = {
    .module     = THIS_MODULE,
    .name     = "iSCSI Initiator over TCP/IP",
    .queuecommand           = iscsi_queuecommand,
    .change_queue_depth = iscsi_change_queue_depth,
    .can_queue    = ISCSI_DEF_XMIT_CMDS_MAX - 1,
    .sg_tablesize   = 4096,
    .max_sectors    = 0xFFFF,
    .cmd_per_lun    = ISCSI_DEF_CMD_PER_LUN,
    .eh_abort_handler       = iscsi_eh_abort,
    .eh_device_reset_handler= iscsi_eh_device_reset,
    .eh_target_reset_handler = iscsi_eh_recover_target,
    .use_clustering         = DISABLE_CLUSTERING,
    .slave_alloc            = iscsi_sw_tcp_slave_alloc,
    .slave_configure        = iscsi_sw_tcp_slave_configure,
    .target_alloc   = iscsi_target_alloc,
    .proc_name    = "iscsi_tcp",
    .this_id    = -1,
  };

  static struct iscsi_transport iscsi_sw_tcp_transport = {
    .owner    = THIS_MODULE,
    .name     = "tcp",
    .caps     = CAP_RECOVERY_L0 | CAP_MULTI_R2T | CAP_HDRDGST | CAP_DATADGST,
    /* session management */
    .create_session   = iscsi_sw_tcp_session_create,
    .destroy_session  = iscsi_sw_tcp_session_destroy,
    /* connection management */
    .create_conn    = iscsi_sw_tcp_conn_create,
    .bind_conn    = iscsi_sw_tcp_conn_bind,
    .destroy_conn   = iscsi_sw_tcp_conn_destroy,
    .attr_is_visible  = iscsi_sw_tcp_attr_is_visible,
    .set_param    = iscsi_sw_tcp_conn_set_param,
    .get_conn_param   = iscsi_sw_tcp_conn_get_param,
    .get_session_param  = iscsi_session_get_param,
    .start_conn   = iscsi_conn_start,
    .stop_conn    = iscsi_sw_tcp_conn_stop,
    /* iscsi host params */
    .get_host_param   = iscsi_sw_tcp_host_get_param,
    .set_host_param   = iscsi_host_set_param,
    /* IO */
    .send_pdu   = iscsi_conn_send_pdu,
    .get_stats    = iscsi_sw_tcp_conn_get_stats,
    /* iscsi task/cmd helpers */
    .init_task    = iscsi_tcp_task_init,
    .xmit_task    = iscsi_tcp_task_xmit,
    .cleanup_task   = iscsi_tcp_cleanup_task,
    /* low level pdu helpers */
    .xmit_pdu   = iscsi_sw_tcp_pdu_xmit,
    .init_pdu   = iscsi_sw_tcp_pdu_init,
    .alloc_pdu    = iscsi_sw_tcp_pdu_alloc,
    /* recovery */
    .session_recovery_timedout = iscsi_session_recovery_timedout,
  };

libiscsi.c
  iscsi_host_alloc(struct scsi_host_template *sht)
    shost = scsi_host_alloc(sht)

  iscsi_host_add(struct Scsi_Host *shost, struct device *pdev)
    scsi_add_host(shost, pdev)
      scsi_add_host_with_dma(host, dev, dev)
        device_add(&shost->shost_gendev)
        device_add(&shost->shost_dev)
        scsi_sysfs_add_host(shost)
        scsi_proc_host_add(shost)

  iscsi_session_setup(struct iscsi_transport *iscsit, struct Scsi_Host *shost)
    cls_session = iscsi_alloc_session(shost, iscsit)
    iscsi_add_session(cls_session, 0)

  iscsi_conn_setup(struct iscsi_cls_session *cls_session, int dd_size, uint32_t conn_idx)
    iscsi_session *session = cls_session->dd_data
    cls_conn = iscsi_create_conn(cls_session, sizeof(*conn) + dd_size, conn_idx)
    conn = cls_conn->dd_data;
    conn->session = session;
    conn->cls_conn = cls_conn;

libiscsi_tcp.c
  iscsi_tcp_conn_setup(struct iscsi_cls_session *cls_session, int dd_data_size, uint32_t conn_idx)
    cls_conn = iscsi_conn_setup(cls_session, sizeof(*tcp_conn) + dd_data_size, conn_idx)
```
