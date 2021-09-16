# bootstrap笔记

## 1. 概念:

1. Bootstrap是最受欢迎的 HTML、CSS 和 JS 前端框架，用于开发响应式布局、**移动设备优先的 WEB 项目**；
2. Bootstrap提供了优雅的HTML和CSS规范，它即是由动态CSS语言Less写成。

特点:

1. 简洁、直观、强悍
2. 可以响应式布局

## 2.导入

使用之前必先导包，所以这里需要引入bootstrap文件：

![image-20210916094011753](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210916094011753.png)

## 3.布局容器

我们在使用bootstrap进行布局还是栅格系统都是基于不就容器进行布局的，所以这里是非常重要的：

主要提供了两个类：`.container`和`.container-fluid`

1. `.container` 类用于固定宽度并支持响应式布局的容器。

   ```html
   <div class="container">
     ...
   </div>
   ```

2. `.container-fluid` 类用于 100% 宽度，占据全部视口（viewport）的容器。

   ```html
   <div class="container-fluid">
     ...
   </div>
   ```

**注意:**由于 padding 等属性的原因，这两种容器类不能互相嵌套；

## 4.栅格系统

bootstrap最重要的就是栅格系统,这个栅格系统会随着屏幕或浏览器尺寸就会改变;

**概念**:栅格系统用于通过一系列的行（row）与列（column）的组合来创建页面布局，你的内容就可以放入这些创建好的布局中

> 栅格系统中系统会自动会把屏幕划分为最多12列,超出的就会跑到下面那一行去

那再来看一下这个栅格系统的网格是如何布局的:

![image-20210916101518434](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210916101518434.png)

### 4.1 工作原理

网格系统通过一系列包含内容的行和列来创建页面布局。下面列出了 Bootstrap 网格系统是如何工作的：

1. 行必须放置在`.container`的class内,以便获得适当的对齐（alignment）和内边距（padding）;
2. 使用行来创建列的水平组;
3. 内容应该放置在列内，且唯有列可以是行的直接子元素;
4. 预定义的网格类，比如 `.row` 和 `.col-xs-4`，可用于快速创建网格布局。LESS 混合类可用于更多语义布局;
5. 列通过内边距（padding）来创建列内容之间的间隙。该内边距是通过 .rows 上的外边距（margin）取负，表示第一列和最后一列的行偏移
6. 网格系统是通过指定您想要横跨的十二个可用的列来创建的。例如，要创建三个相等的列，则使用三个 `.col-xs-4`;

### 4.2 使用

在使用栅格系统中有专门的类是人家写好了我们就只需要拿来用就可以:

常用:

|              类名              |        说明        |
| :----------------------------: | :----------------: |
| xm=x-small（extra small特小号) |      手机屏幕      |
|        sm（small小号）         |     平板，pad      |
|        md（medium中号）        | 笔记本，小屏幕台式 |
|        lg（large大号）         |     超大号屏幕     |

这里详细解释网格系统如何跨设备工作:

![image-20210916102713824](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210916102713824.png)

## 5.案例

由于代码较多,这里就只把我的JS贴出来就可以了:

```javascript
<script type="text/javascript">
  $(function () {
    //设置css
    $("#queryTr th").css("text-align", "center");

    //在页面加载的时候去查询所需要的数据
    function queryList() {
      $.ajax({
        type: "get",
        url: "http://localhost:8080/demo/list",
        success: function (data) {
          //遍历查询出来的数据
          $.each(data, function (index, obj) {
            //动态定义按钮
            //将获取到的数据转为字符串格式
            var edit = '<button id="editBtn" data-obj=' + JSON.stringify(obj) + ' type="button" class="btn btn-primary btn-xs glyphicon glyphicon-pencil">编辑</button>'
            var del = '<button id="delBtn" data-id=' + obj.id + ' type="button" class="btn btn-danger btn-xs glyphicon glyphicon-trash">删除</button>'
            //添加到表格中
            let item = "<tr style='text-align: center;'>"
              + "<td>" + obj.id + "</td>"
              + "<td>" + obj.name + "</td>"
              + "<td>" + (obj.sex ? "男" : "女") + "</td>"
              + "<td>" + obj.address + "</td>"
              + "<td>" + obj.tel + "</td>"
              + "<td>" + obj.card + "</td>"
              + "<td>" + edit + "&emsp;" + del + "</td>"  /* 动态的添加编辑删除按钮 */
              + "</tr>";
            $("#tbody").append(item);
          });
        }
      })
    }

    //调用查询list
    queryList();

    //对添加按钮进行绑定事件
    $("#addBtn").on("click", function () {
      $("#eidtForm").clearForm()/* 清空表单数据 */
      $("#addEditModal").modal("show");//添加修改模态框显示
      /* 手动清除id */
      $("#id").val("")  //在编辑的时候如果不保存,那么浏览器就会缓存你的id,所以这里要清除
    })

    //对添加表单进行发起存储请求
    $("#saveBtn").on("click", function () {
      //发起请求
      $("#eidtForm").ajaxSubmit({
        url: "http://localhost:8080/demo/save",
        method: "post",
        success: function (data) {
          //请求成功
          if (data.success) {
            //关闭模态框
            $("#addEditModal").modal("hide");
            //刷新页面即可
            location.reload();
          } else {
            //将错误信息以弹窗形式弹出
            alert(data.msg);
          }
        }
      })
    })

    //编辑页面-采用事件委托机制,进行动态添加事件
    $("#tbody").on("click", "#editBtn", function () {
      $("#eidtForm").clearForm()/* 清空表单数据 */
      $("#addEditModal").modal("show");//添加修改模态框显示
      //回显
      let obj = $(this).data("obj");
      //将获取到数据进行set表单上
      $("#eidtForm").setForm(obj);
    })

    //删除数据
    $("#tbody").on("click", "#delBtn", function () {
      //打开模态框
      $("#delModal").modal("show");
      //绑定之前事件清除
      $("#delSure").off();
      let id = $(this).data("id");

      $("#delSure").on("click", function () {
        $.ajax({
          data: {id: id},
          url: "http://localhost:8080/demo/delete",
          success: function (data) {
            if (data.success) {
              $("#delModal").modal("hide");
              location.reload();
            } else {
              alert(data.msg);
            }
          }
        })
      });
    })

  })
</script>
```

页面:

![image-20210916103038861](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210916103038861.png)

**总结**:写前端要注意浏览器的缓存问题,所以在编辑数据的时候浏览器可能会保存上次编辑残留的数据,这是要考虑到的!除了缓存还有就是向服务器后端发起AJax请求后的响应!其他的比如HTML页面就没啥好总结的,说白了用bootstrap后人家就写好了CSS跟JS,我们只需要去用就行,最重要的还是去考虑我们如何**获取数据**和**组织数据**

