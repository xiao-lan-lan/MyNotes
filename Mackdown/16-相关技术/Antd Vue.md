# Antd Vue 

## a-table

```html
<a-table 
	:columns="columns"  // 表头
	:dataSource="data"	//表格数据
	rowKey="id"	//行id，没有会报警告
>
  <a slot="name" slot-scope="text,record">{{text}}</a>
  // text 是 slot 数据，record 是整个对象数据
</a-table>
```

```js
const columns = [
    {
      dataIndex: 'name', //表头字段名
      key: 'name',
      scopedSlots: { customRender: 'name' }, //插槽
      customRender: val => {
        const obj = {
          children: val,
          attrs: {}
        }
        if(val === '总计') {
          obj.attrs.colSpan = 0
        }
        return obj
      }, // 合并单元格
      width:'xx%' // 宽度设置百分数，表格滚动大一些，表格一般可以对齐
    },
    {
      title: 'Age',
      dataIndex: 'age',
      key: 'age',
    }
  ];
  const data = [
    {
      key: '1',
      name: 'John Brown',
      age: 32
    },
    {
      key: '2',
      name: 'Jim Green',
      age: 42
    }
  ];
	export default {
    data() {
      return {
        data,
        columns,
      };
    },
  };
```

## 树形控件

```html
<a-tree
    :treeData="treeData"
    @select="this.onSelect"
    @check="this.onCheck"
    :replaceFields="replaceFields"
 />
```

```js
// 数据格式 正常是 key title children
const treeData = [
    {
      name: 'parent 1',
      key: '0-0',
      child: [
        {
          name: '张晨成',
          key: '0-0-0',
          disabled: true,
          child: [
            { name: 'leaf', key: '0-0-0-0', disableCheckbox: true },
            { name: 'leaf', key: '0-0-0-1' },
          ],
        },
        {
          name: 'parent 1-1',
          key: '0-0-1',
          child: [{ key: '0-0-1-0', name:'zcvc' }],
        },
      ],
    },
  ];

export default {
    data() {
      return {
        treeData,
        replaceFields:{ //自定义数据字段
          children:'child',
          title:'name'
        }
      };
    },
    methods: {
      onSelect(selectedKeys, info) {
        console.log('selected', selectedKeys, info);
      },
      onCheck(checkedKeys, info) {
        console.log('onCheck', checkedKeys, info);
      },
    },
  };
```

## 确认对话框

```html
<template>
  <a-button @click="showConfirm">
    Confirm
  </a-button>
</template>
```

```js
<script>
  export default {
    methods: {
      showConfirm() {
        this.$confirm({
          // 标题
          title: 'Do you want to delete these items?',
          // 内容
          content: 'this dialog will be closed after 1 second',
          // 点击确认
          onOk() {
            return new Promise((resolve, reject) => {
              setTimeout(Math.random() > 0.5 ? resolve : reject, 1000);
            }).catch(() => console.log('Oops errors!'));
          },
          // 点击取消
          onCancel() {},
        });
      },
    },
  };
</script>
```

