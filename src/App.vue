<template>
  <div class="todoapp">
    <div class="title">TodoList</div>
  </div>
  <div class="add-affair">
    <input type="text" placeholder="添加新事务..." v-model="affair" @keyup.enter="addAffair"></input>
    <button class="add-btn" @click="addAffair">确认添加</button>
  </div>
  <div class="affair-container">
   <div class="affair-item" v-for="(item, index) in affairlist" :key="index">
      <select class="affair-select" :class="statusClass(item.status)" v-model="item.status">
        <option value="1" >待办</option>
        <option value="2" >进行中</option>
        <option value="3" >已完成</option>
        <option value="4" >失败</option>
      </select>
      <div class="affair-list" :class="statusClass(item.status)">
        <span class="affair-name">{{item.name}}</span>
      </div>
      <button class="delete-btn" @click="deleteAffair(index)">删除</button>
   </div>
  </div>

</template>

<script setup>
import { ref } from 'vue'
const affair = ref('')
const affairlist = ref([])
const addAffair = () => {
  if (!affair.value.trim()) return
  affairlist.value.push({ name: affair.value, status: '1' })
  affair.value = ''
}
const deleteAffair = (index) => {
  affairlist.value.splice(index,1)
}
const statusClass = (status) => {
  return {
    'status-todo': status === '1',
    'status-doing': status === '2',
    'status-done': status === '3',
    'status-fail': status === '4'
  }
}
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
body{
  background: linear-gradient(to right, #dddddd, #999999);
}
.title{
  text-align: center;
  font-size: 6em;
  font-weight: bold;
  background-color: #333;
  color: #fff;
  font-family: 'Courier New', Courier, monospace;
}
.add-affair{
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 15px;
  gap: 10px;
}
.affair-item{
  display: flex;
  align-items: center;
  gap: 10px;
  margin: 15px 0;
}
.affair-list{ 
  flex: 1;
  display: flex;
  align-items: center;
  gap: 10px;
  border:1px solid #999;
  padding:15px 15px;
  border-radius:4px;
}
.affair-container{
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
.affair-select{
  width: 70px;
  height: 54px;
  padding: 5px;
  border-radius:4px;
  box-sizing: border-box;
  cursor: pointer;
}
.status-icon{
  margin-left: 8px;
}
.status-todo{
  background-color: #ddff00;
}
.status-doing{
  background-color: #00d9ff;
}
.status-done{
  background-color: #00fb15;
}
.status-fail{
  background-color: #ff3553;
}
</style>
<style>

</style>