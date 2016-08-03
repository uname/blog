# blog

Windows 下Hexo环境与文章发布指引

0. 默认已经安装了Git
1. clone该项目到本地:
git clone https://github.com/uname/blog.git

2. 若未安装node.js，则先安装之。[下载地址](https://nodejs.org/zh-cn/)
3. 安装hexo博客框架，中文官网: https://hexo.io/zh-cn/
npm install hexo-cli -g
如有需要为npm设置代理，可参考: http://www.cnblogs.com/huang0925/archive/2013/05/17/3083207.html
HTTP代理设置方法：npm config set proxy http://server:port

4. 执行 npm install 安装模块
5. 搞定后如果直接在命令行输入hexo s 提示无hexo命令，则将当前目录下的node_modules\.bin填入PATH环境变量
