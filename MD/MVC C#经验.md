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