# 包
- 一个包是由很多模块组成。
- 在一个程序中可以引入多个包。
- Nodejs中第三方模块由包组成，可以通过包来对一组具有相互依赖关系的模块进行统一管理。
- 在CommonJS中的包目录：
   - package.json :包描述文件。
   - bin :用于存放可执行二进制文件的目录。
   - lib :用于存放 JavaScript 代码的目录。
   - doc :用于存放文档的目录
- 在nodejs中可以通过npm命令来下载第三方的模块（包）。

# npm
- npm 是随同 NodeJS 一起安装的包管理工具。
- npm命令详解
   1. npm -v 查看 npm 版本。
   2. 使用 npm 命令安装模块（npm install Module Name）。
   3. npm uninstall moudleName 卸载模块。
   4. npm list 查看当前目录下已安装的 node 包。
   5. npm info 模块查看模块的版本。
   6. 指定版本安装，例如npm install jquery@1.8.0。
   6. npm i //安装缺失的模块
   
# package.json
- package.json定义了这个项目所需要的各种模块,以及项目的配置信息(比如名称、版本、 许可证等元数据)
- 安装package.json 使用npm init 或者 npm init –yes
- 安装模块并把模块写入 package.json(依赖)中的方法：
   1. npm install 模块 --save
   2. npm install 模块 --save-dev
- package.json中dependencies 与 devDependencies 之间的区别：
   - 使用 npm install node_module –save 自动更新 dependencies 字段值; 
   - 使用 npm install node_module –save-dev 自动更新 devDependencies 字段值; 
   - dependencie 配置当前程序所依赖的其他包。 
   - devDependencie 配置当前程序所依赖的其他包，比如一些工具之类的配置在这里
   
