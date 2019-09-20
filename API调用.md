### API调用

#### 一、API简介

##### 1、API的概念

API（Application Programming Interface，应用程序编程接口）是一些预先定义的函数，目的是提供应用程序与开发人员基于某软件或硬件得以访问一组例程的能力，而又无需访问源码或理解内部工作机制的细节。

##### 2、API的特点

·API是一个明确定义的接口，可以为其他软件提供特定服务
·API可以小到只包含一个单独的函数，也可以大到包含数以百计的类、方法、全局函数、数据类型、枚举类型和常量等
·API的实现可以是私有的，也可以是开源的

##### 5、API的分类

面向对象语言的API
·举例：JavaAPI列表
库与框架的API
·举例：Windows API、Windows DirectX
API与协议
·举例：LDAP应用程序接口
API与设备接口
·举例：PCBIOS调用接口、ASPI for SCSI设备接口
Web API
·举例：Google地图API、新浪微博API、阿里云API市场

##### 6、为什么要使用API？

·快速扩展功能
·避免“造轮子”，提高开发效率
·降低模块之间的耦合度

#### 二、API请求与认证

##### 1、WebAPI协议

Web API一般采用HTTP作为底层协议，HTTP请求机制如下：
·客户端向服务器发送一个请求
·服务器给客户端一个响应，告诉客户端是否可以完成它请求的工作

![1568955551079](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1568955551079.png)

编程语言是互相解耦的！！！

##### 2、HTTP请求包含的内容

为了构造有效的请求，客户端需要包含四个部分：
·URL（API调用地址）
·请求方式 post get delete put
·Headers（请求头）
·Body（请求主体）

![1568955717696](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1568955717696.png)

##### 3、API请求方式

请求方式告诉服务器客户端希望它采取什么动作。常见的请求方式有四种：
·GET：请求服务器获取一个资源
·POST：请求服务器创建一个新的资源
·PUT：请求服务器编辑或更新一个已存在的资源
·DELETE：请求服务器删除一个资源

##### 4、请求头与请求主体

请求头（Headers）
·提供了请求的元信息，是一个简单的项目列表，其中有客户端发送请求的时间和请求主体大小、身份认证等信息。
请求体（Body）
·包含了客户端希望发送给服务器的数据。

##### 5、状态码-成功状态

当成功调用API后，除了返回数据外，还会包含一个状态码，处理成功返回2xx：
HTTP状态码              语义
200OK-[GET]              服务器成功返回用户请求的数据201 CREATED-[POST/PUT/PATCH]用户新建或修改数据成功202Accepted-[*]          表示一个请求已经进入后台排队（异步任务）
204NO CONTENT-[DELETE]        用户删除数据成功

##### 6、状态码-服务端错误码

API未调用成功，则返回错误码。服务端错误码是5xx，表示服务不可用（此时一般建议重试或联系商品页面的API服务商）

错误代码              HTTP状态码      语义    解决方案
Internal Error         500                API网关内部错误        建议重试。
Failed To Invoke Backend Service   500           底层服务错误 API提供者底层服务错误，建议重试，如果重试多次仍然不可用，可联系API服务商解决。 
Service Unavailable           503         服务不可用           建议稍后重试。
Async Service          504       后端服务超时         建议稍后重试。

##### 7、状态码-客户端错误码

客户端错误码为4xx，表示业务报错。此时一般为参数错误、签名错误、请求方式有误或被流控限制等业务类错误。建议详细查看错误码，针对性解决问题。
参考：错误代码表 https://help.aliyun.com/document_detail/43906.html注意：有些API自定义了错误码，具体请查看该API文档中的描述。

##### 8、返回的数据格式

![1568956390613](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1568956390613.png)

##### 9、JSON数据格式表示方法

![1568956445093](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1568956445093.png)

##### 10、简单身份认证（APPCODE方式）

可以通过APPCODE的方式，实现到被调用接口的身份认证，获取访问相关API的调用权限。
使用方法：
·请求Header中添加的Authorization字段；
·配置Authorization字段的值为“APPCODE+半角空格+APPCODE值"。
格式：
Authorization:APPCODE AppCode值示例：
Authorization:APPCODE 3F2504E04F8911D39A0C0305E82C3301

##### 11、API签名认证（AppKey&AppSecret）

AppKey和AppSecret 相当于当前账户的另外一套账号和密码机制。在云市场购买API之后，就可以在控制台找到对应的AppKey和AppSecret。
具体的签名认证方法请参见请求头部字段以及请求签名说明文档

#### 三、API调试与调用

##### 1、API调用步骤

获取API文档>创建应用>获取授权>调用APl

要调用API 需要三个基础条件：
·API：您即将要调用的API，明确API参数定义。
·应用app：作为您调用API时的身份，有AppKey和AppSecret 用于验证您的身份。
·API和App的权限关系：App想调用某个API需要具有该API的权限，这个权限通过授权的功能来建立。

##### 2、获取API文档

·在云市场选择API，在API产品页面即可找到该API的使用说明（文档）。
·购买API服务成功后，进入云市场的管理控制台，就会看见购买的所有API服务。（如果还没有开通API网关服务，那么会同时开通API网关服务，让你使用更流畅）
·可以跳转到API网关的控制台，在已购买API页面，展示购买的所有API服务列表，以及使用情况概况。

##### 3、创建应用

·应用（APP）是调用API服务时的身份。每个APP有一组Key和Secret，可以理解为账号密码，调用API的时候需要将AppKey做参数传入，AppSecret用于签名计算，网关会校验这对密钥对你进行身份认证。
·可以在API网关控制台应用管理页面创建APP，创建成功后，系统会为APP分配一对AppKey和AppSecret。

##### 4、获得授权

·授权，是指授予APP调用某个API的权限。您的APP需要获得API的授权才能调用该API。·如果你在市场购买了API，就可以指定将已购买的API授权给哪些APP，然后这些APP才能调用该API。
·如果您没有APP，购买时云市场会为你创建一个APP，并且授权。

##### 5、API调用注意事项

·每个账号下APP个数上限为10个，APP名称应为账号下唯一。
·调用API的流控限制为，单个IP，QPS不超过100。
·你有权操作购买的API与APP的授权和解除授权。由服务提供方授权给你的APP的API，你无权操作解除授权。
·你的请求需要包含签名信息，请参照文档请求签名说明文档。

#### 四、API实战



