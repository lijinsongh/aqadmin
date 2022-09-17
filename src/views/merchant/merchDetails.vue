<template>
  <div class="merch_content">
    <div class="merch_item">
      <div class="merch_title">
        店铺信息
      </div>
      <div class="merch_main">
        <div class="merch_main_item">
          <span class="merch_item_left"> 店铺名称：</span>
          <span class="merch_item_right"> {{tableData.storeName}}</span>
        </div>
        <div class="merch_main_item">
          <span class="merch_item_left"> 店铺类型：</span>
          <span class="merch_item_right" v-if="tableData.storeType==1">个人</span>
          <span class="merch_item_right" v-if="tableData.storeType==2">个体工商户</span>
          <span class="merch_item_right" v-if="tableData.storeType==3">企业</span>
          <span class="merch_item_right" v-if="tableData.storeType==4">其他组织</span>
        </div>
        <div class="merch_main_item">
          <span class="merch_item_left"> 客服电话：</span>
          <span class="merch_item_right"> {{tableData.serverPhone}}</span>
        </div>
        <div class="merch_main_item">
          <span class="merch_item_left"> 店铺负责人：</span>
          <span class="merch_item_right"> {{tableData.legal}}</span>
        </div>
        <div class="merch_main_item">
          <span class="merch_item_left"> 负责人电话：</span>
          <span class="merch_item_right"> {{tableData.legalPhone}}</span>
        </div>
        <div class="merch_main_item">
          <span class="merch_item_left"> 店铺地址：</span>
          <span class="merch_item_right">{{tableData.storeAddressProvince}}{{tableData.storeAddressCity}}{{tableData.storeAddressDistrict}}{{tableData.storeAddressDetail}}</span>
        </div>
      </div>
    </div>
    <div class="merch_item" v-if="tableData.storeType >1">
      <div class="merch_title">
        主体信息
      </div>
      <div class="merch_main">
        <div class="merch_main_item">
          <span class="merch_item_left" v-if="tableData.storeType == 2">商户名称:</span>
          <span class="merch_item_left" v-if="tableData.storeType == 3">公司名称:</span>
          <span class="merch_item_left" v-if="tableData.storeType == 4">组织名称:</span>
          <span class="merch_item_right"> {{tableData.companyName}}</span>
        </div>
        <div class="merch_main_item">
          <span class="merch_item_left"> 营业执照编号：</span>
          <span class="merch_item_right"> {{tableData.companyCode}}</span>
        </div>
        <div class="merch_main_item">
          <span class="merch_item_left"> 公司注册地址省：</span>
          <span class="merch_item_right">
            {{tableData.companyAddressProvince}}{{tableData.companyAddressCity}}{{tableData.companyAddressDistrict}}{{tableData.companyAddressDetail}}
          </span>
        </div>
        <!-- <div class="merch_main_item">
          <span class="merch_item_left"> 营业期限：</span>
          <span class="merch_item_right"> {{tableData.companyTerm}}</span>
        </div> -->
        <div class="merch_main_item">
          <span class="merch_item_left"> 营业执照照片：</span>
          <img :src="tableData.companyLicenseImg" alt="" width="148" height="148">
        </div>
      </div>
    </div>
    <div class="merch_item">
      <div class="merch_title">
        经营者信息
      </div>
      <div class="merch_main">
        <div class="merch_main_item">
          <span class="merch_item_left"> 经营者姓名：</span>
          <span class="merch_item_right"> {{tableData.storeHead}}</span>
        </div>
        <div class="merch_main_item">
          <span class="merch_item_left"> 证件类型：</span>
          <span class="merch_item_right" v-if="tableData.idCardType==1">中国大陆居民身份证</span>
          <span class="merch_item_right" v-if="tableData.idCardType==2">中国香港居民来往内地通行证</span>
          <span class="merch_item_right" v-if="tableData.idCardType==3">中国澳门居民来往内地通行证</span>
          <span class="merch_item_right" v-if="tableData.idCardType==4">中国台湾居民来往内地通行证</span>
          <span class="merch_item_right" v-if="tableData.idCardType==5">其他国家或地区居民护照</span>
        </div>
        <div class="merch_main_item">
          <span class="merch_item_left"> 身份证号码：</span>
          <span class="merch_item_right">{{tableData.idCardNumber}}</span>
        </div>
        <!-- <div class="merch_main_item">
          <span class="merch_item_left"> 身份证有效期限：</span>
          <span class="merch_item_right"> {{tableData.idCardValidTimeStart}}—{{tableData.idCardValidTimeEnd}}</span>
        </div> -->
        <div class="merch_main_item">
          <span class="merch_item_left"> 证件照片：</span>
          <img :src="tableData.idCardImg1" alt="" width="148" height="148" style="margin-right:40px;">
          <img :src="tableData.idCardImg2" alt="" width="148" height="148">
        </div>
        <div class="merch_main_item">
          <span class="merch_item_left"> 手持证件照：</span>
          <img :src="tableData.idCardImg3" alt="" width="148" height="148">
        </div>
      </div>
    </div>
    <div style="text-align: center;">
      <el-button
        size="mini"
        type="primary"
        @click="prev">返回列表
      </el-button>
      <el-button
        v-if="tableData.status==1"
        size="mini"
        type="primary"
        @click="dispose()">处理
      </el-button>
    </div>
    <!-- 处理弹框-->
    <el-dialog title="处理审核" :visible.sync="dialogFormVisible" center>
    	<div style="margin-bottom: 10px;display: flex;">
    		<span style="width: 200px;display: inline-block;text-align: right;position: relative;top: -5px;">入驻处理：</span>
          <el-radio-group v-model="status" @change="statusChange">
             <el-radio :label="2">同意入驻</el-radio>
             <el-radio :label="3">拒绝入驻</el-radio>
          </el-radio-group>
    	</div>
      <!-- <div style="margin-bottom: 10px;" v-if="isshow">
      	<span style="width: 200px;display: inline-block;text-align: right;">生效日期：</span>
      	<el-date-picker
      	    style="width:215px;"
            v-model="info.stockDate"
      	    type="date"
      	    placeholder="生效日期">
      	</el-date-picker>
      </div>
       <div style="margin-bottom: 10px;" v-if="isshow">
        <span style="width: 200px;display: inline-block;text-align: right;">生效年限：</span>
        <el-input style="width:50%;" v-model="auditYears" placeholder="请输入生效年限"></el-input>&nbsp;&nbsp;&nbsp;&nbsp;年
       </div> -->
    	<div style="margin-bottom: 10px;" v-if="!isshow">
    		<span style="width: 200px;display: inline-block;text-align: right;position:relative;top: -70px;">拒绝理由：</span>
    		<el-input style="width:50%;" type="textarea" :rows="4" v-model="refundReason" placeholder="请输入拒绝理由"></el-input>
    	</div>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="disposeNoticeTo()">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        id:'',
        status:2,
        info: {
        	stockDate:this.getNowTime(),  //日期
        },
        isshow:true,
        auditYears:'',
        refundReason:'',
        tableDataLoading: true,
        dialogFormVisible:false,
        tableData: {},
      }
    },
    methods: {
      // 返回上一级
      prev(){
      	this.$router.back()
      },
      //处理默认选中当前日期
      getNowTime() {
      	 var now = new Date();
      	 var year = now.getFullYear(); //得到年份
      	 var month = now.getMonth(); //得到月份
      	 var date = now.getDate(); //得到日期
      	 month = month + 1;
      	 month = month.toString().padStart(2, "0");
      	 date = date.toString().padStart(2, "0");
      	 var defaultDate = `${year}-${month}-${date}`;
      	 return defaultDate;
      	 this.$set(this.info, "stockDate", defaultDate);
      },
      // 团长跳转详情页
      handleSizeChange (val) {
        this.size = val
        this.dataSelect()
      },
      handleCurrentChange (val) {
        this.page = val
        this.dataSelect()
      },
      // select自动选择
      selectTrigger () {
        this.dataSelect()
      },
      // 查询
      select () {
        this.dataSelect()
      },
      // 重置
      cleans () {
        this.companyName = ''
        this.legalPhone = ''
        this.storeType = 0
        this.dataSelect()
      },
      // 是否入驻
      statusChange(val){
        if(val == 2){
          this.isshow = true
          this.status = 2
		  this.refundReason = '同意'
        }else{
          this.isshow = false
          this.status = 3
		  this.refundReason = ''
        }
      },
      // 处理弹框
      dispose(){
        this.isshow = true
		this.refundReason = '同意'
        this.dialogFormVisible = true
      },
      disposeNoticeTo(){
		if (this.refundReason == '') {
		      this.$notify({
		          title: '提示',
		          duration: 1800,
		          message: '请输入拒绝理由',
		          type: 'warning'
		      });
		      return
		}
        let id = this.$route.query.id
         this.$http({
           url: this.$http.adornUrl2('/self/merchantApply/deal'),
           method: 'post',
           params: this.$http.adornParams({
             'id':id,
             'status':this.status,
             'refundReason': this.refundReason,
           })
         }).then(({data}) => {
           if(data.status == 0){
               this.$message({
               message: '提交成功',
               type: 'success',
               duration: 1500,
             })
             this.dialogFormVisible = false
             this.$router.replace({ name: 'merchantEnter'})
           }
         })
      },
      // 获取数据列表
      dataSelect () {
        let id = this.$route.query.id
        this.$http({
          url: this.$http.adornUrl2('/self/merchantApply/find'),
          method: 'get',
          params: this.$http.adornParams({
            'id':id,
          })
        }).then(({data}) => {
          this.tableDataLoading = false
          let returnData = data.data
          this.tableData = returnData
        })
      }
    },
    activated () {
      this.dataSelect()
    }
  }
</script>

<style scoped="scoped">
  .merch_content{padding-left:5%;}
  .merch_item{margin-bottom: 30px;}
  .merch_item .merch_title{font-size: 18px;border-left:4px solid #3E8EF7;padding-left: 5px;}
  .merch_item .merch_main {padding-left:10px;margin-top: 10px;}
  .merch_item .merch_main .merch_main_item{font-size: 15px;padding: 15px 0;}
  .merch_item .merch_main .merch_main_item .merch_item_left{display: inline-block;width: 150px;text-align: left;}
</style>
