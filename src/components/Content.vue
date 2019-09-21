<template>
    <div class="navbar-text navbar-right content">
      <div class="my-header">
        <span>任务列表</span>
        <div>
          <button type="button" class="btn" :class="{'btn-primary':type==='All'}" @click="handleSelectAll">全部</button>
          <button type="button" class="btn" :class="{'btn-primary':type==='Complete'}" @click="handleSelectComplete">已完成</button>
          <button type="button" class="btn" :class="{'btn-primary':type==='UnComplete'}" @click="handleSelectUnComplete">未完成</button>
        </div>
      </div>
      <div class="container">
        <div class="list" v-for="(item,index) of list" :key="item.id">
          <div class="check">
            <input type="checkbox" v-model="item.complete" class="checkBox" />
            <span 
            class="checkText" 
            :class="{contentText:item.complete}"
            @dblclick="handleEdit(item,index)"
            >
            {{item.content}}
            </span>
            <input class="edit" 
            v-show="item.edit"
            v-model="item.content"
            @keyup.enter="handleInputEdit(item, index, $event)"
            @keyup.esc="handleEditCancel(item)"
            @blur="handleEditBlur(item)"
            >
          </div>
          <div class="container-btn">
            <span class="iconfont edit-icon" @click="handleEdit(item,index)">&#xe600;</span>
            <button 
            type="button" 
            class="btn" 
            :class="{'btn-success' : item.complete,'btn-warning': !item.complete}"
            @click="handleComplete(item)"
            >
            {{item.complete?'完成':'未完成'}}
            </button>
            <button type="button" class="btn btn-danger" @click="handleDeleteList(index)">删除</button>
          </div>
        </div>
      </div>
      <div class="my-footer">
        <input type="checkbox" :checked="!CountUnComputed" @click="handleChecked" />
        <button type="button" 
        class="btn btn-primary" 
        :disabled="CountUnComputed === list.length" 
        @click="handleDeteleAll"
        >
        删除所选
        </button>
      </div>
    </div>
</template>

<script>
export default {
  name: 'Content',
  props: {
    list: Array,
    CountUnComputed: Number,
    type: String,
  },
  methods: {
    handleDeleteList(index){
      this.$emit('delete',index);
    },
    handleComplete(item) {
      this.$emit('complete',item);
    },
    handleDeteleAll(){
      this.$emit('delcomplete');
    },
    handleChecked(e){
      this.$emit('checked',e);
    },
    handleSelectAll(){
      this.$emit('all');
    },
    handleSelectComplete(){
      this.$emit('has');
    },
    handleSelectUnComplete(){
      this.$emit('un');
    },
    handleEdit(item){
      this.$emit('edit',item);
    },
    handleEditCancel(item){
      this.$emit('cancel',item);
    },
    handleEditBlur(item){
      this.$emit('bledit',item);
    },
    handleInputEdit(item, index, $event){
      this.$emit('save',item,index,$event);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>
// /deep/
  .content{
    width: 35%;
    border: 1px solid #ccc;
    box-sizing: border-box;
    padding: 0;
    .my-header{
      width: 100%;
      display: flex;
      justify-content: space-between;
      background: #eee;
      box-sizing: border-box;
      padding: 1%;
    span{
      vertical-align: middle;
    }
    }
    .container{
      padding: 0;
      .list{
        width: 100%;
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 1%;
        border-bottom: 1px solid #eee;
        .check{
          flex: 1;
          position: relative;
          .edit{
            position: absolute;
            left:18px;
            top: 1px;
            width: 268px;
          }
          .checkBox{
            display: inline-block;
            border-radius: 1%;
            margin-right: 2%;
          }
          .contentText{
            color: red;
            text-decoration: line-through;
          }
          .checkText{
            display: inline-block;
            width: 268px;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
          }
        }
        .container-btn{
          width: 35%;
          text-align: right;
          .edit-icon{
            cursor: pointer;
            margin-right: 8px;
            font-size: 20px;
          }
        }
      }
    }
    .my-footer{
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 1.6%;
    }
  }

</style>
