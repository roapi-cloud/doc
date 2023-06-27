# 全流程试用

## 注册账号
请发送邮件到yuping322@gmail.com，申请试用

## 下载测试数据
```bash
curl -L https://zinc-public-data.s3.us-west-2.amazonaws.com/zinc-enl/sample-k8s-logs/k8slog_json.json.zip -o k8slog_json.json.zip
unzip k8slog_json.json.zip
```
## 上传数据到云存储中
```bash
curl http://localhost:5080/api/default/default/_json -i -u "<帐号名：密码>"  -d "@k8slog_json.json"
```

## 云接口查询
```bash
curl -u "<帐号名：密码>" -X 'POST' \
  'http://roapi-cloud.com/api/default/_search' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "aggs": {
    "histogram": "select histogram(_timestamp, '\''30 second'\'') AS zo_sql_key, count(*) AS zo_sql_num from query GROUP BY zo_sql_key ORDER BY zo_sql_key"
  },
  "query": {
    "end_time": 1675185660872049,
    "from": 0,
    "size": 10,
    "sql": "select * from k8s ",
    "start_time": 1675182660872049
  }
}'

```

## 下载云版本roapi

地址：https://github.com/roapi-cloud/roapi-cloud/releases/
```bash
wget https://github.com/roapi-cloud/roapi-cloud/releases/download/v0.01/roapi_linux_amd32.tar.gz
```

## 查询meta获取该组织下有权限的数据
```bash
curl -X 'GET' \
  'http://roapi-cloud.com/api/default/streams' \
  -H 'accept: application/json'
```
## 本地启动roapi程序
```bash
./roapi  --table "test=s3://testfegz/files/default/logs/default/,format=parquet"
```

## 查询本地数据
```bash
curl -X POST -d "SELECT count(*) FROM test " localhost:8080/api/sql

localhost:8080/api/tables/test?columns=kubernetes_container_name,kubernetes_labels_controller_revision_hash&limit=12
```

## 从云端定期更新meta，并注册本地表
```bash
curl -X POST http://localhost:8080/api/table \
   -H 'Content-Type: application/json' \
   -d '[
    {
        "tableName": "tt123",
        "uri": "s3://testfegz/files/default/logs/default/,format=parquet"
    }
]'
```

