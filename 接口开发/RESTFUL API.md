# restful api简介  
## restful api  
- 面向资源  
- http动词(get(获取) post(创建) put(修改) delete(删除))来描述操作  
- api数据格式一般为json  
## 传统api  举例
- 获取用户信息 get /api/user/read  
- 更新用户信息 post /api/user/update  
- 新增用户信息 post /api/user/add  
- 删除用户信息 post /api/user/delete  

## restful api  举例
-  获取用户信息 get /api/user/1  
-  更新用户信息 put /api/user/1  
-  新增用户信息 post /api/user  
-  删除用户信息 delete /api/user/1  

## HTTP常用状态码  
- 200 请求成功  
- 201 创建成功  
- 202 更新成功  
- 400 无效请求  
- 401 地址不存在  
- 403 禁止访问  
- 404 请求资源不存在  
- 500 内部错误  
## api数据结构格式  
- status 业务状态码  
- message 提示信息  
- data 数据层  
# 不可预知的内部异常处理  
# api数据安全  
- sign(授权码)  
- 身份认证token