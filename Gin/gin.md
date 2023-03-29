# gin

gin.Context

It's helpful to break down the different components of the struct and what they represent:

1. Request: This field contains information about the HTTP request that is being handled, such as the HTTP method, URL path, query parameters, headers, and body.
2. Writer: This field is used to send the HTTP response back to the client. You can use methods like Write, WriteString, and JSON to write the response body, and Header to set response headers.
3. Parrams: This field contains any route parameters that were extracted from the URL path using route patterns.
4. Handlers: This field is an array of middleware functions that will be executed before the main handler function.
5. Keys: This field is used to store data that can be shared between middleware functions and the main handler function.
6. Error: This field is used to store any errors that occur during the handling of the request.
7. Accepted: This field contains a list of MIME types that the client has indicated it can accept.
8. Query: This field is a map of query parameter keys to values.
9. FullPath: This field contains the full URL path of the request, including any route parameters.
10. Is Aborted: This field is a boolean that indicates whether the request has been aborted by a middleware function.

## 学习HTTP（超文本传输协议）可以从以下几个方面入手：

### 1. 了解基本概念

首先，了解HTTP的基本概念和背景知识，包括：

- 客户端与服务器之间的通信方式
- URI（统一资源标识符）
- URL（统一资源定位符）
- HTTP的发展历程，如HTTP/1.0、HTTP/1.1、HTTP/2、HTTP/3

### 2. 研究HTTP请求和响应

HTTP的核心是请求（Request）和响应（Response），要熟悉它们的各个组成部分，例如：

- 请求和响应的结构
- HTTP方法（GET、POST、PUT、DELETE等）
- 请求头（Header）的常见字段
- 响应状态码（例如：200 OK, 404 Not Found, 500 Internal Server Error等）
- 响应头（Header）的常见字段

### 3. 学习HTTP的工作原理

深入了解HTTP的工作原理，包括：

- 无状态协议
- 连接管理（如持久连接、管道化请求等）
- 缓存机制
- Cookie和Session
- 安全性（如HTTPS、TLS/SSL等）

### 4. 实践

通过实际编程实践来加深理解：

- 使用命令行工具（如`curl`）发起HTTP请求
- 使用浏览器开发者工具查看HTTP请求和响应
- 编写简单的客户端和服务器程序，实现HTTP请求和响应
- 学习常见的HTTP客户端和服务器库，如Python的`requests`和`Flask`，Node.js的`axios`和`Express`等

### 5. 参考资料

查阅一些参考资料，包括：

- [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/HTTP) - 提供详细的HTTP相关信息
- 《HTTP权威指南》（HTTP: The Definitive Guide） - 一本深入浅出的HTTP教材
- [RFC 7230](https://tools.ietf.org/html/rfc7230) - HTTP/1.1协议规范
- [RFC 7540](https://tools.ietf.org/html/rfc7540) - HTTP/2协议规范
- [RFC 9000](https://tools.ietf.org/html/rfc9000) - HTTP/3协议规范

通过以上步骤，你应该能够逐步掌握HTTP的知识。祝你学习顺利！

## Learn Context-Type

Content-Type is an HTTP header used to indicate the media type of the resource in the message body. It's essential to understand how to work with Content-Type when dealing with APIs, web services, and web development in general. To learn about Content-Type, follow the steps below:

1. Understand HTTP Headers:
   Start by learning the basics of HTTP headers and their role in client-server communication. Headers are metadata that provide additional information about the request or response.

2. Learn about MIME types:
   Content-Type header values are represented by MIME types (e.g., `text/html`, `application/json`). Understanding MIME types is crucial, as they define the format of the data being transmitted.

3. Familiarize yourself with common Content-Types:
   Some common Content-Types include:

   - `text/html` for HTML documents
   - `application/json` for JSON data
   - `text/css` for CSS files
   - `application/javascript` for JavaScript files
   - `image/jpeg` for JPEG images

   There are many other MIME types, so explore the [IANA Media Types list](https://www.iana.org/assignments/media-types/media-types.xhtml) to get familiar with them.

4. Practice using Content-Type in requests and responses:
   When working with APIs, you'll often need to set the Content-Type header in your requests to indicate the format of the data you're sending. Additionally, you should check the Content-Type header in API responses to ensure you handle the received data correctly. Experiment with different APIs to get hands-on experience.

5. Explore Content-Type in web development:
   Content-Type headers are also important in web development. When serving static files, ensure your web server sets the appropriate Content-Type header for each file type.

6. Learn about charset:
   The Content-Type header can also include a `charset` parameter to specify the character encoding used in the transmitted data (e.g., `text/html; charset=UTF-8`). Understand the role of character encoding and how it affects the interpretation of the received data.

7. Read official documentation and specifications:
   Refer to the following resources for a deeper understanding:

   - [MDN Web Docs: Content-Type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type)
   - [RFC 7231: Hypertext Transfer Protocol (HTTP/1.1): Semantics and Content](https://tools.ietf.org/html/rfc7231#section-3.1.1.5)

8. Work on real-world projects:
   Apply your knowledge of Content-Type headers in real-world projects, such as building web applications, APIs, or working with third-party APIs. This will help solidify your understanding and improve your skills.

By following these steps and practicing consistently, you will gain a strong understanding of Content-Type and its practical applications in web development and API communication.

## 详解gin.Context

`gin.Context` 是 Gin Web 框架的核心组件，它封装了一个 HTTP 请求的所有相关信息和操作。`gin.Context` 可以让你轻松地处理请求和生成响应，同时它还提供了许多有用的方法和功能。以下是关于 `gin.Context` 的详细介绍：

1. 请求数据：`gin.Context` 提供了访问请求数据的方法，包括路径参数、查询参数、请求头和请求体等。例如：
   1. `context.Param(key string)`：获取路径参数。
   2. `context.Query(key string)`：获取查询参数。
   3. `context.DefaultQuery(key string, defaultValue string)`：获取查询参数，如果不存在则返回默认值。
   4. `context.GetHeader(key string)`：获取请求头信息。
   5. `context.ShouldBind(obj interface{})`：将请求数据绑定到指定结构体。
2. 响应数据：你可以使用 `gin.Context` 生成和发送 HTTP 响应。例如：
   - `context.JSON(code int, obj interface{})`：以 JSON 格式发送响应。
   - `context.XML(code int, obj interface{})`：以 XML 格式发送响应。
   - `context.HTML(code int, name string, obj interface{})`：渲染并发送 HTML 响应。
   - `context.String(code int, format string, values ...interface{})`：发送格式化的字符串响应。
   - `context.Status(code int)`：设置响应状态码。
   - `context.Header(key string, value string)`：设置响应头信息。
3. 中间件和处理函数：`gin.Context` 在 Gin 框架中充当中间件和处理函数的载体。你可以通过它来访问和操作请求和响应数据。例如：
   1. `context.Next()`：执行下一个中间件/处理函数。
   2. `context.Abort()`：阻止后续中间件/处理函数的执行。
   3. `context.Set(key string, value interface{})`：在 `gin.Context` 中设置键值对，可在中间件和处理函数之间传递数据。
   4. `context.Get(key string) (value interface{}, exists bool)`：从 `gin.Context` 中获取指定键的值。
4. 错误处理：`gin.Context` 提供了一些方法来处理错误。
   1. `context.Error(err error)`：将错误附加到 `gin.Context`。
   2. `context.Errors.Last()`：获取最后一个错误。
   3. `context.Errors.ByType(type ErrorType)`：获取特定类型的错误。
5. 文件上传和下载：`gin.Context` 提供了处理文件上传和下载的方法。
   1. `context.FormFile(name string) (*multipart.FileHeader, error)`：获取上传的文件。
   2. `context.SaveUploadedFile(file *multipart.FileHeader, dst string) error`：保存上传的文件到指定路径。
   3. `context.File(filepath string)`：发送一个文件作为响应。
6. 路由信息：`gin.Context` 提供了关于当前请求的路由信息。
   1. `context.FullPath()`：获取完整的请求路径。
   2. `context.HandlerName()`：获取当前处理函数的名称。

通过熟练使用 `gin.Context`，你可以轻松地处理请求、发送响应、传递数据、处理错误等。在编写基于 Gin 框架的 Web 应用程序时，理解和掌握 `gin.Context` 的使用方法和功能是至关重要的。

## gin的常用函数解析

1. AsciiJSON: Using AsciiJSON to Generates ASCII-only JSON with escaped non-ASCII characters.(转义非ASCII字符)

2. Bind form-data request with custom struct(绑定custom struct)

   ```
   type StructA struct {
   		FieldA string `form: "field_a"`
   }
   func GetData(c *gin.Context) {
   		var a StructA
   		c.Bind(&a)
   }
   ```

3. Bind html checkboxes(绑定表单复选框)

   ```
   type myForm struct {
   		Colors []string `form: "colors[]"`
   }
   func formHandler(c *gin.Context) {
   		var fakeForm myForm
   		c.ShouldBind(&fakeForm)
   }
   # html
   <form action="/" method="POST">
   		<p>Check some colors</p>
       <label for="red">Red</label>
       <input type="checkbox" name="colors[]" value="red" id="red">
       <label for="green">Green</label>
       <input type="checkbox" name="colors[]" value="green" id="green">
       <label for="blue">Blue</label>
       <input type="checkbox" name="colors[]" value="blue" id="blue">
       <input type="submit">
   </form>
   ```

4. Bind query string or post data(绑定查询表单或post数据)

   ```
   type Person struct {
   		Name     string    `form:"name"`
   		Address  string    `form:"address"`
   		Birthday time.Time `form:"birthday" time_format:"2006-01-02" time_utc:"1"`
   }
   func startPage(c *gin.Context) {
   		var person Person
   		if c.ShouldBind(&person) == nil {
   				log.Println(person.Name)
   		}
   }
   ```

5. Bind Uri(绑定URL地址)

   ```
   type Person struct {
   		ID string `uri: "id" binding:"required,uuid"`
   		Name string `uri: "name" binding:"required"`
   }
   route.GET("/:name:id", func(c *gin.Context) {
   		var person Person
   		if err := c.ShouldBindUri(&person); err != nil {
   				c.JSON(400, gin.H{"msg": err})
   				return
   		}
   })
   ```

6. Controlling Log output coloring

   ```
   By default, logs output on console should be colorized depending on the detected. (根据检测的TTY进行着色)
   Never colorize logs:
   gin.DisableConsoleColor()
   Always colorize logs:
   gin.ForceConsoleColor()
   ```

7. Custom HTTP configuration

   ```
   func main() {
   		router := gin.Default()
   		s := &http.Server{
   				Addr: "8080",
   				Handler: router,
   				ReadTimeout: 10 * time.Second,
   				WriteTimeout: 10 * time.Second,
   				MaxHeaderBytes: 1 << 20,
   		}
   		s.ListenAndServe()
   }
   ```

8. Custom log file

   ```
   router.Use(gin.LoggerWithFormatter(func(param gin.LogFormatterParams) string {
   		return fmt.Sprintf("%s - [%s] \"%s %s %s %d %s \"%s\" %s\"\n",
   				param.ClientIP,
   				param.TimeStamp.Format(time.RFC1123),
   				param.Method,
   				param.Path,
   				param.Request.Proto,
   				param.StatusCode,
   				param.Latency,
   				param.Request.UserAgent(),
   				param.ErrorMessage,
   		)
   }))
   router.Use(gin.Recovery())
   ```

9. Custom Middleware(自定义中间件)

   ```
   func Logger() gin.HandlerFunc {
   		return func(c *gin.Context) {
   				t := time.Now()
   				// Set example variable
   				c.Set("example", "12345")
   				// before request
   				c.Next()
   				// after request
   				latency := time.Since(t)
   				log.Print(latency)
   				// access the status we are sending
   				status := c.Writer.Status()
   				log.Println(status)
   		}
   }
   func main() {
   		r := gin.New()
   		r.Use(Logger())
   }
   ```

10. Define format for the log of routes

    ```
    gin.DebugPrintRouteFunc = func(httpMethod, absolutePath, handlerName string, nuHandlers int) {
    	log.Printf("endpoint %v %v %v %v\n", httpMethod, absolutePath, handlerName, nuHandlers)
    }
    ```

11. Goroutines inside a middleware
    When starting new Goroutines inside a middleware or handler, you **SHOULD NOT** use the original context inside it, you have to use a read-only copy.

    ```
    r.GET("/long_async", func(c *gin.Context) {
    		cCp := c.Copy()
    		go func() {
    				time.Sleep(5 * time.Second)
    				log.Println("Done! in path " + cCp.Request.URL.Path)
    		}()
    })
    ```

12. How to write log file

    ```
    gin.DisableConsoleColor()
    f, _ := os.Create("gin.log")
    gin.DefaultWriter = io.MultiWriter(f)
    ```

13. **JSONP**
    Using JSONP to request data from a server in a different. Add callback to response body if the query parameter callback exists.

    ```
    r.GET("/JSONP?callback=x", func(c *gin.Context) {
    		data := map[string]interface{}{
    				"foo": "bar",
    		}
    		// callback is x
    		// Will output : x({\"foo\":\"bar\"})
    		c.JSONP(http.StatusOK, data)
    })
    ```

14. Map as querystring or postform parameters

    ```
    POST /post?ids[a]=1234&ids[b]=hello HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    
    names[first]=thinkerou&names[second]=tianou
    
    router.POST("/post", func(c *gin.Context) {
    		ids := c.QueryMap("ids")
    		names := c.PostFormMap("names")
    })
    // output 
    ids: map[b:hello a:1234], names: map[second:tianou first:thinkerou]
    ```

15. Multipart/Urlencoded binding

    ```
    type LoginForm struct {
    		User string `form:"user" binding:"required"`
    		Password string `form:"password" binding:"required"`
    }
    var form LoginForm
    c.ShouldBind(&form)
    
    curl -v --form user=user --form password=password http://localhost:8080/login
    ```

16. Multipart/Urlencoded form

    ```
    message := c.PostForm("message")
    nick := c.DefaultPostForm("nick", "anonymous")
    ```

17. Only bind query string
    `ShouldBindQuery` function only binds the query params and not the post data.

    ```
    type Person struct {
    		Name string `form:"name"`
    		Address string `form:"address"`
    }
    var person Person
    c.ShouldBindQuery(&person)
    ```

18. Parameters in path

    ```
    router.GET("/user/:name/*action", func(c *gin.Context) {
    		name := c.Param("name")
    		action := c.Param("action")
    })
    ```

19. PureJSON

    Normally, JSON replaces special HTML characters with their unicode entities, e.g. < becomes \u003c. If you want to encode such characters literally, you can use PureJSON instead.

    ```
    c.PureJSON(200, gin.H{
    		"html": "<b>Hello, world!</b>",
    })
    ```

20. Query and post form

    ```
    POST /post?id=1234&page=1 HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    
    name=manu&message=this_is_great
    
    router.POST("/post", func(c *gin.Context) {
    
    		id := c.Query("id")
    		page := c.DefaultQuery("page", "0")
    		name := c.PostForm("name")
    		message := c.PostForm("message")
    
    		fmt.Printf("id: %s; page: %s; name: %s; message: %s", id, page, name, message)
    })
    // output
    id: 1234; page: 1; name: manu; message: this_is_great
    ```

21. Query string parameters

    ```
    // The request responds to a url matching:  /welcome?firstname=Jane&lastname=Doe
    firstname := c.DefaultQuery("firstname", "Guest")
    		lastname := c.Query("lastname") // shortcut for c.Request.URL.Query().Get("lastname")
    ```

22. Redirects
    Issuing a HTTP redirect is easy. Both internal and external locations are supported.

    `c.Redirect(http.StatusMovedPermanently, "http://www.google.com/")`

    Issuing a HTTP redirect from POST. Refer to issue: [#444](https://github.com/gin-gonic/gin/issues/444)

    `c.Redirect(http.StatusFound, "/foo")`
    Issuing a Router redirect, use `HandleContext` like below.

    ```
    c.Request.URL.Path = "/test2"
    r.HandleContext(c)
    ```

23. SecureJSON
    Using SecureJSON to prevent json hijacking. Default prepends `"while(1)",` to response body if the given struct is array values.

    ```
    names := []string{"lena", "austin", "foo"}
    // will output : while(1); ["lena", "austin", "foo"]
    c.SecureJSON(http.StatusOK, names)
    ```

24. Serving data from reader

    ```
    router.GET("/someDataFromReader", func(c *gin.Context) {
    		response, err := http.GET("https://raw.githubusercontent.com/gin-gonic/logo/master/color.png")
    		if err != nil || response.StatusCode != http.StatusOK {
    				c.Status(http.StatusServiceUnavailable)
    				return
    		}
    		reader := response.Body
    		contentLength := response.ContentLength
    		contentType := response.Header.Get("Content-Type")
    		extraHeaders := map[string]string{
    				"Content-Disposition": `attachment; filename="gopher.png"`,
    		}
    		c.DataFromReader(http.StatusOK, contentLength, contentType, reader, extraHeaders)
    })
    ```

25. Serving static files

    ```
    router.Static("/assets", "/assets")
    router.StaticFS("/more_static", http.Dir("my_file_system"))
    router.StaticFile("/favicon.ico", "/resources/favicon.ico")
    ```

26. Set and get a cookie

    ```
    router.GET("/cookie", func(c *gin.Context) {
    		cookie, err := c.Cookie("gin_cookie")
    		if err != nil {
    				cookie = "NotSet"
    				c.SetCookie("gin_cookie", "test", 3600, "/", "localhost", false, true)
    		}
    		fmt.Printf("Cookie value: %s \n", cookie)
    })
    ```

27. Upload files, Multiple files

    ```
    // Set a lower memory limit for multipart forms (default is 32 MiB)
    router.MaxMultipartMemory = 8 << 20 // 8 MiB
    router.POST("/upload", func(c *gin.Context) {
    		// Multipart form
    		form, _ := c.MultipartForm()
    		files := form.File["upload[]"]
    		for _, file := range files {
    				log.Println(file.Filename)
    				// upload the file to specific dst.
    				c.SaveUploadedFile(file, dst)
    		}
    		c.String(http.StatusOK, fmt.Sprintf("%d files uploaded!", len(files)))
    })
    
    curl -X POST http://localhost:8080/upload \
    	-F "upload[]=@/Users/appleboy/test1.zip" \
    	-F "upload[]=@/Users/appleboy/test2.zip" \
    	-H "Content-Type: multipart/form-data"
    ```

28. Upload files, Single file

    ```
    router.MaxMultipartMemory = 8 << 20 // 8 MiB
    router.POST("/upload", func(c *gin.Context) {
    		// single file
    		file, _ := c.FormFile("file")
    		log.Println(file.Filename)
    		// Upload the file to specific dst.
    		c.SaveUploadedFile(file, dst)
    		c.String(http.StatusOK, fmt.Sprintf("'%s' uploaded!", file.Filename))
    })
    
    curl -X POST http://localhost:8080/upload \
    	-F "file=@Users/appleboy/test.zip0" \
    	-H "Content-Type: multipart/form-data"
    ```

29. Using BasicAuth middleware

    ```
    // simulate some private data
    var secrets = gin.H{
    		"foo": gin.H{"email": "foo@bar.com", "phone": "123433"},
    		"austin": gin.H{"email": "austin@example.com", "phone": "666"},
    		"lena":   gin.H{"email": "lena@guapa.com", "phone": "523443"},
    }
    
    // Group using gin.BasicAuth() middleware
    // gin.Accounts is a shortcut for map[string]string
    authorized := r.Group("/admin", gin.BasicAuth(gin.Accounts{
    		"foo":    "bar",
    		"austin": "1234",
    		"lena":   "hello2",
    		"manu":   "4321",
    }))
    
    // /admin/secrets endpoint
    // hit "localhost:8080/admin/secrets"
    authorized.GET("/secrets", func(c *gin.Context) {
    		// get user, it was set by the BasicAuth middleware
    		user := c.MustGet(gin.AuthUserKey).(string)
    		if secret, ok := secrets[user]; ok {
    				c.JSON(http.StatusOK, gin.H{"user": user, "secret": secret})
    		} else {
    				c.JSON(http.StatusOK, gin.H{"user": user, "secret": "NO SECRET :("})
    		}
    })
    ```

30. Using HTTP method

    ```
    router.GET("/someGet", getting)
    router.POST("/somePost", posting)
    router.PUT("/somePut", putting)
    router.DELETE("/someDelete", deleting)
    router.PATCH("/somePatch", patching)
    router.HEAD("/someHead", head)
    router.OPTIONS("/someOptions", options)
    ```

31. Using middleware

    ```
    // creates a router without any middleware by default
    r := gin.New()
    
    // Global middleware
    // Logger middleware will write the logs to gin.DefaultWriter even if you set with GIN_MODE=relese.
    // By default gin.DefaultWriter = os.Stdout
    r.Use(gin.Logger())
    
    // Recovery middleware recovers from any panics and writes a 500 if there was one.
    r.Use(gin.Recovery())
    
    // Per route middleware, you can add as many as you desire.
    r.GET("/benchmark", MyBenchLogger(), benchEndpoint)
    
    // Authorization group
    // authorized := r.Group("/", AuthRequired())
    // exactly the same as:
    authorzied.Use(AuthRequired()){
    		authorized.POST("/login", loginEndpoint)
    		authorized.POST("/submit", submitEndpoint)
    		authorized.POST("/read", readEndpoint)
    		
    		// nested group
    		testing := authorized.Group("testing")
    		testing.GET("/analytics", analyticsEndpoint)
    }
    ```

