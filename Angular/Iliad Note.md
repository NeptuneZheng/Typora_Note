# 		Iliad Angular + Springboot note

## Ⅰ . Import layui to Web

- [如何在 Angular4或5 中使用 layui 或者 layer][https://fly.layui.com/jie/25431/]
- [angular7整合layui框架，实现layer功能的解决办法][https://blog.csdn.net/qq_34309663/article/details/89370858]

### 1. install jQuery and restart app

```javascript
npm install jquery
```

### 2. [downlaod layui][https://www.layui.com/]

### 3. unzip and copy to  src/assets/

![1564994998463](D:\Typora\MKDFiles\media\1564994998463.png)



### 4. import layui to index.html

```html
<head>
  <link rel="stylesheet" href="../src/assets/layui/css/layui.css"  media="all">
  <script src="../src/assets/layui/layui.js"></script>

  <script>
    //注意：导航 依赖 element 模块，否则无法进行功能性操作
    layui.use(['element','form','layer','layedit','laydate','upload','table'], function(){
      var element = layui.element,
        form = layui.form,
        layer = parent.layer === undefined ? layui.layer : top.layer,
        laypage = layui.laypage,
        upload = layui.upload,
        layedit = layui.layedit,
        laydate = layui.laydate,
        $ = layui.jquery,
        table = layui.element;
    });
  </script>
</head>
```

## 2. localStorage VS sessionStorage VS Cookie   and usage

- [localStorage、sessionStorage、Cookie的区别及用法][https://segmentfault.com/a/1190000012057010]
- [说说localStorage](https://www.cnblogs.com/jinguangguo/p/4083919.html)
- [JS 详解 Cookie、 LocalStorage 与 SessionStorage](https://www.cnblogs.com/minigrasshopper/p/8064367.html)
- [彻底理解cookie，session，token][https://www.liangzl.com/get-article-detail-16019.html]

webstorage是本地存储，存储在客户端，包括localStorage和sessionStorage。

| Storage TYPE   | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| localStorage   | **存储格式**: 仅仅存储字符串类型的对象（虽然规范中可以存储其他原生类型的对象，但是目前为止没有浏览器对其进行实现）<br/>**生命周期**: localStorage生命周期是永久,不是缓存级别的,本身不带时效<br/>**存储位置**: 存在于本地的文件系统中,它仅在客户端（即浏览器）中保存，不参与和服务器的通信。<br/>**数据共享**: 不同浏览器无法共享localStorage或sessionStorage中的信息。相同浏览器的不同页面间可以共享相同的localStorage（页面属于相同域名和端口）<br/>**存储大小**: 对于HTML5的localStorage而言，其大小支持为5M（当然，各浏览器的大小差异还是有的）<br/>**应用场景**: 建议不要使用localStorage方式存储敏感信息，哪怕这些信息进行过加密。 |
| sessionStorage | **存储格式**: 与localStorage相似<br/>**生命周期**: 仅在当前会话下有效，关闭页面或浏览器后被清除<br/>**存储位置**: 仅在客户端（即浏览器）中保存，不参与和服务器的通信。<br/>**数据共享**: 和localStorage相似<br/>**存储大小**: 大小为一般为5MB |
| Cookie         | **存储格式**: K/V存储<br/>**生命周期**: 一般由服务器生成，可设置失效时间。如果在浏览器端生成Cookie，默认是关闭浏览器后失效<br/>**存储位置**: 每次都会携带在HTTP头中，如果使用cookie保存过多数据会带来性能问题<br/>**数据共享**: Cookie具有不可跨域名性,浏览器判断一个网站是否能操作另一个网站Cookie的依据是域名。<br/>**存储大小**: 大小为一般为4K，有个数限制（各浏览器不同），一般不能超过20个 |

![1565169319123](D:\Typora\MKDFiles\media\1565169319123.png)

