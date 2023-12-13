# hey

压测工具 (有 bug )
[案例](https://blog.csdn.net/lsc_2019/article/details/118550151)

```shell
用法：hey [选项…] <url>

选项：
-n 请求次数。默认为 200。
-c 并发工作线程数。请确保总请求数不小于并发数量。默认为 50。
-q 每个工作线程的查询每秒限制（QPS）。默认不设限制。
-z 应用发送请求的持续时间。达到指定时间后，应用将停止并退出。如果指定了该选项，则忽略-n选项。示例：-z 10s -z 3m。
-o 输出类型。如果未提供，则打印摘要。"csv" 是唯一支持的替代选项。将响应指标以逗号分隔值格式转储。

-m HTTP 方法，GET、POST、PUT、DELETE、HEAD、OPTIONS 之一。
-H 自定义 HTTP 标头。您可以通过重复该标志来指定多个标头。例如，-H "Accept：text/html" -H "Content-Type：application/xml"。
-t 每个请求的超时时间（秒）。默认为 20，使用 0 表示无限期。
-A HTTP Accept 标头。
-d HTTP 请求体。
-D 从文件中读取 HTTP 请求体。例如，/home/user/file.txt 或 ./file.txt。
-T 内容类型，默认为 "text/html"。
-a 基本身份验证，格式为：用户名:密码。
-x HTTP 代理地址，格式为 host:port。
-h2 启用 HTTP/2。

-host HTTP Host 标头。

-disable-compression 禁用压缩。
-disable-keepalive 禁用 keep-alive，防止在不同的 HTTP 请求之间重用 TCP 连接。
-disable-redirects 禁用 HTTP 重定向跟随。
-cpus 使用的 CPU 核心数。
（当前设备的默认值为 8 个核心
```


## 例子
```shell
hey -z 60s http://localhost:8080/good

hey -c 1 -n 10 'http://192.168.60.70:8008/api/v2/sub_task/details?dag_id=goldendb_mount&logical_date=2023-12-13T03:57:41.292867Z&_t=1702440588773' \
  -H 'Authorization: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoxLCJ1c2VybmFtZSI6ImFkbWluIiwiaWF0IjoxNzAyNDA2Nzc1fQ.uK2e7xQrDngVdcAHtw06gYrsW3w7N1vVnQNIyvSm_Hg'

curl 'http://192.168.60.70:8008/api/v2/sub_task/details?dag_id=goldendb_mount&logical_date=2023-12-13T03:57:41.292867Z&_t=1702441985772' \
  -H 'Accept: application/json, text/plain, */*' \
  -H 'Accept-Language: zh-CN,zh;q=0.9,en;q=0.8' \
  -H 'Authorization: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoxLCJ1c2VybmFtZSI6ImFkbWluIiwiaWF0IjoxNzAyNDA2Nzc1fQ.uK2e7xQrDngVdcAHtw06gYrsW3w7N1vVnQNIyvSm_Hg' \
  -H 'Cache-Control: no-cache' \
  -H 'Cookie: session=.eJylUk1LxDAQ_S85l22SadK0sHjx6sWDF5EwSSZtMbZLmioi_nejHrwueBzeJ4_5YDZm2mc2Rkw7NcwugY0MtBOKOjQhRB65NtE7qTx1KkYAkA6HgWstlDQ-OjIBoAsDBj9w3_UmoO4lSWGMUMJ3xnkxaPAmmkFBL0ToCMF1HXgZSUYjKgi9j8IETS4MjtUiF8ovuNJa2FjyUav5PUdbtmdaa0OsNG3A9ZLz3nGUoACUQ811H0ItFFEG4aE6BZzsXrAcu41LKpS_5SlVJG0eE9WzWjbsghPZednLlt_Z-MjmUi5j24pBnoQ2J81PPR8NN7ytjvlY21S57c2P7Ban-2O92wKlh4XezqL6_Ucvr9ZfH2RjKpbb7zmWcMbs5-WV7A7XRz01rOD0N-N6pNSwY6f8-zaCfX4BXE6y8A.ZXgscg.xO3izcPoKkbWp7lJeZ35xuuGCrY' \
  -H 'Pragma: no-cache' \
  -H 'Proxy-Connection: keep-alive' \
  -H 'Referer: http://192.168.60.70:8008/' \
  -H 'User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36 Edg/120.0.0.0' \
  --compressed \
  --insecure

./bin/hey_linux_amd64 -c 1 -n 20 'http://127.0.0.1:8009/api/v2/sub_task/details?dag_id=goldendb_log_backup&logical_date=2023-12-08T09:29:39.939127Z&_t=1702448543773'

curl 'http://192.168.78.212:8008/api/v2/sub_task/details?dag_id=goldendb_log_backup&logical_date=2023-12-08T09:29:39.939127Z&_t=1702448543773' \
  -H 'Accept: application/json, text/plain, */*' \
  -H 'Accept-Language: zh-CN,zh;q=0.9,en;q=0.8' \
  -H 'Authorization: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoxLCJ1c2VybmFtZSI6ImFkbWluIiwiaWF0IjoxNzAyNDE5Mzc0fQ.wZ8xHmiY1uyzBTlh46W0iCF4h_dsjvG8jOZOO9sbp1Q' \
  -H 'Cache-Control: no-cache' \
  -H 'Cookie: _ga=GA1.1.261683572.1690976330; _ga_J69Z2JCTFB=GS1.1.1691743153.4.0.1691743159.54.0.0; ph_phc_hnhlqe6D2Q4IcQNrFItaqdXJAxQ8RcHkPAFAp74pubv_posthog=%7B%22distinct_id%22%3A%220189b60b-22a9-717d-a924-93f926f87836%22%2C%22%24device_id%22%3A%220189b60b-22a9-717d-a924-93f926f87836%22%2C%22%24user_state%22%3A%22anonymous%22%2C%22%24sesid%22%3A%5B1691743159789%2C%220189e3bf-f92b-7069-9f77-a4dd61cfb4c7%22%2C1691743156523%5D%2C%22%24session_recording_enabled_server_side%22%3Afalse%2C%22%24autocapture_disabled_server_side%22%3Afalse%2C%22%24active_feature_flags%22%3A%5B%5D%2C%22%24enabled_feature_flags%22%3A%7B%22user-age-less-than-7d%22%3Afalse%7D%2C%22%24feature_flag_payloads%22%3A%7B%7D%2C%22event_source%22%3A%22cloud%22%2C%22posthog_first_seen_at%22%3A%222023-08-02T11%3A38%3A55.867Z%22%2C%22posthog_first_distinct_id%22%3A%220189b60b-22a9-717d-a924-93f926f87836%22%2C%22%24flag_call_reported%22%3A%7B%22user-age-less-than-7d%22%3A%5B%22false%22%5D%7D%7D; ph_mqkwGT0JNFqO-zX2t0mW6Tec9yooaVu7xCBlXtHnt5Y_posthog=%7B%22distinct_id%22%3A%220189deaa-1246-77b9-8cf5-0b28e08ed392%22%2C%22%24device_id%22%3A%220189deaa-1246-77b9-8cf5-0b28e08ed392%22%2C%22%24user_state%22%3A%22anonymous%22%7D; lang=zh; oAuthLoginInfo=; expire=1695870902547; refreshToken=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJrdWJlc3BoZXJlIiwic3ViIjoiYWRtaW4iLCJleHAiOjE2OTU4NzgxMDIsImlhdCI6MTY5NTg2MzcwMiwidG9rZW5fdHlwZSI6InJlZnJlc2hfdG9rZW4iLCJ1c2VybmFtZSI6ImFkbWluIiwiZXh0cmEiOnsidW5pbml0aWFsaXplZCI6WyJ0cnVlIl19fQ.MoqaW-SFewkbQbBdmtgaXBOrYNN-EsuUdQa44AG3rAs; wordpress_test_cookie=WP+Cookie+check; wp-settings-time-1=1698304474; sid=a102d466a38359bd10726efdf253f460; session=.eJyNUstqHDEQ_BeBfVrvtqTRa8CY5JZDbiaXbBCSurU7WNYsM5qEEPLv0WKH5GR8Eai7uqr68Yv5vNB6ZmMOZaUd8xOykWVtnRiyUVlmJSwHIwl1pDwoTElF1EKaITs3cMMBuLXDAIIcGIsagkY7RBzAREIEJVyGbLkhECmn6KIWmkvgDkChcjyZEKPNFKzTSgueE-tGLrQ8h0q1sbEtW7eW1iX7Nj9R7Q51xCikitJqAQYcCKUoSslJYZdCI3IeIGJnwnDyawttW32eSqOll4dSeqbMKRTq3065Y5dwIn-e1jYvP9n4lZ1bu4yHA3diz7XdG7sXXIwWLBxaWJ-m2klrokPpJYcHn0vz0l_FJrw_zQWpYvTP81bb7WvyWnbNHjctQRw3mw3GqZb51COK1HFTGc2tn5N_7NBPrwqfZ6TyZaIf9wKEvAN3x-0j8Bv5Qcnro_euL0GqG_ERoAcAejdvuO8el62--H4bmeZaKbVpfhf6_bz_I18mB38nF5Z0nr6TX6Xvh9l3QezbjrVw-re9upWyY9tKy8u1cvb7D9_K1mE.ZXgaWA.Y3p5ozgMdE4rIdV5uJoLb3C4b_s' \
  -H 'Pragma: no-cache' \
  -H 'Proxy-Connection: keep-alive' \
  -H 'Referer: http://192.168.78.212:8008/' \
  -H 'User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36 Edg/120.0.0.0' \
  --compressed \
  --insecure

curl 'http://192.168.60.70:8008/api/v2/sub_task/details?dag_id=goldendb_log_backup&logical_date=2023-12-13T06:54:56.229696Z&_t=1702451057150' \
  -H 'Accept: application/json, text/plain, */*' \
  -H 'Accept-Language: zh-CN,zh;q=0.9,en;q=0.8' \
  -H 'Authorization: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoxLCJ1c2VybmFtZSI6ImFkbWluIiwiaWF0IjoxNzAyNDA2Nzc1fQ.uK2e7xQrDngVdcAHtw06gYrsW3w7N1vVnQNIyvSm_Hg' \
  -H 'Cache-Control: no-cache' \
  -H 'Cookie: session=.eJylUk1LxDAQ_S85l22SadK0sHjx6sWDF5EwSSZtMbZLmioi_nejHrwueBzeJ4_5YDZm2mc2Rkw7NcwugY0MtBOKOjQhRB65NtE7qTx1KkYAkA6HgWstlDQ-OjIBoAsDBj9w3_UmoO4lSWGMUMJ3xnkxaPAmmkFBL0ToCMF1HXgZSUYjKgi9j8IETS4MjtUiF8ovuNJa2FjyUav5PUdbtmdaa0OsNG3A9ZLz3nGUoACUQ811H0ItFFEG4aE6BZzsXrAcu41LKpS_5SlVJG0eE9WzWjbsghPZednLlt_Z-MjmUi5j24pBnoQ2J81PPR8NN7ytjvlY21S57c2P7Ban-2O92wKlh4XezqL6_Ucvr9ZfH2RjKpbb7zmWcMbs5-WV7A7XRz01rOD0N-N6pNSwY6f8-zaCfX4BXE6y8A.ZXgscg.xO3izcPoKkbWp7lJeZ35xuuGCrY' \
  -H 'Pragma: no-cache' \
  -H 'Proxy-Connection: keep-alive' \
  -H 'Referer: http://192.168.60.70:8008/' \
  -H 'User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36 Edg/120.0.0.0' \
  --compressed \
  --insecure

./bin/hey_linux_amd64 -c 1 -n 1 'http://127.0.0.1:8009/api/v2/sub_task/details?dag_id=goldendb_log_backup&logical_date=2023-12-13T06:54:56.229696Z&_t=1702451057150'
```
