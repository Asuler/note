# Vscode 版调试

## 简单项目的调试

简单项目启动方式比较简单，直接 node xxx.js 就能启动

需要先创建一个`.vscode/launch.json`的调试配置文件

![[assets/Pasted image 20250902211109.png]]

点击创建 launch.json 文件后，有可能会出现点了没反应，下面一直显示`激活拓展中`这种文案，这种情况下需要关掉其他的 vscode 插件

然后选择 nodejs

![[assets/Pasted image 20250902212013.png]]

因为启动是 node xxx.js，所以直接选择 nodejs:启动程序

![[assets/Pasted image 20250902212059.png]]

## npm 项目的调试

通过 npm 命令启动的项目，则选择通过 npm 启动

![[assets/Pasted image 20250902223541.png]]

这个时候生成的配置有两个

![[assets/Pasted image 20250902223846.png]]

第一个 Launch via NPM 才是我们对应的

```json
"runtimeArgs": [
	"run-script",
	"debug"
],
```

然后把这里的`runtimeArgs` 改成对应的项目里的 npm 启动命令

我这个项目里启动时 `npm run dev`，所以改成

```json
"runtimeArgs": [
	"run",
	"dev"
],
```

再把左上角的下拉框选择成 `Launch via NPM`后点击播放按钮即可

## 已经启动的项目

要调试已经启动的项目，选择 Node.js: 附加 (attach)，此时会发现生成的文件多了个 port 参数

![[assets/Pasted image 20250902224737.png]]

已经在运行的程序，需要把对应的信息输出给你，所以就需要一个端口来通信

同样的，这里选择 Attach，再点击运行

![[assets/Pasted image 20250902224848.png]]

光是这边监听 9229 端口还不行，原本运行着的 nodejs 项目必须重启，并且以 --inspect-brk 调试模式启动

比如 `node --inspect-brk=9229 index.js` （--inspect 参数必须紧跟着 node 后面）
如果不是通过 node 启动的，那么可以`npm run dev -- --inspect-brk=9229`
这里的 `-- --inspect-brk=9229` 参数只是往 dev 里再传一层，如果里面还套了一层的话，就没办法了

原项目启动了后会弹出一行 ws 的地址
![[assets/Pasted image 20250902233316.png]]

那这个时候再启动 launch.json 的 attach，就能连接上了

## 调试 nodejs 项目源码

直接在代码左侧单击，打上红点，然后调试模式运行
这里在 6，7 行打上断点，然后选择你创建的模式，点击播放按钮，就启动断点了

![[assets/Pasted image 20250902224446.png]]

启动之后我们随便访问一下这个接口，就出来了

![[assets/Pasted image 20250902233601.png]]

## 调试 node_modules 里代码
