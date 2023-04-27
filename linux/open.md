# open 

do_sys_open
    do_sys_openat2
        do_filp_open
            path_openat
                path_init
                    path_get
                        mntget # 给 mnt 增加引用计数
                            mnt_add_count 
                        dget # 给 dentry d_lockref 增加引用计数
