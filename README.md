# 3x_ui2.8.7
该版本库是基于原版2.8.7 进行克隆版本，如果需要做开发，可以按下面的步骤进行配置



开发环境搭建步骤准备系统和工具  

一台 Linux 服务器或本地开发机（推荐 Ubuntu/Debian/CentOS 等，Go 开发环境即可）。  
安装 Go 语言环境（版本 1.20+ 推荐，项目 go.mod 指定了要求）：
从官网 https://go.dev/dl/ 下载最新版，或用命令（Ubuntu 示例）：  

sudo apt update
sudo apt install golang-go

检查：go version（确保 >=1.20）。  
安装 Git（sudo apt install git）。  
可选：安装 Xray 核心（开发时需要测试代理功能）：可以手动下载或在项目运行后自动处理。

克隆源码  

git clone https://github.com/MHSanaei/3x-ui.git
cd 3x-ui

下载依赖  

go mod tidy

这会拉取所有 Go 依赖（包括 Xray 相关库等）。
构建和运行（开发模式）  直接运行（推荐开发时用）：  

go run .

或编译成可执行文件（更稳定）：  

go build -o 3x-ui main.go   # 或直接 go build
./3x-ui

第一次运行会初始化数据库（bin/x-ui.db，使用 sqlite），并提示设置管理员账号/密码/面板端口（默认 2053）。  
运行后访问 http://你的IP:面板端口 登录（用户名/密码是你设置的）。  
修改代码后，重新 go run . 或重启二进制即可看到效果。

修改 UI（前端定制）  前端文件在 web/ 目录下：  web/html/：HTML 模板文件（多页面形式）。  
web/assets/：CSS、JS、图片等静态资源。  
web/controller/、web/service/ 等：Go 代码处理后端 API 和渲染逻辑。

直接编辑 HTML/JS/CSS 文件（比如改 dashboard 页面、添加按钮、改样式）。  
因为没有 npm/Vite 等构建工具，是纯静态 + Go 渲染，所以改完直接重启程序生效，无需额外构建前端。  
如果想加新页面或大改 UI：  在 web/controller/ 加新路由。  
在 web/html/ 加对应 HTML。  
在 Go 代码中注册 handler。

测试和调试  

运行后查看终端日志（有错误会打印）。  
想测试 Xray 功能：登录面板后添加入站（inbound），配置协议，生成客户端链接，用 V2rayN/Clash 等客户端连测试。  
开发时可加 debug 日志：在 Go 代码中用 logger 包打印。  
想模拟生产：用 systemd 或 supervisor 跑 ./3x-ui。

打包自定义版本（可选，部署用）  

改完代码后：go build -o custom-3x-ui  
打包整个项目（包括 web/、bin/ 等）：可以写简单脚本或直接 tar 压缩分发。  
注意：如果加新依赖，记得 go mod tidy 并 commit。

注意事项

项目是 GPL-3.0 许可，修改后可自用/分发，但要遵守开源要求。  
定制开发前建议先 fork 仓库到自己 GitHub，方便后续 PR 或同步上游更新。  
如果你只是小改 UI（如换 logo、改文字），可以直接在一键安装后编辑文件（通常在 /usr/local/x-ui/bin/web/ 或类似路径），但推荐从源码做大改。  
常见问题：端口冲突、防火墙挡住、Xray 内核版本不匹配（项目会自动更新 Xray）。  
想加复杂功能（如新协议支持）：需深入 Xray 文档 + 修改 xray/ 目录代码。


