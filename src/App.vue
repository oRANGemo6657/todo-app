<template>
  <div class="todoapp">
    <div class="title">TodoList</div>
    <div class="total" v-if="affairlist.length==0">
      当前无待办事务，快来添加吧!
    </div>
    <div class="total" v-else-if="affairlist.length>0">      
      当前共{{affairlist.length}}条事务,Go Go Go!
    </div>
  </div>
  <div class="add-affair">
    <input type="text" placeholder="添加新事务..." v-model="affair" @keyup.enter="addAffair"></input>
    <button class="add-btn" @click="addAffair">确认添加</button>
  </div>
  <div class="affair-container">
   <div class="affair-item" v-for="(item, index) in affairlist" :key="index">
      <select class="affair-select" :class="statusClass(item.status)" v-model="item.status">
        <option value="1" >⭕待办</option>
        <option value="2" >❗进行中</option>
        <option value="3" >✅已完成</option>
        <option value="4" >❌失败</option>
      </select>
      <div class="affair-list" :class="statusClass(item.status)">
        <span class="affair-name">{{item.name}}</span>
      </div>
      <button class="delete-btn" @click="deleteAffair(index)">删除</button>
   </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'
const affair = ref('')
const affairlist = ref(JSON.parse(localStorage.getItem('affairlist') || '[]'))
watch(affairlist, (val) => {
  localStorage.setItem('affairlist', JSON.stringify(val))
}, { deep: true })
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

.title{
  text-align: center;
  font-size: 6em;
  font-weight: bold;
  color: #fff;
  font-family: 'Courier New', Courier, monospace;
}
.total{
  text-align: center;
  font-family: YouYuan;
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
  background: rgb(252, 241, 196);
  border-radius:10px;
  padding: 20px;
  margin: 40px 0 auto;

}
.affair-list{ 
  flex: 1;
  display: flex;
  align-items: center;
  gap: 10px;
  border:1px solid #999;
  padding:15px 15px;
  border-radius:10px;
  font-family:FangSong;
  font-weight: bold;
}
.affair-container{
  width: 900px;
  margin: 15px auto 0;
}
.delete-btn{
  width: 50px;
  height: 50px;
  margin-left: auto;
  cursor: pointer;
  color:white;
  font-family:FangSong;
  font-size: 0.9em;
  background-color: red;
  border:none;
  border-radius:1000000px;
  }
.add-btn{
  cursor: pointer;
}
.affair-select{
  width: 102px;
  height: 54px;
  padding: 5px;
  border-radius:4px;
  font-family:FangSong;
  font-size: 1em;
}
.status-todo{
  background: linear-gradient(135deg, #ffff00, #ffd000);
}
.status-doing{
  background: linear-gradient(135deg, #00c8ff, #2088ff);
}
.status-done{
  background: linear-gradient(135deg, #00fb15, #00a86b);
}
.status-fail{
  background: linear-gradient(135deg, #ff3553, #c0392b);
}
.affair-list.status-done .affair-name,
.affair-list.status-fail .affair-name{
  text-decoration: line-through;
  color: rgba(126, 105, 105, 0.7);
}

</style>
<style>
body {
  /* 替换为你原来的渐变颜色 */
  background: linear-gradient(180deg, #ff9966, #ffcc99); 
  background-attachment: fixed;
}
</style>