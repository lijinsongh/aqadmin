<template>
  <div>
	<el-tabs v-model="activeName" @tab-click="handleClick">
		<el-tab-pane label="保证金信息" name="first">
			<div>
				<el-select v-model="way" style="width:150px;margin-left: 10px;" @change="orderfenxi">
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
					@change="animeOrder"
				   >
				 </el-date-picker> 
				<el-row>
					<el-col :span="6" class="cards">
					  <div class="box">
						<div class="box_num">
						  <div class="box_color">累计收入</div>
						  <div class="text_color"><span>{{statiData.sum0 | numFilter}}</span>元</div>
						</div>
					  </div>
					</el-col>
					<el-col :span="6" class="cards">
					  <div class="box">
						<div class="box_num">
						  <div class="box_color">当前收入</div>
						  <div class="text_color"><span>{{statiData.sum1 | numFilter}}</span>元</div>
						</div>
					  </div>
					</el-col>
					<el-col :span="6" class="cards">
					  <div class="box">
						<div class="box_num">
						  <div class="box_color">店铺总数</div>
						  <div class="text_color"><span>{{statiData.count1}}</span>个</div>
						</div>
					  </div>
					</el-col>
					<el-col :span="6" class="cards">
					  <div class="box">
						<div class="box_num">
						  <div class="box_color">已缴纳保证金店铺</div>
						  <div class="text_color"><span>{{statiData.count2}}</span>个</div>
						</div>
					  </div>
					</el-col>
					<el-col :span="6" class="cards">
					  <div class="box">
						<div class="box_num">
						  <div class="box_color">已缴纳永久保证金店铺</div>
						  <div class="text_color"><span>{{statiData.count3}}</span>个</div>
						</div>
					  </div>
					</el-col>
					<el-col :span="6" class="cards">
					  <div class="box">
						<div class="box_num">
						  <div class="box_color">已缴纳年费保证金店铺</div>
						  <div class="text_color"><span>{{statiData.count4}}</span>个</div>
						</div>
					  </div>
					</el-col>
				</el-row>
			</div>
			<div style="margin:2% 0;display: inline-block;">
				<span>店铺名称：</span>
				<el-input style="width: 150px;" @keydown.enter.native="select" clearable placeholder="请输入店铺名称" v-model="storeName"></el-input>&nbsp;&nbsp;&nbsp;&nbsp;
			</div>
			<div style="display: inline-block;">
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="select">查询</el-button>
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="cleans">重置</el-button>
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="refresh">刷新</el-button>
			</div>
			<el-table
			v-loading="tableDataLoading"
			  :data="tableData.content">
			  <el-table-column
				 prop="storeName"
				 label="店铺名称"
				 width="200">
			   </el-table-column>
				  <el-table-column
				   prop="marginMoney"
				   label="保证金金额">
				   <template slot-scope="scope">
						<span style="color: #F56C6C;">{{scope.row.marginMoney}}</span>
				   </template>
				   </el-table-column>
				  <el-table-column
				   prop="marginType"
				   label="保证金类型">
				   <template slot-scope="scope">
						<span v-if="scope.row.marginType==1">永久</span>
						<span v-if="scope.row.marginType==2">年费</span>
				   </template>
				   </el-table-column>
				  <el-table-column
					   label="订单号"
					   width="180">
						 <template slot-scope="scope">
								<span style="color: #4f9dec;cursor: pointer;" @click="orderDetails(scope.row.orderId)">{{scope.row.orderNum?scope.row.orderNum:'暂无'}}</span>
						 </template>
					</el-table-column>
				   <el-table-column
					prop="createTime"
					label="保证金缴纳时间"
					width="160">
					</el-table-column>
				  <el-table-column
					prop="endTime"
					label="保证金到期时间"
					width="160">
				  </el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
			  <el-pagination
				@size-change="handleSizeChange"
				@current-change="handleCurrentChange"
				:page-sizes="[5, 10, 15, 20]"
				:page-size="size"
				:current-page="page"
				layout="total,sizes, prev, pager, next"
				:total="tableData.totalElements">
			  </el-pagination>
			</div>
		</el-tab-pane>
		<el-tab-pane label="保证金退款" name="refund">
			<div style="margin:2% 0;display: inline-block;">
			  <span>订单号：</span>
			  <el-input style="width: 150px;" @keydown.enter.native="select" clearable placeholder="请输入订单号" v-model="orderNum"></el-input>&nbsp;&nbsp;&nbsp;&nbsp;
			</div>
			<div style="display: inline-block;">
			  <el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="select">查询</el-button>
			  <el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="cleans">重置</el-button>
			  <el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="refresh2">刷新</el-button>
			</div>
			<el-table
			v-loading="tableDataLoading"
			  :data="orderData.content">
			 <el-table-column
			  fixed
			  prop="img"
			  label="商品图片">
			  <template slot-scope="scope">
			    <img :src="scope.row.img" alt="" width="60" height="60">
			  </template>
			  </el-table-column>
			  <el-table-column
			   prop="title"
			   label="商品标题"
			   width="200">
			   </el-table-column>
					 <el-table-column
					  prop="detailJson"
					  label="商品规格">
					  <template slot-scope="scope">
					   <span>{{scope.row.detailJson ? scope.row.detailJson : '无'}}</span>
					  </template>
					  </el-table-column>
			   <el-table-column
			    prop="number"
			    label="商品个数">
			    </el-table-column>
			    <el-table-column
			     prop="refund"
			     label="退款原因">
			     </el-table-column>
			  <el-table-column
			   prop="descrition"
			   label="订单备注">
			   <template slot-scope="scope">
			    <span>{{scope.row.descrition ? scope.row.descrition : '未填写'}}</span>
			   </template>
			  </el-table-column>
			  <el-table-column
			   label="订单类型"
					 width="120">
			   <template slot-scope="scope">
			    <span v-if="scope.row.orderType == 1">普通订单</span>
			    <span v-if="scope.row.orderType == 2">拼团订单</span>
			    <span v-if="scope.row.orderType == 3">秒杀订单</span>
			    <span v-if="scope.row.orderType == 4">积分订单</span>
					  <span v-if="scope.row.orderType == 5">保证金订单</span>
			   </template>
			  </el-table-column>
			   <el-table-column
			  prop="orderNum"
			  label="订单号"
			  width="200">
						<template slot-scope="scope">
			          <span style="color: #4f9dec;cursor: pointer;" @click="orderDetails(scope.row.id)">{{scope.row.orderNum}}</span>
			      </template>
			  </el-table-column>
			  <el-table-column
			  prop="consignee"
			  label="收货人"
			  width="120">
			  </el-table-column>
			  <el-table-column
			  prop="mobile"
			  label="手机号"
			  width="120">
			  </el-table-column>
			  <el-table-column
			  prop="detail"
			  label="详细地址"
			  width="120">
					<template slot-scope="scope">
						<span>{{scope.row.provinces}}{{scope.row.detail}}</span>
					</template>
			  </el-table-column>
			  <el-table-column
			   prop="price"
			   label="商品价格">
			   <template slot-scope="scope">
			    <span style="color: #f56c6c;">{{scope.row.price | numFilter}}</span>
			   </template>
			  </el-table-column>
			  <el-table-column
			  prop="payMoney"
			  label="支付金额"
			  width="120">
			  <template slot-scope="scope">
			    <span style="color: #f56c6c;cursor:pointer;">{{scope.row.payMoney | numFilter}}</span>
			  </template>
			  </el-table-column>
			  <el-table-column
			  prop="commissionPrice"
			  label="佣金"
			  width="120">
			  <template slot-scope="scope">
			    <span style="color: #f56c6c;">{{scope.row.commissionPrice | numFilter}}</span>
			  </template>
			  </el-table-column>
			  <el-table-column
			  prop="payWay"
			  label="支付方式"
			   width="120">
			  <template slot-scope="scope">
			    <span style="color: #4f9dec;" v-if="scope.row.payWay === 1 ">微信</span>
			    <span style="color: #4f9dec;" v-if="scope.row.payWay === 2 ">微信公众号</span>
			    <span style="color: #4f9dec;" v-if="scope.row.payWay === 3 ">微信小程序</span>
			    <span style="color: #4f9dec;" v-if="scope.row.payWay === 4 ">app支付宝</span>
			    <span style="color: #4f9dec;" v-if="scope.row.payWay === 5 ">H5支付宝</span>
			    <span style="color: #4f9dec;" v-if="scope.row.payWay === 6 ">零钱</span>
					  <span style="color: #4f9dec;" v-if="scope.row.payWay === 7 ">积分</span>
					  <span style="color: #4f9dec;" v-if="scope.row.payWay === 8 ">支付宝</span>
			    <span style="color: #4f9dec;" v-if="scope.row.payWay === null ">未支付</span>
			  </template>
			  </el-table-column>
			  <el-table-column
			    fixed="right"
			  prop="status"
			  label="订单状态">
			  <template slot-scope="scope">
			    <span style="color: #4f9dec;" v-if="scope.row.status === 1 ">待付款</span>
			    <span style="color: #4f9dec;" v-if="scope.row.status === 2 "> </span>
			    <span style="color: #4f9dec;" v-if="scope.row.status === 3 ">已发货 </span>
			    <span style="color: #4f9dec;" v-if="scope.row.status === 4 ">已收货</span>
			    <span style="color: #4f9dec;" v-if="scope.row.status === 5 ">已取消</span>
			    <span style="color: #4f9dec;" v-if="scope.row.status === 6 ">退款中</span>
			    <span style="color: #4f9dec;" v-if="scope.row.status === 7 ">已退款</span>
			  </template>
			  </el-table-column>
			  <el-table-column
			    prop="createAt"
			    label="创建时间"
			    width="160">
			  </el-table-column>
			  <el-table-column
			    prop="payTime"
			    label="付款时间"
			    width="160">
			    <template slot-scope="scope">
			      <span>{{scope.row.payTime ? scope.row.payTime : '未付款'}}</span>
			    </template>
			  </el-table-column>
			  <el-table-column
			    fixed="right"
			    label="操作"
			    width="100">
			    <template slot-scope="scope">
			      <el-button
			        size="mini"
			        type="primary"
			      :disabled="!isAuth('orderAdmin:refund')"
			        @click="refundClick(scope.row)">确定退款
			      </el-button>
			      <el-button
			        size="mini"
			        type="primary"
			    :disabled="!isAuth('orderAdmin:refuse')"
			        @click="refuseClick(scope.row)">拒绝退款
			      </el-button>
			      <el-button
			          size="mini"
			          type="primary"
			          @click="orderDetails(scope.row.id)">订单详情
			      </el-button>
						<el-button
							size="mini"
							type="primary"
							v-if="scope.row.orderType !==5"
							@click="contact(scope.row)">联系买家
						</el-button>
			    </template>
			  </el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
			  <el-pagination
			  @size-change="handleSizeChange"
			  @current-change="handleCurrentChange"
			  :page-sizes="[5, 10, 15, 20]"
			  :page-size="size"
			  :current-page="page"
			  layout="total,sizes, prev, pager, next"
			  :total="orderData.totalElements">
			  </el-pagination>
			</div>
			<!-- 拒绝退款 -->
			<el-dialog title="拒绝退款" :visible.sync="dialogFormVisible1" center>
			  <div style="margin-bottom: 10px;">
			    <span style="width: 200px;display: inline-block;text-align: right;position: relative;top: -65px;">拒绝退款理由：</span>
			    <el-input style="width:50%;" type="textarea" rows="4" v-model="refusedRefund" placeholder="请输入拒绝退款理由"></el-input>
			  </div>
			  <div slot="footer" class="dialog-footer">
			    <el-button @click="dialogFormVisible1 = false">取 消</el-button>
			    <el-button type="primary" @click="refuseNoticeTo()">确 定</el-button>
			  </div>
			</el-dialog>
		</el-tab-pane>
	</el-tabs>
  </div>
</template>

<script>
	export default {
	    data() {
	      return {
				size:5,
				page:1,
				storeName:'',
				orderType:5,
				status:6,
				orderNum:'',
				refusedRefund:'',
				info: {
					stockDate:this.getNowTime(),  //日期
				},
				way:3,
				ways: [
				{
				    value: 3,
				    label: '按天查询'
				},{
				    value: 2,
				    label: '按月查询'
				}, {
				    value: 1,
				    label: '按年查询'
				}],
				activeName:'first',
				statiData:{},
				formLabelWidth:'200px',
				tableDataLoading:false,
				dialogFormVisible1:false,
				tableData:[],
				orderData:[]
			}
		},
		filters: {
		  numFilter (value) {
		    let realVal = ''
		    if (!isNaN(value) && value!== '') {
		      // 截取当前数据到小数点后两位
		      realVal = parseFloat(value).toFixed(2)
		    } else {
		      realVal = '--'
		    }
		    return realVal
		  }
		},
		methods: {
			// 日期选择
			animeOrder(){
				this.statistics()
			},
			orderfenxi(){
				this.statistics()
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
			//搜索
			select(){
				this.page = 1
				this.size = 5
				this.orderSelect()
			},
			cleans(){
				this.orderNum = ''
				this.status = 6
				this.orderType = 5
				this.orderSelect()
			},
			refresh2(){
				this.page = 1
				this.size = 5
				this.orderSelect()
			},
			 // 退款
			refundClick(row){
				this.$confirm(`确定退款?`, '提示', {
				  confirmButtonText: '确定',
				  cancelButtonText: '取消',
				  type: 'warning'
				}).then(() => {
				  this.$http({
					url: this.$http.adornUrl2('/orders/refundMoney'),
					method: 'get',
					params: this.$http.adornParams({
						'ordersId':row.id
					})
				  }).then(({data}) => {
					  if(data.status == 0){
						this.$message({
							message: '退款成功',
							type: 'success',
							duration: 1500,
							onClose: () => {
								this.orderSelect()
							}
						})
					  }else{
						this.$message({
							message: data.msg,
							type: 'error',
							duration: 1500,
							onClose: () => {
							  this.orderSelect()
							}
						})
					}
				  })
				}).catch(() => {})
			},
			// 拒绝退款
			refuseClick(row){
				this.ordersId = row.id
				this.dialogFormVisible1 = true
			},
			refuseNoticeTo(){
				if(this.refusedRefund == ''){
					this.$message({
						message: '请输入拒绝退款理由',
						type: 'error',
						duration: 1500,
						onClose: () => {
						  this.refusedRefund = ''
						  this.dataSelect()
						}
					})
				}else{
					this.$http({
						url: this.$http.adornUrl2('/orders/refusedRefund'),
						method: 'get',
						params: this.$http.adornParams({
							'ordersId':this.ordersId,
							'refusedRefund':this.refusedRefund,
						})
					}).then(({data}) => {
						this.dialogFormVisible1 = false
						this.$message({
							message: '操作成功',
							type: 'success',
							duration: 1500,
							onClose: () => {
							  this.refusedRefund = ''
							  this.orderSelect()
							}
						})
					})
				}
			},
			// 刷新
			refresh(){
				this.dataSelect()
			},
			handleAvatarSuccess2(file) {
			     this.form1.img = file.data;
			 },
			handleSizeChange(val) {
				this.size = val;
				this.dataSelect()
			},
			handleCurrentChange(val) {
				this.page = val;
				this.dataSelect()
			},
			// 订单详情
			orderDetails (id) {
			    this.$router.push({path: '/orderDetails', query: {id: id}})
			},
			//搜索
			select(){
				this.page = 1
				this.size = 5
				this.dataSelect()
			},
			// 重置
			cleans(){
				this.page = 1
				this.size = 5
				this.storeName = ''
				this.dataSelect()
			},
			handleClick (tab, event) {
			        if (tab._props.label == '保证金信息') {
			          this.page = 1
			          this.size = 5
			          this.dataSelect()
					  this.statistics()
			        }
			        if (tab._props.label == '保证金退款') {
			          this.page = 1
			          this.size = 5
			          this.orderSelect()
			        }
			      },
			// 获取保证金列表数据
			dataSelect () {
			  let page = this.page - 1
			  this.tableDataLoading = true
			  this.$http({
				url: this.$http.adornUrl2('/self/margin/list'),
				method: 'get',
				params: this.$http.adornParams({
					'page':page,
					'size':this.size,
					'storeName':this.storeName
				})
			  }).then(({data}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					this.tableData = returnData
			  })
			},
			// 获取保证金统计
			statistics() {
			  this.$http({
				url: this.$http.adornUrl2('/self/margin/income'),
				method: 'get',
				params: this.$http.adornParams({
					'data':this.info.stockDate,
					'way':this.way
				})
			  }).then(({data}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					this.statiData = returnData
			  })
			},
			// 获取订单数据
			orderSelect () {
			  let page = this.page-1
			  this.tableDataLoading = true
			  this.$http({
				url: this.$http.adornUrl2('/orders/list'),
				method: 'get',
				params: this.$http.adornParams({
					'page':page,
					'size':this.size,
					'orderNum':this.orderNum,
					'status':this.status,
					'orderType':this.orderType
				})
			  }).then(({data}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					this.orderData = returnData
			  })
			},
		},
		mounted() {
			this.dataSelect()
			this.statistics()
		}
	  };
</script>

<style scoped="scoped">
.box{
		padding: 24px;
		border: 1px solid #eee;
		margin: 15px 10px;
	}
	.box_num{
		font-size: 14px;
		color: #66b1ff;
	}
	.box_num .box_color{
		color: #333;
		font-size: 14px;
		margin-bottom: 15px;
	}
	.box_num div span{
		font-size: 20px;
		margin-left: 5px;
	}
	.text_color{
		color: #4f9dec;
	}
	.text_color span{
		margin-right: 5px;
	}
	.el-button+.el-button{
		margin-left: 0;
		margin-top: 5px;
	}
</style>
