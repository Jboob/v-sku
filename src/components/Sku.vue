<template>
<div class="">
  <el-row>
    <el-col :span="24">
      <div class="grid-content bg-purple-dark"></div>
      <el-form ref="form" :model="form" label-width="80px" >
        <el-form-item
           v-for="(domain, index) in form.domains"
           :key="domain.key"
            prop="region">
            <el-row type="flex" justify="start">
              <el-input placeholder="请输入规格" v-model="domain.value" style="width:250px">
               <template slot="prepend">规格名</template>
               </el-input>
               <v-if v-if="index > 0">
                  <el-button style="margin-left:10px"  @click.prevent="removeDomain(domain)" icon="el-icon-delete"  type="danger" round>删除规格</el-button>
                </v-if>
                <el-checkbox style="margin-left:10px" v-if="index == 0" @change="handleCheck" v-model="specChecked">添加规格图片</el-checkbox>
            </el-row>
             <el-row type="flex" justify="start">
               <el-tag
                 :key="tag"
                 v-for="(tag, key) in domain.sku"
                 closable
                 :disable-transitions="false"
                 @close="handleClose(domain.sku,tag)"
                 :hit="true"
                 size="medium"
                 >
                 {{tag}}
               </el-tag>
               <el-input
                 class="input-new-tag"
                 v-if="domain.inputVisible"
                 v-model="domain.inputValue"
                 ref="saveTagInput"
                 size="small"
                 @keyup.enter.native="handleInputConfirm"
                 @blur="handleInputConfirm"
                 >
               </el-input>
               <el-button v-else class="button-new-tag" icon="el-icon-plus" size="small" @click="showInput(index)">添加属性</el-button>
             </el-row>
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
         clickTag: -1,
         specChecked:false,
         addNum: 0,
         form: {
           domains: [{
             inputValue:'',
             inputVisible: false,
             value: '',
             sku:[]
           },
          ],
         },
         input3:"",
         dynamicTags: [],
         inputVisible: false,
         inputValue: ''
      }
    },
    methods: {
      addSku() {
         if (this.addNum <= 1) {
           this.form.domains.push({
             inputVisible: false,
             inputValue:'',
             value: '',
             key: Date.now(),
             sku:[]
           });
         }
         this.addNum = this.addNum + 1;
        },
        handleClose(sku,tag) {
          sku.splice(sku.indexOf(tag), 1);
        },
        showInput(tag) {
          this.clickTag = tag;
          console.log( JSON.stringify(this.clickTag, null, 4));
          console.log( JSON.stringify(this.form.domains, null, 4));
          for (var i = 0; i < this.form.domains.length; i++) {
            if (this.clickTag == i) {
              let domain = this.form.domains[i];
              domain.inputVisible = true;
            }
          }
        },

      handleInputConfirm() {
          for (var i = 0; i < this.form.domains.length; i++) {
              console.log(this.clickTag +"=====" +i);
            if (this.clickTag == i) {
              let domain = this.form.domains[i];
              let inputValue = domain.inputValue;
              if(inputValue){
                console.log( JSON.stringify(inputValue, null, 4));
                domain.sku.push(inputValue);
                domain.inputValue = '';
                domain.inputVisible = false;
              }
            }
          }
      },
      handleCheck(value){
       console.log("check:"+value);
       if (!value) {
         this.ruleForm.fileList = [];
       }
     },
     removeDomain(item) {
       if(this.addNum > 0){
         this.addNum = this.addNum - 1;
         var index = this.form.domains.indexOf(item)
         if (index !== -1) {
           this.form.domains.splice(index, 1)
         }
       }
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
  .el-tag + .el-tag {
    margin-left: 10px;
  }
  .button-new-tag {
    margin-left: 10px;
    height: 32px;
    line-height: 30px;
    padding-top: 0;
    padding-bottom: 0;
  }
  .input-new-tag {
    width: 90px;
    margin-left: 10px;
    vertical-align: bottom;
  }
</style>
