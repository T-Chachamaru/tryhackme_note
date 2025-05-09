# El Bandito

## 信息收集阶段

  - **端口扫描**
      - **发现**：服务器开放了 22 (SSH)、80 (HTTPS)、631 (IPP)、8080 (HTTP Proxy) 端口。注意到 80 端口运行的是 HTTPS 服务。
      - **截图**：[El Bandito 1](./iamges/El_Bandito6.png)
  - **目录扫描**
      - **发现**：通过目录扫描发现网站使用了 Spring 框架，并存在多个受限访问的端点。
      - **截图**：
          - [El Bandito 2](./iamges/El_Bandito1.png)
          - [El Bandito 3](./iamges/El_Bandito2.png)
  - **业务逻辑分析**
      - **发现**：网站存在三个主要的入口点，暗示了可能存在 WebSocket 功能。
      - **截图**：
          - [El Bandito 4](./iamges/El_Bandito3.png)
          - [El Bandito 5](./iamges/El_Bandito4.png)
          - [El Bandito 6](./iamges/El_Bandito5.png)

## 第一个 Flag - 漏洞探测阶段

  - **WebSocket 端点分析**
      - **行动**：使用 Burp Suite 拦截网页请求，观察到存在对 `/ws` 端点的访问。
      - **截图**：
          - [El Bandito 7](./iamges/El_Bandito7.png)
          - [El Bandito 8](./iamges/El_Bandito8.png)
  - **尝试请求走私**
      - **尝试**：构造格式错误的 WebSocket 连接请求，尝试走私请求以访问 Spring 框架的受限配置文件，但尝试失败。
      - **截图**：[El Bandito 9](./iamges/El_Bandito9.png)
  - **SSRF 可能性分析**
      - **发现**：在其他 HTTP 请求包中，发现关键的 `?url` 参数，这暗示了可能存在服务器端请求伪造 (SSRF) 的漏洞。
      - **截图**：
          - [El Bandito 10](./iamges/El_Bandito10.png)
          - [El Bandito 11](./iamges/El_Bandito11.png)

## 第一个 Flag - 漏洞利用阶段

  - **利用 SSRF 访问内部配置**
      - **技术**：利用 `?url` 参数进行 SSRF。首先，使用 Python 搭建一个简单的 HTTP 服务器，该服务器返回 HTTP 状态码 `101` (Switching Protocols)，以误导代理服务器，使其认为 WebSocket 连接已建立。
      - **截图**：[El Bandito 12](./iamges/El_Bandito12.png) (本地 HTTP 服务器)
      - **行动**：通过 SSRF 访问 Spring 框架的 `/env` 配置文件，可能泄露敏感信息。
      - **截图**：[El Bandito 13](./iamges/El_Bandito13.png)
      - **行动**：通过 SSRF 进一步访问 Spring 框架的 `/trace` 路由文件，在该文件中发现了一个凭据以及第一个 flag。
      - **截图**：
          - [El Bandito 14](./iamges/El_Bandito14.png)
          - [El Bandito 15](./iamges/El_Bandito15.png)
          - [El Bandito 16](./iamges/El_Bandito16.png)

## 第二个 Flag - 漏洞探测阶段

  - **分析前端 JavaScript 代码**
      - **发现**：访问 80 端口的 HTTPS 网站，发现一个 JavaScript 文件，该文件实现了两个用户之间的异步通信聊天功能。
      - **截图**：
          - [El Bandito 17](./iamges/El_Bandito17.png)
          - [El Bandito 18](./iamges/El_Bandito18.png)
  - **聊天室交互与协议降级**
      - **行动**：使用已有的凭据登录 `/access` 页面，进入聊天室。抓包分析发现聊天功能使用的是 HTTP/2 协议。
      - **测试**：尝试将访问协议降级为 HTTP/1.1，发现可以成功降级。
      - **截图**：
          - [El Bandito 19](./iamges/El_Bandito19.png)
          - [El Bandito 20](./iamges/El_Bandito20.png)
          - [El Bandito 21](./iamges/El_Bandito21.png)

## 第二个 Flag - 漏洞利用阶段

  - **HTTP/1.1 请求走私测试**
      - **行动**：使用 HTTP/1.1 协议尝试进行请求走私，观察到走私成功。
      - **截图**：[El Bandito 22](./iamges/El_Bandito22.png)
  - **HTTP/2 Web 缓存污染**
      - **技术**：利用 HTTP/2 的特性，通过 Web 缓存污染来获取聊天室内其他用户的聊天记录，从而获取第二个 flag。
      - **截图**：[El Bandito 23](./iamges/El_Bandito23.png)
      - **Payload**：
        ```
        POST / HTTP/2
        Host: elbandito.thm:80
        Cookie: session=eyJ1c2VybmFtZSI6ImhBY2tMSUVOIn0.Zf9h6Q._RuzBukkXUAbWkck_BFx4LA4rcc
        User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
        Content-Length: 0

        POST /send_message HTTP/1.1
        Cookie: session=eyJ1c2VybmFtZSI6ImhBY2tMSUVOIn0.Zf9h6Q._RuzBukkXUAbWkck_BFx4LA4rcc
        Host: elbandito.thm:80
        Content-Length: 900
        Content-Type: application/x-www-form-urlencoded

        data=
        ```