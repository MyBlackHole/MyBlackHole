# append

```shell
HEAD /wdg1%2Fossutil HTTP/1.1
User-Agent: aliyun-sdk-c/3.9.2(Compatible Unknown)
Host: 127.0.0.1
Accept: */*
Date: Sun, 05 Nov 2023 03:12:03 GMT
Authorization: OSS 123:LyCOsTlDav1d/R9BYwtDPcbINqs=

HTTP/1.1 404 Not Found
Server: AliyunOSS
X-Oss-Request-Id: 0A75D59C2AB870151CFCE535
Content-Type: application/xml
Server: WEBrick/1.8.1 (Ruby/3.1.2/2022-04-12) OpenSSL/3.0.8
Date: Sun, 05 Nov 2023 03:12:03 GMT
Content-Length: 273
Connection: Keep-Alive

POST /wdg1%2Fossutil?append&position=0 HTTP/1.1
User-Agent: aliyun-sdk-c/3.9.2(Compatible Unknown)
Host: 127.0.0.1
Accept: */*
Content-Length: 14
Content-Type: application/octet-stream
Date: Sun, 05 Nov 2023 03:12:03 GMT
Authorization: OSS 123:Xx/7xFjihhst6Gm2gicLI3Y1L+U=

test oss c sdk

HTTP/1.1 200 OK
X-Oss-Request-Id: ABA92BA8573C1F137DFAFEAB
Server: AliyunOSS
X-Oss-Next-Append-Position: 14
Date: Sun, 05 Nov 2023 03:12:03 GMT
Content-Length: 0
Connection: Keep-Alive

HEAD /wdg1%2Fossutil HTTP/1.1
User-Agent: aliyun-sdk-c/3.9.2(Compatible Unknown)
Host: 127.0.0.1
Accept: */*
Date: Sun, 05 Nov 2023 03:12:03 GMT
Authorization: OSS 123:LyCOsTlDav1d/R9BYwtDPcbINqs=

HTTP/1.1 200 OK
X-Oss-Request-Id: B31C96E774C822C6B982ADB9
Server: AliyunOSS
Accept-Ranges: bytes
Etag: 303f78f3c81f9ec34fda44fe0cfb4066
Content-Length: 14
Last-Modified: Sun, 05 Nov 2023 03:12:03 GMT
X-Oss-Object-Type: Appendable
X-Oss-Storage-Class: Standard
X-Oss-Server-Time: Sun, 05 Nov 2023 11:12:03 GMT
X-Oss-Next-Append-Position: 14
Date: Sun, 05 Nov 2023 03:12:03 GMT
Connection: Keep-Alive

POST /wdg1%2Fossutil?append&position=14 HTTP/1.1
User-Agent: aliyun-sdk-c/3.9.2(Compatible Unknown)
Host: 127.0.0.1
Accept: */*
Content-Length: 3999
Content-Type: application/octet-stream
Date: Sun, 05 Nov 2023 03:12:03 GMT
Authorization: OSS 123:NBw4Tjyhab/LpSCCTkm3sJmhobw=

#include "aos_log.h"
#include "aos_util.h"
#include "aos_string.h"
#include "aos_status.h"
#include "oss_auth.h"
#include "oss_util.h"
#include "oss_api.h"
#include "oss_config.h"
#include "oss_sample_util.h"

void append_object_from_buffer()
{
    aos_pool_t *p = NULL;
    aos_string_t bucket;
    aos_string_t object;
    char *str = "test oss c sdk";
    aos_status_t *s = NULL;
    int is_cname = 0;
    int64_t position = 0;
    aos_table_t *headers1 = NULL;
    aos_table_t *headers2 = NULL;
    aos_table_t *resp_headers = NULL;
    oss_request_options_t *options = NULL;
    aos_list_t buffer;
    aos_buf_t *content = NULL;
    char *next_append_position = NULL;
    char *object_type = NULL;

    aos_pool_create(&p, NULL);
    options = oss_request_options_create(p);
    init_sample_request_options(options, is_cname);
    headers1 = aos_table_make(p, 0);
    aos_str_set(&bucket, BUCKET_NAME);
    aos_str_set(&object, OBJECT_NAME);

    oss_delete_object(options, &bucket, &object, NULL);
    s = oss_head_object(options, &bucket, &object, headers1, &resp_headers);
    if (aos_status_is_ok(s)) {
        object_type = (char*)(apr_table_get(resp_headers, OSS_OBJECT_TYPE));
        if (0 != strncmp(OSS_OBJECT_TYPE_APPENDABLE, object_type, 
                         strlen(OSS_OBJECT_TYPE_APPENDABLE))) 
        {
            printf("object[%s]'s type[%s] is not Appendable\n", OBJECT_NAME, object_type);
            aos_pool_destroy(p);
            return;
        }

        next_append_position = (char*)(apr_table_get(resp_headers, OSS_NEXT_APPEND_POSITION));
        position = aos_atoi64(next_append_position);
    }
        
    headers2 = aos_table_make(p, 0);
    aos_list_init(&buffer);
    content = aos_buf_pack(p, str, strlen(str));
    aos_list_add_tail(&content->node, &buffer);
    s = oss_append_object_from_buffer(options, &bucket, &object, 
            position, &buffer, headers2, &resp_headers);

    if (aos_status_is_ok(s))
    {
        printf("append object from buffer succeeded\n");
    } else {
        printf("append object from buffer failed\n");
    }

    aos_pool_destroy(p);
}

void append_object_from_file()
{
    aos_pool_t *p = NULL;
    aos_string_t bucket;
    aos_string_t object;
    int is_cname = 0;
    aos_table_t *headers1 = NULL;
    aos_table_t *headers2 = NULL;
    aos_table_t *resp_headers = NULL;
    oss_request_options_t *options = NULL;
    char *filename = __FILE__;
    aos_status_t *s = NULL;
    aos_string_t file;
    int64_t position = 0;
    char *next_append_position = NULL;
    char *object_type = NULL;

    aos_pool_create(&p, NULL);
    options = oss_request_options_create(p);
    init_sample_request_options(options, is_cname);
    headers1 = aos_table_make(options->pool, 0);
    headers2 = aos_table_make(options->pool, 0);
    aos_str_set(&bucket, BUCKET_NAME);
    aos_str_set(&object, OBJECT_NAME);
    aos_str_set(&file, filename);

    s = oss_head_object(options, &bucket, &object, headers1, &resp_headers);
    if(aos_status_is_ok(s)) {
        object_type = (char*)(apr_table_get(resp_headers, OSS_OBJECT_TYPE));
        if (0 != strncmp(OSS_OBJECT_TYPE_APPENDABLE, object_type, 
                         strlen(OSS_OBJECT_TYPE_APPENDABLE))) 
        {
            printf("object[%s]'s type[%s] is not Appendable\n", OBJECT_NAME, object_type);
            aos_pool_destroy(p);
            return;
        }
        
        next_append_position = (char*)(apr_table_get(resp_headers, OSS_NEXT_APPEND_POSITION));
        position = aos_atoi64(next_append_position);
    }

    s = oss_append_object_from_file(options, &bucket, &object, 
                                    position, &file, headers2, &resp_headers);

    if (aos_status_is_ok(s)) {
        printf("append object from file succeeded\n");
    } else {
        printf("append object from file failed\n");
    }    

    aos_pool_destroy(p);
}

void append_object_sample()
{   
    append_object_from_buffer();
    append_object_from_file();
}
HTTP/1.1 200 OK
X-Oss-Request-Id: 57DA1A96806227EB54423A01
Server: AliyunOSS
X-Oss-Next-Append-Position: 4013
Date: Sun, 05 Nov 2023 03:12:03 GMT
Content-Length: 0
Connection: Keep-Alive

```
