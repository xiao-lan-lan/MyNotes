# MockJS

[官方文档](<https://github.com/nuysoft/Mock/wiki/Getting-Started>)

## 安装

```bash
# 安装
npm install mockjs
```

## 创建 mock.js 文件

```js
// 使用 Mock
var Mock = require("mockjs");
// 参数一：请求地址要和axios的一致
// 参数二：请求方式要小写
// 参数三：回调函数，其中 return 请求要返回的数据
var mockdata = Mock.mock("http://localhost:8080/list", "get", ()=>{
  var list = []
  for (var i = 0; i < 10; i++) {
    var obj = {
      "id|+1": 1,
      date: Mock.Random.date(),
      name: Mock.Random.cname(),
      address: Mock.Random.county(true),
      tag: Mock.Random.csentence(1, 3)
    }
    list.push(obj)
  }
  return list
});

// 输出结果
console.log(mockdata);
```

## 在 main.js 中引入

```js
import './mock'
```

## 在组件中使用

```js
    data() {
      return {
         tableData: []
      };
    },

  methods: {
    loadList() {
      axios({
        method: "get",
        url: "http://localhost:8080/list"
      }).then(res => {
        this.tableData=res.data
      });
    }
  },
      
  created() {
    this.loadList();
  },
```

## mock数据

```js
var data = Mock.mock({
    // 属性 list 的值是一个数组，其中含有 1 到 10 个元素
    'list|1-10': [{
        // 属性 id 是一个自增数，起始值为 1，每次增 1
        'id|+1': 1
    }],
  	// 随机日期
  	'date':Mock.Random.date()
})
```