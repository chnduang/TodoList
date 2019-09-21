<template>
  <div class="home">
    <header>
      <h1>TODO-LIST</h1>
    </header>
    <div class="input-group mb-3 my-input">
      <input type="text" class="form-control" 
        placeholder="添加你的任务" 
        aria-label="Recipient's username" 
        aria-describedby="basic-addon2"
        autofocus 
        @keyup.enter="handleAddList"
        v-model="inputValue"
      />
      <div class="input-group-append my-button">
        <button 
        type="button" 
        class="btn btn-outline-primary" 
        @click="handleAddBtnList"
        >
        添加
        </button>
      </div>
    </div>
    <Content
      :type="type"
      :list="selectList"
      :CountUnComputed="getCountUnComputed"
      @delete="handleDelete"
      @complete="handleComplete"
      @delcomplete="handleDeleteAllComplete"
      @checked="handleCheckedAll"
      @all="handleSelectAll"
      @has="handleSelectComplete"
      @un="handleSelectUnComplete"  
      @edit="handleClickEdit"
      @bledit="handleEditBlur"
      @cancel="handleEditCancel"
      @save="handleInputEdit"
    />
    <div class="home-footer">
      <span>共{{getCountAllComputed}}项任务</span>,
      <span class="complete">已完成{{getCountComputed}}项</span>,
      <span class="unComplete">未完成{{getCountUnComputed}}项</span>
    </div>
  </div>
</template>

<script>
import Content from '@/components/Content.vue'
//存取localStorage中的数据
const store = {
    saveList(key,value){
        window.localStorage.setItem(key,JSON.stringify(value));
    },
    fetchList(key){
     return JSON.parse(window.localStorage.getItem(key))||[];
    }
}
const list = store.fetchList("storeData");

// const list = [{
//   id: 0,
//   content:"todo1",
//   complete: true,
//   edit: false
// },{
//   id: 1,
//   content:"todo2",
//   complete: false,
//   edit: false
// }];
export default {
  name: 'Home',
  components: {
    Content,
  },
  data () {
    return {
      list,
      inputValue: "",
      type: "All",
      originContent: ""
    }
  },
  watch:{
    // list:function(){
    //   store.saveList("storeData",this.list);
    // }
    list:{
      handler(){
          store.saveList("storeData",this.list);
      },
      deep:true
    }
  },
  methods:{
    handleAddList(e){
      const last = this.list[this.list.length - 1];
      const id = last ? last.id + 1 : 1;
      if(e.target.value){
        this.list.push({
          id: id, 
          content: e.target.value, 
          complete: false,
          edit: false
        });
        this.inputValue= "";
      }else{
        alert('您未输入任何任务');
      }
    },
    handleAddBtnList(){
      const last = this.list[this.list.length - 1];
      const id = last ? last.id + 1 : 1;
      if(this.inputValue){
        this.list.push({
          id: id, 
          content: this.inputValue, 
          complete: false,
          edit: false
        });
        this.inputValue= "";
      }else{
        alert('您未输入任何任务');
      }
    },
    handleDelete (index){
      const conf = confirm("确定删除这项吗");
      if (conf){
        this.list.splice(index,1);
      }else{
        alert("删除取消");
      }
    },
    handleComplete(item){
      item.complete = !item.complete;
    },
    handleDeleteAllComplete() {
      const confDelete = confirm("确定删除所选项吗");
      if (confDelete){
        this.list = this.list.filter(item => {
          return !item.complete;
        });
      }else{
        alert("删除取消");
      }
    },
    handleCheckedAll(e){
      this.list.forEach(item => {
          item.complete = e.target.checked;
      });
    },
    handleSelectAll(){
      this.type = "All";
    },
    handleSelectComplete(){
      this.type = "Complete";
    },
    handleSelectUnComplete(){
      this.type = "UnComplete";
    },
    handleClickEdit(item){
      item.edit = !item.edit;
      this.originContent = item.content;
    },
    handleEditCancel(item){
      item.edit = false;
      item.content = this.originContent;
    },
    handleEditBlur(item){
      item.edit = false;
    },
    handleInputEdit(item, index, e){
      if (!e.target.value.length) {
        return this.list.splice(index, 1);
      }
      item.content = e.target.value;
      item.edit = false;
      item.complete = false;
    }
  },
  computed: {
    getCountComputed() {
      return this.list.filter(item => {
          return item.complete;
      }).length;
    },
    getCountAllComputed() {
      return this.list.length
    },
    getCountUnComputed() {
      return this.list.filter(item => {
          return !item.complete;
      }).length;
    },
    selectList() {
      switch (this.type){
        case "UnComplete":
          return this.list.filter(item => {
            return !item.complete;
          });
        case "Complete":
          return this.list.filter(item => {
            return item.complete;
          });
        case "All":
          return this.list;
        default:
          return this.list;
      }
    }
  },
}
</script>
<style lang="less" scoped>
  .home{
    display: flex;
    flex-direction: column;
    align-items: center;
    header{
      margin-bottom: 5%;
      padding-top: 1%;
      h1{
        color: #fff;
        font-size: px;
      }
    }
    .my-input{
      width: 35%;
      input{
        border-radius: 4%;
      }
      .my-button{
        margin-left: 30%;
        button{
          border-radius: 10%;
          color: #fff;
        }
      }
    }
    .home-footer{
      width: 35%;
      padding: 1% 0;
      background:oldlace;
      .complete{
        color:rgb(75, 17, 209);
      }
      .unComplete{
        color: #f00;
      }
    }
  }
</style>
