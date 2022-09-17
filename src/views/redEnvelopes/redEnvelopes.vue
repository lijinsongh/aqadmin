<template>
	<div>
		<el-tabs v-model="activeName" @tab-click="handleClick">
			<el-tab-pane label="红包配置" name="first">
				<el-table v-loading="tableDataLoading" :data="tableData">
					<el-table-column fixed prop="type" label="编号" align="center" width="80">
					</el-table-column>
					<el-table-column prop="min" label="类型">
					</el-table-column>
					<el-table-column prop="value" label="内容" width="500">
					</el-table-column>
					<el-table-column prop="createAt" label="创建时间">
					</el-table-column>
					<el-table-column label="操作" prop="id" width="120">
						<template slot-scope="scope">
							<el-button size="mini" type="primary" :disabled="!isAuth('redEnvelopes:update')"
								@click="amend(scope.$index, scope.row)">编辑
							</el-button>
						</template>
					</el-table-column>
				</el-table>

			</el-tab-pane>
			<el-tab-pane label="收益排行" name="second">
				<div style = "margin:2% 0;display: inline-block;">
				  <span>昵称：</span>
				  <el-input style = "width: 150px;" @keydown.enter.native = "select" clearable placeholder = "输入昵称"
				            v-model = "nickName"></el-input>&nbsp;&nbsp;&nbsp;&nbsp;
				</div>
				<div style = "display: inline-block;">
				  <el-button style = "margin-left:15px;" size = "mini" type = "primary" icon = "document" @click = "select">查询</el-button>
				  <el-button style = "margin-left:15px;" size = "mini" type = "primary" icon = "document" @click = "cleans">重置</el-button>
					
				</div>
				<el-table v-loading="tableDataLoading" :data="tableDataS.content">
					<el-table-column fixed prop="rankNum" label="排名" align="center" width="100"></el-table-column>
					<el-table-column fixed prop="nickName" label="昵称" width="120">
					  <template slot-scope="scope">
					    <span style="color: #f56c6c;cursor: pointer;" @click="updates(scope.row)">{{ scope.row.nickName ? scope.row.nickName : '未设置' }}</span>
					  </template>
					</el-table-column>
					<el-table-column prop = "imageUrl" label = "头像">
					<template slot-scope = "scope">
						<img :src = "scope.row.imageUrl" alt = "" width = "60" height = "60">
					</template>
					</el-table-column>
					<el-table-column prop="money" label="收益" width="500"></el-table-column>
					
				</el-table>
				<div style = "text-align: center;margin-top: 10px;">
				  <el-pagination @size-change = "handleSizeChangeS" @current-change = "handleCurrentChangeS" :page-sizes = "[10,20,30,40,50]"
				                 :page-size = "size" :current-page = "page" layout = "total,sizes, prev, pager, next,jumper"
				                 :total = "tableDataS.totalElements">
				  </el-pagination>
				</div>
			</el-tab-pane>
			<el-tab-pane label="邀请排行" name="third">
				<div style = "margin:2% 0;display: inline-block;">
				  <span>昵称：</span>
				  <el-input style = "width: 150px;" @keydown.enter.native = "selectY" clearable placeholder = "输入昵称"
				            v-model = "nickName"></el-input>&nbsp;&nbsp;&nbsp;&nbsp;
				</div>
				<div style = "display: inline-block;">
				  <el-button style = "margin-left:15px;" size = "mini" type = "primary" icon = "document" @click = "selectY">查询</el-button>
				  <el-button style = "margin-left:15px;" size = "mini" type = "primary" icon = "document" @click = "cleansY">重置</el-button>
					
				</div>
				<el-table v-loading="tableDataLoading" :data="tableDataY.content">
					<el-table-column fixed prop="rankNum" label="排名" align="center" width="100"></el-table-column>
					<el-table-column fixed prop="nickName" label="昵称" width="120">
					  <template slot-scope="scope">
					    <span style="color: #f56c6c;cursor: pointer;" @click="updates(scope.row)">{{ scope.row.nickName ? scope.row.nickName : '未设置' }}</span>
					  </template>
					</el-table-column>
					<el-table-column prop = "imageUrl" label = "头像">
					<template slot-scope = "scope">
						<img :src = "scope.row.imageUrl" alt = "" width = "60" height = "60">
					</template>
					</el-table-column>
					<el-table-column prop="money" label="收益" width="500"></el-table-column>
				</el-table>
				<div style = "text-align: center;margin-top: 10px;">
				  <el-pagination @size-change = "handleSizeChangeY" @current-change = "handleCurrentChangeY" :page-sizes = "[10,20,30,40,50]"
				                 :page-size = "size" :current-page = "page" layout = "total,sizes, prev, pager, next,jumper"
				                 :total = "tableDataY.totalElements">
				  </el-pagination>
				</div>
			</el-tab-pane>

		</el-tabs>
		<!-- 修改弹框 -->
		<el-dialog title="修改" :visible.sync="dialogFormVisible" center>
			<el-form :model="form">
				<el-form-item label="配置类型：" :label-width="formLabelWidth">
					<el-input v-model="form.min" style="width:65%;" readonly></el-input>
				</el-form-item>
				<el-form-item label="内容：" :label-width="formLabelWidth">
					<el-input v-model="form.value" style="width:65%;"></el-input>
				</el-form-item>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click="dialogFormVisible = false">取 消</el-button>
				<el-button type="primary" @click="amendNoticeTo()">确 定</el-button>
			</div>
		</el-dialog>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				size: 10,
				page: 0,
				min: '',
				value: '',
				id: '',
				deployname: 'hongbao',
				activeName: 'first',
				dialogFormVisible: false,
				tableDataLoading: true,
				formLabelWidth: '200px',
				form: {
					id: '',
					min: '',
					value: '',
					type: '',
				},
				tableData: [],
				tableDataY: {},
				tableDataS: {},
				checkBoxData: [], //多选框选择的值
				nickName:'',
				phone:'',
			}
		},
		methods: {
			updates(row) {
				this.$router.push({
					path: '/userDetail',
					query: {
						userId: row.userId,
					}
				})
			},
			handleSizeChangeS(val) {
				this.size = val
				this.dataSelectS()
			},
			handleCurrentChangeS(val) {
				this.page = val - 1
				this.dataSelectS()
			},
			handleSizeChangeY (val) {
			  this.size = val
			  this.dataSelectY()
			},
			handleCurrentChangeY (val) {
			  this.page = val
			  this.dataSelectY()
			},
			handleClick(tab, event) {
				this.nickName = ''
				this.phone = ''
				if (tab._props.label == '红包配置') {
					this.deployname = 'hongbao'
					this.dataSelect()
				}
				if (tab._props.label == '收益排行') {
					this.dataSelectS()
				}
				if (tab._props.label == '邀请排行') {
					this.dataSelectY()
				}

			},
			// 重置
			cleans() {
				this.min = ''
				this.dataSelect()
			},

			// 修改弹框
			amend(index, rows) {
				this.dialogFormVisible = true
				this.form.id = rows.id
				this.form.type = rows.type
				this.form.min = rows.min
				this.form.value = rows.value
			},
			// 修改商品类别
			amendNoticeTo() {
				this.$http({
					url: this.$http.adornUrl2('/common/save'),
					method: 'post',
					data: this.$http.adornData({
						'id': this.form.id,
						'type': this.form.type,
						'value': this.form.value,
					})
				}).then(({
					data
				}) => {
					if (data && data.status === 0) {
						this.dialogFormVisible = false
						this.$message({
							message: '操作成功',
							type: 'success',
							duration: 1500,
							onClose: () => {
								this.dataSelect()
							}
						})
					} else {
						this.$message.error(data.msg)
					}
				})
			},
			// 获取数据列表
			dataSelect() {
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2(`/common/type/condition/${this.deployname}`),
					method: 'get',
					params: this.$http.adornParams({
						'page': this.page,
						'size': this.size
					})
				}).then(({
					data
				}) => {
					if (data && data.status === 0) {
						this.tableDataLoading = false
						let returnData = data.data
						this.tableData = returnData
					}
				})
			},
			// 获取邀请人排行数据列表
			dataSelectY() {
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2('/userMoney/selectInviteMoneyList'),
					method: 'get',
					params: this.$http.adornParams({
						'page': this.page,
						'size': this.size,
						'nickName':this.nickName,
						'phone':this.phone
					})
				}).then(({
					data
				}) => {
					if (data && data.status === 0) {
						this.tableDataLoading = false
						let returnData = data.data
						this.tableDataY = returnData
					}
				})
			},
			// 获取收益排行数据列表
			dataSelectS() {
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2('/userMoney/selectInviteMoneyList'),
					method: 'get',
					params: this.$http.adornParams({
						'page': this.page,
						'size': this.size,
						'nickName':this.nickName,
						'phone':this.phone
					})
				}).then(({
					data
				}) => {
					if (data && data.status === 0) {
						this.tableDataLoading = false
						let returnData = data.data
						this.tableDataS = returnData
					}
				})
			},
			select(){
				this.page = 0
				this.dataSelectS()
			},
			cleans(){
				this.page = 0
				this.nickName = ''
				this.dataSelectS()
			},
			selectY(){
				this.page = 0
				this.dataSelectY()
			},
			cleansY(){
				this.page = 0
				this.nickName = ''
				this.dataSelectY()
			}
		},
		mounted() {
			this.dataSelect()
		}
	}
</script>

<style scoped="scoped">
	.eit {
		height: 120px;
	}
</style>
