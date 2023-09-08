### 32、margin值设置百分比相对那个而言？
相对于父元素的宽度而言的

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    * {
      margin: 0;
      padding: 0;
    }

    .parent {
      width: 500px;
      height: 300px;
      background-color: pink;
    }

    .child {
      width: 200px;
      height: 100px;
      /* 50px 25px */
      margin: 10% 5%;
      background-color: red;
    }
  </style>
</head>

<body>
  <div class="parent">
    <div class="child"></div>
  </div>
</body>

</html>
```


### margin 的上下左右设置百分比有什么不一样？
没有get到面试官的点，我觉得上下左右设置百分比没什么不一样，都是相对父元素的 `width` 而言的