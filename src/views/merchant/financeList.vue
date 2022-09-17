<template>
  <el-tabs v-model="activeName" @tab-click="handleClick">
    <el-tab-pane label="提现管理" name="first">
      <div style="margin:2% 0;display: inline-block;">
        <div style="position: relative;display: inline-block;">
          <el-input style="width: 200px;" @keydown.enter.native="zfbselect" placeholder="请输入支付宝号查询"
                    v-model="zhifubao"></el-input>&nbsp;&nbsp;
          <span @click="zfbselect" style="position: absolute;right: 18px;top:8px;"><icon-svg name="shousuo"
                                                                                             class="site-sidebar__menu-icon"></icon-svg></span>
        </div>
        <div style="position: relative;display: inline-block;">
          <el-input style="width: 200px;" @keydown.enter.native="qdselect" placeholder="请输入会员编号查询"
                    v-model="relationId"></el-input>&nbsp;&nbsp;
          <span @click="qdselect" style="position: absolute;right: 18px;top:8px;"><icon-svg name="shousuo"
                                                                                            class="site-sidebar__menu-icon"></icon-svg></span>
        </div>
      </div>
      <div style="display: inline-block;">
        <el-button style='margin:0 0 20px 20px;'  size="mini" type="primary" icon="document" @click="transferClcik()"
                   :disabled="checkBoxData.length <= 0 || !isAuth('financeList:accounts')">批量转账
        </el-button>
        <el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="cleans">重置</el-button>
        <el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="refresh">刷新</el-button>
      </div>
      <el-table
        @selection-change="changeFun"
        v-loading="tableDataLoading"
        :data="tableData">
        <el-table-column type="selection">
        </el-table-column>
        <el-table-column
          fixed
          prop="id"
          label="编号"
          width="80">
        </el-table-column>
        <el-table-column
          fixed
          prop="relationId"
          label="会员编号"
          width="120">
          <template slot-scope="scope">
            <span style="color: #4f9dec;cursor: pointer;" @click="updates(scope.row)">{{scope.row.relationId}}</span>
          </template>
        </el-table-column>
        <el-table-column
          prop="zhifubao"
          label="支付宝"
          width="150">
        </el-table-column>
        <el-table-column
          prop="zhifubaoName"
          label="姓名"
          width="100">
        </el-table-column>
        <el-table-column
          prop="money"
          label="提现金额"
          width="100">
        </el-table-column>
		<el-table-column
		  prop="state"
		  label="状态"
		  width="100">
		  <template slot-scope="scope">
		    <span style="color: #4f9dec;cursor: pointer;" v-if="scope.row.state == 0">待转账</span>
			<span v-if="scope.row.state == 1">已转账</span>
			<span v-if="scope.row.state == -1">已退款</span>
		  </template>
		</el-table-column>
		<el-table-column
		  prop="refund"
		  label="退款原因"
		  width="220">
		</el-table-column>
        <el-table-column
          prop="createAt"
          label="申请时间"
          width="170">
        </el-table-column>
        <el-table-column
          prop="outAt"
          label="转账/退款时间"
		  width="180">
        </el-table-column>
        <el-table-column
          prop="orderNumber"
          label="转账订单号"
		  width="150">
        </el-table-column>
        <el-table-column
		  fixed="right"
          label="操作"
		  width="150">
          <template slot-scope="scope">
            <el-button
              size="mini"
              type="primary"
			  :disabled="!isAuth('financeList:accounts')"
              @click="batch(scope.row)">转账
            </el-button>
			<el-button
			  size="mini"
			  type="primary"
			 :disabled="!isAuth('financeList:refund')"
			  @click="refund(scope.row)">退款
			</el-button>
          </template>
        </el-table-column>
      </el-table>
      <div style="text-align: center;margin-top: 10px;">
        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :page-sizes="[10, 20, 30, 50, 100]"
          :page-size="size"
          :current-page="page"
          layout="total,sizes, prev, pager, next"
          :total="totalnum">
        </el-pagination>
      </div>
	  <!-- 退款弹框 -->
	  <el-dialog title="退款" :visible.sync="dialogFormVisible" center>
	  	<el-form :model="form">
	  	  <el-form-item label="退款原因：" :label-width="formLabelWidth" >
	  	    <el-input v-model="form.content"  type="textarea" rows="4" placeholder="请输入退款原因" style="width:65%;"></el-input>
	  	  </el-form-item>
	  	</el-form>
	    <div slot="footer" class="dialog-footer">
	      <el-button @click="dialogFormVisible = false">取 消</el-button>
	      <el-button type="primary" @click="refundNoticeTo()">确 定</el-button>
	    </div>
	  </el-dialog>
    </el-tab-pane>
    <el-tab-pane label="提现统计" name="third">
      <div>
        <el-select v-model="way" style="width:150px;margin-left: 10px;" @change="animeDat">
          <el-option v-for="item in ways" :key="item.value" :label="item.label" :value="item.value">
          </el-option>
        </el-select>&nbsp;&nbsp;&nbsp;
        <el-date-picker
          style="width: 160px;margin-left: 10px;"
          v-model="info.stockDate"
          align="right"
          type="datetime"
          format="yyyy-MM-dd"
          value-format="yyyy-MM-dd"
          placeholder="选择开始时间"
          @change="timeDate"
        >
        </el-date-picker> &nbsp;&nbsp;&nbsp;
      </div>
      <el-row>
        <el-col :span="6" class="cards">
          <div class="box">
            <div class="box_num">
              <div class="box_color">累计提现金额</div>
              <div class="text_color" v-if="!isNaN(sumMoneyByTime)">
                <span>{{sumMoneyByTime ? sumMoneyByTime : 0 }}</span>元
              </div>
              <div class="text_color" v-if="isNaN(sumMoneyByTime)"><span>0</span>元</div>
            </div>
          </div>
        </el-col>
        <el-col :span="6" class="cards">
          <div class="box">
            <div class="box_num">
              <div class="box_color">提现笔数</div>
              <div class="text_color">
                <span>{{withdrawData.countNumberByTime ? withdrawData.countNumberByTime : 0}}</span>笔
              </div>
            </div>
          </div>
        </el-col>
        <el-col :span="6" class="cards">
          <div class="box">
            <div class="box_num">
              <div class="box_color">待转账人数</div>
              <div class="text_color">
                <span>{{withdrawData.notOutMoneyByTime ? withdrawData.notOutMoneyByTime :0}}</span>人
              </div>
            </div>
          </div>
        </el-col>
      </el-row>
    </el-tab-pane>
	<el-tab-pane label="充值记录" name="fourth">
		<div style="display: inline-block;">
			<span>开始时间：</span>
			<el-date-picker
			  style="width: 160px;margin-left: 10px;"
			  v-model="startTime"
			  align="right"
			  type="datetime"
			  format="yyyy-MM-dd"
			  value-format="yyyy-MM-dd"
			  placeholder="选择开始时间"
			>
			</el-date-picker>&nbsp;&nbsp;&nbsp;
			<span>截止时间：</span>
			<el-date-picker
			  style="width: 160px;margin-left: 10px;"
			  v-model="endTime"
			  align="right"
			  type="datetime"
			  format="yyyy-MM-dd"
			  value-format="yyyy-MM-dd"
			  placeholder="选择截止时间"
			>
			</el-date-picker>
			<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="timeDate2">查询</el-button>
      <el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="refresh1">刷新</el-button>
    </div>
		<el-table
		  v-loading="tableDataLoading"
		  :data="rechargeData">
		  <el-table-column
		    fixed
		    prop="id"
		    label="编号"
		    width="80">
		  </el-table-column>
		  <el-table-column
		    prop="nickName"
		    label="用户名称">
			<template slot-scope="scope">
			  <span style="color: #4f9dec;cursor: pointer;"  @click="updates(scope.row)">{{scope.row.nickName ? scope.row.nickName : 'null'}}</span>
			</template>
		  </el-table-column>
		  <el-table-column
		    prop="orderId"
		    label="充值订单号"
		    width="250">
		  </el-table-column>
		  <el-table-column
		    prop="money"
		    label="充值金额"
		    width="100">
		  </el-table-column>
		  <el-table-column
		    prop="userId"
		    label="会员编号"
		    width="100">
		  </el-table-column>
		  <el-table-column
		    label="分类"
		    width="120">
		    <template slot-scope="scope">
		      <span style="color: #4f9dec;cursor: pointer;" v-if="scope.row.classify == 1">微信</span>
		  	  <span style="color: #4f9dec;cursor: pointer;" v-if="scope.row.classify == 2">支付宝</span>
		    </template>
		  </el-table-column>
		  <el-table-column
		    label="状态"
		    width="120">
		    <template slot-scope="scope">
		        <span style="color: #4f9dec;" v-if="scope.row.state == 0">待支付</span>
		  		<span style="color: #4f9dec;" v-if="scope.row.state == 1">支付成功</span>
		  		<span style="color: #4f9dec;" v-if="scope.row.state == 2">支付失败</span>
		    </template>
		  </el-table-column>
		  <el-table-column
		    prop="createTime"
		    label="创建时间"
		    width="170">
		  </el-table-column>
		  <el-table-column
		    prop="payTime"
		    label="支付时间">
		  </el-table-column>
		</el-table>
		<div style="text-align: center;margin-top: 10px;">
		  <el-pagination
		    @size-change="handleSizeChange1"
		    @current-change="handleCurrentChange1"
		    :page-sizes="[10, 20, 30, 50, 100]"
		    :page-size="size"
		    :current-page="page"
		    layout="total,sizes, prev, pager, next"
		    :total="totalnum2">
		  </el-pagination>
		</div>
	</el-tab-pane>
  </el-tabs>
</template>

<script>
  export default {
    data () {
      return {
        page: 1,
        size: 10,
        balance: -1,
        zhifubao: '',
        tkstatus: -1,
        platform: 'all',
		phone:'',
		content:'',
		startTime:'',
		endTime:'',
        cashId: '',
        type: -1,
        totalnum: 0,
		totalnum2:0,
        sqxTotal: '',
        userTotal: '',
        taunTotal: '',
        relationId: '',
        sumMoneyByTime: '',
        activeName: 'first',
        formLabelWidth:'200px',
        dialogFormVisible:false,
        tableDataLoading: false,
        tableData: [],
        rechargeData:[],
        withdrawData: {},
        checkBoxData: [],  //多选框选择的值
        way: 3,
        form:{
          cashId:'',
          content:'',
        },
        item:'',
        info: {
          stockDate: this.getNowTime(),  //日期
        },
        info2: {
          stockDate2: this.getNowTime2(),  //日期
        },
        platforms: [
          {
            value: 'all',
            label: '全部'
          }, {
            value: 'tb',
            label: '淘宝'
          }, {
            value: 'jd',
            label: '京东'
          }, {
            value: 'pdd',
            label: '拼多多'
          }],
        ways: [
          {
            value: 3,
            label: '按天查询'
          }, {
            value: 2,
            label: '按月查询'
          }, {
            value: 1,
            label: '按年查询'
          }],
      }
    },
    methods: {
		// 会员详情跳转
		updates (row) {
		  this.$router.push({path: '/userDetail', query: {userId: row.userId}})
		},
      // 批量转账
      transferClcik (id) {
        let _this = this
        var ids = id ? [id] : this.checkBoxData.map(item => {
          return item.id
        })
        for (var i in ids) {
          _this.$http({
            url: _this.$http.adornUrl2(`/cash/v1/alipay/${ids[i]}`),
            method: 'post',
            data: _this.$http.adornData({})
          }).then(({data}) => {
            if (data.status == 0) {
              _this.$notify({
                title: '提示',
                duration: 1800,
                message: data.data,
                type: 'warning'
              })
              return
            }
            if (data.status == 9999) {
              _this.$notify({
                title: '提示',
                duration: 1800,
                message: data.msg,
                type: 'error'
              })
              return
            }
          })
        }
      },
      // 多选
      changeFun (val) {
        this.checkBoxData = val
      },
      cleans () {
        this.zhifubao = ''
        this.relationId = ''
        this.dataSelect()
      },
      // 结算
      close () {
        this.$http({
          url: this.$http.adornUrl2('/cash/userToTalMoney'),
          method: 'post',
          data: this.$http.adornData({})
        }).then(({data}) => {
			if(data.status==0){
				this.$message({
					message: '操作成功',
					type: 'success',
					duration: 1500,
					onClose: () => {
					  this.dataSelect()
					}
				})
			}
        })
      },
      //转账
      batch (row) {
        let cashId = row.id
        if(row.state == 1){
        	this.$message({
        	  message: '已转账，请勿重复操作！',
        	  type: 'error',
        	  duration: 2500
        	})
        	return
        }
        this.$confirm(`确定转账?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$http({
        	url: this.$http.adornUrl2(`/cash/v1/alipay/${cashId}`),
        	method: 'post',
        	data: this.$http.adornData(cashId, false)
          }).then(({data}) => {
        	if (data.status == 0) {
        	  this.$message({
        		message: data.data,
        		type: 'success',
        		duration: 1500,
				onClose: () => {
				  this.dataSelect()
				}
        	  })
        	  return
        	}
        	if (data.status == 9999) {
        	  this.$message({
        		message: data.msg,
        		type: 'error',
        		duration: 2500
        	  })
        	  return
        	}
          })
        })

      },
	  // 退款
	  refund(row){
		  if(row.state == -1){
			this.$message({
			  message: '已退款，请勿重复操作！',
			  type: 'error',
			  duration: 2500
			})
		  }else{
			this.dialogFormVisible = true
			this.form.cashId = row.id
		  }
	  },
	  // 退款操作
	  refundNoticeTo(){
		if(this.form.content==''){
			this.$message({
			  message: '请输入退款原因！',
			  type: 'error',
			  duration: 1500
			})
			return
		}
		this.$http({
			url: this.$http.adornUrl2(`/cash/v1/refund/${this.form.cashId}/${this.form.content}`),
			method: 'post',
			data: this.$http.adornData({})
		}).then(({data}) => {
			if (data.status == 0) {
				this.dialogFormVisible = false
				this.$message({
				  message: '操作成功',
				  type: 'success',
				  duration: 1500,
				  onClose: () => {
					this.form.content=''
					this.dataSelect()
				  }
				})
				return
			}
			if (data.status == -100) {
				this.dialogFormVisible = false
				this.$message({
				  message: data.msg,
				  type: 'error',
				  duration: 2500,
				  onClose: () => {
					this.form.content=''
				  }
				})
				return
			}
		})
	  },
      timeDate () {
        this.withdrawSelect()
      },
		timeDate2 () {
			this.rechargeSelect()
		},
      // 日期选择
      animeDat (value) {
        let vanumber = value
        if (vanumber === 3) {
          this.withdrawSelect()
        }
        if (vanumber === 2) {
          this.item = this.info.stockDate
          this.info.stockDate = this.info.stockDate.substring(0, 7)
          this.withdrawSelect()
          this.info.stockDate = this.item
        }
        if (vanumber === 1) {
          this.item = this.info.stockDate
          this.info.stockDate = this.info.stockDate.substring(0, 4)
          this.withdrawSelect()
          this.info.stockDate = this.item
        }
      },
      //处理默认选中当前日期
      getNowTime () {
        var now = new Date()
        var year = now.getFullYear() //得到年份
        var month = now.getMonth() //得到月份
        var date = now.getDate() //得到日期
        month = month + 1
        month = month.toString().padStart(2, '0')
        date = date.toString().padStart(2, '0')
        var defaultDate = `${year}-${month}-${date}`
        return defaultDate
        this.$set(this.info, 'stockDate', defaultDate)
      },
	  //处理默认选中当前日期
	  getNowTime2 () {
	    var now = new Date()
	    var year = now.getFullYear() //得到年份
	    var month = now.getMonth()-now.getMonth() //得到月份
	    var date = now.getDate()-now.getDate()+1 //得到日期
	    month = month + 1
	    month = month.toString().padStart(2, '0')
	    date = date.toString().padStart(2, '0')
	    var defaultDate = `${year}-${month}-${date}`
	    return defaultDate
	    this.$set(this.info, 'stockDate', defaultDate)
	  },
      handleSizeChange (val) {
        this.size = val
        this.dataSelect()
      },
      handleCurrentChange (val) {
        this.page = val
        this.dataSelect()
      },
	  handleSizeChange1(val) {
	    this.size = val
	    this.rechargeSelect()
	  },
	  handleCurrentChange1 (val) {
	    this.page = val
	    this.rechargeSelect()
	  },
    // 刷新
    refresh(){
     this.dataSelect()
    },
    // 刷新
    refresh1(){
     this.rechargeSelect()
    },
      handleClick (tab, event) {
        if (tab._props.label == '提现管理') {
          this.page = 1
          this.size = 10
          this.dataSelect()
          this.withdrawSelect()
        }
        if (tab._props.label == '提现统计') {
          this.way = 3
          this.withdrawSelect()
        }
		if (tab._props.label == '充值记录') {
      this.page = 1
      this.size = 10
		  this.rechargeSelect()
		}
      },
      // 支付宝账号查询
      zfbselect () {
        this.tableDataLoading = true
        if (this.zhifubao == '') {
          this.$notify({
            title: '提示',
            duration: 1800,
            message: '请输入支付宝账号',
            type: 'error'
          })
          return
        }
        this.$http({
          url: this.$http.adornUrl2(`/cash/phone/${this.zhifubao}`),
          method: 'get',
          params: this.$http.adornParams({})
        }).then(({data}) => {
          this.tableDataLoading = false
          let returnData = data.data
          this.tableData = returnData
          this.totalnum = returnData.length
        })
      },
      // 渠道账号查询
      qdselect () {
        if (this.relationId == '') {
          this.$notify({
            title: '提示',
            duration: 1800,
            message: '请输入渠道账号',
            type: 'error'
          })
          return
        }
        this.tableDataLoading = true
        this.$http({
          url: this.$http.adornUrl2(`/cash/list/${this.relationId}`),
          method: 'get',
          params: this.$http.adornParams({})
        }).then(({data}) => {
          this.tableDataLoading = false
          let returnData = data.data
          this.tableData = returnData
          this.totalnum = returnData.length
        })
      },
      // 获取数据列表
      dataSelect () {
        this.tableDataLoading = true
        this.$http({
          url: this.$http.adornUrl2(`/cash/page/${this.page - 1}/${this.size}`),
          method: 'get',
          params: this.$http.adornParams({})
        }).then(({data}) => {
          this.tableDataLoading = false
          let returnData = data.data
          this.tableData = returnData.content
          this.totalnum = returnData.totalElements
        })
      },
      // 提现统计
      withdrawSelect () {
        this.$http({
          url: this.$http.adornUrl2('/statistics/getSumMoneyByTime'),
          method: 'get',
          params: this.$http.adornParams({
            'time': this.info.stockDate,
          })
        }).then(({data}) => {
          let returnData = data.data
          this.withdrawData = returnData
          this.sumMoneyByTime = parseFloat(returnData.sumMoneyByTime).toFixed(2)
        })
      },
	  // 获取充值记录
	  rechargeSelect () {
		if(this.endTime == ''){
			this.endTime = this.info.stockDate
		}
		if(this.startTime == ''){
			this.startTime = this.info2.stockDate2
		}
	    this.tableDataLoading = true
	    this.$http({
	      url: this.$http.adornUrl2(`/cash/selectPayDetails?page=${this.page - 1}&size=${this.size}&startTime=${this.startTime}&endTime=${this.endTime}`),
	      method: 'post',
	      data: this.$http.adornData({})
	    }).then(({data}) => {
	      this.tableDataLoading = false
	      let returnData = data.data
	      this.rechargeData = returnData.content
	      this.totalnum2 = returnData.totalElements
	    })
	  }
    },
    mounted () {
      this.dataSelect()
    }
  }
</script>

<style scoped="scoped">
  .box {
	height: 110px;
    padding: 24px;
    border: 1px solid #eee;
    margin: 15px 10px;
  }

  .box_num {
    font-size: 14px;
    color: #66b1ff;
  }

  .box_num .box_color {
    color: #333;
    font-size: 14px;
    margin-bottom: 15px;
  }

  .box_num div span {
    font-size: 20px;
    margin-left: 5px;
  }

  .text_color {
    color: #4f9dec;
  }

  .text_color span {
    margin-right: 5px;
  }
</style>
