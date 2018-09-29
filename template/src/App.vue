<template>
  <div id="app">

    <div class="Dialogbox" v-show="Isshow">
      <button @click="close" class="close_btn">X</button>
      <div class="box" >
        <div class="left-box">
          <p>待选人员：</p>
          <el-tree :data="data" :props="defaultProps" @node-click="handleNodeClick"></el-tree>
        </div>

        <button class="add" @click="add">添加</button>
        <div class="right">
          <p>已选人员：</p>
          <ul class="content">
            <li v-for="(key,index) in arr" :key="key">{{key}}
              <button @click="deletedata(key)" class="delete_btn">X</button>
            </li>
          </ul>
        </div>

      </div>
      <div class="btn_box"><button class="btn"  @click="confirm">确定</button><button class="btn" @click="cancel">取消</button></div>
    </div>

    <div class="inputbox">
      <button class="empty" @click="empty">清空</button>
      <input type="textarea" class="inputconten" v-model="val" @focus="ISSHOW"/>
    </div>
  </div>
</template>
<script>

  export default {
    name: 'App',
    created(){
      this.arr = JSON.parse(localStorage.getItem("data")) || [];
    },
    data() {
      return {
        Isshow:false,
        arr: [],
        val:"",
       currdata:"",
        data: [{
          label: '集团总部',
          children: [{
            label: '行政部',
            children: [{
              label: '人力资源招聘'
            }]
          }]
        }, {
          label: '华北一区',
          children: [{
            label: '市场一部',
            children: [{
              label: '石家庄组'
            }]
          }, {
            label: '市场二部',
            children: [{
              label: '太原组'
            }]
          }]
        }, {
          label: '国美互联网公司',
          children: [{
            label: '技术部',
            children: [{
              label: '前端组'
            }]
          }, {
            label: '产品组',
            children: [{
              label: '项目一组'
            }]
          }]
        }],
        defaultProps: {
          children: 'children',
          label: 'label'
        }
      };
    },
    methods: {
      close(){
             this.Isshow=false;
      },
      handleNodeClick(data,aa,qq) {
       this.currdata=data.label
      },
      add(){
        if (this.arr.every(item => this.currdata !== item)) {
          this.arr.push(this.currdata)
        }
      },
      deletedata(key){
        this.arr = this.arr.filter(item => item != key)
        localStorage.setItem("data", JSON.stringify(this.arr))
      },
      empty(){
        localStorage.clear("data");
        this.arr = [];
        this.val=""
      },
      ISSHOW(){
          this.Isshow=true;
      },
      confirm(){
        let str = "";
        this.arr.forEach((item, index) => {
          str += item + "；  "
        })
        this.val=str;
        localStorage.setItem("data", JSON.stringify(this.arr))
        this.Isshow=false;
      },
      cancel(){
        this.Isshow=false;
        this.arr = JSON.parse(localStorage.getItem("data")) || [];
      }
    }
  }
</script>

<style>
  /*如果是公共样式，最好写在App.vue 中*/
  * {
    margin: 0;
    padding: 0;
  }
  .Dialogbox{
    min-width:650px;
    padding: 10px;
    width:60%;
    margin:0 auto;
    border: 1px solid blue;
    position: relative;
    background:#F0F0F0;
    padding-bottom:50px;
    box-sizing: border-box;
  }
  ul li {
    list-style: none
  }

  a {
    text-decoration: none;
  }

  .box {
    display: flex;
    clear: both;
  }

  .el-tree {
    min-height: 400px;
    width: 300px;
    border: 1px solid #333;
  }
.right{
   width:35%;
}
  .content {
    width:100%;
    min-height: 400px;
    flex: 1;
    border: 1px solid #333;
    background:#fff;
    padding-left:10px;
  }
.content>li{
  width:200px;
  height:35px;
  line-height:35px;
}
.close_btn{
  float: right;
  padding: 5px;
  opacity:.2;
}
  .close_btn:hover{
    opacity:1;
  }
  .delete_btn {
    float: right;
    padding: 5px;
  }
  .delete_btn:hover {
    background: red;
  }
  .inputbox {

    width:60%;
    min-width:650px;
    margin:0 auto;
    margin-top: 20px;
    position: relative;
    height: 100px;
  }

  /*input*/
  .inputconten {
    display: block;
    height:44px;
    width: 100%;
    vertical-align: top;
    padding-left: 45px;
    box-sizing:border-box;
  }

  .empty {
    outline:none;
    display: none;
    width: 45px;
    height:44px;
    border:none;
    background: #F0F0F0;
    position: absolute;
    left: 0;
    top: 0;
  }
  .empty:hover{
    background: red;
    color:#fff;
  }
  .inputbox:hover > .empty {
    display: block;
  }
  .btn_box{
    margin-top: 10px;
    text-align:center;
  }
.btn{
   margin-left:50px;
   height:50px;
   width: 100px;
   border:1px solid #333;
   border-radius: 10px;
   background:#fff;
   outline: none;
 }
  .btn:active{
    border: none;
    height:50px;
    width: 100px;
    border:1px solid green;
    background:green;
    color:#fff;
    /*border-radius: 10px;*/
  }
  .el-tree-node__content>.el-tree-node__expand-icon {
    padding-left: 12px;
    padding-right:12px;
    padding-top: 6px;
    padding-button: 6px;
  }
  .add {
    margin: 10px;
    margin-top: 20px;
    height: 40px;
    width: 60px;
    padding:5px;
  }
  .el-tree-node:focus>.el-tree-node__content {
    background-color:red;
  }
</style>
