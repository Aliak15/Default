```bash

npm search <package-name> --registry=https://registry.npmmirror.com/
npm config set registry https://registry.npmmirror.com/
npm config get registry

npm install -g <package-name>
npm uninstall -g <package-name>@<version>
npm root -g  # 查看全局安装包的位置
npm list [-g] # 列出已安装
npm install  # 根据 package.json 中列出的依赖包来安装项目所需的所有node_modules依赖
rm -rf node_modules  # 删除依赖
npm run build  # 打包项目, 会生成dist文件夹/ js和map文件

```


