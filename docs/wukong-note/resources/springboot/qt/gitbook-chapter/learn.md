# 上传GitHub pages

## 方法一
项目根目录下创建 docs文件夹，将_book目录下的所有复制过来<br>
或将 _book 目录重命名 docs 目录


## 方法二
> 创建gh-pages 分支
1.进入_book文件夹下
```dash
cd _book
```

2.git初始化
```
git init
```

3.创建gh-pages分支
```
git checkout --orphan gh-pages
```


4.添加文件到暂存区
```
git add .
```


5.添加信息
```
git commit -m "init project"
```


6.设置远程仓库地址

git remote add origin https://github.com/WuKongNotNull/spring-boot-quickstart-wukongnote.git

7.推送项目到 gh-pages分支
```angular2html
git push origin gh-pages

```

