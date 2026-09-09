<template>
  <div class="todoapp">
    <div class="title">Todo App</div>
  </div>
  <div class="add-deal">
    <input type="text" placeholder="Add New Deal..." v-model="deal"></input>
    <button class="add-btn">添加事务</button>
  </div>
  <div class="deal-container">
   <div class="deal-list">
      <input type="checkbox" class="deal-checkbox"></input>
      <span class="deal-name">Hi!欢迎来到Todo App</span>
      <button class="delete-btn">删除事务</button>
   </div>
   <div class="deal-list">
      <input type="checkbox" class="deal-checkbox"></input>
      <span class="deal-name">sleep</span>
      <button class="delete-btn">删除事务</button>
   </div>
   <div class="deal-list">
      <input type="checkbox" class="deal-checkbox"></input>
      <span class="deal-name">drink</span>
      <button class="delete-btn">删除事务</button>
   </div>
   
  </div>

</template>

<script setup>
</script>
<style scoped>
button{
  width: 100px;
  height: 40px;
  padding: 5px;
  box-sizing: border-box;
}
input{
  width: 950px;
  height: 40px;
  padding: 5px;
  font-size: 1em;
  box-sizing: border-box;
}
.title{
  text-align: center;
  font-size: 4em;
  font-weight: bold;
}
.add-deal{
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
}
.deal-list{ 
  width:800px;
  margin: 15px auto 0;
  display: flex;
  align-items: center;
  gap: 10px;
  border:1px solid #999;
  padding:12px 16px;
  border-radius:4px;
}
.deal-checkbox{
  width: 20px;
  height: 20px;
  cursor: pointer;
}
.deal-container{
  width: 900px;
  margin: 15px auto 0;
}
.delete-btn{
  margin-left: auto;
  cursor: pointer;
  }
.add-btn{
  cursor: pointer;
}
</style>
<style>
body{
  margin:0;
}
</style>