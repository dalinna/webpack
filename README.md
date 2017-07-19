# webpack
第一个次用webpack进行打包，分享给初学打包的你，希望共同进步


正式使用Webpack前的准备

新建一个空的练习文件夹（此处命名为webpackageExercise）,在该文件夹中创建一个package.json文件，这是一个标准的npm说明文件，包括当前项目的依赖模块，自定义的脚本任务等等。
在终端中使用npm init命令可以自动创建这个package.json文件

npm init

输入这个命令后，终端会问你一系列诸如项目名称，项目描述，作者等信息，不过不用担心，如果你不准备在npm中发布你的模块，这些问题的答案都不重要，回车默认即可。

package.json文件已经就绪，我们在本项目中安装Webpack作为依赖包

//全局安装
npm install -g webpack
//安装到你的项目目录
npm install --save-dev webpack
回到之前的空文件夹，并在里面创建两个文件夹,app文件夹和public文件夹，app文件夹用来存放原始数据和我们将写的JavaScript模块，public文件夹用来存放准备给浏览器读取的数据（包括使用webpack生成的打包后的js文件以及一个index.html文件）。在这里还需要创建三个文件，index.html 文件放在public文件夹中，两个js文件（Greeter.js和main.js）放在app文件夹中

index.html文件只有最基础的html代码，它唯一的目的就是加载打包后的js文件（bundle.js）

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Webpack Sample Project</title>
  </head>
  <body>
    <div id='root'>
    </div>
    <script src="bundle.js"></script>
  </body>
</html>

Greeter.js只包括一个用来返回包含问候信息的html元素的函数。

// Greeter.js
module.exports = function() {
  var greet = document.createElement('div');
  greet.textContent = "Hi there and greetings!";
  return greet;
};


main.js用来把Greeter模块返回的节点插入页面。

//main.js 
var greeter = require('./Greeter.js');
document.getElementById('root').appendChild(greeter());

通过配置文件来使用Webpack
定义一个配置文件，这个配置文件其实也是一个简单的JavaScript模块，可以把所有的与构建相关的信息放在里面。

在当前练习文件夹的根目录下新建一个名为webpack.config.js的文件，并在其中进行最最简单的配置，如下所示，它包含入口文件路径和存放打包后文件的地方的路径。



module.exports = {
  entry:  __dirname + "/app/main.js",//已多次提及的唯一入口文件
  output: {
    path: __dirname + "/public",//打包后的文件存放的地方
    filename: "bundle.js"//打包后输出文件的文件名
  },
  devServer: {
    contentBase: './public',//默认webpack-dev-server会为根文件夹提供本地服务器，如果想为另外一个目录下的文件提供本地服务器，应该在这里设置其所在目录（本例设置到"build"目录）
    inline: true,
    port: 3333
  }
}
进行如下配置后可以使用简单的npm start命令来起服务。在package.json中对npm的脚本部分进行相关设置即可，设置方法如下。

{
  "name": "webpackageExercise",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "dev": "webpack",
    "start": "webpack-dev-server" 
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^3.3.0"
  }
}

Webpack可以用提供一个可选的本地开发服务器，让你的浏览器监测你的代码的修改，并自动刷新修改后的结果，这个本地服务器基于node.js构建，不过它是一个单独的组件，在webpack中进行配置之前需要单独安装它作为项目依赖

npm install --save-dev webpack-dev-server



