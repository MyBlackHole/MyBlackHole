# xfs

## function_graph 案例

- xfs_log_mount
```shell
sysctl kernel.ftrace_enabled=1
dir=/sys/kernel/debug/tracing

<!-- 执行栈 -->
echo function_graph > ${dir}/current_tracer

<!-- <!-- 过滤指定函数 --> -->
<!-- echo xfs_log* > set_ftrace_filter -->

<!-- 过滤指定模块 -->
echo '*:mod:xfs' > set_ftrace_filter

<!-- 重置 trace -->
echo 0 > trace

<!-- 启用 -->
echo 1 > ${dir}/tracing_on
<!-- 停用 -->
echo 0 > ${dir}/tracing_on

root@black:/sys/kernel/debug/tracing# cat trace
# tracer: function_graph
#
# CPU  DURATION                  FUNCTION CALLS
# |     |   |                     |   |   |   |
 12)               |  xfs_log_mount [xfs]() {
 12)               |    xfs_log_calc_minimum_size [xfs]() {
 12)               |      xfs_log_get_max_trans_res [xfs]() {
 12)   0.301 us    |        xfs_log_calc_max_attrsetm_res [xfs]();
 12)   4.298 us    |      }
 12)   0.441 us    |      xfs_log_calc_unit_res [xfs]();
 12)   6.231 us    |    }
 12)   3.477 us    |    xfs_log_check_lsn [xfs]();
 12) # 7271.568 us |  }
  6)   3.817 us    |  xfs_log_check_lsn [xfs]();
  6)   0.831 us    |  xfs_log_check_lsn [xfs]();
  6)   0.892 us    |  xfs_log_check_lsn [xfs]();
  6)   0.851 us    |  xfs_log_check_lsn [xfs]();
  6)   0.762 us    |  xfs_log_check_lsn [xfs]();
  6)   0.811 us    |  xfs_log_check_lsn [xfs]();
  6)   0.772 us    |  xfs_log_check_lsn [xfs]();
  6)   0.891 us    |  xfs_log_check_lsn [xfs]();
  6)   0.772 us    |  xfs_log_check_lsn [xfs]();
 12) + 17.051 us   |  xfs_log_mount_finish [xfs]();
  6)   0.791 us    |  xfs_log_check_lsn [xfs]();
  6)   0.802 us    |  xfs_log_check_lsn [xfs]();
  6)   0.752 us    |  xfs_log_check_lsn [xfs]();
  6)   0.801 us    |  xfs_log_check_lsn [xfs]();
  6)   0.761 us    |  xfs_log_check_lsn [xfs]();
  6)   0.762 us    |  xfs_log_check_lsn [xfs]();
  6)   0.752 us    |  xfs_log_check_lsn [xfs]();
  6)   0.751 us    |  xfs_log_check_lsn [xfs]();
 ------------------------------------------
 12) pool-ud-338369 => kworker-327415
 ------------------------------------------

 12)               |  xfs_log_worker [xfs]() {
 12)   1.553 us    |    xfs_log_need_covered.isra.0 [xfs]();
 12)   4.058 us    |    xfs_log_force [xfs]();
 12) + 14.136 us   |  }





# tracer: function_graph
#
# CPU  DURATION                  FUNCTION CALLS
# |     |   |                     |   |   |   |
 14)               |  xfs_init_fs_context [xfs]() {
 14)   1.112 us    |    kmem_alloc [xfs]();
 14)   5.350 us    |  }
 14)   1.172 us    |  xfs_fs_parse_param [xfs]();
 14)               |  xfs_fs_get_tree [xfs]() {
 14)               |    xfs_fs_fill_super [xfs]() {
 14)   0.420 us    |      xfs_fs_validate_params [xfs]();
 14)               |      xfs_open_devices [xfs]() {
 14)               |        xfs_alloc_buftarg [xfs]() {
 14)   0.391 us    |          kmem_alloc [xfs]();
 14)   0.431 us    |          xfs_setsize_buftarg_early.isra.0 [xfs]();
 14)   3.517 us    |        }
 14)   4.398 us    |      }
 14) ! 290.877 us  |      xfs_init_mount_workqueues [xfs]();
 14)               |      xfs_readsb [xfs]() {
 14)               |        xfs_buf_read_uncached [xfs]() {
 14)               |          xfs_buf_get_uncached [xfs]() {
 14)   1.443 us    |            _xfs_buf_alloc [xfs]();
 14)   1.953 us    |            xfs_buf_alloc_pages [xfs]();
 14)   0.310 us    |            _xfs_buf_map_pages [xfs]();
 14)   4.999 us    |          }
 14)               |          __xfs_buf_submit [xfs]() {
 14)   0.291 us    |            xfs_buf_hold [xfs]();
 14)               |            _xfs_buf_ioapply [xfs]() {
 14)   9.558 us    |              xfs_buf_ioapply_map [xfs]();
 14) + 10.670 us   |            }
 14) ! 506.355 us  |            xfs_buf_iowait [xfs]();
 12)   8.125 us    |  xfs_buf_bio_end_io [xfs]();
 ------------------------------------------
 12)  zvol-424012   => kworker-432959
 ------------------------------------------

 12)               |  xfs_buf_ioend_work [xfs]() {
 12)   1.743 us    |    xfs_buf_ioend [xfs]();
 12)   3.426 us    |  }
 14)   0.341 us    |            xfs_buf_rele [xfs]();
 14) ! 519.238 us  |          }
 14) ! 525.300 us  |        }
 14)               |        xfs_sb_from_disk [xfs]() {
 14)               |          __xfs_sb_from_disk [xfs]() {
 14)   0.311 us    |            xfs_sb_quota_from_disk [xfs]();
 14)   1.202 us    |          }
 14)   1.843 us    |        }
 14)   0.351 us    |        xfs_buf_unlock [xfs]();
 14)               |        xfs_buf_rele [xfs]() {
 14)               |          xfs_buf_free [xfs]() {
 14)   1.383 us    |            xfs_buf_free_pages [xfs]();
 14)   2.344 us    |          }
 14)   2.956 us    |        }
 14)               |        xfs_buf_read_uncached [xfs]() {
 14)               |          xfs_buf_get_uncached [xfs]() {
 14)   0.692 us    |            _xfs_buf_alloc [xfs]();
 14)   0.591 us    |            xfs_buf_alloc_pages [xfs]();
 14)   0.290 us    |            _xfs_buf_map_pages [xfs]();
 14)   2.635 us    |          }
 14)               |          __xfs_buf_submit [xfs]() {
 14)   0.290 us    |            xfs_buf_hold [xfs]();
 14)               |            _xfs_buf_ioapply [xfs]() {
 14)   3.286 us    |              xfs_buf_ioapply_map [xfs]();
 14)   3.897 us    |            }
 14) + 51.105 us   |            xfs_buf_iowait [xfs]();
  8)   3.827 us    |  xfs_buf_bio_end_io [xfs]();
 ------------------------------------------
  8)  zvol-424015   => kworker-349921
 ------------------------------------------

  8)               |  xfs_buf_ioend_work [xfs]() {
  8)               |    xfs_buf_ioend [xfs]() {
  8)               |      xfs_sb_read_verify [xfs]() {
  8)   0.551 us    |        __xfs_sb_from_disk [xfs]();
  8)               |        xfs_validate_sb_common [xfs]() {
  8)   0.391 us    |          xfs_verify_magic [xfs]();
  8)   0.301 us    |          xfs_sb_good_version [xfs]();
  8)   0.311 us    |          xfs_validate_stripe_geometry [xfs]();
  8)   2.735 us    |        }
  8)   0.290 us    |        xfs_validate_sb_read [xfs]();
  8)   6.061 us    |      }
  8)   7.584 us    |    }
  8)   8.766 us    |  }
 14)   0.341 us    |            xfs_buf_rele [xfs]();
 14) + 56.975 us   |          }
 14) + 60.422 us   |        }
 14)               |        xfs_sb_from_disk [xfs]() {
 14)               |          __xfs_sb_from_disk [xfs]() {
 14)   0.291 us    |            xfs_sb_quota_from_disk [xfs]();
 14)   0.881 us    |          }
 14)   1.423 us    |        }
 14)   0.301 us    |        xfs_sb_version_to_features [xfs]();
 14)   1.192 us    |        xfs_reinit_percpu_counters [xfs]();
 14)   0.331 us    |        xfs_buf_unlock [xfs]();
 14) ! 597.683 us  |      }
 14)   0.300 us    |      xfs_finish_flags [xfs]();
 14)               |      xfs_setup_devices [xfs]() {
 14)   0.491 us    |        xfs_setsize_buftarg [xfs]();
 14)   1.222 us    |      }
 14)   0.291 us    |      xfs_sb_validate_fsb_count [xfs]();
 14)   0.281 us    |      xfs_sb_validate_fsb_count [xfs]();
 14)   0.291 us    |      xfs_verify_fileoff [xfs]();
 14)               |      xfs_filestream_mount [xfs]() {
 14)               |        xfs_mru_cache_create [xfs]() {
 14)   0.381 us    |          kmem_alloc [xfs]();
 14)   0.401 us    |          kmem_alloc [xfs]();
 14)   1.663 us    |        }
 14)   2.374 us    |      }
 14)               |      xfs_mountfs [xfs]() {
 14)               |        xfs_sb_mount_common [xfs]() {
 14)   0.290 us    |          xfs_allocbt_maxrecs [xfs]();
 14)   0.300 us    |          xfs_allocbt_maxrecs [xfs]();
 14)   0.291 us    |          xfs_bmbt_maxrecs [xfs]();
 14)   0.291 us    |          xfs_bmbt_maxrecs [xfs]();
 14)   0.301 us    |          xfs_rmapbt_maxrecs [xfs]();
 14)   0.301 us    |          xfs_rmapbt_maxrecs [xfs]();
 14)   0.290 us    |          xfs_refcountbt_maxrecs [xfs]();
 14)   0.300 us    |          xfs_refcountbt_maxrecs [xfs]();
 14)   0.291 us    |          xfs_alloc_set_aside [xfs]();
 14)   0.301 us    |          xfs_alloc_ag_max_usable [xfs]();
 14)   7.363 us    |        }
 14)   0.300 us    |        xfs_validate_new_dalign [xfs]();
 14)               |        xfs_alloc_compute_maxlevels [xfs]() {
 14)   0.290 us    |          xfs_btree_compute_maxlevels [xfs]();
 14)   1.022 us    |        }
 14)               |        xfs_bmap_compute_maxlevels [xfs]() {
 14)   0.290 us    |          xfs_bmdr_maxrecs [xfs]();
 14)   0.902 us    |        }
 14)               |        xfs_bmap_compute_maxlevels [xfs]() {
 14)   0.281 us    |          xfs_bmdr_maxrecs [xfs]();
 14)   0.842 us    |        }
 14)   0.290 us    |        xfs_bmap_compute_attr_offset [xfs]();
 14)               |        xfs_ialloc_setup_geometry [xfs]() {
 14)   0.300 us    |          xfs_inobt_maxrecs [xfs]();
 14)   0.280 us    |          xfs_inobt_maxrecs [xfs]();
 14)   0.290 us    |          xfs_btree_compute_maxlevels [xfs]();
 14)   2.174 us    |        }
 14)   0.301 us    |        xfs_rmapbt_compute_maxlevels [xfs]();
 14)               |        xfs_refcountbt_compute_maxlevels [xfs]() {
 14)   0.301 us    |          xfs_btree_compute_maxlevels [xfs]();
 14)   0.852 us    |        }
 14)   0.301 us    |        xfs_update_alignment [xfs]();
 14) + 11.090 us   |        xfs_error_sysfs_init [xfs]();
 14)   0.361 us    |        xfs_uuid_mount [xfs]();
 14)               |        xfs_check_sizes [xfs]() {
 14)               |          xfs_buf_read_uncached [xfs]() {
 14)               |            xfs_buf_get_uncached [xfs]() {
 14)   0.511 us    |              _xfs_buf_alloc [xfs]();
 14)   1.192 us    |              xfs_buf_alloc_pages [xfs]();
 14)   0.291 us    |              _xfs_buf_map_pages [xfs]();
 14)   3.075 us    |            }
 14)               |            __xfs_buf_submit [xfs]() {
 14)   0.291 us    |              xfs_buf_hold [xfs]();
 14)               |              _xfs_buf_ioapply [xfs]() {
 14)   3.957 us    |                xfs_buf_ioapply_map [xfs]();
 14)   4.559 us    |              }
 14) ! 184.942 us  |              xfs_buf_iowait [xfs]();
 10)   6.622 us    |  xfs_buf_bio_end_io [xfs]();
 ------------------------------------------
 10)  zvol-424013   =>  <...>-359062 
 ------------------------------------------

 10)               |  xfs_buf_ioend_work [xfs]() {
 10)   2.214 us    |    xfs_buf_ioend [xfs]();
 10)   4.027 us    |  }
 14)   0.311 us    |              xfs_buf_rele [xfs]();
 14) ! 191.473 us  |            }
 14) ! 195.381 us  |          }
 14)   0.331 us    |          xfs_buf_unlock [xfs]();
 14)               |          xfs_buf_rele [xfs]() {
 14)               |            xfs_buf_free [xfs]() {
 14)   0.461 us    |              xfs_buf_free_pages [xfs]();
 14)   1.052 us    |            }
 14)   1.633 us    |          }
 14) ! 198.436 us  |        }
 14)   0.300 us    |        xfs_rtmount_init [xfs]();
 14)               |        xfs_da_mount [xfs]() {
 14)   0.451 us    |          kmem_alloc [xfs]();
 14)   0.361 us    |          kmem_alloc [xfs]();
 14)   1.633 us    |        }
 14)               |        xfs_trans_init [xfs]() {
 14)               |          xfs_trans_resv_calc [xfs]() {
 14)               |            xfs_calc_write_reservation [xfs]() {
 14)   0.280 us    |              xfs_calc_inode_res.isra.0 [xfs]();
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.932 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.822 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.822 us    |              }
 14)               |              xfs_calc_refcountbt_reservation [xfs]() {
 14)               |                xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                  xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                }
 14)               |                xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                  xfs_buf_log_overhead [xfs]();
 14)   1.332 us    |                }
 14)   3.046 us    |              }
 14)   9.958 us    |            }
 14)               |            xfs_calc_itruncate_reservation [xfs]() {
 14)   0.280 us    |              xfs_calc_inode_res.isra.0 [xfs]();
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |              }
 14)               |              xfs_calc_refcountbt_reservation [xfs]() {
 14)               |                xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                  xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                }
 14)               |                xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                  xfs_buf_log_overhead [xfs]();
 14)   0.842 us    |                }
 14)   2.464 us    |              }
 14)   6.853 us    |            }
 14)               |            xfs_calc_rename_reservation [xfs]() {
 14)   0.291 us    |              xfs_calc_inode_res.isra.0 [xfs]();
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)   4.118 us    |            }
 14)               |            xfs_calc_link_reservation [xfs]() {
 14)               |              xfs_calc_iunlink_remove_reservation.isra.0 [xfs]() {
 14)               |                xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                  xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                }
 14)   1.373 us    |              }
 14)   0.291 us    |              xfs_calc_inode_res.isra.0 [xfs]();
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)   5.770 us    |            }
 14)               |            xfs_calc_remove_reservation [xfs]() {
 14)               |              xfs_calc_iunlink_add_reservation.isra.0 [xfs]() {
 14)               |                xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                  xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                }
 14)   1.383 us    |              }
 14)   0.291 us    |              xfs_calc_inode_res.isra.0 [xfs]();
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)   5.760 us    |            }
 14)               |            xfs_calc_symlink_reservation [xfs]() {
 14)               |              xfs_calc_icreate_reservation [xfs]() {
 14)               |                xfs_calc_icreate_resv_alloc [xfs]() {
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)               |                  xfs_calc_inode_chunk_res [xfs]() {
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.821 us    |                    }
 14)   1.393 us    |                  }
 14)               |                  xfs_calc_inobt_res [xfs]() {
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                    }
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                    }
 14)   2.485 us    |                  }
 14)               |                  xfs_calc_finobt_res [xfs]() {
 14)               |                    xfs_calc_inobt_res [xfs]() {
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                      }
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.851 us    |                      }
 14)   2.805 us    |                    }
 14)   3.356 us    |                  }
 14)   9.407 us    |                }
 14)               |                xfs_calc_create_resv_modify [xfs]() {
 14)   0.280 us    |                  xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)               |                  xfs_calc_finobt_res [xfs]() {
 14)               |                    xfs_calc_inobt_res [xfs]() {
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                      }
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                      }
 14)   2.474 us    |                    }
 14)   3.016 us    |                  }
 14)   6.292 us    |                }
 14) + 16.621 us   |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14) + 18.424 us   |            }
 14)               |            xfs_calc_icreate_reservation [xfs]() {
 14)               |              xfs_calc_icreate_resv_alloc [xfs]() {
 14)               |                xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                  xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                }
 14)               |                xfs_calc_inode_chunk_res [xfs]() {
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)   1.382 us    |                }
 14)               |                xfs_calc_inobt_res [xfs]() {
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)   2.464 us    |                }
 14)               |                xfs_calc_finobt_res [xfs]() {
 14)               |                  xfs_calc_inobt_res [xfs]() {
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                    }
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                    }
 14)   2.465 us    |                  }
 14)   3.015 us    |                }
 14)   9.007 us    |              }
 14)               |              xfs_calc_create_resv_modify [xfs]() {
 14)   0.290 us    |                xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                  xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                }
 14)               |                xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                  xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                }
 14)               |                xfs_calc_finobt_res [xfs]() {
 14)               |                  xfs_calc_inobt_res [xfs]() {
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                    }
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                    }
 14)   2.465 us    |                  }
 14)   3.015 us    |                }
 14)   6.292 us    |              }
 14) + 16.089 us   |            }
 14)               |            xfs_calc_create_tmpfile_reservation [xfs]() {
 14)               |              xfs_calc_icreate_resv_alloc [xfs]() {
 14)               |                xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                  xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                }
 14)               |                xfs_calc_inode_chunk_res [xfs]() {
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)   1.372 us    |                }
 14)               |                xfs_calc_inobt_res [xfs]() {
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.842 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)   2.464 us    |                }
 14)               |                xfs_calc_finobt_res [xfs]() {
 14)               |                  xfs_calc_inobt_res [xfs]() {
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                    }
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                    }
 14)   2.464 us    |                  }
 14)   3.016 us    |                }
 14)   9.247 us    |              }
 14)               |              xfs_calc_iunlink_add_reservation.isra.0 [xfs]() {
 14)               |                xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                  xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                }
 14)   1.382 us    |              }
 14) + 11.441 us   |            }
 14)               |            xfs_calc_mkdir_reservation [xfs]() {
 14)               |              xfs_calc_icreate_reservation [xfs]() {
 14)               |                xfs_calc_icreate_resv_alloc [xfs]() {
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)               |                  xfs_calc_inode_chunk_res [xfs]() {
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                    }
 14)   1.372 us    |                  }
 14)               |                  xfs_calc_inobt_res [xfs]() {
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                    }
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                    }
 14)   2.464 us    |                  }
 14)               |                  xfs_calc_finobt_res [xfs]() {
 14)               |                    xfs_calc_inobt_res [xfs]() {
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                      }
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                      }
 14)   2.465 us    |                    }
 14)   3.006 us    |                  }
 14)   8.997 us    |                }
 14)               |                xfs_calc_create_resv_modify [xfs]() {
 14)   0.291 us    |                  xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)               |                  xfs_calc_finobt_res [xfs]() {
 14)               |                    xfs_calc_inobt_res [xfs]() {
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                      }
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                      }
 14)   2.464 us    |                    }
 14)   3.006 us    |                  }
 14)   6.272 us    |                }
 14) + 16.080 us   |              }
 14) + 16.631 us   |            }
 14)               |            xfs_calc_ifree_reservation [xfs]() {
 14)   0.291 us    |              xfs_calc_inode_res.isra.0 [xfs]();
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.822 us    |              }
 14)               |              xfs_calc_iunlink_remove_reservation.isra.0 [xfs]() {
 14)               |                xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                  xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                }
 14)   1.373 us    |              }
 14)               |              xfs_calc_inode_chunk_res [xfs]() {
 14)               |                xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                  xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                }
 14)               |                xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                  xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                }
 14)   2.465 us    |              }
 14)               |              xfs_calc_inobt_res [xfs]() {
 14)               |                xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                  xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                }
 14)               |                xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                  xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                }
 14)   2.465 us    |              }
 14)               |              xfs_calc_finobt_res [xfs]() {
 14)               |                xfs_calc_inobt_res [xfs]() {
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   1.152 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)   2.775 us    |                }
 14)   3.326 us    |              }
 14) + 12.613 us   |            }
 14)               |            xfs_calc_addafork_reservation [xfs]() {
 14)   0.290 us    |              xfs_calc_inode_res.isra.0 [xfs]();
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |              }
 14)   5.210 us    |            }
 14)               |            xfs_calc_attrinval_reservation [xfs]() {
 14)   0.290 us    |              xfs_calc_inode_res.isra.0 [xfs]();
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)   4.118 us    |            }
 14)               |            xfs_calc_attrsetm_reservation.isra.0 [xfs]() {
 14)   0.291 us    |              xfs_calc_inode_res.isra.0 [xfs]();
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)   3.026 us    |            }
 14)               |            xfs_calc_attrrm_reservation [xfs]() {
 14)   0.291 us    |              xfs_calc_inode_res.isra.0 [xfs]();
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.842 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)   5.209 us    |            }
 14)               |            xfs_calc_growrtalloc_reservation [xfs]() {
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)   0.291 us    |              xfs_calc_inode_res.isra.0 [xfs]();
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)   4.117 us    |            }
 14)               |            xfs_calc_qm_dqalloc_reservation [xfs]() {
 14)               |              xfs_calc_write_reservation [xfs]() {
 14)   0.280 us    |                xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                  xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                }
 14)               |                xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                  xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                }
 14)               |                xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                  xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                }
 14)               |                xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                  xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                }
 14)               |                xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                  xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                }
 14)               |                xfs_calc_refcountbt_reservation [xfs]() {
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   1.092 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)   2.725 us    |                }
 14)   9.267 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |              }
 14) + 10.910 us   |            }
 14)               |            xfs_calc_qm_setqlim_reservation [xfs]() {
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |              }
 14)   1.383 us    |            }
 14)               |            xfs_calc_clear_agi_bucket_reservation.isra.0 [xfs]() {
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |              }
 14)   1.383 us    |            }
 14)               |            xfs_calc_growdata_reservation [xfs]() {
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)   2.485 us    |            }
 14)               |            xfs_calc_ichange_reservation.isra.0 [xfs]() {
 14)   0.291 us    |              xfs_calc_inode_res.isra.0 [xfs]();
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)   1.924 us    |            }
 14)               |            xfs_calc_swrite_reservation.isra.0 [xfs]() {
 14)   0.281 us    |              xfs_calc_inode_res.isra.0 [xfs]();
 14)   0.831 us    |            }
 14)               |            xfs_calc_swrite_reservation.isra.0 [xfs]() {
 14)   0.281 us    |              xfs_calc_inode_res.isra.0 [xfs]();
 14)   0.831 us    |            }
 14)               |            xfs_calc_attrsetrt_reservation [xfs]() {
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)   2.474 us    |            }
 14)               |            xfs_calc_clear_agi_bucket_reservation.isra.0 [xfs]() {
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.822 us    |              }
 14)   1.372 us    |            }
 14)               |            xfs_calc_growrtzero_reservation.isra.0 [xfs]() {
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)   1.382 us    |            }
 14)               |            xfs_calc_growrtfree_reservation [xfs]() {
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)   0.291 us    |              xfs_calc_inode_res.isra.0 [xfs]();
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)               |              xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |              }
 14)   4.107 us    |            }
 14) ! 166.007 us  |          }
 14) ! 166.778 us  |        }
 14)               |        xfs_initialize_perag [xfs]() {
 14)   0.361 us    |          xfs_perag_get [xfs]();
 14)   0.520 us    |          kmem_alloc [xfs]();
 14)   1.233 us    |          xfs_buf_hash_init [xfs]();
 14)   0.301 us    |          __xfs_agino_range [xfs]();
 14)   0.341 us    |          xfs_perag_get [xfs]();
 14)   0.470 us    |          kmem_alloc [xfs]();
 14)   0.641 us    |          xfs_buf_hash_init [xfs]();
 14)   0.291 us    |          __xfs_agino_range [xfs]();
 14)   0.341 us    |          xfs_perag_get [xfs]();
 14)   0.972 us    |          kmem_alloc [xfs]();
 14)   0.571 us    |          xfs_buf_hash_init [xfs]();
 14)   0.301 us    |          __xfs_agino_range [xfs]();
 14)   0.331 us    |          xfs_perag_get [xfs]();
 14)   1.183 us    |          kmem_alloc [xfs]();
 14)   0.631 us    |          xfs_buf_hash_init [xfs]();
 14)   0.290 us    |          __xfs_agino_range [xfs]();
 14)               |          xfs_set_inode_alloc [xfs]() {
 14)   0.330 us    |            xfs_perag_get [xfs]();
 14)   0.300 us    |            xfs_perag_put [xfs]();
 14)   0.330 us    |            xfs_perag_get [xfs]();
 14)   0.290 us    |            xfs_perag_put [xfs]();
 14)   0.321 us    |            xfs_perag_get [xfs]();
 14)   0.291 us    |            xfs_perag_put [xfs]();
 14)   0.331 us    |            xfs_perag_get [xfs]();
 14)   0.291 us    |            xfs_perag_put [xfs]();
 14)   4.999 us    |          }
 14)               |          xfs_prealloc_blocks [xfs]() {
 14)   0.300 us    |            xfs_refc_block [xfs]();
 14)   0.942 us    |          }
 14) + 21.179 us   |        }
 14)   0.461 us    |        xfs_inodegc_register_shrinker [xfs]();
 14)               |        xfs_log_mount [xfs]() {
 14)   6.813 us    |          xfs_printk_level [xfs]();
 14)               |          xlog_alloc_log [xfs]() {
 14)   0.561 us    |            kmem_alloc [xfs]();
 14)   0.301 us    |            xlog_grant_head_init [xfs]();
 14)   0.290 us    |            xlog_grant_head_init [xfs]();
 14)   0.290 us    |            xlog_get_iclog_buffer_size [xfs]();
 14)   0.440 us    |            kmem_alloc [xfs]();
 14)   0.501 us    |            kmem_alloc [xfs]();
 14)   0.491 us    |            kmem_alloc [xfs]();
 14)   0.490 us    |            kmem_alloc [xfs]();
 14)   0.451 us    |            kmem_alloc [xfs]();
 14)   0.480 us    |            kmem_alloc [xfs]();
 14)   0.822 us    |            kmem_alloc [xfs]();
 14)   0.622 us    |            kmem_alloc [xfs]();
 14)               |            xlog_cil_init [xfs]() {
 14)   0.411 us    |              kmem_alloc [xfs]();
 14)   0.601 us    |              kmem_alloc [xfs]();
 14)   0.291 us    |              xlog_cil_ctx_switch [xfs]();
 14) + 44.262 us   |            }
 14) ! 125.192 us  |          }
 14)               |          xfs_log_calc_minimum_size [xfs]() {
 14)               |            xfs_log_get_max_trans_res [xfs]() {
 14)   0.290 us    |              xfs_log_calc_max_attrsetm_res [xfs]();
 14)               |              xfs_trans_resv_calc [xfs]() {
 14)               |                xfs_calc_write_reservation [xfs]() {
 14)   0.280 us    |                  xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.851 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.842 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)               |                  xfs_calc_refcountbt_reservation [xfs]() {
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                    }
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                    }
 14)   2.465 us    |                  }
 14)   9.047 us    |                }
 14)               |                xfs_calc_itruncate_reservation [xfs]() {
 14)   0.291 us    |                  xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)               |                  xfs_calc_refcountbt_reservation [xfs]() {
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                    }
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.822 us    |                    }
 14)   2.464 us    |                  }
 14)   7.053 us    |                }
 14)               |                xfs_calc_rename_reservation [xfs]() {
 14)   0.280 us    |                  xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)   4.108 us    |                }
 14)               |                xfs_calc_link_reservation [xfs]() {
 14)               |                  xfs_calc_iunlink_remove_reservation.isra.0 [xfs]() {
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                    }
 14)   1.372 us    |                  }
 14)   0.290 us    |                  xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.821 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)   5.741 us    |                }
 14)               |                xfs_calc_remove_reservation [xfs]() {
 14)               |                  xfs_calc_iunlink_add_reservation.isra.0 [xfs]() {
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                    }
 14)   1.382 us    |                  }
 14)   0.290 us    |                  xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)   5.721 us    |                }
 14)               |                xfs_calc_symlink_reservation [xfs]() {
 14)               |                  xfs_calc_icreate_reservation [xfs]() {
 14)               |                    xfs_calc_icreate_resv_alloc [xfs]() {
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                      }
 14)               |                      xfs_calc_inode_chunk_res [xfs]() {
 14)               |                        xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                          xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                        }
 14)   1.393 us    |                      }
 14)               |                      xfs_calc_inobt_res [xfs]() {
 14)               |                        xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                          xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                        }
 14)               |                        xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                          xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                        }
 14)   2.475 us    |                      }
 14)               |                      xfs_calc_finobt_res [xfs]() {
 14)               |                        xfs_calc_inobt_res [xfs]() {
 14)               |                          xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                            xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                          }
 14)               |                          xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                            xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                          }
 14)   2.464 us    |                        }
 14)   3.006 us    |                      }
 14)   9.047 us    |                    }
 14)               |                    xfs_calc_create_resv_modify [xfs]() {
 14)   0.291 us    |                      xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                      }
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                      }
 14)               |                      xfs_calc_finobt_res [xfs]() {
 14)               |                        xfs_calc_inobt_res [xfs]() {
 14)               |                          xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                            xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                          }
 14)               |                          xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                            xfs_buf_log_overhead [xfs]();
 14)   0.841 us    |                          }
 14)   2.635 us    |                        }
 14)   3.176 us    |                      }
 14)   6.452 us    |                    }
 14) + 16.330 us   |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14) + 17.963 us   |                }
 14)               |                xfs_calc_icreate_reservation [xfs]() {
 14)               |                  xfs_calc_icreate_resv_alloc [xfs]() {
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                    }
 14)               |                    xfs_calc_inode_chunk_res [xfs]() {
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                      }
 14)   1.383 us    |                    }
 14)               |                    xfs_calc_inobt_res [xfs]() {
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                      }
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                      }
 14)   2.465 us    |                    }
 14)               |                    xfs_calc_finobt_res [xfs]() {
 14)               |                      xfs_calc_inobt_res [xfs]() {
 14)               |                        xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                          xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                        }
 14)               |                        xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                          xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                        }
 14)   2.464 us    |                      }
 14)   3.016 us    |                    }
 14)   8.997 us    |                  }
 14)               |                  xfs_calc_create_resv_modify [xfs]() {
 14)   0.281 us    |                    xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                    }
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.822 us    |                    }
 14)               |                    xfs_calc_finobt_res [xfs]() {
 14)               |                      xfs_calc_inobt_res [xfs]() {
 14)               |                        xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                          xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                        }
 14)               |                        xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                          xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                        }
 14)   2.464 us    |                      }
 14)   3.006 us    |                    }
 14)   6.271 us    |                  }
 14) + 16.070 us   |                }
 14)               |                xfs_calc_create_tmpfile_reservation [xfs]() {
 14)               |                  xfs_calc_icreate_resv_alloc [xfs]() {
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                    }
 14)               |                    xfs_calc_inode_chunk_res [xfs]() {
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                      }
 14)   1.373 us    |                    }
 14)               |                    xfs_calc_inobt_res [xfs]() {
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.841 us    |                      }
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                      }
 14)   2.465 us    |                    }
 14)               |                    xfs_calc_finobt_res [xfs]() {
 14)               |                      xfs_calc_inobt_res [xfs]() {
 14)               |                        xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                          xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                        }
 14)               |                        xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                          xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                        }
 14)   2.464 us    |                      }
 14)   3.006 us    |                    }
 14)   9.006 us    |                  }
 14)               |                  xfs_calc_iunlink_add_reservation.isra.0 [xfs]() {
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                    }
 14)   1.382 us    |                  }
 14) + 11.171 us   |                }
 14)               |                xfs_calc_mkdir_reservation [xfs]() {
 14)               |                  xfs_calc_icreate_reservation [xfs]() {
 14)               |                    xfs_calc_icreate_resv_alloc [xfs]() {
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                      }
 14)               |                      xfs_calc_inode_chunk_res [xfs]() {
 14)               |                        xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                          xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                        }
 14)   1.373 us    |                      }
 14)               |                      xfs_calc_inobt_res [xfs]() {
 14)               |                        xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                          xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                        }
 14)               |                        xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                          xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                        }
 14)   2.465 us    |                      }
 14)               |                      xfs_calc_finobt_res [xfs]() {
 14)               |                        xfs_calc_inobt_res [xfs]() {
 14)               |                          xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                            xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                          }
 14)               |                          xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                            xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                          }
 14)   2.464 us    |                        }
 14)   3.016 us    |                      }
 14)   8.996 us    |                    }
 14)               |                    xfs_calc_create_resv_modify [xfs]() {
 14)   0.281 us    |                      xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                      }
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                      }
 14)               |                      xfs_calc_finobt_res [xfs]() {
 14)               |                        xfs_calc_inobt_res [xfs]() {
 14)               |                          xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                            xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                          }
 14)               |                          xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                            xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                          }
 14)   2.464 us    |                        }
 14)   3.016 us    |                      }
 14)   6.271 us    |                    }
 14) + 16.070 us   |                  }
 14) + 16.620 us   |                }
 14)               |                xfs_calc_ifree_reservation [xfs]() {
 14)   0.291 us    |                  xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)               |                  xfs_calc_iunlink_remove_reservation.isra.0 [xfs]() {
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                    }
 14)   1.373 us    |                  }
 14)               |                  xfs_calc_inode_chunk_res [xfs]() {
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                    }
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                    }
 14)   2.475 us    |                  }
 14)               |                  xfs_calc_inobt_res [xfs]() {
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                    }
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                    }
 14)   2.465 us    |                  }
 14)               |                  xfs_calc_finobt_res [xfs]() {
 14)               |                    xfs_calc_inobt_res [xfs]() {
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                      }
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                      }
 14)   2.464 us    |                    }
 14)   3.006 us    |                  }
 14) + 12.282 us   |                }
 14)               |                xfs_calc_addafork_reservation [xfs]() {
 14)   0.291 us    |                  xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                    xfs_buf_log_overhead [xfs]();
 14)   1.182 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)   5.540 us    |                }
 14)               |                xfs_calc_attrinval_reservation [xfs]() {
 14)   0.291 us    |                  xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.822 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)   4.097 us    |                }
 14)               |                xfs_calc_attrsetm_reservation.isra.0 [xfs]() {
 14)   0.291 us    |                  xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)   3.005 us    |                }
 14)               |                xfs_calc_attrrm_reservation [xfs]() {
 14)   0.281 us    |                  xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)   5.169 us    |                }
 14)               |                xfs_calc_growrtalloc_reservation [xfs]() {
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.822 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.280 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)   0.291 us    |                  xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14)   4.087 us    |                }
 14)               |                xfs_calc_qm_dqalloc_reservation [xfs]() {
 14)               |                  xfs_calc_write_reservation [xfs]() {
 14)   0.280 us    |                    xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                    }
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.821 us    |                    }
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                    }
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                    }
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                    }
 14)               |                    xfs_calc_refcountbt_reservation [xfs]() {
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.822 us    |                      }
 14)               |                      xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                        xfs_buf_log_overhead [xfs]();
 14)   0.822 us    |                      }
 14)   2.454 us    |                    }
 14)   8.987 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.832 us    |                  }
 14) + 10.609 us   |                }
 14)               |                xfs_calc_qm_setqlim_reservation [xfs]() {
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.822 us    |                  }
 14)   1.372 us    |                }
 14)               |                xfs_calc_clear_agi_bucket_reservation.isra.0 [xfs]() {
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.942 us    |                  }
 14)   1.483 us    |                }
 14)               |                xfs_calc_growdata_reservation [xfs]() {
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.821 us    |                  }
 14)   2.455 us    |                }
 14)               |                xfs_calc_ichange_reservation.isra.0 [xfs]() {
 14)   0.290 us    |                  xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.821 us    |                  }
 14)   1.914 us    |                }
 14)               |                xfs_calc_swrite_reservation.isra.0 [xfs]() {
 14)   0.290 us    |                  xfs_calc_inode_res.isra.0 [xfs]();
 14)   0.832 us    |                }
 14)               |                xfs_calc_swrite_reservation.isra.0 [xfs]() {
 14)   0.280 us    |                  xfs_calc_inode_res.isra.0 [xfs]();
 14)   0.832 us    |                }
 14)               |                xfs_calc_attrsetrt_reservation [xfs]() {
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.821 us    |                  }
 14)   2.465 us    |                }
 14)               |                xfs_calc_clear_agi_bucket_reservation.isra.0 [xfs]() {
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)   1.363 us    |                }
 14)               |                xfs_calc_growrtzero_reservation.isra.0 [xfs]() {
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)   1.373 us    |                }
 14)               |                xfs_calc_growrtfree_reservation [xfs]() {
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)   0.280 us    |                  xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)   4.078 us    |                }
 14) ! 163.722 us  |              }
 14)               |              xfs_calc_write_reservation_minlogsize [xfs]() {
 14)               |                xfs_calc_write_reservation [xfs]() {
 14)   0.290 us    |                  xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.821 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.821 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)   7.354 us    |                }
 14)   7.904 us    |              }
 14)               |              xfs_calc_itruncate_reservation_minlogsize [xfs]() {
 14)               |                xfs_calc_itruncate_reservation [xfs]() {
 14)   0.290 us    |                  xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.821 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.821 us    |                  }
 14)   5.190 us    |                }
 14)   5.931 us    |              }
 14)               |              xfs_calc_qm_dqalloc_reservation_minlogsize [xfs]() {
 14)               |                xfs_calc_qm_dqalloc_reservation [xfs]() {
 14)               |                  xfs_calc_write_reservation [xfs]() {
 14)   0.290 us    |                    xfs_calc_inode_res.isra.0 [xfs]();
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.821 us    |                    }
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                    }
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                    }
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                    }
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.281 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                    }
 14)               |                    xfs_calc_buf_res [xfs]() {
 14)   0.291 us    |                      xfs_buf_log_overhead [xfs]();
 14)   0.831 us    |                    }
 14)   7.344 us    |                  }
 14)               |                  xfs_calc_buf_res [xfs]() {
 14)   0.290 us    |                    xfs_buf_log_overhead [xfs]();
 14)   0.822 us    |                  }
 14)   8.976 us    |                }
 14)   9.528 us    |              }
 14) ! 189.219 us  |            }
 14)               |            xfs_log_calc_unit_res [xfs]() {
 14)   0.300 us    |              xlog_calc_unit_res [xfs]();
 14)   0.962 us    |            }
 14) ! 191.223 us  |          }
 14)               |          xfs_trans_ail_init [xfs]() {
 14)   0.451 us    |            kmem_alloc [xfs]();
 14) + 43.300 us   |          }
 14)               |          xlog_recover [xfs]() {
 14)               |            xlog_find_tail [xfs]() {
 14)               |              xlog_find_head [xfs]() {
 14)               |                xlog_find_zeroed [xfs]() {
 14)   0.511 us    |                  xlog_alloc_buffer [xfs]();
 14)               |                  xlog_bread [xfs]() {
 14)               |                    xlog_do_io [xfs]() {
 14) + 86.921 us   |                      xfs_rw_bdev [xfs]();
 ------------------------------------------
  8) kworker-349921 => xfsaild-439264
 ------------------------------------------

  8)               |  xfsaild [xfs]() {
 14) + 87.692 us   |                    }
 14) + 88.243 us   |                  }
 14)               |                  xlog_bread [xfs]() {
 14)               |                    xlog_do_io [xfs]() {
 14) + 63.447 us   |                      xfs_rw_bdev [xfs]();
 14) + 64.069 us   |                    }
 14) + 64.629 us   |                  }
 14)               |                  xlog_find_cycle_start.constprop.0 [xfs]() {
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 51.726 us   |                        xfs_rw_bdev [xfs]();
 14) + 52.377 us   |                      }
 14) + 52.958 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 52.036 us   |                        xfs_rw_bdev [xfs]();
 14) + 52.768 us   |                      }
 14) + 53.449 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 52.076 us   |                        xfs_rw_bdev [xfs]();
 14) + 52.737 us   |                      }
 14) + 53.329 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 27.802 us   |                        xfs_rw_bdev [xfs]();
 14) + 28.563 us   |                      }
 14) + 29.154 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 27.501 us   |                        xfs_rw_bdev [xfs]();
 14) + 28.142 us   |                      }
 14) + 28.733 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 28.422 us   |                        xfs_rw_bdev [xfs]();
 14) + 29.184 us   |                      }
 14) + 29.785 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 29.555 us   |                        xfs_rw_bdev [xfs]();
 14) + 30.326 us   |                      }
 14) + 30.927 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 28.823 us   |                        xfs_rw_bdev [xfs]();
 14) + 29.544 us   |                      }
 14) + 30.216 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 27.410 us   |                        xfs_rw_bdev [xfs]();
 14) + 28.062 us   |                      }
 14) + 28.653 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 58.509 us   |                        xfs_rw_bdev [xfs]();
 14) + 59.500 us   |                      }
 14) + 60.091 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 27.942 us   |                        xfs_rw_bdev [xfs]();
 14) + 28.623 us   |                      }
 14) + 29.314 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 27.481 us   |                        xfs_rw_bdev [xfs]();
 14) + 28.142 us   |                      }
 14) + 28.743 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 22.992 us   |                        xfs_rw_bdev [xfs]();
 14) + 23.634 us   |                      }
 14) + 24.234 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 23.624 us   |                        xfs_rw_bdev [xfs]();
 14) + 24.344 us   |                      }
 14) + 24.936 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 24.495 us   |                        xfs_rw_bdev [xfs]();
 14) + 25.277 us   |                      }
 14) + 25.978 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 23.183 us   |                        xfs_rw_bdev [xfs]();
 14) + 24.155 us   |                      }
 14) + 24.826 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 21.400 us   |                        xfs_rw_bdev [xfs]();
 14) + 22.021 us   |                      }
 14) + 22.582 us   |                    }
 14) ! 584.549 us  |                  }
 14)               |                  xlog_find_verify_cycle [xfs]() {
 14)   0.861 us    |                    xlog_alloc_buffer [xfs]();
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 25.837 us   |                        xfs_rw_bdev [xfs]();
 14) + 26.529 us   |                      }
 14) + 27.100 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 36.076 us   |                        xfs_rw_bdev [xfs]();
 14) + 36.768 us   |                      }
 14) + 37.339 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 22.612 us   |                        xfs_rw_bdev [xfs]();
 14) + 23.223 us   |                      }
 14) + 23.784 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 23.203 us   |                        xfs_rw_bdev [xfs]();
 14) + 23.814 us   |                      }
 14) + 24.375 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 22.041 us   |                        xfs_rw_bdev [xfs]();
 14) + 22.702 us   |                      }
 14) + 23.263 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 22.401 us   |                        xfs_rw_bdev [xfs]();
 14) + 23.013 us   |                      }
 14) + 23.573 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 22.842 us   |                        xfs_rw_bdev [xfs]();
 14) + 23.453 us   |                      }
 14) + 24.024 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 23.383 us   |                        xfs_rw_bdev [xfs]();
 14) + 24.004 us   |                      }
 14) + 24.575 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 22.762 us   |                        xfs_rw_bdev [xfs]();
 14) + 23.373 us   |                      }
 14) + 23.944 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 22.672 us   |                        xfs_rw_bdev [xfs]();
 14) + 23.314 us   |                      }
 14) + 23.904 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 22.571 us   |                        xfs_rw_bdev [xfs]();
 14) + 23.283 us   |                      }
 14) + 23.944 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 22.101 us   |                        xfs_rw_bdev [xfs]();
 14) + 22.912 us   |                      }
 14) + 23.503 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 22.631 us   |                        xfs_rw_bdev [xfs]();
 14) + 23.243 us   |                      }
 14) + 23.814 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 22.121 us   |                        xfs_rw_bdev [xfs]();
 14) + 22.772 us   |                      }
 14) + 23.373 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 22.151 us   |                        xfs_rw_bdev [xfs]();
 14) + 22.782 us   |                      }
 14) + 23.383 us   |                    }
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 22.331 us   |                        xfs_rw_bdev [xfs]();
 14) + 23.043 us   |                      }
 14) + 23.623 us   |                    }
 14) ! 404.547 us  |                  }
 14)               |                  xlog_find_verify_log_record [xfs]() {
 14)   2.244 us    |                    xlog_alloc_buffer [xfs]();
 14)               |                    xlog_bread [xfs]() {
 14)               |                      xlog_do_io [xfs]() {
 14) + 28.693 us   |                        xfs_rw_bdev [xfs]();
 14) + 29.434 us   |                      }
 14) + 30.015 us   |                    }
 14)   0.381 us    |                    xlog_header_check_mount [xfs]();
 14) + 34.483 us   |                  }
 14) # 1179.598 us |                }
 14) # 1180.300 us |              }
 14)   0.481 us    |              xlog_alloc_buffer [xfs]();
 14)               |              xlog_rseek_logrec_hdr [xfs]() {
 14)               |                xlog_bread [xfs]() {
 14)               |                  xlog_do_io [xfs]() {
 14) + 22.872 us   |                    xfs_rw_bdev [xfs]();
 14) + 23.493 us   |                  }
 14) + 24.034 us   |                }
 14)               |                xlog_bread [xfs]() {
 14)               |                  xlog_do_io [xfs]() {
 14) + 22.111 us   |                    xfs_rw_bdev [xfs]();
 14) + 22.732 us   |                  }
 14) + 23.303 us   |                }
 14) + 48.330 us   |              }
 14)               |              xlog_check_unmount_rec [xfs]() {
 14)               |                xlog_bread [xfs]() {
 14)               |                  xlog_do_io [xfs]() {
 14) + 22.672 us   |                    xfs_rw_bdev [xfs]();
 14) + 23.323 us   |                  }
 14) + 23.964 us   |                }
 14) + 24.596 us   |              }
 14)               |              xlog_clear_stale_blocks [xfs]() {
 14)               |                xlog_write_log_records [xfs]() {
 14) ! 373.840 us  |                  xlog_alloc_buffer [xfs]();
 14)   1.342 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.390 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.390 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.390 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.390 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.390 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.380 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.380 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.330 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.301 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.330 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.301 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.301 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.421 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.330 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.480 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.330 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.301 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.301 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.390 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.390 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.401 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.390 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.390 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.390 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.390 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.380 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.360 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.360 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.350 us    |                  xlog_add_record [xfs]();
 14)   0.350 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.481 us    |                  xlog_add_record [xfs]();
 14)   0.380 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.360 us    |                  xlog_add_record [xfs]();
 14)   0.370 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   1.332 us    |                  xlog_add_record [xfs]();
 14)   0.420 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.431 us    |                  xlog_add_record [xfs]();
 14)   0.330 us    |                  xlog_add_record [xfs]();
 14)   0.421 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.601 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.350 us    |                  xlog_add_record [xfs]();
 14)   0.370 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.330 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.330 us    |                  xlog_add_record [xfs]();
 14)   0.330 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.330 us    |                  xlog_add_record [xfs]();
 14)   0.360 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.360 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.350 us    |                  xlog_add_record [xfs]();
 14)   0.521 us    |                  xlog_add_record [xfs]();
 14)   0.441 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.330 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.330 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.410 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.380 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.330 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.761 us    |                  xlog_add_record [xfs]();
 14)   0.651 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.380 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.732 us    |                  xlog_add_record [xfs]();
 14)   0.652 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.350 us    |                  xlog_add_record [xfs]();
 14)   0.390 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.350 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.531 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.370 us    |                  xlog_add_record [xfs]();
 14)   0.370 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.330 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.441 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.380 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.642 us    |                  xlog_add_record [xfs]();
 14)   0.411 us    |                  xlog_add_record [xfs]();
 14)   0.380 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.732 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.380 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.380 us    |                  xlog_add_record [xfs]();
 14)   0.360 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.441 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.330 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.701 us    |                  xlog_add_record [xfs]();
 14)   0.601 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.431 us    |                  xlog_add_record [xfs]();
 14)   0.742 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.360 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.360 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.360 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.360 us    |                  xlog_add_record [xfs]();
 14)   0.400 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.370 us    |                  xlog_add_record [xfs]();
 14)   0.350 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.400 us    |                  xlog_add_record [xfs]();
 14)   0.330 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.370 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.350 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.591 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.370 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.350 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.390 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.410 us    |                  xlog_add_record [xfs]();
 14)   0.561 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.370 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.350 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.441 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.511 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.350 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.380 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.360 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.380 us    |                  xlog_add_record [xfs]();
 14)   0.441 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.500 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.400 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.400 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.360 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.401 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.400 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.400 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.390 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.421 us    |                  xlog_add_record [xfs]();
 14)   0.401 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.340 us    |                  xlog_add_record [xfs]();
 14)   0.361 us    |                  xlog_add_record [xfs]();
 14)   0.461 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.330 us    |                  xlog_add_record [xfs]();
 14)   0.330 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.390 us    |                  xlog_add_record [xfs]();
 14)   0.381 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.390 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.390 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.401 us    |                  xlog_add_record [xfs]();
 14)   0.400 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.400 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.351 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.401 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.390 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.391 us    |                  xlog_add_record [xfs]();
 14)   0.390 us    |                  xlog_add_record [xfs]();
 14)   0.371 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.341 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.330 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.330 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.331 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.311 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.321 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.320 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)   0.310 us    |                  xlog_add_record [xfs]();
 14)               |                  xlog_bwrite [xfs]() {
 14)               |                    xlog_do_io [xfs]() {
 14) # 2705.677 us |                      xfs_rw_bdev [xfs]();
 14) # 2707.560 us |                    }
 14) # 2708.833 us |                  }
 14) # 5500.538 us |                }
 14) # 5501.739 us |              }
 14) # 6758.761 us |            }
 14)   1.172 us    |            xfs_log_check_lsn [xfs]();
 14) # 6762.708 us |          }
 14)               |          xlog_cil_init_post_recovery [xfs]() {
 14)               |            xlog_ticket_alloc [xfs]() {
 14)   1.402 us    |              xlog_calc_unit_res [xfs]();
 14)   3.997 us    |            }
 14)   5.941 us    |          }
 14) # 7157.677 us |        }
 14)               |        xfs_inodegc_start [xfs]() {
 14)   2.044 us    |          xfs_inodegc_queue_all [xfs]();
 14)   4.227 us    |        }
 14)               |        xfs_blockgc_start [xfs]() {
 14)   1.493 us    |          xfs_perag_get_tag [xfs]();
 14)   3.367 us    |        }
 14)               |        xfs_iget [xfs]() {
 14)   1.162 us    |          xfs_perag_get [xfs]();
 14)               |          xfs_iget_cache_miss [xfs]() {
 14)               |            xfs_inode_alloc [xfs]() {
 14)   1.623 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.153 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.152 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.152 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.152 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.152 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.152 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.162 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.152 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.152 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.152 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.153 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.152 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.152 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.163 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.162 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.152 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.152 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.152 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.162 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.152 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.152 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.152 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.152 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.163 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.152 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.152 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.152 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.152 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.152 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.152 us    |              xfs_fs_inode_init_once [xfs]();
 14)   1.162 us    |              xfs_fs_inode_init_once [xfs]();
 14) + 86.650 us   |            }
 14)               |            xfs_imap [xfs]() {
 14)   1.052 us    |              xfs_perag_get [xfs]();
 14)               |              xfs_imap_lookup [xfs]() {
 14)               |                xfs_ialloc_read_agi [xfs]() {
 14)               |                  xfs_read_agi [xfs]() {
 14)               |                    xfs_trans_read_buf_map [xfs]() {
 14)               |                      xfs_buf_read_map [xfs]() {
 14)               |                        xfs_buf_get_map [xfs]() {
 14)   1.012 us    |                          xfs_perag_get [xfs]();
 14)               |                          xfs_buf_find_insert.constprop.0 [xfs]() {
 14)   1.613 us    |                            _xfs_buf_alloc [xfs]();
 14)   1.303 us    |                            kmem_alloc [xfs]();
 14)   6.151 us    |                          }
 14) + 10.429 us   |                        }
 14)               |                        __xfs_buf_submit [xfs]() {
 14)   0.891 us    |                          xfs_buf_hold [xfs]();
 14)               |                          _xfs_buf_ioapply [xfs]() {
 14)   7.444 us    |                            xfs_buf_ioapply_map [xfs]();
 14)   9.848 us    |                          }
 14) + 62.044 us   |                          xfs_buf_iowait [xfs]();
  0)   7.875 us    |  xfs_buf_bio_end_io [xfs]();
 ------------------------------------------
  0)    zvol-905    =>   kworker-18  
 ------------------------------------------

  0)               |  xfs_buf_ioend_work [xfs]() {
  0)               |    xfs_buf_ioend [xfs]() {
  0)               |      xfs_agi_read_verify [xfs]() {
  0)               |        xfs_agi_verify [xfs]() {
  0)   0.962 us    |          xfs_log_check_lsn [xfs]();
  0)   0.421 us    |          xfs_verify_magic [xfs]();
  0)   3.647 us    |        }
  0)   5.671 us    |      }
  0)   8.195 us    |    }
  0)   9.487 us    |  }
 14)   1.112 us    |                          xfs_buf_rele [xfs]();
 14) + 78.595 us   |                        }
 14) + 91.849 us   |                      }
 14) + 93.753 us   |                    }
 14)   0.901 us    |                    xfs_buf_set_ref [xfs]();
 14) + 97.440 us   |                  }
 14) + 99.454 us   |                }
 14)               |                xfs_inobt_init_cursor [xfs]() {
 14)   5.790 us    |                  xfs_inobt_init_common [xfs]();
 14)   7.995 us    |                }
 14)               |                xfs_btree_lookup [xfs]() {
 14)   0.902 us    |                  xfs_inobt_init_ptr_from_cur [xfs]();
 14)               |                  xfs_btree_lookup_get_block [xfs]() {
 14)   1.002 us    |                    xfs_btree_ptr_to_daddr [xfs]();
 14)               |                    xfs_btree_read_buf_block.constprop.0 [xfs]() {
 14)   0.892 us    |                      xfs_btree_ptr_to_daddr [xfs]();
 14)               |                      xfs_trans_read_buf_map [xfs]() {
 14)               |                        xfs_buf_read_map [xfs]() {
 14)               |                          xfs_buf_get_map [xfs]() {
 14)   1.052 us    |                            xfs_perag_get [xfs]();
 14)               |                            xfs_buf_find_insert.constprop.0 [xfs]() {
 14)   1.603 us    |                              _xfs_buf_alloc [xfs]();
 14)   2.054 us    |                              xfs_buf_alloc_pages [xfs]();
 14)   6.472 us    |                            }
 14)   0.932 us    |                            _xfs_buf_map_pages [xfs]();
 14) + 12.003 us   |                          }
 14)               |                          __xfs_buf_submit [xfs]() {
 14)   0.882 us    |                            xfs_buf_hold [xfs]();
 14)               |                            _xfs_buf_ioapply [xfs]() {
 14)   5.891 us    |                              xfs_buf_ioapply_map [xfs]();
 14)   7.955 us    |                            }
 14) + 55.352 us   |                            xfs_buf_iowait [xfs]();
 ------------------------------------------
  0)   kworker-18   =>    zvol-905   
 ------------------------------------------

  0)   2.976 us    |  xfs_buf_bio_end_io [xfs]();
 ------------------------------------------
  0)    zvol-905    =>   kworker-18  
 ------------------------------------------

  0)               |  xfs_buf_ioend_work [xfs]() {
  0)               |    xfs_buf_ioend [xfs]() {
  0)               |      xfs_inobt_read_verify [xfs]() {
  0)               |        xfs_btree_sblock_verify_crc [xfs]() {
  0)   0.411 us    |          xfs_log_check_lsn [xfs]();
  0)   2.896 us    |        }
  0)               |        xfs_inobt_verify [xfs]() {
  0)   0.381 us    |          xfs_verify_magic [xfs]();
  0)   0.521 us    |          xfs_btree_sblock_v5hdr_verify [xfs]();
  0)   0.521 us    |          xfs_btree_sblock_verify [xfs]();
  0)   3.075 us    |        }
  0)   7.524 us    |      }
  0)   9.607 us    |    }
  0) + 10.429 us   |  }
 14)   1.072 us    |                            xfs_buf_rele [xfs]();
 14) + 69.549 us   |                          }
 14) + 84.065 us   |                        }
 14) + 85.789 us   |                      }
 14)               |                      xfs_btree_set_refs [xfs]() {
 14)   0.882 us    |                        xfs_buf_set_ref [xfs]();
 14)   2.615 us    |                      }
 14) + 92.792 us   |                    }
 14)   0.942 us    |                    xfs_btree_setbuf [xfs]();
 14) + 98.492 us   |                  }
 14)               |                  xfs_lookup_get_search_key [xfs]() {
 14)   0.922 us    |                    xfs_btree_rec_offset [xfs]();
 14)   0.902 us    |                    xfs_inobt_init_key_from_rec [xfs]();
 14)   4.708 us    |                  }
 14)   0.911 us    |                  xfs_inobt_key_diff [xfs]();
 14) ! 109.963 us  |                }
 14)               |                xfs_inobt_get_rec [xfs]() {
 14)               |                  xfs_btree_get_rec [xfs]() {
 14)   0.971 us    |                    xfs_btree_rec_offset [xfs]();
 14)   2.695 us    |                  }
 14)   4.769 us    |                }
 14)               |                xfs_trans_brelse [xfs]() {
 14)   1.112 us    |                  xfs_buf_unlock [xfs]();
 14)   1.693 us    |                  xfs_buf_rele [xfs]();
 14)   5.380 us    |                }
 14)               |                xfs_btree_del_cursor [xfs]() {
 14)               |                  xfs_trans_brelse [xfs]() {
 14)   0.992 us    |                    xfs_buf_unlock [xfs]();
 14)   1.232 us    |                    xfs_buf_rele [xfs]();
 14)   4.718 us    |                  }
 14)   0.912 us    |                  xfs_perag_put [xfs]();
 14)   8.335 us    |                }
 14) ! 242.598 us  |              }
 14)   0.891 us    |              xfs_perag_put [xfs]();
 14) ! 248.148 us  |            }
 14)               |            xfs_imap_to_bp [xfs]() {
 14)               |              xfs_trans_read_buf_map [xfs]() {
 14)               |                xfs_buf_read_map [xfs]() {
 14)               |                  xfs_buf_get_map [xfs]() {
 14)   1.032 us    |                    xfs_perag_get [xfs]();
 14)               |                    xfs_buf_find_insert.constprop.0 [xfs]() {
 14)   1.303 us    |                      _xfs_buf_alloc [xfs]();
 14)   4.188 us    |                      xfs_buf_alloc_pages [xfs]();
 14)   8.225 us    |                    }
 14)   0.941 us    |                    _xfs_buf_map_pages [xfs]();
 14) + 13.665 us   |                  }
 14)               |                  __xfs_buf_submit [xfs]() {
 14)   0.882 us    |                    xfs_buf_hold [xfs]();
 14)               |                    _xfs_buf_ioapply [xfs]() {
 14)   6.382 us    |                      xfs_buf_ioapply_map [xfs]();
 14)   8.436 us    |                    }
 14) + 99.955 us   |                    xfs_buf_iowait [xfs]();
 ------------------------------------------
  0)   kworker-18   =>    zvol-905   
 ------------------------------------------

  0)   2.776 us    |  xfs_buf_bio_end_io [xfs]();
 ------------------------------------------
  0)    zvol-905    =>   kworker-18  
 ------------------------------------------

  0)               |  xfs_buf_ioend_work [xfs]() {
  0)               |    xfs_buf_ioend [xfs]() {
  0)               |      xfs_inode_buf_read_verify [xfs]() {
  0)               |        xfs_inode_buf_verify [xfs]() {
  0)   0.761 us    |          xfs_buf_offset [xfs]();
  0)   0.361 us    |          xfs_verify_magic16 [xfs]();
  0)   0.351 us    |          xfs_buf_offset [xfs]();
  0)   0.340 us    |          xfs_verify_magic16 [xfs]();
  0)   0.391 us    |          xfs_buf_offset [xfs]();
  0)   0.421 us    |          xfs_verify_magic16 [xfs]();
  0)   0.390 us    |          xfs_buf_offset [xfs]();
  0)   0.381 us    |          xfs_verify_magic16 [xfs]();
  0)   0.391 us    |          xfs_buf_offset [xfs]();
  0)   0.411 us    |          xfs_verify_magic16 [xfs]();
  0)   0.391 us    |          xfs_buf_offset [xfs]();
  0)   0.370 us    |          xfs_verify_magic16 [xfs]();
  0)   0.421 us    |          xfs_buf_offset [xfs]();
  0)   0.401 us    |          xfs_verify_magic16 [xfs]();
  0)   0.350 us    |          xfs_buf_offset [xfs]();
  0)   0.401 us    |          xfs_verify_magic16 [xfs]();
  0)   0.371 us    |          xfs_buf_offset [xfs]();
  0)   0.350 us    |          xfs_verify_magic16 [xfs]();
  0)   0.351 us    |          xfs_buf_offset [xfs]();
  0)   0.351 us    |          xfs_verify_magic16 [xfs]();
  0)   0.391 us    |          xfs_buf_offset [xfs]();
  0)   0.381 us    |          xfs_verify_magic16 [xfs]();
  0)   0.371 us    |          xfs_buf_offset [xfs]();
  0)   0.381 us    |          xfs_verify_magic16 [xfs]();
  0)   0.370 us    |          xfs_buf_offset [xfs]();
  0)   0.371 us    |          xfs_verify_magic16 [xfs]();
  0)   0.341 us    |          xfs_buf_offset [xfs]();
  0)   0.350 us    |          xfs_verify_magic16 [xfs]();
  0)   0.340 us    |          xfs_buf_offset [xfs]();
  0)   0.381 us    |          xfs_verify_magic16 [xfs]();
  0)   0.381 us    |          xfs_buf_offset [xfs]();
  0)   0.380 us    |          xfs_verify_magic16 [xfs]();
  0)   0.431 us    |          xfs_buf_offset [xfs]();
  0)   0.410 us    |          xfs_verify_magic16 [xfs]();
  0)   0.391 us    |          xfs_buf_offset [xfs]();
  0)   0.441 us    |          xfs_verify_magic16 [xfs]();
  0)   0.370 us    |          xfs_buf_offset [xfs]();
  0)   0.381 us    |          xfs_verify_magic16 [xfs]();
  0)   0.371 us    |          xfs_buf_offset [xfs]();
  0)   0.411 us    |          xfs_verify_magic16 [xfs]();
  0)   0.411 us    |          xfs_buf_offset [xfs]();
  0)   0.390 us    |          xfs_verify_magic16 [xfs]();
  0)   0.371 us    |          xfs_buf_offset [xfs]();
  0)   0.361 us    |          xfs_verify_magic16 [xfs]();
  0)   0.401 us    |          xfs_buf_offset [xfs]();
  0)   0.371 us    |          xfs_verify_magic16 [xfs]();
  0)   0.371 us    |          xfs_buf_offset [xfs]();
  0)   0.371 us    |          xfs_verify_magic16 [xfs]();
  0)   0.360 us    |          xfs_buf_offset [xfs]();
  0)   0.401 us    |          xfs_verify_magic16 [xfs]();
  0)   0.351 us    |          xfs_buf_offset [xfs]();
  0)   0.350 us    |          xfs_verify_magic16 [xfs]();
  0)   0.371 us    |          xfs_buf_offset [xfs]();
  0)   0.421 us    |          xfs_verify_magic16 [xfs]();
  0)   0.390 us    |          xfs_buf_offset [xfs]();
  0)   0.401 us    |          xfs_verify_magic16 [xfs]();
  0)   0.411 us    |          xfs_buf_offset [xfs]();
  0)   0.381 us    |          xfs_verify_magic16 [xfs]();
  0)   0.341 us    |          xfs_buf_offset [xfs]();
  0)   0.321 us    |          xfs_verify_magic16 [xfs]();
  0)   0.311 us    |          xfs_buf_offset [xfs]();
  0)   0.300 us    |          xfs_verify_magic16 [xfs]();
  0)   0.320 us    |          xfs_buf_offset [xfs]();
  0)   0.310 us    |          xfs_verify_magic16 [xfs]();
  0) + 47.067 us   |        }
  0) + 47.839 us   |      }
  0) + 50.002 us   |    }
  0) + 50.804 us   |  }
 14)   1.102 us    |                    xfs_buf_rele [xfs]();
 14) ! 114.622 us  |                  }
 14) ! 130.801 us  |                }
 14) ! 132.515 us  |              }
 14) ! 134.217 us  |            }
 14)   0.932 us    |            xfs_buf_offset [xfs]();
 14)               |            xfs_inode_from_disk [xfs]() {
 14)               |              xfs_dinode_verify.part.0 [xfs]() {
 14)   0.912 us    |                xfs_mode_to_ftype [xfs]();
 14)   0.962 us    |                xfs_dinode_verify_fork [xfs]();
 14)   1.603 us    |                xfs_inode_validate_extsize [xfs]();
 14)   1.163 us    |                xfs_inode_validate_cowextsize [xfs]();
 14) + 12.222 us   |              }
 14)               |              xfs_iformat_data_fork [xfs]() {
 14)               |                xfs_iformat_local [xfs]() {
 14)               |                  xfs_init_local_fork [xfs]() {
 14)   1.242 us    |                    kmem_alloc [xfs]();
 14)   3.066 us    |                  }
 14)   4.949 us    |                }
 14)               |                xfs_ifork_verify_local_data [xfs]() {
 14)               |                  xfs_dir2_sf_verify [xfs]() {
 14)               |                    xfs_dir_ino_validate [xfs]() {
 14)               |                      xfs_verify_dir_ino [xfs]() {
 14)               |                        xfs_agino_range [xfs]() {
 14)   0.922 us    |                          __xfs_agino_range [xfs]();
 14)   2.785 us    |                        }
 14)   4.639 us    |                      }
 14)   6.642 us    |                    }
 14)               |                    xfs_dir_ino_validate [xfs]() {
 14)               |                      xfs_verify_dir_ino [xfs]() {
 14)               |                        xfs_agino_range [xfs]() {
 14)   0.891 us    |                          __xfs_agino_range [xfs]();
 14)   2.565 us    |                        }
 14)   4.288 us    |                      }
 14)   5.981 us    |                    }
 14) + 15.489 us   |                  }
 14) + 17.562 us   |                }
 14) + 25.357 us   |              }
 14) + 41.016 us   |            }
 14)   0.891 us    |            xfs_buf_set_ref [xfs]();
 14)               |            xfs_trans_brelse [xfs]() {
 14)   1.031 us    |              xfs_buf_unlock [xfs]();
 14)   1.182 us    |              xfs_buf_rele [xfs]();
 14)   4.719 us    |            }
 14)   0.922 us    |            xfs_iget_check_free_state [xfs]();
 14)   1.152 us    |            xfs_ilock_nowait [xfs]();
 14) ! 529.748 us  |          }
 14)   0.901 us    |          xfs_perag_put [xfs]();
 14)               |          xfs_setup_inode [xfs]() {
 14)               |            xfs_diflags_to_iflags [xfs]() {
 14)   0.931 us    |              xfs_ip2xflags [xfs]();
 14)   2.805 us    |            }
 14)   4.960 us    |          }
 14)   0.922 us    |          xfs_setup_iops [xfs]();
 14) ! 543.954 us  |        }
 14)   0.982 us    |        xfs_iunlock [xfs]();
 14)               |        xfs_rtmount_inodes [xfs]() {
 14)               |          xfs_iget [xfs]() {
 14)   1.032 us    |            xfs_perag_get [xfs]();
 14)               |            xfs_iget_cache_miss [xfs]() {
 14)   3.086 us    |              xfs_inode_alloc [xfs]();
 14)               |              xfs_imap [xfs]() {
 14)   1.062 us    |                xfs_perag_get [xfs]();
 14)   0.901 us    |                xfs_perag_put [xfs]();
 14)   4.539 us    |              }
 14)               |              xfs_imap_to_bp [xfs]() {
 14)               |                xfs_trans_read_buf_map [xfs]() {
 14)               |                  xfs_buf_read_map [xfs]() {
 14)               |                    xfs_buf_get_map [xfs]() {
 14)   1.012 us    |                      xfs_perag_get [xfs]();
 14)               |                      xfs_buf_find_lock [xfs]() {
 14)   1.143 us    |                        xfs_buf_lock [xfs]();
 14)   3.025 us    |                      }
 14)   0.901 us    |                      xfs_perag_put [xfs]();
 14)   0.902 us    |                      _xfs_buf_map_pages [xfs]();
 14) + 10.209 us   |                    }
 14) + 11.942 us   |                  }
 14) + 13.645 us   |                }
 14) + 15.349 us   |              }
 14)   0.882 us    |              xfs_buf_offset [xfs]();
 14)               |              xfs_inode_from_disk [xfs]() {
 14)               |                xfs_dinode_verify.part.0 [xfs]() {
 14)   0.932 us    |                  xfs_mode_to_ftype [xfs]();
 14)   0.962 us    |                  xfs_dinode_verify_fork [xfs]();
 14)   0.951 us    |                  xfs_inode_validate_extsize [xfs]();
 14)   0.912 us    |                  xfs_inode_validate_cowextsize [xfs]();
 14)   8.796 us    |                }
 14)               |                xfs_iformat_data_fork [xfs]() {
 14)   0.982 us    |                  xfs_iformat_extents [xfs]();
 14)   2.925 us    |                }
 14) + 14.417 us   |              }
 14)   0.892 us    |              xfs_buf_set_ref [xfs]();
 14)               |              xfs_trans_brelse [xfs]() {
 14)   1.012 us    |                xfs_buf_unlock [xfs]();
 14)   1.052 us    |                xfs_buf_rele [xfs]();
 14)   4.559 us    |              }
 14)   0.892 us    |              xfs_iget_check_free_state [xfs]();
 14) + 52.758 us   |            }
 14)   0.902 us    |            xfs_perag_put [xfs]();
 14)               |            xfs_setup_inode [xfs]() {
 14)               |              xfs_diflags_to_iflags [xfs]() {
 14)   0.932 us    |                xfs_ip2xflags [xfs]();
 14)   2.665 us    |              }
 14)   4.448 us    |            }
 14)   0.912 us    |            xfs_setup_iops [xfs]();
 14) + 65.421 us   |          }
 14)               |          xfs_trans_alloc_empty [xfs]() {
 14)               |            xfs_trans_alloc [xfs]() {
 14)   0.981 us    |              xfs_trans_reserve [xfs]();
 14)   6.773 us    |            }
 14)   8.585 us    |          }
 14)   1.002 us    |          xfs_ilock [xfs]();
 14)   0.902 us    |          xfs_iread_extents [xfs]();
 14)   0.942 us    |          xfs_iunlock [xfs]();
 14)               |          xfs_trans_cancel [xfs]() {
 14)   0.992 us    |            xfs_trans_unreserve_and_mod_sb [xfs]();
 14)   0.922 us    |            xfs_trans_unreserve_and_mod_dquots [xfs]();
 14)   0.911 us    |            xfs_trans_free_items [xfs]();
 14)               |            xfs_trans_free [xfs]() {
 14)   1.012 us    |              xfs_extent_busy_clear [xfs]();
 14)   0.911 us    |              xfs_trans_free_dqinfo [xfs]();
 14)   4.919 us    |            }
 14) + 12.543 us   |          }
 14)               |          xfs_iget [xfs]() {
 14)   1.032 us    |            xfs_perag_get [xfs]();
 14)               |            xfs_iget_cache_miss [xfs]() {
 14)   3.607 us    |              xfs_inode_alloc [xfs]();
 14)               |              xfs_imap [xfs]() {
 14)   1.012 us    |                xfs_perag_get [xfs]();
 14)   0.902 us    |                xfs_perag_put [xfs]();
 14)   4.459 us    |              }
 14)               |              xfs_imap_to_bp [xfs]() {
 14)               |                xfs_trans_read_buf_map [xfs]() {
 14)               |                  xfs_buf_read_map [xfs]() {
 14)               |                    xfs_buf_get_map [xfs]() {
 14)   1.012 us    |                      xfs_perag_get [xfs]();
 14)               |                      xfs_buf_find_lock [xfs]() {
 14)   1.052 us    |                        xfs_buf_lock [xfs]();
 14)   2.755 us    |                      }
 14)   0.891 us    |                      xfs_perag_put [xfs]();
 14)   0.892 us    |                      _xfs_buf_map_pages [xfs]();
 14)   9.838 us    |                    }
 14) + 11.542 us   |                  }
 14) + 13.244 us   |                }
 14) + 14.938 us   |              }
 14)   0.892 us    |              xfs_buf_offset [xfs]();
 14)               |              xfs_inode_from_disk [xfs]() {
 14)               |                xfs_dinode_verify.part.0 [xfs]() {
 14)   0.902 us    |                  xfs_mode_to_ftype [xfs]();
 14)   0.912 us    |                  xfs_dinode_verify_fork [xfs]();
 14)   0.952 us    |                  xfs_inode_validate_extsize [xfs]();
 14)   0.901 us    |                  xfs_inode_validate_cowextsize [xfs]();
 14)   8.586 us    |                }
 14)               |                xfs_iformat_data_fork [xfs]() {
 14)   0.912 us    |                  xfs_iformat_extents [xfs]();
 14)   2.614 us    |                }
 14) + 13.896 us   |              }
 14)   0.892 us    |              xfs_buf_set_ref [xfs]();
 14)               |              xfs_trans_brelse [xfs]() {
 14)   1.002 us    |                xfs_buf_unlock [xfs]();
 14)   1.001 us    |                xfs_buf_rele [xfs]();
 14)   4.519 us    |              }
 14)   0.892 us    |              xfs_iget_check_free_state [xfs]();
 14) + 51.776 us   |            }
 14)   0.892 us    |            xfs_perag_put [xfs]();
 14)               |            xfs_setup_inode [xfs]() {
 14)               |              xfs_diflags_to_iflags [xfs]() {
 14)   0.902 us    |                xfs_ip2xflags [xfs]();
 14)   2.605 us    |              }
 14)   4.378 us    |            }
 14)   0.892 us    |            xfs_setup_iops [xfs]();
 14) + 64.269 us   |          }
 14)               |          xfs_trans_alloc_empty [xfs]() {
 14)               |            xfs_trans_alloc [xfs]() {
 14)   0.892 us    |              xfs_trans_reserve [xfs]();
 14)   2.835 us    |            }
 14)   4.529 us    |          }
 14)   0.952 us    |          xfs_ilock [xfs]();
 14)   0.892 us    |          xfs_iread_extents [xfs]();
 14)   1.353 us    |          xfs_iunlock [xfs]();
 14)               |          xfs_trans_cancel [xfs]() {
 14)   0.912 us    |            xfs_trans_unreserve_and_mod_sb [xfs]();
 14)   0.922 us    |            xfs_trans_unreserve_and_mod_dquots [xfs]();
 14)   0.891 us    |            xfs_trans_free_items [xfs]();
 14)               |            xfs_trans_free [xfs]() {
 14)   0.912 us    |              xfs_extent_busy_clear [xfs]();
 14)   0.881 us    |              xfs_trans_free_dqinfo [xfs]();
 14)   4.448 us    |            }
 14) + 11.281 us   |          }
 14) ! 184.080 us  |        }
 14)               |        xfs_check_summary_counts [xfs]() {
 14)               |          xfs_verify_icount [xfs]() {
 14)   1.032 us    |            xfs_perag_get [xfs]();
 14)   0.902 us    |            xfs_perag_put [xfs]();
 14)   0.992 us    |            xfs_perag_get [xfs]();
 14)   0.891 us    |            xfs_perag_put [xfs]();
 14)   0.992 us    |            xfs_perag_get [xfs]();
 14)   0.882 us    |            xfs_perag_put [xfs]();
 14)   1.002 us    |            xfs_perag_get [xfs]();
 14)   0.882 us    |            xfs_perag_put [xfs]();
 14) + 15.268 us   |          }
 14)   0.972 us    |          xfs_fs_measure_sickness [xfs]();
 14) + 19.165 us   |        }
 14)               |        xfs_fs_reserve_ag_blocks [xfs]() {
 14)   1.012 us    |          xfs_perag_get [xfs]();
 14)               |          xfs_ag_resv_init [xfs]() {
 14)               |            xfs_refcountbt_calc_reserves [xfs]() {
 14)               |              xfs_alloc_read_agf [xfs]() {
 14)               |                xfs_read_agf [xfs]() {
 14)               |                  xfs_trans_read_buf_map [xfs]() {
 14)               |                    xfs_buf_read_map [xfs]() {
 14)               |                      xfs_buf_get_map [xfs]() {
 14)   1.012 us    |                        xfs_perag_get [xfs]();
 14)               |                        xfs_buf_find_insert.constprop.0 [xfs]() {
 14)   1.313 us    |                          _xfs_buf_alloc [xfs]();
 14)   1.313 us    |                          kmem_alloc [xfs]();
 14)   5.420 us    |                        }
 14)   9.127 us    |                      }
 14)               |                      __xfs_buf_submit [xfs]() {
 14)   0.882 us    |                        xfs_buf_hold [xfs]();
 14)               |                        _xfs_buf_ioapply [xfs]() {
 14)   6.081 us    |                          xfs_buf_ioapply_map [xfs]();
 14)   7.975 us    |                        }
 14) + 38.631 us   |                        xfs_buf_iowait [xfs]();
 ------------------------------------------
  0)   kworker-18   =>    zvol-905   
 ------------------------------------------

  0)   3.327 us    |  xfs_buf_bio_end_io [xfs]();
 ------------------------------------------
  0)    zvol-905    =>   kworker-18  
 ------------------------------------------

  0)               |  xfs_buf_ioend_work [xfs]() {
  0)               |    xfs_buf_ioend [xfs]() {
  0)               |      xfs_agf_read_verify [xfs]() {
  0)               |        xfs_agf_verify [xfs]() {
  0)   0.411 us    |          xfs_log_check_lsn [xfs]();
  0)   0.381 us    |          xfs_verify_magic [xfs]();
  0)   2.385 us    |        }
  0)   3.737 us    |      }
  0)   5.941 us    |    }
  0)   6.732 us    |  }
 14)   1.132 us    |                        xfs_buf_rele [xfs]();
 14) + 52.907 us   |                      }
 14) + 64.569 us   |                    }
 14) + 66.272 us   |                  }
 14)   0.892 us    |                  xfs_buf_set_ref [xfs]();
 14) + 69.699 us   |                }
 14) + 71.823 us   |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   1.022 us    |                xfs_buf_unlock [xfs]();
 14)   1.172 us    |                xfs_buf_rele [xfs]();
 14)   4.698 us    |              }
 14)   0.942 us    |              xfs_btree_calc_size [xfs]();
 14) + 81.190 us   |            }
 14)               |            xfs_finobt_calc_reserves [xfs]() {
 14)               |              xfs_ialloc_read_agi [xfs]() {
 14)               |                xfs_read_agi [xfs]() {
 14)               |                  xfs_trans_read_buf_map [xfs]() {
 14)               |                    xfs_buf_read_map [xfs]() {
 14)               |                      xfs_buf_get_map [xfs]() {
 14)   1.032 us    |                        xfs_perag_get [xfs]();
 14)               |                        xfs_buf_find_lock [xfs]() {
 14)   1.042 us    |                          xfs_buf_lock [xfs]();
 14)   2.735 us    |                        }
 14)   0.882 us    |                        xfs_perag_put [xfs]();
 14)   8.276 us    |                      }
 14)   9.988 us    |                    }
 14) + 11.692 us   |                  }
 14)   0.882 us    |                  xfs_buf_set_ref [xfs]();
 14) + 15.078 us   |                }
 14) + 16.801 us   |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   0.992 us    |                xfs_buf_unlock [xfs]();
 14)   1.152 us    |                xfs_buf_rele [xfs]();
 14)   4.689 us    |              }
 14)   0.932 us    |              xfs_btree_calc_size [xfs]();
 14) + 25.788 us   |            }
 14)               |            __xfs_ag_resv_init [xfs]() {
 14)   1.393 us    |              xfs_mod_freecounter [xfs]();
 14)   3.126 us    |            }
 14)   0.902 us    |            xfs_rmapbt_calc_reserves [xfs]();
 14)               |            __xfs_ag_resv_init [xfs]() {
 14)   1.011 us    |              xfs_mod_freecounter [xfs]();
 14)   2.685 us    |            }
 14)               |            xfs_alloc_read_agf [xfs]() {
 14)               |              xfs_read_agf [xfs]() {
 14)               |                xfs_trans_read_buf_map [xfs]() {
 14)               |                  xfs_buf_read_map [xfs]() {
 14)               |                    xfs_buf_get_map [xfs]() {
 14)   1.021 us    |                      xfs_perag_get [xfs]();
 14)               |                      xfs_buf_find_lock [xfs]() {
 14)   0.992 us    |                        xfs_buf_lock [xfs]();
 14)   2.425 us    |                      }
 14)   0.601 us    |                      xfs_perag_put [xfs]();
 14)   6.943 us    |                    }
 14)   8.376 us    |                  }
 14)   9.798 us    |                }
 14)   0.591 us    |                xfs_buf_set_ref [xfs]();
 14) + 12.353 us   |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   0.651 us    |                xfs_buf_unlock [xfs]();
 14)   0.732 us    |                xfs_buf_rele [xfs]();
 14)   3.025 us    |              }
 14) + 17.252 us   |            }
 14) ! 137.314 us  |          }
 14)   0.360 us    |          xfs_perag_put [xfs]();
 14)   0.411 us    |          xfs_perag_get [xfs]();
 14)               |          xfs_ag_resv_init [xfs]() {
 14)               |            xfs_refcountbt_calc_reserves [xfs]() {
 14)               |              xfs_alloc_read_agf [xfs]() {
 14)               |                xfs_read_agf [xfs]() {
 14)               |                  xfs_trans_read_buf_map [xfs]() {
 14)               |                    xfs_buf_read_map [xfs]() {
 14)               |                      xfs_buf_get_map [xfs]() {
 14)   0.401 us    |                        xfs_perag_get [xfs]();
 14)               |                        xfs_buf_find_insert.constprop.0 [xfs]() {
 14)   0.591 us    |                          _xfs_buf_alloc [xfs]();
 14)   0.531 us    |                          kmem_alloc [xfs]();
 14)   2.154 us    |                        }
 14)   3.837 us    |                      }
 14)               |                      __xfs_buf_submit [xfs]() {
 14)   0.291 us    |                        xfs_buf_hold [xfs]();
 14)               |                        _xfs_buf_ioapply [xfs]() {
 14)   3.396 us    |                          xfs_buf_ioapply_map [xfs]();
 14)   4.128 us    |                        }
 14) + 84.967 us   |                        xfs_buf_iowait [xfs]();
 ------------------------------------------
  0)   kworker-18   =>    zvol-905   
 ------------------------------------------

  0)   2.635 us    |  xfs_buf_bio_end_io [xfs]();
 ------------------------------------------
  0)    zvol-905    =>   kworker-18  
 ------------------------------------------

  0)               |  xfs_buf_ioend_work [xfs]() {
  0)               |    xfs_buf_ioend [xfs]() {
  0)               |      xfs_agf_read_verify [xfs]() {
  0)               |        xfs_agf_verify [xfs]() {
  0)   0.391 us    |          xfs_log_check_lsn [xfs]();
  0)   0.401 us    |          xfs_verify_magic [xfs]();
  0)   2.074 us    |        }
  0)   3.206 us    |      }
  0)   4.709 us    |    }
  0)   5.480 us    |  }
 14)   0.401 us    |                        xfs_buf_rele [xfs]();
 14) + 91.248 us   |                      }
 14) + 96.017 us   |                    }
 14) + 96.668 us   |                  }
 14)   0.290 us    |                  xfs_buf_set_ref [xfs]();
 14) + 97.861 us   |                }
 14) + 98.562 us   |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   0.330 us    |                xfs_buf_unlock [xfs]();
 14)   0.381 us    |                xfs_buf_rele [xfs]();
 14)   1.523 us    |              }
 14)   0.301 us    |              xfs_btree_calc_size [xfs]();
 14) ! 101.588 us  |            }
 14)               |            xfs_finobt_calc_reserves [xfs]() {
 14)               |              xfs_ialloc_read_agi [xfs]() {
 14)               |                xfs_read_agi [xfs]() {
 14)               |                  xfs_trans_read_buf_map [xfs]() {
 14)               |                    xfs_buf_read_map [xfs]() {
 14)               |                      xfs_buf_get_map [xfs]() {
 14)   0.341 us    |                        xfs_perag_get [xfs]();
 14)               |                        xfs_buf_find_insert.constprop.0 [xfs]() {
 14)   0.471 us    |                          _xfs_buf_alloc [xfs]();
 14)   0.461 us    |                          kmem_alloc [xfs]();
 14)   1.854 us    |                        }
 14)   3.065 us    |                      }
 14)               |                      __xfs_buf_submit [xfs]() {
 14)   0.291 us    |                        xfs_buf_hold [xfs]();
 14)               |                        _xfs_buf_ioapply [xfs]() {
 14)   2.835 us    |                          xfs_buf_ioapply_map [xfs]();
 14)   3.567 us    |                        }
 14) + 30.326 us   |                        xfs_buf_iowait [xfs]();
 ------------------------------------------
  0)   kworker-18   =>    zvol-905   
 ------------------------------------------

  0)   2.365 us    |  xfs_buf_bio_end_io [xfs]();
 ------------------------------------------
  0)    zvol-905    =>   kworker-18  
 ------------------------------------------

  0)               |  xfs_buf_ioend_work [xfs]() {
  0)               |    xfs_buf_ioend [xfs]() {
  0)               |      xfs_agi_read_verify [xfs]() {
  0)               |        xfs_agi_verify [xfs]() {
  0)   0.380 us    |          xfs_log_check_lsn [xfs]();
  0)   0.391 us    |          xfs_verify_magic [xfs]();
  0)   2.084 us    |        }
  0)   3.226 us    |      }
  0)   4.849 us    |    }
  0)   5.620 us    |  }
 14)   0.401 us    |                        xfs_buf_rele [xfs]();
 14) + 36.046 us   |                      }
 14) + 40.034 us   |                    }
 14) + 40.685 us   |                  }
 14)   0.291 us    |                  xfs_buf_set_ref [xfs]();
 14) + 41.787 us   |                }
 14) + 42.418 us   |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   0.331 us    |                xfs_buf_unlock [xfs]();
 14)   0.401 us    |                xfs_buf_rele [xfs]();
 14)   1.542 us    |              }
 14)   0.300 us    |              xfs_btree_calc_size [xfs]();
 14) + 45.454 us   |            }
 14)               |            __xfs_ag_resv_init [xfs]() {
 14)   0.341 us    |              xfs_mod_freecounter [xfs]();
 14)   0.892 us    |            }
 14)   0.291 us    |            xfs_rmapbt_calc_reserves [xfs]();
 14)               |            __xfs_ag_resv_init [xfs]() {
 14)   0.330 us    |              xfs_mod_freecounter [xfs]();
 14)   0.882 us    |            }
 14)               |            xfs_alloc_read_agf [xfs]() {
 14)               |              xfs_read_agf [xfs]() {
 14)               |                xfs_trans_read_buf_map [xfs]() {
 14)               |                  xfs_buf_read_map [xfs]() {
 14)               |                    xfs_buf_get_map [xfs]() {
 14)   0.331 us    |                      xfs_perag_get [xfs]();
 14)               |                      xfs_buf_find_lock [xfs]() {
 14)   0.340 us    |                        xfs_buf_lock [xfs]();
 14)   0.892 us    |                      }
 14)   0.291 us    |                      xfs_perag_put [xfs]();
 14)   2.655 us    |                    }
 14)   3.216 us    |                  }
 14)   3.787 us    |                }
 14)   0.290 us    |                xfs_buf_set_ref [xfs]();
 14)   4.889 us    |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   0.321 us    |                xfs_buf_unlock [xfs]();
 14)   0.331 us    |                xfs_buf_rele [xfs]();
 14)   1.453 us    |              }
 14)   7.174 us    |            }
 14) ! 158.573 us  |          }
 14)   0.290 us    |          xfs_perag_put [xfs]();
 14)   0.330 us    |          xfs_perag_get [xfs]();
 14)               |          xfs_ag_resv_init [xfs]() {
 14)               |            xfs_refcountbt_calc_reserves [xfs]() {
 14)               |              xfs_alloc_read_agf [xfs]() {
 14)               |                xfs_read_agf [xfs]() {
 14)               |                  xfs_trans_read_buf_map [xfs]() {
 14)               |                    xfs_buf_read_map [xfs]() {
 14)               |                      xfs_buf_get_map [xfs]() {
 14)   0.321 us    |                        xfs_perag_get [xfs]();
 14)               |                        xfs_buf_find_insert.constprop.0 [xfs]() {
 14)   0.551 us    |                          _xfs_buf_alloc [xfs]();
 14)   1.242 us    |                          kmem_alloc [xfs]();
 14)   2.695 us    |                        }
 14)   4.178 us    |                      }
 14)               |                      __xfs_buf_submit [xfs]() {
 14)   0.290 us    |                        xfs_buf_hold [xfs]();
 14)               |                        _xfs_buf_ioapply [xfs]() {
 14)   2.976 us    |                          xfs_buf_ioapply_map [xfs]();
 14)   3.717 us    |                        }
 14) + 39.323 us   |                        xfs_buf_iowait [xfs]();
 ------------------------------------------
  0)   kworker-18   =>    zvol-905   
 ------------------------------------------

  0)   2.314 us    |  xfs_buf_bio_end_io [xfs]();
 ------------------------------------------
  0)    zvol-905    =>   kworker-18  
 ------------------------------------------

  0)               |  xfs_buf_ioend_work [xfs]() {
  0)               |    xfs_buf_ioend [xfs]() {
  0)               |      xfs_agf_read_verify [xfs]() {
  0)               |        xfs_agf_verify [xfs]() {
  0)   0.391 us    |          xfs_log_check_lsn [xfs]();
  0)   0.401 us    |          xfs_verify_magic [xfs]();
  0)   2.124 us    |        }
  0)   3.306 us    |      }
  0)   4.899 us    |    }
  0)   5.700 us    |  }
 14)   0.401 us    |                        xfs_buf_rele [xfs]();
 14) + 45.234 us   |                      }
 14) + 50.263 us   |                    }
 14) + 50.834 us   |                  }
 14)   0.291 us    |                  xfs_buf_set_ref [xfs]();
 14) + 51.966 us   |                }
 14) + 52.607 us   |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   0.331 us    |                xfs_buf_unlock [xfs]();
 14)   0.371 us    |                xfs_buf_rele [xfs]();
 14)   1.523 us    |              }
 14)   0.300 us    |              xfs_btree_calc_size [xfs]();
 14) + 55.573 us   |            }
 14)               |            xfs_finobt_calc_reserves [xfs]() {
 14)               |              xfs_ialloc_read_agi [xfs]() {
 14)               |                xfs_read_agi [xfs]() {
 14)               |                  xfs_trans_read_buf_map [xfs]() {
 14)               |                    xfs_buf_read_map [xfs]() {
 14)               |                      xfs_buf_get_map [xfs]() {
 14)   0.330 us    |                        xfs_perag_get [xfs]();
 14)               |                        xfs_buf_find_insert.constprop.0 [xfs]() {
 14)   0.471 us    |                          _xfs_buf_alloc [xfs]();
 14)   0.511 us    |                          kmem_alloc [xfs]();
 14)   1.883 us    |                        }
 14)   3.106 us    |                      }
 14)               |                      __xfs_buf_submit [xfs]() {
 14)   0.290 us    |                        xfs_buf_hold [xfs]();
 14)               |                        _xfs_buf_ioapply [xfs]() {
 14)   2.244 us    |                          xfs_buf_ioapply_map [xfs]();
 14)   2.975 us    |                        }
 14) + 28.572 us   |                        xfs_buf_iowait [xfs]();
 ------------------------------------------
  0)   kworker-18   =>    zvol-905   
 ------------------------------------------

  0)   2.314 us    |  xfs_buf_bio_end_io [xfs]();
 ------------------------------------------
  0)    zvol-905    =>   kworker-18  
 ------------------------------------------

  0)               |  xfs_buf_ioend_work [xfs]() {
  0)               |    xfs_buf_ioend [xfs]() {
  0)               |      xfs_agi_read_verify [xfs]() {
  0)               |        xfs_agi_verify [xfs]() {
  0)   0.360 us    |          xfs_log_check_lsn [xfs]();
  0)   0.341 us    |          xfs_verify_magic [xfs]();
  0)   1.994 us    |        }
  0)   3.125 us    |      }
  0)   4.698 us    |    }
  0)   6.061 us    |  }
 14)   0.351 us    |                        xfs_buf_rele [xfs]();
 14) + 33.682 us   |                      }
 14) + 37.590 us   |                    }
 14) + 38.160 us   |                  }
 14)   0.290 us    |                  xfs_buf_set_ref [xfs]();
 14) + 39.253 us   |                }
 14) + 39.813 us   |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   0.330 us    |                xfs_buf_unlock [xfs]();
 14)   0.371 us    |                xfs_buf_rele [xfs]();
 14)   1.803 us    |              }
 14)   0.300 us    |              xfs_btree_calc_size [xfs]();
 14) + 43.090 us   |            }
 14)               |            __xfs_ag_resv_init [xfs]() {
 14)   0.391 us    |              xfs_mod_freecounter [xfs]();
 14)   0.942 us    |            }
 14)   0.290 us    |            xfs_rmapbt_calc_reserves [xfs]();
 14)               |            __xfs_ag_resv_init [xfs]() {
 14)   0.331 us    |              xfs_mod_freecounter [xfs]();
 14)   0.881 us    |            }
 14)               |            xfs_alloc_read_agf [xfs]() {
 14)               |              xfs_read_agf [xfs]() {
 14)               |                xfs_trans_read_buf_map [xfs]() {
 14)               |                  xfs_buf_read_map [xfs]() {
 14)               |                    xfs_buf_get_map [xfs]() {
 14)   0.331 us    |                      xfs_perag_get [xfs]();
 14)               |                      xfs_buf_find_lock [xfs]() {
 14)   0.341 us    |                        xfs_buf_lock [xfs]();
 14)   0.891 us    |                      }
 14)   0.290 us    |                      xfs_perag_put [xfs]();
 14)   2.665 us    |                    }
 14)   3.226 us    |                  }
 14)   3.797 us    |                }
 14)   0.291 us    |                xfs_buf_set_ref [xfs]();
 14)   4.899 us    |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   0.321 us    |                xfs_buf_unlock [xfs]();
 14)   0.321 us    |                xfs_buf_rele [xfs]();
 14)   1.453 us    |              }
 14)   7.163 us    |            }
 14) ! 109.822 us  |          }
 14)   0.291 us    |          xfs_perag_put [xfs]();
 14)   0.321 us    |          xfs_perag_get [xfs]();
 14)               |          xfs_ag_resv_init [xfs]() {
 14)               |            xfs_refcountbt_calc_reserves [xfs]() {
 14)               |              xfs_alloc_read_agf [xfs]() {
 14)               |                xfs_read_agf [xfs]() {
 14)               |                  xfs_trans_read_buf_map [xfs]() {
 14)               |                    xfs_buf_read_map [xfs]() {
 14)               |                      xfs_buf_get_map [xfs]() {
 14)   0.330 us    |                        xfs_perag_get [xfs]();
 14)               |                        xfs_buf_find_insert.constprop.0 [xfs]() {
 14)   0.451 us    |                          _xfs_buf_alloc [xfs]();
 14)   0.461 us    |                          kmem_alloc [xfs]();
 14)   1.804 us    |                        }
 14)   3.266 us    |                      }
 14)               |                      __xfs_buf_submit [xfs]() {
 14)   0.291 us    |                        xfs_buf_hold [xfs]();
 14)               |                        _xfs_buf_ioapply [xfs]() {
 14)   2.865 us    |                          xfs_buf_ioapply_map [xfs]();
 14)   3.617 us    |                        }
 14) + 78.845 us   |                        xfs_buf_iowait [xfs]();
 ------------------------------------------
  0)   kworker-18   =>    zvol-905   
 ------------------------------------------

  0)   2.795 us    |  xfs_buf_bio_end_io [xfs]();
 ------------------------------------------
  0)    zvol-905    =>   kworker-18  
 ------------------------------------------

  0)               |  xfs_buf_ioend_work [xfs]() {
  0)               |    xfs_buf_ioend [xfs]() {
  0)               |      xfs_agf_read_verify [xfs]() {
  0)               |        xfs_agf_verify [xfs]() {
  0)   0.391 us    |          xfs_log_check_lsn [xfs]();
  0)   0.421 us    |          xfs_verify_magic [xfs]();
  0)   2.054 us    |        }
  0)   3.146 us    |      }
  0)   5.180 us    |    }
  0)   5.992 us    |  }
 14)   0.351 us    |                        xfs_buf_rele [xfs]();
 14) + 84.796 us   |                      }
 14) + 88.894 us   |                    }
 14) + 89.485 us   |                  }
 14)   0.291 us    |                  xfs_buf_set_ref [xfs]();
 14) + 90.607 us   |                }
 14) + 91.239 us   |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   0.330 us    |                xfs_buf_unlock [xfs]();
 14)   0.381 us    |                xfs_buf_rele [xfs]();
 14)   1.523 us    |              }
 14)   0.300 us    |              xfs_btree_calc_size [xfs]();
 14) + 94.234 us   |            }
 14)               |            xfs_finobt_calc_reserves [xfs]() {
 14)               |              xfs_ialloc_read_agi [xfs]() {
 14)               |                xfs_read_agi [xfs]() {
 14)               |                  xfs_trans_read_buf_map [xfs]() {
 14)               |                    xfs_buf_read_map [xfs]() {
 14)               |                      xfs_buf_get_map [xfs]() {
 14)   0.331 us    |                        xfs_perag_get [xfs]();
 14)               |                        xfs_buf_find_insert.constprop.0 [xfs]() {
 14)   0.641 us    |                          _xfs_buf_alloc [xfs]();
 14)   0.551 us    |                          kmem_alloc [xfs]();
 14)   2.103 us    |                        }
 14)   3.316 us    |                      }
 14)               |                      __xfs_buf_submit [xfs]() {
 14)   0.280 us    |                        xfs_buf_hold [xfs]();
 14)               |                        _xfs_buf_ioapply [xfs]() {
 14)   3.276 us    |                          xfs_buf_ioapply_map [xfs]();
 14)   4.078 us    |                        }
 14) + 28.943 us   |                        xfs_buf_iowait [xfs]();
 ------------------------------------------
  0)   kworker-18   =>    zvol-905   
 ------------------------------------------

  0)   2.174 us    |  xfs_buf_bio_end_io [xfs]();
 ------------------------------------------
  0)    zvol-905    =>   kworker-18  
 ------------------------------------------

  0)               |  xfs_buf_ioend_work [xfs]() {
  0)               |    xfs_buf_ioend [xfs]() {
  0)               |      xfs_agi_read_verify [xfs]() {
  0)               |        xfs_agi_verify [xfs]() {
  0)   0.390 us    |          xfs_log_check_lsn [xfs]();
  0)   0.401 us    |          xfs_verify_magic [xfs]();
  0)   2.064 us    |        }
  0)   3.145 us    |      }
  0)   4.799 us    |    }
  0)   5.510 us    |  }
 14)   0.340 us    |                        xfs_buf_rele [xfs]();
 14) + 35.356 us   |                      }
 14) + 39.503 us   |                    }
 14) + 40.054 us   |                  }
 14)   0.671 us    |                  xfs_buf_set_ref [xfs]();
 14) + 41.527 us   |                }
 14) + 42.087 us   |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   0.330 us    |                xfs_buf_unlock [xfs]();
 14)   0.380 us    |                xfs_buf_rele [xfs]();
 14)   1.583 us    |              }
 14)   0.301 us    |              xfs_btree_calc_size [xfs]();
 14) + 45.153 us   |            }
 14)               |            __xfs_ag_resv_init [xfs]() {
 14)   0.380 us    |              xfs_mod_freecounter [xfs]();
 14)   0.932 us    |            }
 14)   0.291 us    |            xfs_rmapbt_calc_reserves [xfs]();
 14)               |            __xfs_ag_resv_init [xfs]() {
 14)   0.330 us    |              xfs_mod_freecounter [xfs]();
 14)   0.872 us    |            }
 14)               |            xfs_alloc_read_agf [xfs]() {
 14)               |              xfs_read_agf [xfs]() {
 14)               |                xfs_trans_read_buf_map [xfs]() {
 14)               |                  xfs_buf_read_map [xfs]() {
 14)               |                    xfs_buf_get_map [xfs]() {
 14)   0.341 us    |                      xfs_perag_get [xfs]();
 14)               |                      xfs_buf_find_lock [xfs]() {
 14)   0.340 us    |                        xfs_buf_lock [xfs]();
 14)   0.902 us    |                      }
 14)   0.291 us    |                      xfs_perag_put [xfs]();
 14)   2.665 us    |                    }
 14)   3.226 us    |                  }
 14)   3.777 us    |                }
 14)   0.290 us    |                xfs_buf_set_ref [xfs]();
 14)   4.879 us    |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   0.320 us    |                xfs_buf_unlock [xfs]();
 14)   0.331 us    |                xfs_buf_rele [xfs]();
 14)   1.463 us    |              }
 14)   7.144 us    |            }
 14) ! 150.508 us  |          }
 14)   0.291 us    |          xfs_perag_put [xfs]();
 14) ! 564.473 us  |        }
 14)               |        xfs_log_mount_finish [xfs]() {
 14) + 13.575 us   |          xfs_printk_level [xfs]();
 14)               |          xfs_buftarg_drain [xfs]() {
 14)   5.270 us    |            xfs_buftarg_wait [xfs]();
 14)   0.481 us    |            xfs_buftarg_drain_rele [xfs]();
 14)   0.311 us    |            xfs_buftarg_drain_rele [xfs]();
 14)   0.311 us    |            xfs_buftarg_drain_rele [xfs]();
 14)   0.321 us    |            xfs_buftarg_drain_rele [xfs]();
 14)   0.311 us    |            xfs_buftarg_drain_rele [xfs]();
 14)   0.311 us    |            xfs_buftarg_drain_rele [xfs]();
 14)   0.310 us    |            xfs_buftarg_drain_rele [xfs]();
 14)   0.320 us    |            xfs_buftarg_drain_rele [xfs]();
 14)   0.320 us    |            xfs_buftarg_drain_rele [xfs]();
 14)   0.310 us    |            xfs_buftarg_drain_rele [xfs]();
 14)               |            xfs_buf_rele [xfs]() {
 14)   0.310 us    |              xfs_perag_put [xfs]();
 14)   0.641 us    |              xfs_buf_free [xfs]();
 14)   1.954 us    |            }
 14)               |            xfs_buf_rele [xfs]() {
 14)   0.290 us    |              xfs_perag_put [xfs]();
 14)   0.461 us    |              xfs_buf_free [xfs]();
 14)   1.683 us    |            }
 14)               |            xfs_buf_rele [xfs]() {
 14)   0.291 us    |              xfs_perag_put [xfs]();
 14)   0.381 us    |              xfs_buf_free [xfs]();
 14)   1.583 us    |            }
 14)               |            xfs_buf_rele [xfs]() {
 14)   0.290 us    |              xfs_perag_put [xfs]();
 14)   0.370 us    |              xfs_buf_free [xfs]();
 14)   1.563 us    |            }
 14)               |            xfs_buf_rele [xfs]() {
 14)   0.291 us    |              xfs_perag_put [xfs]();
 14)   0.521 us    |              xfs_buf_free [xfs]();
 14)   1.854 us    |            }
 14)               |            xfs_buf_rele [xfs]() {
 14)   0.291 us    |              xfs_perag_put [xfs]();
 14)   0.411 us    |              xfs_buf_free [xfs]();
 14)   1.603 us    |            }
 14)               |            xfs_buf_rele [xfs]() {
 14)   0.291 us    |              xfs_perag_put [xfs]();
 14)   0.401 us    |              xfs_buf_free [xfs]();
 14)   2.013 us    |            }
 14)               |            xfs_buf_rele [xfs]() {
 14)   0.280 us    |              xfs_perag_put [xfs]();
 14)               |              xfs_buf_free [xfs]() {
 14)   2.936 us    |                xfs_buf_free_pages [xfs]();
 14)   3.556 us    |              }
 14)   4.739 us    |            }
 14)               |            xfs_buf_rele [xfs]() {
 14)   0.290 us    |              xfs_perag_put [xfs]();
 14)               |              xfs_buf_free [xfs]() {
 14)   0.360 us    |                xfs_buf_free_pages [xfs]();
 14)   0.952 us    |              }
 14)   2.194 us    |            }
 14)               |            xfs_buf_rele [xfs]() {
 14)   0.291 us    |              xfs_perag_put [xfs]();
 14)   0.411 us    |              xfs_buf_free [xfs]();
 14)   1.613 us    |            }
 14) + 37.038 us   |          }
 14) + 52.958 us   |        }
 14)               |        xfs_fs_unreserve_ag_blocks [xfs]() {
 14)   0.331 us    |          xfs_perag_get [xfs]();
 14)               |          xfs_ag_resv_free [xfs]() {
 14)               |            __xfs_ag_resv_free [xfs]() {
 14)   0.341 us    |              xfs_mod_freecounter [xfs]();
 14)   0.961 us    |            }
 14)               |            __xfs_ag_resv_free [xfs]() {
 14)   0.321 us    |              xfs_mod_freecounter [xfs]();
 14)   0.882 us    |            }
 14)   2.655 us    |          }
 14)   0.290 us    |          xfs_perag_put [xfs]();
 14)   0.320 us    |          xfs_perag_get [xfs]();
 14)               |          xfs_ag_resv_free [xfs]() {
 14)               |            __xfs_ag_resv_free [xfs]() {
 14)   0.330 us    |              xfs_mod_freecounter [xfs]();
 14)   0.892 us    |            }
 14)               |            __xfs_ag_resv_free [xfs]() {
 14)   0.351 us    |              xfs_mod_freecounter [xfs]();
 14)   0.922 us    |            }
 14)   2.615 us    |          }
 14)   0.291 us    |          xfs_perag_put [xfs]();
 14)   0.361 us    |          xfs_perag_get [xfs]();
 14)               |          xfs_ag_resv_free [xfs]() {
 14)               |            __xfs_ag_resv_free [xfs]() {
 14)   0.341 us    |              xfs_mod_freecounter [xfs]();
 14)   0.891 us    |            }
 14)               |            __xfs_ag_resv_free [xfs]() {
 14)   0.341 us    |              xfs_mod_freecounter [xfs]();
 14)   0.891 us    |            }
 14)   2.575 us    |          }
 14)   0.291 us    |          xfs_perag_put [xfs]();
 14)   0.331 us    |          xfs_perag_get [xfs]();
 14)               |          xfs_ag_resv_free [xfs]() {
 14)               |            __xfs_ag_resv_free [xfs]() {
 14)   0.340 us    |              xfs_mod_freecounter [xfs]();
 14)   0.882 us    |            }
 14)               |            __xfs_ag_resv_free [xfs]() {
 14)   0.340 us    |              xfs_mod_freecounter [xfs]();
 14)   0.892 us    |            }
 14)   2.575 us    |          }
 14)   0.290 us    |          xfs_perag_put [xfs]();
 14) + 16.401 us   |        }
 14)               |        xfs_reserve_blocks [xfs]() {
 14)   0.370 us    |          xfs_mod_freecounter [xfs]();
 14)   0.361 us    |          xfs_mod_freecounter [xfs]();
 14)   2.064 us    |        }
 14)               |        xfs_fs_reserve_ag_blocks [xfs]() {
 14)   0.331 us    |          xfs_perag_get [xfs]();
 14)               |          xfs_ag_resv_init [xfs]() {
 14)               |            xfs_refcountbt_calc_reserves [xfs]() {
 14)               |              xfs_alloc_read_agf [xfs]() {
 14)               |                xfs_read_agf [xfs]() {
 14)               |                  xfs_trans_read_buf_map [xfs]() {
 14)               |                    xfs_buf_read_map [xfs]() {
 14)               |                      xfs_buf_get_map [xfs]() {
 14)   0.321 us    |                        xfs_perag_get [xfs]();
 14)               |                        xfs_buf_find_insert.constprop.0 [xfs]() {
 14)   0.511 us    |                          _xfs_buf_alloc [xfs]();
 14)   0.511 us    |                          kmem_alloc [xfs]();
 14)   1.994 us    |                        }
 14)   3.185 us    |                      }
 14)               |                      __xfs_buf_submit [xfs]() {
 14)   0.291 us    |                        xfs_buf_hold [xfs]();
 14)               |                        _xfs_buf_ioapply [xfs]() {
 14)   5.300 us    |                          xfs_buf_ioapply_map [xfs]();
 14)   5.992 us    |                        }
 14) + 87.231 us   |                        xfs_buf_iowait [xfs]();
 ------------------------------------------
  8) xfsaild-439264 =>    zvol-905   
 ------------------------------------------

  8)   7.895 us    |  xfs_buf_bio_end_io [xfs]();
 ------------------------------------------
  8)    zvol-905    => kworker-349921
 ------------------------------------------

  8)               |  xfs_buf_ioend_work [xfs]() {
  8)               |    xfs_buf_ioend [xfs]() {
  8)               |      xfs_agf_read_verify [xfs]() {
  8)               |        xfs_agf_verify [xfs]() {
  8)   0.912 us    |          xfs_log_check_lsn [xfs]();
  8)   0.722 us    |          xfs_verify_magic [xfs]();
  8)   3.847 us    |        }
  8)   6.161 us    |      }
  8)   9.257 us    |    }
  8) + 11.461 us   |  }
 14)   0.350 us    |                        xfs_buf_rele [xfs]();
 14) + 95.246 us   |                      }
 14) + 99.404 us   |                    }
 14) + 99.965 us   |                  }
 14)   0.290 us    |                  xfs_buf_set_ref [xfs]();
 14) ! 101.067 us  |                }
 14) ! 101.628 us  |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   0.331 us    |                xfs_buf_unlock [xfs]();
 14)   0.401 us    |                xfs_buf_rele [xfs]();
 14)   1.542 us    |              }
 14)   0.300 us    |              xfs_btree_calc_size [xfs]();
 14) ! 104.573 us  |            }
 14)               |            xfs_finobt_calc_reserves [xfs]() {
 14)               |              xfs_ialloc_read_agi [xfs]() {
 14)               |                xfs_read_agi [xfs]() {
 14)               |                  xfs_trans_read_buf_map [xfs]() {
 14)               |                    xfs_buf_read_map [xfs]() {
 14)               |                      xfs_buf_get_map [xfs]() {
 14)   0.330 us    |                        xfs_perag_get [xfs]();
 14)               |                        xfs_buf_find_insert.constprop.0 [xfs]() {
 14)   0.491 us    |                          _xfs_buf_alloc [xfs]();
 14)   0.521 us    |                          kmem_alloc [xfs]();
 14)   1.903 us    |                        }
 14)   3.116 us    |                      }
 14)               |                      __xfs_buf_submit [xfs]() {
 14)   0.290 us    |                        xfs_buf_hold [xfs]();
 14)               |                        _xfs_buf_ioapply [xfs]() {
 14)   2.735 us    |                          xfs_buf_ioapply_map [xfs]();
 14)   3.386 us    |                        }
 14) + 40.114 us   |                        xfs_buf_iowait [xfs]();
 ------------------------------------------
  8) kworker-349921 =>    zvol-905   
 ------------------------------------------

  8)   3.316 us    |  xfs_buf_bio_end_io [xfs]();
 ------------------------------------------
  8)    zvol-905    => kworker-349921
 ------------------------------------------

  8)               |  xfs_buf_ioend_work [xfs]() {
  8)               |    xfs_buf_ioend [xfs]() {
  8)               |      xfs_agi_read_verify [xfs]() {
  8)               |        xfs_agi_verify [xfs]() {
  8)   0.661 us    |          xfs_log_check_lsn [xfs]();
  8)   0.651 us    |          xfs_verify_magic [xfs]();
  8)   3.446 us    |        }
  8)   5.359 us    |      }
  8)   7.854 us    |    }
  8)   9.197 us    |  }
 14)   0.351 us    |                        xfs_buf_rele [xfs]();
 14) + 45.514 us   |                      }
 14) + 49.451 us   |                    }
 14) + 50.002 us   |                  }
 14)   0.290 us    |                  xfs_buf_set_ref [xfs]();
 14) + 51.104 us   |                }
 14) + 51.766 us   |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   0.330 us    |                xfs_buf_unlock [xfs]();
 14)   0.380 us    |                xfs_buf_rele [xfs]();
 14)   1.523 us    |              }
 14)   0.301 us    |              xfs_btree_calc_size [xfs]();
 14) + 54.671 us   |            }
 14)               |            __xfs_ag_resv_init [xfs]() {
 14)   0.461 us    |              xfs_mod_freecounter [xfs]();
 14)   1.012 us    |            }
 14)   0.291 us    |            xfs_rmapbt_calc_reserves [xfs]();
 14)               |            __xfs_ag_resv_init [xfs]() {
 14)   0.330 us    |              xfs_mod_freecounter [xfs]();
 14)   0.882 us    |            }
 14)               |            xfs_alloc_read_agf [xfs]() {
 14)               |              xfs_read_agf [xfs]() {
 14)               |                xfs_trans_read_buf_map [xfs]() {
 14)               |                  xfs_buf_read_map [xfs]() {
 14)               |                    xfs_buf_get_map [xfs]() {
 14)   0.341 us    |                      xfs_perag_get [xfs]();
 14)               |                      xfs_buf_find_lock [xfs]() {
 14)   0.340 us    |                        xfs_buf_lock [xfs]();
 14)   0.902 us    |                      }
 14)   0.291 us    |                      xfs_perag_put [xfs]();
 14)   2.705 us    |                    }
 14)   3.256 us    |                  }
 14)   3.807 us    |                }
 14)   0.290 us    |                xfs_buf_set_ref [xfs]();
 14)   4.909 us    |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   0.321 us    |                xfs_buf_unlock [xfs]();
 14)   0.321 us    |                xfs_buf_rele [xfs]();
 14)   1.453 us    |              }
 14)   7.184 us    |            }
 14) ! 170.465 us  |          }
 14)   0.290 us    |          xfs_perag_put [xfs]();
 14)   0.320 us    |          xfs_perag_get [xfs]();
 14)               |          xfs_ag_resv_init [xfs]() {
 14)               |            xfs_refcountbt_calc_reserves [xfs]() {
 14)               |              xfs_alloc_read_agf [xfs]() {
 14)               |                xfs_read_agf [xfs]() {
 14)               |                  xfs_trans_read_buf_map [xfs]() {
 14)               |                    xfs_buf_read_map [xfs]() {
 14)               |                      xfs_buf_get_map [xfs]() {
 14)   0.331 us    |                        xfs_perag_get [xfs]();
 14)               |                        xfs_buf_find_insert.constprop.0 [xfs]() {
 14)   0.691 us    |                          _xfs_buf_alloc [xfs]();
 14)   0.401 us    |                          kmem_alloc [xfs]();
 14)   1.983 us    |                        }
 14)   3.176 us    |                      }
 14)               |                      __xfs_buf_submit [xfs]() {
 14)   0.290 us    |                        xfs_buf_hold [xfs]();
 14)               |                        _xfs_buf_ioapply [xfs]() {
 14)   3.085 us    |                          xfs_buf_ioapply_map [xfs]();
 14)   4.478 us    |                        }
 14) + 39.563 us   |                        xfs_buf_iowait [xfs]();
 ------------------------------------------
  8) kworker-349921 =>    zvol-905   
 ------------------------------------------

  8)   3.006 us    |  xfs_buf_bio_end_io [xfs]();
 ------------------------------------------
  8)    zvol-905    => kworker-349921
 ------------------------------------------

  8)               |  xfs_buf_ioend_work [xfs]() {
  8)               |    xfs_buf_ioend [xfs]() {
  8)               |      xfs_agf_read_verify [xfs]() {
  8)               |        xfs_agf_verify [xfs]() {
  8)   0.671 us    |          xfs_log_check_lsn [xfs]();
  8)   0.652 us    |          xfs_verify_magic [xfs]();
  8)   3.186 us    |        }
  8)   4.949 us    |      }
  8)   7.013 us    |    }
  8)   8.375 us    |  }
 14)   0.351 us    |                        xfs_buf_rele [xfs]();
 14) + 46.065 us   |                      }
 14) + 50.043 us   |                    }
 14) + 50.603 us   |                  }
 14)   0.290 us    |                  xfs_buf_set_ref [xfs]();
 14) + 51.706 us   |                }
 14) + 52.256 us   |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   0.321 us    |                xfs_buf_unlock [xfs]();
 14)   0.381 us    |                xfs_buf_rele [xfs]();
 14)   1.513 us    |              }
 14)   0.301 us    |              xfs_btree_calc_size [xfs]();
 14) + 55.172 us   |            }
 14)               |            xfs_finobt_calc_reserves [xfs]() {
 14)               |              xfs_ialloc_read_agi [xfs]() {
 14)               |                xfs_read_agi [xfs]() {
 14)               |                  xfs_trans_read_buf_map [xfs]() {
 14)               |                    xfs_buf_read_map [xfs]() {
 14)               |                      xfs_buf_get_map [xfs]() {
 14)   0.330 us    |                        xfs_perag_get [xfs]();
 14)               |                        xfs_buf_find_insert.constprop.0 [xfs]() {
 14)   0.390 us    |                          _xfs_buf_alloc [xfs]();
 14)   0.431 us    |                          kmem_alloc [xfs]();
 14)   1.713 us    |                        }
 14)   2.926 us    |                      }
 14)               |                      __xfs_buf_submit [xfs]() {
 14)   0.290 us    |                        xfs_buf_hold [xfs]();
 14)               |                        _xfs_buf_ioapply [xfs]() {
 14)   3.306 us    |                          xfs_buf_ioapply_map [xfs]();
 14)   3.957 us    |                        }
 14) + 38.250 us   |                        xfs_buf_iowait [xfs]();
 ------------------------------------------
  8) kworker-349921 =>    zvol-905   
 ------------------------------------------

  8)   2.936 us    |  xfs_buf_bio_end_io [xfs]();
 ------------------------------------------
  8)    zvol-905    => kworker-349921
 ------------------------------------------

  8)               |  xfs_buf_ioend_work [xfs]() {
  8)               |    xfs_buf_ioend [xfs]() {
  8)               |      xfs_agi_read_verify [xfs]() {
  8)               |        xfs_agi_verify [xfs]() {
  8)   0.651 us    |          xfs_log_check_lsn [xfs]();
  8)   0.641 us    |          xfs_verify_magic [xfs]();
  8)   3.366 us    |        }
  8)   5.099 us    |      }
  8)   7.163 us    |    }
  8)   8.486 us    |  }
 14)   0.350 us    |                        xfs_buf_rele [xfs]();
 14) + 44.222 us   |                      }
 14) + 47.948 us   |                    }
 14) + 48.520 us   |                  }
 14)   0.291 us    |                  xfs_buf_set_ref [xfs]();
 14) + 49.611 us   |                }
 14) + 50.193 us   |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   0.330 us    |                xfs_buf_unlock [xfs]();
 14)   0.381 us    |                xfs_buf_rele [xfs]();
 14)   1.523 us    |              }
 14)   0.300 us    |              xfs_btree_calc_size [xfs]();
 14) + 53.088 us   |            }
 14)               |            __xfs_ag_resv_init [xfs]() {
 14)   0.340 us    |              xfs_mod_freecounter [xfs]();
 14)   0.892 us    |            }
 14)   0.291 us    |            xfs_rmapbt_calc_reserves [xfs]();
 14)               |            __xfs_ag_resv_init [xfs]() {
 14)   0.330 us    |              xfs_mod_freecounter [xfs]();
 14)   0.882 us    |            }
 14)               |            xfs_alloc_read_agf [xfs]() {
 14)               |              xfs_read_agf [xfs]() {
 14)               |                xfs_trans_read_buf_map [xfs]() {
 14)               |                  xfs_buf_read_map [xfs]() {
 14)               |                    xfs_buf_get_map [xfs]() {
 14)   0.341 us    |                      xfs_perag_get [xfs]();
 14)               |                      xfs_buf_find_lock [xfs]() {
 14)   0.341 us    |                        xfs_buf_lock [xfs]();
 14)   0.902 us    |                      }
 14)   0.290 us    |                      xfs_perag_put [xfs]();
 14)   2.755 us    |                    }
 14)   3.306 us    |                  }
 14)   3.868 us    |                }
 14)   0.281 us    |                xfs_buf_set_ref [xfs]();
 14)   4.959 us    |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   0.321 us    |                xfs_buf_unlock [xfs]();
 14)   0.321 us    |                xfs_buf_rele [xfs]();
 14)   1.452 us    |              }
 14)   7.234 us    |            }
 14) ! 119.420 us  |          }
 14)   0.290 us    |          xfs_perag_put [xfs]();
 14)   0.330 us    |          xfs_perag_get [xfs]();
 14)               |          xfs_ag_resv_init [xfs]() {
 14)               |            xfs_refcountbt_calc_reserves [xfs]() {
 14)               |              xfs_alloc_read_agf [xfs]() {
 14)               |                xfs_read_agf [xfs]() {
 14)               |                  xfs_trans_read_buf_map [xfs]() {
 14)               |                    xfs_buf_read_map [xfs]() {
 14)               |                      xfs_buf_get_map [xfs]() {
 14)   0.320 us    |                        xfs_perag_get [xfs]();
 14)               |                        xfs_buf_find_insert.constprop.0 [xfs]() {
 14)   0.662 us    |                          _xfs_buf_alloc [xfs]();
 14)   0.691 us    |                          kmem_alloc [xfs]();
 14)   2.244 us    |                        }
 14)   3.917 us    |                      }
 14)               |                      __xfs_buf_submit [xfs]() {
 14)   0.411 us    |                        xfs_buf_hold [xfs]();
 14)               |                        _xfs_buf_ioapply [xfs]() {
 14)   3.276 us    |                          xfs_buf_ioapply_map [xfs]();
 14)   4.007 us    |                        }
 14) ! 251.795 us  |                        xfs_buf_iowait [xfs]();
 ------------------------------------------
  8) kworker-349921 =>    zvol-905   
 ------------------------------------------

  8)   3.266 us    |  xfs_buf_bio_end_io [xfs]();
 ------------------------------------------
  8)    zvol-905    => kworker-349921
 ------------------------------------------

  8)               |  xfs_buf_ioend_work [xfs]() {
  8)               |    xfs_buf_ioend [xfs]() {
  8)               |      xfs_agf_read_verify [xfs]() {
  8)               |        xfs_agf_verify [xfs]() {
  8)   0.731 us    |          xfs_log_check_lsn [xfs]();
  8)   0.701 us    |          xfs_verify_magic [xfs]();
  8)   3.466 us    |        }
  8)   5.339 us    |      }
  8)   8.015 us    |    }
  8)   9.427 us    |  }
 14)   0.401 us    |                        xfs_buf_rele [xfs]();
 14) ! 258.056 us  |                      }
 14) ! 262.825 us  |                    }
 14) ! 263.386 us  |                  }
 14)   0.320 us    |                  xfs_buf_set_ref [xfs]();
 14) ! 264.549 us  |                }
 14) ! 265.130 us  |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   0.351 us    |                xfs_buf_unlock [xfs]();
 14)   0.421 us    |                xfs_buf_rele [xfs]();
 14)   1.823 us    |              }
 14)   0.321 us    |              xfs_btree_calc_size [xfs]();
 14) ! 268.456 us  |            }
 14)               |            xfs_finobt_calc_reserves [xfs]() {
 14)               |              xfs_ialloc_read_agi [xfs]() {
 14)               |                xfs_read_agi [xfs]() {
 14)               |                  xfs_trans_read_buf_map [xfs]() {
 14)               |                    xfs_buf_read_map [xfs]() {
 14)               |                      xfs_buf_get_map [xfs]() {
 14)   0.381 us    |                        xfs_perag_get [xfs]();
 14)               |                        xfs_buf_find_insert.constprop.0 [xfs]() {
 14)   0.952 us    |                          _xfs_buf_alloc [xfs]();
 14)   1.643 us    |                          kmem_alloc [xfs]();
 14)   3.707 us    |                        }
 14)   5.139 us    |                      }
 14)               |                      __xfs_buf_submit [xfs]() {
 14)   0.300 us    |                        xfs_buf_hold [xfs]();
 14)               |                        _xfs_buf_ioapply [xfs]() {
 14)   4.909 us    |                          xfs_buf_ioapply_map [xfs]();
 14)   5.600 us    |                        }
 14) + 43.561 us   |                        xfs_buf_iowait [xfs]();
 ------------------------------------------
  8) kworker-349921 =>    zvol-905   
 ------------------------------------------

  8)   3.286 us    |  xfs_buf_bio_end_io [xfs]();
 ------------------------------------------
  8)    zvol-905    => kworker-349921
 ------------------------------------------

  8)               |  xfs_buf_ioend_work [xfs]() {
  8)               |    xfs_buf_ioend [xfs]() {
  8)               |      xfs_agi_read_verify [xfs]() {
  8)               |        xfs_agi_verify [xfs]() {
  8)   0.712 us    |          xfs_log_check_lsn [xfs]();
  8)   0.691 us    |          xfs_verify_magic [xfs]();
  8)   3.677 us    |        }
  8)   5.600 us    |      }
  8)   7.865 us    |    }
  8)   9.207 us    |  }
 14)   0.361 us    |                        xfs_buf_rele [xfs]();
 14) + 51.425 us   |                      }
 14) + 57.456 us   |                    }
 14) + 58.068 us   |                  }
 14)   0.311 us    |                  xfs_buf_set_ref [xfs]();
 14) + 59.289 us   |                }
 14) + 59.921 us   |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   0.361 us    |                xfs_buf_unlock [xfs]();
 14)   0.471 us    |                xfs_buf_rele [xfs]();
 14)   1.694 us    |              }
 14)   0.331 us    |              xfs_btree_calc_size [xfs]();
 14) + 63.146 us   |            }
 14)               |            __xfs_ag_resv_init [xfs]() {
 14)   0.431 us    |              xfs_mod_freecounter [xfs]();
 14)   1.022 us    |            }
 14)   0.311 us    |            xfs_rmapbt_calc_reserves [xfs]();
 14)               |            __xfs_ag_resv_init [xfs]() {
 14)   0.371 us    |              xfs_mod_freecounter [xfs]();
 14)   0.962 us    |            }
 14)               |            xfs_alloc_read_agf [xfs]() {
 14)               |              xfs_read_agf [xfs]() {
 14)               |                xfs_trans_read_buf_map [xfs]() {
 14)               |                  xfs_buf_read_map [xfs]() {
 14)               |                    xfs_buf_get_map [xfs]() {
 14)   0.380 us    |                      xfs_perag_get [xfs]();
 14)               |                      xfs_buf_find_lock [xfs]() {
 14)   0.371 us    |                        xfs_buf_lock [xfs]();
 14)   0.952 us    |                      }
 14)   0.321 us    |                      xfs_perag_put [xfs]();
 14)   2.875 us    |                    }
 14)   3.507 us    |                  }
 14)   4.117 us    |                }
 14)   0.310 us    |                xfs_buf_set_ref [xfs]();
 14)   5.330 us    |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   0.351 us    |                xfs_buf_unlock [xfs]();
 14)   0.351 us    |                xfs_buf_rele [xfs]();
 14)   1.572 us    |              }
 14)   7.774 us    |            }
 14) ! 343.664 us  |          }
 14)   0.311 us    |          xfs_perag_put [xfs]();
 14)   0.361 us    |          xfs_perag_get [xfs]();
 14)               |          xfs_ag_resv_init [xfs]() {
 14)               |            xfs_refcountbt_calc_reserves [xfs]() {
 14)               |              xfs_alloc_read_agf [xfs]() {
 14)               |                xfs_read_agf [xfs]() {
 14)               |                  xfs_trans_read_buf_map [xfs]() {
 14)               |                    xfs_buf_read_map [xfs]() {
 14)               |                      xfs_buf_get_map [xfs]() {
 14)   0.361 us    |                        xfs_perag_get [xfs]();
 14)               |                        xfs_buf_find_insert.constprop.0 [xfs]() {
 14)   0.852 us    |                          _xfs_buf_alloc [xfs]();
 14)   0.471 us    |                          kmem_alloc [xfs]();
 14)   2.245 us    |                        }
 14)   4.418 us    |                      }
 14)               |                      __xfs_buf_submit [xfs]() {
 14)   0.311 us    |                        xfs_buf_hold [xfs]();
 14)               |                        _xfs_buf_ioapply [xfs]() {
 14)   4.077 us    |                          xfs_buf_ioapply_map [xfs]();
 14)   4.719 us    |                        }
 14) + 74.117 us   |                        xfs_buf_iowait [xfs]();
 ------------------------------------------
  8) kworker-349921 =>    zvol-905   
 ------------------------------------------

  8)   5.480 us    |  xfs_buf_bio_end_io [xfs]();
 ------------------------------------------
  8)    zvol-905    => kworker-349921
 ------------------------------------------

  8)               |  xfs_buf_ioend_work [xfs]() {
  8)               |    xfs_buf_ioend [xfs]() {
  8)               |      xfs_agf_read_verify [xfs]() {
  8)               |        xfs_agf_verify [xfs]() {
  8)   0.882 us    |          xfs_log_check_lsn [xfs]();
  8)   0.701 us    |          xfs_verify_magic [xfs]();
  8)   4.268 us    |        }
  8)   6.452 us    |      }
  8)   9.378 us    |    }
  8) + 10.800 us   |  }
 14)   0.341 us    |                        xfs_buf_rele [xfs]();
 14) + 80.949 us   |                      }
 14) + 86.270 us   |                    }
 14) + 86.870 us   |                  }
 14)   0.320 us    |                  xfs_buf_set_ref [xfs]();
 14) + 88.083 us   |                }
 14) + 88.684 us   |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   0.361 us    |                xfs_buf_unlock [xfs]();
 14)   0.401 us    |                xfs_buf_rele [xfs]();
 14)   2.044 us    |              }
 14)   0.331 us    |              xfs_btree_calc_size [xfs]();
 14) + 92.220 us   |            }
 14)               |            xfs_finobt_calc_reserves [xfs]() {
 14)               |              xfs_ialloc_read_agi [xfs]() {
 14)               |                xfs_read_agi [xfs]() {
 14)               |                  xfs_trans_read_buf_map [xfs]() {
 14)               |                    xfs_buf_read_map [xfs]() {
 14)               |                      xfs_buf_get_map [xfs]() {
 14)   0.371 us    |                        xfs_perag_get [xfs]();
 14)               |                        xfs_buf_find_insert.constprop.0 [xfs]() {
 14)   0.641 us    |                          _xfs_buf_alloc [xfs]();
 14)   0.811 us    |                          kmem_alloc [xfs]();
 14)   2.384 us    |                        }
 14)   3.697 us    |                      }
 14)               |                      __xfs_buf_submit [xfs]() {
 14)   0.320 us    |                        xfs_buf_hold [xfs]();
 14)               |                        _xfs_buf_ioapply [xfs]() {
 14)   2.705 us    |                          xfs_buf_ioapply_map [xfs]();
 14)   3.386 us    |                        }
 14) + 40.865 us   |                        xfs_buf_iowait [xfs]();
 ------------------------------------------
  8) kworker-349921 =>    zvol-905   
 ------------------------------------------

  8)   3.366 us    |  xfs_buf_bio_end_io [xfs]();
 ------------------------------------------
  8)    zvol-905    => kworker-349921
 ------------------------------------------

  8)               |  xfs_buf_ioend_work [xfs]() {
  8)               |    xfs_buf_ioend [xfs]() {
  8)               |      xfs_agi_read_verify [xfs]() {
  8)               |        xfs_agi_verify [xfs]() {
  8)   0.722 us    |          xfs_log_check_lsn [xfs]();
  8)   0.701 us    |          xfs_verify_magic [xfs]();
  8)   3.717 us    |        }
  8)   5.690 us    |      }
  8)   7.945 us    |    }
  8)   9.327 us    |  }
 14)   0.360 us    |                        xfs_buf_rele [xfs]();
 14) + 46.376 us   |                      }
 14) + 50.964 us   |                    }
 14) + 51.575 us   |                  }
 14)   0.321 us    |                  xfs_buf_set_ref [xfs]();
 14) + 52.777 us   |                }
 14) + 53.389 us   |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   0.351 us    |                xfs_buf_unlock [xfs]();
 14)   0.401 us    |                xfs_buf_rele [xfs]();
 14)   1.603 us    |              }
 14)   0.331 us    |              xfs_btree_calc_size [xfs]();
 14) + 56.504 us   |            }
 14)               |            __xfs_ag_resv_init [xfs]() {
 14)   0.361 us    |              xfs_mod_freecounter [xfs]();
 14)   0.961 us    |            }
 14)   0.320 us    |            xfs_rmapbt_calc_reserves [xfs]();
 14)               |            __xfs_ag_resv_init [xfs]() {
 14)   0.360 us    |              xfs_mod_freecounter [xfs]();
 14)   0.962 us    |            }
 14)               |            xfs_alloc_read_agf [xfs]() {
 14)               |              xfs_read_agf [xfs]() {
 14)               |                xfs_trans_read_buf_map [xfs]() {
 14)               |                  xfs_buf_read_map [xfs]() {
 14)               |                    xfs_buf_get_map [xfs]() {
 14)   0.371 us    |                      xfs_perag_get [xfs]();
 14)               |                      xfs_buf_find_lock [xfs]() {
 14)   0.361 us    |                        xfs_buf_lock [xfs]();
 14)   0.951 us    |                      }
 14)   0.310 us    |                      xfs_perag_put [xfs]();
 14)   2.905 us    |                    }
 14)   3.517 us    |                  }
 14)   4.138 us    |                }
 14)   0.310 us    |                xfs_buf_set_ref [xfs]();
 14)   5.320 us    |              }
 14)               |              xfs_trans_brelse [xfs]() {
 14)   0.360 us    |                xfs_buf_unlock [xfs]();
 14)   0.361 us    |                xfs_buf_rele [xfs]();
 14)   1.573 us    |              }
 14)   7.785 us    |            }
 14) ! 160.807 us  |          }
 14)   0.310 us    |          xfs_perag_put [xfs]();
 14) ! 800.428 us  |        }
 14) # 9789.919 us |      }
 14) * 10720.07 us |    }
 14) * 12896.33 us |  }
 14)   0.471 us    |  xfs_fs_free [xfs]();
  2)   3.667 us    |  xfs_fs_show_options [xfs]();
  7)   4.088 us    |  xfs_fs_show_options [xfs]();
 ------------------------------------------
  8) kworker-349921 =>   systemd-1   
 ------------------------------------------

  8)   2.845 us    |  xfs_fs_show_options [xfs]();
  6)   3.075 us    |  xfs_fs_show_options [xfs]();
 ------------------------------------------
 12) kworker-432959 =>  gvfs-ud-2241 
 ------------------------------------------

 12)   4.739 us    |  xfs_fs_show_options [xfs]();
  7)   2.524 us    |  xfs_fs_show_options [xfs]();
  7)   1.933 us    |  xfs_fs_show_options [xfs]();
 12) + 15.979 us   |  xfs_fs_show_options [xfs]();
 12)   2.284 us    |  xfs_fs_show_options [xfs]();
 12)   2.675 us    |  xfs_fs_show_options [xfs]();
 12)   2.164 us    |  xfs_fs_show_options [xfs]();
 ------------------------------------------
  6)  tracker-2032  =>  systemd-1862 
 ------------------------------------------

  6)   3.858 us    |  xfs_fs_show_options [xfs]();
 12)   2.935 us    |  xfs_fs_show_options [xfs]();
  8)   1.773 us    |  xfs_fs_show_options [xfs]();
 12)   4.218 us    |  xfs_fs_show_options [xfs]();
 12)   3.797 us    |  xfs_fs_show_options [xfs]();
 12)   3.155 us    |  xfs_fs_show_options [xfs]();
 12)   2.375 us    |  xfs_fs_show_options [xfs]();
 12)   6.522 us    |  xfs_fs_show_options [xfs]();
 12)   9.558 us    |  xfs_fs_show_options [xfs]();
 12)   3.196 us    |  xfs_fs_show_options [xfs]();
 12)   2.464 us    |  xfs_fs_show_options [xfs]();
 12)   1.864 us    |  xfs_fs_show_options [xfs]();
 12)   1.113 us    |  xfs_fs_show_options [xfs]();
 12)   1.072 us    |  xfs_fs_show_options [xfs]();
 12)   1.042 us    |  xfs_fs_show_options [xfs]();
 12)   1.062 us    |  xfs_fs_show_options [xfs]();
 12)   1.664 us    |  xfs_fs_show_options [xfs]();
 12)   1.382 us    |  xfs_fs_show_options [xfs]();
 12)   1.152 us    |  xfs_fs_show_options [xfs]();
 12)   2.154 us    |  xfs_fs_show_options [xfs]();
 12)   2.865 us    |  xfs_fs_show_options [xfs]();
 12)   1.753 us    |  xfs_fs_show_options [xfs]();
 12)   1.193 us    |  xfs_fs_show_options [xfs]();
 12)   1.032 us    |  xfs_fs_show_options [xfs]();
 12)   1.042 us    |  xfs_fs_show_options [xfs]();
 12)   1.012 us    |  xfs_fs_show_options [xfs]();
 12)   1.022 us    |  xfs_fs_show_options [xfs]();
 12)   1.011 us    |  xfs_fs_show_options [xfs]();
 ------------------------------------------
  0)   kworker-18   =>  pool-439277  
 ------------------------------------------

  0)               |  xfs_vn_lookup [xfs]() {
 ------------------------------------------
  6)  systemd-1862  =>  pool-439276  
 ------------------------------------------

  6)               |  xfs_vn_lookup [xfs]() {
  0)               |    xfs_lookup [xfs]() {
  6)               |    xfs_lookup [xfs]() {
  0)               |      xfs_dir_lookup [xfs]() {
  6)               |      xfs_dir_lookup [xfs]() {
  0)   0.991 us    |        kmem_alloc [xfs]();
  6)   1.012 us    |        kmem_alloc [xfs]();
  0)               |        xfs_dir2_hashname [xfs]() {
  6)               |        xfs_dir2_hashname [xfs]() {
  0)   0.691 us    |          xfs_da_hashname [xfs]();
  6)   0.681 us    |          xfs_da_hashname [xfs]();
  0)   2.134 us    |        }
  6)   2.144 us    |        }
  0)               |        xfs_ilock_data_map_shared [xfs]() {
  6)               |        xfs_ilock_data_map_shared [xfs]() {
  0)   0.721 us    |          xfs_ilock [xfs]();
  6)   0.722 us    |          xfs_ilock [xfs]();
  0)   2.044 us    |        }
  6)   2.044 us    |        }
  0)               |        xfs_dir2_sf_lookup [xfs]() {
  6)               |        xfs_dir2_sf_lookup [xfs]() {
  0)               |          xfs_dir2_compname [xfs]() {
  6)               |          xfs_dir2_compname [xfs]() {
  0)   0.651 us    |            xfs_da_compname [xfs]();
  6)   0.651 us    |            xfs_da_compname [xfs]();
  0)   1.903 us    |          }
  6)   2.024 us    |          }
  0)   3.446 us    |        }
  6)   3.536 us    |        }
  0)   0.721 us    |        xfs_iunlock [xfs]();
  6)   0.722 us    |        xfs_iunlock [xfs]();
  0) + 14.788 us   |      }
  6) + 14.717 us   |      }
  0) + 16.260 us   |    }
  6) + 16.109 us   |    }
  0) + 21.570 us   |  }
  6) + 19.747 us   |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.450 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.331 us    |      xfs_ilock [xfs]();
 12)   0.951 us    |    }
 12)   0.320 us    |    xfs_iunlock [xfs]();
 12)   3.927 us    |  }
 12)   0.460 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.301 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.301 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.351 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.291 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   4.578 us    |      }
 12)   5.460 us    |    }
 12)   6.271 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.300 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.852 us    |    }
 12)   1.413 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.862 us    |    }
 12)   1.423 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.311 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.310 us    |      xfs_ilock [xfs]();
 12)   0.862 us    |    }
 12)   0.301 us    |    xfs_iunlock [xfs]();
 12)   2.595 us    |  }
 12)   0.330 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.291 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.280 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.320 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.401 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.361 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.707 us    |      }
 12)   4.338 us    |    }
 12)   4.999 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.381 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   1.072 us    |    }
 12)   1.763 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.331 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.901 us    |    }
 12)   1.483 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.301 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.310 us    |      xfs_ilock [xfs]();
 12)   0.862 us    |    }
 12)   0.301 us    |    xfs_iunlock [xfs]();
 12)   2.584 us    |  }
 12)   0.320 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.301 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.291 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.291 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.311 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.266 us    |      }
 12)   3.827 us    |    }
 12)   4.409 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.852 us    |    }
 12)   1.403 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.851 us    |    }
 12)   1.423 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.301 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.310 us    |      xfs_ilock [xfs]();
 12)   0.862 us    |    }
 12)   0.301 us    |    xfs_iunlock [xfs]();
 12)   2.574 us    |  }
 12)   0.320 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.301 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.330 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.310 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.276 us    |      }
 12)   3.837 us    |    }
 12)   4.408 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.851 us    |    }
 12)   1.403 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.851 us    |    }
 12)   1.433 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.300 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.311 us    |      xfs_ilock [xfs]();
 12)   0.861 us    |    }
 12)   0.301 us    |    xfs_iunlock [xfs]();
 12)   2.565 us    |  }
 12)   0.321 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.291 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.280 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.310 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.296 us    |      }
 12)   3.857 us    |    }
 12)   4.438 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.841 us    |    }
 12)   1.413 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.851 us    |    }
 12)   1.423 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.310 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.311 us    |      xfs_ilock [xfs]();
 12)   0.851 us    |    }
 12)   0.300 us    |    xfs_iunlock [xfs]();
 12)   2.575 us    |  }
 12)   0.311 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.290 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.291 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.281 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.321 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.291 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.246 us    |      }
 12)   3.807 us    |    }
 12)   4.368 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.290 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.842 us    |    }
 12)   1.412 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.851 us    |    }
 12)   1.413 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.301 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.311 us    |      xfs_ilock [xfs]();
 12)   0.861 us    |    }
 12)   0.300 us    |    xfs_iunlock [xfs]();
 12)   2.565 us    |  }
 12)   0.321 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.290 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.280 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.310 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.291 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.256 us    |      }
 12)   3.817 us    |    }
 12)   4.388 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.851 us    |    }
 12)   1.413 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.851 us    |    }
 12)   1.433 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.301 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.310 us    |      xfs_ilock [xfs]();
 12)   0.852 us    |    }
 12)   0.301 us    |    xfs_iunlock [xfs]();
 12)   2.574 us    |  }
 12)   0.320 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.290 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.310 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.281 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.236 us    |      }
 12)   3.808 us    |    }
 12)   4.388 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.290 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.852 us    |    }
 12)   1.413 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.861 us    |    }
 12)   1.423 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.300 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.311 us    |      xfs_ilock [xfs]();
 12)   0.861 us    |    }
 12)   0.300 us    |    xfs_iunlock [xfs]();
 12)   2.585 us    |  }
 12)   0.311 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.290 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.321 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.291 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.236 us    |      }
 12)   3.797 us    |    }
 12)   4.368 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.290 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.852 us    |    }
 12)   1.403 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.851 us    |    }
 12)   1.423 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.301 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.311 us    |      xfs_ilock [xfs]();
 12)   0.861 us    |    }
 12)   0.300 us    |    xfs_iunlock [xfs]();
 12)   2.575 us    |  }
 12)   0.321 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.290 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.280 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.320 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.236 us    |      }
 12)   3.797 us    |    }
 12)   4.358 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.851 us    |    }
 12)   1.403 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.851 us    |    }
 12)   1.413 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.311 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.311 us    |      xfs_ilock [xfs]();
 12)   0.851 us    |    }
 12)   0.300 us    |    xfs_iunlock [xfs]();
 12)   2.575 us    |  }
 12)   0.321 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.291 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.281 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.291 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.311 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.281 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.246 us    |      }
 12)   3.807 us    |    }
 12)   4.378 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.290 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.842 us    |    }
 12)   1.412 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.290 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.852 us    |    }
 12)   1.413 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.300 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.310 us    |      xfs_ilock [xfs]();
 12)   0.862 us    |    }
 12)   0.301 us    |    xfs_iunlock [xfs]();
 12)   2.565 us    |  }
 12)   0.321 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.291 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.281 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.291 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.311 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.291 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.246 us    |      }
 12)   3.807 us    |    }
 12)   4.368 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.290 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.842 us    |    }
 12)   1.412 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.290 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.862 us    |    }
 12)   1.422 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.300 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.301 us    |      xfs_ilock [xfs]();
 12)   0.861 us    |    }
 12)   0.300 us    |    xfs_iunlock [xfs]();
 12)   2.565 us    |  }
 12)   0.321 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.290 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.280 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.320 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.291 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.246 us    |      }
 12)   3.807 us    |    }
 12)   4.378 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.280 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.852 us    |    }
 12)   1.413 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.851 us    |    }
 12)   1.413 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.311 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.311 us    |      xfs_ilock [xfs]();
 12)   0.852 us    |    }
 12)   0.300 us    |    xfs_iunlock [xfs]();
 12)   2.575 us    |  }
 12)   0.321 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.291 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.291 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.291 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.311 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.291 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.236 us    |      }
 12)   3.797 us    |    }
 12)   4.378 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.290 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.842 us    |    }
 12)   1.412 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.290 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.852 us    |    }
 12)   1.423 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.310 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.311 us    |      xfs_ilock [xfs]();
 12)   0.861 us    |    }
 12)   0.300 us    |    xfs_iunlock [xfs]();
 12)   2.575 us    |  }
 12)   0.321 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.290 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.310 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.246 us    |      }
 12)   3.807 us    |    }
 12)   4.368 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.851 us    |    }
 12)   1.413 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.851 us    |    }
 12)   1.413 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.301 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.310 us    |      xfs_ilock [xfs]();
 12)   0.862 us    |    }
 12)   0.301 us    |    xfs_iunlock [xfs]();
 12)   2.574 us    |  }
 12)   0.310 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.291 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.291 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.291 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.311 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.280 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.236 us    |      }
 12)   3.797 us    |    }
 12)   4.378 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.842 us    |    }
 12)   1.412 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.290 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.852 us    |    }
 12)   1.422 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.300 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.311 us    |      xfs_ilock [xfs]();
 12)   0.861 us    |    }
 12)   0.300 us    |    xfs_iunlock [xfs]();
 12)   2.575 us    |  }
 12)   0.331 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.301 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.291 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.291 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.311 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.291 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.236 us    |      }
 12)   3.797 us    |    }
 12)   4.368 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.290 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.842 us    |    }
 12)   1.412 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.851 us    |    }
 12)   1.423 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.310 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.311 us    |      xfs_ilock [xfs]();
 12)   0.851 us    |    }
 12)   0.300 us    |    xfs_iunlock [xfs]();
 12)   2.575 us    |  }
 12)   0.331 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.280 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.310 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.280 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.236 us    |      }
 12)   3.797 us    |    }
 12)   4.368 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.851 us    |    }
 12)   1.413 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.852 us    |    }
 12)   1.422 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.311 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.311 us    |      xfs_ilock [xfs]();
 12)   0.851 us    |    }
 12)   0.300 us    |    xfs_iunlock [xfs]();
 12)   2.565 us    |  }
 12)   0.321 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.291 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.291 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.281 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.321 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.291 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.246 us    |      }
 12)   3.807 us    |    }
 12)   4.378 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.290 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.852 us    |    }
 12)   1.402 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.851 us    |    }
 12)   1.423 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.311 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.311 us    |      xfs_ilock [xfs]();
 12)   0.862 us    |    }
 12)   0.311 us    |    xfs_iunlock [xfs]();
 12)   2.574 us    |  }
 12)   0.320 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.290 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.291 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.281 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.311 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.281 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.236 us    |      }
 12)   3.797 us    |    }
 12)   4.378 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.290 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.852 us    |    }
 12)   1.402 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.851 us    |    }
 12)   1.423 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.301 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.301 us    |      xfs_ilock [xfs]();
 12)   0.861 us    |    }
 12)   0.300 us    |    xfs_iunlock [xfs]();
 12)   2.575 us    |  }
 12)   0.321 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.290 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.310 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.236 us    |      }
 12)   3.807 us    |    }
 12)   4.378 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.851 us    |    }
 12)   1.413 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.851 us    |    }
 12)   1.423 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.301 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.311 us    |      xfs_ilock [xfs]();
 12)   0.862 us    |    }
 12)   0.300 us    |    xfs_iunlock [xfs]();
 12)   2.565 us    |  }
 12)   0.321 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.291 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.280 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.310 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.246 us    |      }
 12)   3.807 us    |    }
 12)   4.368 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.851 us    |    }
 12)   1.413 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.852 us    |    }
 12)   1.423 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.300 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.310 us    |      xfs_ilock [xfs]();
 12)   0.862 us    |    }
 12)   0.301 us    |    xfs_iunlock [xfs]();
 12)   2.575 us    |  }
 12)   0.331 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.290 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.310 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.281 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.246 us    |      }
 12)   3.807 us    |    }
 12)   4.388 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.290 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.852 us    |    }
 12)   1.413 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.851 us    |    }
 12)   1.423 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.311 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.310 us    |      xfs_ilock [xfs]();
 12)   0.862 us    |    }
 12)   0.301 us    |    xfs_iunlock [xfs]();
 12)   2.575 us    |  }
 12)   0.320 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.291 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.291 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.281 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.311 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.236 us    |      }
 12)   3.807 us    |    }
 12)   4.379 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.842 us    |    }
 12)   1.413 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.290 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.852 us    |    }
 12)   1.422 us    |  }
 12)               |  xfs_dir_open [xfs]() {
 12)   0.311 us    |    xfs_file_open [xfs]();
 12)               |    xfs_ilock_data_map_shared [xfs]() {
 12)   0.311 us    |      xfs_ilock [xfs]();
 12)   0.861 us    |    }
 12)   0.310 us    |    xfs_iunlock [xfs]();
 12)   2.575 us    |  }
 12)   0.321 us    |  xfs_vn_getattr [xfs]();
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
 12)   0.290 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
 12)   0.290 us    |        xfs_dir2_sf_get_ino [xfs]();
 12)   0.280 us    |        xfs_dir2_sf_get_ftype [xfs]();
 12)   0.310 us    |        xfs_dir2_namecheck [xfs]();
 12)   0.280 us    |        xfs_dir2_sf_nextentry [xfs]();
 12)   3.246 us    |      }
 12)   3.807 us    |    }
 12)   4.378 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.301 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.851 us    |    }
 12)   1.413 us    |  }
 12)               |  xfs_file_readdir [xfs]() {
 12)               |    xfs_readdir [xfs]() {
 12)   0.291 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
 12)   0.861 us    |    }
 12)   1.423 us    |  }
  6)               |  xfs_dir_open [xfs]() {
  6)   0.391 us    |    xfs_file_open [xfs]();
  6)               |    xfs_ilock_data_map_shared [xfs]() {
  6)   0.341 us    |      xfs_ilock [xfs]();
  6)   0.901 us    |    }
  6)   0.320 us    |    xfs_iunlock [xfs]();
  6)   3.196 us    |  }
  6)   0.511 us    |  xfs_vn_getattr [xfs]();
  0)               |  xfs_file_readdir [xfs]() {
  0)               |    xfs_readdir [xfs]() {
  0)               |      xfs_dir2_sf_getdents.isra.0 [xfs]() {
  0)   0.671 us    |        xfs_dir2_sf_get_parent_ino [xfs]();
  0)   0.671 us    |        xfs_dir2_sf_get_ino [xfs]();
  0)   0.661 us    |        xfs_dir2_sf_get_ftype [xfs]();
  0)   0.772 us    |        xfs_dir2_namecheck [xfs]();
  0)   0.652 us    |        xfs_dir2_sf_nextentry [xfs]();
  0)   8.506 us    |      }
  0)   9.929 us    |    }
  0) + 11.632 us   |  }
  0)               |  xfs_file_readdir [xfs]() {
  0)               |    xfs_readdir [xfs]() {
  0)   0.671 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
  0)   1.924 us    |    }
  0)   3.206 us    |  }
  0)               |  xfs_file_readdir [xfs]() {
  0)               |    xfs_readdir [xfs]() {
  0)   0.651 us    |      xfs_dir2_sf_getdents.isra.0 [xfs]();
  0)   1.913 us    |    }
  0)   3.236 us    |  }
 ------------------------------------------
 14) pool-ud-439256 =>    <idle>-0   
 ------------------------------------------

 14)   4.739 us    |  xfs_buf_free_callback [xfs]();
 14)   0.782 us    |  xfs_buf_free_callback [xfs]();
.....
 12)   6.101 us    |  xfs_fs_show_options [xfs]();
 12)   1.142 us    |  xfs_fs_show_options [xfs]();
.....
 15)   8.556 us    |  xfs_vn_getattr [xfs]();
 ------------------------------------------
  2)  systemd-1862  => findmnt-439283
 ------------------------------------------

  2)   3.667 us    |  xfs_fs_show_options [xfs]();
  9)   5.310 us    |  xfs_fs_show_options [xfs]();
  9)   0.972 us    |  xfs_vn_getattr [xfs]();
  9)               |  xfs_fs_statfs [xfs]() {
  9)               |    xfs_inodegc_push [xfs]() {
  9)   1.723 us    |      xfs_inodegc_queue_all [xfs]();
  9)   3.206 us    |    }
  9)   7.224 us    |  }
  9)   0.701 us    |  xfs_vn_getattr [xfs]();
  9)               |  xfs_dir_open [xfs]() {
  9)   0.812 us    |    xfs_file_open [xfs]();
  9)               |    xfs_ilock_data_map_shared [xfs]() {
  9)   0.721 us    |      xfs_ilock [xfs]();
  9)   2.084 us    |    }
  9)   0.711 us    |    xfs_iunlock [xfs]();
  9)   6.543 us    |  }
  9)               |  xfs_file_ioctl [xfs]() {
  9)               |    xfs_ioc_fsgeometry [xfs]() {
  9)   0.771 us    |      xfs_fs_geometry [xfs]();
  9)   0.931 us    |      xfs_fsop_geom_health [xfs]();
  9)   4.218 us    |    }
  9)   5.731 us    |  }
  9)   0.741 us    |  xfs_vn_getattr [xfs]();
  9)               |  xfs_fs_statfs [xfs]() {
  9)               |    xfs_inodegc_push [xfs]() {
  9)   1.052 us    |      xfs_inodegc_queue_all [xfs]();
  9)   2.305 us    |    }
  9)   5.059 us    |  }
  9)   0.712 us    |  xfs_vn_getattr [xfs]();
 ------------------------------------------
 10)  <...>-359062  =>  udisksd-1559 
 ------------------------------------------

 10)   5.099 us    |  xfs_fs_show_options [xfs]();
 10)   2.605 us    |  xfs_fs_show_options [xfs]();
.....
 ------------------------------------------
 14)    <idle>-0    =>  gvfs-ud-2241 
 ------------------------------------------

 14)   5.139 us    |  xfs_fs_show_options [xfs]();
 14)   8.837 us    |  xfs_fs_show_options [xfs]();
 14)   2.595 us    |  xfs_fs_show_options [xfs]();
 14)   2.535 us    |  xfs_fs_show_options [xfs]();
 13)   1.272 us    |  xfs_vn_getattr [xfs]();
 14)   2.294 us    |  xfs_fs_show_options [xfs]();
 14)   2.685 us    |  xfs_fs_show_options [xfs]();
 14)   2.465 us    |  xfs_fs_show_options [xfs]();
 14)   2.284 us    |  xfs_fs_show_options [xfs]();
 14)   2.655 us    |  xfs_fs_show_options [xfs]();
 14)   2.444 us    |  xfs_fs_show_options [xfs]();
 ------------------------------------------
  0)  pool-439277   => findmnt-439294
 ------------------------------------------

  0)   5.000 us    |  xfs_fs_show_options [xfs]();
 14)   4.088 us    |  xfs_fs_show_options [xfs]();
 14)   3.606 us    |  xfs_fs_show_options [xfs]();
 ------------------------------------------
  2) findmnt-439283 => xfs_spa-439295
 ------------------------------------------

  2)   3.457 us    |  xfs_fs_show_options [xfs]();
  2)   0.470 us    |  xfs_vn_getattr [xfs]();
  2)               |  xfs_fs_statfs [xfs]() {
  2)               |    xfs_inodegc_push [xfs]() {
  2)   0.952 us    |      xfs_inodegc_queue_all [xfs]();
  2)   1.753 us    |    }
  2)   3.977 us    |  }
  2)   0.311 us    |  xfs_vn_getattr [xfs]();
  2)               |  xfs_dir_open [xfs]() {
  2)   0.330 us    |    xfs_file_open [xfs]();
  2)               |    xfs_ilock_data_map_shared [xfs]() {
  2)   0.320 us    |      xfs_ilock [xfs]();
  2)   0.972 us    |    }
  2)   0.321 us    |    xfs_iunlock [xfs]();
  2)   3.005 us    |  }
  2)               |  xfs_file_ioctl [xfs]() {
  2)               |    xfs_ioc_fsgeometry [xfs]() {
  2)   0.371 us    |      xfs_fs_geometry [xfs]();
  2)   0.451 us    |      xfs_fsop_geom_health [xfs]();
  2)   2.064 us    |    }
  2)   2.905 us    |  }
  2)   0.330 us    |  xfs_vn_getattr [xfs]();
  2)               |  xfs_fs_statfs [xfs]() {
  2)               |    xfs_inodegc_push [xfs]() {
  2)   0.481 us    |      xfs_inodegc_queue_all [xfs]();
  2)   1.032 us    |    }
  2)   2.254 us    |  }
  2)   0.321 us    |  xfs_vn_getattr [xfs]();
 14)   2.344 us    |  xfs_fs_show_options [xfs]();
 ------------------------------------------
  0) findmnt-439294 =>  udisksd-1559 
 ------------------------------------------

  0)   3.136 us    |  xfs_fs_show_options [xfs]();
  0)   2.655 us    |  xfs_fs_show_options [xfs]();
.....
 ------------------------------------------
  0)  udisksd-1559  => losetup-439299
 ------------------------------------------

  0)   3.036 us    |  xfs_vn_getattr [xfs]();
 14)   2.184 us    |  xfs_fs_show_options [xfs]();
 14)   3.567 us    |  xfs_fs_show_options [xfs]();
 14)   3.406 us    |  xfs_fs_show_options [xfs]();
 14)   3.406 us    |  xfs_fs_show_options [xfs]();
  4)   5.129 us    |  xfs_fs_show_options [xfs]();
 ------------------------------------------
 15) losetup-439281 =>  gvfs-ud-2241 
 ------------------------------------------

 15)   4.578 us    |  xfs_fs_show_options [xfs]();
 15)   3.606 us    |  xfs_fs_show_options [xfs]();
 ------------------------------------------
  2) xfs_spa-439295 => xfs_spa-439302
 ------------------------------------------

  2)   3.036 us    |  xfs_fs_show_options [xfs]();
  2)   0.441 us    |  xfs_vn_getattr [xfs]();
  2)               |  xfs_fs_statfs [xfs]() {
  2)               |    xfs_inodegc_push [xfs]() {
  2)   0.551 us    |      xfs_inodegc_queue_all [xfs]();
  2)   1.353 us    |    }
  2)   3.396 us    |  }
  2)   0.320 us    |  xfs_vn_getattr [xfs]();
  2)               |  xfs_dir_open [xfs]() {
  2)   0.330 us    |    xfs_file_open [xfs]();
  2)               |    xfs_ilock_data_map_shared [xfs]() {
  2)   0.310 us    |      xfs_ilock [xfs]();
  2)   0.952 us    |    }
  2)   0.321 us    |    xfs_iunlock [xfs]();
  2)   3.025 us    |  }
  2)               |  xfs_file_ioctl [xfs]() {
  2)               |    xfs_ioc_fsgeometry [xfs]() {
  2)   0.691 us    |      xfs_fs_geometry [xfs]();
  2)   0.461 us    |      xfs_fsop_geom_health [xfs]();
  2)   2.414 us    |    }
  2)   3.146 us    |  }
  2)   0.331 us    |  xfs_vn_getattr [xfs]();
  2)               |  xfs_fs_statfs [xfs]() {
  2)               |    xfs_inodegc_push [xfs]() {
  2)   0.480 us    |      xfs_inodegc_queue_all [xfs]();
  2)   1.032 us    |    }
  2)   2.264 us    |  }
  2)   0.310 us    |  xfs_vn_getattr [xfs]();
 15)   2.274 us    |  xfs_fs_show_options [xfs]();
 ------------------------------------------
  0) losetup-439299 =>  udisksd-1559 
 ------------------------------------------

  0)   3.156 us    |  xfs_fs_show_options [xfs]();
  3)   1.863 us    |  xfs_fs_show_options [xfs]();
.....
 ------------------------------------------
 14)  gvfs-ud-2241  => kworker-419899
 ------------------------------------------

 14)               |  xfs_log_worker [xfs]() {
 14)   1.062 us    |    xfs_fs_writable [xfs]();
 14)               |    xfs_log_need_covered.isra.0 [xfs]() {
 14)   0.971 us    |      xlog_cil_empty [xfs]();
 14)   3.316 us    |    }
 14)               |    xfs_log_force [xfs]() {
 14)               |      xlog_cil_force_seq [xfs]() {
 14)   2.614 us    |        xlog_cil_push_now.isra.0 [xfs]();
 14)   4.608 us    |      }
 14)   7.423 us    |    }
 14)   1.272 us    |    xfs_ail_push_all [xfs]();
 14) + 23.724 us   |  }
```
