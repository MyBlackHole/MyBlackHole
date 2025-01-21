# timer

定时器的状态信息

```shell
crash> timer
JIFFIES
7902623324

TIMER_BASES[0][BASE_STD]: ffff955b3ec1a940
  EXPIRES        TTE         TIMER_LIST     FUNCTION
  7902623457      133  ffff953c5e61d0d8  ffffffffb8ce7d20  <cursor_timer_handler>
  7902729000   105676  ffff955b3ec15cc0  ffffffffb883c1e0  <mce_timer_fn>
TIMER_BASES[0][BASE_DEF]: ffff955b3ec1bbc0
  EXPIRES        TTE      TIMER_LIST     FUNCTION
  7902624000   676  ffffffffb9b286e0  ffffffffb88c9670  <delayed_work_timer_fn>
  7902624000   676  ffff955b3ec1fb40  ffffffffb88c9670  <delayed_work_timer_fn>

TIMER_BASES[1][BASE_STD]: ffff955b3ec5a940
  EXPIRES        TTE         TIMER_LIST     FUNCTION
  7902618997    -4327  ffff953c997a6ec8  ffffffffb8bfa7b0  <blk_rq_timed_out_timer>
  7902621377    -1947  ffff954cad51b488  ffffffffb8fed450  <tcp_delack_timer>
  7902622265    -1059  ffff953c6e76d108  ffffffffb8fed450  <tcp_delack_timer>
  7902622265    -1059  ffff953c6e9eab08  ffffffffb8fed450  <tcp_delack_timer>
  7902622265    -1059  ffff953c6e76c788  ffffffffb8fed450  <tcp_delack_timer>
  7902622863     -461  ffff9545da366d88  ffffffffb8fed450  <tcp_delack_timer>
  7902622905     -419  ffff953c6d408508  ffffffffb8fed450  <tcp_delack_timer>
  7902631064     7740  ffff955afb290b38  ffffffffc0b40270  <iscsit_handle_nopin_timeout>
  7902728997   105673  ffff955b3ec55cc0  ffffffffb883c1e0  <mce_timer_fn>
TIMER_BASES[1][BASE_DEF]: ffff955b3ec5bbc0
  EXPIRES        TTE       TIMER_LIST     FUNCTION
  7902618997  -4327  ffff955b3ec5fb40  ffffffffb88c9670  <delayed_work_timer_fn>

TIMER_BASES[2][BASE_STD]: ffff955b3ec9a940
  EXPIRES        TTE         TIMER_LIST     FUNCTION
  7902626994     3670  ffff953c6d0ceec8  ffffffffb8bfa7b0  <blk_rq_timed_out_timer>
  7902626994     3670  ffff954472fb1cf8  ffffffffb8bfa7b0  <blk_rq_timed_out_timer>
  7902728994   105670  ffff955b3ec95cc0  ffffffffb883c1e0  <mce_timer_fn>
TIMER_BASES[2][BASE_DEF]: ffff955b3ec9bbc0
  EXPIRES        TTE      TIMER_LIST     FUNCTION
  7902623994   670  ffff955b3ec9fb40  ffffffffb88c9670  <delayed_work_timer_fn>

TIMER_BASES[3][BASE_STD]: ffff955b3ecda940
  EXPIRES        TTE         TIMER_LIST     FUNCTION
  7902627480     4156  ffff9592e857bb38  ffffffffc0b40270  <iscsit_handle_nopin_timeout>
  7902728991   105667  ffff955b3ecd5cc0  ffffffffb883c1e0  <mce_timer_fn>
TIMER_BASES[3][BASE_DEF]: ffff955b3ecdbbc0
  EXPIRES        TTE      TIMER_LIST     FUNCTION
  7902623991   667  ffff955b3ecdfb40  ffffffffb88c9670  <delayed_work_timer_fn>

TIMER_BASES[4][BASE_STD]: ffff955b3ed1a940
  EXPIRES        TTE         TIMER_LIST     FUNCTION
  7902623441      117  ffff9545da366d40  ffffffffb8fee100  <tcp_write_timer>
  7902728988   105664  ffff955b3ed15cc0  ffffffffb883c1e0  <mce_timer_fn>
TIMER_BASES[4][BASE_DEF]: ffff955b3ed1bbc0
  EXPIRES        TTE      TIMER_LIST     FUNCTION
  7902623988   664  ffff955b3ed1fb40  ffffffffb88c9670  <delayed_work_timer_fn>

TIMER_BASES[5][BASE_STD]: ffff955b3ed5a940
  EXPIRES        TTE         TIMER_LIST     FUNCTION
  7902728985   105661  ffff955b3ed55cc0  ffffffffb883c1e0  <mce_timer_fn>
TIMER_BASES[5][BASE_DEF]: ffff955b3ed5bbc0
  EXPIRES        TTE     TIMER_LIST     FUNCTION
  (none)

TIMER_BASES[6][BASE_STD]: ffff955b3ed9a940
  EXPIRES        TTE         TIMER_LIST     FUNCTION
  7902728982   105658  ffff955b3ed95cc0  ffffffffb883c1e0  <mce_timer_fn>
TIMER_BASES[6][BASE_DEF]: ffff955b3ed9bbc0
  EXPIRES        TTE         TIMER_LIST     FUNCTION
```
