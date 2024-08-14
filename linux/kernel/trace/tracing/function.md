# function

- xfs_log_check_lsn
```shell
sysctl kernel.ftrace_enabled=1
dir=/sys/kernel/debug/tracing

<!-- 执行栈 -->
echo function > ${dir}/current_tracer

<!-- 过滤指定函数 --> 
echo xfs_log_check_lsn > ${dir}/set_ftrace_filter 

<!-- 跟踪堆栈 -->
echo 1 > ${dir}/set_ftrace_trace

<!-- 重置 trace -->
echo 0 > ${dir}/trace

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
....
```
