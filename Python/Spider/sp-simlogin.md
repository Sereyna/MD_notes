# 模拟登录
---
## 1. 模拟登录原理
**session和cookie的区别**

1. session是由服务器维护的，并由服务器解释，通过set-cookie交给浏览器
2. cookie是浏览器的工具，并在后续的每一次请求中都带上这些值

**其他登录模式**
1. auth2.0（例：微博）
2. json web token(jwt)

## 2. 破解常用验证码

**第三方库**
1. 超级鹰
2. 云打码

### 2.1 滑动验证码

**手动解析出网站的滑动验证有什么步骤**

以bilibili为例：

1. 鼠标移动到正确的元素上，显示出没有缺口的图片并下载
2. 点击元素显示出有缺口的图片并下载
3. 对比两张图片找出缺口的移动像素
4. 拖动元素

### 2.1.1 破解
**两种方案**
- 使用OpenCV识别
- 使用机器学习方法识别




### 2.2 点击按钮进行智能验证
**出现场景**

在输入完账号密码后，需要点击一下智能验证控件。

**检测原理**

检测当前浏览器窗口下`window.navigator`对象中是否包含`webdriver`这个属性。如果你是采用`selenuim`自然免不了`webdriver`，这个时候`window.navigator`对象中就会包含`webdriver`属性，但是在我们平常使用浏览器时，这个属性是`undefined`，一旦被网站上的JS判断出这个属性的不同，就露馅了。。。

#### 2.2.1 破解

**第一种**（==没验证过==）
用pyppeteer解决反爬的方法，安装python第三方库asyncio、pyppeteer和pyppeteer_stealth然后一顿操作猛如虎似乎可以绕过智能验证？这个方法我没试，大家可以试试看，附上相关博文：使用Python自动填写问卷星(pyppeteer反爬虫版)。

**第二种**
修改`window.navigator.webdriver`属性的值为`undefined`，`python`设置`window.navigator.webdriver`的属性有两种方式：
1. 通过js脚本直接修改浏览器的`window.navigator.webdriver`的属性，此方法适用于任意浏览器，并且此方法是在浏览器启动之后执行，也就是`driver.get()`方法之后执行，示例代码如下：
```py
# 设置 window.navigator.webdriver 属性为 False
driver.execute_script("Object.defineProperties(navigator,{webdriver:{get:()=>undefined}})")

注：括号里面的js脚本一定要用Object.defineProperties(navigator,{webdriver:{get:()=>undefined}})这句，
不能用Object.defineProperty(navigator, 'webdriver', {get: () => undefined})，
后面这句亲测无效
```

==此方法亲测有效==

2. 谷歌浏览器的话可以直接与浏览器的`Chrome DevTools Protocol (CDP)`进行交互，通过`cdp命令`去设置`window.navigator.webdriver`的属性为`undefined`，此方法只适用于谷歌浏览器，并且此方法需要在浏览器启动之前执行，也就是`driver.get()`方法之前执行，示例代码如下：
```py
# 这行代码的作用是将webdriver这个属性置为undefined
driver.execute_cdp_cmd('Page.addScriptToEvaluateOnNewDocument', {'source': 'Object.defineProperty(navigator, "webdriver", {get: () => undefined})'
})
```

## 3. Cookie池
**为什么要设计Cookie池**

即为什么要将 模拟登录 和 爬虫服务 分离开来

1. 登录才能访问的网站越来越多，需要拿到Cookie才能访问
2. 单账号会受到访问频率限制
3. 模拟登录逻辑复杂
4. 登录可能采用不同的语言开发——node.js的puppeteer
5. 同一种语言的不同版本

**Cookie池的优点**

1. 服务分离——多语言开发
2. 组件分离——比如Redis可以换成mysql等
3. 服务分别部署，防止网站变化导致的爬虫宕机

### 3.1 Cookie池系统原理
![Cookie池系统原理](<sp-img/Screenshot from 2024-06-18 17-22-03.png>)
- Cookie检测服务——检测Cookie的有效性
- Redis——爬虫会随机从Redis当中抽取Cookie

### 3.2 Cookie池具体实现

**面临的挑战**

1. 如何发现Cookie池不够了
2. 各个网站的Cookie如何分开进行管理
3. 如何及时发现某个Cookie失效了
4. 新加入的网站如何快速接入系统
5. 统一配置

## 4. IP代理池

---
# 【引用】
只在点击按钮的时候校验 element 点击按钮进行智能验证 https://blog.51cto.com/u_12195/10186959