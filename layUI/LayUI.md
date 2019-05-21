# LayUi 说明

## LayUI 底层方法

### 全局配置

​	方法：layui.config(options)

```javascript
layui.config({
  dir: '/res/layui/' //layui.js 所在路径（注意，如果是script单独引入layui.js，无需设定该参数。），一般情况下可以无视
  ,version: false //一般用于更新模块缓存，默认不开启。设为true即让浏览器不缓存。也可以设为一个固定的值，如：201610
  ,debug: false //用于开启调试模式，默认false，如果设为true，则JS模块的节点会保留在页面
  ,base: '' //设定扩展的Layui模块的所在目录，一般用于外部模块扩展
});
```

##模块的定义

​	方法：*layui.define([mods], callback)*

​	通过该方法可定义一个 *Layui模块*。参数mods是可选的，用于声明该模块所依赖的模块。callback即为模块加载完毕的回调函数，它返回一个exports参数，用于输出该模块的接口。

```javascript
layui.define(function(exports){
  //do something
  exports('demo', function(){
    alert('Hello World!');
  });
});
```

​	exports是一个函数，它接受两个参数，第一个参数为模块名，第二个参数为模块接口，当你声明了上述的一个模块后，你就可以在外部使用了，demo就会注册到layui对象下，即可通过 *layui.demo()* 去执行该模块的接口。也可以在定义一个模块的时候，声明该模块所需的依赖，如：

```javascript
layui.define(['layer', 'laypage'], function(exports){
  //do something
  
  exports('demo', function(){
    alert('Hello World!');
  });
});
```

​	***['layer', 'laypage']***传入的模块可以是一个字符串或者形如一个数据形式的多个字符串

### 使用模块

​	方法：*layui.use([mods], callback)*

​	Layui的内置模块并非默认就加载的，他必须在你执行该方法后才会加载。它的参数跟上述的 define方法完全一样。 另外请注意，mods里面必须是一个合法的模块名，不能包含目录。

```javascript
layui.use(['laypage', 'layedit'], function(){
  var laypage = layui.laypage
  ,layedit = layui.layedit;
  
  //do something
});
```

------

# 模块

##Layui.Table表格模块

快速使用**table**模块

1.  在页面上创建一个table元素

```html
<table id="demo"></table>
```

2.  通过 table.render() 对表格进行渲染

```js
<script>
layui.use('table', function(){
  var table = layui.table;
  //第一个实例
  table.render({
    elem: '#demo'
    ,height: 312
    ,url: '/demo/table/user/' //数据接口
    ,page: true //开启分页
    ,cols: [[ //表头
      {field: 'id', title: 'ID', width:80, sort: true, fixed: 'left'}
      ,{field: 'username', title: '用户名', width:80}
      ,{field: 'sex', title: '性别', width:80, sort: true}
      ,{field: 'city', title: '城市', width:80} 
      ,{field: 'sign', title: '签名', width: 177}
      ,{field: 'experience', title: '积分', width: 80, sort: true}
      ,{field: 'score', title: '评分', width: 80, sort: true}
      ,{field: 'classify', title: '职业', width: 80}
      ,{field: 'wealth', title: '财富', width: 135, sort: true}
    ]]
  });
});
</script>
```

### 三种渲染方式

| 方式 | 机制                                                         | 适用场景               |                                                              |
| :--- | ------------------------------------------------------------ | :--------------------- | ------------------------------------------------------------ |
| 01.  | [方法渲染](https://www.layui.com/doc/modules/table.html#methodRender) | 用JS方法的配置完成渲染 | （推荐）无需写过多的 HTML，在 JS 中指定原始元素，再设定各项参数即可。 |
| 02.  | [自动渲染](https://www.layui.com/doc/modules/table.html#autoRender) | HTML配置，自动渲染     | 无需写过多 JS，可专注于 HTML 表头部分                        |
| 03.  | [转换静态表格](https://www.layui.com/doc/modules/table.html#parseTable) | 转化一段已有的表格元素 | 无需配置数据接口，在JS中指定表格元素，并简单地给表头加上自定义属性即可 |

#### 方法渲染

​	方法渲染将表格的参数放置到js中,原始的html中只需要一个容器即可

```html
<table id="demo" lay-filter="test"></table>
<script>
layui.use('table', function(){
var table = layui.table; 
//执行渲染
table.render({
  elem: '#demo' //指定原始表格元素选择器（推荐id选择器）
  ,height: 315 //容器高度
  ,cols: [{}] //设置表头
  //,…… //更多参数参考右侧目录：基本参数选项
});
</script>
```

​	**PS**:table.render()*方法返回一个对象：var tableIns = table.render(options)，可用于对当前表格进行“重载”等操作

#### 自动渲染

​	在一段 table 容器中配置好相应的参数，由 table 模块内部自动对其完成渲染，而无需你写初始的渲染方法。实现自动渲染需要三步:

1.  带有 *class="layui-table"* 的 *<table>* 标签。 

2.  对标签设置属性 *lay-data=""* 用于配置一些基础参数 
3.  在 *<th>* 标签中设置属性*lay-data=""*用于配置表头信息

```html
<table class="layui-table" lay-data="{height:315, url:'/demo/table/user/', page:true, id:'test'}" lay-filter="test">
  <thead>
    <tr>
      <th lay-data="{field:'id', width:80, sort: true}">ID</th>
      <th lay-data="{field:'username', width:80}">用户名</th>
      <th lay-data="{field:'sex', width:80, sort: true}">性别</th>
      <th lay-data="{field:'city'}">城市</th>
      <th lay-data="{field:'sign'}">签名</th>
      <th lay-data="{field:'experience', sort: true}">积分</th>
      <th lay-data="{field:'score', sort: true}">评分</th>
      <th lay-data="{field:'classify'}">职业</th>
      <th lay-data="{field:'wealth', sort: true}">财富</th>
    </tr>
  </thead>
</table>
```

####转换静态表格

```html
<table lay-filter="demo">
  <thead>
    <tr>
      <th lay-data="{field:'username', width:100}">昵称</th>
      <th lay-data="{field:'experience', width:80, sort:true}">积分</th>
      <th lay-data="{field:'sign'}">签名</th>
    </tr> 
  </thead>
  <tbody>
    <tr>
      <td>贤心1</td>
      <td>66</td>
      <td>人生就像是一场修行a</td>
    </tr>
  </tbody>
</table>
<scrip>
	layui.user('table',function(){
    	var table = layui.table;
    	//转换静态表格
		table.init('demo', {
 	 		height: 315 //设置高度
  			,limit: 10 //注意：请务必确保 limit 参数（默认：10）是与你服务端限定的数据条数一致
  			//支持所有基础参数
		}); 
    })
</scrip>
```

###table 模块的基本参数

| 参数           | 类型               | 说明                                                         | 示例值                                                       |
| :------------- | :----------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| elem           | String/DOM         | 指定原始 table 容器的选择器或 DOM，方法渲染方式必填          | "#demo"                                                      |
| cols           | Array              | 设置表头。值是一个二维数组。方法渲染方式必填                 | [详见表头参数](https://www.layui.com/doc/modules/table.html#cols) |
| url（等）      | -                  | 异步数据接口相关参数。其中 url 参数为必填项                  | [详见异步接口](https://www.layui.com/doc/modules/table.html#async) |
| toolbar        | String/DOM/Boolean | 开启表格头部工具栏区域，该参数支持四种类型值：toolbar: '#toolbarDemo' *//指向自定义工具栏模板选择器*toolbar: '<div>xxx</div>' *//直接传入工具栏模板字符*toolbar: true *//仅开启工具栏，不显示左侧模板*toolbar: 'default' *//让工具栏左侧显示默认的内置模板*注意：  1. 该参数为 layui 2.4.0 开始新增。  2. 若需要“列显示隐藏”、“导出”、“打印”等功能，则必须开启该参数 | false                                                        |
| defaultToolbar | Array              | 自由配置头部工具栏右侧的图标，数组支持以下可选值：filter: *显示筛选图标*exports: *显示导出图标*print: *显示打印图标*可根据值的顺序显示排版图标，如：  *defaultToolbar: ['filter', 'print', 'exports']*该参数为 layui 2.4.1 新增 | ['filter', 'print']                                          |
| width          | Number             | 设定容器宽度。table容器的默认宽度是跟随它的父元素铺满，你也可以设定一个固定值，当容器中的内容超出了该宽度时，会自动出现横向滚动条。 | 1000                                                         |
| height         | Number/String      | 设定容器高度                                                 | [详见height](https://www.layui.com/doc/modules/table.html#height) |
| cellMinWidth   | Number             | （layui 2.2.1 新增）全局定义所有常规单元格的最小宽度（默认：60），一般用于列宽自动分配的情况。其优先级低于表头参数中的 minWidth | 100                                                          |
| done           | Function           | 数据渲染完的回调。你可以借此做一些其它的操作                 | [详见done回调](https://www.layui.com/doc/modules/table.html#done) |
| data           | Array              | 直接赋值数据。既适用于只展示一页数据，也非常适用于对一段已知数据进行多页展示。 | [{}, {}, {}, {}, …]                                          |
| totalRow       | Boolean            | 是否开启合计行区域。layui 2.4.0 新增                         | false                                                        |
| page           | Boolean/Object     | 开启分页（默认：false） 注：从 layui 2.2.0 开始，支持传入一个对象，里面可包含 [laypage](https://www.layui.com/doc/modules/laypage.html#options) 组件所有支持的参数（jump、elem除外） | {theme: '#c00'}                                              |
| limit          | Number             | 每页显示的条数（默认：10）。值务必对应 limits 参数的选项。 优先级低于 page 参数中的 limit 参数。 | 30                                                           |
| limits         | Array              | 每页条数的选择项，默认：[10,20,30,40,50,60,70,80,90]。 优先级低于 page 参数中的 limits 参数。 | [30,60,90]                                                   |
| loading        | Boolean            | 是否显示加载条（默认：true）。如果设置 false，则在切换分页时，不会出现加载条。该参数只适用于 url 参数开启的方式 | false                                                        |
| title          | String             | 定义 table 的大标题（在文件导出等地方会用到）layui 2.4.0 新增 | "用户表"                                                     |
| text           | Object             | 自定义文本，如空数据时的异常提示等。注：layui 2.2.5 开始新增。 | [详见自定义文本](https://www.layui.com/doc/modules/table.html#text) |
| autoSort       | Boolean            | 默认 true，即直接由 table 组件在前端自动处理排序。  若为 false，则需自主排序，通常由服务端直接返回排序好的数据。  注意：该参数为 layui 2.4.4 新增 | [详见监听排序](https://www.layui.com/doc/modules/table.html#onsort) |
| initSort       | Object             | 初始排序状态。用于在数据表格渲染完毕时，默认按某个字段排序。 | [详见初始排序](https://www.layui.com/doc/modules/table.html#initSort) |
| id             | String             | 设定容器唯一 id。id 是对表格的数据操作方法上是必要的传递条件，它是表格容器的索引，你在下文诸多地方都将会见识它的存在。   值得注意的是：从 layui 2.2.0 开始，该参数也可以自动从 *<table id="test"></table>* 中的 id 参数中获取。 | test                                                         |
| skin（等）     | -                  | 设定表格各种外观、尺寸等                                     | [详见表格风格](https://www.layui.com/doc/modules/table.html#skin) |

### col表头的参数

| 参数         | 类型          | 说明                                                         | 示例值                                                       |
| :----------- | :------------ | :----------------------------------------------------------- | :----------------------------------------------------------- |
| field        | String        | 设定字段名。字段名的设定非常重要，且是表格数据列的唯一标识   | username                                                     |
| title        | String        | 设定标题名称                                                 | 用户名                                                       |
| width        | Number/String | 设定列宽，若不填写，则自动分配；若填写，则支持值为：数字、百分比  请结合实际情况，对不同列做不同设定。 | 200 30%                                                      |
| minWidth     | Number        | 局部定义当前常规单元格的最小宽度（默认：60），一般用于列宽自动分配的情况。其优先级高于基础参数中的 cellMinWidth | 100                                                          |
| type         | String        | 设定列类型。可选值有：normal（常规列，无需设定）checkbox（复选框列）radio（单选框列，layui 2.4.0 新增）numbers（序号列）space（空列） | 任意一个可选值                                               |
| LAY_CHECKED  | Boolean       | 是否全选状态（默认：false）。必须复选框列开启后才有效，如果设置 true，则表示复选框默认全部选中。 | true                                                         |
| fixed        | String        | 固定列。可选值有：*left*（固定在左）、*right*（固定在右）。一旦设定，对应的列将会被固定在左或右，不随滚动条而滚动。  注意：*如果是固定在左，该列必须放在表头最前面；如果是固定在右，该列必须放在表头最后面。* | left（同 true） right                                        |
| hide         | Boolean       | 是否初始隐藏列，默认：false。layui 2.4.0 新增                | true                                                         |
| totalRow     | Boolean       | 是否开启该列的自动合计功能，默认：false。layui 2.4.0 新增    | true                                                         |
| totalRowText | String        | 用于显示自定义的合计文本。layui 2.4.0 新增                   | "合计："                                                     |
| sort         | Boolean       | 是否允许排序（默认：false）。如果设置 true，则在对应的表头显示排序icon，从而对列开启排序功能。注意：*不推荐对值同时存在“数字和普通字符”的列开启排序，因为会进入字典序比对*。比如：*'贤心' > '2' > '100'*，这可能并不是你想要的结果，但字典序排列算法（ASCII码比对）就是如此。 | true                                                         |
| unresize     | Boolean       | 是否禁用拖拽列宽（默认：false）。默认情况下会根据列类型（type）来决定是否禁用，如复选框列，会自动禁用。而其它普通列，默认允许拖拽列宽，当然你也可以设置 true 来禁用该功能。 | false                                                        |
| edit         | String        | 单元格编辑类型（默认不开启）目前只支持：*text*（输入框）     | text                                                         |
| event        | String        | 自定义单元格点击事件名，以便在 [tool](https://www.layui.com/doc/modules/table.html#ontool) 事件中完成对该单元格的业务处理 | 任意字符                                                     |
| style        | String        | 自定义单元格样式。即传入 CSS 样式                            | background-color: #5FB878; color: #fff;                      |
| align        | String        | 单元格排列方式。可选值有：*left*（默认）、*center*（居中）、*right*（居右） | center                                                       |
| colspan      | Number        | 单元格所占列数（默认：1）。一般用于多级表头                  | 3                                                            |
| rowspan      | Number        | 单元格所占行数（默认：1）。一般用于多级表头                  | 2                                                            |
| templet      | String        | 自定义列模板，模板遵循 [laytpl](https://www.layui.com/doc/modules/laytpl.html) 语法。这是一个非常实用的功能，你可借助它实现逻辑处理，以及将原始数据转化成其它格式，如时间戳转化为日期字符等 | [详见自定义模板](https://www.layui.com/doc/modules/table.html#templet) |
| toolbar      | String        | 绑定列工具条。设定后，可在每行列中出现一些自定义的操作性按钮 | [详见列工具条](https://www.layui.com/doc/modules/table.html#toolbar) |

### templet自定义列模板

​	在默认情况下，单元格的内容是完全按照数据接口返回的content原样输出的，如果你想对某列的单元格添加链接等其它元素，你可以借助该参数来轻松实现。这是一个非常实用且强大的功能，你的表格内容会因此而丰富多样。

#### 自定义列模板使用方式

1.  **绑定模版选择器。**

```html
table.render({
  cols: [[
    {field:'title', title: '文章标题', width: 200, templet: '#titleTpl'} //这里的templet值是模板元素的选择器
    ,{field:'id', title:'ID', width:100}
  ]]
});
 
等价于：
<th lay-data="{field:'title', width: 200, templet: '#titleTpl'}">文章标题</th>
<th lay-data="{field:'id', width:100}">ID</th>
```

模板内容:

```html
<script type="text/html" id="titleTpl">
  <a href="/detail/{{d.id}}" class="layui-table-link">{{d.title}}</a>
</script>
 
注意：上述的 {{d.id}}、{{d.title}} 是动态内容，它对应数据接口返回的字段名。除此之外，你还可以读取到以下额外字段：
     序号：{{ d.LAY_INDEX }} （该额外字段为 layui 2.2.0 新增）
 
由于模板遵循 laytpl 语法（建议细读 laytpl文档 ），因此在模板中你可以写任意脚本语句（如 if else/for等）：
<script type="text/html" id="titleTpl">
  {{#  if(d.id < 100){ }}
    <a href="/detail/{{d.id}}" class="layui-table-link">{{d.title}}</a>
  {{#  } else { }}
    {{d.title}}(普通用户)
  {{#  } }}
</script>
```

2.  **函数转义**

    templet 开始支持函数形式，函数返回一个参数 d，包含接口返回的所有字段和数据。

```javascript
table.render({
  cols: [[
    {field:'title', title: '文章标题', width: 200
      ,templet: function(d){
        return 'ID：'+ d.id +'，标题：<span style="color: #c00;">'+ d.title +'</span>'
      }
    }
    ,{field:'id', title:'ID', width:100}
  ]]
}); 
```

3.  **直接赋值模版字符**

```html
templet: '<div><a href="/detail/{{d.id}}" class="layui-table-link">{{d.title}}</a></div>'
 
注意：这里一定要被一层 <div></div> 包裹，否则无法读取到模板
```

### toolbar 工具条

####说明

​	为表格添加增加,修改,删除的操作按钮

```html
table.render({
  cols: [[
    {field:'id', title:'ID', width:100}
    ,{fixed: 'right', width:150, align:'center', toolbar: '#barDemo'} //这里的toolbar值是模板元素的选择器
  ]]
});
 
等价于：
<th lay-data="{field:'id', width:100}">ID</th>
<th lay-data="{fixed: 'right', width:150, align:'center', toolbar: '#barDemo'}"></th>
```

toolbar 模板:

```html
<script type="text/html" id="barDemo">
  <a class="layui-btn layui-btn-xs" lay-event="detail">查看</a>
  <a class="layui-btn layui-btn-xs" lay-event="edit">编辑</a>
  <a class="layui-btn layui-btn-danger layui-btn-xs" lay-event="del">删除</a>
  
  <!-- 这里同样支持 laytpl 语法，如： -->
  {{#  if(d.auth > 2){ }}
    <a class="layui-btn layui-btn-xs" lay-event="check">审核</a>
  {{#  } }}
</script>
 
注意：属性 lay-event="" 是模板的关键所在，值可随意定义。
```

#### 使用toolbar工具条

```javascript
//监听工具条
table.on('tool(test)', function(obj){ //注：tool是工具条事件名，test是table原始容器的属性 lay-filter="对应的值"
  var data = obj.data; //获得当前行数据
  var layEvent = obj.event; //获得 lay-event 对应的值（也可以是表头的 event 参数对应的值）
  var tr = obj.tr; //获得当前行 tr 的DOM对象
 
  if(layEvent === 'detail'){ //查看
    //do somehing
  } else if(layEvent === 'del'){ //删除
    layer.confirm('真的删除行么', function(index){
      obj.del(); //删除对应行（tr）的DOM结构，并更新缓存
      layer.close(index);
      //向服务端发送删除指令
    });
  } else if(layEvent === 'edit'){ //编辑
    //do something
    
    //同步更新缓存对应的值
    obj.update({
      username: '123'
      ,title: 'xxx'
    });
  }
});
```

### 异步数据接口

#### 异步数据接口参数

| 参数名      | 功能                                                         |
| :---------- | :----------------------------------------------------------- |
| url         | 接口地址。默认会自动传递两个参数：*?page=1&limit=30*（该参数可通过 request 自定义）  *page* 代表当前页码、*limit* 代表每页数据量 |
| method      | 接口http请求类型，默认：get                                  |
| where       | 接口的其它参数。如：*where: {token: 'sasasas', id: 123}*     |
| contentType | 发送到服务端的内容编码类型。如果你要发送 json 内容，可以设置：*contentType: 'application/json'* |
| headers     | 接口的请求头。如：*headers: {token: 'sasasas'}*              |

**parseData**:

​	数据预解析回调函数，用于将返回的任意数据格式解析成 table 组件规定的数据格式

```javascript
table.render({
  elem: '#demp'
  ,url: ''
  ,parseData: function(res){ //res 即为原始返回的数据
    return {
      "code": res.status, //解析接口状态
      "msg": res.message, //解析提示文本
      "count": res.total, //解析数据长度
      "data": res.data.item //解析数据列表
    };
  }
  //,…… //其他参数
});    
```

**request**:

​	用于对分页请求的参数：page、limit重新设定名称

```javascript
table.render({
  elem: '#demp'
  ,url: ''
  ,request: {
    pageName: 'curr' //页码的参数名称，默认：page
    ,limitName: 'nums' //每页数据量的参数名，默认：limit
  }
  //,…… //其他参数
});  
```

**response**:

​	新规定返回的数据格式，那么可以借助 response 参数

```javascript
table.render({
  elem: '#demp'
  ,url: ''
  ,response: {
    statusName: 'status' //规定数据状态的字段名称，默认：code
    ,statusCode: 200 //规定成功的状态码，默认：0
    ,msgName: 'hint' //规定状态信息的字段名称，默认：msg
    ,countName: 'total' //规定数据总数的字段名称，默认：count
    ,dataName: 'rows' //规定数据列表的字段名称，默认：data
  } 
  //,…… //其他参数
});    
```

#### 调用实例

```javascript
//“方法级渲染”配置方式
table.render({ //其它参数在此省略
  url: '/api/data/'
  //where: {token: 'sasasas', id: 123} //如果无需传递额外参数，可不加该参数
  //method: 'post' //如果无需自定义HTTP类型，可不加该参数
  //request: {} //如果无需自定义请求参数，可不加该参数
  //response: {} //如果无需自定义数据响应名称，可不加该参数
}); 
 
等价于（“自动化渲染”配置方式）
<table class="layui-table" lay-data="{url:'/api/data/'}" lay-filter="test"> …… </table>
```

### 渲染后回调

​	无论是异步请求数据，还是直接赋值数据，都会触发该回调。可以利用该回调做一些表格以外元素的渲染。

```javascript
table.render({ //其它参数在此省略
  done: function(res, curr, count){
    //如果是异步请求数据方式，res即为你接口返回的信息。
    //如果是直接赋值的方式，res即为：{data: [], count: 99} data为当前页数据、count为数据总长度
    console.log(res);
    
    //得到当前页码
    console.log(curr); 
    
    //得到数据总量
    console.log(count);
  }
});
```

### Table基本用法

​	基础用法是table模块的关键组成部分，目前所开放的所有方法如下：

```javascript
> table.set(options); //设定全局默认参数。options即各项基础参数
> table.on('event(filter)', callback); //事件监听。event为内置事件名（详见下文），filter为容器lay-filter设定的值
> table.init(filter, options); //filter为容器lay-filter设定的值，options即各项基础参数。例子见：转换静态表格
> table.checkStatus(id); //获取表格选中行（下文会有详细介绍）。id 即为 id 参数对应的值
> table.render(options); //用于表格方法级渲染，核心方法。应该不用再过多解释了，详见：方法级渲染
> table.reload(id, options); //表格重载
> table.resize(id); //重置表格尺寸
> table.exportFile(id, data, type); //导出数据
```

#### 获取选中行

​	该方法可获取到表格所有的选中行相关数据 

​	语法:*table.checkStatus('ID')*，其中 *ID* 为基础参数 id 对应的值

```javascript
【自动化渲染】
<table class="layui-table" lay-data="{id: 'idTest'}"> …… </table>
 
【方法渲染】
table.render({ //其它参数省略
  id: 'idTest'
});

var checkStatus = table.checkStatus('idTest'); //idTest 即为基础参数 id 对应的值 
console.log(checkStatus.data) //获取选中行的数据
console.log(checkStatus.data.length) //获取选中行数量，可作为是否有选中行的条件
console.log(checkStatus.isAll ) //表格是否全选
```

#### 表格重载

​	很多时候，你需要对表格进行重载。比如数据全局搜索。以下方法可以帮你轻松实现这类需求（可任选一种）

| 语法                      | 说明                                                         | 适用场景       |
| ------------------------- | ------------------------------------------------------------ | -------------- |
| table.reload(ID, options) | 参数 *ID* 即为基础参数id对应的值，见：[设定容器唯一ID](https://www.layui.com/doc/modules/table.html#id)  参数 *options* 即为各项基础参数  注意：*该方法为 2.1.0 版本中新增* | 所有渲染方式   |
| tableIns.reload(options)  | 对象 *tableIns* 来源于 table.render() 方法的实例  参数 *options* 即为各项基础参数 | 仅限方法级渲染 |

##### 用法示例

1.  自动渲染重载

```javascript
【HTML】
<table class="layui-table" lay-data="{id: 'idTest'}"> …… </table>
 
【JS】
table.reload('idTest', {
  url: '/api/table/search'
  ,where: {} //设定异步数据接口的额外参数
  //,height: 300
});
```

2.  方法渲染重载

```javascript
//所获得的 tableIns 即为当前容器的实例
var tableIns = table.render({
  elem: '#id'
  ,cols: [] //设置表头
  ,url: '/api/data' //设置异步接口
  ,id: 'idTest'
}); 
 
//这里以搜索为例
tableIns.reload({
  where: { //设定异步数据接口的额外参数，任意设
    aaaaaa: 'xxx'
    ,bbb: 'yyy'
    //…
  }
  ,page: {
    curr: 1 //重新从第 1 页开始
  }
});
//上述方法等价于
table.reload('idTest', {
  where: { //设定异步数据接口的额外参数，任意设
    aaaaaa: 'xxx'
    ,bbb: 'yyy'
    //…
  }
  ,page: {
    curr: 1 //重新从第 1 页开始
  }
});
```

#### 数据导出

​	尽管 table 的工具栏内置了数据导出按钮，但有时你可能需要通过方法去导出任意数据，那么可以借助以下方法： 
​	语法：*table.exportFile(id, data, type)*

```javascript
var ins1 = table.render({
  elem: '#demo'
  ,id: 'test'
  //,…… //其它参数
})      
      
//将上述表格示例导出为 csv 文件
table.exportFile(ins1.config.id, data); //data 为该实例中的任意数量的数据
```

#### 事件监听

​	语法：*table.on('event(filter)', callback);* 注：event为内置事件名，filter为容器lay-filter设定的值 ,table模块在Layui事件机制中注册了专属事件,注意不要占用table名。示例:

```javascript
//以复选框事件为例
table.on('checkbox(test)', function(obj){
  console.log(obj)
});
```

##### 监听头部工具栏(左侧)

​	点击头部工具栏区域设定了属性为 *lay-event=""* 的元素时触发,配合工具栏模板使用

```javascript
原始容器
<table id="demo" lay-filter="test"></table>
 
工具栏模板：
<script type="text/html" id="toolbarDemo">
  <div class="layui-btn-container">
    <button class="layui-btn layui-btn-sm" lay-event="add">添加</button>
    <button class="layui-btn layui-btn-sm" lay-event="delete">删除</button>
    <button class="layui-btn layui-btn-sm" lay-event="update">编辑</button>
  </div>
</script>
 
<script;>
//JS 调用：
table.render({
  elem: '#demo'
  ,toolbar: '#toolbarDemo'
  //,…… //其他参数
});
 
//监听事件
table.on('toolbar(test)', function(obj){
  var checkStatus = table.checkStatus(obj.config.id);
  switch(obj.event){
    case 'add':
      layer.msg('添加');
    break;
    case 'delete':
      layer.msg('删除');
    break;
    case 'update':
      layer.msg('编辑');
    break;
  };
});
</script>
```

##### 监听复选框

​	点击复选框时触发，回调函数返回一个object参数，携带的成员如下

```javascript
table.on('checkbox(test)', function(obj){
  console.log(obj.checked); //当前是否选中状态
  console.log(obj.data); //选中行的相关数据
  console.log(obj.type); //如果触发的是全选，则为：all，如果触发的是单选，则为：one
});
```

##### 监听单元格编辑

​	单元格被编辑，且值发生改变时触发，回调函数返回一个object参数，携带的成员如下：

```javascript
table.on('edit(test)', function(obj){ //注：edit是固定事件名，test是table原始容器的属性 lay-filter="对应的值"
  console.log(obj.value); //得到修改后的值
  console.log(obj.field); //当前编辑的字段名
  console.log(obj.data); //所在行的所有相关数据  
});
```

##### 监听双击事件

​	点击或双击时触发

```javascript
//监听行单击事件
table.on('row(test)', function(obj){
  console.log(obj.tr) //得到当前行元素对象
  console.log(obj.data) //得到当前行数据
  //obj.del(); //删除当前行
  //obj.update(fields) //修改当前行数据
});
 
//监听行双击事件
table.on('rowDouble(test)', function(obj){
  //obj 同上
});
```

------

## Layui.form表单模块

### 说明

​	layui 针对各种表单元素做了较为全面的UI支持，你无需去书写那些 UI 结构，你只需要写 HTML 原始的 input、select、textarea 这些基本的标签即可。我们在 UI 上的渲染只要求一点，你必须给表单体系所在的父元素加上*class="layui-form"*，一切的工作都会在你加载完form模块后，自动完成。

**实例**

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>layui.form小例子</title>
<link rel="stylesheet" href="layui.css" media="all">
</head>
<body>
<form class="layui-form"> <!-- 提示：如果你不想用form，你可以换成div等任何一个普通元素 -->
  <div class="layui-form-item">
    <label class="layui-form-label">输入框</label>
    <div class="layui-input-block">
      <input type="text" name="" placeholder="请输入" autocomplete="off" class="layui-input">
    </div>
  </div>
  <div class="layui-form-item">
    <label class="layui-form-label">下拉选择框</label>
    <div class="layui-input-block">
      <select name="interest" lay-filter="aihao">
        <option value="0">写作</option>
        <option value="1">阅读</option>
      </select>
    </div>
  </div>
  <div class="layui-form-item">
    <label class="layui-form-label">复选框</label>
    <div class="layui-input-block">
      <input type="checkbox" name="like[write]" title="写作">
      <input type="checkbox" name="like[read]" title="阅读">
    </div>
  </div>
  <div class="layui-form-item">
    <label class="layui-form-label">开关关</label>
    <div class="layui-input-block">
      <input type="checkbox" lay-skin="switch">
    </div>
  </div>
  <div class="layui-form-item">
    <label class="layui-form-label">开关开</label>
    <div class="layui-input-block">
      <input type="checkbox" checked lay-skin="switch">
    </div>
  </div>
  <div class="layui-form-item">
    <label class="layui-form-label">单选框</label>
    <div class="layui-input-block">
      <input type="radio" name="sex" value="0" title="男">
      <input type="radio" name="sex" value="1" title="女" checked>
    </div>
  </div>
  <div class="layui-form-item layui-form-text">
    <label class="layui-form-label">请填写描述</label>
    <div class="layui-input-block">
      <textarea placeholder="请输入内容" class="layui-textarea"></textarea>
    </div>
  </div>
  <div class="layui-form-item">
    <div class="layui-input-block">
      <button class="layui-btn" lay-submit lay-filter="*">立即提交</button>
      <button type="reset" class="layui-btn layui-btn-primary">重置</button>
    </div>
  </div>
  <!-- 更多表单结构排版请移步文档左侧【页面元素-表单】一项阅览 -->
</form>
<script src="layui.js"></script>
<script>
layui.use('form', function(){
  var form = layui.form;
  
  //各种基于事件的操作，下面会有进一步介绍
});
</script>
</body>
</html>
```

