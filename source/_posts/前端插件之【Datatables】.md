---
title: 前端功能之【Datatables插件】
categories:
- 前端功能实现
tags:
- 前端功能实现
---

Datatables 功能插件的使用详细讲解！

<!--more-->

### 一、

#### 1、引用插件

> 首先引入 Datatables 插件、Bootstrap、jQuery 插件。

```javascript
<!--DataTables js-->
<script src="assets/plugins/Datatable/js/jquery.dataTables.js"></script>
<script src="assets/plugins/Datatable/js/dataTables.bootstrap4.js"></script>
```



#### 2、HTML 表格

> 在界面添加 HTML 表格代码。

```html
<table id="example">
    <!-- 行标题 -->
    <thead>
        <tr>
            <!-- 每列的标题 -->
            <th class="wd-15p">权限</th>
            <th class="wd-15p">登陆号</th>
            <th class="wd-20p">密码</th>
            <th class="wd-15p">操作</th>
        </tr>
    </thead>
    <!-- 表格内容 -->
    <tbody>
        <tr>
            <td>Bella</td>
            <td>Chloe</td>
            <td>System Developer</td>
            <td>2018/03/12</td>
        </tr>
    </tbody>
</table>
```



#### 3、Datatables 语言设置

> Datatables 默认的语言设置是英文，我们要将其设置为中文。在 `$(document).render(function(){})` `HTML` 结构渲染完进行设置。

```javascript
$(document).ready(()=>{
	let table = $('#example').DataTable({
        // Datatable 语言设置
        'language': {
            // 左上角的显示数量设置
            "lengthMenu": '每页显示<select class="form-control">'
            + '<option value="10">10</option>'
            + '<option value="20">20</option>'
            + '<option value="30">30</option>'
            + '<option value="40">40</option>'
            + '<option value="50">50</option>' + '</select>条',
            // 右下角的翻页设置
            "paginate": {
                "first": "首页",
                "last": "尾页",
                "previous": "上一页",
                "next": "下一页"
            },
            "processing": "加载中...",  // DataTables载入数据时，是否显示进度条 
            "emptyTable": "暂无数据",   // 表格无数据时显示情况
            "info": "共 _PAGES_ 页  _TOTAL_ 条数据  ",
            "infoEmpty": "暂无数据",
            "emptyTable": "暂无要处理的数据...",  //表格中无数据
            "search": "搜索:",
            "infoFiltered": " —— 从  _MAX_ 条数据中筛选",
            "zeroRecords": "没有找到记录"
        },
        // datatables 自带的 ajax 请求数据
        "ajax": {
            url: '', // 请求的 url
            method: 'post', // 请求方式
            timeout: 5000,  // 请求延迟
            dataType: 'JSON', // 数据类型
        },
        let alldata = ['role', 'name', 'password'];
        // 设置后台返回的字段名，Datatable 会自动填充
		'columns': [
						{ 'data': 'role' },
						{ 'data': 'name' },
						{ 'data': 'password' },
						// name 为每一行传入的 data 字段
						{ 'data': 'name',
								'render':function(data,type,row,meta){
                                // 自定义代码（一般添加增、删、改按钮），data 为传入的 name 值
								var html = `<button name="add" id="add_${data}">添加</button>
											<button name="delete" id="delete_${data}">删除</button>` 
								return html;
							} 
						}
					],
         // cells 存储着所有行对象（通过 . 可获取属性）
         createdRow: function (row, data, dataIndex, cells) {
         for (let i = 0; i < alldata.length; i++) {
             // 为每一行元素添加唯一标识 ID
             cells[i].id = alldata[i]
         }
    }
}
```



#### 4、为 Table 添加点击事件

> 通过事件委托的方式添加事件监听。
>
> 1）虽然监听事件添加到 `table` 上，但是可以通过  `event` 事件对象判断点击了哪一个 `id` 或 `name` 的控件。

```javascript
$('#example').click(function (event) {
    // 每一行数据中每个字段的 name 值和 id 值
    if (event.target.name && event.target.id) {
        // 通过判断 name 值来确定删除/添加
        switch (event.target.name) {
            case 'add':
                // 对表格做添加数据
                // ......
                break;
            case 'delete':
                // 对该行做删除处理
                // ......
        }
    }
});
```



#### 5、获取同行的所有数据

> 通过点击某按钮的 id 获取父节点，从而达到获取同行的数据。
>
> 1）得到同一行数据之后，我们可以进行传值编辑或者删除一行数据。

```javascript
// 同行某点击事件的 id
function getSameValue(id) {
    // 获取该 id 父节点的所有兄弟节点（存储所有数据的结点）
    let values = $('#' + id).parent().parent().siblings();
    // 以键值对的方式存储到 map 中
    let result = new Map();
    // 将同一行的数据 name:value 遍历出来
    for (let obj of values) {
        // 通过结点得到想要的属性存储起来
        result.set(obj.id, obj.outerText);
    }
    // 返回 map
    return result;
}
```



#### 6、删除一行数据

> 得到同一行数据之后，进行删除操作。
>
> 1）删除一行需要的到该行的 DOM 对象，可以通过目标值的父节点获取

```javascript
let row = event.target.parentNode.（...）.parentNode;
// 进行删除更新
table.row(row).remove().draw(false);
```



#### 7、添加一行数据

> 

```javascript
let table = $('#example').DataTable({})
// 更新数据
table.ajax.reload(null, true);
```



