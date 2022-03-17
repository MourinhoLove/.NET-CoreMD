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

## 路由解析

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



### 5.查询语句

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

