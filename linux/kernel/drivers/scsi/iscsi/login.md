```c
struct iscsi_login_rsp {
	uint8_t status_class;	/* see Login RSP ststus classes below */
	uint8_t status_detail;	/* see Login RSP Status details below */
	uint8_t rsvd4[10];
};



struct iscsi_hdr {
	uint8_t		other[12];
};
```

```shell
main
    usr/idbm.c:idbm_create_rec(targetname, tpgt, ip, port, iface, 1);
        usr/idbm.c:idbm_node_setup_defaults(rec);
            usr/idbm.c:idbm_setup_session_defaults(rec);
    exec_node_op
        login_portals
            for_each_matched_rec
                __for_each_matched_rec
                    usr/idbm.c:idbm_for_each_rec
                        usr/idbm.c:idbm_for_each_node
                            usr/idbm.c:node_fn
                                usr/idbm.c:idbm_for_each_portal
                                    usr/idbm.c:portal_fn
                                        usr/idbm.c:idbm_for_each_iface
                                            usr/idbm.c:iface_fn
                                                usr/iscsiadm.c:rec_match_fn
                                                    usr/iscsiadm.c:link_recs
            usr/session_mgmt.c:iscsi_login_portals
                usr/session_mgmt.c:__iscsi_login_portals
                    usr/session_mgmt.c:iscsi_login_portal
                        usr/iscsi_sysfs.c:iscsi_sysfs_for_each_session
                        usr/session_mgmt.c:__iscsi_login_portal
                            usr/iscsid_req.c:iscsid_req_by_rec_async
                                usr/iscsid_req.c:iscsid_request
                usr/session_mgmt.c:__iscsi_login_portals
                    usr/session_mgmt.c:iscsid_login_reqs_wait
                        usr/iscsid_req.c:iscsid_req_wait
                            usr/iscsid_req.c:iscsid_response




#0  link_recs (data=0x7fffffffde00, rec=0x7fffffff9c90) at ../usr/iscsiadm.c:380
#1  0x00005555555ad1d1 in rec_match_fn (data=0x7fffffffdd80, rec=0x7fffffff9c90) at ../usr/iscsiadm.c:646
#2  0x000055555558e9ff in iface_fn (data=0x7fffffffdd20, rec=0x7fffffff9c90) at ../usr/idbm.c:1994
#3  0x000055555558e5d8 in idbm_for_each_iface (found=0x7fffffffdd78, data=0x7fffffffdd20, fn=0x55555558e9c8 <iface_fn>,
    targetname=0x7ffff72e1083 "iqn.2023-05.black.com:server", tpgt=1, ip=0x7ffff72c0083 "192.168.2.77", port=3260, ruw_lock=true)
    at ../usr/idbm.c:1891
#4  0x000055555558ea77 in portal_fn (found=0x7fffffffdd78, data=0x7fffffffdd20,
    targetname=0x7ffff72e1083 "iqn.2023-05.black.com:server", tpgt=1, ip=0x7ffff72c0083 "192.168.2.77", port=3260, ruw_lock=true)
    at ../usr/idbm.c:2008
#5  0x000055555558e84b in idbm_for_each_portal (found=0x7fffffffdd78, data=0x7fffffffdd20, fn=0x55555558ea01 <portal_fn>,
    targetname=0x7ffff72e1083 "iqn.2023-05.black.com:server", ruw_lock=true) at ../usr/idbm.c:1945
#6  0x000055555558ead0 in node_fn (found=0x7fffffffdd78, data=0x7fffffffdd20,
    targetname=0x7ffff72e1083 "iqn.2023-05.black.com:server", ruw_lock=true) at ../usr/idbm.c:2018
#7  0x000055555558e984 in idbm_for_each_node (found=0x7fffffffdd78, data=0x7fffffffdd20, fn=0x55555558ea8e <node_fn>, ruw_lock=true)
    at ../usr/idbm.c:1980
#8  0x000055555558eb41 in idbm_for_each_rec (found=0x7fffffffdd78, data=0x7fffffffdd80, fn=0x5555555ad145 <rec_match_fn>,
    ruw_lock=true) at ../usr/idbm.c:2029
#9  0x00005555555ad24e in __for_each_matched_rec (verbose=1, rec=0x5555555e7960, data=0x7fffffffde00, fn=0x5555555ac61d <link_recs>)
    at ../usr/iscsiadm.c:660
#10 0x00005555555ad2f2 in for_each_matched_rec (rec=0x5555555e7960, data=0x7fffffffde00, fn=0x5555555ac61d <link_recs>)
    at ../usr/iscsiadm.c:677
#11 0x00005555555ad342 in login_portals (pattern_rec=0x5555555e7960, wait=true) at ../usr/iscsiadm.c:686
#12 0x00005555555b265d in exec_node_op (ctx=0x5555555e7910, op=0, do_login=1, do_logout=0, do_show=0, do_rescan=0, do_stats=0,
    wait=true, info_level=-1, rec=0x5555555e7960, params=0x7fffffffe000) at ../usr/iscsiadm.c:2897
#13 0x00005555555b5072 in main (argc=8, argv=0x7fffffffe2b8) at ../usr/iscsiadm.c:4004
```
