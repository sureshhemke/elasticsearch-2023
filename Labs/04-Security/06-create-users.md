# Create Users

```
POST _security/user/atin
{
 "roles": ["read_only", "kibana_user"]
 "full_name": "Atin Gupta",
 "email": "atingupta2005@gmail.com",
 "password": "Atin@123"
}
```

```
GET  _security/user/atin
```

- Logout from kibana and login with this new user
