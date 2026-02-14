# 登录、注册页

## 用户注册（普通用户）
接口地址 POST /api/users/register
请求体
| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| email | string | 是 | 邮箱地址 |
| password | string | 是 | 密码（至少8位） |
| confirmPassword | string | 是 | 确认密码 |
| username | string | 否 | 用户昵称 |

请求体示例
{
    "email": "user@company.com",
    "password": "password12345",
    "confirmPassword": "password12345",
    "username": "张三"
}

成功响应 (201 Created)
{
    "code": 201,
    "message": "User registered successfully",
    "data": {
        "id": "user_001",
        "email": "user@company.com",
        "username": "张三",
        "role": "user"
    }
}

错误响应 (400 Bad Request)
{
    "code": 400,
    "message": "Registration failed",
    "errors": ["Email already exists"]
}

---

## 用户登录（普通用户）
接口地址 POST /api/users/login
请求体
| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| email | string | 是 | 邮箱地址 |
| password | string | 是 | 密码 |

请求体示例
{
    "email": "user@company.com",
    "password": "password12345"
}

成功响应 (200 OK)
{
    "code": 200,
    "message": "Login successful",
    "data": {
        "id": "user_001",
        "email": "user@company.com",
        "username": "张三",
        "role": "user",
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
    }
}

错误响应 (401 Unauthorized)
{
    "code": 401,
    "message": "Invalid email or password"
}

---

## 商家注册
接口地址 POST /api/business/register
请求体
| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| email | string | 是 | 邮箱地址 |
| password | string | 是 | 密码（至少8位） |
| confirmPassword | string | 是 | 确认密码 |
| businessName | string | 是 | 商家名称/公司名 |
| contactPhone | string | 是 | 联系电话 |

请求体示例
{
    "email": "business@company.com",
    "password": "password12345",
    "confirmPassword": "password12345",
    "businessName": "XX酒店集团",
    "contactPhone": "13800138000"
}

成功响应 (201 Created)
{
    "code": 201,
    "message": "Business account registered successfully",
    "data": {
        "id": "business_001",
        "email": "business@company.com",
        "businessName": "XX酒店集团",
        "role": "business",
        "status": "pending"  // 待审核状态
    }
}

---

## 商家登录
接口地址 POST /api/business/login
请求体
| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| email | string | 是 | 邮箱地址 |
| password | string | 是 | 密码 |

请求体示例
{
    "email": "business@company.com",
    "password": "password12345"
}

成功响应 (200 OK)
{
    "code": 200,
    "message": "Login successful",
    "data": {
        "id": "business_001",
        "email": "business@company.com",
        "businessName": "XX酒店集团",
        "role": "business",
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
    }
}

---

## 管理员登录
接口地址 POST /api/admin/login
请求体
| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| email | string | 是 | 管理员邮箱 |
| password | string | 是 | 密码 |

请求体示例
{
    "email": "admin@company.com",
    "password": "admin12345"
}

成功响应 (200 OK)
{
    "code": 200,
    "message": "Login successful",
    "data": {
        "id": "admin_001",
        "email": "admin@company.com",
        "role": "admin",
        "permissions": ["approve_hotel", "offline_hotel", "manage_users"],
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
    }
}

---

## 获取当前用户信息（用于判断角色）
接口地址 GET /api/users/profile
请求头
| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| Authorization | string | 是 | Bearer {token} |

成功响应 (200 OK)
{
    "code": 200,
    "message": "Success",
    "data": {
        "id": "user_001",
        "email": "user@company.com",
        "role": "user",  // user / business / admin
        "username": "张三"
    }
}



# 超级管理员页
## 创建管理员
接口地址 POST /api/super-admin/user/create-admin
请求体：
{
    "email": "new-admin@company.com",
    "role": "admin"
}
响应：
{
    "code": 201,
    "message": "Admin created",
    "data": {
        "id": "admin_123",
        "email": "new-admin@company.com",
        "role": "admin"
    }
}






# 商户-酒店页
## 创建酒店
接口地址 POST /api/business/hotels
请求体 (json)
{
    "name": "新酒店名称",
    "nameEn": "酒店英文名",
    "city": "北京",
    "location": "五道口",
    "starRating": 5,
    "roomType": 大床房,
    "price": 1500,
    "openingTime": "24h",
    "nearbyAttractions": "故宫",
    "transportation": "地铁",
    "status": "under_review"
}
成功相应(201 created)(json)
{
    "code": 201,
    "message": "Hotel created successfully",
    "data":{
        "id":"新生成的酒店ID"，
    }
}

## 查询酒店
接口地址  GET /api/business/hotels??name=新酒店名称&nameEn=酒店英文名&city=北京&location=五道口&starRating=5&roomType=大床房&price=1500&openingTime=24h&nearbyAttractions=故宫&transportation=地铁    //没有提供参数的等式不会出现在查询字符串里面
请求体：无
成功相应(201 created)(json)
{
    "code": 201,
    "message": "Hotel created successfully",
    "data":{
        "id":"被查询的酒店ID"，
    }
}


## 删除酒店
接口地址 DELETE /api/business/hotels/{id}
请求体 无 或者空json:{}
成功相应(201 created)(json)
{
    "code": 201,
    "message": "Hotel deleted successfully",
    "data":{
        "id":"被删除的酒店ID"，
    }
}

## 编辑酒店
接口地址 PUT /api/business/hotels/{id}
请求体 (json)
{
    "name": "新酒店名称",
    "city": "北京",
    "location": "五道口",
    "starRating": 5
    //被修改的字段
}
成功相应(201 created)(json)
{
    "code": 201,
    "message": "Hotel updated successfully",
    "data":{
        "id":"被修改的的酒店ID"，
    }
    "status": "under_review", //待审核
    "starRating": 5
}
