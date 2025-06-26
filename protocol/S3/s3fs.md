# S3FS

## install
```shell
paru -S s3fs-fuse
```

## config
```shell
mkdir /mnt/s3

echo ACCESS_KEY_ID:SECRET_ACCESS_KEY > $HOME/.passwd-s3fs
chmod 600 $HOME/.passwd-s3fs
```

## mount
```shell
s3fs bucket_name /mnt/s3 -o passwd_file=$HOME/.passwd-s3fs -o url=http://s3.amazonaws.com -o use_path_request_style
```
