<template>
	<el-tabs v-model="activeName" @tab-click="handleClick">
		<div v-if="activeName!='ziying'&&activeName!='fenxiao'">
			<div style="margin:2% 0;display: inline-block;">
				<span>订单状态：</span>
				<el-select v-model="tkstatus" placeholder="请选择类型" style="width:150px;"
					@change="selectTrigger(tkstatus)">
					<el-option v-for="item in orderStatus" :key="item.value" :label="item.label" :value="item.value">
					</el-option>
				</el-select>&nbsp;&nbsp;&nbsp;&nbsp;
				<span>结算状态：</span>
				<el-select v-model="balance" placeholder="请选择类型" style="width:150px;" @change="selectTrigger(balance)">
					<el-option v-for="item in closeStatus" :key="item.value" :label="item.label" :value="item.value">
					</el-option>
				</el-select>&nbsp;&nbsp;&nbsp;&nbsp;
				<el-input style="width: 150px;" @keydown.enter.native="select" clearable placeholder="输入会员号查询"
					v-model="relationid"></el-input>&nbsp;&nbsp;&nbsp;&nbsp;
				<el-input style="width: 150px;" @keydown.enter.native="select" clearable placeholder="输入订单号查询"
					v-model="tradeId"></el-input>&nbsp;&nbsp;&nbsp;&nbsp;
				<el-input style="width: 150px;" @keydown.enter.native="select" clearable placeholder="输入团长号/直属查询"
					v-model="invitation"></el-input>
			</div>
			<div style="display: inline-block;">
				<el-button style="margin-left:15px;" size="mini" type="primary" icon="document" @click="select">查询
				</el-button>
				<el-button style="margin-left:15px;" size="mini" type="primary" icon="document" @click="cleans">重置
				</el-button>
				<el-button style="margin-left:15px;" :disabled="!isAuth('order:sync')" size="mini" type="primary"
					icon="document" @click="synOrder">同步订单</el-button>
				<el-button style="margin-left:15px;" size="mini" type="success" icon="document" @click="derive">导出Excel
				</el-button>
			</div>
		</div>

		<el-tab-pane label="全部订单" name="first">
			<el-table v-loading="tableDataLoading" :data="tableData.content">
				<el-table-column fixed prop="item_img" label="商品图片">
					<template slot-scope="scope">
						　　<img :src="scope.row.item_img" width="40" height="40" />
					</template>
				</el-table-column>
				<el-table-column prop="item_title" label="商品名称" width="200">
				</el-table-column>
				<el-table-column prop="relation_id" label="会员号" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="memberClick(scope.row.relation_id)">{{ scope.row.relation_id }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitation" label="直属" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitation,)">{{ scope.row.invitation }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitations" label="非直属" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitations)">{{ scope.row.invitations }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="invitation" label="团长号" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitation)">{{ scope.row.invitation }}</span>
					</template>
				</el-table-column>
				<el-table-column prop="trade_id" label="订单号" width="170">
				</el-table-column>
				<el-table-column prop="item_price" label="商品单价">
				</el-table-column>
				<el-table-column prop="alipay_total_price" label="实付金额">
				</el-table-column>
				<el-table-column prop="pub_share_pre_fee" label="商品总佣金" width="120">
				</el-table-column>
				<el-table-column prop="pub_share_pre_fee_user" label="会员佣金">
					<template slot-scope="scope">
						<div>{{ scope.row.isGif !== 0 ? '首单' : '' }}</div>
						<span
							style="color: #f56c6c;">{{ scope.row.tk_status !== 13 ? scope.row.pub_share_pre_fee_user : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitationMoney" label="直属佣金" width="120">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.invitationMoney ? scope.row.invitationMoney : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitationMoneys" label="非直属佣金" width="130">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.invitationMoneys ? scope.row.invitationMoneys : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="invitationMoney" label="团长佣金">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.tk_status !== 13 ? scope.row.invitationMoney : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="pub_share_pre_fee_user_s" label="平台佣金">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{
          scope.row.tk_status !== 13 ? (scope.row.pub_share_pre_fee - scope.row.pub_share_pre_fee_user - scope.row.invitationMoneys - scope.row.invitationMoney).toFixed(2) : '—'
        }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="pub_share_pre_fee_user_s" label="平台佣金">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{
                scope.row.tk_status !== 13 ? (scope.row.pub_share_pre_fee - scope.row.pub_share_pre_fee_user - scope.row.invitationMoney).toFixed(2) : '—'
              }}</span>
					</template>
				</el-table-column>
				<el-table-column prop="tk_paid_time" label="支付时间" width="160">
				</el-table-column>
				<el-table-column prop="fromInfo" label="来源渠道">
					<template slot-scope="scope">
						<span v-if="scope.row.fromInfo == 'pdd'">拼多多</span>
						<span v-if="scope.row.fromInfo == 'tb'">淘宝</span>
						<span v-if="scope.row.fromInfo == 'jd'">京东</span>
						<span v-if="scope.row.fromInfo == 'mt'">美团</span>
						<span v-if="scope.row.fromInfo == 'elm'">饿了么</span>
						<span v-if="scope.row.fromInfo == 'dm'">多麦</span>
						<span v-if="scope.row.fromInfo == 'wph'">唯品会</span>
						<span v-if="scope.row.fromInfo == 'dy'">抖音</span>
					</template>
				</el-table-column>
				<el-table-column fixed="right" prop="tk_status" label="订单状态">
					<template slot-scope="scope">
						<span v-if="scope.row.tk_status == 3">已结算</span>
						<span v-if="scope.row.tk_status == 12">已付款</span>
						<span v-if="scope.row.tk_status == 13">已失效</span>
					</template>
				</el-table-column>
				<el-table-column fixed="right" prop="balance" label="平台结算状态" width="120">
					<template slot-scope="scope">
						<span v-if="scope.row.balance == 1" style="color: #4f9dec;">已结算</span>
						<span v-if="scope.row.balance == 0" style="color: #4f9dec;">未结算</span>
					</template>
				</el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
				<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange"
					:page-sizes="[10,20,30,40,50]" :page-size="size" :current-page="page"
					layout="total,sizes, prev, pager, next,jumper" :total="tableData.totalElements">
				</el-pagination>
			</div>
		</el-tab-pane>
		<el-tab-pane label="淘宝订单" name="second">
			<el-table v-loading="tableDataLoading" :data="tableData.content">
				<el-table-column fixed prop="item_img" label="商品图片">
					<template slot-scope="scope">
						　　<img :src="scope.row.item_img" width="40" height="40" />
					</template>
				</el-table-column>
				<el-table-column prop="item_title" label="商品名称" width="200">
				</el-table-column>
				<el-table-column prop="relation_id" label="会员号" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="memberClick(scope.row.relation_id)">{{ scope.row.relation_id }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitation" label="直属" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitation)">{{ scope.row.invitation }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitations" label="非直属" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitations)">{{ scope.row.invitations }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="invitation" label="团长号" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitation)">{{ scope.row.invitation }}</span>
					</template>
				</el-table-column>
				<el-table-column prop="trade_id" label="订单号" width="170">
				</el-table-column>
				<el-table-column prop="item_price" label="商品单价">
				</el-table-column>
				<el-table-column prop="alipay_total_price" label="实付金额">
				</el-table-column>
				<el-table-column prop="pub_share_pre_fee" label="商品总佣金" width="120">
				</el-table-column>
				<el-table-column prop="pub_share_pre_fee_user" label="会员佣金">
					<template slot-scope="scope">
						<div>{{ scope.row.isGif !== 0 ? '首单' : '' }}</div>
						<span
							style="color: #f56c6c;">{{ scope.row.tk_status !== 13 ? scope.row.pub_share_pre_fee_user : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitationMoney" label="直属佣金" width="120">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.invitationMoney ? scope.row.invitationMoney : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitationMoneys" label="非直属佣金" width="130">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.invitationMoneys ? scope.row.invitationMoneys : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="invitationMoney" label="团长佣金">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.tk_status !== 13 ? scope.row.invitationMoney : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="pub_share_pre_fee_user_s" label="平台佣金">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{
              scope.row.tk_status !== 13 ? (scope.row.pub_share_pre_fee - scope.row.pub_share_pre_fee_user - scope.row.invitationMoneys - scope.row.invitationMoney).toFixed(2) : '—'
            }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="pub_share_pre_fee_user_s" label="平台佣金">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{
                scope.row.tk_status !== 13 ? (scope.row.pub_share_pre_fee - scope.row.pub_share_pre_fee_user - scope.row.invitationMoney).toFixed(2) : '—'
              }}</span>
					</template>
				</el-table-column>
				<el-table-column prop="tk_paid_time" label="支付时间" width="160">
				</el-table-column>
				<el-table-column prop="fromInfo" label="来源渠道">
					<template slot-scope="scope">
						<span v-if="scope.row.fromInfo == 'pdd'">拼多多</span>
						<span v-if="scope.row.fromInfo == 'tb'">淘宝</span>
						<span v-if="scope.row.fromInfo == 'jd'">京东</span>
						<span v-if="scope.row.fromInfo == 'mt'">美团</span>
						<span v-if="scope.row.fromInfo == 'elm'">饿了么</span>
						<span v-if="scope.row.fromInfo == 'dm'">多麦</span>
						<span v-if="scope.row.fromInfo == 'wph'">唯品会</span>
						<span v-if="scope.row.fromInfo == 'dy'">抖音</span>
					</template>
				</el-table-column>
				<el-table-column fixed="right" prop="tk_status" label="订单状态">
					<template slot-scope="scope">
						<span v-if="scope.row.tk_status == 3">已结算</span>
						<span v-if="scope.row.tk_status == 12">已付款</span>
						<span v-if="scope.row.tk_status == 13">已失效</span>
					</template>
				</el-table-column>
				<el-table-column fixed="right" prop="balance" label="平台结算状态" width="120">
					<template slot-scope="scope">
						<span v-if="scope.row.balance == 1" style="color: #4f9dec;cursor: pointer;">已结算</span>
						<span v-if="scope.row.balance == 0" style="color: #4f9dec;cursor: pointer;">未结算</span>
					</template>
				</el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
				<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange"
					:page-sizes="[10,20,30,40,50]" :page-size="size" :current-page="page"
					layout="total,sizes, prev, pager, next,jumper" :total="tableData.totalElements">
				</el-pagination>
			</div>
		</el-tab-pane>
		<el-tab-pane label="拼多多订单" name="third">
			<el-table v-loading="tableDataLoading" :data="tableData.content">
				<el-table-column fixed prop="item_img" label="商品图片">
					<template slot-scope="scope">
						　　<img :src="scope.row.item_img" width="40" height="40" />
					</template>
				</el-table-column>
				<el-table-column prop="item_title" label="商品名称" width="200">
				</el-table-column>
				<el-table-column prop="relation_id" label="会员号" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="memberClick(scope.row.relation_id)">{{ scope.row.relation_id }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitation" label="直属" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitation)">{{ scope.row.invitation }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitations" label="非直属" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitations)">{{ scope.row.invitations }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="invitation" label="团长号" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitation)">{{ scope.row.invitation }}</span>
					</template>
				</el-table-column>
				<el-table-column prop="trade_id" label="订单号" width="170">
				</el-table-column>
				<el-table-column prop="item_price" label="商品单价">
				</el-table-column>
				<el-table-column prop="alipay_total_price" label="实付金额">
				</el-table-column>
				<el-table-column prop="pub_share_pre_fee" label="商品总佣金" width="120">
				</el-table-column>
				<el-table-column prop="pub_share_pre_fee_user" label="会员佣金">
					<template slot-scope="scope">
						<div>{{ scope.row.isGif !== 0 ? '首单' : '' }}</div>
						<span
							style="color: #f56c6c;">{{ scope.row.tk_status !== 13 ? scope.row.pub_share_pre_fee_user : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitationMoney" label="直属佣金" width="120">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.invitationMoney ? scope.row.invitationMoney : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitationMoneys" label="非直属佣金" width="130">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.invitationMoneys ? scope.row.invitationMoneys : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="invitationMoney" label="团长佣金">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.tk_status !== 13 ? scope.row.invitationMoney : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="pub_share_pre_fee_user_s" label="平台佣金">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{
              scope.row.tk_status !== 13 ? (scope.row.pub_share_pre_fee - scope.row.pub_share_pre_fee_user - scope.row.invitationMoneys - scope.row.invitationMoney).toFixed(2) : '—'
            }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="pub_share_pre_fee_user_s" label="平台佣金">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{
                scope.row.tk_status !== 13 ? (scope.row.pub_share_pre_fee - scope.row.pub_share_pre_fee_user - scope.row.invitationMoney).toFixed(2) : '—'
              }}</span>
					</template>
				</el-table-column>
				<el-table-column prop="tk_paid_time" label="支付时间" width="160">
				</el-table-column>
				<el-table-column prop="fromInfo" label="来源渠道">
					<template slot-scope="scope">
						<span v-if="scope.row.fromInfo == 'pdd'">拼多多</span>
						<span v-if="scope.row.fromInfo == 'tb'">淘宝</span>
						<span v-if="scope.row.fromInfo == 'jd'">京东</span>
						<span v-if="scope.row.fromInfo == 'mt'">美团</span>
						<span v-if="scope.row.fromInfo == 'elm'">饿了么</span>
						<span v-if="scope.row.fromInfo == 'dm'">多麦</span>
						<span v-if="scope.row.fromInfo == 'wph'">唯品会</span>
						<span v-if="scope.row.fromInfo == 'dy'">抖音</span>
					</template>
				</el-table-column>
				<el-table-column fixed="right" prop="tk_status" label="订单状态">
					<template slot-scope="scope">
						<span v-if="scope.row.tk_status == 3">已结算</span>
						<span v-if="scope.row.tk_status == 12">已付款</span>
						<span v-if="scope.row.tk_status == 13">已失效</span>
					</template>
				</el-table-column>
				<el-table-column fixed="right" prop="balance" label="平台结算状态" width="120">
					<template slot-scope="scope">
						<span v-if="scope.row.balance == 1" style="color: #4f9dec;">已结算</span>
						<span v-if="scope.row.balance == 0" style="color: #4f9dec;">未结算</span>
					</template>
				</el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
				<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange"
					:page-sizes="[10,20,30,40,50]" :page-size="size" :current-page="page"
					layout="total,sizes, prev, pager, next,jumper" :total="tableData.totalElements">
				</el-pagination>
			</div>
		</el-tab-pane>
		<el-tab-pane label="京东订单" name="fourth">
			<el-table v-loading="tableDataLoading" :data="tableData.content">
				<el-table-column fixed prop="item_img" label="商品图片">
					<template slot-scope="scope">
						　　<img :src="scope.row.item_img" width="40" height="40" />
					</template>
				</el-table-column>
				<el-table-column prop="item_title" label="商品名称" width="200">
				</el-table-column>
				<el-table-column prop="relation_id" label="会员号" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="memberClick(scope.row.relation_id)">{{ scope.row.relation_id }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitation" label="直属" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitation)">{{ scope.row.invitation }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitations" label="非直属" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitations)">{{ scope.row.invitations }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="invitation" label="团长号" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitation)">{{ scope.row.invitation }}</span>
					</template>
				</el-table-column>
				<el-table-column prop="trade_id" label="订单号" width="170">
				</el-table-column>
				<el-table-column prop="item_price" label="商品单价">
				</el-table-column>
				<el-table-column prop="alipay_total_price" label="实付金额">
				</el-table-column>
				<el-table-column prop="pub_share_pre_fee" label="商品总佣金" width="120">
				</el-table-column>
				<el-table-column prop="pub_share_pre_fee_user" label="会员佣金">
					<template slot-scope="scope">
						<div>{{ scope.row.isGif !== 0 ? '首单' : '' }}</div>
						<span
							style="color: #4f9dec;">{{ scope.row.tk_status !== 13 ? scope.row.pub_share_pre_fee_user : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitationMoney" label="直属佣金" width="120">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.invitationMoney ? scope.row.invitationMoney : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitationMoneys" label="非直属佣金" width="130">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.invitationMoneys ? scope.row.invitationMoneys : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="invitationMoney" label="团长佣金">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.tk_status !== 13 ? scope.row.invitationMoney : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="pub_share_pre_fee_user_s" label="平台佣金">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{
              scope.row.tk_status !== 13 ? (scope.row.pub_share_pre_fee - scope.row.pub_share_pre_fee_user - scope.row.invitationMoneys - scope.row.invitationMoney).toFixed(2) : '—'
            }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="pub_share_pre_fee_user_s" label="平台佣金">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{
                scope.row.tk_status !== 13 ? (scope.row.pub_share_pre_fee - scope.row.pub_share_pre_fee_user - scope.row.invitationMoney).toFixed(2) : '—'
              }}</span>
					</template>
				</el-table-column>
				<el-table-column prop="tk_paid_time" label="支付时间" width="160">
				</el-table-column>
				<el-table-column prop="fromInfo" label="来源渠道">
					<template slot-scope="scope">
						<span v-if="scope.row.fromInfo == 'pdd'">拼多多</span>
						<span v-if="scope.row.fromInfo == 'tb'">淘宝</span>
						<span v-if="scope.row.fromInfo == 'jd'">京东</span>
						<span v-if="scope.row.fromInfo == 'mt'">美团</span>
						<span v-if="scope.row.fromInfo == 'elm'">饿了么</span>
						<span v-if="scope.row.fromInfo == 'dm'">多麦</span>
						<span v-if="scope.row.fromInfo == 'wph'">唯品会</span>
						<span v-if="scope.row.fromInfo == 'dy'">抖音</span>
					</template>
				</el-table-column>
				<el-table-column fixed="right" prop="tk_status" label="订单状态">
					<template slot-scope="scope">
						<span v-if="scope.row.tk_status == 3">已结算</span>
						<span v-if="scope.row.tk_status == 12">已付款</span>
						<span v-if="scope.row.tk_status == 13">已失效</span>
					</template>
				</el-table-column>
				<el-table-column fixed="right" prop="balance" label="平台结算状态" width="120">
					<template slot-scope="scope">
						<span v-if="scope.row.balance == 1" style="color: #4f9dec;">已结算</span>
						<span v-if="scope.row.balance == 0" style="color: #4f9dec;">未结算</span>
					</template>
				</el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
				<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange"
					:page-sizes="[10,20,30,40,50]" :page-size="size" :current-page="page"
					layout="total,sizes, prev, pager, next,jumper" :total="tableData.totalElements">
				</el-pagination>
			</div>
		</el-tab-pane>
		<el-tab-pane label="美团订单" name="fifth">
			<el-table v-loading="tableDataLoading" :data="tableData.content">
				<el-table-column fixed prop="item_img" label="商品图片">
					<template slot-scope="scope">
						　　<img :src="scope.row.item_img" width="40" height="40" />
					</template>
				</el-table-column>
				<el-table-column prop="item_title" label="商品名称" width="200">
				</el-table-column>
				<el-table-column prop="relation_id" label="会员号" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="memberClick(scope.row.relation_id)">{{ scope.row.relation_id }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitation" label="直属" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitation)">{{ scope.row.invitation }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitations" label="非直属" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitations)">{{ scope.row.invitations }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="invitation" label="团长号" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitation)">{{ scope.row.invitation }}</span>
					</template>
				</el-table-column>
				<el-table-column prop="trade_id" label="订单号" width="170">
				</el-table-column>
				<el-table-column prop="item_price" label="商品单价">
				</el-table-column>
				<el-table-column prop="alipay_total_price" label="实付金额">
				</el-table-column>
				<el-table-column prop="pub_share_pre_fee" label="商品总佣金" width="120">
				</el-table-column>
				<el-table-column prop="pub_share_pre_fee_user" label="会员佣金">
					<template slot-scope="scope">
						<div>{{ scope.row.isGif !== 0 ? '首单' : '' }}</div>
						<span
							style="color: #4f9dec;">{{ scope.row.tk_status !== 13 ? scope.row.pub_share_pre_fee_user : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitationMoney" label="直属佣金" width="120">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.invitationMoney ? scope.row.invitationMoney : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitationMoneys" label="非直属佣金" width="130">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.invitationMoneys ? scope.row.invitationMoneys : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="invitationMoney" label="团长佣金">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.tk_status !== 13 ? scope.row.invitationMoney : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="pub_share_pre_fee_user_s" label="平台佣金">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{
          scope.row.tk_status !== 13 ? (scope.row.pub_share_pre_fee - scope.row.pub_share_pre_fee_user - scope.row.invitationMoneys - scope.row.invitationMoney).toFixed(2) : '—'
        }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="pub_share_pre_fee_user_s" label="平台佣金">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{
          scope.row.tk_status !== 13 ? (scope.row.pub_share_pre_fee - scope.row.pub_share_pre_fee_user - scope.row.invitationMoney).toFixed(2) : '—'
        }}</span>
					</template>
				</el-table-column>
				<el-table-column prop="tk_paid_time" label="支付时间" width="160">
				</el-table-column>
				<el-table-column prop="fromInfo" label="来源渠道">
					<template slot-scope="scope">
						<span v-if="scope.row.fromInfo == 'pdd'">拼多多</span>
						<span v-if="scope.row.fromInfo == 'tb'">淘宝</span>
						<span v-if="scope.row.fromInfo == 'jd'">京东</span>
						<span v-if="scope.row.fromInfo == 'mt'">美团</span>
						<span v-if="scope.row.fromInfo == 'elm'">饿了么</span>
						<span v-if="scope.row.fromInfo == 'dm'">多麦</span>
						<span v-if="scope.row.fromInfo == 'wph'">唯品会</span>
						<span v-if="scope.row.fromInfo == 'dy'">抖音</span>
					</template>
				</el-table-column>
				<el-table-column fixed="right" prop="tk_status" label="订单状态">
					<template slot-scope="scope">
						<span v-if="scope.row.tk_status == 3">已结算</span>
						<span v-if="scope.row.tk_status == 12">已付款</span>
						<span v-if="scope.row.tk_status == 13">已失效</span>
					</template>
				</el-table-column>
				<el-table-column fixed="right" prop="balance" label="平台结算状态" width="120">
					<template slot-scope="scope">
						<span v-if="scope.row.balance == 1" style="color: #4f9dec;">已结算</span>
						<span v-if="scope.row.balance == 0" style="color: #4f9dec;">未结算</span>
					</template>
				</el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
				<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange"
					:page-sizes="[10,20,30,40,50]" :page-size="size" :current-page="page"
					layout="total,sizes, prev, pager, next,jumper" :total="tableData.totalElements">
				</el-pagination>
			</div>
		</el-tab-pane>
		<el-tab-pane label="饿了么订单" name="sixth">
			<el-table v-loading="tableDataLoading" :data="tableData.content">
				<el-table-column fixed prop="item_img" label="商品图片">
					<template slot-scope="scope">
						　　<img :src="scope.row.item_img" width="40" height="40" />
					</template>
				</el-table-column>
				<el-table-column prop="item_title" label="商品名称" width="200">
				</el-table-column>
				<el-table-column prop="relation_id" label="会员号" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="memberClick(scope.row.relation_id)">{{ scope.row.relation_id }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitation" label="直属" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitation)">{{ scope.row.invitation }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitations" label="非直属" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitations)">{{ scope.row.invitations }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="invitation" label="团长号" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitation)">{{ scope.row.invitation }}</span>
					</template>
				</el-table-column>
				<el-table-column prop="trade_id" label="订单号" width="170">
				</el-table-column>
				<el-table-column prop="item_price" label="商品单价">
				</el-table-column>
				<el-table-column prop="alipay_total_price" label="实付金额">
				</el-table-column>
				<el-table-column prop="pub_share_pre_fee" label="商品总佣金" width="120">
				</el-table-column>
				<el-table-column prop="pub_share_pre_fee_user" label="会员佣金">
					<template slot-scope="scope">
						<div>{{ scope.row.isGif !== 0 ? '首单' : '' }}</div>
						<span
							style="color: #4f9dec;">{{ scope.row.tk_status !== 13 ? scope.row.pub_share_pre_fee_user : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitationMoney" label="直属佣金" width="120">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.invitationMoney ? scope.row.invitationMoney : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitationMoneys" label="非直属佣金" width="130">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.invitationMoneys ? scope.row.invitationMoneys : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="invitationMoney" label="团长佣金">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.tk_status !== 13 ? scope.row.invitationMoney : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="pub_share_pre_fee_user_s" label="平台佣金">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{
          scope.row.tk_status !== 13 ? (scope.row.pub_share_pre_fee - scope.row.pub_share_pre_fee_user - scope.row.invitationMoneys - scope.row.invitationMoney).toFixed(2) : '—'
        }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="pub_share_pre_fee_user_s" label="平台佣金">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{
          scope.row.tk_status !== 13 ? (scope.row.pub_share_pre_fee - scope.row.pub_share_pre_fee_user - scope.row.invitationMoney).toFixed(2) : '—'
        }}</span>
					</template>
				</el-table-column>
				<el-table-column prop="tk_paid_time" label="支付时间" width="160">
				</el-table-column>
				<el-table-column prop="fromInfo" label="来源渠道">
					<template slot-scope="scope">
						<span v-if="scope.row.fromInfo == 'pdd'">拼多多</span>
						<span v-if="scope.row.fromInfo == 'tb'">淘宝</span>
						<span v-if="scope.row.fromInfo == 'jd'">京东</span>
						<span v-if="scope.row.fromInfo == 'mt'">美团</span>
						<span v-if="scope.row.fromInfo == 'elm'">饿了么</span>
						<span v-if="scope.row.fromInfo == 'dm'">多麦</span>
						<span v-if="scope.row.fromInfo == 'wph'">唯品会</span>
						<span v-if="scope.row.fromInfo == 'dy'">抖音</span>
					</template>
				</el-table-column>
				<el-table-column fixed="right" prop="tk_status" label="订单状态">
					<template slot-scope="scope">
						<span v-if="scope.row.tk_status == 3">已结算</span>
						<span v-if="scope.row.tk_status == 12">已付款</span>
						<span v-if="scope.row.tk_status == 13">已失效</span>
					</template>
				</el-table-column>
				<el-table-column fixed="right" prop="balance" label="平台结算状态" width="120">
					<template slot-scope="scope">
						<span v-if="scope.row.balance == 1" style="color: #4f9dec;">已结算</span>
						<span v-if="scope.row.balance == 0" style="color: #4f9dec;">未结算</span>
					</template>
				</el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
				<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange"
					:page-sizes="[10,20,30,40,50]" :page-size="size" :current-page="page"
					layout="total,sizes, prev, pager, next,jumper" :total="tableData.totalElements">
				</el-pagination>
			</div>
		</el-tab-pane>
		<el-tab-pane label="多麦订单" name="seventh">
			<el-table v-loading="tableDataLoading" :data="tableData.content">
				<el-table-column fixed prop="item_img" label="商品图片">
					<template slot-scope="scope">
						　　<img :src="scope.row.item_img" width="40" height="40" />
					</template>
				</el-table-column>
				<el-table-column prop="item_title" label="商品名称" width="200">
				</el-table-column>
				<el-table-column prop="relation_id" label="会员号" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="memberClick(scope.row.relation_id)">{{ scope.row.relation_id }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitation" label="直属" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitation)">{{ scope.row.invitation }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitations" label="非直属" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitations)">{{ scope.row.invitations }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="invitation" label="团长号" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitation)">{{ scope.row.invitation }}</span>
					</template>
				</el-table-column>
				<el-table-column prop="trade_id" label="订单号" width="170">
				</el-table-column>
				<el-table-column prop="item_price" label="商品单价">
				</el-table-column>
				<el-table-column prop="alipay_total_price" label="实付金额">
				</el-table-column>
				<el-table-column prop="pub_share_pre_fee" label="商品总佣金" width="120">
				</el-table-column>
				<el-table-column prop="pub_share_pre_fee_user" label="会员佣金">
					<template slot-scope="scope">
						<div>{{ scope.row.isGif !== 0 ? '首单' : '' }}</div>
						<span
							style="color: #4f9dec;">{{ scope.row.tk_status !== 13 ? scope.row.pub_share_pre_fee_user : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitationMoney" label="直属佣金" width="120">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.invitationMoney ? scope.row.invitationMoney : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitationMoneys" label="非直属佣金" width="130">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.invitationMoneys ? scope.row.invitationMoneys : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="invitationMoney" label="团长佣金">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.tk_status !== 13 ? scope.row.invitationMoney : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="pub_share_pre_fee_user_s" label="平台佣金">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{
          scope.row.tk_status !== 13 ? (scope.row.pub_share_pre_fee - scope.row.pub_share_pre_fee_user - scope.row.invitationMoneys - scope.row.invitationMoney).toFixed(2) : '—'
        }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="pub_share_pre_fee_user_s" label="平台佣金">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{
          scope.row.tk_status !== 13 ? (scope.row.pub_share_pre_fee - scope.row.pub_share_pre_fee_user - scope.row.invitationMoney).toFixed(2) : '—'
        }}</span>
					</template>
				</el-table-column>
				<el-table-column prop="tk_paid_time" label="支付时间" width="160">
				</el-table-column>
				<el-table-column prop="fromInfo" label="来源渠道">
					<template slot-scope="scope">
						<span v-if="scope.row.fromInfo == 'pdd'">拼多多</span>
						<span v-if="scope.row.fromInfo == 'tb'">淘宝</span>
						<span v-if="scope.row.fromInfo == 'jd'">京东</span>
						<span v-if="scope.row.fromInfo == 'mt'">美团</span>
						<span v-if="scope.row.fromInfo == 'elm'">饿了么</span>
						<span v-if="scope.row.fromInfo == 'dm'">多麦</span>
						<span v-if="scope.row.fromInfo == 'wph'">唯品会</span>
						<span v-if="scope.row.fromInfo == 'dy'">抖音</span>
					</template>
				</el-table-column>
				<el-table-column fixed="right" prop="tk_status" label="订单状态">
					<template slot-scope="scope">
						<span v-if="scope.row.tk_status == 3">已结算</span>
						<span v-if="scope.row.tk_status == 12">已付款</span>
						<span v-if="scope.row.tk_status == 13">已失效</span>
					</template>
				</el-table-column>
				<el-table-column fixed="right" prop="balance" label="平台结算状态" width="120">
					<template slot-scope="scope">
						<span v-if="scope.row.balance == 1" style="color: #4f9dec;">已结算</span>
						<span v-if="scope.row.balance == 0" style="color: #4f9dec;">未结算</span>
					</template>
				</el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
				<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange"
					:page-sizes="[10,20,30,40,50]" :page-size="size" :current-page="page"
					layout="total,sizes, prev, pager, next,jumper" :total="tableData.totalElements">
				</el-pagination>
			</div>
		</el-tab-pane>
		<el-tab-pane label="唯品会订单" name="wph">
			<el-table v-loading="tableDataLoading" :data="tableData.content">
				<el-table-column fixed prop="item_img" label="商品图片">
					<template slot-scope="scope">
						　　<img :src="scope.row.item_img" width="40" height="40" />
					</template>
				</el-table-column>
				<el-table-column prop="item_title" label="商品名称" width="200">
				</el-table-column>
				<el-table-column prop="relation_id" label="会员号" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="memberClick(scope.row.relation_id)">{{ scope.row.relation_id }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitation" label="直属" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitation)">{{ scope.row.invitation }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitations" label="非直属" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitations)">{{ scope.row.invitations }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="invitation" label="团长号" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitation)">{{ scope.row.invitation }}</span>
					</template>
				</el-table-column>
				<el-table-column prop="trade_id" label="订单号" width="170">
				</el-table-column>
				<el-table-column prop="item_price" label="商品单价">
				</el-table-column>
				<el-table-column prop="alipay_total_price" label="实付金额">
				</el-table-column>
				<el-table-column prop="pub_share_pre_fee" label="商品总佣金" width="120">
				</el-table-column>
				<el-table-column prop="pub_share_pre_fee_user" label="会员佣金">
					<template slot-scope="scope">
						<div>{{ scope.row.isGif !== 0 ? '首单' : '' }}</div>
						<span
							style="color: #4f9dec;">{{ scope.row.tk_status !== 13 ? scope.row.pub_share_pre_fee_user : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitationMoney" label="直属佣金" width="120">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.invitationMoney ? scope.row.invitationMoney : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitationMoneys" label="非直属佣金" width="130">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.invitationMoneys ? scope.row.invitationMoneys : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="invitationMoney" label="团长佣金">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.tk_status !== 13 ? scope.row.invitationMoney : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="pub_share_pre_fee_user_s" label="平台佣金">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{
          scope.row.tk_status !== 13 ? (scope.row.pub_share_pre_fee - scope.row.pub_share_pre_fee_user - scope.row.invitationMoneys - scope.row.invitationMoney).toFixed(2) : '—'
        }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="pub_share_pre_fee_user_s" label="平台佣金">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{
          scope.row.tk_status !== 13 ? (scope.row.pub_share_pre_fee - scope.row.pub_share_pre_fee_user - scope.row.invitationMoney).toFixed(2) : '—'
        }}</span>
					</template>
				</el-table-column>
				<el-table-column prop="tk_paid_time" label="支付时间" width="160">
				</el-table-column>
				<el-table-column prop="fromInfo" label="来源渠道">
					<template slot-scope="scope">
						<span v-if="scope.row.fromInfo == 'pdd'">拼多多</span>
						<span v-if="scope.row.fromInfo == 'tb'">淘宝</span>
						<span v-if="scope.row.fromInfo == 'jd'">京东</span>
						<span v-if="scope.row.fromInfo == 'mt'">美团</span>
						<span v-if="scope.row.fromInfo == 'elm'">饿了么</span>
						<span v-if="scope.row.fromInfo == 'dm'">多麦</span>
						<span v-if="scope.row.fromInfo == 'wph'">唯品会</span>
						<span v-if="scope.row.fromInfo == 'dy'">抖音</span>
					</template>
				</el-table-column>
				<el-table-column fixed="right" prop="tk_status" label="订单状态">
					<template slot-scope="scope">
						<span v-if="scope.row.tk_status == 3">已结算</span>
						<span v-if="scope.row.tk_status == 12">已付款</span>
						<span v-if="scope.row.tk_status == 13">已失效</span>
					</template>
				</el-table-column>
				<el-table-column fixed="right" prop="balance" label="平台结算状态" width="120">
					<template slot-scope="scope">
						<span v-if="scope.row.balance == 1" style="color: #4f9dec;">已结算</span>
						<span v-if="scope.row.balance == 0" style="color: #4f9dec;">未结算</span>
					</template>
				</el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
				<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange"
					:page-sizes="[10,20,30,40,50]" :page-size="size" :current-page="page"
					layout="total,sizes, prev, pager, next,jumper" :total="tableData.totalElements">
				</el-pagination>
			</div>
		</el-tab-pane>
		<el-tab-pane label="抖音订单" name="dy">
			<el-table v-loading="tableDataLoading" :data="tableData.content">
				<el-table-column fixed prop="item_img" label="商品图片">
					<template slot-scope="scope">
						　　<img :src="scope.row.item_img" width="40" height="40" />
					</template>
				</el-table-column>
				<el-table-column prop="item_title" label="商品名称" width="200">
				</el-table-column>
				<el-table-column prop="relation_id" label="会员号" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="memberClick(scope.row.relation_id)">{{ scope.row.relation_id }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitation" label="直属" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitation)">{{ scope.row.invitation }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitations" label="非直属" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitations)">{{ scope.row.invitations }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="invitation" label="团长号" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="colonelClick(scope.row.invitation)">{{ scope.row.invitation }}</span>
					</template>
				</el-table-column>
				<el-table-column prop="trade_id" label="订单号" width="170">
				</el-table-column>
				<el-table-column prop="item_price" label="商品单价">
				</el-table-column>
				<el-table-column prop="alipay_total_price" label="实付金额">
				</el-table-column>
				<el-table-column prop="pub_share_pre_fee" label="商品总佣金" width="120">
				</el-table-column>
				<el-table-column prop="pub_share_pre_fee_user" label="会员佣金">
					<template slot-scope="scope">
						<div>{{ scope.row.isGif !== 0 ? '首单' : '' }}</div>
						<span
							style="color: #4f9dec;">{{ scope.row.tk_status !== 13 ? scope.row.pub_share_pre_fee_user : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitationMoney" label="直属佣金" width="120">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.invitationMoney ? scope.row.invitationMoney : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="invitationMoneys" label="非直属佣金" width="130">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.invitationMoneys ? scope.row.invitationMoneys : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="invitationMoney" label="团长佣金">
					<template slot-scope="scope">
						<span
							style="color: #f56c6c;">{{ scope.row.tk_status !== 13 ? scope.row.invitationMoney : '—' }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==2" prop="pub_share_pre_fee_user_s" label="平台佣金">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{
          scope.row.tk_status !== 13 ? (scope.row.pub_share_pre_fee - scope.row.pub_share_pre_fee_user - scope.row.invitationMoneys - scope.row.invitationMoney).toFixed(2) : '—'
        }}</span>
					</template>
				</el-table-column>
				<el-table-column v-if="this.valuenum==1" prop="pub_share_pre_fee_user_s" label="平台佣金">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{
          scope.row.tk_status !== 13 ? (scope.row.pub_share_pre_fee - scope.row.pub_share_pre_fee_user - scope.row.invitationMoney).toFixed(2) : '—'
        }}</span>
					</template>
				</el-table-column>
				<el-table-column prop="tk_paid_time" label="支付时间" width="160">
				</el-table-column>
				<el-table-column prop="fromInfo" label="来源渠道">
					<template slot-scope="scope">
						<span v-if="scope.row.fromInfo == 'pdd'">拼多多</span>
						<span v-if="scope.row.fromInfo == 'tb'">淘宝</span>
						<span v-if="scope.row.fromInfo == 'jd'">京东</span>
						<span v-if="scope.row.fromInfo == 'mt'">美团</span>
						<span v-if="scope.row.fromInfo == 'elm'">饿了么</span>
						<span v-if="scope.row.fromInfo == 'dm'">多麦</span>
						<span v-if="scope.row.fromInfo == 'wph'">唯品会</span>
						<span v-if="scope.row.fromInfo == 'dy'">抖音</span>
					</template>
				</el-table-column>
				<el-table-column fixed="right" prop="tk_status" label="订单状态">
					<template slot-scope="scope">
						<span v-if="scope.row.tk_status == 3">已结算</span>
						<span v-if="scope.row.tk_status == 12">已付款</span>
						<span v-if="scope.row.tk_status == 13">已失效</span>
					</template>
				</el-table-column>
				<el-table-column fixed="right" prop="balance" label="平台结算状态" width="120">
					<template slot-scope="scope">
						<span v-if="scope.row.balance == 1" style="color: #4f9dec;">已结算</span>
						<span v-if="scope.row.balance == 0" style="color: #4f9dec;">未结算</span>
					</template>
				</el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
				<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange"
					:page-sizes="[10,20,30,40,50]" :page-size="size" :current-page="page"
					layout="total,sizes, prev, pager, next,jumper" :total="tableData.totalElements">
				</el-pagination>
			</div>
		</el-tab-pane>
		<el-tab-pane label="自营订单" name="ziying">
			<div style="margin:5px;display: inline-block;">
				<span>订单状态：</span>
				<el-select v-model="status" placeholder="请选择订单状态" style="width:150px;" @change="selectZ()">
					<el-option v-for="item in statusmain" :key="item.value" :label="item.label" :value="item.value">
					</el-option>
				</el-select>&nbsp;&nbsp;&nbsp;&nbsp;
			</div>
			<div style="margin:5px;display: inline-block;">
				<span>订单号：</span>
				<el-input style="width: 150px;" @keydown.enter.native="selectZ" clearable placeholder="请输入订单号"
					v-model="orderNum"></el-input>&nbsp;&nbsp;&nbsp;&nbsp;
			</div>
			<div style="margin:5px;display: inline-block;">
				<span>订单类型：</span>

				<el-select v-model="orderType" placeholder="请选择订单状态" style="width:150px;" @change="selectZ()">
					<el-option v-for="item in orderTypes" :key="item.value" :label="item.label" :value="item.value">
					</el-option>
				</el-select>&nbsp;&nbsp;&nbsp;&nbsp;
			</div>
			<div style="margin:5px;display: inline-block;">
				<span>开始时间：</span>
				<el-date-picker style="width: 160px;margin-left: 10px;" v-model="startTime" align="right"
					type="datetime" format="yyyy-MM-dd" value-format="yyyy-MM-dd" placeholder="选择开始时间">
				</el-date-picker>&nbsp;&nbsp;&nbsp;
			</div>
			<div style="margin:5px;display: inline-block;">
				<span>截止时间：</span>
				<el-date-picker style="width: 160px;margin-left: 10px;" v-model="endTime" align="right" type="datetime"
					format="yyyy-MM-dd" value-format="yyyy-MM-dd" placeholder="选择截止时间">
				</el-date-picker>
			</div>
			<div style="display: inline-block;">
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="selectZ">查询
				</el-button>
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="cleansZ">重置
				</el-button>
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="selectZ">刷新
				</el-button>
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="exportBtn">导出
				</el-button>
			</div>
			<el-table v-loading="tableDataLoading" :data="tableData.content">
				<el-table-column fixed prop="img" label="商品图片">
					<template slot-scope="scope">
						<img :src="scope.row.img" alt="" width="60" height="60">
					</template>
				</el-table-column>
				<el-table-column prop="title" label="商品标题" width="200">
				</el-table-column>
				<el-table-column prop="detailJson" label="商品规格">
					<template slot-scope="scope">
						<span>{{scope.row.detailJson ? scope.row.detailJson : '无'}}</span>
					</template>
				</el-table-column>
				<el-table-column prop="number" label="商品个数">
				</el-table-column>
				<el-table-column prop="descrition" label="订单备注">
					<template slot-scope="scope">
						<span>{{scope.row.descrition ? scope.row.descrition : '未填写'}}</span>
					</template>
				</el-table-column>
				<el-table-column label="订单类型" width="120">
					<template slot-scope="scope">
						<span v-if="scope.row.orderType == 1">普通订单</span>
						<span v-if="scope.row.orderType == 2">拼团订单</span>
						<span v-if="scope.row.orderType == 3">秒杀订单</span>
						<span v-if="scope.row.orderType == 4">积分订单</span>
						<span v-if="scope.row.orderType == 5">保证金订单</span>
					</template>
				</el-table-column>
				<el-table-column prop="orderNum" label="订单号" width="200">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;"
							@click="orderDetails(scope.row.id)">{{scope.row.orderNum}}</span>
					</template>
				</el-table-column>
				<el-table-column prop="consignee" label="收货人" width="120">
				</el-table-column>
				<el-table-column prop="mobile" label="手机号" width="120">
				</el-table-column>
				<el-table-column prop="detail" label="详细地址" width="120">
					<template slot-scope="scope">
						<span>{{scope.row.provinces}}{{scope.row.detail}}</span>
					</template>
				</el-table-column>
				<el-table-column prop="price" label="商品价格">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{scope.row.price | numFilter}}</span>
					</template>
				</el-table-column>
				<el-table-column prop="payMoney" label="支付金额" width="120">
					<template slot-scope="scope">
						<span style="color: #f56c6c;cursor:pointer;">{{scope.row.payMoney | numFilter}}</span>
					</template>
				</el-table-column>
				<el-table-column prop="commissionPrice" label="佣金" width="120">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{scope.row.commissionPrice?scope.row.commissionPrice : 0}}</span>
					</template>
				</el-table-column>
				<el-table-column prop="refund" label="退款原因" width="160">
					<template slot-scope="scope">
						<span>{{scope.row.refund ? scope.row.refund : '未退款'}}</span>
					</template>
				</el-table-column>
				<el-table-column prop="refusedRefund" label="驳回原因" width="160">
					<template slot-scope="scope">
						<span>{{scope.row.refusedRefund ? scope.row.refusedRefund : '未拒绝'}}</span>
					</template>
				</el-table-column>
				<el-table-column prop="payWay" label="支付方式" width="120">
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
				<el-table-column fixed="right" prop="status" label="订单状态">
					<template slot-scope="scope">
						<span style="color: #4f9dec;" v-if="scope.row.status === 1 ">待付款</span>
						<span style="color: #4f9dec;" v-if="scope.row.status === 2 ">待发货 </span>
						<span style="color: #999999;" v-if="scope.row.status === 3 ">已发货 </span>
						<span style="color: #999999;" v-if="scope.row.status === 4 ">已收货</span>
						<span style="color: #999999;" v-if="scope.row.status === 5 ">已取消</span>
						<span style="color: #4f9dec;" v-if="scope.row.status === 6 ">退款中</span>
						<span style="color: #999999;" v-if="scope.row.status === 7 ">已退款</span>
						<span style="color: #999999;" v-if="scope.row.status === 8 ">已驳回</span>
						<span style="color: #999999;" v-if="scope.row.status === 9 ">拼团中</span>
						<span style="color: #999999;" v-if="scope.row.status === 10 ">已评价</span>
					</template>
				</el-table-column>
				<el-table-column prop="createAt" label="创建时间" width="160">
				</el-table-column>
				<el-table-column prop="payTime" label="付款时间" width="160">
				</el-table-column>
				<el-table-column prop="finishTime" label="收货时间" width="160">
				</el-table-column>
				<el-table-column fixed="right" label="操作" width="100">
					<template slot-scope="scope">
						<el-button size="mini" type="primary" @click="orderDetails(scope.row.id)" style="margin: 5px;">
							订单详情
						</el-button>
						<el-button size="mini" type="primary" v-if="scope.row.orderType !==5"
							@click="contact(scope.row)" style="margin: 5px;">联系买家
						</el-button>
						<el-button size="mini" type="primary" :disabled="!isAuth('orderAdmin:refund')"
							v-if="scope.row.status === 2||scope.row.status === 3||scope.row.status === 4 "
							@click="refundClick(scope.row)" style="margin: 5px;">退款
						</el-button>
					</template>
				</el-table-column>
			</el-table>
			<div style="color: #B94A48;font-size: 20px;margin-top: 10px;display: inline-block;">
				本页累计交易统计：{{totalMoney.toFixed(2)}}元</div>
			<div style="text-align: center;margin-top: 10px;">
				<el-pagination @size-change="handleSizeChangeZy" @current-change="handleCurrentChangeZy"
					:page-sizes="[10, 20, 30, 50, 100]" :page-size="size" :current-page="page"
					layout="total,sizes, prev, pager, next" :total="tableData.totalElements">
				</el-pagination>
			</div>
		</el-tab-pane>
		<el-tab-pane label="自营分销订单" name="fenxiao">
			<div>
				<div style="margin:2% 0;display: inline-block;">
					<span>分销状态：</span>
					<el-select v-model="status" placeholder="请选择分销类型" style="width:150px;" @change="select1()">
						<el-option v-for="item in statusnum" :key="item.value" :label="item.label" :value="item.value">
						</el-option>
					</el-select>&nbsp;&nbsp;&nbsp;&nbsp;
					<span>手机号：</span>
					<el-input style="width: 150px;" @keydown.enter.native="select1" clearable placeholder="请输入手机号"
						v-model="phone"></el-input>&nbsp;&nbsp;&nbsp;&nbsp;
				</div>
				<!-- <div style="margin:5px;display: inline-block;">
					  		<span>开始时间：</span>
					  		<el-date-picker style="width: 160px;margin-left: 10px;" v-model="startTime" align="right"
					  			type="datetime" format="yyyy-MM-dd" value-format="yyyy-MM-dd" placeholder="选择开始时间">
					  		</el-date-picker>&nbsp;&nbsp;&nbsp;
					  	</div>
					  	<div style="margin:5px;display: inline-block;">
					  		<span>截止时间：</span>
					  		<el-date-picker style="width: 160px;margin-left: 10px;" v-model="endTime" align="right" type="datetime"
					  			format="yyyy-MM-dd" value-format="yyyy-MM-dd" placeholder="选择截止时间">
					  		</el-date-picker>
					  	</div> -->
				<div style="display: inline-block;">
					<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="select1">查询
					</el-button>
					<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="cleans1">重置
					</el-button>
					<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="refresh1">刷新
					</el-button>
					<!-- <el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="exportBtn">
					  			导出
					  		</el-button> -->
				</div>
				<el-table v-loading="tableDataLoading" :data="fenxiaoData.content">
					<el-table-column prop="id" label="编号">
					</el-table-column>
					<el-table-column prop="userId" label="用户id">
						<template slot-scope="scope">
							<span style="color: #4f9dec;cursor:pointer;"
								@click="updates(scope.row)">{{scope.row.userId}}</span>
						</template>
					</el-table-column>
					<el-table-column prop="phone" label="手机号">
						<template slot-scope="scope">
							<span>{{scope.row.phone?scope.row.phone:'未绑定'}}</span>
						</template>
					</el-table-column>
					<el-table-column prop="commissionPrice" label="订单佣金">
					</el-table-column>
					<el-table-column prop="payMoney" label="支付金额">
						<template slot-scope="scope">
							<span style="color: #f56c6c;cursor:pointer;">{{scope.row.payMoney | numFilter}}</span>
						</template>
					</el-table-column>
					<el-table-column prop="status" label="分销状态">
						<template slot-scope="scope">
							<span v-if="scope.row.status == 1">未到账</span>
							<span v-if="scope.row.status == 2">已到账</span>
						</template>
					</el-table-column>
					<el-table-column prop="detail" label="分销明细">
					</el-table-column>
					<el-table-column prop="createAt" label="创建时间">
					</el-table-column>
				</el-table>
				<div style="color: #B94A48;font-size: 20px;margin-top: 10px;display: inline-block;">
					本页累计交易统计：{{totalMoney.toFixed(2)}}元</div>
				<div style="text-align: center;margin-top: 10px;">
					<el-pagination @size-change="handleSizeChange1" @current-change="handleCurrentChange1"
						:page-sizes="[10, 20, 30, 50, 100]" :page-size="size" :current-page="page"
						layout="total,sizes, prev, pager, next" :total="fenxiaoData.totalElements">
					</el-pagination>
				</div>
			</div>
		</el-tab-pane>
	</el-tabs>
</template>

<script>
	export default {
		data() {
			return {
				size: 10,
				page: 1,
				balance: -1,
				invitation: '',
				relationid: '',
				tradeId: '',
				invitValue: -1,
				value: '',
				content: '',
				tkstatus: -1,
				totalnum: 0,
				valuenum: '',
				from: 'all',
				activeName: 'first',
				tableDataLoading: false,
				tableData: [],
				orderStatus: [{
					value: -1,
					label: '全部'
				}, {
					value: 12,
					label: '已付款'
				}, {
					value: 3,
					label: '已结算'
				}, {
					value: 13,
					label: '已失效'
				}],
				closeStatus: [{
					value: -1,
					label: '全部'
				}, {
					value: 1,
					label: '已结算'
				}, {
					value: 0,
					label: '未结算'
				}],
				info: {
					stockDate: this.getNowTime(), //日期
				},
				info2: {
					stockDate2: this.getNowTime2(), //日期
				},
				startTime: '',
				endTime: '',
				status: 0,
				statusmain: [{
						value: 0,
						label: '全部'
					},
					{
						value: 1,
						label: '待付款'
					},
					{
						value: 2,
						label: '待发货'
					},
					{
						value: 3,
						label: '已发货'
					},
					{
						value: 4,
						label: '已收货'
					},
					{
						value: 5,
						label: '已取消'
					},
					{
						value: 6,
						label: '退款中'
					},
					{
						value: 7,
						label: '已退款'
					},
					{
						value: 8,
						label: '已驳回'
					},
					{
						value: 9,
						label: '拼团中'
					},
					{
						value: 10,
						label: '已评价'
					}
				],
				orderNum: '',
				orderType: 0,
				orderTypes: [{
						value: 0,
						label: '全部'
					},
					{
						value: 1,
						label: '普通订单'
					},
					{
						value: 2,
						label: '拼团订单'
					},
					{
						value: 3,
						label: '秒杀订单'
					},
					{
						value: 4,
						label: '积分订单'
					},
					{
						value: 5,
						label: '保证金订单'
					}
				],
				totalMoney: 0,
				fenxiaoData: {},
				statusnum: [{
					value: 0,
					label: '全部'
				}, {
					value: 1,
					label: '未到账'
				}, {
					value: 2,
					label: '已到账'
				}],
				phone: '',
			}
		},
		filters: {
			numFilter(value) {
				let realVal = ''
				if (!isNaN(value) && value !== '') {
					// 截取当前数据到小数点后两位
					realVal = parseFloat(value).toFixed(2)
				} else {
					realVal = '--'
				}
				return realVal
			}
		},
		methods: {
			//处理默认选中当前日期
			getNowTime() {
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
			getNowTime2() {
				var now = new Date()
				var year = now.getFullYear() //得到年份
				var month = now.getMonth() - now.getMonth() //得到月份
				var date = now.getDate() - now.getDate() + 1 //得到日期
				month = month + 1
				month = month.toString().padStart(2, '0')
				date = date.toString().padStart(2, '0')
				var defaultDate = `${year}-${month}-${date}`
				return defaultDate
				this.$set(this.info, 'stockDate', defaultDate)
			},
			handleSizeChange(val) {
				this.size = val
				this.dataSelect()
			},
			handleCurrentChange(val) {
				this.page = val
				this.dataSelect()
			},
			handleSizeChangeZy(val) {
				this.size = val
				this.dataSelectZy()
			},
			handleCurrentChangeZy(val) {
				this.page = val
				this.dataSelectZy()
			},
			handleSizeChange1(val) {
				this.size = val;
				this.dataSelectFx()
			},
			handleCurrentChange1(val) {
				this.page = val;
				this.dataSelectFx()
			},
			derive() {
				let invitation = -1
				let relationid = -1
				let tradeId = -1
				if (this.invitation) {
					invitation = this.invitation
				}
				if (this.tradeId) {
					tradeId = this.tradeId
				}
				if (this.relationid) {
					relationid = this.relationid
				}
				this.tableDataLoading = true
				this.paginationShow = false
				this.$http({
					url: this.$http.adornUrl2('/order/orderListExcel'),
					method: 'get',
					responseType: 'blob',
					params: this.$http.adornParams({
						'balance': this.balance,
						'invitation': invitation,
						'tradeId': tradeId,
						'relation_id': relationid,
						'tk_status': this.tkstatus,
						'from': this.from,
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let blob = new Blob([data], {
						type: 'application/vnd.ms-excel,application/vnd.openxmlformats-officedocument.spreadsheetml.sheet'
					})
					if (window.navigator.msSaveOrOpenBlob) {
						navigator.msSaveBlob(blob)
					} else {
						let url = window.URL.createObjectURL(blob)
						let elink = document.createElement('a')
						elink.download = '订单列表.xlsx'
						elink.style.display = 'none'
						elink.href = url
						document.body.appendChild(elink)
						elink.click()
						document.body.removeChild(elink)
					}
				})
			},
			handleClick(tab, event) {
				if (tab._props.label == '淘宝订单') {
					this.size = 10
					this.page = 1
					this.from = 'tb'
					this.dataSelect()
				}
				if (tab._props.label == '拼多多订单') {
					this.size = 10
					this.page = 1
					this.from = 'pdd'
					this.dataSelect()
				}
				if (tab._props.label == '京东订单') {
					this.size = 10
					this.page = 1
					this.from = 'jd'
					this.dataSelect()
				}
				if (tab._props.label == '全部订单') {
					this.size = 10
					this.page = 1
					this.from = 'all'
					this.dataSelect()
				}
				if (tab._props.label == '美团订单') {
					this.size = 10
					this.page = 1
					this.from = 'mt'
					this.dataSelect()
				}
				if (tab._props.label == '饿了么订单') {
					this.size = 10
					this.page = 1
					this.from = 'elm'
					this.dataSelect()
				}
				if (tab._props.label == '多麦订单') {
					this.size = 10
					this.page = 1
					this.from = 'dm'
					this.dataSelect()
				}
				if (tab._props.label == '唯品会订单') {
					this.size = 10
					this.page = 1
					this.from = 'wph'
					this.dataSelect()
				}
				if (tab._props.label == '抖音订单') {
					this.size = 10
					this.page = 1
					this.from = 'dy'
					this.dataSelect()
				}
				if (tab._props.label == '自营订单') {
					this.size = 10
					this.page = 1
					this.from = ''
					this.dataSelectZy()
				}
				if (tab._props.label == '自营分销订单') {
					this.size = 10
					this.page = 1
					this.from = ''
					this.dataSelectFx()
				}

			},
			// 同步订单
			synOrder() {
				this.$http({
					url: this.$http.adornUrl2('/order/tallyOrder'),
					method: 'get',
					params: this.$http.adornParams({})
				}).then(({
					data
				}) => {
					if (data.status == 0) {
						this.$message({
							message: '订单同步中',
							type: 'success',
							duration: 1500,
							onClose: () => {
								this.dataSelect()
							}
						})
					}
					if (data.status == -100) {
						this.$message({
							message: data.msg,
							type: 'error',
							duration: 1500,
						})
					}
				})
			},
			// 会员跳转详情页
			memberClick(relationId) {
				this.$http({
					url: this.$http.adornUrl2('/user/relationId'),
					method: 'get',
					params: this.$http.adornParams({
						'relationId': relationId,
					})
				}).then(({
					data
				}) => {
					let returnData = data.data
					let id = returnData.id
					if (this.valuenum == 1) {
						this.$router.push({
							path: '/userDetail',
							query: {
								userId: id
							}
						})
					} else {
						this.$router.push({
							path: '/userDetailThree',
							query: {
								userId: id
							}
						})
					}
				})
			},
			// 团长跳转详情页
			colonelClick(relationId) {
				this.$http({
					url: this.$http.adornUrl2('/user/relationId'),
					method: 'get',
					params: this.$http.adornParams({
						'relationId': relationId,
					})
				}).then(({
					data
				}) => {
					let returnData = data.data
					let id = returnData.id
					if (this.valuenum == 1) {
						this.$router.push({
							path: '/userDetail',
							query: {
								userId: id
							}
						})
					} else {
						this.$router.push({
							path: '/userDetailThree',
							query: {
								userId: id
							}
						})
					}
				})
			},
			// select自动选择
			selectTrigger() {
				this.dataSelect()
			},
			// 查询
			select() {
				if (!this.invitation) {
					if (this.$route.query.invitation) {
						this.invitation = this.$route.query.invitation
					} else {
						this.invitation = ''
					}
				}
				if (!this.tradeId) {
					if (this.$route.query.tradeId) {
						this.tradeId = this.$route.query.tradeId
					} else {
						this.tradeId = ''
					}
				}
				if (!this.relationid) {
					if (this.$route.query.relationid) {
						this.relationid = this.$route.query.relationid
					} else {
						this.relationid = ''
					}
				}
				this.dataSelect()
			},
			// 重置
			cleans() {
				this.relationid = ''
				this.invitation = ''
				this.invitValue = ''
				this.tradeId = ''
				this.tkstatus = -1
				this.balance = -1
				this.dataSelect()
			},
			// 获取数据列表
			dataSelect() {
				let invitation = -1
				let relationid = -1
				let tradeId = -1
				if (this.invitation) {
					invitation = this.invitation
				}
				if (this.relationid) {
					relationid = this.relationid
				}
				if (this.tradeId) {
					tradeId = this.tradeId.trim()
				}
				this.tableDataLoading = true
				this.paginationShow = false
				this.$http({
					url: this.$http.adornUrl2(`/order/orderList/${this.page - 1}/${this.size}`),
					method: 'get',
					params: this.$http.adornParams({
						'balance': this.balance,
						'invitation': invitation,
						'relation_id': relationid,
						'tradeId': tradeId,
						'tk_status': this.tkstatus,
						'from': this.from,
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let init = data.data;
					for (var i in init.content) {
						if (init.content[i].item_img) {

							if (init.content[i].item_img.indexOf('https') == -1) {
								init.content[i].item_img = 'https:' + init.content[i].item_img
							}

						}
					}

					this.tableData = init

					this.$nextTick(function() {
						this.paginationShow = true
					})
				})
			},
			valueSelect() {
				this.$http({
					url: this.$http.adornUrl2('/common/type/91'),
					method: 'get',
					params: this.$http.adornParams({})
				}).then(({
					data
				}) => {
					let returnData = data.data
					this.valuenum = returnData.value
					this.dataSelect()
				})
			},
			// 获取自营订单数据
			dataSelectZy() {
				// if (this.endTime == '') {
				// 	this.endTime = this.info.stockDate
				// }
				// if (this.startTime == '') {
				// 	this.startTime = this.info2.stockDate2
				// }
				let page = this.page - 1
				this.totalMoney = 0
				this.tableDataLoading = true
				var endTime = this.endTime
				if (this.endTime != '') {
					endTime = this.endTime + " 23:59:59"
				}
				this.$http({
					url: this.$http.adornUrl2('/orders/list'),
					method: 'get',
					params: this.$http.adornParams({
						'page': page,
						'size': this.size,
						'orderNum': this.orderNum,
						'status': this.status,
						'orderType': this.orderType,
						'startTime': this.startTime,
						'endTime': endTime
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					this.tableData = returnData
					for (var i in this.tableData.content) {
						if (this.tableData.content[i].payMoney) {
							this.totalMoney = this.totalMoney + this.tableData.content[i].payMoney
						}
					}

				})
			},
			//搜索
			selectZ() {
				this.page = 1
				this.size = 10
				this.dataSelectZy()
			},
			cleansZ() {
				this.orderNum = ''
				this.status = 0
				this.orderType = 0
				this.startTime = '',
					this.endTime = '',
					this.dataSelectZy()
			},
			// 导出
			exportBtn() {
				if (this.endTime == '') {
					this.endTime = this.info.stockDate
				}
				if (this.startTime == '') {
					this.startTime = this.info2.stockDate2
				}
				var endTime = this.endTime
				if (this.endTime != '') {
					endTime = this.endTime + " 23:59:59"
				}
				this.$http({
					url: this.$http.adornUrl2('/orders/listExcel'),
					method: 'get',
					responseType: 'blob',
					params: this.$http.adornParams({
						// 'page': page,
						// 'size': this.size,
						'orderNum': this.orderNum,
						'status': this.status,
						'orderType': this.orderType,
						'startTime': this.startTime,
						'endTime': endTime
					})
				}).then(({
					data
				}) => {
					let blob = new Blob([data], {
						type: 'application/vnd.ms-excel,application/vnd.openxmlformats-officedocument.spreadsheetml.sheet'
					})
					if (window.navigator.msSaveOrOpenBlob) {
						navigator.msSaveBlob(blob)
					} else {
						let url = window.URL.createObjectURL(blob)
						let elink = document.createElement('a')
						elink.download = '订单列表.xlsx'
						elink.style.display = 'none'
						elink.href = url
						document.body.appendChild(elink)
						elink.click()
						document.body.removeChild(elink)
					}
				})
			},
			// 详情
			orderDetails(id) {
				this.$router.push({
					path: '/orderDetails',
					query: {
						id: id
					}
				})
			},
			// 联系买家
			contact(row) {
				this.userId = row.userId;
				this.$http({
					url: this.$http.adornUrl2(`/user/${row.userId}`),
					method: "get",
					params: this.$http.adornParams({}),
				}).then(({
					data
				}) => {
					let returnData = data.data;
					if (returnData == null) {
						this.$message({
							message: "用户信息不正确",
							type: "error",
							duration: 1500,
						});
					} else {
						this.userHead = returnData.image_url;
						this.userName = returnData.nickName;
						this.contactSelect();
					}
				});
			},
			contactSelect() {
				this.$http({
					url: this.$http.adornUrl2("/chat/save"),
					method: "post",
					data: this.$http.adornData({
						userId: this.userId,
						userHead: this.userHead,
						userName: this.userName,
						storeId: this.storeId,
						storeHead: this.storeHead,
						storeName: this.storeName,
					}),
				}).then(({
					data
				}) => {
					if (data.status == 0) {
						console.log("data :>> ", data.userName);
						this.$confirm(`联系买家${data.data.userName}?`, "提示", {
							confirmButtonText: "确定",
							cancelButtonText: "取消",
							type: "warning",
						}).then(() => {
							this.$router.push({
								path: "/vueMchat",
								query: {
									userId: data.data.userId,
									userName: data.data.userName
								}
							});
						});
					}
				});
			},
			// 退款
			refundClick(row) {
				this.$confirm(`确定退款?`, '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					this.$http({
						url: this.$http.adornUrl2('/orders/refundMoney'),
						method: 'get',
						params: this.$http.adornParams({
							'ordersId': row.id
						})
					}).then(({
						data
					}) => {
						if (data.status == 0) {
							this.$message({
								message: '退款成功',
								type: 'success',
								duration: 1500,
								onClose: () => {
									this.dataSelect()
								}
							})
						} else {
							this.$message({
								message: data.msg,
								type: 'error',
								duration: 1500,
								onClose: () => {
									this.dataSelect()
								}
							})
						}
					})
				}).catch(() => {})
			},
			// 拒绝退款
			refuseClick(row) {
				this.ordersId = row.id
				this.dialogFormVisible1 = true
			},
			refuseNoticeTo() {
				if (this.refusedRefund == '') {
					this.$message({
						message: '请输入拒绝退款理由',
						type: 'error',
						duration: 1500,
						onClose: () => {
							this.refusedRefund = ''
							this.dataSelect()
						}
					})
				} else {
					this.$http({
						url: this.$http.adornUrl2('/orders/refusedRefund'),
						method: 'get',
						params: this.$http.adornParams({
							'ordersId': this.ordersId,
							'refusedRefund': this.refusedRefund,
						})
					}).then(({
						data
					}) => {
						this.dialogFormVisible1 = false
						this.$message({
							message: '操作成功',
							type: 'success',
							duration: 1500,
							onClose: () => {
								this.refusedRefund = ''
								this.dataSelect()
							}
						})
					})
				}

			},
			// 分销订单
			dataSelectFx() {
				let page = this.page - 1
				this.tableDataLoading = true
				this.totalMoney = 0
				this.$http({
					url: this.$http.adornUrl2('/ordersRelation/list'),
					method: 'get',
					params: this.$http.adornParams({
						'page': page,
						'size': this.size,
						'userId': '',
						'phone': this.phone,
						'status': this.status,
						'moneyFrom': 0,
						'startTime': '',
						'endTime': ''
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					for (var i in data.data.content) {
						if (data.data.content[i].payMoney) {
							this.totalMoney = this.totalMoney + data.data.content[i].payMoney
						}
					}
					this.fenxiaoData = returnData
				})
			},
			select1() {
				this.page = 1
				this.size = 10
				this.dataSelectFx()
			},
			cleans1() {
				this.phone = ''
				this.status = 0
				this.dataSelectFx()
			},
			// 刷新
			refresh1() {
				this.dataSelectFx()
			},
		},
		mounted() {
			this.valueSelect()
			this.select()
		}
	}
</script>

<style>

</style>
