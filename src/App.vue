<template>
  <div class="todoapp">
    <button class="clear-btn" @click="clearDoneAndFail">🧹清除已完成/失败事务</button>
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
  <div class="filter-bar">
    <label>筛选：</label>
    <select v-model="filter">
      <option value="all">全部</option>
      <option value="active">⭕待办 & ❗进行中</option>
      <option value="completed">✅已完成 & ❌失败</option>
    </select>
  </div>
  <div class="affair-container">
   <div class="affair-item" v-for="item in filteredList" :key="item.id">
      <select class="affair-select" :class="statusClass(item.status)" v-model="item.status">
        <option value="1" >⭕待办</option>
        <option value="2" >❗进行中</option>
        <option value="3" >✅已完成</option>
        <option value="4" >❌失败</option>
      </select>
      <div class="affair-list" :class="statusClass(item.status)">
        <span class="affair-name">{{item.name}}</span>
      </div>
      <button class="delete-btn" @click="deleteAffair(item.id)">删除</button>
   </div>
  </div>
</template>

<script setup>
import { ref, watch, computed } from 'vue'
const affair = ref('')
const affairlist = ref(JSON.parse(localStorage.getItem('affairlist') || '[]'))
watch(affairlist, (val) => {
  localStorage.setItem('affairlist', JSON.stringify(val))
}, { deep: true })
const addAffair = () => {
  if (!affair.value.trim()) return
  affairlist.value.push({ id: Date.now(), name: affair.value, status: '1' })
  affair.value = ''
}
const deleteAffair = (id) => {
  const idx = affairlist.value.findIndex(i => i.id === id)
  if (idx !== -1) affairlist.value.splice(idx, 1)
}
const statusClass = (status) => {
  return {
    'status-todo': status === '1',
    'status-doing': status === '2',
    'status-done': status === '3',
    'status-fail': status === '4'
  }
}
const clearDoneAndFail = () => {
  affairlist.value = affairlist.value.filter(item => item.status !== '3' && item.status !== '4')
}
const statusOrder = { '1': 0, '2': 1, '3': 2, '4': 3 }
const sortedList = computed(() => {
  return [...affairlist.value].sort((a, b) => {
    return statusOrder[a.status] - statusOrder[b.status]
  })
})
const filter = ref('all')
const filteredList = computed(() => {
  if (filter.value === 'all') return sortedList.value
  if (filter.value === 'active') return sortedList.value.filter(item => item.status === '1' || item.status === '2')
  if (filter.value === 'completed') return sortedList.value.filter(item => item.status === '3' || item.status === '4')
  return sortedList.value
})
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
  height: 42px;
  padding: 5px;
  font-size: 1em;
  box-sizing: border-box;
  border-radius:5px;
  border: 1px solid #999;
}
.todoapp{
  position: relative;
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
  box-shadow: 0 4px 12px rgba(0,0,0,0.15)

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
  word-break: break-all;
  overflow-wrap: break-word;
  min-width: 0;
}
.affair-container{
  width: 900px;
  margin: 15px auto 0;
}
.delete-btn{
  width: 48px;
  height: 48px;
  margin-left: auto;
  cursor: pointer;
  color: white;
  font-family:FangSong;
  font-size: 0.9em;
  background-color:red;
  border:none;
  border-radius:1000000px;
  box-shadow: 0 5px 10px rgba(0,0,0,0.15);
  transition: all 0.3s ease;
}
.delete-btn:hover{
  background-color: #cc0000;
  transform: scale(1.1);
  box-shadow: 0 8px 16px rgba(255,0,0,0.3);
}
.delete-btn:active{
  transform: translateY(0);
  box-shadow: 0 3px 6px rgba(0,0,0,0.15);
}
.clear-btn{
  position: absolute;
  top: 20px;
  right: 20px;
  width: auto;
  height: auto;
  padding: 10px 20px;
  background: rgba(255,255,255,0.9);
  border: none;
  border-radius: 20px;
  cursor: pointer;
  font-family: FangSong;
  font-weight: bold;
  color: #ff6b6b;
  box-shadow: 0 5px 10px rgba(0,0,0,0.15);
  transition: all 0.3s ease;
}
.clear-btn:hover{
  background: #fff;
  transform: translateY(-2px);
  box-shadow: 0 8px 16px rgba(0,0,0,0.2);
}
.clear-btn:active{
  transform: translateY(0);
  box-shadow: 0 3px 6px rgba(0,0,0,0.15);
}
.add-btn{
  cursor: pointer;
}
.filter-bar{
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  margin: 15px 0;
  font-family: FangSong;
  font-size: 1em;
}
.filter-bar select{
  padding: 8px 12px;
  border-radius: 6px;
  border: 1px solid #999;
  font-family: FangSong;
  font-size: 1em;
  cursor: pointer;
  background: #fff;
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
.affair-name{
  word-break: break-all;
  overflow-wrap: break-word;
}
.affair-list.status-done .affair-name,
.affair-list.status-fail .affair-name{
  text-decoration: line-through;
  color: rgba(126, 105, 105, 0.7);
}
</style>
<style>
body {
  background: linear-gradient(180deg, #ff9966, #ffcc99); 
  background-attachment: fixed;
}
</style>