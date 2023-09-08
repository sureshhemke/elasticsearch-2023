# Define Roles

```
POST _security/role/read_only
{
 "indices": [
  {
   "names": ["*"],
   "privileges": ["read"]
  }
 ]
}
```

```
GET _security/role/read_only 
```

