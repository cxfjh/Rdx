## Rdx 快速开始

### 1. 引入库

直接在 HTML 顶部中引入 Rdx.js (压缩版本大小11kb)，无需任何构建步骤：

```html
<script src="https://cxfjh.cn/js/rdx/0.0.1.js"></script>        <!-- 完整版本 -->
<script src="https://cxfjh.cn/js/rdx/min.0.0.1.js"></script>    <!-- 压缩版本 -->
```

### 2. 基础示例

```html
<body>
   <!-- 响应式计数示例，{{ }} 执行表达式语句时需要加上 .value -->
   <h1>{{ count }} {{ count.value + 2 }}</h1>

   <!-- 事件绑定示例，里面执行的是 JavaScript 代码，需要加 .value -->
   <button r-click="count.value++">增加</button>
   <button r-click="count.value--">减少</button>

   <!-- 双向绑定 -->
   <input type="text" r-model="name" placeholder="输入名字">
   <p>你好，{{ name }}!</p>

   <script>
      // 定义响应式数据
      const count = ref(0);
      const name = ref("");

      // r-model 双向绑定需要provide注册
      provide({ name });
   </script>
</body>
```

## 核心功能文档

### 1. 响应式系统

#### ref - 基础类型响应式

```javascript
// 定义基础类型响应式数据
const count = ref(0);
const name = ref("张三");

// 访问或修改值（需要 .value）
console.log(count.value); // 0
count.value++; // 修改值，页面自动更新
```

#### reactive - 对象/数组响应式

```javascript
// 定义对象响应式数据
const user = reactive({
   name: "李四",
   age: 20
});

// 直接修改，无需写 .value
user.age = 21;

// 数组响应式
const list = reactive([1, 2, 3]);
list.push(4);
```

#### provide - 双向绑定注册

```javascript
// 用于 r-model 双向绑定的变量需要 provide 注册
const name = ref("");
const age = ref(0);

provide({ name, age }); // 注册
```

### 2. 模板指令

| 指令        | 作用    | 示例                                            |
|-----------|-------|-----------------------------------------------|
| `r-if`    | 条件渲染  | `<div r-if="count.value > 5">显示</div>`        |
| `r-click` | 点击事件  | `<button r-click="count.value++">增加</button>` |
| `r-model` | 双向绑定  | `<input type="text" r-model="name">`          |
| `r-for`   | 循环渲染  | `<div r-for="count">第{{ index }}项</div>`      |
| `r-arr`   | 数组循环  | `<div r-arr="list">值：{{ value }}</div>`       |
| `r-api`   | 接口请求  | `<div r-api="https://api.com/data">...</div>` |
| `r-route` | 路由跳转  | `<button r-route="home">首页</button>`          |
| `r-cp`    | 组件使用  | `<div r-cp="user" $name="张三"></div>`          |
| `r-dom`   | 组件引用  | `<div r-dom="user"></div>`                    |
| `r`       | DOM引用 | `<div r="container">容器</div>`                 |

#### 指令详细示例

##### r-if 条件渲染

```html
<!-- 支持表达式，表达式需要写 JS 代码 -->
<div r-if="count.value % 2 === 0">偶数</div>
<div r-if="num % 2 !== 0">奇数</div>
<div r-if="user.age > 18">成年</div>

<script>
   const count = ref(0);
   const num = 1;
   const user = reactive({ age: 18 });
</script>
```

##### r-for 循环渲染

```html
<!-- 循环 count 次，索引从1开始，索引 index  -->
<div r-for="count">
   <p>第{{ index }}项</p>
</div>

<!-- 自定义索引名-->
<div r-for="5" index="i">
   <p>{{ i }}</p>
</div>

<script>
   const count = ref(5);
</script>
```

##### r-arr 数组循环

```html
<!-- 循环 list 数组，值 value，索引 index  -->
<div r-arr="list">
   索引：{{ index }} - 值：{{ value.name }}
</div>

<!-- 自定义值和索引名-->
<div r-arr="list" value="user" index="idx">
   {{ user.name }} - {{ user.age }} - {{ idx }}
</div>

<div r-arr="['苹果', '香蕉']">
   索引：{{ index }} - 值：{{ value }}
</div>

<script>
   const list = reactive([
      { name: "张三", age: 18 },
      { name: "李四", age: 10 },
   ]);
</script>
```

##### r-api 接口请求

```html
<!-- r-api 请求的地址 -->
<!-- list 请求结果的data数据 -->
<div r-api="https://sp.cxfjh.cn/api/test" list="data">
   {{ value.name }}
</div>

<!-- meth 请求的地址 -->
<!-- r-api 请求的地址 支持 JS 脚本代码 -->
<!-- hdr 请求头 支持 JS 脚本代码 hdr='{"Authorization": tk}'-->
<!-- data-body 请求的参数 支持 JS 脚本代码 data-body="data" -->
<!-- list 列表数据绑定的变量名 -->
<!-- refr 绑定刷新id -->
<!-- arr 需自行渲染数据 -->
<!-- aw 需手动请求并返回 _aw 请求完成是true, 未完成是false -->
<div r-api="https://sp.cxfjh.cn/api/test" meth="post" hdr='hdr' data-body='{"name": name.value, "age": age}' list="info" refr="#btn" arr="data" aw>
   <div r-arr="data">{{ value.name }}</div>
   <span>{{ _aw ? '请求完成' : '正在等待请求'  }}</span>
   <button id="btn">加载数据</button>
</div>

<script>
   const name = ref("fjh");
   const age = 17;

   const hdr = {
      "Authorization": "token",
      "Content-Type": "application/json",
   };
</script>

<!-- r-api、hdr、data-body 支持 JS 脚本代码 -->

<!-- r-api="url" r-api="url.value" r-api="https://sp.cxfjh.cn/api/{{url}}" r-api="https://sp.cxfjh.cn/api/{{url.value}}" -->
<!-- hdr="hdr" hdr="hdr.value" hdr='{"Authorization": token}' hdr='{"Authorization": token.value}' -->
<!-- data-body="info" data-body="info.value" data-body='{"name": name, "age": age}' data-body='{"name": name.value}' -->
```

##### r-click 事件指令

###### 基础用法：点击事件, 执行的是JS脚本代码

```html
<!-- 因为 r-click 指令需要执行 JS 脚本代码，所以需要加 .value -->
<button r-click="count.value++">增加计数</button>
<button r-click="info.age = 0">重置年龄</button>
<button r-click="count.value++; alert('当前计数：' + count.value)">增加并弹窗</button>

<div>
   <div>当前计数：{{ count.value }}</div>
   <div>当前年龄：{{ info.age }}</div>
</div>

<!-- 调用函数 -->
<button r-click="handleSubmit()">提交</button>

<script>
   const count = ref(0);
   const info = reactive({ age: 0 });

   // 定义全局函数
   const handleSubmit = () => {
      console.log("提交数据", count.value, info.age);
      alert("提交成功！");
   };
</script>
```

###### 扩展事件类型（非click事件）

通过元素属性指定事件类型，支持所有原生事件：

```html
<!-- 双击事件 -->
<div r-click="alert('双击触发')" dblclick>双击我</div>

<!-- 鼠标悬停事件 -->
<div r-click="console.log('鼠标移入')" mouseover>鼠标移入</div>

<!-- 键盘事件 -->
<input type="text" r-click="console.log('按下了：' + event.key)" keydown placeholder="按下键盘触发">
```

###### 键盘事件按键过滤

支持按特定按键触发事件，内置常用按键别名（Enter、Esc等）：

```html
<!-- 只在按下 Enter 键时触发 -->
<input type="text" r-click="console.log('按下了：' + event.key)" keydown="enter" placeholder="按 Enter 搜索">

<!-- 只在按下 Esc 键时触发 -->
<input type="text" r-click="console.log('按下了：' + event.key)" keydown="esc" placeholder="按 Esc 清空">

<input type="text" r-click="console.log('按下了：' + event.key)" keydown="ctrl+x" placeholder="按 Ctrl+S 保存">
```

##### r-cp 组件指令

```html
<!-- 使用 template 标签定义名为 "user-card" 的组件 -->
<template r-cp="user-card">
   <div class="card">
      {{ nameA }} - {{ age }}
   </div>
</template>

<!-- 使用组件，使用 $ 来传递数据，驼峰变量使用 - 短横杠 name-a => nameA -->
<div r-cp="user-card" $name-a="张三" $age="18"></div>
```

### 3. 组件系统

#### 定义组件

配合使用`dom() 和 r-dom `函数定义组件，支持模板、样式、脚本分离：

```html
<body>
   <div id="user"></div>

   <!-- 使用 r-dom 指令渲染组件 -->
   <div r-dom="user" $init-age="12"></div>
</body>

<script>
   const UserComponent = dom("user", {
      // HTML 模板
      template: `
           <div class="user-card">
               <h3>{{ username }}</h3>
               <p ref="p">年龄：{{ age.value }}</p>
               <button r-click="increaseAge()">增加年龄</button>
           </div>
        `,

      // 样式
      style: `
           .user-card {
               border: 1px solid #ccc;
               padding: 16px;
               border-radius: 8px;
           }
               
           button {
               background: #42b983;
               color: white;
               border: none;
               padding: 8px 16px;
               border-radius: 4px;
           }
        `,

      // 脚本逻辑
      script: ({ $pro, $refs }) => {
         // 初始化数据
         const setup = ($) => {
            // 如果存在值，则返回值，否则返回默认值, 因为外部参数优先级高
            const age = ref($pro.age.value);
            const username = ref("匿名用户");

            console.log($pro.age.value, $pro.title.value, $pro.x);

            // 方法
            const increaseAge = () => {
               age.value++;
               console.log($refs.p.innerText);
            };

            // 通过 $ 定义的数据可以不用 return ($ 可以通过修改 setup 函数参数可以自定义名字 setup(ctx) ctx.text)
            $.text = ref("Hello");
            $.setText = () => $.text.value = "Hello World";
            // 如果 $ 定义的命名一样, return 暴露的数据会覆盖 $ 暴露的数据, return 优先级高

            return { age, username, increaseAge };
         };

         // 生命周期钩子
         function mounted() {
            console.log("组件 DOM 挂载完成");
            console.log(this.$refs.p); // DOM 挂载完成可以获取元素了
            console.log(this.age.value, this.text.value)
         }

         function unmounted() {
            console.log(`组件 DOM 销毁时调用`);
            console.log(this.$refs.p);
         }

         return { setup, mounted, unmounted };
      },

      // 样式隔离（默认启用）
      sty: true,

      // 定义初始或默认值
      pro: {
         age: ref(18)
         title: ref("用户信息")
         x: 12
      }

      // 自动挂载到指定元素
      // to: "#user",
   });

   // name 指定渲染到 id 为 user 的元素， sty 默认为 true 表示启用样式隔离，pro 传递参数
   const uc = UserComponent({ name: "#user", sty: true, pro: { age: ref(11) } });

   setTimeout(() => {
      console.log(uc.root()); // 获取组件的根元素

      uc.age.value = 20; // 修改内部数据
      uc.$pro.age = 15; // 修改默认值

      uc.delSty(); // 删除共享样式
      uc.del(false); // 删除组件，true 和 false 表示是否删除共享样式，默认为 false 不删除
   }, 2000);
</script>
```

### 4. 路由系统

#### 定义路由页面, r-page 注册路由, &route 指定渲染容器, r-route 导航, route 路由容器

```html
<body>
   <!-- 定义路由页面, r-page 注册路由, &route 绑定路由容器(默认 view)  -->
   <div r-page="home">
      <h3>首页 (我要渲染到 view 容器里)</h3>
   </div >
   <div r-page="about" &route="info">
      <h3>关于 (我要渲染到 info 容器里)</h3>
   </div>
   <div r-page="set" &route="view">
      <h3>设置 (我要渲染到 view 容器里)</h3>
   </div>
   <div r-page="data" &route="info">
      <h3>数据 (我要渲染到 info 容器里)</h3>
   </div>

   <!-- 路由容器, route 指定容器名称 -->
   <div>
      <h1>我是 view 容器</h1>
      <div route="view"></div>
   </div>
   <div>
      <h1>我是 info 容器</h1>
      <div route="info"></div>
   </div>

   <!-- 路由导航, r-route 导航到指定页面, router.nav 跳转路由 -->
   <button r-route="home">首页 (view 容器里)</button>
   <button r-route="about">关于 (info 容器里)</button>
   <button r-route="set">设置 (view 容器里)</button>
   <button r-click="router.nav('data')">（编程式导航）数据 (info 容器里)</button>
</body>
```

### 5. DOM引用

配合使用`r 和 onMounted()`指令获取DOM元素：

```html
<!-- 定义引用，使用响应式数据需要加 {{  }} -->
<body>
   <div r="{{ name }}">内容容器</div>
   <input type="text" r="username">
</body>

<!-- 初始阶段不能立即访问 -->
<script>
   const name = ref("text");

   // onMounted 在初始化访问引用
   onMounted(() => {
      $r.text.innerText = "onMounted1";
      console.log($r.text); // 获取DOM元素
      $r.username.value = "1";
   });
</script>

<!-- 给标签加一个src，功能和 onMounted 一样 -->
<script src>
   $r.text.innerText = "onMounted2";
   console.log($r.text); // 获取DOM元素
   $r.username.value = "2";
</script>

```

## 注意事项

1. **响应式访问**：
   - `ref`类型数据在脚本中需要通过`.value`访问/修改
   - `reactive`类型数据在脚本中无需`.value`访问/修改
   - 模板中使用时无需`.value`（指令内部已处理）

2. **r-model绑定**：
   - 必须通过`provide`注册后才能使用
   - 支持input/select/checkbox/radio等表单元素

3. **组件样式隔离**：
   - 默认启用样式隔离，组件样式不会影响全局
   - 可通过`sty: false`关闭隔离

4. **表达式解析**：
   - 模板中的`{{ }}`支持JS表达式
   - 指令中的值（如r-if/r-click）直接执行JS代码

