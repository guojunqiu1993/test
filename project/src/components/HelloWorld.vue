<template>
  <div class="hello">
    <el-button type="success" @click="buttonSet()">成功按钮</el-button>


    <el-dialog  :visible.sync="meanVisible">
    <el-form class="commonForm" label-width="130px" :rules="meanFormRules" @close="meanCloseDialog('meansData')" :model="meansData" ref="meansData" style="width:97%;margin-top:0px;">
      <el-form-item label="资料项名称" prop="mean_name">
        <el-input v-model="meansData.mean_name" ></el-input>
      </el-form-item>
      <el-form-item label="审核类型" prop="audit_type">
        <el-select v-model="meansData.audit_type" placeholder="请选择" style="width:100%;">
            <el-option
              v-for="item in auditTypeArrOptions"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="计分类型" prop="score_type">
        <el-select v-model="meansData.score_type" placeholder="请选择" style="width:100%;">
            <el-option
              v-for="item in scoreTypeArrOptions"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="重复上传" prop="upload_repeat">
        <el-select v-model="meansData.upload_repeat" placeholder="请选择" style="width:100%;">
            <el-option
              v-for="item in uploadRepeatArrOptions"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="规则编辑">
        <el-table :data="uploadEditData" style="width: 100%" align="center">
          <el-table-column label="部门类型" width="100" show-overflow-tooltip>
            <template slot-scope="scope">
              <span v-show="scope.row.dept_type==1">管理员部门</span>
              <span v-show="scope.row.dept_type==2">发言人部门</span>
            </template>
          </el-table-column>
          <el-table-column label="计分规则" align="center">
            <el-table-column label="单次分值" align="center" width="100px">
              <template slot-scope="scope">
                  <el-input v-model="scope.row.single_score"  oninput = "if(value.length>4)value=value.slice(0,4);if(value==0)value='';value= value.match(/\d+(\.\d{0,1})?/) ? value.match(/\d+(\.\d{0,1})?/)[0]  : '';value=value.replace(/[^\d.]/g,'')"></el-input>
              </template>
            </el-table-column>
            <el-table-column label="计分上限" align="center" width="100px">
              <template slot-scope="scope">
                  <el-input  v-model="scope.row.score_up_limit" oninput = "if(value.length>4)value=value.slice(0,4);if(value==0)value='';value= value.match(/\d+(\.\d{0,1})?/) ? value.match(/\d+(\.\d{0,1})?/)[0]  : '';value=value.replace(/[^\d.]/g,'')"></el-input>
            
              </template>
            </el-table-column>
            <el-table-column label="计分周期" align="center">
                <template slot-scope="scope">
                    <el-select v-model="scope.row.score_cycle_type" placeholder="请选择">
                        <el-option
                          v-for="item in scoreCircleArrOptions"
                          :key="item.value"
                          :label="item.label"
                          :value="item.value">
                        </el-option>
                    </el-select>
                </template>
            </el-table-column>
            <el-table-column label="计分延期" align="center" width="100px">
              <template slot-scope="scope">
                  <el-input v-model="scope.row.score_delay" oninput = "if(value.length>3)value=value.slice(0,3);if(value==0)value='';value=value.replace(/[^\d]/g,'')"></el-input>
                  
              </template>
            </el-table-column>
          </el-table-column>
      </el-table>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button >取 消</el-button>
      <el-button >确 定</el-button>
    </div>
  </el-dialog>

  </div>
  
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      means_word:'',
          filesData:[],
          meanVisible:false,
          meanFormRules:{
            mean_name:[
              { required: true, trigger: 'blur' }
            ]
          },
          meansData:{
            mean_name:'',
            audit_type:'1',
            score_type:'1',
            upload_repeat:'1',
            upload_data_id:''
          },
          auditTypeArrOptions:[
            {'label':'自动审核','value':'1'},
            {'label':'手动审核','value':'2'}
          ],
          scoreTypeArrOptions:[
            {'label':'加分','value':'1'},
            {'label':'扣分','value':'2'}
          ],
          uploadRepeatArrOptions:[
            {'label':'支持','value':'1'},
            {'label':'不支持','value':'0'}
          ],
          uploadEditData:[
            {
              dept_type:'1',
              single_score:'',
              score_up_limit:'',
              score_cycle_type:1,
              score_delay:''
            },
            {
              dept_type:'2',
              single_score:'',
              score_up_limit:'',
              score_cycle_type:1,
              score_delay:''
            }],
          scoreCircleArrOptions:[
            {'label':'按周','value':1},
            {'label':'按两周','value':2},
            {'label':'按月','value':3},
            {'label':'按当前考核期','value':4}
          ],
    }
  },
  methods: {
    buttonSet(){
      this.meanVisible=true
      this.uploadEditData=[
      {
        dept_type:'1',
        single_score:'',
        score_up_limit:'',
        score_cycle_type:1,
        score_delay:''
      },
      {
        dept_type:'2',
        single_score:'',
        score_up_limit:'',
        score_cycle_type:1,
        score_delay:''
      }]
    },
    meanCloseDialog(formData){
      this.meanVisible=false
      this.$refs[formData].clearValidate()
      
    },

  }
}
</script>


<style scoped>

</style>


<!-- 产品要求 
    单次分值:它是正数,并且1到4个字符串,可以为小数 只能保留一位  oninput = "if(value.length>4)value=value.slice(0,4);if(value==0)value='';value= value.match(/\d+(\.\d{0,1})?/) ? value.match(/\d+(\.\d{0,1})?/)[0]  : '';value=value.replace(/[^\d.]/g,'')"
    计分上限:它是正数,并且1到4个字符串,可以为小数 只能保留一位
    计分延期:它是正数,并且1到3个字符串,不可以为小数


    我的问题：我测试发现有时候数字自动变回来,当我输入很快的时候 好奇怪 或者你有没有其他方式去限制也行 满足产品要求就行
-->
