### Thymeleaf的使用

#### 1、th:fragment

```html
<head th:fragment="head(title)">    
    <meta charset="UTF-8"> 
    <meta name="viewport" content="width=device-width, initial-scale=1.0">    
    <title th:text="${title}">博客详情</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/semantic-ui/2.2.4/semantic.min.css">
    <link rel="stylesheet"  th:href="@{/css/typo.css}">
    <link rel="stylesheet"  th:href="@{/css/animate.css}">
    <link rel="stylesheet"  th:href="@{/lib/prism/prism.css}">
    <link rel="stylesheet"  th:href="@{/lib/tocbot/tocbot.css}">
    <link rel="stylesheet"  th:href="@{/css/me.css}">
</head>
```

可以定义html中可重用的代码，还可以在使用的地方传入参数。比如上述代码中定义可重复使用的head，并可以在使用的地方传入标题名。

```html
<head th:replace="_fragments::head('首页')"></head>
```

可以使用th:replace向页面中插入fragment。以上代码等同第一段代码。

#### 2、th:block 

```html
<th:block th:fragment="script">
   <script src="../static/lib/tocbot/tocbot.min.js"th:src="@{/lib/tocbot/tocbot.min.js}">	</script>
   <script src="../static/lib/qrcode/qrcode.min.js"th:src="@{/lib/qrcode/qrcode.min.js}">	</script>
</th:block>
```

定义一个区域，但只显示被包裹的内容。

#### 3、th:if、th:unless

​	th:if： 标签只有在th:if中条件成立时才显示。

​	th:unless: 表达式中的条件不成立，标签才会显示其内容。 （可用于表单验证）

```html
<div class="ui mini message negative" th:unless="${#strings.isEmpty(message)}" th:text="${message}"></div>
```