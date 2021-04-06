# HTML

# CSS

# JS

# VUE

1. 点击按钮携带数据，路由跳转打开新页面

   ```js
   const url = this.$router.resolve({
   	path: `/budget/adjust/detail/${r.budgetId}/${r.instanceId}`,
   	query: {
   		isOpen: true
   	}
   })
   window.open(url.href, '_blank')
   ```