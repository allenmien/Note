npm升级

```
sudo npm install npm -g
```

npm本地安装

```
npm install express  
```

npm全局安装

```
npm install express -g
```

package.json

切换到项目目录下

```
npm install 
或npm install --save-dev
```

npm淘宝镜像

临时使用

```
npm --registry https://registry.npm.taobao.org install express
```

持久使用

```
npm config set registry https://registry.npm.taobao.org
npm config get registry
```

cnpm

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
cnpm install commander
```

