# minio
## install
```bash
docker run \
   -p 9000:9000 \
   -p 9001:9001 \
   --name minio \
   -v ~/minio/data:/data \
   -e "MINIO_ROOT_USER=ROOTNAME" \
   -e "MINIO_ROOT_PASSWORD=CHANGEME123" \
   quay.io/minio/minio server /data --console-address ":9001"
```

# mc
## install
```bash
wget https://dl.min.io/client/mc/release/linux-amd64/mc
chmod +x mc
./mc --help
```

## 用法
### 配置客户端
```bash
mc alias set myminio http://minio-server:9000 ACCESS_KEY SECRET_KEY
```
### 列出bucket
```bash
mc ls myminio/mybucket
```

### 创建bucket
```bash
mc ls myminio/newbucket
```

### 删除bucket
```bash
mc mb -r myminio/mybucket
```

### 下载文件
```bash
mc cp myminio/mybucket/myfile.txt .
```

### 同步文件夹
```bash
mc sync /path/to/local/foder minio/mybucket
```

### 删除文件
```bash
mc rm myminio/mybucket/myfile.txt
```

### 删除bucket中的所有文件
```bash
mc rm myminio/mybucket/*
```

### 获取对象信息
```bash
mc stat myminio/mybucket/myfile.txt
```