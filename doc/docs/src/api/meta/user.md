# 用户相关

## 创建用户

```bash
POST /api/{organization}/users
```
## 更新用户

```bash
PUT /api/{organization}/users/{user_email}
```

## 增加一个用户到组织

```bash
POST /api/{organization}/users/{user_email}
```
## 删除组织中的一个用户

```bash
DELETE /api/{organization}/users/{user_email}
```
## 列举一个组织中的用户

```bash
GET /api/{organization}/users
```

