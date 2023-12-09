# nfsd

## function_graph 案例

-  nfs nfsd4_create_file|nfsd4_remove
```shell
sysctl kernel.ftrace_enabled=1
dir=/sys/kernel/debug/tracing

<!-- 执行栈 -->
echo function_graph > ${dir}/current_tracer

<!-- <!-- 过滤指定函数 --> -->
<!-- echo nfsd4_remove > set_ftrace_filter -->

<!-- 过滤指定模块 -->
echo '*:mod:nfsd' > set_ftrace_filter

<!-- 启用 -->
echo 1 > ${dir}/tracing_on
<!-- 停用 -->
echo 0 > ${dir}/tracing_on

root@black:/sys/kernel/debug/tracing# cat trace
# tracer: function_graph
#
# CPU  DURATION                  FUNCTION CALLS
# |     |   |                     |   |   |   |
<!-- 创建文件 -->
 14)   5.750 us    |  nfsd_init_request [nfsd]();
 14)               |  nfsd_dispatch [nfsd]() {
 14)               |    nfs4svc_decode_compoundargs [nfsd]() {
 14)               |      nfsd4_decode_compound.constprop.0 [nfsd]() {
 14)   0.892 us    |        OPDESC [nfsd]();
 14)               |        nfsd4_decode_sequence [nfsd]() {
 14)   0.921 us    |          nfsd4_decode_sessionid4 [nfsd]();
 14)   2.785 us    |        }
 14)   0.891 us    |        nfsd4_cache_this_op [nfsd]();
 14)               |        nfsd4_max_reply [nfsd]() {
 14)   0.882 us    |          nfsd4_sequence_rsize [nfsd]();
 14)   2.895 us    |        }
 14)   0.871 us    |        OPDESC [nfsd]();
 14)               |        nfsd4_decode_putfh [nfsd]() {
 14)   0.892 us    |          svcxdr_savemem [nfsd]();
 14)   2.805 us    |        }
 14)   0.881 us    |        nfsd4_cache_this_op [nfsd]();
 14)               |        nfsd4_max_reply [nfsd]() {
 14)   0.882 us    |          nfsd4_only_status_rsize [nfsd]();
 14)   2.735 us    |        }
 14)   0.871 us    |        OPDESC [nfsd]();
 14)   0.882 us    |        nfsd4_cache_this_op [nfsd]();
 14)               |        nfsd4_max_reply [nfsd]() {
 14)   0.932 us    |          nfsd4_getattr_rsize [nfsd]();
 14)   2.765 us    |        }
 14) + 31.007 us   |      }
 14) + 32.881 us   |    }
 14)   1.112 us    |    nfsd_cache_lookup [nfsd]();
 14)               |    nfsd4_proc_compound [nfsd]() {
 14)   1.293 us    |      nfsd_minorversion [nfsd]();
 14)               |      nfsd4_sequence [nfsd]() {
 14)               |        find_in_sessionid_hashtbl [nfsd]() {
 14)   0.962 us    |          get_client_locked [nfsd]();
 14)   3.677 us    |        }
 14)   7.013 us    |      }
 14)               |      nfsd4_encode_operation [nfsd]() {
 14)   1.182 us    |        nfsd4_encode_sequence [nfsd]();
 14)   3.346 us    |      }
 14)   0.922 us    |      nfsd4_cstate_clear_replay [nfsd]();
 14)               |      nfsd4_putfh [nfsd]() {
 14)   0.912 us    |        fh_put [nfsd]();
 14)               |        fh_verify [nfsd]() {
 14)               |          nfsd_set_fh_dentry [nfsd]() {
 14)               |            rqst_exp_find [nfsd]() {
 14)               |              exp_find [nfsd]() {
 14)   2.555 us    |                exp_find_key [nfsd]();
 14)               |                exp_get_by_name [nfsd]() {
 14)   1.022 us    |                  svc_export_match [nfsd]();
 14)   3.997 us    |                  svc_export_upcall [nfsd]();
 14)   8.446 us    |                }
 14) + 13.916 us   |              }
 14) + 15.849 us   |            }
 14) + 21.740 us   |          }
 14)               |          nfsd_setuser_and_check_port [nfsd]() {
 14)   0.891 us    |            nfsexp_flags [nfsd]();
 14)   6.612 us    |            nfsd_setuser [nfsd]();
 14)   0.911 us    |            nfserrno [nfsd]();
 14) + 12.052 us   |          }
 14)   0.901 us    |          nfsd_permission [nfsd]();
 14) + 38.451 us   |        }
 14) + 42.087 us   |      }
 14)   0.902 us    |      clear_current_stateid [nfsd]();
 14)   0.901 us    |      check_nfsd_access [nfsd]();
 14)               |      nfsd4_encode_operation [nfsd]() {
 14)   0.881 us    |        nfsd4_encode_noop [nfsd]();
 14)   2.775 us    |      }
 14)   0.882 us    |      nfsd4_cstate_clear_replay [nfsd]();
 14)               |      nfsd4_getattr [nfsd]() {
 14)               |        fh_verify [nfsd]() {
 14)               |          nfsd_setuser_and_check_port [nfsd]() {
 14)   0.872 us    |            nfsexp_flags [nfsd]();
 14)   5.881 us    |            nfsd_setuser [nfsd]();
 14)   0.892 us    |            nfserrno [nfsd]();
 14) + 10.920 us   |          }
 14)   0.872 us    |          check_nfsd_access [nfsd]();
 14)   0.882 us    |          nfsd_permission [nfsd]();
 14) + 16.019 us   |        }
 14) + 17.713 us   |      }
 14)               |      nfsd4_encode_operation [nfsd]() {
 14)               |        nfsd4_encode_getattr [nfsd]() {
 14)               |          nfsd4_encode_fattr [nfsd]() {
 14)   0.931 us    |            nfsd4_encode_bitmap [nfsd]();
 14)   0.911 us    |            fsid_source [nfsd]();
 14)               |            nfsd4_encode_user [nfsd]() {
 14)   2.545 us    |              encode_name_from_id [nfsd]();
 14)   5.700 us    |            }
 14)               |            nfsd4_encode_group [nfsd]() {
 14)   1.302 us    |              encode_name_from_id [nfsd]();
 14)   3.106 us    |            }
 14) + 18.685 us   |          }
 14) + 20.528 us   |        }
 14) + 22.612 us   |      }
 14)   0.882 us    |      nfsd4_cstate_clear_replay [nfsd]();
 14)   1.192 us    |      fh_put [nfsd]();
 14)   0.911 us    |      fh_put [nfsd]();
 14) ! 118.509 us  |    }
 14)               |    nfs4svc_encode_compoundres [nfsd]() {
 14)               |      nfsd4_sequence_done [nfsd]() {
 14)   1.102 us    |        copy_cred [nfsd]();
 14)               |        nfsd4_put_session_locked [nfsd]() {
 14)   1.273 us    |          put_client_renew_locked [nfsd]();
 14)   3.156 us    |        }
 14)   7.775 us    |      }
 14)   9.558 us    |    }
 14)   0.991 us    |    nfsd_cache_update [nfsd]();
 14) ! 169.493 us  |  }
 14)   0.922 us    |  nfsd4_release_compoundargs [nfsd]();
 14)   2.715 us    |  nfsd_init_request [nfsd]();
 14)               |  nfsd_dispatch [nfsd]() {
 14)               |    nfs4svc_decode_compoundargs [nfsd]() {
 14)               |      nfsd4_decode_compound.constprop.0 [nfsd]() {
 14)   0.892 us    |        OPDESC [nfsd]();
 14)               |        nfsd4_decode_sequence [nfsd]() {
 14)   0.932 us    |          nfsd4_decode_sessionid4 [nfsd]();
 14)   2.695 us    |        }
 14)   0.902 us    |        nfsd4_cache_this_op [nfsd]();
 14)               |        nfsd4_max_reply [nfsd]() {
 14)   0.882 us    |          nfsd4_sequence_rsize [nfsd]();
 14)   2.675 us    |        }
 14)   0.871 us    |        OPDESC [nfsd]();
 14)               |        nfsd4_decode_putfh [nfsd]() {
 14)   0.882 us    |          svcxdr_savemem [nfsd]();
 14)   2.705 us    |        }
 14)   0.872 us    |        nfsd4_cache_this_op [nfsd]();
 14)               |        nfsd4_max_reply [nfsd]() {
 14)   0.882 us    |          nfsd4_only_status_rsize [nfsd]();
 14)   2.624 us    |        }
 14)   0.882 us    |        OPDESC [nfsd]();
 14)   0.872 us    |        nfsd4_cache_this_op [nfsd]();
 14)               |        nfsd4_max_reply [nfsd]() {
 14)   0.922 us    |          nfsd4_getattr_rsize [nfsd]();
 14)   2.674 us    |        }
 14) + 29.494 us   |      }
 14) + 31.248 us   |    }
 14)   1.062 us    |    nfsd_cache_lookup [nfsd]();
 14)               |    nfsd4_proc_compound [nfsd]() {
 14)   0.952 us    |      nfsd_minorversion [nfsd]();
 14)               |      nfsd4_sequence [nfsd]() {
 14)               |        find_in_sessionid_hashtbl [nfsd]() {
 14)   0.962 us    |          get_client_locked [nfsd]();
 14)   2.955 us    |        }
 14)   5.971 us    |      }
 14)               |      nfsd4_encode_operation [nfsd]() {
 14)   1.072 us    |        nfsd4_encode_sequence [nfsd]();
 14)   2.985 us    |      }
 14)   0.902 us    |      nfsd4_cstate_clear_replay [nfsd]();
 14)               |      nfsd4_putfh [nfsd]() {
 14)   0.922 us    |        fh_put [nfsd]();
 14)               |        fh_verify [nfsd]() {
 14)               |          nfsd_set_fh_dentry [nfsd]() {
 14)               |            rqst_exp_find [nfsd]() {
 14)               |              exp_find [nfsd]() {
 14)   1.913 us    |                exp_find_key [nfsd]();
 14)               |                exp_get_by_name [nfsd]() {
 14)   1.032 us    |                  svc_export_match [nfsd]();
 14)   1.062 us    |                  svc_export_upcall [nfsd]();
 14)   5.089 us    |                }
 14)   9.738 us    |              }
 14) + 11.601 us   |            }
 14) + 15.589 us   |          }
 14)               |          nfsd_setuser_and_check_port [nfsd]() {
 14)   0.891 us    |            nfsexp_flags [nfsd]();
 14)   6.221 us    |            nfsd_setuser [nfsd]();
 14)   0.901 us    |            nfserrno [nfsd]();
 14) + 11.471 us   |          }
 14)   0.911 us    |          nfsd_permission [nfsd]();
 14) + 31.538 us   |        }
 14) + 35.085 us   |      }
 14)   0.882 us    |      clear_current_stateid [nfsd]();
 14)   0.951 us    |      check_nfsd_access [nfsd]();
 14)               |      nfsd4_encode_operation [nfsd]() {
 14)   0.891 us    |        nfsd4_encode_noop [nfsd]();
 14)   2.745 us    |      }
 14)   0.892 us    |      nfsd4_cstate_clear_replay [nfsd]();
 14)               |      nfsd4_getattr [nfsd]() {
 14)               |        fh_verify [nfsd]() {
 14)               |          nfsd_setuser_and_check_port [nfsd]() {
 14)   0.891 us    |            nfsexp_flags [nfsd]();
 14)   5.851 us    |            nfsd_setuser [nfsd]();
 14)   0.912 us    |            nfserrno [nfsd]();
 14) + 10.920 us   |          }
 14)   0.881 us    |          check_nfsd_access [nfsd]();
 14)   0.891 us    |          nfsd_permission [nfsd]();
 14) + 16.050 us   |        }
 14) + 17.742 us   |      }
 14)               |      nfsd4_encode_operation [nfsd]() {
 14)               |        nfsd4_encode_getattr [nfsd]() {
 14)               |          nfsd4_encode_fattr [nfsd]() {
 14)   0.942 us    |            nfsd4_encode_bitmap [nfsd]();
 14)   0.892 us    |            fsid_source [nfsd]();
 14)               |            nfsd4_encode_user [nfsd]() {
 14)   2.064 us    |              encode_name_from_id [nfsd]();
 14)   3.957 us    |            }
 14)               |            nfsd4_encode_group [nfsd]() {
 14)   1.292 us    |              encode_name_from_id [nfsd]();
 14)   3.096 us    |            }
 14) + 15.869 us   |          }
 14) + 17.733 us   |        }
 14) + 19.556 us   |      }
 14)   0.891 us    |      nfsd4_cstate_clear_replay [nfsd]();
 14)   1.192 us    |      fh_put [nfsd]();
 14)   0.901 us    |      fh_put [nfsd]();
 14) ! 106.447 us  |    }
 14)               |    nfs4svc_encode_compoundres [nfsd]() {
 14)               |      nfsd4_sequence_done [nfsd]() {
 14)   1.092 us    |        copy_cred [nfsd]();
 14)               |        nfsd4_put_session_locked [nfsd]() {
 14)   1.162 us    |          put_client_renew_locked [nfsd]();
 14)   3.086 us    |        }
 14)   7.283 us    |      }
 14)   9.067 us    |    }
 14)   0.992 us    |    nfsd_cache_update [nfsd]();
 14) ! 154.265 us  |  }
 14)   0.912 us    |  nfsd4_release_compoundargs [nfsd]();
 14)   1.673 us    |  nfsd_init_request [nfsd]();
 14)               |  nfsd_dispatch [nfsd]() {
 14)               |    nfs4svc_decode_compoundargs [nfsd]() {
 14)               |      nfsd4_decode_compound.constprop.0 [nfsd]() {
 14)   0.881 us    |        OPDESC [nfsd]();
 14)               |        nfsd4_decode_sequence [nfsd]() {
 14)   0.912 us    |          nfsd4_decode_sessionid4 [nfsd]();
 14)   2.665 us    |        }
 14)   0.892 us    |        nfsd4_cache_this_op [nfsd]();
 14)               |        nfsd4_max_reply [nfsd]() {
 14)   0.892 us    |          nfsd4_sequence_rsize [nfsd]();
 14)   2.615 us    |        }
 14)   0.881 us    |        OPDESC [nfsd]();
 14)               |        nfsd4_decode_putfh [nfsd]() {
 14)   0.881 us    |          svcxdr_savemem [nfsd]();
 14)   2.635 us    |        }
 14)   0.882 us    |        nfsd4_cache_this_op [nfsd]();
 14)               |        nfsd4_max_reply [nfsd]() {
 14)   0.892 us    |          nfsd4_only_status_rsize [nfsd]();
 14)   2.615 us    |        }
 14)   0.881 us    |        OPDESC [nfsd]();
 14)               |        nfsd4_decode_readdir [nfsd]() {
 14)   0.922 us    |          nfsd4_decode_verifier4 [nfsd]();
 14)   2.936 us    |        }
 14)   0.882 us    |        nfsd4_cache_this_op [nfsd]();
 14)               |        nfsd4_max_reply [nfsd]() {
 14)   0.962 us    |          nfsd4_readdir_rsize [nfsd]();
 14)   2.895 us    |        }
 14) + 32.730 us   |      }
 14) + 34.434 us   |    }
 14)   0.992 us    |    nfsd_cache_lookup [nfsd]();
 14)               |    nfsd4_proc_compound [nfsd]() {
 14)   0.922 us    |      nfsd_minorversion [nfsd]();
 14)               |      nfsd4_sequence [nfsd]() {
 14)               |        find_in_sessionid_hashtbl [nfsd]() {
 14)   0.942 us    |          get_client_locked [nfsd]();
 14)   2.755 us    |        }
 14)   5.330 us    |      }
 14)               |      nfsd4_encode_operation [nfsd]() {
 14)   1.503 us    |        nfsd4_encode_sequence [nfsd]();
 14)   3.296 us    |      }
 14)   0.891 us    |      nfsd4_cstate_clear_replay [nfsd]();
 14)               |      nfsd4_putfh [nfsd]() {
 14)   0.912 us    |        fh_put [nfsd]();
 14)               |        fh_verify [nfsd]() {
 14)               |          nfsd_set_fh_dentry [nfsd]() {
 14)               |            rqst_exp_find [nfsd]() {
 14)               |              exp_find [nfsd]() {
 14)   1.463 us    |                exp_find_key [nfsd]();
 14)               |                exp_get_by_name [nfsd]() {
 14)   0.892 us    |                  svc_export_match [nfsd]();
 14)   0.952 us    |                  svc_export_upcall [nfsd]();
 14)   4.749 us    |                }
 14)   8.857 us    |              }
 14) + 10.640 us   |            }
 14) + 13.655 us   |          }
 14)               |          nfsd_setuser_and_check_port [nfsd]() {
 14)   0.882 us    |            nfsexp_flags [nfsd]();
 14)   5.631 us    |            nfsd_setuser [nfsd]();
 14)   0.892 us    |            nfserrno [nfsd]();
 14) + 10.739 us   |          }
 14)   0.892 us    |          nfsd_permission [nfsd]();
 14) + 28.663 us   |        }
 14) + 32.140 us   |      }
 14)   0.882 us    |      clear_current_stateid [nfsd]();
 14)   0.902 us    |      check_nfsd_access [nfsd]();
 14)               |      nfsd4_encode_operation [nfsd]() {
 14)   0.892 us    |        nfsd4_encode_noop [nfsd]();
 14)   2.685 us    |      }
 14)   0.891 us    |      nfsd4_cstate_clear_replay [nfsd]();
 14)   0.932 us    |      nfsd4_readdir [nfsd]();
 14)               |      nfsd4_encode_operation [nfsd]() {
 14)               |        nfsd4_encode_readdir [nfsd]() {
 14)               |          nfsd_readdir [nfsd]() {
 14)               |            nfsd_open [nfsd]() {
 14)               |              fh_verify [nfsd]() {
 14)               |                nfsd_setuser_and_check_port [nfsd]() {
 14)   0.881 us    |                  nfsexp_flags [nfsd]();
 14)   5.630 us    |                  nfsd_setuser [nfsd]();
 14)   0.901 us    |                  nfserrno [nfsd]();
 14) + 10.700 us   |                }
 14)   0.892 us    |                check_nfsd_access [nfsd]();
 14)   1.253 us    |                nfsd_permission [nfsd]();
 14) + 16.210 us   |              }
 14)               |              __nfsd_open.constprop.0 [nfsd]() {
 14)   0.901 us    |                nfsd_open_break_lease.part.0 [nfsd]();
 14)   1.001 us    |                nfserrno [nfsd]();
 14)   9.177 us    |              }
 14) + 28.081 us   |            }
 13)   6.191 us    |  svc_export_request [nfsd]();
 14)               |            nfsd_buffered_readdir [nfsd]() {
 14)   1.092 us    |              nfsd_buffered_filldir [nfsd]();
 14)   0.942 us    |              nfsd_buffered_filldir [nfsd]();
 14)   0.942 us    |              nfsd_buffered_filldir [nfsd]();
 14)   0.942 us    |              nfsd_buffered_filldir [nfsd]();
 14)   0.931 us    |              nfsd_buffered_filldir [nfsd]();
 14)   0.942 us    |              nfsd_buffered_filldir [nfsd]();
 14)   0.931 us    |              nfsd_buffered_filldir [nfsd]();
 14)   0.942 us    |              nfsd_buffered_filldir [nfsd]();
 14)   0.942 us    |              nfsd_buffered_filldir [nfsd]();
 14)   0.932 us    |              nfsd_buffered_filldir [nfsd]();
 14)   0.932 us    |              nfsd_buffered_filldir [nfsd]();
 14)   0.941 us    |              nfsd_buffered_filldir [nfsd]();
 14)               |              nfsd4_encode_dirent [nfsd]() {
 14)               |                nfsd4_encode_dirent_fattr [nfsd]() {
 14)               |                  nfsd_mountpoint [nfsd]() {
 14)   0.902 us    |                    nfsd4_is_junction [nfsd]();
 14)   2.806 us    |                  }
 14)               |                  nfsd4_encode_fattr [nfsd]() {
 14)               |                    fh_compose [nfsd]() {
 14)   1.593 us    |                      _fh_update.part.0.isra.0 [nfsd]();
 14)   3.737 us    |                    }
 14)   0.912 us    |                    nfsd4_encode_bitmap [nfsd]();
 14)   0.882 us    |                    fsid_source [nfsd]();
 14)               |                    nfsd4_encode_user [nfsd]() {
 14)   1.653 us    |                      encode_name_from_id [nfsd]();
 14)   3.456 us    |                    }
 14)               |                    nfsd4_encode_group [nfsd]() {
 14)   1.282 us    |                      encode_name_from_id [nfsd]();
 14)   3.036 us    |                    }
 14)   1.092 us    |                    fh_put [nfsd]();
 14) + 22.351 us   |                  }
 14) + 29.795 us   |                }
 14) + 32.079 us   |              }
 14)               |              nfsd4_encode_dirent [nfsd]() {
 14)               |                nfsd4_encode_dirent_fattr [nfsd]() {
 14)               |                  nfsd_mountpoint [nfsd]() {
 14)   1.012 us    |                    nfsd4_is_junction [nfsd]();
 14)   2.735 us    |                  }
 14)               |                  nfsd4_encode_fattr [nfsd]() {
 14)               |                    fh_compose [nfsd]() {
 14)   0.992 us    |                      _fh_update.part.0.isra.0 [nfsd]();
 14)   2.775 us    |                    }
 14)   0.911 us    |                    nfsd4_encode_bitmap [nfsd]();
 14)   0.902 us    |                    fsid_source [nfsd]();
 14)               |                    nfsd4_encode_user [nfsd]() {
 14)   1.312 us    |                      encode_name_from_id [nfsd]();
 14)   3.076 us    |                    }
 14)               |                    nfsd4_encode_group [nfsd]() {
 14)   1.282 us    |                      encode_name_from_id [nfsd]();
 14)   3.026 us    |                    }
 14)   1.082 us    |                    fh_put [nfsd]();
 14) + 19.636 us   |                  }
 14) + 25.908 us   |                }
 14) + 28.734 us   |              }
 14)               |              nfsd4_encode_dirent [nfsd]() {
 14)               |                nfsd4_encode_dirent_fattr [nfsd]() {
 14)               |                  nfsd_mountpoint [nfsd]() {
 14)   0.892 us    |                    nfsd4_is_junction [nfsd]();
 14)   2.876 us    |                  }
 14)               |                  nfsd4_encode_fattr [nfsd]() {
 14)               |                    fh_compose [nfsd]() {
 14)   0.982 us    |                      _fh_update.part.0.isra.0 [nfsd]();
 14)   2.775 us    |                    }
 14)   0.912 us    |                    nfsd4_encode_bitmap [nfsd]();
 14)   0.891 us    |                    fsid_source [nfsd]();
 14)               |                    nfsd4_encode_user [nfsd]() {
 14)   1.303 us    |                      encode_name_from_id [nfsd]();
 14)   3.065 us    |                    }
 14)               |                    nfsd4_encode_group [nfsd]() {
 14)   1.273 us    |                      encode_name_from_id [nfsd]();
 14)   3.035 us    |                    }
 14)   1.062 us    |                    fh_put [nfsd]();
 14) + 19.256 us   |                  }
 14) + 25.417 us   |                }
 14) + 27.601 us   |              }
 14)   0.931 us    |              nfsd4_encode_dirent [nfsd]();
 14)               |              nfsd4_encode_dirent [nfsd]() {
 14)               |                nfsd4_encode_dirent_fattr [nfsd]() {
 14)               |                  nfsd_mountpoint [nfsd]() {
 14)   0.892 us    |                    nfsd4_is_junction [nfsd]();
 14)   2.585 us    |                  }
 14)               |                  nfsd4_encode_fattr [nfsd]() {
 14)               |                    fh_compose [nfsd]() {
 14)   0.972 us    |                      _fh_update.part.0.isra.0 [nfsd]();
 14)   2.765 us    |                    }
 14)   0.911 us    |                    nfsd4_encode_bitmap [nfsd]();
 14)   0.892 us    |                    fsid_source [nfsd]();
 14)               |                    nfsd4_encode_user [nfsd]() {
 14)   1.312 us    |                      encode_name_from_id [nfsd]();
 14)   3.066 us    |                    }
 14)               |                    nfsd4_encode_group [nfsd]() {
 14)   1.282 us    |                      encode_name_from_id [nfsd]();
 14)   3.026 us    |                    }
 14)   1.072 us    |                    fh_put [nfsd]();
 14) + 19.336 us   |                  }
 14) + 25.207 us   |                }
 14) + 27.400 us   |              }
 14)               |              nfsd4_encode_dirent [nfsd]() {
 14)               |                nfsd4_encode_dirent_fattr [nfsd]() {
 14)               |                  nfsd_mountpoint [nfsd]() {
 14)   1.082 us    |                    nfsd4_is_junction [nfsd]();
 14)   2.765 us    |                  }
 14)               |                  nfsd4_encode_fattr [nfsd]() {
 14)               |                    fh_compose [nfsd]() {
 14)   0.992 us    |                      _fh_update.part.0.isra.0 [nfsd]();
 14)   2.765 us    |                    }
 14)   0.911 us    |                    nfsd4_encode_bitmap [nfsd]();
 14)   0.902 us    |                    fsid_source [nfsd]();
 14)               |                    nfsd4_encode_user [nfsd]() {
 14)   1.312 us    |                      encode_name_from_id [nfsd]();
 14)   3.076 us    |                    }
 14)               |                    nfsd4_encode_group [nfsd]() {
 14)   1.282 us    |                      encode_name_from_id [nfsd]();
 14)   3.036 us    |                    }
 14)   1.072 us    |                    fh_put [nfsd]();
 14) + 19.285 us   |                  }
 14) + 25.287 us   |                }
 14) + 27.431 us   |              }
 14)   0.932 us    |              nfsd4_encode_dirent [nfsd]();
 14)               |              nfsd4_encode_dirent [nfsd]() {
 14)               |                nfsd4_encode_dirent_fattr [nfsd]() {
 14)               |                  nfsd_mountpoint [nfsd]() {
 14)   1.022 us    |                    nfsd4_is_junction [nfsd]();
 14)   2.715 us    |                  }
 14)               |                  nfsd4_encode_fattr [nfsd]() {
 14)               |                    fh_compose [nfsd]() {
 14)   0.972 us    |                      _fh_update.part.0.isra.0 [nfsd]();
 14)   2.765 us    |                    }
 14)   0.912 us    |                    nfsd4_encode_bitmap [nfsd]();
 14)   0.891 us    |                    fsid_source [nfsd]();
 14)               |                    nfsd4_encode_user [nfsd]() {
 14)   1.313 us    |                      encode_name_from_id [nfsd]();
 14)   3.075 us    |                    }
 14)               |                    nfsd4_encode_group [nfsd]() {
 14)   1.273 us    |                      encode_name_from_id [nfsd]();
 14)   3.025 us    |                    }
 14)   1.062 us    |                    fh_put [nfsd]();
 14) + 19.236 us   |                  }
 14) + 25.346 us   |                }
 14) + 27.531 us   |              }
 14)               |              nfsd4_encode_dirent [nfsd]() {
 14)               |                nfsd4_encode_dirent_fattr [nfsd]() {
 14)               |                  nfsd_mountpoint [nfsd]() {
 14)   0.892 us    |                    nfsd4_is_junction [nfsd]();
 14)   2.585 us    |                  }
 14)               |                  nfsd4_encode_fattr [nfsd]() {
 14)               |                    fh_compose [nfsd]() {
 14)   0.981 us    |                      _fh_update.part.0.isra.0 [nfsd]();
 14)   2.765 us    |                    }
 14)   0.912 us    |                    nfsd4_encode_bitmap [nfsd]();
 14)   0.891 us    |                    fsid_source [nfsd]();
 14)               |                    nfsd4_encode_user [nfsd]() {
 14)   1.313 us    |                      encode_name_from_id [nfsd]();
 14)   3.065 us    |                    }
 14)               |                    nfsd4_encode_group [nfsd]() {
 14)   1.283 us    |                      encode_name_from_id [nfsd]();
 14)   3.025 us    |                    }
 14)   1.062 us    |                    fh_put [nfsd]();
 14) + 19.386 us   |                  }
 14) + 25.197 us   |                }
 14) + 27.371 us   |              }
 14)               |              nfsd4_encode_dirent [nfsd]() {
 14)               |                nfsd4_encode_dirent_fattr [nfsd]() {
 14)               |                  nfsd_mountpoint [nfsd]() {
 14)   1.012 us    |                    nfsd4_is_junction [nfsd]();
 14)   2.695 us    |                  }
 14)               |                  nfsd4_encode_fattr [nfsd]() {
 14)               |                    fh_compose [nfsd]() {
 14)   0.982 us    |                      _fh_update.part.0.isra.0 [nfsd]();
 14)   2.775 us    |                    }
 14)   0.911 us    |                    nfsd4_encode_bitmap [nfsd]();
 14)   0.892 us    |                    fsid_source [nfsd]();
 14)               |                    nfsd4_encode_user [nfsd]() {
 14)   1.312 us    |                      encode_name_from_id [nfsd]();
 14)   3.066 us    |                    }
 14)               |                    nfsd4_encode_group [nfsd]() {
 14)   1.282 us    |                      encode_name_from_id [nfsd]();
 14)   3.026 us    |                    }
 14)   1.062 us    |                    fh_put [nfsd]();
 14) + 19.516 us   |                  }
 14) + 25.397 us   |                }
 14) + 27.591 us   |              }
 14)               |              nfsd4_encode_dirent [nfsd]() {
 14)               |                nfsd4_encode_dirent_fattr [nfsd]() {
 14)               |                  nfsd_mountpoint [nfsd]() {
 14)   0.882 us    |                    nfsd4_is_junction [nfsd]();
 14)   2.575 us    |                  }
 14)               |                  nfsd4_encode_fattr [nfsd]() {
 14)               |                    fh_compose [nfsd]() {
 14)   0.972 us    |                      _fh_update.part.0.isra.0 [nfsd]();
 14)   2.765 us    |                    }
 14)   0.912 us    |                    nfsd4_encode_bitmap [nfsd]();
 14)   0.892 us    |                    fsid_source [nfsd]();
 14)               |                    nfsd4_encode_user [nfsd]() {
 14)   1.302 us    |                      encode_name_from_id [nfsd]();
 14)   3.076 us    |                    }
 14)               |                    nfsd4_encode_group [nfsd]() {
 14)   1.282 us    |                      encode_name_from_id [nfsd]();
 14)   3.036 us    |                    }
 14)   1.052 us    |                    fh_put [nfsd]();
 14) + 19.225 us   |                  }
 14) + 25.006 us   |                }
 14) + 27.190 us   |              }
 14)               |              nfsd4_encode_dirent [nfsd]() {
 14)               |                nfsd4_encode_dirent_fattr [nfsd]() {
 14)               |                  nfsd_mountpoint [nfsd]() {
 14)   1.022 us    |                    nfsd4_is_junction [nfsd]();
 14)   2.705 us    |                  }
 14)               |                  nfsd4_encode_fattr [nfsd]() {
 14)               |                    fh_compose [nfsd]() {
 14)   0.981 us    |                      _fh_update.part.0.isra.0 [nfsd]();
 14)   3.236 us    |                    }
 14)   0.912 us    |                    nfsd4_encode_bitmap [nfsd]();
 14)   0.892 us    |                    fsid_source [nfsd]();
 14)               |                    nfsd4_encode_user [nfsd]() {
 14)   1.312 us    |                      encode_name_from_id [nfsd]();
 14)   3.085 us    |                    }
 14)               |                    nfsd4_encode_group [nfsd]() {
 14)   1.272 us    |                      encode_name_from_id [nfsd]();
 14)   3.036 us    |                    }
 14)   1.062 us    |                    fh_put [nfsd]();
 14) + 19.776 us   |                  }
 14) + 25.768 us   |                }
 14) + 27.901 us   |              }
 14) ! 356.929 us  |            }
 14) ! 389.339 us  |          }
 14) ! 391.703 us  |        }
 14) ! 393.596 us  |      }
 14)   0.952 us    |      nfsd4_cstate_clear_replay [nfsd]();
 14)   1.062 us    |      fh_put [nfsd]();
 14)   0.952 us    |      fh_put [nfsd]();
 14) ! 458.758 us  |    }
 14)               |    nfs4svc_encode_compoundres [nfsd]() {
 14)               |      nfsd4_sequence_done [nfsd]() {
 14)   1.002 us    |        copy_cred [nfsd]();
 14)               |        nfsd4_put_session_locked [nfsd]() {
 14)   1.062 us    |          put_client_renew_locked [nfsd]();
 14)   2.825 us    |        }
 14)   6.762 us    |      }
 14)   8.536 us    |    }
 14)   1.002 us    |    nfsd_cache_update [nfsd]();
 14) ! 508.850 us  |  }
 14)   0.892 us    |  nfsd4_release_compoundargs [nfsd]();
 ------------------------------------------
 14)  nfsd-352337   => rpc.mou-352326
 ------------------------------------------

 14)               |  svc_export_parse [nfsd]() {
 14)   1.263 us    |    check_export [nfsd]();
 14)   0.902 us    |    nfsd4_setup_layout_type [nfsd]();
 14)   1.132 us    |    svc_export_match [nfsd]();
 14)               |    svc_export_alloc [nfsd]() {
 14) + 11.992 us   |      nfsd_percpu_counters_init [nfsd]();
 14) + 14.617 us   |    }
 14)               |    svc_export_init [nfsd]() {
 14)   2.655 us    |      nfsd_percpu_counters_reset [nfsd]();
 14)   4.459 us    |    }
 14)   0.942 us    |    export_update [nfsd]();
 14)               |    svc_export_put [nfsd]() {
 14)   0.881 us    |      nfsd4_fslocs_free [nfsd]();
 14)   4.779 us    |      nfsd_percpu_counters_destroy [nfsd]();
 14)   9.447 us    |    }
 14)   1.032 us    |    nfsd4_fslocs_free [nfsd]();
 14) + 86.059 us   |  }
 ------------------------------------------
 14) rpc.mou-352326 =>  nfsd-352337
 ------------------------------------------

 14)   5.810 us    |  nfsd_init_request [nfsd]();
 14)               |  nfsd_dispatch [nfsd]() {
 14)               |    nfs4svc_decode_compoundargs [nfsd]() {
 14)               |      nfsd4_decode_compound.constprop.0 [nfsd]() {
 14)   0.701 us    |        OPDESC [nfsd]();
 14)               |        nfsd4_decode_sequence [nfsd]() {
 14)   0.692 us    |          nfsd4_decode_sessionid4 [nfsd]();
 14)   2.214 us    |        }
 14)   0.651 us    |        nfsd4_cache_this_op [nfsd]();
 14)               |        nfsd4_max_reply [nfsd]() {
 14)   0.651 us    |          nfsd4_sequence_rsize [nfsd]();
 14)   2.194 us    |        }
 14)   0.641 us    |        OPDESC [nfsd]();
 14)               |        nfsd4_decode_putfh [nfsd]() {
 14)   0.661 us    |          svcxdr_savemem [nfsd]();
 14)   2.064 us    |        }
 14)   0.641 us    |        nfsd4_cache_this_op [nfsd]();
 14)               |        nfsd4_max_reply [nfsd]() {
 14)   0.661 us    |          nfsd4_only_status_rsize [nfsd]();
 14)   2.064 us    |        }
 14)   0.652 us    |        OPDESC [nfsd]();
 14)   0.641 us    |        nfsd4_cache_this_op [nfsd]();
 14)               |        nfsd4_max_reply [nfsd]() {
 14)   0.681 us    |          nfsd4_getattr_rsize [nfsd]();
 14)   2.034 us    |        }
 14) + 23.604 us   |      }
 14) + 25.337 us   |    }
 14)   0.861 us    |    nfsd_cache_lookup [nfsd]();
 14)               |    nfsd4_proc_compound [nfsd]() {
 14)   0.821 us    |      nfsd_minorversion [nfsd]();
 14)               |      nfsd4_sequence [nfsd]() {
 14)               |        find_in_sessionid_hashtbl [nfsd]() {
 14)   0.742 us    |          get_client_locked [nfsd]();
 14)   3.506 us    |        }
 14)   6.562 us    |      }
 14)               |      nfsd4_encode_operation [nfsd]() {
 14)   0.882 us    |        nfsd4_encode_sequence [nfsd]();
 14)   2.414 us    |      }
 14)   0.661 us    |      nfsd4_cstate_clear_replay [nfsd]();
 14)               |      nfsd4_putfh [nfsd]() {
 14)   0.681 us    |        fh_put [nfsd]();
 14)               |        fh_verify [nfsd]() {
 14)               |          nfsd_set_fh_dentry [nfsd]() {
 14)               |            rqst_exp_find [nfsd]() {
 14)               |              exp_find [nfsd]() {
 14)   1.914 us    |                exp_find_key [nfsd]();
 14)               |                exp_get_by_name [nfsd]() {
 14)   0.851 us    |                  svc_export_match [nfsd]();
 14)   2.645 us    |                }
 14)   6.743 us    |              }
 14)   8.165 us    |            }
 14) + 13.926 us   |          }
 14)               |          nfsd_setuser_and_check_port [nfsd]() {
 14)   0.661 us    |            nfsexp_flags [nfsd]();
 14)   5.190 us    |            nfsd_setuser [nfsd]();
 14)   0.671 us    |            nfserrno [nfsd]();
 14)   9.247 us    |          }
 14)   0.791 us    |          nfsd_permission [nfsd]();
 14) + 26.839 us   |        }
 14) + 29.584 us   |      }
 14)   0.651 us    |      clear_current_stateid [nfsd]();
 14)   0.661 us    |      check_nfsd_access [nfsd]();
 14)               |      nfsd4_encode_operation [nfsd]() {
 14)   0.651 us    |        nfsd4_encode_noop [nfsd]();
 14)   2.074 us    |      }
 14)   0.642 us    |      nfsd4_cstate_clear_replay [nfsd]();
 14)               |      nfsd4_getattr [nfsd]() {
 14)               |        fh_verify [nfsd]() {
 14)               |          nfsd_setuser_and_check_port [nfsd]() {
 14)   0.641 us    |            nfsexp_flags [nfsd]();
 14)   4.288 us    |            nfsd_setuser [nfsd]();
 14)   0.651 us    |            nfserrno [nfsd]();
 14)   8.024 us    |          }
 14)   0.651 us    |          check_nfsd_access [nfsd]();
 14)   0.641 us    |          nfsd_permission [nfsd]();
 14) + 11.782 us   |        }
 14) + 13.024 us   |      }
 14)               |      nfsd4_encode_operation [nfsd]() {
 14)               |        nfsd4_encode_getattr [nfsd]() {
 14)               |          nfsd4_encode_fattr [nfsd]() {
 14)   0.692 us    |            nfsd4_encode_bitmap [nfsd]();
 14)   0.671 us    |            fsid_source [nfsd]();
 14)               |            nfsd4_encode_user [nfsd]() {
 14)   2.194 us    |              encode_name_from_id [nfsd]();
 14)   3.637 us    |            }
 14)               |            nfsd4_encode_group [nfsd]() {
 14)   0.952 us    |              encode_name_from_id [nfsd]();
 14)   2.264 us    |            }
 14) + 13.715 us   |          }
 14) + 15.198 us   |        }
 14) + 16.691 us   |      }
 14)   0.651 us    |      nfsd4_cstate_clear_replay [nfsd]();
 14)   0.881 us    |      fh_put [nfsd]();
 14)   0.671 us    |      fh_put [nfsd]();
 14) + 87.211 us   |    }
 14)               |    nfs4svc_encode_compoundres [nfsd]() {
 14)               |      nfsd4_sequence_done [nfsd]() {
 14)   0.902 us    |        copy_cred [nfsd]();
 14)               |        nfsd4_put_session_locked [nfsd]() {
 14)   0.882 us    |          put_client_renew_locked [nfsd]();
 14)   2.324 us    |        }
 14)   5.811 us    |      }
 14)   7.113 us    |    }
 14)   0.732 us    |    nfsd_cache_update [nfsd]();
 14) ! 126.123 us  |  }
 14)   0.681 us    |  nfsd4_release_compoundargs [nfsd]();
 14)   3.186 us    |  nfsd_init_request [nfsd]();
 14)               |  nfsd_dispatch [nfsd]() {
 14)               |    nfs4svc_decode_compoundargs [nfsd]() {
 14)               |      nfsd4_decode_compound.constprop.0 [nfsd]() {
 14)   0.290 us    |        OPDESC [nfsd]();
 14)               |        nfsd4_decode_sequence [nfsd]() {
 14)   0.301 us    |          nfsd4_decode_sessionid4 [nfsd]();
 14)   0.952 us    |        }
 14)   0.290 us    |        nfsd4_cache_this_op [nfsd]();
 14)               |        nfsd4_max_reply [nfsd]() {
 14)   0.290 us    |          nfsd4_sequence_rsize [nfsd]();
 14)   1.964 us    |        }
 14)   0.280 us    |        OPDESC [nfsd]();
 14)               |        nfsd4_decode_putfh [nfsd]() {
 14)   0.290 us    |          svcxdr_savemem [nfsd]();
 14)   1.132 us    |        }
 14)   0.291 us    |        nfsd4_cache_this_op [nfsd]();
 14)               |        nfsd4_max_reply [nfsd]() {
 14)   0.291 us    |          nfsd4_only_status_rsize [nfsd]();
 14)   1.052 us    |        }
 14)   0.290 us    |        OPDESC [nfsd]();
 14)   0.281 us    |        nfsd4_cache_this_op [nfsd]();
 14)               |        nfsd4_max_reply [nfsd]() {
 14)   0.300 us    |          nfsd4_getattr_rsize [nfsd]();
 14)   1.242 us    |        }
 14) + 13.835 us   |      }
 14) + 14.567 us   |    }
 14)   0.511 us    |    nfsd_cache_lookup [nfsd]();
 14)               |    nfsd4_proc_compound [nfsd]() {
 14)   0.411 us    |      nfsd_minorversion [nfsd]();
 14)               |      nfsd4_sequence [nfsd]() {
 14)               |        find_in_sessionid_hashtbl [nfsd]() {
 14)   0.311 us    |          get_client_locked [nfsd]();
 14)   1.643 us    |        }
 14)   3.136 us    |      }
 14)               |      nfsd4_encode_operation [nfsd]() {
 14)   0.350 us    |        nfsd4_encode_sequence [nfsd]();
 14)   1.272 us    |      }
 14)   0.300 us    |      nfsd4_cstate_clear_replay [nfsd]();
 14)               |      nfsd4_putfh [nfsd]() {
 14)   0.310 us    |        fh_put [nfsd]();
 14)               |        fh_verify [nfsd]() {
 14)               |          nfsd_set_fh_dentry [nfsd]() {
 14)               |            rqst_exp_find [nfsd]() {
 14)               |              exp_find [nfsd]() {
 14)   1.243 us    |                exp_find_key [nfsd]();
 14)               |                exp_get_by_name [nfsd]() {
 14)   0.451 us    |                  svc_export_match [nfsd]();
 14)   1.383 us    |                }
 14)   3.597 us    |              }
 14)   4.268 us    |            }
 14)   5.831 us    |          }
 14)               |          nfsd_setuser_and_check_port [nfsd]() {
 14)   0.300 us    |            nfsexp_flags [nfsd]();
 14)   2.314 us    |            nfsd_setuser [nfsd]();
 14)   0.300 us    |            nfserrno [nfsd]();
 14)   4.218 us    |          }
 14)   0.290 us    |          nfsd_permission [nfsd]();
 14) + 11.702 us   |        }
 14) + 12.964 us   |      }
 14)   0.281 us    |      clear_current_stateid [nfsd]();
 14)   0.291 us    |      check_nfsd_access [nfsd]();
 14)               |      nfsd4_encode_operation [nfsd]() {
 14)   0.291 us    |        nfsd4_encode_noop [nfsd]();
 14)   0.972 us    |      }
 14)   0.280 us    |      nfsd4_cstate_clear_replay [nfsd]();
 14)               |      nfsd4_getattr [nfsd]() {
 14)               |        fh_verify [nfsd]() {
 14)               |          nfsd_setuser_and_check_port [nfsd]() {
 14)   0.290 us    |            nfsexp_flags [nfsd]();
 14)   1.943 us    |            nfsd_setuser [nfsd]();
 14)   0.290 us    |            nfserrno [nfsd]();
 14)   3.597 us    |          }
 14)   0.291 us    |          check_nfsd_access [nfsd]();
 14)   0.291 us    |          nfsd_permission [nfsd]();
 14)   5.259 us    |        }
 14)   5.811 us    |      }
 14)               |      nfsd4_encode_operation [nfsd]() {
 14)               |        nfsd4_encode_getattr [nfsd]() {
 14)               |          nfsd4_encode_fattr [nfsd]() {
 14)   0.311 us    |            nfsd4_encode_bitmap [nfsd]();
 14)   0.291 us    |            fsid_source [nfsd]();
 14)               |            nfsd4_encode_user [nfsd]() {
 14)   1.353 us    |              encode_name_from_id [nfsd]();
 14)   2.023 us    |            }
 14)               |            nfsd4_encode_group [nfsd]() {
 14)   0.430 us    |              encode_name_from_id [nfsd]();
 14)   1.012 us    |            }
 14)   6.653 us    |          }
 14)   7.323 us    |        }
 14)   8.085 us    |      }
 14)   0.301 us    |      nfsd4_cstate_clear_replay [nfsd]();
 14)   0.371 us    |      fh_put [nfsd]();
 14)   0.300 us    |      fh_put [nfsd]();
 14) + 40.806 us   |    }
 14)               |    nfs4svc_encode_compoundres [nfsd]() {
 14)               |      nfsd4_sequence_done [nfsd]() {
 14)   0.421 us    |        copy_cred [nfsd]();
 14)               |        nfsd4_put_session_locked [nfsd]() {
 14)   0.450 us    |          put_client_renew_locked [nfsd]();
 14)   1.092 us    |        }
 14)   2.875 us    |      }
 14)   3.446 us    |    }
 14)   0.321 us    |    nfsd_cache_update [nfsd]();
 14) + 62.456 us   |  }
 14)   0.301 us    |  nfsd4_release_compoundargs [nfsd]();
 10)   5.129 us    |  nfsd_init_request [nfsd]();
 10)               |  nfsd_dispatch [nfsd]() {
 10)               |    nfs4svc_decode_compoundargs [nfsd]() {
 10)               |      nfsd4_decode_compound.constprop.0 [nfsd]() {
 10)   0.901 us    |        OPDESC [nfsd]();
 10)               |        nfsd4_decode_sequence [nfsd]() {
 10)   0.922 us    |          nfsd4_decode_sessionid4 [nfsd]();
 10)   2.785 us    |        }
 10)   0.892 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   0.882 us    |          nfsd4_sequence_rsize [nfsd]();
 10)   2.875 us    |        }
 10)   0.882 us    |        OPDESC [nfsd]();
 10)               |        nfsd4_decode_putfh [nfsd]() {
 10)   0.891 us    |          svcxdr_savemem [nfsd]();
 10)   2.815 us    |        }
 10)   0.882 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   0.892 us    |          nfsd4_only_status_rsize [nfsd]();
 10)   2.736 us    |        }
 10)   0.882 us    |        OPDESC [nfsd]();
 10)   0.882 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   0.932 us    |          nfsd4_getattr_rsize [nfsd]();
 10)   2.775 us    |        }
 10) + 31.027 us   |      }
 10) + 32.890 us   |    }
 10)   1.212 us    |    nfsd_cache_lookup [nfsd]();
 10)               |    nfsd4_proc_compound [nfsd]() {
 10)   1.061 us    |      nfsd_minorversion [nfsd]();
 10)               |      nfsd4_sequence [nfsd]() {
 10)               |        find_in_sessionid_hashtbl [nfsd]() {
 10)   0.972 us    |          get_client_locked [nfsd]();
 10)   3.586 us    |        }
 10)   7.163 us    |      }
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)   1.142 us    |        nfsd4_encode_sequence [nfsd]();
 10)   3.246 us    |      }
 10)   0.922 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)               |      nfsd4_putfh [nfsd]() {
 10)   0.922 us    |        fh_put [nfsd]();
 10)               |        fh_verify [nfsd]() {
 10)               |          nfsd_set_fh_dentry [nfsd]() {
 10)               |            rqst_exp_find [nfsd]() {
 10)               |              exp_find [nfsd]() {
 10)   2.424 us    |                exp_find_key [nfsd]();
 10)               |                exp_get_by_name [nfsd]() {
 10)   1.242 us    |                  svc_export_match [nfsd]();
 10)   3.526 us    |                }
 10)   8.766 us    |              }
 10) + 10.729 us   |            }
 10) + 16.941 us   |          }
 10)               |          nfsd_setuser_and_check_port [nfsd]() {
 10)   0.922 us    |            nfsexp_flags [nfsd]();
 10)   6.712 us    |            nfsd_setuser [nfsd]();
 10)   0.912 us    |            nfserrno [nfsd]();
 10) + 12.182 us   |          }
 10)   0.912 us    |          nfsd_permission [nfsd]();
 10) + 33.802 us   |        }
 10) + 37.460 us   |      }
 10)   0.902 us    |      clear_current_stateid [nfsd]();
 10)   0.912 us    |      check_nfsd_access [nfsd]();
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)   0.892 us    |        nfsd4_encode_noop [nfsd]();
 10)   2.795 us    |      }
 10)   0.882 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)               |      nfsd4_getattr [nfsd]() {
 10)               |        fh_verify [nfsd]() {
 10)               |          nfsd_setuser_and_check_port [nfsd]() {
 10)   0.891 us    |            nfsexp_flags [nfsd]();
 10)   5.981 us    |            nfsd_setuser [nfsd]();
 10)   0.912 us    |            nfserrno [nfsd]();
 10) + 11.071 us   |          }
 10)   0.882 us    |          check_nfsd_access [nfsd]();
 10)   0.882 us    |          nfsd_permission [nfsd]();
 10) + 16.210 us   |        }
 10) + 17.913 us   |      }
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)               |        nfsd4_encode_getattr [nfsd]() {
 10)               |          nfsd4_encode_fattr [nfsd]() {
 10)   0.962 us    |            nfsd4_encode_bitmap [nfsd]();
 10)   0.901 us    |            fsid_source [nfsd]();
 10)               |            nfsd4_encode_user [nfsd]() {
 10)   2.865 us    |              encode_name_from_id [nfsd]();
 10)   4.849 us    |            }
 10)               |            nfsd4_encode_group [nfsd]() {
 10)   1.322 us    |              encode_name_from_id [nfsd]();
 10)   3.146 us    |            }
 10) + 19.165 us   |          }
 10) + 21.038 us   |        }
 10) + 23.053 us   |      }
 10)   0.922 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)   1.202 us    |      fh_put [nfsd]();
 10)   0.921 us    |      fh_put [nfsd]();
 10) ! 114.812 us  |    }
 10)               |    nfs4svc_encode_compoundres [nfsd]() {
 10)               |      nfsd4_sequence_done [nfsd]() {
 10)   1.112 us    |        copy_cred [nfsd]();
 10)               |        nfsd4_put_session_locked [nfsd]() {
 10)   1.142 us    |          put_client_renew_locked [nfsd]();
 10)   3.086 us    |        }
 10)   7.724 us    |      }
 10)   9.488 us    |    }
 10)   0.992 us    |    nfsd_cache_update [nfsd]();
 10) ! 165.596 us  |  }
 10)   0.922 us    |  nfsd4_release_compoundargs [nfsd]();
 10)   5.691 us    |  nfsd_init_request [nfsd]();
 10)               |  nfsd_dispatch [nfsd]() {
 10)               |    nfs4svc_decode_compoundargs [nfsd]() {
 10)               |      nfsd4_decode_compound.constprop.0 [nfsd]() {
 10)   0.931 us    |        OPDESC [nfsd]();
 10)               |        nfsd4_decode_sequence [nfsd]() {
 10)   1.152 us    |          nfsd4_decode_sessionid4 [nfsd]();
 10)   3.577 us    |        }
 10)   1.222 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   1.102 us    |          nfsd4_sequence_rsize [nfsd]();
 10)   3.557 us    |        }
 10)   1.132 us    |        OPDESC [nfsd]();
 10)               |        nfsd4_decode_putfh [nfsd]() {
 10)   1.082 us    |          svcxdr_savemem [nfsd]();
 10)   3.416 us    |        }
 10)   1.242 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   1.112 us    |          nfsd4_only_status_rsize [nfsd]();
 10)   3.416 us    |        }
 10)   1.173 us    |        OPDESC [nfsd]();
 10)   1.262 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   1.122 us    |          nfsd4_getattr_rsize [nfsd]();
 10)   3.407 us    |        }
 10) + 37.890 us   |      }
 10) + 39.934 us   |    }
 10)   1.713 us    |    nfsd_cache_lookup [nfsd]();
 10)               |    nfsd4_proc_compound [nfsd]() {
 10)   1.122 us    |      nfsd_minorversion [nfsd]();
 10)               |      nfsd4_sequence [nfsd]() {
 10)               |        find_in_sessionid_hashtbl [nfsd]() {
 10)   0.962 us    |          get_client_locked [nfsd]();
 10)   3.727 us    |        }
 10)   7.374 us    |      }
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)   1.212 us    |        nfsd4_encode_sequence [nfsd]();
 10)   3.366 us    |      }
 10)   1.052 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)               |      nfsd4_putfh [nfsd]() {
 10)   1.122 us    |        fh_put [nfsd]();
 10)               |        fh_verify [nfsd]() {
 10)               |          nfsd_set_fh_dentry [nfsd]() {
 10)               |            rqst_exp_find [nfsd]() {
 10)               |              exp_find [nfsd]() {
 10)   3.086 us    |                exp_find_key [nfsd]();
 10)               |                exp_get_by_name [nfsd]() {
 10)   1.302 us    |                  svc_export_match [nfsd]();
 10)   4.107 us    |                }
 10) + 10.659 us   |              }
 10) + 13.044 us   |            }
 10) + 18.644 us   |          }
 10)               |          nfsd_setuser_and_check_port [nfsd]() {
 10)   1.132 us    |            nfsexp_flags [nfsd]();
 10)   7.784 us    |            nfsd_setuser [nfsd]();
 10)   1.112 us    |            nfserrno [nfsd]();
 10) + 14.627 us   |          }
 10)   1.042 us    |          nfsd_permission [nfsd]();
 10) + 38.542 us   |        }
 10) + 42.739 us   |      }
 10)   0.902 us    |      clear_current_stateid [nfsd]();
 10)   0.902 us    |      check_nfsd_access [nfsd]();
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)   2.054 us    |        nfsd4_encode_noop [nfsd]();
 10)   4.018 us    |      }
 10)   1.022 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)               |      nfsd4_getattr [nfsd]() {
 10)               |        fh_verify [nfsd]() {
 10)               |          nfsd_setuser_and_check_port [nfsd]() {
 10)   1.102 us    |            nfsexp_flags [nfsd]();
 10)   6.983 us    |            nfsd_setuser [nfsd]();
 10)   1.122 us    |            nfserrno [nfsd]();
 10) + 13.375 us   |          }
 10)   1.112 us    |          check_nfsd_access [nfsd]();
 10)   1.132 us    |          nfsd_permission [nfsd]();
 10) + 19.827 us   |        }
 10) + 21.841 us   |      }
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)               |        nfsd4_encode_getattr [nfsd]() {
 10)               |          nfsd4_encode_fattr [nfsd]() {
 10)   1.202 us    |            nfsd4_encode_bitmap [nfsd]();
 10)   1.132 us    |            fsid_source [nfsd]();
 10)               |            nfsd4_encode_user [nfsd]() {
 10)   3.316 us    |              encode_name_from_id [nfsd]();
 10)   5.510 us    |            }
 10)               |            nfsd4_encode_group [nfsd]() {
 10)   1.312 us    |              encode_name_from_id [nfsd]();
 10)   3.116 us    |            }
 10) + 20.047 us   |          }
 10) + 22.120 us   |        }
 10) + 24.465 us   |      }
 10)   0.892 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)   1.192 us    |      fh_put [nfsd]();
 10)   1.042 us    |      fh_put [nfsd]();
 10) ! 129.499 us  |    }
 10)               |    nfs4svc_encode_compoundres [nfsd]() {
 10)               |      nfsd4_sequence_done [nfsd]() {
 10)   1.413 us    |        copy_cred [nfsd]();
 10)               |        nfsd4_put_session_locked [nfsd]() {
 10)   1.413 us    |          put_client_renew_locked [nfsd]();
 10)   3.716 us    |        }
 10)   9.307 us    |      }
 10) + 11.491 us   |    }
 10)   1.252 us    |    nfsd_cache_update [nfsd]();
 10) ! 190.582 us  |  }
 10)   1.122 us    |  nfsd4_release_compoundargs [nfsd]();
 10)   5.160 us    |  nfsd_init_request [nfsd]();
 10)               |  nfsd_dispatch [nfsd]() {
 10)               |    nfs4svc_decode_compoundargs [nfsd]() {
 10)               |      nfsd4_decode_compound.constprop.0 [nfsd]() {
 10)   0.912 us    |        OPDESC [nfsd]();
 10)               |        nfsd4_decode_sequence [nfsd]() {
 10)   0.922 us    |          nfsd4_decode_sessionid4 [nfsd]();
 10)   2.815 us    |        }
 10)   0.901 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   0.891 us    |          nfsd4_sequence_rsize [nfsd]();
 10)   2.986 us    |        }
 10)   0.881 us    |        OPDESC [nfsd]();
 10)               |        nfsd4_decode_putfh [nfsd]() {
 10)   0.892 us    |          svcxdr_savemem [nfsd]();
 10)   2.915 us    |        }
 10)   0.892 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   0.892 us    |          nfsd4_only_status_rsize [nfsd]();
 10)   2.855 us    |        }
 10)   0.882 us    |        OPDESC [nfsd]();
 10)   0.891 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   0.932 us    |          nfsd4_getattr_rsize [nfsd]();
 10)   2.815 us    |        }
 10) + 32.260 us   |      }
 10) + 34.203 us   |    }
 10)   1.153 us    |    nfsd_cache_lookup [nfsd]();
 10)               |    nfsd4_proc_compound [nfsd]() {
 10)   1.072 us    |      nfsd_minorversion [nfsd]();
 10)               |      nfsd4_sequence [nfsd]() {
 10)               |        find_in_sessionid_hashtbl [nfsd]() {
 10)   0.972 us    |          get_client_locked [nfsd]();
 10)   3.787 us    |        }
 10)   7.694 us    |      }
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)   1.072 us    |        nfsd4_encode_sequence [nfsd]();
 10)   3.206 us    |      }
 10)   0.912 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)               |      nfsd4_putfh [nfsd]() {
 10)   1.051 us    |        fh_put [nfsd]();
 10)               |        fh_verify [nfsd]() {
 10)               |          nfsd_set_fh_dentry [nfsd]() {
 10)               |            rqst_exp_find [nfsd]() {
 10)               |              exp_find [nfsd]() {
 10)   2.635 us    |                exp_find_key [nfsd]();
 10)               |                exp_get_by_name [nfsd]() {
 10)   1.182 us    |                  svc_export_match [nfsd]();
 10)   3.516 us    |                }
 10)   9.077 us    |              }
 10) + 11.371 us   |            }
 10) + 16.881 us   |          }
 10)               |          nfsd_setuser_and_check_port [nfsd]() {
 10)   0.892 us    |            nfsexp_flags [nfsd]();
 10)   6.542 us    |            nfsd_setuser [nfsd]();
 10)   0.932 us    |            nfserrno [nfsd]();
 10) + 12.062 us   |          }
 10)   0.902 us    |          nfsd_permission [nfsd]();
 10) + 33.622 us   |        }
 10) + 38.431 us   |      }
 10)   0.881 us    |      clear_current_stateid [nfsd]();
 10)   0.901 us    |      check_nfsd_access [nfsd]();
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)   0.881 us    |        nfsd4_encode_noop [nfsd]();
 10)   2.785 us    |      }
 10)   0.882 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)               |      nfsd4_getattr [nfsd]() {
 10)               |        fh_verify [nfsd]() {
 10)               |          nfsd_setuser_and_check_port [nfsd]() {
 10)   0.892 us    |            nfsexp_flags [nfsd]();
 10)   5.851 us    |            nfsd_setuser [nfsd]();
 10)   0.891 us    |            nfserrno [nfsd]();
 10) + 10.920 us   |          }
 10)   0.882 us    |          check_nfsd_access [nfsd]();
 10)   0.891 us    |          nfsd_permission [nfsd]();
 10) + 16.040 us   |        }
 10) + 17.753 us   |      }
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)               |        nfsd4_encode_getattr [nfsd]() {
 10)               |          nfsd4_encode_fattr [nfsd]() {
 10)   0.952 us    |            nfsd4_encode_bitmap [nfsd]();
 10)   0.892 us    |            fsid_source [nfsd]();
 10)               |            nfsd4_encode_user [nfsd]() {
 10)   2.585 us    |              encode_name_from_id [nfsd]();
 10)   4.529 us    |            }
 10)               |            nfsd4_encode_group [nfsd]() {
 10)   1.302 us    |              encode_name_from_id [nfsd]();
 10)   3.096 us    |            }
 10) + 18.213 us   |          }
 10) + 20.328 us   |        }
 10) + 22.411 us   |      }
 10)   0.921 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)   1.222 us    |      fh_put [nfsd]();
 10)   0.902 us    |      fh_put [nfsd]();
 10) ! 115.583 us  |    }
 10)               |    nfs4svc_encode_compoundres [nfsd]() {
 10)               |      nfsd4_sequence_done [nfsd]() {
 10)   1.102 us    |        copy_cred [nfsd]();
 10)               |        nfsd4_put_session_locked [nfsd]() {
 10)   1.122 us    |          put_client_renew_locked [nfsd]();
 10)   3.006 us    |        }
 10)   7.584 us    |      }
 10)   9.347 us    |    }
 10)   0.982 us    |    nfsd_cache_update [nfsd]();
 10) ! 167.379 us  |  }
 10)   0.922 us    |  nfsd4_release_compoundargs [nfsd]();
 10)   5.129 us    |  nfsd_init_request [nfsd]();
 10)               |  nfsd_dispatch [nfsd]() {
 10)               |    nfs4svc_decode_compoundargs [nfsd]() {
 10)               |      nfsd4_decode_compound.constprop.0 [nfsd]() {
 10)   0.892 us    |        OPDESC [nfsd]();
 10)               |        nfsd4_decode_sequence [nfsd]() {
 10)   0.921 us    |          nfsd4_decode_sessionid4 [nfsd]();
 10)   2.805 us    |        }
 10)   0.892 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   0.881 us    |          nfsd4_sequence_rsize [nfsd]();
 10)   2.875 us    |        }
 10)   0.881 us    |        OPDESC [nfsd]();
 10)               |        nfsd4_decode_putfh [nfsd]() {
 10)   0.892 us    |          svcxdr_savemem [nfsd]();
 10)   2.795 us    |        }
 10)   0.881 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   0.891 us    |          nfsd4_only_status_rsize [nfsd]();
 10)   3.596 us    |        }
 10)   0.882 us    |        OPDESC [nfsd]();
 10)   0.882 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   0.922 us    |          nfsd4_getattr_rsize [nfsd]();
 10)   2.805 us    |        }
 10) + 31.819 us   |      }
 10) + 33.732 us   |    }
 10)   1.092 us    |    nfsd_cache_lookup [nfsd]();
 10)               |    nfsd4_proc_compound [nfsd]() {
 10)   1.072 us    |      nfsd_minorversion [nfsd]();
 10)               |      nfsd4_sequence [nfsd]() {
 10)               |        find_in_sessionid_hashtbl [nfsd]() {
 10)   0.962 us    |          get_client_locked [nfsd]();
 10)   3.716 us    |        }
 10)   7.464 us    |      }
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)   1.213 us    |        nfsd4_encode_sequence [nfsd]();
 10)   3.366 us    |      }
 10)   0.912 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)               |      nfsd4_putfh [nfsd]() {
 10)   0.932 us    |        fh_put [nfsd]();
 10)               |        fh_verify [nfsd]() {
 10)               |          nfsd_set_fh_dentry [nfsd]() {
 10)               |            rqst_exp_find [nfsd]() {
 10)               |              exp_find [nfsd]() {
 10)   2.545 us    |                exp_find_key [nfsd]();
 10)               |                exp_get_by_name [nfsd]() {
 10)   1.032 us    |                  svc_export_match [nfsd]();
 10)   3.206 us    |                }
 10)   8.526 us    |              }
 10) + 10.469 us   |            }
 10) + 14.867 us   |          }
 10)               |          nfsd_setuser_and_check_port [nfsd]() {
 10)   0.902 us    |            nfsexp_flags [nfsd]();
 10)   6.472 us    |            nfsd_setuser [nfsd]();
 10)   0.912 us    |            nfserrno [nfsd]();
 10) + 11.972 us   |          }
 10)   0.912 us    |          nfsd_permission [nfsd]();
 10) + 31.568 us   |        }
 10) + 35.235 us   |      }
 10)   0.891 us    |      clear_current_stateid [nfsd]();
 10)   0.902 us    |      check_nfsd_access [nfsd]();
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)   0.882 us    |        nfsd4_encode_noop [nfsd]();
 10)   2.775 us    |      }
 10)   0.891 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)               |      nfsd4_getattr [nfsd]() {
 10)               |        fh_verify [nfsd]() {
 10)               |          nfsd_setuser_and_check_port [nfsd]() {
 10)   0.881 us    |            nfsexp_flags [nfsd]();
 10)   5.840 us    |            nfsd_setuser [nfsd]();
 10)   0.902 us    |            nfserrno [nfsd]();
 10) + 10.910 us   |          }
 10)   0.881 us    |          check_nfsd_access [nfsd]();
 10)   0.901 us    |          nfsd_permission [nfsd]();
 10) + 16.040 us   |        }
 10) + 17.752 us   |      }
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)               |        nfsd4_encode_getattr [nfsd]() {
 10)               |          nfsd4_encode_fattr [nfsd]() {
 10)   0.942 us    |            nfsd4_encode_bitmap [nfsd]();
 10)   0.892 us    |            fsid_source [nfsd]();
 10)               |            nfsd4_encode_user [nfsd]() {
 10)   2.665 us    |              encode_name_from_id [nfsd]();
 10)   4.688 us    |            }
 10)               |            nfsd4_encode_group [nfsd]() {
 10)   1.312 us    |              encode_name_from_id [nfsd]();
 10)   3.126 us    |            }
 10) + 17.703 us   |          }
 10) + 19.566 us   |        }
 10) + 21.630 us   |      }
 10)   0.902 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)   1.212 us    |      fh_put [nfsd]();
 10)   0.901 us    |      fh_put [nfsd]();
 10) ! 111.606 us  |    }
 10)               |    nfs4svc_encode_compoundres [nfsd]() {
 10)               |      nfsd4_sequence_done [nfsd]() {
 10)   1.112 us    |        copy_cred [nfsd]();
 10)               |        nfsd4_put_session_locked [nfsd]() {
 10)   1.132 us    |          put_client_renew_locked [nfsd]();
 10)   3.026 us    |        }
 10)   7.614 us    |      }
 10)   9.378 us    |    }
 10)   0.992 us    |    nfsd_cache_update [nfsd]();
 10) ! 163.622 us  |  }
 10)   0.922 us    |  nfsd4_release_compoundargs [nfsd]();
 10)   4.528 us    |  nfsd_init_request [nfsd]();
 10)               |  nfsd_dispatch [nfsd]() {
 10)               |    nfs4svc_decode_compoundargs [nfsd]() {
 10)               |      nfsd4_decode_compound.constprop.0 [nfsd]() {
 10)   0.892 us    |        OPDESC [nfsd]();
 10)               |        nfsd4_decode_sequence [nfsd]() {
 10)   0.931 us    |          nfsd4_decode_sessionid4 [nfsd]();
 10)   2.785 us    |        }
 10)   0.891 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   0.891 us    |          nfsd4_sequence_rsize [nfsd]();
 10)   2.865 us    |        }
 10)   0.881 us    |        OPDESC [nfsd]();
 10)               |        nfsd4_decode_putfh [nfsd]() {
 10)   0.882 us    |          svcxdr_savemem [nfsd]();
 10)   2.815 us    |        }
 10)   0.881 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   0.891 us    |          nfsd4_only_status_rsize [nfsd]();
 10)   2.745 us    |        }
 10)   0.881 us    |        OPDESC [nfsd]();
 10)               |        nfsd4_decode_open [nfsd]() {
 10)   0.941 us    |          nfsd4_decode_share_access [nfsd]();
 10)   0.931 us    |          nfsd4_decode_clientid4 [nfsd]();
 10)               |          nfsd4_decode_opaque [nfsd]() {
 10)   0.881 us    |            svcxdr_savemem [nfsd]();
 10)   2.645 us    |          }
 10)   1.392 us    |          nfsd4_decode_fattr4.constprop.0 [nfsd]();
 10)               |          nfsd4_decode_component4 [nfsd]() {
 10)   0.881 us    |            svcxdr_savemem [nfsd]();
 10)   2.715 us    |          }
 10) + 14.497 us   |        }
 10)   0.881 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   0.892 us    |          nfsd4_open_rsize [nfsd]();
 10)   2.595 us    |        }
 10)   0.882 us    |        OPDESC [nfsd]();
 10)   0.892 us    |        nfsd4_decode_noop [nfsd]();
 10)   0.882 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   0.892 us    |          nfsd4_getfh_rsize [nfsd]();
 10)   2.585 us    |        }
 10)   0.881 us    |        OPDESC [nfsd]();
 10)   0.932 us    |        nfsd4_decode_access [nfsd]();
 10)   0.882 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   0.882 us    |          nfsd4_access_rsize [nfsd]();
 10)   2.594 us    |        }
 10)   0.881 us    |        OPDESC [nfsd]();
 10)   0.882 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   0.921 us    |          nfsd4_getattr_rsize [nfsd]();
 10)   2.785 us    |        }
 10) + 70.390 us   |      }
 10) + 72.254 us   |    }
 10)   1.272 us    |    nfsd_cache_lookup [nfsd]();
 10)               |    nfsd4_proc_compound [nfsd]() {
 10)   0.952 us    |      nfsd_minorversion [nfsd]();
 10)               |      nfsd4_sequence [nfsd]() {
 10)               |        find_in_sessionid_hashtbl [nfsd]() {
 10)   0.962 us    |          get_client_locked [nfsd]();
 10)   3.717 us    |        }
 10)   7.254 us    |      }
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)   1.212 us    |        nfsd4_encode_sequence [nfsd]();
 10)   3.195 us    |      }
 10)   0.912 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)               |      nfsd4_putfh [nfsd]() {
 10)   0.942 us    |        fh_put [nfsd]();
 10)               |        fh_verify [nfsd]() {
 10)               |          nfsd_set_fh_dentry [nfsd]() {
 10)               |            rqst_exp_find [nfsd]() {
 10)               |              exp_find [nfsd]() {
 10)   2.424 us    |                exp_find_key [nfsd]();
 10)               |                exp_get_by_name [nfsd]() {
 10)   1.152 us    |                  svc_export_match [nfsd]();
 10)   3.416 us    |                }
 10)   8.626 us    |              }
 10) + 10.609 us   |            }
 10) + 15.899 us   |          }
 10)               |          nfsd_setuser_and_check_port [nfsd]() {
 10)   0.942 us    |            nfsexp_flags [nfsd]();
 10)   6.422 us    |            nfsd_setuser [nfsd]();
 10)   0.922 us    |            nfserrno [nfsd]();
 10) + 12.723 us   |          }
 10)   0.902 us    |          nfsd_permission [nfsd]();
 10) + 33.261 us   |        }
 10) + 36.929 us   |      }
 10)   0.892 us    |      clear_current_stateid [nfsd]();
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)   0.892 us    |        nfsd4_encode_noop [nfsd]();
 10)   2.766 us    |      }
 10)   0.892 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)   0.891 us    |      nfsd4_open_rsize [nfsd]();
 10)   0.902 us    |      nfsd4_check_resp_size [nfsd]();
 10)               |      nfsd4_open [nfsd]() {
 10)               |        nfsd4_process_open1 [nfsd]() {
 10)   0.952 us    |          set_client [nfsd]();
 10)   1.132 us    |          find_openstateowner_str_locked [nfsd]();
 10)   0.912 us    |          find_openstateowner_str_locked [nfsd]();
 10)   3.647 us    |          nfs4_alloc_stid [nfsd]();
 10) + 14.146 us   |        }
 10)   0.941 us    |        check_attr_support.constprop.0 [nfsd]();
 10)               |        do_open_lookup.constprop.0 [nfsd]() {
 10)               |          nfsd4_create_file [nfsd]() {
 10)               |            fh_verify [nfsd]() {
 10)               |              nfsd_setuser_and_check_port [nfsd]() {
 10)   0.891 us    |                nfsexp_flags [nfsd]();
 10)   6.001 us    |                nfsd_setuser [nfsd]();
 10)   0.892 us    |                nfserrno [nfsd]();
 10) + 11.080 us   |              }
 10)   0.902 us    |              check_nfsd_access [nfsd]();
 10)   1.173 us    |              nfsd_permission [nfsd]();
 10) + 16.671 us   |            }
 10)   0.912 us    |            nfsd4_acl_to_attr [nfsd]();
 10)               |            fh_verify [nfsd]() {
 10)               |              nfsd_setuser_and_check_port [nfsd]() {
 10)   0.881 us    |                nfsexp_flags [nfsd]();
 10)   5.690 us    |                nfsd_setuser [nfsd]();
 10)   0.901 us    |                nfserrno [nfsd]();
 10) + 10.750 us   |              }
 10)   0.892 us    |              check_nfsd_access [nfsd]();
 10)               |              nfsd_permission [nfsd]() {
 10)   0.882 us    |                nfsexp_flags [nfsd]();
 10)   2.856 us    |              }
 10) + 17.863 us   |            }
 10)   1.452 us    |            fh_compose [nfsd]();
 10)               |            fh_fill_pre_attrs [nfsd]() {
 10)   0.902 us    |              nfserrno [nfsd]();
 10)   4.648 us    |            }
 10)               |            fh_fill_post_attrs [nfsd]() {
 10)   0.901 us    |              nfserrno [nfsd]();
 10)   3.517 us    |            }
 10)               |            nfsd_create_setattr [nfsd]() {
 10) # 6558.781 us |              commit_metadata [nfsd]();
 10)   1.924 us    |              nfserrno [nfsd]();
 10)   2.484 us    |              commit_metadata [nfsd]();
 10)   1.092 us    |              nfserrno [nfsd]();
 10)               |              fh_update [nfsd]() {
 10)   1.663 us    |                _fh_update.part.0.isra.0 [nfsd]();
 10)   3.908 us    |              }
 10) # 6577.676 us |            }
 10) # 6710.371 us |          }
 10)               |          do_open_permission [nfsd]() {
 10)               |            fh_verify [nfsd]() {
 10)               |              nfsd_setuser_and_check_port [nfsd]() {
 10)   1.002 us    |                nfsexp_flags [nfsd]();
 10) + 11.391 us   |                nfsd_setuser [nfsd]();
 10)   1.002 us    |                nfserrno [nfsd]();
 10) + 17.692 us   |              }
 10)   1.001 us    |              check_nfsd_access [nfsd]();
 10)               |              nfsd_permission [nfsd]() {
 10)   1.041 us    |                nfsexp_flags [nfsd]();
 10)   3.185 us    |              }
 10) + 26.318 us   |            }
 10) + 28.523 us   |          }
 10) # 6743.663 us |        }
 10)               |        nfsd4_process_open2 [nfsd]() {
 10)   3.467 us    |          nfsd4_file_hash_insert [nfsd]();
 10)   1.072 us    |          nfsd4_find_existing_open.isra.0 [nfsd]();
 10)               |          nfs4_get_vfs_file [nfsd]() {
 10)   1.082 us    |            __nfs4_file_get_access [nfsd]();
 10)               |            nfsd_file_acquire_opened [nfsd]() {
 10)               |              nfsd_file_do_acquire [nfsd]() {
 10)               |                fh_verify [nfsd]() {
 10)               |                  nfsd_setuser_and_check_port [nfsd]() {
 10)   1.072 us    |                    nfsexp_flags [nfsd]();
 10)   7.684 us    |                    nfsd_setuser [nfsd]();
 10)   1.122 us    |                    nfserrno [nfsd]();
 10) + 14.237 us   |                  }
 10)   1.122 us    |                  check_nfsd_access [nfsd]();
 10)               |                  nfsd_permission [nfsd]() {
 10)   1.092 us    |                    nfsexp_flags [nfsd]();
 10)   3.186 us    |                  }
 10) + 22.692 us   |                }
 10)   5.460 us    |                nfsd_file_mark_find_or_create.constprop.0 [nfsd]();
 10)   1.283 us    |                nfsd_file_check_write_error [nfsd]();
 10) + 38.071 us   |              }
 10) + 40.305 us   |            }
 10)   1.162 us    |            nfsd_open_break_lease [nfsd]();
 10)   1.082 us    |            nfserrno [nfsd]();
 10) + 50.694 us   |          }
 10)   1.252 us    |          nfs4_inc_and_copy_stateid [nfsd]();
 10)               |          nfs4_open_delegation [nfsd]() {
 10)   1.242 us    |            nfs4_set_delegation [nfsd]();
 10)   4.508 us    |          }
 10)   1.072 us    |          put_nfs4_file [nfsd]();
 10)   1.864 us    |          nfs4_put_stid [nfsd]();
 10) + 74.167 us   |        }
 10)   1.673 us    |        fh_put [nfsd]();
 10)   1.272 us    |        fh_put [nfsd]();
 10)               |        nfsd4_cleanup_open_state [nfsd]() {
 10)   1.333 us    |          nfs4_put_stateowner [nfsd]();
 10)   3.637 us    |        }
 10)   1.223 us    |        nfsd4_bump_seqid [nfsd]();
 10) # 6852.074 us |      }
 10)               |      nfsd4_set_openstateid [nfsd]() {
 10)   1.132 us    |        put_stateid [nfsd]();
 10)   3.436 us    |      }
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)               |        nfsd4_encode_open [nfsd]() {
 10)   1.212 us    |          nfsd4_encode_bitmap [nfsd]();
 10)   4.098 us    |        }
 10)   7.003 us    |      }
 10)   1.142 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)   1.172 us    |      nfsd4_getfh [nfsd]();
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)   1.383 us    |        nfsd4_encode_getfh [nfsd]();
 10)   3.938 us    |      }
 10)   1.102 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)               |      nfsd4_access [nfsd]() {
 10)               |        nfsd_access [nfsd]() {
 10)               |          fh_verify [nfsd]() {
 10)               |            nfsd_setuser_and_check_port [nfsd]() {
 10)   1.102 us    |              nfsexp_flags [nfsd]();
 10)   7.153 us    |              nfsd_setuser [nfsd]();
 10)   1.102 us    |              nfserrno [nfsd]();
 10) + 13.535 us   |            }
 10)   1.103 us    |            check_nfsd_access [nfsd]();
 10)   1.072 us    |            nfsd_permission [nfsd]();
 10) + 19.736 us   |          }
 10)   2.073 us    |          nfsd_permission [nfsd]();
 10)               |          nfsd_permission [nfsd]() {
 10)   1.192 us    |            nfserrno [nfsd]();
 10)   3.457 us    |          }
 10)               |          nfsd_permission [nfsd]() {
 10)   1.132 us    |            nfsexp_flags [nfsd]();
 10)   3.516 us    |          }
 10)               |          nfsd_permission [nfsd]() {
 10)   0.982 us    |            nfsexp_flags [nfsd]();
 10)   3.236 us    |          }
 10)   1.282 us    |          nfsd_permission [nfsd]();
 10)               |          nfsd_permission [nfsd]() {
 10)   1.142 us    |            nfsexp_flags [nfsd]();
 10)   3.406 us    |          }
 10)   1.302 us    |          nfsd_permission [nfsd]();
 10) + 47.487 us   |        }
 10) + 49.641 us   |      }
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)   1.122 us    |        nfsd4_encode_access [nfsd]();
 10)   3.457 us    |      }
 10)   1.092 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)               |      nfsd4_getattr [nfsd]() {
 10)               |        fh_verify [nfsd]() {
 10)               |          nfsd_setuser_and_check_port [nfsd]() {
 10)   1.132 us    |            nfsexp_flags [nfsd]();
 10)   7.183 us    |            nfsd_setuser [nfsd]();
 10)   1.152 us    |            nfserrno [nfsd]();
 10) + 13.745 us   |          }
 10)   1.102 us    |          check_nfsd_access [nfsd]();
 10)   1.162 us    |          nfsd_permission [nfsd]();
 10) + 20.207 us   |        }
 10) + 22.512 us   |      }
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)               |        nfsd4_encode_getattr [nfsd]() {
 10)               |          nfsd4_encode_fattr [nfsd]() {
 10)   1.182 us    |            nfsd4_encode_bitmap [nfsd]();
 10)   1.152 us    |            fsid_source [nfsd]();
 10)               |            nfsd4_encode_user [nfsd]() {
 10)   3.096 us    |              encode_name_from_id [nfsd]();
 10)   5.631 us    |            }
 10)               |            nfsd4_encode_group [nfsd]() {
 10)   1.633 us    |              encode_name_from_id [nfsd]();
 10)   3.626 us    |            }
 10) + 21.610 us   |          }
 10) + 23.774 us   |        }
 10) + 25.998 us   |      }
 10)   0.922 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)   1.152 us    |      fh_put [nfsd]();
 10)   0.952 us    |      fh_put [nfsd]();
 10) # 7061.521 us |    }
 10)               |    nfs4svc_encode_compoundres [nfsd]() {
 10)               |      nfsd4_sequence_done [nfsd]() {
 10)   1.232 us    |        copy_cred [nfsd]();
 10)               |        nfsd4_put_session_locked [nfsd]() {
 10)   1.242 us    |          put_client_renew_locked [nfsd]();
 10)   3.587 us    |        }
 10)   9.427 us    |      }
 10) + 11.371 us   |    }
 10)   0.981 us    |    nfsd_cache_update [nfsd]();
 10) # 7154.271 us |  }
 10)   0.912 us    |  nfsd4_release_compoundargs [nfsd]();
 10)   2.876 us    |  nfsd_init_request [nfsd]();
 10)               |  nfsd_dispatch [nfsd]() {
 10)               |    nfs4svc_decode_compoundargs [nfsd]() {
 10)               |      nfsd4_decode_compound.constprop.0 [nfsd]() {
 10)   0.892 us    |        OPDESC [nfsd]();
 10)               |        nfsd4_decode_sequence [nfsd]() {
 10)   0.922 us    |          nfsd4_decode_sessionid4 [nfsd]();
 10)   2.805 us    |        }
 10)   0.891 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   0.882 us    |          nfsd4_sequence_rsize [nfsd]();
 10)   2.765 us    |        }
 10)   0.881 us    |        OPDESC [nfsd]();
 10)               |        nfsd4_decode_putfh [nfsd]() {
 10)   0.882 us    |          svcxdr_savemem [nfsd]();
 10)   2.795 us    |        }
 10)   0.881 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   0.891 us    |          nfsd4_only_status_rsize [nfsd]();
 10)   2.635 us    |        }
 10)   0.872 us    |        OPDESC [nfsd]();
 10)               |        nfsd4_decode_setattr [nfsd]() {
 10)   0.921 us    |          nfsd4_decode_stateid4 [nfsd]();
 10)   1.503 us    |          nfsd4_decode_fattr4.constprop.0 [nfsd]();
 10)   5.249 us    |        }
 10)   0.882 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   0.881 us    |          nfsd4_setattr_rsize [nfsd]();
 10)   2.575 us    |        }
 10)   0.872 us    |        OPDESC [nfsd]();
 10)   0.882 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   0.922 us    |          nfsd4_getattr_rsize [nfsd]();
 10)   2.785 us    |        }
 10) + 43.831 us   |      }
 10) + 45.685 us   |    }
 10)   1.123 us    |    nfsd_cache_lookup [nfsd]();
 10)               |    nfsd4_proc_compound [nfsd]() {
 10)   1.042 us    |      nfsd_minorversion [nfsd]();
 10)               |      nfsd4_sequence [nfsd]() {
 10)               |        find_in_sessionid_hashtbl [nfsd]() {
 10)   0.961 us    |          get_client_locked [nfsd]();
 10)   3.116 us    |        }
 10)   6.172 us    |      }
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)   1.062 us    |        nfsd4_encode_sequence [nfsd]();
 10)   3.125 us    |      }
 10)   0.912 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)               |      nfsd4_putfh [nfsd]() {
 10)   0.921 us    |        fh_put [nfsd]();
 10)               |        fh_verify [nfsd]() {
 10)               |          nfsd_set_fh_dentry [nfsd]() {
 10)               |            rqst_exp_find [nfsd]() {
 10)               |              exp_find [nfsd]() {
 10)   2.534 us    |                exp_find_key [nfsd]();
 10)               |                exp_get_by_name [nfsd]() {
 10)   0.942 us    |                  svc_export_match [nfsd]();
 10)   3.126 us    |                }
 10)   8.465 us    |              }
 10) + 10.389 us   |            }
 10)   1.042 us    |            nfsd_acceptable [nfsd]();
 10) + 20.097 us   |          }
 10)               |          nfsd_setuser_and_check_port [nfsd]() {
 10)   0.902 us    |            nfsexp_flags [nfsd]();
 10)   6.352 us    |            nfsd_setuser [nfsd]();
 10)   0.912 us    |            nfserrno [nfsd]();
 10) + 11.631 us   |          }
 10)   0.911 us    |          nfsd_permission [nfsd]();
 10) + 36.127 us   |        }
 10) + 39.683 us   |      }
 10)   0.902 us    |      clear_current_stateid [nfsd]();
 10)   0.912 us    |      check_nfsd_access [nfsd]();
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)   0.892 us    |        nfsd4_encode_noop [nfsd]();
 10)   2.695 us    |      }
 10)   0.882 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)   0.892 us    |      nfsd4_setattr_rsize [nfsd]();
 10)   0.911 us    |      nfsd4_check_resp_size [nfsd]();
 10)               |      nfsd4_get_setattrstateid [nfsd]() {
 10)   0.901 us    |        get_stateid [nfsd]();
 10)   2.645 us    |      }
 10)               |      nfsd4_setattr [nfsd]() {
 10)   0.952 us    |        check_attr_support.constprop.0 [nfsd]();
 10)   0.902 us    |        nfsd4_acl_to_attr [nfsd]();
 10)               |        nfsd_setattr [nfsd]() {
 10)               |          fh_verify [nfsd]() {
 10)               |            nfsd_setuser_and_check_port [nfsd]() {
 10)   0.892 us    |              nfsexp_flags [nfsd]();
 10)   5.871 us    |              nfsd_setuser [nfsd]();
 10)   0.901 us    |              nfserrno [nfsd]();
 10) + 11.000 us   |            }
 10)   0.882 us    |            check_nfsd_access [nfsd]();
 10)               |            nfsd_permission [nfsd]() {
 10)   0.882 us    |              nfsexp_flags [nfsd]();
 10)   2.715 us    |            }
 10) + 17.943 us   |          }
 10)               |          __nfsd_setattr [nfsd]() {
 10)   1.103 us    |            nfsd_file_fsnotify_handle_event [nfsd]();
 10) + 33.913 us   |          }
 10) # 4566.993 us |          commit_metadata [nfsd]();
 10)   1.333 us    |          nfserrno [nfsd]();
 10) # 4626.083 us |        }
 10)   0.881 us    |        nfserrno [nfsd]();
 10)   0.881 us    |        nfserrno [nfsd]();
 10) # 4635.671 us |      }
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)   0.942 us    |        nfsd4_encode_setattr [nfsd]();
 10)   2.935 us    |      }
 10)   0.902 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)               |      nfsd4_getattr [nfsd]() {
 10)               |        fh_verify [nfsd]() {
 10)               |          nfsd_setuser_and_check_port [nfsd]() {
 10)   0.892 us    |            nfsexp_flags [nfsd]();
 10)   8.616 us    |            nfsd_setuser [nfsd]();
 10)   0.932 us    |            nfserrno [nfsd]();
 10) + 13.886 us   |          }
 10)   0.891 us    |          check_nfsd_access [nfsd]();
 10)   0.912 us    |          nfsd_permission [nfsd]();
 10) + 19.195 us   |        }
 10) + 20.959 us   |      }
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)               |        nfsd4_encode_getattr [nfsd]() {
 10)               |          nfsd4_encode_fattr [nfsd]() {
 10)   0.981 us    |            nfsd4_encode_bitmap [nfsd]();
 10)   0.891 us    |            fsid_source [nfsd]();
 10)               |            nfsd4_encode_user [nfsd]() {
 10)   2.144 us    |              encode_name_from_id [nfsd]();
 10)   4.749 us    |            }
 10)               |            nfsd4_encode_group [nfsd]() {
 10)   1.292 us    |              encode_name_from_id [nfsd]();
 10)   3.086 us    |            }
 10) + 17.161 us   |          }
 10) + 18.915 us   |        }
 10) + 20.738 us   |      }
 10)   0.902 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)   1.252 us    |      fh_put [nfsd]();
 10)   0.922 us    |      fh_put [nfsd]();
 10) # 4766.292 us |    }
 10)               |    nfs4svc_encode_compoundres [nfsd]() {
 10)               |      nfsd4_sequence_done [nfsd]() {
 10)   1.092 us    |        copy_cred [nfsd]();
 10)               |        nfsd4_put_session_locked [nfsd]() {
 10)   1.162 us    |          put_client_renew_locked [nfsd]();
 10)   2.985 us    |        }
 10)   8.265 us    |      }
 10) + 10.078 us   |    }
 10)   0.982 us    |    nfsd_cache_update [nfsd]();
 10) # 4829.909 us |  }
 10)   0.912 us    |  nfsd4_release_compoundargs [nfsd]();
 10)   1.864 us    |  nfsd_init_request [nfsd]();
 10)               |  nfsd_dispatch [nfsd]() {
 10)               |    nfs4svc_decode_compoundargs [nfsd]() {
 10)               |      nfsd4_decode_compound.constprop.0 [nfsd]() {
 10)   0.911 us    |        OPDESC [nfsd]();
 10)               |        nfsd4_decode_sequence [nfsd]() {
 10)   0.931 us    |          nfsd4_decode_sessionid4 [nfsd]();
 10)   2.705 us    |        }
 10)   0.902 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   0.892 us    |          nfsd4_sequence_rsize [nfsd]();
 10)   2.685 us    |        }
 10)   0.872 us    |        OPDESC [nfsd]();
 10)               |        nfsd4_decode_putfh [nfsd]() {
 10)   0.892 us    |          svcxdr_savemem [nfsd]();
 10)   2.725 us    |        }
 10)   0.881 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   0.901 us    |          nfsd4_only_status_rsize [nfsd]();
 10)   2.655 us    |        }
 10)   0.872 us    |        OPDESC [nfsd]();
 10)   0.891 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   0.951 us    |          nfsd4_getattr_rsize [nfsd]();
 10)   2.695 us    |        }
 10)   0.882 us    |        OPDESC [nfsd]();
 10)               |        nfsd4_decode_close [nfsd]() {
 10)   0.921 us    |          nfsd4_decode_stateid4 [nfsd]();
 10)   2.695 us    |        }
 10)   0.882 us    |        nfsd4_cache_this_op [nfsd]();
 10)               |        nfsd4_max_reply [nfsd]() {
 10)   0.892 us    |          nfsd4_status_stateid_rsize [nfsd]();
 10)   2.605 us    |        }
 10) + 39.784 us   |      }
 10) + 41.566 us   |    }
 10)   1.012 us    |    nfsd_cache_lookup [nfsd]();
 10)               |    nfsd4_proc_compound [nfsd]() {
 10)   0.951 us    |      nfsd_minorversion [nfsd]();
 10)               |      nfsd4_sequence [nfsd]() {
 10)               |        find_in_sessionid_hashtbl [nfsd]() {
 10)   0.962 us    |          get_client_locked [nfsd]();
 10)   3.026 us    |        }
 10)   5.831 us    |      }
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)   1.022 us    |        nfsd4_encode_sequence [nfsd]();
 10)   2.865 us    |      }
 10)   0.891 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)               |      nfsd4_putfh [nfsd]() {
 10)   0.891 us    |        fh_put [nfsd]();
 10)               |        fh_verify [nfsd]() {
 10)               |          nfsd_set_fh_dentry [nfsd]() {
 10)               |            rqst_exp_find [nfsd]() {
 10)               |              exp_find [nfsd]() {
 10)   1.864 us    |                exp_find_key [nfsd]();
 10)               |                exp_get_by_name [nfsd]() {
 10)   0.952 us    |                  svc_export_match [nfsd]();
 10)   3.066 us    |                }
 10)   7.604 us    |              }
 10)   9.458 us    |            }
 10)   1.032 us    |            nfsd_acceptable [nfsd]();
 10) + 16.831 us   |          }
 10)               |          nfsd_setuser_and_check_port [nfsd]() {
 10)   1.433 us    |            nfsexp_flags [nfsd]();
 10)   5.961 us    |            nfsd_setuser [nfsd]();
 10)   0.891 us    |            nfserrno [nfsd]();
 10) + 11.682 us   |          }
 10)   0.892 us    |          nfsd_permission [nfsd]();
 10) + 32.831 us   |        }
 10) + 36.317 us   |      }
 10)   0.901 us    |      clear_current_stateid [nfsd]();
 10)   0.882 us    |      check_nfsd_access [nfsd]();
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)   0.892 us    |        nfsd4_encode_noop [nfsd]();
 10)   2.695 us    |      }
 10)   0.891 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)               |      nfsd4_getattr [nfsd]() {
 10)               |        fh_verify [nfsd]() {
 10)               |          nfsd_setuser_and_check_port [nfsd]() {
 10)   0.902 us    |            nfsexp_flags [nfsd]();
 10)   5.680 us    |            nfsd_setuser [nfsd]();
 10)   0.902 us    |            nfserrno [nfsd]();
 10) + 10.770 us   |          }
 10)   0.881 us    |          check_nfsd_access [nfsd]();
 10)   0.891 us    |          nfsd_permission [nfsd]();
 10) + 15.869 us   |        }
 10) + 17.562 us   |      }
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)               |        nfsd4_encode_getattr [nfsd]() {
 10)               |          nfsd4_encode_fattr [nfsd]() {
 10)   0.922 us    |            nfsd4_encode_bitmap [nfsd]();
 10)   4.088 us    |          }
 10)   5.780 us    |        }
 10)   7.554 us    |      }
 10)   0.892 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)   0.881 us    |      nfsd4_status_stateid_rsize [nfsd]();
 10)   0.912 us    |      nfsd4_check_resp_size [nfsd]();
 10)               |      nfsd4_get_closestateid [nfsd]() {
 10)   0.912 us    |        get_stateid [nfsd]();
 10)   2.625 us    |      }
 10)               |      nfsd4_close [nfsd]() {
 10)               |        nfs4_preprocess_seqid_op [nfsd]() {
 10)               |          nfsd4_lookup_stateid [nfsd]() {
 10)   1.052 us    |            set_client [nfsd]();
 10)   1.423 us    |            find_stateid_by_type [nfsd]();
 10)   5.290 us    |          }
 10)   7.594 us    |        }
 10)   0.942 us    |        nfsd4_bump_seqid [nfsd]();
 10)   0.982 us    |        nfs4_inc_and_copy_stateid [nfsd]();
 10)   0.992 us    |        unhash_open_stateid [nfsd]();
 10)   1.192 us    |        put_ol_stateid_locked [nfsd]();
 10)   0.932 us    |        free_ol_stateid_reaplist [nfsd]();
 10)               |        nfs4_put_stid [nfsd]() {
 10)               |          nfs4_free_ol_stateid [nfsd]() {
 10)               |            release_all_access [nfsd]() {
 10)               |              nfs4_file_put_access [nfsd]() {
 10)               |                nfsd_file_put [nfsd]() {
 10)               |                  nfsd_file_free [nfsd]() {
 10)   1.322 us    |                    nfsd_file_unhash [nfsd]();
 10)   1.092 us    |                    nfsd_file_check_write_error [nfsd]();
 10) + 17.683 us   |                  }
 10) + 19.726 us   |                }
 10) + 21.710 us   |              }
 10) + 23.734 us   |            }
 10)               |            nfs4_put_stateowner [nfsd]() {
 10)   0.902 us    |              nfs4_unhash_openowner [nfsd]();
 10)   1.453 us    |              nfs4_free_openowner [nfsd]();
 10)   5.751 us    |            }
 10) + 32.710 us   |          }
 10)               |          put_nfs4_file [nfsd]() {
 10)   1.373 us    |            nfsd4_file_hash_remove [nfsd]();
 10)   3.336 us    |          }
 10) + 40.234 us   |        }
 10) + 60.451 us   |      }
 10)               |      nfsd4_set_closestateid [nfsd]() {
 10)   0.901 us    |        put_stateid [nfsd]();
 10)   2.615 us    |      }
 10)               |      nfsd4_encode_operation [nfsd]() {
 10)   1.032 us    |        nfsd4_encode_close [nfsd]();
 10)   2.826 us    |      }
 10)   0.892 us    |      nfsd4_cstate_clear_replay [nfsd]();
 10)   1.112 us    |      fh_put [nfsd]();
 10)   0.902 us    |      fh_put [nfsd]();
 10) ! 170.986 us  |    }
 10)               |    nfs4svc_encode_compoundres [nfsd]() {
 10)               |      nfsd4_sequence_done [nfsd]() {
 10)   0.982 us    |        copy_cred [nfsd]();
 10)               |        nfsd4_put_session_locked [nfsd]() {
 10)   1.062 us    |          put_client_renew_locked [nfsd]();
 10)   2.856 us    |        }
 10)   7.434 us    |      }
 10)   9.167 us    |    }
 10)   0.972 us    |    nfsd_cache_update [nfsd]();
 10) ! 228.972 us  |  }
 10)   0.912 us    |  nfsd4_release_compoundargs [nfsd]();
 ------------------------------------------
 13) rpc.mou-352326 => kworker-359599
 ------------------------------------------

 13)   7.504 us    |  nfsd_file_mark_free [nfsd]();
 14)   6.672 us    |  nfsd_init_request [nfsd]();
 14)               |  nfsd_dispatch [nfsd]() {
 14)               |    nfs4svc_decode_compoundargs [nfsd]() {
 14)               |      nfsd4_decode_compound.constprop.0 [nfsd]() {
 14)   1.222 us    |        OPDESC [nfsd]();
 14)               |        nfsd4_decode_sequence [nfsd]() {
 14)   1.263 us    |          nfsd4_decode_sessionid4 [nfsd]();
 14)   3.657 us    |        }
 14)   1.272 us    |        nfsd4_cache_this_op [nfsd]();
 14)               |        nfsd4_max_reply [nfsd]() {
 14)   1.432 us    |          nfsd4_sequence_rsize [nfsd]();
 14)   4.077 us    |        }
 14)   1.182 us    |        OPDESC [nfsd]();
 14)               |        nfsd4_decode_putfh [nfsd]() {
 14)   1.142 us    |          svcxdr_savemem [nfsd]();
 14)   3.707 us    |        }
 14)   1.193 us    |        nfsd4_cache_this_op [nfsd]();
 14)               |        nfsd4_max_reply [nfsd]() {
 14)   1.142 us    |          nfsd4_only_status_rsize [nfsd]();
 14)   3.456 us    |        }
 14)   1.052 us    |        OPDESC [nfsd]();
 14)   1.192 us    |        nfsd4_cache_this_op [nfsd]();
 14)               |        nfsd4_max_reply [nfsd]() {
 14)   1.202 us    |          nfsd4_getattr_rsize [nfsd]();
 14)   3.527 us    |        }
 14) + 40.835 us   |      }
 14) + 43.470 us   |    }
 14)   1.864 us    |    nfsd_cache_lookup [nfsd]();
 14)               |    nfsd4_proc_compound [nfsd]() {
 14)   1.533 us    |      nfsd_minorversion [nfsd]();
 14)               |      nfsd4_sequence [nfsd]() {
 14)               |        find_in_sessionid_hashtbl [nfsd]() {
 14)   1.392 us    |          get_client_locked [nfsd]();
 14)   4.498 us    |        }
 14)   9.558 us    |      }
 14)               |      nfsd4_encode_operation [nfsd]() {
 14)   1.664 us    |        nfsd4_encode_sequence [nfsd]();
 14)   4.549 us    |      }
 14)   1.162 us    |      nfsd4_cstate_clear_replay [nfsd]();
 14)               |      nfsd4_putfh [nfsd]() {
 14)   1.232 us    |        fh_put [nfsd]();
 14)               |        fh_verify [nfsd]() {
 14)               |          nfsd_set_fh_dentry [nfsd]() {
 14)               |            rqst_exp_find [nfsd]() {
 14)               |              exp_find [nfsd]() {
 14)   3.316 us    |                exp_find_key [nfsd]();
 14)               |                exp_get_by_name [nfsd]() {
 14)   2.464 us    |                  svc_export_match [nfsd]();
 14)   5.640 us    |                }
 14) + 12.844 us   |              }
 14) + 15.469 us   |            }
 14) + 22.551 us   |          }
 14)               |          nfsd_setuser_and_check_port [nfsd]() {
 14)   1.072 us    |            nfsexp_flags [nfsd]();
 14)   8.596 us    |            nfsd_setuser [nfsd]();
 14)   1.192 us    |            nfserrno [nfsd]();
 14) + 15.990 us   |          }
 14)   1.142 us    |          nfsd_permission [nfsd]();
 14) + 45.634 us   |        }
 14) + 50.573 us   |      }
 14)   1.172 us    |      clear_current_stateid [nfsd]();
 14)   1.142 us    |      check_nfsd_access [nfsd]();
 14)               |      nfsd4_encode_operation [nfsd]() {
 14)   1.223 us    |        nfsd4_encode_noop [nfsd]();
 14)   3.727 us    |      }
 14)   1.153 us    |      nfsd4_cstate_clear_replay [nfsd]();
 14)               |      nfsd4_getattr [nfsd]() {
 14)               |        fh_verify [nfsd]() {
 14)               |          nfsd_setuser_and_check_port [nfsd]() {
 14)   1.162 us    |            nfsexp_flags [nfsd]();
 14)   7.323 us    |            nfsd_setuser [nfsd]();
 14)   1.222 us    |            nfserrno [nfsd]();
 14) + 13.946 us   |          }
 14)   2.214 us    |          check_nfsd_access [nfsd]();
 14)   1.183 us    |          nfsd_permission [nfsd]();
 14) + 21.790 us   |        }
 14) + 24.215 us   |      }
 14)               |      nfsd4_encode_operation [nfsd]() {
 14)               |        nfsd4_encode_getattr [nfsd]() {
 14)               |          nfsd4_encode_fattr [nfsd]() {
 14)   1.322 us    |            nfsd4_encode_bitmap [nfsd]();
 14)   1.272 us    |            fsid_source [nfsd]();
 14)               |            nfsd4_encode_user [nfsd]() {
 14)   3.637 us    |              encode_name_from_id [nfsd]();
 14)   6.252 us    |            }
 14)               |            nfsd4_encode_group [nfsd]() {
 14)   1.703 us    |              encode_name_from_id [nfsd]();
 14)   4.157 us    |            }
 14) + 23.524 us   |          }
 14) + 26.069 us   |        }
 14) + 29.054 us   |      }
 14)   1.102 us    |      nfsd4_cstate_clear_replay [nfsd]();
 14)   1.443 us    |      fh_put [nfsd]();
 14)   1.182 us    |      fh_put [nfsd]();
 14) ! 151.279 us  |    }
 14)               |    nfs4svc_encode_compoundres [nfsd]() {
 14)               |      nfsd4_sequence_done [nfsd]() {
 14)   1.633 us    |        copy_cred [nfsd]();
 14)               |        nfsd4_put_session_locked [nfsd]() {
 14)   1.493 us    |          put_client_renew_locked [nfsd]();
 14)   4.047 us    |        }
 14) + 10.620 us   |      }
 14) + 13.125 us   |    }
 14)   1.262 us    |    nfsd_cache_update [nfsd]();
 14) ! 218.494 us  |  }
 14)   1.162 us    |  nfsd4_release_compoundargs [nfsd]();
 ------------------------------------------
 10)  nfsd-352337   =>  bash-386416
 ------------------------------------------

 10)   4.969 us    |  nfsd_file_slab_free [nfsd]();
 10)   1.092 us    |  nfsd4_free_file_rcu [nfsd]();



<!-- 删除文件 -->
 ------------------------------------------
  6)  nfsd-352337   => kworker-198477
 ------------------------------------------

  6)               |  laundromat_main [nfsd]() {
  6)               |    nfs4_laundromat [nfsd]() {
  6)   1.222 us    |      nfsd4_end_grace [nfsd]();
  6)   1.974 us    |      nfs4_get_client_reaplist [nfsd]();
  6)   1.022 us    |      nfs4_process_client_reaplist [nfsd]();
  6) + 12.553 us   |    }
  6) + 22.662 us   |  }
 14)   6.973 us    |  nfsd_init_request [nfsd]();
 14)               |  nfsd_dispatch [nfsd]() {
 14)               |    nfs4svc_decode_compoundargs [nfsd]() {
 14)               |      nfsd4_decode_compound.constprop.0 [nfsd]() {
 14)   0.921 us    |        OPDESC [nfsd]();
 14)               |        nfsd4_decode_sequence [nfsd]() {
 14)   0.961 us    |          nfsd4_decode_sessionid4 [nfsd]();
 14)   2.885 us    |        }
 14)   0.892 us    |        nfsd4_cache_this_op [nfsd]();
 14)               |        nfsd4_max_reply [nfsd]() {
 14)   0.892 us    |          nfsd4_sequence_rsize [nfsd]();
 14)   2.995 us    |        }
 14) + 13.255 us   |      }
 14) + 15.328 us   |    }
 14)   1.242 us    |    nfsd_cache_lookup [nfsd]();
 14)               |    nfsd4_proc_compound [nfsd]() {
 14)   1.252 us    |      nfsd_minorversion [nfsd]();
 14)               |      nfsd4_sequence [nfsd]() {
 14)               |        find_in_sessionid_hashtbl [nfsd]() {
 14)   0.962 us    |          get_client_locked [nfsd]();
 14)   4.499 us    |        }
 14)   9.237 us    |      }
 14)               |      nfsd4_encode_operation [nfsd]() {
 14)   1.272 us    |        nfsd4_encode_sequence [nfsd]();
 14)   3.947 us    |      }
 14)   0.912 us    |      nfsd4_cstate_clear_replay [nfsd]();
 14)   0.922 us    |      fh_put [nfsd]();
 14)   0.882 us    |      fh_put [nfsd]();
 14) + 25.266 us   |    }
 14)               |    nfs4svc_encode_compoundres [nfsd]() {
 14)               |      nfsd4_sequence_done [nfsd]() {
 14)   1.252 us    |        copy_cred [nfsd]();
 14)               |        nfsd4_put_session_locked [nfsd]() {
 14)   1.292 us    |          put_client_renew_locked [nfsd]();
 14)   3.326 us    |        }
 14)   9.607 us    |      }
 14) + 11.481 us   |    }
 14)   0.982 us    |    nfsd_cache_update [nfsd]();
 14) + 61.253 us   |  }
 14)   0.911 us    |  nfsd4_release_compoundargs [nfsd]();
 ------------------------------------------
  2) kworker-348451 =>  nfsd-352337
 ------------------------------------------

  2)   8.376 us    |  nfsd_init_request [nfsd]();
  2)               |  nfsd_dispatch [nfsd]() {
  2)               |    nfs4svc_decode_compoundargs [nfsd]() {
  2)               |      nfsd4_decode_compound.constprop.0 [nfsd]() {
  2)   0.881 us    |        OPDESC [nfsd]();
  2)               |        nfsd4_decode_sequence [nfsd]() {
  2)   0.921 us    |          nfsd4_decode_sessionid4 [nfsd]();
  2)   2.936 us    |        }
  2)   0.892 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.882 us    |          nfsd4_sequence_rsize [nfsd]();
  2)   3.006 us    |        }
  2)   0.882 us    |        OPDESC [nfsd]();
  2)               |        nfsd4_decode_putfh [nfsd]() {
  2)   0.882 us    |          svcxdr_savemem [nfsd]();
  2)   2.805 us    |        }
  2)   0.881 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.881 us    |          nfsd4_only_status_rsize [nfsd]();
  2)   2.715 us    |        }
  2)   0.871 us    |        OPDESC [nfsd]();
  2)   0.882 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.922 us    |          nfsd4_getattr_rsize [nfsd]();
  2)   2.776 us    |        }
  2) + 31.197 us   |      }
  2) + 33.272 us   |    }
  2)   1.152 us    |    nfsd_cache_lookup [nfsd]();
  2)               |    nfsd4_proc_compound [nfsd]() {
  2)   1.082 us    |      nfsd_minorversion [nfsd]();
  2)               |      nfsd4_sequence [nfsd]() {
  2)               |        find_in_sessionid_hashtbl [nfsd]() {
  2)   0.962 us    |          get_client_locked [nfsd]();
  2)   3.717 us    |        }
  2)   7.865 us    |      }
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)   1.212 us    |        nfsd4_encode_sequence [nfsd]();
  2)   3.366 us    |      }
  2)   0.901 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)               |      nfsd4_putfh [nfsd]() {
  2)   0.912 us    |        fh_put [nfsd]();
  2)               |        fh_verify [nfsd]() {
  2)               |          nfsd_set_fh_dentry [nfsd]() {
  2)               |            rqst_exp_find [nfsd]() {
  2)               |              exp_find [nfsd]() {
  2)   2.434 us    |                exp_find_key [nfsd]();
  2)               |                exp_get_by_name [nfsd]() {
  2)   1.193 us    |                  svc_export_match [nfsd]();
  2)   3.757 us    |                }
  2)   8.986 us    |              }
  2) + 10.931 us   |            }
  2) + 17.853 us   |          }
  2)               |          nfsd_setuser_and_check_port [nfsd]() {
  2)   0.942 us    |            nfsexp_flags [nfsd]();
  2)   6.842 us    |            nfsd_setuser [nfsd]();
  2)   0.922 us    |            nfserrno [nfsd]();
  2) + 12.934 us   |          }
  2)   1.012 us    |          nfsd_permission [nfsd]();
  2) + 35.736 us   |        }
  2) + 39.573 us   |      }
  2)   0.891 us    |      clear_current_stateid [nfsd]();
  2)   0.902 us    |      check_nfsd_access [nfsd]();
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)   0.882 us    |        nfsd4_encode_noop [nfsd]();
  2)   2.785 us    |      }
  2)   0.881 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)               |      nfsd4_getattr [nfsd]() {
  2)               |        fh_verify [nfsd]() {
  2)               |          nfsd_setuser_and_check_port [nfsd]() {
  2)   0.872 us    |            nfsexp_flags [nfsd]();
  2)   6.121 us    |            nfsd_setuser [nfsd]();
  2)   0.892 us    |            nfserrno [nfsd]();
  2) + 11.160 us   |          }
  2)   0.882 us    |          check_nfsd_access [nfsd]();
  2)   0.882 us    |          nfsd_permission [nfsd]();
  2) + 16.240 us   |        }
  2) + 17.923 us   |      }
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)               |        nfsd4_encode_getattr [nfsd]() {
  2)               |          nfsd4_encode_fattr [nfsd]() {
  2)   0.961 us    |            nfsd4_encode_bitmap [nfsd]();
  2)   0.902 us    |            fsid_source [nfsd]();
  2)               |            nfsd4_encode_user [nfsd]() {
  2)   2.594 us    |              encode_name_from_id [nfsd]();
  2)   4.548 us    |            }
  2)               |            nfsd4_encode_group [nfsd]() {
  2)   1.293 us    |              encode_name_from_id [nfsd]();
  2)   3.095 us    |            }
  2) + 18.124 us   |          }
  2) + 19.956 us   |        }
  2) + 22.221 us   |      }
  2)   0.892 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)   1.182 us    |      fh_put [nfsd]();
  2)   0.911 us    |      fh_put [nfsd]();
  2) ! 117.447 us  |    }
  2)               |    nfs4svc_encode_compoundres [nfsd]() {
  2)               |      nfsd4_sequence_done [nfsd]() {
  2)   1.092 us    |        copy_cred [nfsd]();
  2)               |        nfsd4_put_session_locked [nfsd]() {
  2)   1.142 us    |          put_client_renew_locked [nfsd]();
  2)   3.036 us    |        }
  2)   7.594 us    |      }
  2)   9.368 us    |    }
  2)   0.982 us    |    nfsd_cache_update [nfsd]();
  2) ! 168.632 us  |  }
  2)   0.912 us    |  nfsd4_release_compoundargs [nfsd]();
  2)   1.162 us    |  nfsd_init_request [nfsd]();
  2)               |  nfsd_dispatch [nfsd]() {
  2)               |    nfs4svc_decode_compoundargs [nfsd]() {
  2)               |      nfsd4_decode_compound.constprop.0 [nfsd]() {
  2)   0.641 us    |        OPDESC [nfsd]();
  2)               |        nfsd4_decode_sequence [nfsd]() {
  2)   0.662 us    |          nfsd4_decode_sessionid4 [nfsd]();
  2)   1.944 us    |        }
  2)   0.651 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.652 us    |          nfsd4_sequence_rsize [nfsd]();
  2)   1.934 us    |        }
  2)   0.641 us    |        OPDESC [nfsd]();
  2)               |        nfsd4_decode_putfh [nfsd]() {
  2)   0.652 us    |          svcxdr_savemem [nfsd]();
  2)   1.914 us    |        }
  2)   0.651 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.651 us    |          nfsd4_only_status_rsize [nfsd]();
  2)   1.904 us    |        }
  2)   0.651 us    |        OPDESC [nfsd]();
  2)   0.651 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.671 us    |          nfsd4_getattr_rsize [nfsd]();
  2)   1.924 us    |        }
  2) + 21.220 us   |      }
  2) + 22.472 us   |    }
  2)   0.731 us    |    nfsd_cache_lookup [nfsd]();
  2)               |    nfsd4_proc_compound [nfsd]() {
  2)   0.671 us    |      nfsd_minorversion [nfsd]();
  2)               |      nfsd4_sequence [nfsd]() {
  2)               |        find_in_sessionid_hashtbl [nfsd]() {
  2)   0.692 us    |          get_client_locked [nfsd]();
  2)   2.014 us    |        }
  2)   3.978 us    |      }
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)   0.751 us    |        nfsd4_encode_sequence [nfsd]();
  2)   2.043 us    |      }
  2)   0.651 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)               |      nfsd4_putfh [nfsd]() {
  2)   0.681 us    |        fh_put [nfsd]();
  2)               |        fh_verify [nfsd]() {
  2)               |          nfsd_set_fh_dentry [nfsd]() {
  2)               |            rqst_exp_find [nfsd]() {
  2)               |              exp_find [nfsd]() {
  2)   1.062 us    |                exp_find_key [nfsd]();
  2)               |                exp_get_by_name [nfsd]() {
  2)   0.671 us    |                  svc_export_match [nfsd]();
  2)   2.174 us    |                }
  2)   5.130 us    |              }
  2)   6.422 us    |            }
  2)   8.646 us    |          }
  2)               |          nfsd_setuser_and_check_port [nfsd]() {
  2)   0.641 us    |            nfsexp_flags [nfsd]();
  2)   4.077 us    |            nfsd_setuser [nfsd]();
  2)   0.671 us    |            nfserrno [nfsd]();
  2)   7.845 us    |          }
  2)   0.661 us    |          nfsd_permission [nfsd]();
  2) + 20.297 us   |        }
  2) + 22.882 us   |      }
  2)   0.642 us    |      clear_current_stateid [nfsd]();
  2)   0.661 us    |      check_nfsd_access [nfsd]();
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)   0.642 us    |        nfsd4_encode_noop [nfsd]();
  2)   1.954 us    |      }
  2)   0.641 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)               |      nfsd4_getattr [nfsd]() {
  2)               |        fh_verify [nfsd]() {
  2)               |          nfsd_setuser_and_check_port [nfsd]() {
  2)   0.641 us    |            nfsexp_flags [nfsd]();
  2)   4.158 us    |            nfsd_setuser [nfsd]();
  2)   0.651 us    |            nfserrno [nfsd]();
  2)   7.874 us    |          }
  2)   0.652 us    |          check_nfsd_access [nfsd]();
  2)   0.651 us    |          nfsd_permission [nfsd]();
  2) + 11.601 us   |        }
  2) + 12.844 us   |      }
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)               |        nfsd4_encode_getattr [nfsd]() {
  2)               |          nfsd4_encode_fattr [nfsd]() {
  2)   0.671 us    |            nfsd4_encode_bitmap [nfsd]();
  2)   0.651 us    |            fsid_source [nfsd]();
  2)               |            nfsd4_encode_user [nfsd]() {
  2)   1.102 us    |              encode_name_from_id [nfsd]();
  2)   2.494 us    |            }
  2)               |            nfsd4_encode_group [nfsd]() {
  2)   0.931 us    |              encode_name_from_id [nfsd]();
  2)   2.204 us    |            }
  2) + 10.379 us   |          }
  2) + 11.661 us   |        }
  2) + 12.964 us   |      }
  2)   0.651 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)   0.851 us    |      fh_put [nfsd]();
  2)   0.672 us    |      fh_put [nfsd]();
  2) + 71.703 us   |    }
  2)               |    nfs4svc_encode_compoundres [nfsd]() {
  2)               |      nfsd4_sequence_done [nfsd]() {
  2)   0.721 us    |        copy_cred [nfsd]();
  2)               |        nfsd4_put_session_locked [nfsd]() {
  2)   0.771 us    |          put_client_renew_locked [nfsd]();
  2)   2.064 us    |        }
  2)   4.939 us    |      }
  2)   6.221 us    |    }
  2)   0.712 us    |    nfsd_cache_update [nfsd]();
  2) ! 105.525 us  |  }
  2)   0.651 us    |  nfsd4_release_compoundargs [nfsd]();
  2)   1.884 us    |  nfsd_init_request [nfsd]();
  2)               |  nfsd_dispatch [nfsd]() {
  2)               |    nfs4svc_decode_compoundargs [nfsd]() {
  2)               |      nfsd4_decode_compound.constprop.0 [nfsd]() {
  2)   0.642 us    |        OPDESC [nfsd]();
  2)               |        nfsd4_decode_sequence [nfsd]() {
  2)   0.681 us    |          nfsd4_decode_sessionid4 [nfsd]();
  2)   2.004 us    |        }
  2)   0.651 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.642 us    |          nfsd4_sequence_rsize [nfsd]();
  2)   1.984 us    |        }
  2)   0.641 us    |        OPDESC [nfsd]();
  2)               |        nfsd4_decode_putfh [nfsd]() {
  2)   0.651 us    |          svcxdr_savemem [nfsd]();
  2)   1.964 us    |        }
  2)   0.651 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.642 us    |          nfsd4_only_status_rsize [nfsd]();
  2)   1.914 us    |        }
  2)   0.651 us    |        OPDESC [nfsd]();
  2)               |        nfsd4_decode_readdir [nfsd]() {
  2)   0.681 us    |          nfsd4_decode_verifier4 [nfsd]();
  2)   2.234 us    |        }
  2)   0.651 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.772 us    |          nfsd4_readdir_rsize [nfsd]();
  2)   2.284 us    |        }
  2) + 24.525 us   |      }
  2) + 25.837 us   |    }
  2)   0.712 us    |    nfsd_cache_lookup [nfsd]();
  2)               |    nfsd4_proc_compound [nfsd]() {
  2)   0.672 us    |      nfsd_minorversion [nfsd]();
  2)               |      nfsd4_sequence [nfsd]() {
  2)               |        find_in_sessionid_hashtbl [nfsd]() {
  2)   0.691 us    |          get_client_locked [nfsd]();
  2)   2.094 us    |        }
  2)   4.318 us    |      }
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)   0.762 us    |        nfsd4_encode_sequence [nfsd]();
  2)   2.074 us    |      }
  2)   0.651 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)               |      nfsd4_putfh [nfsd]() {
  2)   0.651 us    |        fh_put [nfsd]();
  2)               |        fh_verify [nfsd]() {
  2)               |          nfsd_set_fh_dentry [nfsd]() {
  2)               |            rqst_exp_find [nfsd]() {
  2)               |              exp_find [nfsd]() {
  2)   1.112 us    |                exp_find_key [nfsd]();
  2)               |                exp_get_by_name [nfsd]() {
  2)   0.661 us    |                  svc_export_match [nfsd]();
  2)   2.194 us    |                }
  2)   5.199 us    |              }
  2)   6.492 us    |            }
  2)   8.716 us    |          }
  2)               |          nfsd_setuser_and_check_port [nfsd]() {
  2)   0.651 us    |            nfsexp_flags [nfsd]();
  2)   4.128 us    |            nfsd_setuser [nfsd]();
  2)   0.671 us    |            nfserrno [nfsd]();
  2)   7.894 us    |          }
  2)   0.652 us    |          nfsd_permission [nfsd]();
  2) + 19.757 us   |        }
  2) + 22.301 us   |      }
  2)   0.651 us    |      clear_current_stateid [nfsd]();
  2)   0.661 us    |      check_nfsd_access [nfsd]();
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)   0.641 us    |        nfsd4_encode_noop [nfsd]();
  2)   1.954 us    |      }
  2)   0.661 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)   0.681 us    |      nfsd4_readdir [nfsd]();
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)               |        nfsd4_encode_readdir [nfsd]() {
  2)               |          nfsd_readdir [nfsd]() {
  2)               |            nfsd_open [nfsd]() {
  2)               |              fh_verify [nfsd]() {
  2)               |                nfsd_setuser_and_check_port [nfsd]() {
  2)   0.642 us    |                  nfsexp_flags [nfsd]();
  2)   4.147 us    |                  nfsd_setuser [nfsd]();
  2)   0.662 us    |                  nfserrno [nfsd]();
  2)   7.905 us    |                }
  2)   0.651 us    |                check_nfsd_access [nfsd]();
  2)   0.872 us    |                nfsd_permission [nfsd]();
  2) + 11.882 us   |              }
  2)               |              __nfsd_open.constprop.0 [nfsd]() {
  2)   0.661 us    |                nfsd_open_break_lease.part.0 [nfsd]();
  2)   0.742 us    |                nfserrno [nfsd]();
  2)   6.933 us    |              }
  2) + 20.788 us   |            }
  2)               |            nfsd_buffered_readdir [nfsd]() {
  2)   0.802 us    |              nfsd_buffered_filldir [nfsd]();
  2)   0.691 us    |              nfsd_buffered_filldir [nfsd]();
  2)   0.692 us    |              nfsd_buffered_filldir [nfsd]();
  2)   0.681 us    |              nfsd_buffered_filldir [nfsd]();
  2)   0.692 us    |              nfsd_buffered_filldir [nfsd]();
  2)   0.691 us    |              nfsd_buffered_filldir [nfsd]();
  2)   0.692 us    |              nfsd_buffered_filldir [nfsd]();
  2)   0.681 us    |              nfsd_buffered_filldir [nfsd]();
  2)   0.692 us    |              nfsd_buffered_filldir [nfsd]();
  2)   0.691 us    |              nfsd_buffered_filldir [nfsd]();
  2)   0.682 us    |              nfsd_buffered_filldir [nfsd]();
  2)   0.691 us    |              nfsd_buffered_filldir [nfsd]();
  2)   0.682 us    |              nfsd_buffered_filldir [nfsd]();
  2)               |              nfsd4_encode_dirent [nfsd]() {
  2)               |                nfsd4_encode_dirent_fattr [nfsd]() {
  2)               |                  nfsd_mountpoint [nfsd]() {
  2)   0.661 us    |                    nfsd4_is_junction [nfsd]();
  2)   2.043 us    |                  }
  2)               |                  nfsd4_encode_fattr [nfsd]() {
  2)               |                    fh_compose [nfsd]() {
  2)   1.032 us    |                      _fh_update.part.0.isra.0 [nfsd]();
  2)   2.635 us    |                    }
  2)   0.681 us    |                    nfsd4_encode_bitmap [nfsd]();
  2)   0.651 us    |                    fsid_source [nfsd]();
  2)               |                    nfsd4_encode_user [nfsd]() {
  2)   1.202 us    |                      encode_name_from_id [nfsd]();
  2)   2.525 us    |                    }
  2)               |                    nfsd4_encode_group [nfsd]() {
  2)   0.932 us    |                      encode_name_from_id [nfsd]();
  2)   2.234 us    |                    }
  2)   0.832 us    |                    fh_put [nfsd]();
  2) + 17.652 us   |                  }
  2) + 23.183 us   |                }
  2) + 24.866 us   |              }
  2)               |              nfsd4_encode_dirent [nfsd]() {
  2)               |                nfsd4_encode_dirent_fattr [nfsd]() {
  2)               |                  nfsd_mountpoint [nfsd]() {
  2)   0.761 us    |                    nfsd4_is_junction [nfsd]();
  2)   2.013 us    |                  }
  2)               |                  nfsd4_encode_fattr [nfsd]() {
  2)               |                    fh_compose [nfsd]() {
  2)   0.722 us    |                      _fh_update.part.0.isra.0 [nfsd]();
  2)   2.034 us    |                    }
  2)   0.671 us    |                    nfsd4_encode_bitmap [nfsd]();
  2)   0.662 us    |                    fsid_source [nfsd]();
  2)               |                    nfsd4_encode_user [nfsd]() {
  2)   0.961 us    |                      encode_name_from_id [nfsd]();
  2)   2.254 us    |                    }
  2)               |                    nfsd4_encode_group [nfsd]() {
  2)   0.932 us    |                      encode_name_from_id [nfsd]();
  2)   2.194 us    |                    }
  2)   0.791 us    |                    fh_put [nfsd]();
  2) + 14.197 us   |                  }
  2) + 18.654 us   |                }
  2) + 20.688 us   |              }
  2)               |              nfsd4_encode_dirent [nfsd]() {
  2)               |                nfsd4_encode_dirent_fattr [nfsd]() {
  2)               |                  nfsd_mountpoint [nfsd]() {
  2)   0.652 us    |                    nfsd4_is_junction [nfsd]();
  2)   1.904 us    |                  }
  2)               |                  nfsd4_encode_fattr [nfsd]() {
  2)               |                    fh_compose [nfsd]() {
  2)   0.721 us    |                      _fh_update.part.0.isra.0 [nfsd]();
  2)   2.024 us    |                    }
  2)   0.671 us    |                    nfsd4_encode_bitmap [nfsd]();
  2)   0.651 us    |                    fsid_source [nfsd]();
  2)               |                    nfsd4_encode_user [nfsd]() {
  2)   0.962 us    |                      encode_name_from_id [nfsd]();
  2)   2.235 us    |                    }
  2)               |                    nfsd4_encode_group [nfsd]() {
  2)   0.922 us    |                      encode_name_from_id [nfsd]();
  2)   2.194 us    |                    }
  2)   0.772 us    |                    fh_put [nfsd]();
  2) + 14.086 us   |                  }
  2) + 18.404 us   |                }
  2) + 20.027 us   |              }
  2)   0.692 us    |              nfsd4_encode_dirent [nfsd]();
  2)               |              nfsd4_encode_dirent [nfsd]() {
  2)               |                nfsd4_encode_dirent_fattr [nfsd]() {
  2)               |                  nfsd_mountpoint [nfsd]() {
  2)   0.651 us    |                    nfsd4_is_junction [nfsd]();
  2)   1.913 us    |                  }
  2)               |                  nfsd4_encode_fattr [nfsd]() {
  2)               |                    fh_compose [nfsd]() {
  2)   0.722 us    |                      _fh_update.part.0.isra.0 [nfsd]();
  2)   2.024 us    |                    }
  2)   0.661 us    |                    nfsd4_encode_bitmap [nfsd]();
  2)   0.662 us    |                    fsid_source [nfsd]();
  2)               |                    nfsd4_encode_user [nfsd]() {
  2)   0.951 us    |                      encode_name_from_id [nfsd]();
  2)   2.244 us    |                    }
  2)               |                    nfsd4_encode_group [nfsd]() {
  2)   0.931 us    |                      encode_name_from_id [nfsd]();
  2)   2.195 us    |                    }
  2)   0.771 us    |                    fh_put [nfsd]();
  2) + 14.267 us   |                  }
  2) + 18.564 us   |                }
  2) + 20.177 us   |              }
  2)               |              nfsd4_encode_dirent [nfsd]() {
  2)               |                nfsd4_encode_dirent_fattr [nfsd]() {
  2)               |                  nfsd_mountpoint [nfsd]() {
  2)   0.792 us    |                    nfsd4_is_junction [nfsd]();
  2)   2.034 us    |                  }
  2)               |                  nfsd4_encode_fattr [nfsd]() {
  2)               |                    fh_compose [nfsd]() {
  2)   0.721 us    |                      _fh_update.part.0.isra.0 [nfsd]();
  2)   2.034 us    |                    }
  2)   0.671 us    |                    nfsd4_encode_bitmap [nfsd]();
  2)   0.661 us    |                    fsid_source [nfsd]();
  2)               |                    nfsd4_encode_user [nfsd]() {
  2)   0.961 us    |                      encode_name_from_id [nfsd]();
  2)   2.245 us    |                    }
  2)               |                    nfsd4_encode_group [nfsd]() {
  2)   0.932 us    |                      encode_name_from_id [nfsd]();
  2)   2.194 us    |                    }
  2)   0.782 us    |                    fh_put [nfsd]();
  2) + 14.767 us   |                  }
  2) + 19.165 us   |                }
  2) + 20.738 us   |              }
  2)               |              nfsd4_encode_dirent [nfsd]() {
  2)               |                nfsd4_encode_dirent_fattr [nfsd]() {
  2)               |                  nfsd_mountpoint [nfsd]() {
  2)   0.651 us    |                    nfsd4_is_junction [nfsd]();
  2)   2.033 us    |                  }
  2)               |                  nfsd4_encode_fattr [nfsd]() {
  2)               |                    fh_compose [nfsd]() {
  2)   0.722 us    |                      _fh_update.part.0.isra.0 [nfsd]();
  2)   2.034 us    |                    }
  2)   0.661 us    |                    nfsd4_encode_bitmap [nfsd]();
  2)   0.652 us    |                    fsid_source [nfsd]();
  2)               |                    nfsd4_encode_user [nfsd]() {
  2)   0.952 us    |                      encode_name_from_id [nfsd]();
  2)   2.244 us    |                    }
  2)               |                    nfsd4_encode_group [nfsd]() {
  2)   0.921 us    |                      encode_name_from_id [nfsd]();
  2)   2.194 us    |                    }
  2)   0.781 us    |                    fh_put [nfsd]();
  2) + 14.147 us   |                  }
  2) + 18.615 us   |                }
  2) + 20.298 us   |              }
  2)   0.681 us    |              nfsd4_encode_dirent [nfsd]();
  2)               |              nfsd4_encode_dirent [nfsd]() {
  2)               |                nfsd4_encode_dirent_fattr [nfsd]() {
  2)               |                  nfsd_mountpoint [nfsd]() {
  2)   0.761 us    |                    nfsd4_is_junction [nfsd]();
  2)   2.013 us    |                  }
  2)               |                  nfsd4_encode_fattr [nfsd]() {
  2)               |                    fh_compose [nfsd]() {
  2)   0.711 us    |                      _fh_update.part.0.isra.0 [nfsd]();
  2)   2.044 us    |                    }
  2)   0.671 us    |                    nfsd4_encode_bitmap [nfsd]();
  2)   0.661 us    |                    fsid_source [nfsd]();
  2)               |                    nfsd4_encode_user [nfsd]() {
  2)   0.952 us    |                      encode_name_from_id [nfsd]();
  2)   2.234 us    |                    }
  2)               |                    nfsd4_encode_group [nfsd]() {
  2)   0.931 us    |                      encode_name_from_id [nfsd]();
  2)   2.194 us    |                    }
  2)   0.771 us    |                    fh_put [nfsd]();
  2) + 14.136 us   |                  }
  2) + 18.665 us   |                }
  2) + 20.258 us   |              }
  2)               |              nfsd4_encode_dirent [nfsd]() {
  2)               |                nfsd4_encode_dirent_fattr [nfsd]() {
  2)               |                  nfsd_mountpoint [nfsd]() {
  2)   0.652 us    |                    nfsd4_is_junction [nfsd]();
  2)   1.904 us    |                  }
  2)               |                  nfsd4_encode_fattr [nfsd]() {
  2)               |                    fh_compose [nfsd]() {
  2)   0.711 us    |                      _fh_update.part.0.isra.0 [nfsd]();
  2)   2.033 us    |                    }
  2)   0.672 us    |                    nfsd4_encode_bitmap [nfsd]();
  2)   0.661 us    |                    fsid_source [nfsd]();
  2)               |                    nfsd4_encode_user [nfsd]() {
  2)   0.962 us    |                      encode_name_from_id [nfsd]();
  2)   2.234 us    |                    }
  2)               |                    nfsd4_encode_group [nfsd]() {
  2)   0.932 us    |                      encode_name_from_id [nfsd]();
  2)   2.194 us    |                    }
  2)   0.771 us    |                    fh_put [nfsd]();
  2) + 14.276 us   |                  }
  2) + 18.774 us   |                }
  2) + 20.378 us   |              }
  2)               |              nfsd4_encode_dirent [nfsd]() {
  2)               |                nfsd4_encode_dirent_fattr [nfsd]() {
  2)               |                  nfsd_mountpoint [nfsd]() {
  2)   0.751 us    |                    nfsd4_is_junction [nfsd]();
  2)   1.983 us    |                  }
  2)               |                  nfsd4_encode_fattr [nfsd]() {
  2)               |                    fh_compose [nfsd]() {
  2)   0.722 us    |                      _fh_update.part.0.isra.0 [nfsd]();
  2)   2.034 us    |                    }
  2)   0.661 us    |                    nfsd4_encode_bitmap [nfsd]();
  2)   0.661 us    |                    fsid_source [nfsd]();
  2)               |                    nfsd4_encode_user [nfsd]() {
  2)   0.962 us    |                      encode_name_from_id [nfsd]();
  2)   2.244 us    |                    }
  2)               |                    nfsd4_encode_group [nfsd]() {
  2)   0.921 us    |                      encode_name_from_id [nfsd]();
  2)   2.194 us    |                    }
  2)   0.781 us    |                    fh_put [nfsd]();
  2) + 14.156 us   |                  }
  2) + 18.505 us   |                }
  2) + 20.107 us   |              }
  2)               |              nfsd4_encode_dirent [nfsd]() {
  2)               |                nfsd4_encode_dirent_fattr [nfsd]() {
  2)               |                  nfsd_mountpoint [nfsd]() {
  2)   0.641 us    |                    nfsd4_is_junction [nfsd]();
  2)   1.893 us    |                  }
  2)               |                  nfsd4_encode_fattr [nfsd]() {
  2)               |                    fh_compose [nfsd]() {
  2)   0.722 us    |                      _fh_update.part.0.isra.0 [nfsd]();
  2)   2.034 us    |                    }
  2)   0.681 us    |                    nfsd4_encode_bitmap [nfsd]();
  2)   0.662 us    |                    fsid_source [nfsd]();
  2)               |                    nfsd4_encode_user [nfsd]() {
  2)   0.962 us    |                      encode_name_from_id [nfsd]();
  2)   2.234 us    |                    }
  2)               |                    nfsd4_encode_group [nfsd]() {
  2)   0.921 us    |                      encode_name_from_id [nfsd]();
  2)   2.204 us    |                    }
  2)   0.781 us    |                    fh_put [nfsd]();
  2) + 14.147 us   |                  }
  2) + 18.965 us   |                }
  2) + 20.588 us   |              }
  2)               |              nfsd4_encode_dirent [nfsd]() {
  2)               |                nfsd4_encode_dirent_fattr [nfsd]() {
  2)               |                  nfsd_mountpoint [nfsd]() {
  2)   0.752 us    |                    nfsd4_is_junction [nfsd]();
  2)   1.994 us    |                  }
  2)               |                  nfsd4_encode_fattr [nfsd]() {
  2)               |                    fh_compose [nfsd]() {
  2)   0.721 us    |                      _fh_update.part.0.isra.0 [nfsd]();
  2)   2.034 us    |                    }
  2)   0.671 us    |                    nfsd4_encode_bitmap [nfsd]();
  2)   0.661 us    |                    fsid_source [nfsd]();
  2)               |                    nfsd4_encode_user [nfsd]() {
  2)   0.952 us    |                      encode_name_from_id [nfsd]();
  2)   2.235 us    |                    }
  2)               |                    nfsd4_encode_group [nfsd]() {
  2)   0.932 us    |                      encode_name_from_id [nfsd]();
  2)   2.194 us    |                    }
  2)   0.772 us    |                    fh_put [nfsd]();
  2) + 14.126 us   |                  }
  2) + 18.544 us   |                }
  2) + 20.117 us   |              }
  2) ! 292.260 us  |            }
  2) ! 316.976 us  |          }
  2) ! 318.828 us  |        }
  2) ! 320.231 us  |      }
  2)   0.701 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)   0.812 us    |      fh_put [nfsd]();
  2)   0.691 us    |      fh_put [nfsd]();
  2) ! 366.727 us  |    }
  2)               |    nfs4svc_encode_compoundres [nfsd]() {
  2)               |      nfsd4_sequence_done [nfsd]() {
  2)   0.711 us    |        copy_cred [nfsd]();
  2)               |        nfsd4_put_session_locked [nfsd]() {
  2)   0.882 us    |          put_client_renew_locked [nfsd]();
  2)   2.285 us    |        }
  2)   5.420 us    |      }
  2)   6.813 us    |    }
  2)   0.771 us    |    nfsd_cache_update [nfsd]();
  2) ! 404.618 us  |  }
  2)   0.661 us    |  nfsd4_release_compoundargs [nfsd]();
  2)   3.887 us    |  nfsd_init_request [nfsd]();
  2)               |  nfsd_dispatch [nfsd]() {
  2)               |    nfs4svc_decode_compoundargs [nfsd]() {
  2)               |      nfsd4_decode_compound.constprop.0 [nfsd]() {
  2)   0.651 us    |        OPDESC [nfsd]();
  2)               |        nfsd4_decode_sequence [nfsd]() {
  2)   0.681 us    |          nfsd4_decode_sessionid4 [nfsd]();
  2)   2.194 us    |        }
  2)   0.651 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.651 us    |          nfsd4_sequence_rsize [nfsd]();
  2)   2.123 us    |        }
  2)   0.641 us    |        OPDESC [nfsd]();
  2)               |        nfsd4_decode_putfh [nfsd]() {
  2)   0.652 us    |          svcxdr_savemem [nfsd]();
  2)   2.054 us    |        }
  2)   0.651 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.642 us    |          nfsd4_only_status_rsize [nfsd]();
  2)   2.004 us    |        }
  2)   0.641 us    |        OPDESC [nfsd]();
  2)   0.641 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.672 us    |          nfsd4_getattr_rsize [nfsd]();
  2)   2.024 us    |        }
  2) + 23.203 us   |      }
  2) + 25.236 us   |    }
  2)   0.821 us    |    nfsd_cache_lookup [nfsd]();
  2)               |    nfsd4_proc_compound [nfsd]() {
  2)   0.792 us    |      nfsd_minorversion [nfsd]();
  2)               |      nfsd4_sequence [nfsd]() {
  2)               |        find_in_sessionid_hashtbl [nfsd]() {
  2)   0.711 us    |          get_client_locked [nfsd]();
  2)   2.775 us    |        }
  2)   5.680 us    |      }
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)   0.872 us    |        nfsd4_encode_sequence [nfsd]();
  2)   2.414 us    |      }
  2)   0.662 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)               |      nfsd4_putfh [nfsd]() {
  2)   0.671 us    |        fh_put [nfsd]();
  2)               |        fh_verify [nfsd]() {
  2)               |          nfsd_set_fh_dentry [nfsd]() {
  2)               |            rqst_exp_find [nfsd]() {
  2)               |              exp_find [nfsd]() {
  2)   2.134 us    |                exp_find_key [nfsd]();
  2)               |                exp_get_by_name [nfsd]() {
  2)   0.772 us    |                  svc_export_match [nfsd]();
  2)   2.575 us    |                }
  2)   6.822 us    |              }
  2)   8.245 us    |            }
  2) + 11.801 us   |          }
  2)               |          nfsd_setuser_and_check_port [nfsd]() {
  2)   0.661 us    |            nfsexp_flags [nfsd]();
  2)   4.869 us    |            nfsd_setuser [nfsd]();
  2)   0.671 us    |            nfserrno [nfsd]();
  2)   8.896 us    |          }
  2)   0.661 us    |          nfsd_permission [nfsd]();
  2) + 24.175 us   |        }
  2) + 26.880 us   |      }
  2)   0.662 us    |      clear_current_stateid [nfsd]();
  2)   0.671 us    |      check_nfsd_access [nfsd]();
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)   0.651 us    |        nfsd4_encode_noop [nfsd]();
  2)   2.034 us    |      }
  2)   0.651 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)               |      nfsd4_getattr [nfsd]() {
  2)               |        fh_verify [nfsd]() {
  2)               |          nfsd_setuser_and_check_port [nfsd]() {
  2)   0.652 us    |            nfsexp_flags [nfsd]();
  2)   4.358 us    |            nfsd_setuser [nfsd]();
  2)   0.661 us    |            nfserrno [nfsd]();
  2)   8.095 us    |          }
  2)   0.651 us    |          check_nfsd_access [nfsd]();
  2)   0.661 us    |          nfsd_permission [nfsd]();
  2) + 11.862 us   |        }
  2) + 13.104 us   |      }
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)               |        nfsd4_encode_getattr [nfsd]() {
  2)               |          nfsd4_encode_fattr [nfsd]() {
  2)   0.691 us    |            nfsd4_encode_bitmap [nfsd]();
  2)   0.672 us    |            fsid_source [nfsd]();
  2)               |            nfsd4_encode_user [nfsd]() {
  2)   2.083 us    |              encode_name_from_id [nfsd]();
  2)   3.527 us    |            }
  2)               |            nfsd4_encode_group [nfsd]() {
  2)   0.952 us    |              encode_name_from_id [nfsd]();
  2)   2.284 us    |            }
  2) + 13.545 us   |          }
  2) + 14.927 us   |        }
  2) + 16.480 us   |      }
  2)   0.651 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)   0.881 us    |      fh_put [nfsd]();
  2)   0.661 us    |      fh_put [nfsd]();
  2) + 83.725 us   |    }
  2)               |    nfs4svc_encode_compoundres [nfsd]() {
  2)               |      nfsd4_sequence_done [nfsd]() {
  2)   0.812 us    |        copy_cred [nfsd]();
  2)               |        nfsd4_put_session_locked [nfsd]() {
  2)   0.832 us    |          put_client_renew_locked [nfsd]();
  2)   2.225 us    |        }
  2)   5.601 us    |      }
  2)   6.903 us    |    }
  2)   0.721 us    |    nfsd_cache_update [nfsd]();
  2) ! 122.215 us  |  }
  2)   0.661 us    |  nfsd4_release_compoundargs [nfsd]();
  2)   1.172 us    |  nfsd_init_request [nfsd]();
  2)               |  nfsd_dispatch [nfsd]() {
  2)               |    nfs4svc_decode_compoundargs [nfsd]() {
  2)               |      nfsd4_decode_compound.constprop.0 [nfsd]() {
  2)   0.651 us    |        OPDESC [nfsd]();
  2)               |        nfsd4_decode_sequence [nfsd]() {
  2)   0.682 us    |          nfsd4_decode_sessionid4 [nfsd]();
  2)   2.334 us    |        }
  2)   0.651 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.641 us    |          nfsd4_sequence_rsize [nfsd]();
  2)   1.924 us    |        }
  2)   0.641 us    |        OPDESC [nfsd]();
  2)               |        nfsd4_decode_putfh [nfsd]() {
  2)   0.641 us    |          svcxdr_savemem [nfsd]();
  2)   1.934 us    |        }
  2)   0.651 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.642 us    |          nfsd4_only_status_rsize [nfsd]();
  2)   1.904 us    |        }
  2)   0.641 us    |        OPDESC [nfsd]();
  2)   0.652 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.661 us    |          nfsd4_getattr_rsize [nfsd]();
  2)   1.933 us    |        }
  2) + 21.449 us   |      }
  2) + 22.691 us   |    }
  2)   0.711 us    |    nfsd_cache_lookup [nfsd]();
  2)               |    nfsd4_proc_compound [nfsd]() {
  2)   0.671 us    |      nfsd_minorversion [nfsd]();
  2)               |      nfsd4_sequence [nfsd]() {
  2)               |        find_in_sessionid_hashtbl [nfsd]() {
  2)   0.711 us    |          get_client_locked [nfsd]();
  2)   2.044 us    |        }
  2)   3.897 us    |      }
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)   0.741 us    |        nfsd4_encode_sequence [nfsd]();
  2)   2.024 us    |      }
  2)   0.651 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)               |      nfsd4_putfh [nfsd]() {
  2)   0.661 us    |        fh_put [nfsd]();
  2)               |        fh_verify [nfsd]() {
  2)               |          nfsd_set_fh_dentry [nfsd]() {
  2)               |            rqst_exp_find [nfsd]() {
  2)               |              exp_find [nfsd]() {
  2)   1.132 us    |                exp_find_key [nfsd]();
  2)               |                exp_get_by_name [nfsd]() {
  2)   0.651 us    |                  svc_export_match [nfsd]();
  2)   2.164 us    |                }
  2)   5.190 us    |              }
  2)   6.492 us    |            }
  2)   8.707 us    |          }
  2)               |          nfsd_setuser_and_check_port [nfsd]() {
  2)   0.641 us    |            nfsexp_flags [nfsd]();
  2)   4.098 us    |            nfsd_setuser [nfsd]();
  2)   0.661 us    |            nfserrno [nfsd]();
  2)   7.844 us    |          }
  2)   0.662 us    |          nfsd_permission [nfsd]();
  2) + 19.727 us   |        }
  2) + 22.261 us   |      }
  2)   0.651 us    |      clear_current_stateid [nfsd]();
  2)   0.651 us    |      check_nfsd_access [nfsd]();
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)   0.651 us    |        nfsd4_encode_noop [nfsd]();
  2)   1.954 us    |      }
  2)   0.651 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)               |      nfsd4_getattr [nfsd]() {
  2)               |        fh_verify [nfsd]() {
  2)               |          nfsd_setuser_and_check_port [nfsd]() {
  2)   0.651 us    |            nfsexp_flags [nfsd]();
  2)   4.088 us    |            nfsd_setuser [nfsd]();
  2)   0.651 us    |            nfserrno [nfsd]();
  2)   7.804 us    |          }
  2)   0.652 us    |          check_nfsd_access [nfsd]();
  2)   0.651 us    |          nfsd_permission [nfsd]();
  2) + 11.521 us   |        }
  2) + 12.773 us   |      }
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)               |        nfsd4_encode_getattr [nfsd]() {
  2)               |          nfsd4_encode_fattr [nfsd]() {
  2)   0.681 us    |            nfsd4_encode_bitmap [nfsd]();
  2)   0.641 us    |            fsid_source [nfsd]();
  2)               |            nfsd4_encode_user [nfsd]() {
  2)   1.032 us    |              encode_name_from_id [nfsd]();
  2)   2.375 us    |            }
  2)               |            nfsd4_encode_group [nfsd]() {
  2)   0.932 us    |              encode_name_from_id [nfsd]();
  2)   2.214 us    |            }
  2) + 10.108 us   |          }
  2) + 11.351 us   |        }
  2) + 12.644 us   |      }
  2)   0.651 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)   0.791 us    |      fh_put [nfsd]();
  2)   0.641 us    |      fh_put [nfsd]();
  2) + 71.131 us   |    }
  2)               |    nfs4svc_encode_compoundres [nfsd]() {
  2)               |      nfsd4_sequence_done [nfsd]() {
  2)   0.732 us    |        copy_cred [nfsd]();
  2)               |        nfsd4_put_session_locked [nfsd]() {
  2)   0.772 us    |          put_client_renew_locked [nfsd]();
  2)   2.044 us    |        }
  2)   4.869 us    |      }
  2)   6.141 us    |    }
  2)   0.711 us    |    nfsd_cache_update [nfsd]();
  2) ! 105.084 us  |  }
  2)   0.642 us    |  nfsd4_release_compoundargs [nfsd]();
  2)   4.749 us    |  nfsd_init_request [nfsd]();
  2)               |  nfsd_dispatch [nfsd]() {
  2)               |    nfs4svc_decode_compoundargs [nfsd]() {
  2)               |      nfsd4_decode_compound.constprop.0 [nfsd]() {
  2)   0.851 us    |        OPDESC [nfsd]();
  2)               |        nfsd4_decode_sequence [nfsd]() {
  2)   0.862 us    |          nfsd4_decode_sessionid4 [nfsd]();
  2)   2.585 us    |        }
  2)   0.932 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.832 us    |          nfsd4_sequence_rsize [nfsd]();
  2)   2.675 us    |        }
  2)   0.791 us    |        OPDESC [nfsd]();
  2)               |        nfsd4_decode_putfh [nfsd]() {
  2)   0.831 us    |          svcxdr_savemem [nfsd]();
  2)   2.565 us    |        }
  2)   0.892 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.821 us    |          nfsd4_only_status_rsize [nfsd]();
  2)   2.525 us    |        }
  2)   0.812 us    |        OPDESC [nfsd]();
  2)   1.142 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.852 us    |          nfsd4_getattr_rsize [nfsd]();
  2)   2.535 us    |        }
  2) + 29.074 us   |      }
  2) + 30.817 us   |    }
  2)   1.572 us    |    nfsd_cache_lookup [nfsd]();
  2)               |    nfsd4_proc_compound [nfsd]() {
  2)   0.982 us    |      nfsd_minorversion [nfsd]();
  2)               |      nfsd4_sequence [nfsd]() {
  2)               |        find_in_sessionid_hashtbl [nfsd]() {
  2)   1.213 us    |          get_client_locked [nfsd]();
  2)   3.396 us    |        }
  2)   6.732 us    |      }
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)   0.992 us    |        nfsd4_encode_sequence [nfsd]();
  2)   2.915 us    |      }
  2)   0.831 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)               |      nfsd4_putfh [nfsd]() {
  2)   0.842 us    |        fh_put [nfsd]();
  2)               |        fh_verify [nfsd]() {
  2)               |          nfsd_set_fh_dentry [nfsd]() {
  2)               |            rqst_exp_find [nfsd]() {
  2)               |              exp_find [nfsd]() {
  2)   2.133 us    |                exp_find_key [nfsd]();
  2)               |                exp_get_by_name [nfsd]() {
  2)   1.032 us    |                  svc_export_match [nfsd]();
  2)   3.156 us    |                }
  2)   7.864 us    |              }
  2)   9.728 us    |            }
  2) + 14.366 us   |          }
  2)               |          nfsd_setuser_and_check_port [nfsd]() {
  2)   0.801 us    |            nfsexp_flags [nfsd]();
  2)   5.981 us    |            nfsd_setuser [nfsd]();
  2)   0.842 us    |            nfserrno [nfsd]();
  2) + 11.020 us   |          }
  2)   0.862 us    |          nfsd_permission [nfsd]();
  2) + 29.645 us   |        }
  2) + 33.111 us   |      }
  2)   0.832 us    |      clear_current_stateid [nfsd]();
  2)   0.822 us    |      check_nfsd_access [nfsd]();
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)   0.822 us    |        nfsd4_encode_noop [nfsd]();
  2)   2.555 us    |      }
  2)   0.832 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)               |      nfsd4_getattr [nfsd]() {
  2)               |        fh_verify [nfsd]() {
  2)               |          nfsd_setuser_and_check_port [nfsd]() {
  2)   1.533 us    |            nfsexp_flags [nfsd]();
  2)   5.279 us    |            nfsd_setuser [nfsd]();
  2)   0.861 us    |            nfserrno [nfsd]();
  2) + 10.750 us   |          }
  2)   0.802 us    |          check_nfsd_access [nfsd]();
  2)   0.811 us    |          nfsd_permission [nfsd]();
  2) + 15.449 us   |        }
  2) + 17.021 us   |      }
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)               |        nfsd4_encode_getattr [nfsd]() {
  2)               |          nfsd4_encode_fattr [nfsd]() {
  2)   0.891 us    |            nfsd4_encode_bitmap [nfsd]();
  2)   0.872 us    |            fsid_source [nfsd]();
  2)               |            nfsd4_encode_user [nfsd]() {
  2)   1.994 us    |              encode_name_from_id [nfsd]();
  2)   3.787 us    |            }
  2)               |            nfsd4_encode_group [nfsd]() {
  2)   1.233 us    |              encode_name_from_id [nfsd]();
  2)   2.855 us    |            }
  2) + 15.539 us   |          }
  2) + 17.241 us   |        }
  2) + 19.005 us   |      }
  2)   0.832 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)   1.103 us    |      fh_put [nfsd]();
  2)   0.862 us    |      fh_put [nfsd]();
  2) ! 102.289 us  |    }
  2)               |    nfs4svc_encode_compoundres [nfsd]() {
  2)               |      nfsd4_sequence_done [nfsd]() {
  2)   1.062 us    |        copy_cred [nfsd]();
  2)               |        nfsd4_put_session_locked [nfsd]() {
  2)   1.153 us    |          put_client_renew_locked [nfsd]();
  2)   2.885 us    |        }
  2)   7.113 us    |      }
  2)   8.797 us    |    }
  2)   0.892 us    |    nfsd_cache_update [nfsd]();
  2) ! 149.716 us  |  }
  2)   0.851 us    |  nfsd4_release_compoundargs [nfsd]();
  2)   5.119 us    |  nfsd_init_request [nfsd]();
  2)               |  nfsd_dispatch [nfsd]() {
  2)               |    nfs4svc_decode_compoundargs [nfsd]() {
  2)               |      nfsd4_decode_compound.constprop.0 [nfsd]() {
  2)   0.651 us    |        OPDESC [nfsd]();
  2)               |        nfsd4_decode_sequence [nfsd]() {
  2)   0.672 us    |          nfsd4_decode_sessionid4 [nfsd]();
  2)   2.044 us    |        }
  2)   0.651 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.641 us    |          nfsd4_sequence_rsize [nfsd]();
  2)   2.144 us    |        }
  2)   0.641 us    |        OPDESC [nfsd]();
  2)               |        nfsd4_decode_putfh [nfsd]() {
  2)   0.651 us    |          svcxdr_savemem [nfsd]();
  2)   2.205 us    |        }
  2)   0.641 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.642 us    |          nfsd4_only_status_rsize [nfsd]();
  2)   2.034 us    |        }
  2)   0.641 us    |        OPDESC [nfsd]();
  2)               |        nfsd4_decode_remove [nfsd]() {
  2)               |          nfsd4_decode_component4 [nfsd]() {
  2)   0.641 us    |            svcxdr_savemem [nfsd]();
  2)   1.994 us    |          }
  2)   3.327 us    |        }
  2)   0.641 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.651 us    |          nfsd4_remove_rsize [nfsd]();
  2)   1.974 us    |        }
  2) + 27.000 us   |      }
  2) + 28.383 us   |    }
  2)   0.822 us    |    nfsd_cache_lookup [nfsd]();
  2)               |    nfsd4_proc_compound [nfsd]() {
  2)   0.781 us    |      nfsd_minorversion [nfsd]();
  2)               |      nfsd4_sequence [nfsd]() {
  2)               |        find_in_sessionid_hashtbl [nfsd]() {
  2)   0.702 us    |          get_client_locked [nfsd]();
  2)   3.126 us    |        }
  2)   6.161 us    |      }
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)   0.992 us    |        nfsd4_encode_sequence [nfsd]();
  2)   2.655 us    |      }
  2)   0.661 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)               |      nfsd4_putfh [nfsd]() {
  2)   0.672 us    |        fh_put [nfsd]();
  2)               |        fh_verify [nfsd]() {
  2)               |          nfsd_set_fh_dentry [nfsd]() {
  2)               |            rqst_exp_find [nfsd]() {
  2)               |              exp_find [nfsd]() {
  2)   2.184 us    |                exp_find_key [nfsd]();
  2)               |                exp_get_by_name [nfsd]() {
  2)   0.851 us    |                  svc_export_match [nfsd]();
  2)   2.535 us    |                }
  2)   6.813 us    |              }
  2)   8.275 us    |            }
  2) + 13.666 us   |          }
  2)               |          nfsd_setuser_and_check_port [nfsd]() {
  2)   0.662 us    |            nfsexp_flags [nfsd]();
  2)   4.989 us    |            nfsd_setuser [nfsd]();
  2)   0.682 us    |            nfserrno [nfsd]();
  2)   9.037 us    |          }
  2)   0.852 us    |          nfsd_permission [nfsd]();
  2) + 26.439 us   |        }
  2) + 29.114 us   |      }
  2)   0.652 us    |      clear_current_stateid [nfsd]();
  2)   0.671 us    |      check_nfsd_access [nfsd]();
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)   0.652 us    |        nfsd4_encode_noop [nfsd]();
  2)   2.044 us    |      }
  2)   0.661 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)   0.652 us    |      nfsd4_remove_rsize [nfsd]();
  2)   0.671 us    |      nfsd4_check_resp_size [nfsd]();
  2)               |      nfsd4_remove [nfsd]() {
  2)               |        nfsd_unlink [nfsd]() {
  2)               |          fh_verify [nfsd]() {
  2)               |            nfsd_setuser_and_check_port [nfsd]() {
  2)   0.651 us    |              nfsexp_flags [nfsd]();
  2)   4.258 us    |              nfsd_setuser [nfsd]();
  2)   0.661 us    |              nfserrno [nfsd]();
  2)   7.994 us    |            }
  2)   0.651 us    |            check_nfsd_access [nfsd]();
  2)               |            nfsd_permission [nfsd]() {
  2)   0.651 us    |              nfsexp_flags [nfsd]();
  2)   2.424 us    |            }
  2) + 13.565 us   |          }
  2)               |          fh_fill_pre_attrs [nfsd]() {
  2)   0.661 us    |            nfserrno [nfsd]();
  2)   2.905 us    |          }
  2)               |          fh_fill_post_attrs [nfsd]() {
  2)   0.671 us    |            nfserrno [nfsd]();
  2)   2.555 us    |          }
  2) # 9735.458 us |          commit_metadata [nfsd]();
  2)   1.914 us    |          nfserrno [nfsd]();
  2) # 9862.343 us |        }
  2) # 9864.948 us |      }
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)   0.922 us    |        nfsd4_encode_remove [nfsd]();
  2)   3.497 us    |      }
  2)   0.902 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)   1.413 us    |      fh_put [nfsd]();
  2)   0.922 us    |      fh_put [nfsd]();
  2) # 9931.780 us |    }
  2)               |    nfs4svc_encode_compoundres [nfsd]() {
  2)               |      nfsd4_sequence_done [nfsd]() {
  2)   1.212 us    |        copy_cred [nfsd]();
  2)               |        nfsd4_put_session_locked [nfsd]() {
  2)   1.173 us    |          put_client_renew_locked [nfsd]();
  2)   3.106 us    |        }
  2) + 10.099 us   |      }
  2) + 12.012 us   |    }
  2)   0.972 us    |    nfsd_cache_update [nfsd]();
  2) # 9980.691 us |  }
  2)   0.901 us    |  nfsd4_release_compoundargs [nfsd]();
  2)   3.807 us    |  nfsd_init_request [nfsd]();
  2)               |  nfsd_dispatch [nfsd]() {
  2)               |    nfs4svc_decode_compoundargs [nfsd]() {
  2)               |      nfsd4_decode_compound.constprop.0 [nfsd]() {
  2)   0.682 us    |        OPDESC [nfsd]();
  2)               |        nfsd4_decode_sequence [nfsd]() {
  2)   0.681 us    |          nfsd4_decode_sessionid4 [nfsd]();
  2)   2.064 us    |        }
  2)   0.651 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.651 us    |          nfsd4_sequence_rsize [nfsd]();
  2)   2.144 us    |        }
  2)   0.641 us    |        OPDESC [nfsd]();
  2)               |        nfsd4_decode_putfh [nfsd]() {
  2)   0.651 us    |          svcxdr_savemem [nfsd]();
  2)   2.054 us    |        }
  2)   0.641 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.651 us    |          nfsd4_only_status_rsize [nfsd]();
  2)   2.225 us    |        }
  2)   0.641 us    |        OPDESC [nfsd]();
  2)   0.641 us    |        nfsd4_cache_this_op [nfsd]();
  2)               |        nfsd4_max_reply [nfsd]() {
  2)   0.682 us    |          nfsd4_getattr_rsize [nfsd]();
  2)   2.054 us    |        }
  2) + 23.854 us   |      }
  2) + 25.227 us   |    }
  2)   1.052 us    |    nfsd_cache_lookup [nfsd]();
  2)               |    nfsd4_proc_compound [nfsd]() {
  2)   0.801 us    |      nfsd_minorversion [nfsd]();
  2)               |      nfsd4_sequence [nfsd]() {
  2)               |        find_in_sessionid_hashtbl [nfsd]() {
  2)   0.701 us    |          get_client_locked [nfsd]();
  2)   3.076 us    |        }
  2)   5.801 us    |      }
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)   0.882 us    |        nfsd4_encode_sequence [nfsd]();
  2)   2.424 us    |      }
  2)   0.672 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)               |      nfsd4_putfh [nfsd]() {
  2)   0.671 us    |        fh_put [nfsd]();
  2)               |        fh_verify [nfsd]() {
  2)               |          nfsd_set_fh_dentry [nfsd]() {
  2)               |            rqst_exp_find [nfsd]() {
  2)               |              exp_find [nfsd]() {
  2)   2.084 us    |                exp_find_key [nfsd]();
  2)               |                exp_get_by_name [nfsd]() {
  2)   0.771 us    |                  svc_export_match [nfsd]();
  2)   2.455 us    |                }
  2)   6.632 us    |              }
  2)   8.055 us    |            }
  2) + 11.351 us   |          }
  2)               |          nfsd_setuser_and_check_port [nfsd]() {
  2)   0.662 us    |            nfsexp_flags [nfsd]();
  2)   4.859 us    |            nfsd_setuser [nfsd]();
  2)   0.681 us    |            nfserrno [nfsd]();
  2)   8.946 us    |          }
  2)   0.781 us    |          nfsd_permission [nfsd]();
  2) + 23.904 us   |        }
  2) + 26.599 us   |      }
  2)   0.661 us    |      clear_current_stateid [nfsd]();
  2)   0.672 us    |      check_nfsd_access [nfsd]();
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)   0.651 us    |        nfsd4_encode_noop [nfsd]();
  2)   2.043 us    |      }
  2)   0.652 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)               |      nfsd4_getattr [nfsd]() {
  2)               |        fh_verify [nfsd]() {
  2)               |          nfsd_setuser_and_check_port [nfsd]() {
  2)   0.651 us    |            nfsexp_flags [nfsd]();
  2)   4.639 us    |            nfsd_setuser [nfsd]();
  2)   0.661 us    |            nfserrno [nfsd]();
  2)   8.385 us    |          }
  2)   0.652 us    |          check_nfsd_access [nfsd]();
  2)   0.651 us    |          nfsd_permission [nfsd]();
  2) + 12.163 us   |        }
  2) + 13.405 us   |      }
  2)               |      nfsd4_encode_operation [nfsd]() {
  2)               |        nfsd4_encode_getattr [nfsd]() {
  2)               |          nfsd4_encode_fattr [nfsd]() {
  2)   0.702 us    |            nfsd4_encode_bitmap [nfsd]();
  2)   0.651 us    |            fsid_source [nfsd]();
  2)               |            nfsd4_encode_user [nfsd]() {
  2)   2.124 us    |              encode_name_from_id [nfsd]();
  2)   3.556 us    |            }
  2)               |            nfsd4_encode_group [nfsd]() {
  2)   0.951 us    |              encode_name_from_id [nfsd]();
  2)   2.264 us    |            }
  2) + 12.964 us   |          }
  2) + 14.326 us   |        }
  2) + 15.789 us   |      }
  2)   0.662 us    |      nfsd4_cstate_clear_replay [nfsd]();
  2)   0.872 us    |      fh_put [nfsd]();
  2)   0.661 us    |      fh_put [nfsd]();
  2) + 82.993 us   |    }
  2)               |    nfs4svc_encode_compoundres [nfsd]() {
  2)               |      nfsd4_sequence_done [nfsd]() {
  2)   0.801 us    |        copy_cred [nfsd]();
  2)               |        nfsd4_put_session_locked [nfsd]() {
  2)   0.832 us    |          put_client_renew_locked [nfsd]();
  2)   2.204 us    |        }
  2)   5.580 us    |      }
  2)   7.294 us    |    }
  2)   0.721 us    |    nfsd_cache_update [nfsd]();
  2) ! 121.704 us  |  }
  2)   0.682 us    |  nfsd4_release_compoundargs [nfsd]();
```


