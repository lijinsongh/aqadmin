<template>
	<div>
		<el-tabs v-model="activeName" @tab-click="handleClick">
			<el-tab-pane label="待处理" name="first">
				<div style="margin:2% 0;display: inline-block;">
					<!-- <span>片区筛选：</span>
					<el-select v-model="campusId" style="width:150px;margin-left: 10px;" @change="animeDat(campusId)">
						<el-option v-for="item in homeData1" :key="item.campusId" :label="item.campusName"
							:value="item.campusId">
						</el-option>
					</el-select>&nbsp;&nbsp;&nbsp; -->
					<span>店铺类型：</span>
					<el-select v-model="storeType" placeholder="请选择店铺类型" style="width:150px;"
						@change="selectTrigger(platform)">
						<el-option v-for="item in storeTypes" :key="item.value" :label="item.label" :value="item.value">
						</el-option>
					</el-select>&nbsp;&nbsp;&nbsp;&nbsp;
					<el-input style="width: 150px;" @keydown.enter.native="select" clearable placeholder="请输入手机号"
						v-model="legalPhone"></el-input>&nbsp;&nbsp;&nbsp;&nbsp;
					<el-input style="width: 150px;" @keydown.enter.native="select" clearable placeholder="商家名称筛选"
						v-model="storeName"></el-input>
				</div>
				<div style="display: inline-block;">
					<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="select">查询
					</el-button>
					<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="cleans">重置
					</el-button>
				</div>
				<el-table v-loading="tableDataLoading" :data="tableData.content">
					<el-table-column prop="storeName" label="商家名称" width="120">
					</el-table-column>
					<el-table-column label="店铺类型" width="120">
						<template slot-scope="scope">
							<span v-if="scope.row.storeType == 1">个人</span>
							<span v-if="scope.row.storeType == 2">个体工商户</span>
							<span v-if="scope.row.storeType == 3">企业</span>
							<span v-if="scope.row.storeType == 4">其他组织</span>
						</template>
					</el-table-column>
					<el-table-column prop="legal" label="负责人">
					</el-table-column>
					<el-table-column prop="legalPhone" label="手机号">
					</el-table-column>
					<el-table-column prop="createTime" label="创建时间">
					</el-table-column>
					<el-table-column label="操作" width="120">
						<template slot-scope="scope">
							<el-button size="mini" type="primary" :disabled="!isAuth('merchantEnter:dispose')"
								@click="dispose(scope.row)">处理
							</el-button>
						</template>
					</el-table-column>
				</el-table>
				<div style="text-align: center;margin-top: 10px;">
					<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange"
						:page-sizes="[5, 10, 15, 20]" :page-size="size" :current-page="page"
						layout="total,sizes, prev, pager, next" :total="tableData.totalElements">
					</el-pagination>
				</div>
			</el-tab-pane>
			<el-tab-pane label="已通过" name="thirdly">
				<div style="margin:2% 0;display: inline-block;">
					<!-- <span>片区筛选：</span>
					<el-select v-model="campusId" style="width:150px;margin-left: 10px;" @change="animeDat(campusId)">
						<el-option v-for="item in homeData1" :key="item.campusId" :label="item.campusName"
							:value="item.campusId">
						</el-option>
					</el-select>&nbsp;&nbsp;&nbsp; -->
					<span>店铺类型：</span>
					<el-select v-model="storeType" placeholder="请选择店铺类型" style="width:150px;"
						@change="selectTrigger(platform)">
						<el-option v-for="item in storeTypes" :key="item.value" :label="item.label" :value="item.value">
						</el-option>
					</el-select>&nbsp;&nbsp;&nbsp;&nbsp;
					<el-input style="width: 150px;" @keydown.enter.native="select" clearable placeholder="请输入手机号"
						v-model="legalPhone"></el-input>&nbsp;&nbsp;&nbsp;&nbsp;
					<el-input style="width: 150px;" @keydown.enter.native="select" clearable placeholder="商家名称筛选"
						v-model="storeName"></el-input>
				</div>
				<div style="display: inline-block;">
					<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="select">查询
					</el-button>
					<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="cleans">重置
					</el-button>
				</div>
				<el-table v-loading="tableDataLoading" :data="tableData.content">
					<el-table-column prop="storeName" label="商家名称" width="120">
					</el-table-column>
					<el-table-column label="店铺类型" width="120">
						<template slot-scope="scope">
							<span v-if="scope.row.storeType == 1">个人</span>
							<span v-if="scope.row.storeType == 2">个体工商户</span>
							<span v-if="scope.row.storeType == 3">企业</span>
							<span v-if="scope.row.storeType == 4">其他组织</span>
						</template>
					</el-table-column>
					<el-table-column prop="legal" label="负责人">
					</el-table-column>
					<el-table-column prop="legalPhone" label="手机号">
					</el-table-column>
					<el-table-column prop="createTime" label="创建时间">
					</el-table-column>
					<el-table-column label="操作" width="120">
						<template slot-scope="scope">
							<el-button size="mini" type="primary" @click="dispose(scope.row)">详情
							</el-button>
						</template>
					</el-table-column>
				</el-table>
				<div style="text-align: center;margin-top: 10px;">
					<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange"
						:page-sizes="[5, 10, 15, 20]" :page-size="size" :current-page="page"
						layout="total,sizes, prev, pager, next" :total="tableData.totalElements">
					</el-pagination>
				</div>
			</el-tab-pane>
			<el-tab-pane label="已拒绝" name="second">
				<div style="margin:2% 0;display: inline-block;">
					<!-- <span>片区筛选：</span>
					<el-select v-model="campusId" style="width:150px;margin-left: 10px;" @change="animeDat(campusId)">
						<el-option v-for="item in homeData1" :key="item.campusId" :label="item.campusName"
							:value="item.campusId">
						</el-option>
					</el-select>&nbsp;&nbsp;&nbsp; -->
					<span>店铺类型：</span>
					<el-select v-model="storeType" placeholder="请选择店铺类型" style="width:150px;"
						@change="selectTrigger(platform)">
						<el-option v-for="item in storeTypes" :key="item.value" :label="item.label" :value="item.value">
						</el-option>
					</el-select>&nbsp;&nbsp;&nbsp;&nbsp;
					<el-input style="width: 150px;" @keydown.enter.native="select" clearable placeholder="请输入手机号"
						v-model="legalPhone"></el-input>&nbsp;&nbsp;&nbsp;&nbsp;
					<el-input style="width: 150px;" @keydown.enter.native="select" clearable placeholder="商家名称筛选"
						v-model="storeName"></el-input>
				</div>
				<div style="display: inline-block;">
					<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="select">查询
					</el-button>
					<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="cleans">重置
					</el-button>
				</div>
				<el-table v-loading="tableDataLoading" :data="tableData.content">
					<el-table-column prop="storeName" label="商家名称" width="120">
					</el-table-column>
					<el-table-column label="店铺类型" width="120">
						<template slot-scope="scope">
							<span v-if="scope.row.storeType == 1">个人</span>
							<span v-if="scope.row.storeType == 2">个体工商户</span>
							<span v-if="scope.row.storeType == 3">企业</span>
							<span v-if="scope.row.storeType == 4">其他组织</span>
						</template>
					</el-table-column>
					<el-table-column prop="legal" label="负责人">
					</el-table-column>
					<el-table-column prop="legalPhone" label="手机号">
					</el-table-column>
					<el-table-column prop="createTime" label="创建时间">
					</el-table-column>
					<el-table-column label="操作" width="120">
						<template slot-scope="scope">
							<el-button size="mini" type="primary" @click="dispose(scope.row)">详情
							</el-button>
						</template>
					</el-table-column>
				</el-table>
				<div style="text-align: center;margin-top: 10px;">
					<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange"
						:page-sizes="[5, 10, 15, 20]" :page-size="size" :current-page="page"
						layout="total,sizes, prev, pager, next" :total="tableData.totalElements">
					</el-pagination>
				</div>
			</el-tab-pane>
		</el-tabs>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				size: 5,
				page: 1,
				storeName: '',
				legalPhone: '',
				activeName: 'first',
				storeType: 0,
				storeTypes: [{
						value: 0,
						label: '全部'
					},
					{
						value: 1,
						label: '个人'
					}, {
						value: 2,
						label: '个体工商户'
					}, {
						value: 3,
						label: '企业'
					}, {
						value: 4,
						label: '其他组织'
					},
				],
				status: 1,
				info: {
					stockDate: this.getNowTime(), //日期
				},
				id: '',
				createTimeStart: '',
				createTimeEnd: '',
				refundReason: '',
				auditTime: '',
				auditYears: '',
				isshow: true,
				tableDataLoading: true,
				dialogFormVisible: false,
				tableData: [],
				campusId:'',
				homeData1:{},
			}
		},
		methods: {
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
			handleSizeChange(val) {
				this.size = val
				this.dataSelect()
			},
			handleCurrentChange(val) {
				this.page = val
				this.dataSelect()
			},
			handleClick(tab, event) {
				if (tab._props.label == '待处理') {
					this.status = 1
					this.size = 5
					this.page = 1
					this.dataSelect()
				}
				if (tab._props.label == '已通过') {
					this.size = 5
					this.page = 1
					this.status = 2
					this.dataSelect()
				}
				if (tab._props.label == '已拒绝') {
					this.size = 5
					this.page = 1
					this.status = 3
					this.dataSelect()
				}
			},
			// select自动选择
			selectTrigger() {
				this.dataSelect()
			},
			// 查询
			select() {
				this.dataSelect()
			},
			// 重置
			cleans() {
				this.storeName = ''
				this.legalPhone = ''
				this.storeType = 0
				// this.status = 1
				// this.activeName = 'first'
				this.createTimeStart = ''
				this.createTimeEnd = ''
				// this.campusId = ''
				this.dataSelect()
			},
			// 是否入驻
			statusChange(val) {
				if (val == 2) {
					this.isshow = true
					this.status = 2
				} else {
					this.isshow = false
					this.status = 3
				}
			},
			// 处理跳转
			dispose(row) {
				this.$router.push({
					path: '/merchDetails',
					query: {
						id: row.id
					}
				})
			},
			// 获取数据列表
			dataSelect() {
				let page = this.page - 1
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2('/self/merchantApply/list'),
					method: 'get',
					params: this.$http.adornParams({
						'page': page,
						'size': this.size,
						'storeName': this.storeName,
						'legalPhone': this.legalPhone,
						'storeType': this.storeType,
						'status': this.status,
						'createTimeStart': this.createTimeStart,
						'createTimeEnd': this.createTimeEnd
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let returnData = data.data
					this.tableData = returnData
				})
			},
			// 获取片区数据列表
			homeSelect() {
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2('/helpCampus/selectCampusList'),
					method: 'get',
					params: this.$http.adornParams({
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					this.homeData1 = returnData
				})
			},
			// select选择事件
			animeDat(state) {
				this.page = 1
				this.dataSelect()
			},
		},
		activated() {
			this.dataSelect()
			// this.homeSelect() 
		}
	}
</script>

<style scoped="scoped">
	.el-button+.el-button {
		margin-left: 0 !important;
		margin-top: 5px !important;
	}
</style>
