<template>
<div class="">
  <el-row>
    <el-col :span="24">
      <div class="grid-content bg-purple-dark"></div>
      <el-form ref="form" :model="form" label-width="80px" style="float:left">
        <el-form-item
           v-for="(domain, index) in form.domains"
            prop="region">
            <el-input placeholder="请输入规格" v-model="input3" style="width:250px">
             <template slot="prepend">规格名</template>
           </el-input>

         </el-form-item>
         <el-button v-if="this.addNum <= 1"  @click="addSku" icon="el-icon-plus" type="primary" round>新增规格</el-button>
       </el-form>
    </el-col>


  </el-row>
</div>

</template>
<script >
  export default {
    data() {
      return {
         addNum: 0,
         form: {
           domains: [{
             value: ''
           }],
       },
      }
    },
    methods: {
      addSku() {
         if (this.addNum <= 1) {
           this.form.domains.push({
             value: '',
             key: Date.now()
           });
         }
         this.addNum = this.addNum + 1;
        },
    }
  }
</script>
<style media="screen">
  .el-row {
    margin-bottom: 20px;
    &:last-child {
      margin-bottom: 0;
    }
  }
  .el-col {
    border-radius: 4px;
  }
  .bg-purple-dark {
    background: #99a9bf;
  }
  .bg-purple {
    background: #d3dce6;
  }
  .bg-purple-light {
    background: #e5e9f2;
  }
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
  }
  .row-bg {
    padding: 10px 0;
    background-color: #f9fafc;
  }
</style>
