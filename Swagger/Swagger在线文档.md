# Go语言实践

## Gin-Swagger在线文档生成

[Gin实践 连载八 为它加上Swagger](https://segmentfault.com/a/1190000013808421)

### 安装swag命令

1. go get（不推荐）

```bash
go get -u github.com/swaggo/swag/cmd/swag

```

**注意：** 若$GOPAT/bin没有加入$PATH中，则需要其可执行文件移动到/usr/local/bin（环境变量）目录下

```bash
mv $GOPATH/bin/swag    /usr/local/bin 

```

2. gopm get（推荐）

由于该包引用golang.org上的包，正常情况下无法访问，则可以使用[gopm](https://gopm.io)进行安装

```bash
gopm get -g  -v  github.com/swaggo/swag/cmd/swag

cd $GOPATH/src/github.com/swaggo/swag/swag

go install -v
```

同理将可执行文件移动到usr/local/bin下

### 验证swag是否安装成功

```bash
swag -v
```

### 安装gin-swagger

```bash
go get -u github.com/swaggo/gin-swagger

go get -u github.com/swaggo/gin-swagger/swaggerFiles
```

## Swagger简介

Swagger 是一个强大的 API 文档构建工具，可以自动为 RESTful API 生成 Swagger 格式的文档，可以在浏览器中查看 API 文档，也可以通过调用接口来返回 API 文档（JSON 格式）。Swagger 通常会展示如下信息：

1. HTTP 方法（GET、POST、PUT、DELETE 等）
2. URL 路径
3. HTTP 消息体（消息体中的参数名和类型）
4. 参数位置
5. 参数是否必选
6. 返回的参数（参数名和类型）
7. 请求和返回的媒体类型

Swagger 还有一个强大的功能：可以通过 API 文档描述的参数来构建请求，测试 API。

### 文档语法说明

Swagger需要将相应的注释或注解编写到方法上，再利用生成器自动生成说明文件
gin-swagger给出的范例

```go
// @Summary Add a new pet to the store
// @Description get string by ID
// @Accept  json
// @Produce  json
// @Param   some_id     path    int     true        "Some ID"
// @Success 200 {string} string    "ok"
// @Failure 400 {object} web.APIError "We need ID!!"
// @Failure 404 {object} web.APIError "Can not find ID"
// @Router /testapi/get-string-by-int/{some_id} [get]
```

示例

```go
// @Summary Add new user to the database
// @Description Add a new user
// @Tags user
// @Accept  json
// @Produce  json
// @Param user body user.CreateRequest true "Create a new user"
// @Success 200 {object} user.CreateResponse "{"code":0,"message":"OK","data":{"username":"zhangsan"}}"
// @Router /user [post]
func Create(c *gin.Context) {
    ...
}

```

+ Summary：简单阐述 API 的功能
+ Description：API 详细描述
+ Tags：API 所属分类
+ Accept：API 接收参数的格式
+ Produce：输出的数据格式，这里是 JSON 格式
+ Param：参数，分为 6 个字段，其中第 6 个字段是可选的，各字段含义为：
    1. 参数名称
    2. 参数在 HTTP 请求中的位置（body、path、query）
    3. 参数类型（string、int、bool 等）
    4. 是否必须（true、false）
    5. 参数描述
    6. 选项，这里用的是 default() 用来指定默认值
+ Success：成功返回数据格式，分为 4 个字段
    1. HTTP 返回 Code
    2. 返回数据类型
    3. 返回数据模型
    4. 说明
+ 路由格式，分为 2 个字段：
    1. API 路径
    2. HTTP 方法

### 生成

在项目的根目录下，执行初始化命令

```bash
swag init

```

**注意：** 如果提示swag命令未找到，则必须将swag命令移动到bash可执行的环境变量下

执行成功之后，会在项目根目录下生成docs

```tree
docs/
├── docs.go
└── swagger
    ├── swagger.json
    └── swagger.yaml
```

### 验证

运行项目，访问http://127.0.0.1:8080/swagger/index.html，查看API是否生成正确
