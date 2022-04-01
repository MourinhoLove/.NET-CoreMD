# MVC C#经验

## 代码实践

### - 创建跳转的URL

```javascript
var url = '@Url.Action("Privacy", "Home")';
window.location.href = url;
```

这里的**Privacy** 是控制器方法的名字，他返回了一个View。**Home**是控制器的名称

@Url.Action 方法还能传递参数例如

```javascript
 var url = '@Url.Action("Details", "Branch", new { id = "__id__" })';
 window.location.href = url.replace('__id__', id);
```

### - 异步的编程方法

- 控制器的写法

  ```c#
  [HttpPost("[controller]/dologin")]
  public async Task<IActionResult> login(string name, string password)
  {
      var value = await _service.login(name, password);
      if (value)
      {
          return Ok("登录成功");
      } 
      else
      {
          return NotFound("登录失败");
      }
  
  }
  ```

  简单的来说就是 方法名中加`async`

  实现里面加`await`

- 服务的写法

  ```c#
  public async Task<bool> login(string name, string password)
  {
      using (_dbcontex)
      {
          var list = await _dbcontex.People
              .FromSqlInterpolated($@"select * 
                                      from user.people 
                                      where Name = {name} 
                                      and Password = {password}")
              .ToListAsync();
          if (!list.Any())
          {
              return false;
          } else
          {
              return true;
          }
      }
  }
  ```

  让你返回的是`ToListAsync`就需要`await`修饰

  `async Task<bool>`这里返回一个异步的bool值

  ***是否需要异步需要自己考虑**

如果你在一个控制器里所有的方法都是异步 那么我推荐你都用异步就行了。

### - 样式引入的问题

```html
<script src="~/libs/jquery/dist/jquery.js"></script>
```

这里的`~/`指的是根目录，一般样式文件和JS都放在根目录里面

### - 样式错乱的问题

请在cshtml里面采用

```c#
@{
    Layout = null;
}
```

用来取消自带的布局

### - layout布局的详细问题

.net core 的APP项目 默认会使用他自己的布局模板

是什么意思呢。就是他给你创建了html的头和尾 然后中间再给你一个占位符。让你显示模板的内容

学习了2个礼拜我现在才知道他具体的布局方式。

### - JS和CSS的引入

#### CSS

```html
<link rel="stylesheet" href="../lib/layui/css/layui.css" />
```

`..`代表上一级目录

`.` 代表目前所在的目录

`/` 代表根目录

#### JS

```html
<script src="../lib/layui/layui.js"></script>
<script src="../lib/jquery/dist/jquery.min.js"></script>、
<script src="../js/addUser.js"></script>
```

这里要注意 引入JS的位置要在`</body>`前，你也可以放在`<head>`里

### 为啥初始化后是HomeController的页面呢

当我们创建一个新的MVC项目的时候, 你会发现默认进去的是Home的index.html 这里同学们就有疑问了为啥是这个

其实这跟初始化的时候路由指定有关

```c#
app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Privacy}/{id?}");

```

- 第一个路径段 `{controller=Home}` 映射到控制器名称。
- 第二段 `{action=Index}` 映射到`{action=Index}`名称。
- 第三段 `{id?}` 用于可选 `id`。 `{id?}` 中的 `?` 使其成为可选。 `id` 用于映射到模型实体。

这里指定了默认的控制器是home，动作是Index,这里我改成了Privacy。你们可以新建一个控制器和View 进行修改。看看初始化的时候页面会不会有变化

### 指定返回的view

我们知道控制器告诉.NET 显示什么view

方法是：

```c#
public IActionResult Index()
{
    return View("Login");
}
```

这里我指定了该控制器显示的view是Login

## EF使用教程

<details》作者

这里有两种驱动方式。一种是我用代码去创建数据库。一种是我先建好数据库，再用代码去访问。这里介绍第二种方法。第一种暂时还没好好研究

### 1.创建对应的model

```c#
[Table("people")]
public class Person
{
    [Key]
    public int Id { get; set; }
    //[NotMapped]
    public string Name { get; set; }
    public string Password { get; set; }
    [NotMapped]
    public DateTime CreatedAt { get; set; }
}
```

### 2.注册上下文

```c#
public class PersonContext : DbContext
{
   // private IConfiguration Configuration { get; }

    public DbSet<Person> People { get; set; }
    public PersonContext(DbContextOptions<PersonContext> options)
        : base(options)
    {

    }
}
```

### 3.在入口进行依赖注入

```c#
services.AddDbContext<PersonContext>(options => options.UseMySql(Configuration.GetConnectionString("MySQL"), MySqlServerVersion.LatestSupportedServerVersion));
```

### 4.在服务或者控制器的构造函数中得到数据库对象

```c#
private readonly PersonService _service;

public LoginController(PersonContext dbContext)
{
    this._service = new PersonService(dbContext);
}
```

### 5.根据原有的数据库创建数据

>Scaffold-DbContext "server=localhost;Port=3306;Database=sql_hr; User=root;Password=9988;" Pomelo.EntityFrameworkCore.MySql -OutputDir Models -Tables employees,offices
>
>PM> Scaffold-DbContext "server=localhost;Port=3306;Database=sql_inventory; User=root;Password=9988;" Pomelo.EntityFrameworkCore.MySql -OutputDir Models -Context ProductContext -DataAnnotations

### 5.查询数据

#### - 查询所有

```c#
public List<Person> getAll()
{
    using (_dbcontex)
    {
        var list = _dbcontex.People
             .FromSqlRaw("SELECT * FROM user.people")
             .ToList();
        return list;
    }
}
```

#### - 根据用户输入 查询相关内容

```c#
public bool login(string name, string password)
{
    using (_dbcontex)
    {
        var list = _dbcontex.People
            .FromSqlInterpolated($@"select * 
                                    from user.people 
                                    where Name = {name} and Password = {password}")
            .ToList();
        if (!list.Any())
        {
            return false;
        } else
        {
            return true;
        }
    }

}
```

`ToList()` :返回的是一个查询的数组

`FromSqlRaw` : 最普通的查询方式

`FromSqlInterpolated` : 适用于根据参数进行查询

#### -SQL的写法

>```c#
>.FromSqlInterpolated($@"select * 
>                   from user.people 
>                   where Name = {name} 
>                   and Password = {password}")
>```
>
>这里记得用FromSqlInterpolated的时候 要添加`$`
>
>`@`用于创造一个可以换行的字符串

详情可以参考[官网的链接](**https://docs.microsoft.com/zh-cn/ef/core/querying/raw-sql**)

## 异步或者同步的写法

### 同步

```c#
[HttpPost("[controller]/getLsit")]
public IActionResult GetList(string name, string password)
{
    var list = this._service.getAll();
    return Ok(list);
}
```

### 异步

```c#
[HttpPost("[controller]/dologin")]
public async Task<IActionResult> login(string name, string password)
{
    var value = await _service.login(name, password);
    if (value != null)
    {
        return Ok("登录成功");
    } 
    else
    {
        return NotFound("登录失败");
    }

}
public async Task<Person> login(string name, string password)
{

    using (_dbcontex)
        {
            var list = await _dbcontex.People
                .FromSqlInterpolated($@"select * 
                                        from user.people 
                                        where Name = {name} 
                                        and Password = {password}")
                .ToListAsync();
            if (!list.Any())
            {
                return null;
            } else
            {
                return list.First();
            }
        }
}
```

## ORM的Linq

### 简介

通过微软的EF Core框架跟数据库交互的时候，你需要考虑用原生SQL或者Linq来跟数据库做交互

这里我们推荐用Linq

### 查询

#### 1. 获取一个表的所有数据

```c#
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

这里的ToList()就是Linq提供给你的

#### 2. 获取单个数据

```c#
var blog = context.Blogs
    .Single(b => b.BlogId == 1);
```

这里根据id查出表中id = 2的数据

#### 3. 筛选

```c#
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Where(b => b.Url.Contains("dotnet"))
        .ToList();
}
```

获取表中URL包含dotnet的字符串

#### 4.分页

```c#
var list = await _dbcontex.Products
          .Where(p => p.Name.Contains("D"))
          .Skip(index)
          .Take(limit)
          .ToListAsync();
```

`Skip` :跳过多少数据 进行查询

`Take` :获取多少数据

分页的计算公式（默认从1开始）:

`var index = (page - 1) * limit;`

比如前端需要 第一页的数据，每次获取10条

#### 5. 多表查询

1. Include

   如果你的实体类里面有一个外键，指向了另一张表。你可以使用`Include`把那张表的所有数据拿过来，比如说

   ```c#
   var items = await _dbcontex
       .Tags
       .Where(x => x.Date.ToString() == date)
       .Include(Item => Item.Item)
       .ToListAsync();
   ```

   这里就把Item指向的内容一起拿过来。当然有一个前提就是你需要在实体类里面定义好你的数据

   像这样

   ```c#
   [ForeignKey("ItemId")]
   [InverseProperty("Tags")]
   public virtual Item Item { get; set; } = null!;
   ```

   指定了外键

   个人觉得这个可能会用的不多。毕竟我可能需要返一个自定义的选项给前端

#### 6.匿名参数

```c#
.Join(_dbcontex.Items, m => m.ItemId, mt => mt.ItemId, (m,mt)=> new
{
    claaName = m.ClassName,
    tagId = m.TagId,
    startDate = m.StartDate,
    endDate = m.EndDate,
    teacherName = mt.TeacherName,
    itemId = mt.ItemId
})
```

`_dbcontex.Items` : 要链接的表

`m => m.ItemId`： 主表链接字段

`mt => mt.ItemId`: 副表连接字段

通过这种写法，可以创建一个临时的类，因为有时候数据需要返给前端需要的格式。如果这个格式只在该方法使用，那么可以考虑用匿名参数创建临时的class。

#### 7.数组分组

```c#
.GroupBy(u => u.GroupID)
.Select(grp => grp.ToList())
```

把相同id的数据分为一组

#### 8.Join的用法 这里联合了三张表

````c#
.Join(
    _dbcontext.Teachers,
    s => s.TeacherId,
    t => t.TeacherId,
    (s, t) => new
    {
        s,
        t.TeacherName
    })
.Join(
    _dbcontext.Courses,
    s => s.s.CourseId,
    c => c.CourseId,
    (s, c) => new
    {
        TeacherName = s.TeacherName,
        TeacherId = s.s.TeacherId,
        CourseID = c.CourseId,
        CourseName = c.CourseName,
        StartDate = s.s.StartDate,
        EndDate = s.s.EndDate
    })
````



### 新增

### 1. ADD 

```c#
_dbcontex.Add(product);
var isSucceed = await _dbcontex.SaveChangesAsync();
```

### 元组

有时候你需要方法返回2个值

这时候你可以使用元组

例如

` Task<(List<Product>, int)>`

## 错误拦截

### 方法一

```c#
AddControllersWithViews(config => config.Filters.Add(typeof(ModelValidateActionFilterAttribute)))
```

自定义一个筛选器

```c#
 public override void OnActionExecuting(ActionExecutingContext context)
    {
        if (!context.ModelState.IsValid)
        {
            //公共返回数据类
            ReturnMsg returnMsg = new ReturnMsg() { Code = "-1" };

            //获取具体的错误消息
            foreach (var item in context.ModelState.Values)
            {
                //遍历所有项目的中的所有错误信息
                foreach (var err in item.Errors)
                {
                    //消息拼接,用|隔开，前端根据容易解析
                    returnMsg.Msg += $"{err.ErrorMessage}|";
                }
            }
            context.Result = new JsonResult(returnMsg);
        }

    }
}
```

但是这个方法返回的是200的status code
