#       Angular8 Ionic4 Video Learning Notes

## 一. new Angular project

### 1. use npm/cnpm install angular-cli

```linux
npm install -g angular/cli   
OR
cnpm install -g angular/cli   //use China mirra
```

### 2. create angular project

```linux
ng new angularProject //include npm i for lib install
```

if don't want use npm i could with  below command:

```linux
ng new angularProject --skip-install   //just new project not install lib 
cnpm install // install lib
```

### 3. start project

```linux
ng serve --open
```



## 二. Project Structure Analysis

![1565783903747](D:\Typora\MKDFiles\media\1565783903747.png)



## 三. ng -g command

- [Angular中文手册][https://www.angular.cn/tutorial/toh-pt1]

### 1. use `ng -g` or `ng generate` show all available commands

![1565784630376](D:\Typora\MKDFiles\media\1565784630376.png)



> ***只有表单数据才能实行MVVM（数据双向绑定）***
>
> ***{{var | json}}: 利用管道符，以json的方式显示对象数据，不然会显示成[object object]***
>
> ***父子组件之间可以互相传值，但是不能调用对方的方法；组件可以调用服务的方法，服务也可以调用服务的方法***
>
> ***ViewChild可以用来获取子组件的实例，调用子组件的方法***

## 四.

