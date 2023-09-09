Check node status
```
GET _cat/nodes?v
```
Check Index status
```
GET _cat/indices?v
```

Check node status with specific field
```
GET /_cat/nodes?v=true&h=id,ip,port,v,m
```
