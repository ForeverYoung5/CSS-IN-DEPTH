# `Flexbox`的原则

给元素添加`display:flex`，该元素变成了一个*弹性容器*，它的直接子元素变成了*弹性子元素*。

弹性子元素默认是在同一行按照从左到右的顺序并排排列。**弹性子元素高度相等**（这也是`Flexbox`可以轻易实现等高布局的原因），该高度由它们的内容决定。

弹性容器像块元素一样填满可用宽度，但是弹性子元素不一定填满其弹性容器的宽度。


# 案例一

![enter description here](./images/1692933733904.png)

代码准备：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Home</title>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width" />
    <style>
      :root {
        box-sizing: border-box;
      }
      *,
      ::before,
      ::after {
        box-sizing: inherit;
      }

      body {
        background: #eee;
      }

      body * + * {
        margin-top: 1.5em;
      }
      .container {
        max-width: 1080px;
        margin: 0 auto;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <header>
        <h1>Ink</h1>
      </header>
      <nav>
        <ul class="site-nav">
          <li><a href="">Home</a></li>
          <li><a href="">Features</a></li>
          <li><a href="">Pricing</a></li>
          <li><a href="">Support</a></li>
          <li class="nav-right"><a href="">About</a></li>
        </ul>
      </nav>
      <main class="flex">
        <div class="column-main title">
          <h1>Team collaboration done right</h1>
          <p>
            Team collaboration done right,Team collaboration done right,Team
            collaboration done right,Team collaboration done right.
          </p>
        </div>

        <div class="column-sidebar">
          <div class="title">
            <form class="login-form" action="">
              <h3>login</h3>
              <p>
                <label for="username">Username</label>
                <input id="username" type="text" name="username" />
              </p>
              <p>
                <label for="password">Password</label>
                <input id="password" type="password" name="password" />
              </p>
              <button type="submit">Login</button>
            </form>
          </div>
          <div class="title centered">
            <small>starting at</small>
            <div class="cost">
              <span class="cost-currency">$</span>
              <span class="cost-dollars">20</span>
              <span class="cost-cents">.00</span>
            </div>
            <a class="cta-button" href="">Sign up</a>
          </div>
        </div>
      </main>
    </div>
  </body>
</html>

```

首先完成导航栏的样式：

```css
      .site-nav {
        display: flex;
        list-style-type: none;
        background-color: #5f4b44;
        padding-left: 0;
        padding: 0.5em;
      }
      .site-nav > li {
        margin-top: 0;
      }

      .site-nav > li > a {
        background-color: #cc6b5a;
        color: white;
        text-decoration: none;
        padding: 0.5em 1em;
        display: block; /*撑开父元素高度 */
      }
      .site-nav > li + li {
        margin-left: 1.5em;
      }
      .site-nav > .nav-right {
        margin-left: auto;
      }
```

注意这里的链接被设置为块级元素。如果链接还是行内元素，那么它给父元素贡献的高度会根据行高计算，而不是根据内边距和内容，这样不符合预期。另外，这里给水平方向设置的内边距比垂直方向的要多一点，因为从美学上来讲这样更让人愉悦。

`Flexbox`允许使用`margin: auto`来填充弹性子元素之间的可用空间。如此可以让最后一个元素靠右。

---

接下来实现主体部分：

简写声明`flex: 2`和`flex: 1`设置了一个弹性基准值为0%，因此容器宽度的100%都是剩余宽度（减去两列之间`1.5em`的外边距）。剩余宽度会分配给两列：第一列得到2/3的宽度，第二列得到1/3的宽度

```css
      .title {
        padding: 1.5em;
        background-color: white;
      }
      .flex {
        display: flex;
      }
      .flex > * + * {
        margin-top: 0;
        margin-left: 1.5em;
      }
      .column-main {
        flex: 2;
      }
      .column-sidebar {
        flex: 1;
      }
```



案例一知识点：

1. 子组合器（`>`）被放在两个 `CSS` 选择器之间。它只匹配那些被第二个选择器匹配的元素，这些元素是被第一个选择器匹配的元素的直接子元素。


# `flex`

`flex`属性是三个不同大小属性的简写：`flex-grow`、`flex-shrink`和`flex-basis`。

`flex-basis`：定义了元素大小的基准值，即一个初始的“主尺寸”。`flex-basis`属性可以设置为任意的`width`值，包括`px`、`em`、百分比。它的初始值是`auto`，此时浏览器会检查元素是否设置了`width`属性值。如果有，则使用`width`的值作为`flex-basis`的值；如果没有，则用元素内容自身的大小。如果`flex-basis`的值不是`auto`, `width`属性会被忽略。

`flex-grow`：每个弹性子元素的`flex-basis`值计算出来后，它们（加上子元素之间的外边距）加起来会占据一定的宽度。加起来的宽度不一定正好填满弹性容器的宽度，可能会有留白。多出来的留白（或剩余宽度）会按照`flex-grow`（增长因子）的值分配给每个弹性子元素，`flex-grow`的值为非负整数。如果一个弹性子元素的`flex-grow`值为0，那么它的宽度不会超过`flex-basis`的值；如果某个弹性子元素的增长因子非0，那么这些元素会增长到所有的剩余空间被分配完，也就意味着弹性子元素会填满容器的宽度。`flex-grow`的值越大，元素的“权重”越高，也就会占据更大的剩余宽度。一个`flex-grow:2`的子元素增长的宽度为`flex-grow: 1`的子元素的两倍。
	   ![enter description here](./images/1692956056032.png)
	   
`flex-shrink`属性与`flex-grow`遵循相似的原则。计算出弹性子元素的初始主尺寸后，它们的累加值可能会超出弹性容器的可用宽度。如果不用`flex-shrink`，就会导致溢出。`flex-shrink`属性与`flex-grow`遵循相似的原则。计算出弹性子元素的初始主尺寸后，它们的累加值可能会超出弹性容器的可用宽度。如果不用`flex-shrink`，就会导致溢出。

## 实际应用

![enter description here](./images/1692958877078.png)

