# 
SYSCALL_DEFINE2 umount
    ksys_umount
        do_umount
        path_umount
            mntput_no_expire
                cleanup_mnt
                    deactivate_super
                        deactivate_locked_super
                            fs->kill_sb
