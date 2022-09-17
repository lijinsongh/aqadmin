<template>
	<el-tabs v-model="activeName" @tab-click="handleClick">
		<el-tab-pane label="店铺信息" name="first">
			<div style="width: 100%;">
				<div class="shop_item">
					<div class="shop_content" style="max-width: 800px;margin: auto;">
						<div class="content_item">
							<div style="display: flex;align-items: center;margin: 2% 0;">
								<span style="display: inline-block;text-align: left;width: 120px;">店铺logo：</span>
			  			<div
									style="width:148px;height:148px;background-color: #fbfdff; border: 1px dashed #c0ccda;border-radius: 6px;text-align: center;line-height: 148px;">
									<el-upload class="avatar-uploader" v-model="logo"
										action="https://tbshopsadmin.hnqxkjfz.com/tao/alioss/upload"
										:show-file-list="false" :on-success="handleAvatarSuccess">
										<img v-if="logo" :src="logo" class="avatar"
											style="width: 148px;height: 148px;border-radius:6px;" />
										<i v-else class="el-icon-plus avatar-uploader-icon"
											style="font-size: 28px;color: #8c939d"></i>
									</el-upload>
								</div>
							</div>
						</div>
						<div class="content_item">
							<div style="display: flex;align-items: center;margin: 2% 0;">
								<span style="display: inline-block;text-align: left;width: 120px;">店铺背景图：</span>
								<div style="display: flex;">
									<div v-for="(item,index) of imgs" style="display: inline-block;">
										<div style="position: relative;margin: 5px;">
											<img :src="item" class="avatar"
												style="width:148px;height: 148px;border-radius: 6px;" />
											<div @click="clear(index)" class="divhove" v-if="imgs">
												<i class="el-icon-delete"
													style="margin: 0;font-size: 18px;color: #fff;"></i>
											</div>
										</div>
									</div>
									<el-upload ref='upload'
										action="https://tbshopsadmin.hnqxkjfz.com/tao/alioss/upload"
										list-type="picture-card" :on-success="handleUploadSuccess"
										:on-change="handleChange" :on-remove="handleRemove">
										<i class="el-icon-plus"></i>
									</el-upload>
								</div>
							</div>
						</div>
						<div class="content_item">
							<span style="width: 120px;text-align: left;display:inline-block;">店铺名称:</span>
							<el-input style="width:50%;" v-if="isshow" disabled class="margin15" placeholder="请输入店铺名称"
								v-model="storeName" autosize></el-input>
							<el-input style="width:50%;" v-if="!isshow" class="margin15" placeholder="请输入店铺名称"
								v-model="storeName" autosize></el-input>
						</div>
						<div class="content_item">
							<span>店铺发货地址:</span>
							<el-input style="width:50%;" v-if="isshow" disabled class="margin15" placeholder="请输入店铺发货地址"
								v-model="shipAddress" autosize></el-input>
							<el-input style="width:50%;" v-if="!isshow" class="margin15" placeholder="请输入店铺发货地址"
								v-model="shipAddress" autosize></el-input>
						</div>
						<div class="content_item">
							<span>店铺评分:</span>
							<el-input style="width:50%;" disabled class="margin15" placeholder="请输入店铺评分"
								v-model="score" autosize></el-input>
							<!-- <el-input style="width:50%;" v-if="!isshow" class="margin15" placeholder="请输入店铺评分"
								v-model="score" autosize></el-input> -->
						</div>
						<div class="content_item">
							<span style="position: relative;top: -65px;">店铺描述:</span>
							<el-input style="width:50%;" v-if="isshow" type="textarea" :rows="4" disabled
								class="margin15" placeholder="请输入店铺描述" v-model="describes"></el-input>
							<el-input style="width:50%;" v-if="!isshow" type="textarea" :rows="4" class="margin15"
								placeholder="请输入店铺描述" v-model="describes"></el-input>
						</div>
					</div>
				</div>
				<div class="shop_item">
					<div class="shop_title" style="max-width: 1000px;display: flex;margin: 20px auto;font-size: 20px;">
					</div>
					<div class="shop_content" style="max-width: 800px;margin: auto;">
			  	<div class="content_item">
							<span>店铺退货地址:</span>
							<el-input style="width:50%;" v-if="isshow" disabled class="margin15" placeholder="请输入店铺退货地址"
								v-model="refundAddress" autosize></el-input>
							<el-input style="width:50%;" v-if="!isshow" class="margin15" placeholder="请输入店铺退货地址"
								v-model="refundAddress" autosize></el-input>
						</div>
						<div class="content_item">
							<span>店铺退货收货人:</span>
							<el-input style="width:50%;" v-if="isshow" disabled class="margin15"
								placeholder="请输入店铺退货收货人" v-model="refundContact" autosize></el-input>
							<el-input style="width:50%;" v-if="!isshow" class="margin15" placeholder="请输入店铺退货收货人"
								v-model="refundContact" autosize></el-input>
						</div>
						<div class="content_item">
							<span>店铺退货手机号:</span>
							<el-input style="width:50%;" v-if="isshow" disabled class="margin15" placeholder="请输入店铺手机号"
								v-model="refundMobile" autosize></el-input>
							<el-input style="width:50%;" v-if="!isshow" class="margin15" placeholder="请输入店铺手机号"
								v-model="refundMobile" autosize></el-input>
						</div>
					</div>
				</div>
				<div class="shop_item" style="text-align:left;margin-left: 20%;">
					<el-button v-if="isbtn" style="margin-bottom: 20px;" size="mini" type="primary"
						:disabled="!isAuth('merchantList:update')" @click="updates">修改
					</el-button>
					<div v-if="!isbtn">
						<el-button style="margin-bottom: 20px;" size="mini" type="primary" @click="cancel">取消
						</el-button>
						<el-button style="margin-bottom: 20px;" size="mini" type="primary" @click="saveClick">保存
						</el-button>
					</div>
				</div>
			</div>
		</el-tab-pane>
		<el-tab-pane label="店铺收入" name="second">
			<el-select v-model="way" style="width:150px;margin-left: 10px;" @change="animeDat">
				<el-option v-for="item in ways" :key="item.value" :label="item.label" :value="item.value">
				</el-option>
			</el-select>&nbsp;&nbsp;&nbsp;
			<el-date-picker style="width: 160px;margin-left: 10px;" v-model="info.stockDate" align="right"
				type="datetime" format="yyyy-MM-dd" value-format="yyyy-MM-dd" placeholder="选择开始时间" @change="timeDate">
			</el-date-picker>
			</div>
			<el-row>
				<el-col :span="6" class="cards">
					<div class="box">
						<div class="box_num">
							<div class="box_color">总收入</div>
							<div class="text_color"><span>{{sumincome | numFilter}}</span>元</div>
						</div>
					</div>
				</el-col>
				<el-col :span="6" class="cards">
					<div class="box">
						<div class="box_num">
							<div class="box_color">当前收入</div>
							<div class="text_color"><span>{{income | numFilter}}</span>元</div>
						</div>
					</div>
				</el-col>
				<el-col :span="6" class="cards">
					<div class="box">
						<div class="box_num">
							<div class="box_color">微信收入</div>
							<div class="text_color"><span>{{wxincome | numFilter}}</span>元</div>
						</div>
					</div>
				</el-col>
				<el-col :span="6" class="cards">
					<div class="box">
						<div class="box_num">
							<div class="box_color">支付宝收入</div>
							<div class="text_color"><span>{{zfbincome | numFilter}}</span>元</div>
						</div>
					</div>
				</el-col>
			</el-row>
			<el-row>
				<el-col :span="6" class="cards">
					<div class="box">
						<div class="box_num">
							<div class="box_color">零钱收入</div>
							<div class="text_color"><span>{{yueicome | numFilter}}</span>元</div>
						</div>
					</div>
				</el-col>
				<el-col :span="6" class="cards">
					<div class="box">
						<div class="box_num">
							<div class="box_color">订单年成交量</div>
							<div class="text_color"><span>{{ordersum0}}</span>件</div>
						</div>
					</div>
				</el-col>
				<el-col :span="6" class="cards">
					<div class="box">
						<div class="box_num">
							<div class="box_color">订单月成交量</div>
							<div class="text_color"><span>{{ordersum1}}</span>件</div>
						</div>
					</div>
				</el-col>
				<el-col :span="6" class="cards">
					<div class="box">
						<div class="box_num">
							<div class="box_color">订单日成交量</div>
							<div class="text_color"><span>{{ordersum2}}</span>件</div>
						</div>
					</div>
				</el-col>
			</el-row>
		</el-tab-pane>
		<el-tab-pane label="余额明细" name="yue">
			<el-table v-loading="tableDataLoading" :data="tableDataYe.content">

				<el-table-column prop="id" label="编号" >
				</el-table-column>
				<el-table-column prop="title" label="标题">
				</el-table-column>
				<el-table-column prop="content" label="内容" >
				</el-table-column>

				<el-table-column prop="money" label="金额" >
					<template slot-scope="scope">
						<span v-if="scope.row.type==1" style="color:limegreen;cursor: pointer;">+{{scope.row.money}}</span>
						<span v-else style="color: red;cursor: pointer;">-{{scope.row.money}}</span>
					</template>
				</el-table-column>
				<el-table-column prop="createTime" label="时间" >
				</el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
				<el-pagination @size-change="handleSizeChangeYe" @current-change="handleCurrentChangeYe"
					:page-sizes="[10, 20, 30, 50, 100]" :page-size="size" :current-page="page"
					layout="total,sizes, prev, pager, next" :total="tableDataYe.totalElements">
				</el-pagination>
			</div>
		</el-tab-pane>
		<el-tab-pane label="提现记录" name="third">
			<el-table v-loading="tableDataLoading" :data="withdrawData.content">
				<el-table-column fixed prop="id" label="编号" width="80">
				</el-table-column>
				<el-table-column prop="zhifubao" label="支付宝账号">
				</el-table-column>
				<el-table-column prop="zhifubaoName" label="支付宝名称">
				</el-table-column>
				<el-table-column prop="money" label="提现金额">
				</el-table-column>
				<el-table-column prop="refund" label="退款原因">
					<template slot-scope="scope">
						<span>{{scope.row.refund ? scope.row.refund : '未退款'}}</span>
					</template>
				</el-table-column>
				<el-table-column prop="createAt" label="申请时间" width="170">
				</el-table-column>
				<el-table-column prop="outAt" label="转账/退款时间" width="160">
				</el-table-column>
				<el-table-column prop="orderNumber" label="转账订单号" width="160">
				</el-table-column>
				<el-table-column prop="state" label="状态" width="100">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;" v-if="scope.row.state == 0">待转账</span>
						<span v-if="scope.row.state == 1">已转账</span>
						<span v-if="scope.row.state == -1">已退款</span>
					</template>
				</el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
				<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange"
					:page-sizes="[10, 20, 30, 40, 50]" :page-size="size" :current-page="page"
					layout="total,sizes, prev, pager, next" :total="withdrawData.totalElements">
				</el-pagination>
			</div>
		</el-tab-pane>
		<el-tab-pane label="商品分类" name="classify">
			<div style="float: right;margin-right:2%;">
				<el-button style='margin: 10px 0;' :disabled="!isAuth('merchantList:add')" size="mini" type="primary"
					icon="document" @click="addNotice">添加分类</el-button>
			</div>
			<el-table v-loading="tableDataLoading" :data="typeDatas.content" row-key="id">
				<el-table-column prop="storeTypeId" label="编号" width="80">
				</el-table-column>
				<el-table-column prop="img" label="图片">
					<template slot-scope="scope">
						<img :src="scope.row.img" alt="" width="40" height="40">
					</template>
				</el-table-column>
				<el-table-column prop="name" label="名称">
				</el-table-column>
				<el-table-column prop="createTime" label="创建时间">
				</el-table-column>
				<el-table-column label="操作" width="180">
					<template slot-scope="scope">
						<el-button size="mini" type="primary" :disabled="!isAuth('merchantList:update')"
							@click="updatesclass(scope.$index, scope.row)">修改
						</el-button>
						<el-button size="mini" type="danger" :disabled="!isAuth('merchantList:delete')"
							@click="deletes(scope.row)">删除
						</el-button>
					</template>
				</el-table-column>
			</el-table>
			<!-- 添加分类 -->
			<el-dialog title="添加分类" :visible.sync="dialogFormVisible" center>
				<div style="margin-bottom: 10px;">
					<span style="width: 200px;display: inline-block;text-align: right;">分类名称：</span>
					<el-input style="width:50%;" v-model="name" placeholder="请输入分类名称"></el-input>
				</div>
				<div style="margin-bottom: 10px;display:flex;">
					<span style="width: 200px;display: inline-block;text-align: right;">分类图标：</span>
					<div
						style=" width:148px;height:148px;border: 1px dashed #c0ccda;border-radius: 6px;text-align: center;line-height: 148px;">
						<el-upload class="avatar-uploader" v-model="img"
							action="https://tbshopsadmin.hnqxkjfz.com/tao/alioss/upload" :show-file-list="false"
							:on-success="handleAvatarSuccess1">
							<img v-if="img" :src="img" class="avatar"
								style="border-radius: 6px;width: 148px;height: 148px;" />
							<i v-else class="el-icon-plus avatar-uploader-icon"></i>
						</el-upload>
					</div>
				</div>
				<div slot="footer" class="dialog-footer">
					<el-button @click="dialogFormVisible = false">取 消</el-button>
					<el-button type="primary" @click="releasNoticeTo()">确 定</el-button>
				</div>
			</el-dialog>
			<!-- 修改分类 -->
			<el-dialog title="修改分类" :visible.sync="dialogFormVisible1" center>
				<el-form :model="form">
					<el-form-item label="分类名称：" :label-width="formLabelWidth">
						<el-input v-model="form.name" style="width:65%;" placeholder="请输入分类名称"></el-input>
					</el-form-item>
					<el-form-item label="图标：" :label-width="formLabelWidth">
						<div
							style=" width:148px;height:148px;border: 1px dashed #c0ccda;border-radius: 6px;text-align: center;line-height: 148px;">
							<el-upload class="avatar-uploader" v-model="form.img"
								action="https://tbshopsadmin.hnqxkjfz.com/tao/alioss/upload"
								:show-file-list="false" :on-success="handleAvatarSuccess2">
								<img v-if="form.img" :src="form.img" class="avatar"
									style="border-radius: 6px;width: 148px;height: 148px;" />
								<i v-else class="el-icon-plus avatar-uploader-icon"></i>
							</el-upload>
						</div>
					</el-form-item>
				</el-form>
				<div slot="footer" class="dialog-footer">
					<el-button @click="dialogFormVisible1 = false">取 消</el-button>
					<el-button type="primary" @click="classNoticeTo()">确 定</el-button>
				</div>
			</el-dialog>
		</el-tab-pane>
		<el-tab-pane label="商品规格" name="cation">
			<div>
				<div style="float: right;margin-right:2%;">
					<el-button style='margin: 10px 0;' :disabled="!isAuth('specification:add')" size="mini" type="primary"
						icon="document" @click="cationAddNotice">添加规格</el-button>
				</div>
				<el-table v-loading="tableDataLoading" :data="cationDatas.content" row-key="id">
					<el-table-column prop="id" label="id" width="80">
					</el-table-column>
					<el-table-column prop="ruleName" label="规格名称">
					</el-table-column>
					<el-table-column label="规格值">
						<template slot-scope="scope">
							<div v-for="(item,index) in scope.row.ruleValue" :key="index">
								<div>
									<span>{{item.value}}:</span>
									<span>{{item.detail}}</span>
								</div>
							</div>
						</template>
					</el-table-column>
					<el-table-column prop="createTime" label="创建时间">
					</el-table-column>
					<el-table-column label="操作" width="180">
						<template slot-scope="scope">
							<el-button size="mini" type="primary" :disabled="!isAuth('specification:update')"
								@click="cationUpdates(scope.row)">修改
							</el-button>
							<el-button size="mini" type="danger" :disabled="!isAuth('specification:delete')"
								@click="cationDeletes(scope.row)">删除
							</el-button>
						</template>
					</el-table-column>
				</el-table>
				<div style="text-align: center;margin-top: 10px;">
					<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange"
						:page-sizes="[10, 20, 30, 50, 100]" :page-size="size" :current-page="page"
						layout="total,sizes, prev, pager, next" :total="cationDatas.totalElements">
					</el-pagination>
				</div>
				<!-- 添加规格 -->
				<el-dialog title="添加规格" customClass="customWidth" :visible.sync="dialogFormVisibleC">
					<div style="margin-bottom: 10px;">
						<span style="width: 80px;display: inline-block;text-align: right;">规格标题：</span>
						<el-input style="width:200px" v-model="ruleName" placeholder="请输入标题名称 例:衣服"></el-input>
					</div>
					<div style="margin:40px 0 0 73px;">
						<div class="ruleitem" v-for="(item,index) in this.ruleValue" :key="index">
							<div>{{item.value}} <span @click="deleterule(index)">
									<icon-svg name="schu" class="ruleicon"></icon-svg>
								</span></div>
							<div>
								<el-tag :key="tag" v-for="(tag,j) in item.detail.split(',')" closable
									:disable-transitions="false" @close="handleClose(tag,j,item)">
									{{tag}}
								</el-tag>
								<el-input class="input-new-tag" v-model="inputValues[index]" ref="saveTagInput" size="small"
									style="width:120px;" placeholder="请输入属性名"
									@keyup.enter.native="handleInputConfirm(index,item)">
								</el-input>
								<el-button class="button-new-tag" size="small" @click="handleInputConfirm(index,item)">添加
								</el-button>
							</div>
						</div>
					</div>
					<div style="margin:30px 0 0 83px;">
						<div v-if="cationisshow" class="btn_specif">
							<el-button type="primary" @click="btnToclick()">添加新规格</el-button>
						</div>
					</div>
					<div style="margin:10px 0 0 0px;">
						<div v-if="!cationisshow" style="margin-top: 20px;" :visible.sync="dialogFormVisibleC">
							<div style="display: inline-block;">
								<span style="width: 80px;display: inline-block;text-align: right;">规格：</span>
								<el-input style="width:50%;" v-model="releobject.value" placeholder="例:颜色"></el-input>
							</div>
							<div style="display: inline-block;">
								<span style="width: 80px;display: inline-block;text-align: right;">规格值：</span>
								<el-input style="width:50%;" v-model="releobject.detail" placeholder="例:黑色"></el-input>
							</div>
							<div style="display: inline-block;">
								<el-button type="primary" @click="speciTo()">确 定</el-button>
								<el-button @click="speciTotwo()">取 消</el-button>
							</div>
						</div>
					</div>
					<div slot="footer" class="dialog-footer">
						<el-button @click="dialogFormVisibleC = false">取 消</el-button>
						<el-button type="primary" @click="specificationTo()">确 定</el-button>
					</div>
				</el-dialog>
				<!-- 修改规格 -->
				<el-dialog title="修改规格" customClass="customWidth" :visible.sync="dialogFormVisibleC1">
					<el-form :model="cationform">
						<el-form-item label="规格标题：">
							<el-input v-model="cationform.ruleName" style="width:200px;" placeholder="请输入分类名称"></el-input>
						</el-form-item>
						<el-form-item>
							<div style="margin-left:73px;">
								<div class="ruleitem" v-for="(item,index) in cationform.ruleValue" :key="index">
									<div>{{item.value}} <span @click="deleterule1(index)">
											<icon-svg name="schu" class="ruleicon"></icon-svg>
										</span></div>
									<div>
										<el-tag :key="tag" v-for="(tag,j) in item.detail.split(',')" closable
											:disable-transitions="false" @close="handleClose1(tag,j,item)">
											{{tag}}
										</el-tag>
										<el-input class="input-new-tag" v-model="inputValues[index]" ref="saveTagInput"
											size="small" style="width:120px;" placeholder="请输入属性名"
											@keyup.enter.native="handleInputConfirm1(index,item)">
										</el-input>
										<el-button class="button-new-tag" size="small" @click="handleInputConfirm(index,item)">
											添加</el-button>
									</div>
								</div>
							</div>
						</el-form-item>
						<el-form-item>
							<div style="margin-left:83px;">
								<div v-if="isshow" class="btn_specif">
									<el-button type="primary" @click="btnToclick1()">添加新规格</el-button>
								</div>
							</div>
							<div style="margin-top:10px;">
								<div v-if="!isshow" style="margin-top: 20px;" :visible.sync="dialogFormVisibleC">
									<div style="display: inline-block;">
										<span style="width: 100px;display: inline-block;text-align: right;">规格：</span>
										<el-input style="width:50%;" v-model="releobject.value" placeholder="请输入规格"></el-input>
									</div>
									<div style="display: inline-block;">
										<span style="width: 100px;display: inline-block;text-align: right;">规格值：</span>
										<el-input style="width:50%;" v-model="releobject.detail" placeholder="请输入规格值">
										</el-input>
									</div>
									<div style="display: inline-block;">
										<el-button type="primary" @click="speciTo1()">确 定</el-button>
										<el-button @click="speciTotwo1()">取 消</el-button>
									</div>
								</div>
							</div>
						</el-form-item>
					</el-form>
					<div slot="footer" class="dialog-footer">
						<el-button @click="dialogFormVisibleC1 = false">取 消</el-button>
						<el-button type="primary" @click="cationAmendNoticeTo()">确 定</el-button>
					</div>
				</el-dialog>
			</div>
		</el-tab-pane>
		<el-tab-pane label="商品管理" name="shop">
			<div style="margin:2% 0;display: inline-block;">
				<span>商品标题：</span>
				<el-input style="width: 150px;" @keydown.enter.native="select" clearable placeholder="请输入商品标题"
					v-model="title"></el-input>&nbsp;&nbsp;&nbsp;&nbsp;
				<span>商品类型：</span>
				<el-cascader style="width:150px;" v-model="value" :options="classDatas" :show-all-levels="false"
					:change-on-select="true" :props="{ expandTrigger: 'hover' }" @change="handleChange">
				</el-cascader>&nbsp;&nbsp;&nbsp;&nbsp;
				<span>商品状态：</span>
				<el-select v-model="status" placeholder="请选择商品状态" style="width:150px;" @change="select(status)">
					<el-option v-for="item in statusmain" :key="item.value" :label="item.label" :value="item.value">
					</el-option>
				</el-select>&nbsp;&nbsp;&nbsp;&nbsp;
			</div>
			<div style="display: inline-block;">
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="select">查询
				</el-button>
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="cleans">重置
				</el-button>
				<el-button style='margin-left:15px;' :disabled="!isAuth('shopAdmin:add')" size="mini" type="primary"
					icon="document" @click="addShop">发布商品</el-button>
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="refresh">刷新
				</el-button>
			</div>
			<el-table v-loading="tableDataLoading" :data="shopData.content">
				<el-table-column fixed prop="id" label="编号" width="80">
				</el-table-column>
				<el-table-column fixed prop="coverImg" label="商品图片">
					<template slot-scope="scope">
						<img :src="scope.row.coverImg" alt="" width="60" height="60">
					</template>
				</el-table-column>
				<el-table-column prop="type" label="商品类型">
					<template slot-scope="scope">
						<span v-if="scope.row.type!=null">{{scope.row.type.name}}</span>
						<span v-else>无</span>
					</template>
				</el-table-column>
				<el-table-column prop="title" label="商品标题" width="200">
				</el-table-column>
				<el-table-column prop="originalPrice" label="商品原价">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{scope.row.originalPrice | numFilter}}</span>
					</template>
				</el-table-column>
				<el-table-column prop="price" label="商品价格">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{scope.row.price | numFilter}}</span>
					</template>
				</el-table-column>
				<el-table-column prop="memberPrice" label="会员价格">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{scope.row.memberPrice | numFilter}}</span>
					</template>
				</el-table-column>
				<el-table-column prop="commissionPrice" label="佣金">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{scope.row.commissionPrice | numFilter}}</span>
					</template>
				</el-table-column>
				<el-table-column prop="sales" label="商品销量">
				</el-table-column>
				<el-table-column prop="buyReason" label="必买理由" width="200">
				</el-table-column>
				<el-table-column label="精选好物" width="120">
					<template slot-scope="scope">
						<span v-if="scope.row.isSelect == 0">否</span>
						<span v-if="scope.row.isSelect == 1">是</span>
					</template>
				</el-table-column>
				<el-table-column label="首页商品" width="120">
					<template slot-scope="scope">
						<span v-if="scope.row.homeGoods== 0">否</span>
						<span v-if="scope.row.homeGoods == 1">是</span>
					</template>
				</el-table-column>
				<el-table-column label="每日推荐" width="120">
					<template slot-scope="scope">
						<span v-if="scope.row.isRecommend== 0">否</span>
						<span v-if="scope.row.isRecommend == 1">是</span>
					</template>
				</el-table-column>
				<el-table-column label="是否需要发货" width="150">
					<template slot-scope="scope">
						<span v-if="scope.row.isExpress== 2">虚拟商品无需发货</span>
						<span v-if="scope.row.isExpress == 1">普通商品需要发货</span>
						<span v-if="scope.row.isExpress == 3">门店核销无需发货</span>
					</template>
				</el-table-column>

				<el-table-column prop="createAt" label="创建时间" width="160">
				</el-table-column>
				<el-table-column fixed='right' label="上下架">
					<template slot-scope="scope">
						<el-switch v-model="scope.row.status" @change="change(scope.row.id)" :active-value="openValue"
							:inactive-value="closeValue" active-color="#13ce66" inactive-color="#ff4949">
						</el-switch>
					</template>
				</el-table-column>
				<el-table-column fixed="right" prop="status" label="商品状态">
					<template slot-scope="scope">
						<span style="color: #4f9dec;cursor: pointer;" v-if="scope.row.status === 1 ">上架</span>
						<span style="color: #4f9dec;cursor: pointer;" v-if="scope.row.status === 2 ">下架</span>
					</template>
				</el-table-column>
				<el-table-column fixed="right" label="操作" width="180">
					<template slot-scope="scope">
						<el-button size="mini" type="primary" @click="discuss(scope.$index, scope.row)">商品评价
						</el-button>
						<el-button size="mini" type="primary" :disabled="!isAuth('merchantList:update')"
							@click="shopUpdates(scope.$index, scope.row)">修改
						</el-button>
					</template>
				</el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
				<el-pagination @size-change="handleSizeChange1" @current-change="handleCurrentChange1"
					:page-sizes="[10, 20, 30, 40, 50]" :page-size="size" :current-page="page"
					layout="total,sizes, prev, pager, next" :total="shopData.totalElements">
				</el-pagination>
			</div>
		</el-tab-pane>
		<el-tab-pane label="虚拟商品" name="virtual">
			<div style="margin:2% 0;display: inline-block;">
				<span>商品标题：</span>
				<el-input style="width: 150px;" @keydown.enter.native="select" clearable placeholder="请输入商品标题"
					v-model="title"></el-input>
			</div>
			<div style="display: inline-block;">
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="select2">查询
				</el-button>
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="cleans2">重置
				</el-button>
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="refresh">刷新
				</el-button>
			</div>
			<el-table v-loading="tableDataLoading" :data="virtualData.content">
				<el-table-column prop="id" label="编号" width="80">
				</el-table-column>
				<el-table-column prop="coverImg" label="商品图片">
					<template slot-scope="scope">
						<img :src="scope.row.coverImg" alt="" width="60" height="60">
					</template>
				</el-table-column>
				<el-table-column prop="title" label="商品标题" width="200">
				</el-table-column>
				<el-table-column prop="isJiFenGoods" label="是否积分商品">
					<template slot-scope="scope">
						<span v-if="scope.row.isJiFenGoods == 1">是</span>
						<span v-else>否</span>
					</template>
				</el-table-column>
				<el-table-column prop="originalPrice" label="商品原价">
				</el-table-column>
				<el-table-column prop="price" label="商品价格">
					<template slot-scope="scope">
						<span v-if="scope.row.isJiFenGoods == 1">{{scope.row.price}} 积分</span>
						<span v-else>{{scope.row.price}}</span>
					</template>
				</el-table-column>
				<el-table-column prop="memberPrice" label="会员价格">
				</el-table-column>
				<el-table-column prop="virtualSum" label="虚拟商品总数">
				</el-table-column>
				<el-table-column prop="virtualCount" label="虚拟商品库存">
				</el-table-column>
				<el-table-column fixed="right" label="操作" width="150">
					<template slot-scope="scope">
						<el-button size="mini" type="primary" :disabled="!isAuth('merchantList:replan')"
							@click="program(scope.row)">补货
						</el-button>
						<el-button size="mini" type="primary" :disabled="!isAuth('merchantList:update')"
							@click="compile(scope.row)">编辑
						</el-button>
					</template>
				</el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
				<el-pagination @size-change="handleSizeChange2" @current-change="handleCurrentChange2"
					:page-sizes="[10, 20, 30, 40, 50]" :page-size="size" :current-page="page"
					layout="total,sizes, prev, pager, next" :total="virtualData.totalElements">
				</el-pagination>
			</div>
			<!-- 补货 -->
			<el-dialog title="补货" :visible.sync="dialogFormVisible2" center>
				<div style="margin-bottom: 10px;">
					<span style="width: 200px;display: inline-block;text-align: right;">卡密链接：</span>
					<el-input style="width:60%;" v-model="linkUrl" placeholder="请输入卡密链接"></el-input>
				</div>
				<div style="margin-bottom: 10px;">
					<span
						style="width: 200px;display: inline-block;text-align: right;position: relative;top: -110px;">卡密内容：</span>
					<el-input style="width:60%;" v-model="content" type="textarea" :rows="6" wrap="virtual"
						placeholder="请输入卡密内容"></el-input>
					<p style="font-size: 13px;color: #999999;padding-left: 200px;margin-top: 5px;">*可以批量新增，每行一个，可以智能去重
					</p>
				</div>
				<div slot="footer" class="dialog-footer">
					<el-button @click="dialogFormVisible2 = false">取 消</el-button>
					<el-button type="primary" @click="programNoticeTo()">确 定</el-button>
				</div>
			</el-dialog>
		</el-tab-pane>
		<el-tab-pane label="订单管理" name="order">
			<div style="margin:2% 0;display: inline-block;">
				<span>订单状态：</span>
				<el-select v-model="status" placeholder="请选择订单状态" style="width:150px;" @change="select1(status)">
					<el-option v-for="item in statusmain1" :key="item.value" :label="item.label" :value="item.value">
					</el-option>
				</el-select>&nbsp;&nbsp;&nbsp;&nbsp;
				<span>订单号：</span>
				<el-input style="width: 150px;" @keydown.enter.native="select" clearable placeholder="请输入订单号"
					v-model="orderNum"></el-input>&nbsp;&nbsp;&nbsp;&nbsp;
				<span>订单类型：</span>
				<el-select v-model="orderType" placeholder="请选择订单状态" style="width:150px;" @change="select1()">
					<el-option v-for="item in orderTypes" :key="item.value" :label="item.label" :value="item.value">
					</el-option>
				</el-select>&nbsp;&nbsp;&nbsp;&nbsp;
			</div>
			<div style="display: inline-block;">
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="select1">查询
				</el-button>
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="cleans1">重置
				</el-button>
			</div>
			<el-table v-loading="tableDataLoading" :data="orderData.content">
				<el-table-column fixed prop="img" label="商品图片">
					<template slot-scope="scope">
						<img :src="scope.row.img" alt="" width="60" height="60">
					</template>
				</el-table-column>
				<el-table-column prop="title" label="商品标题" width="200">
				</el-table-column>
				<el-table-column prop="type" label="商品类型">
				</el-table-column>
				<el-table-column prop="price" label="商品价格">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{scope.row.price | numFilter}}</span>
					</template>
				</el-table-column>
				<el-table-column prop="number" label="商品个数">
				</el-table-column>
				<el-table-column prop="descrition" label="订单备注">
					<template slot-scope="scope">
						<span>{{scope.row.descrition ? scope.row.descrition : '未填写'}}</span>
					</template>
				</el-table-column>
				<el-table-column label="订单类型">
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
						<span>{{scope.row.orderNum}}</span>
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
				<el-table-column prop="payMoney" label="支付金额" width="120">
					<template slot-scope="scope">
						<span style="color: #f56c6c;cursor:pointer;">{{scope.row.payMoney | numFilter}}</span>
					</template>
				</el-table-column>
				<el-table-column prop="commissionPrice" label="佣金" width="120">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{scope.row.commissionPrice | numFilter}}</span>
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
					<template slot-scope="scope">
						<span>{{scope.row.payTime ? scope.row.payTime : '未付款'}}</span>
					</template>
				</el-table-column>
				<el-table-column prop="expressTime" label="收货时间" width="160">
					<template slot-scope="scope">
						<span>{{scope.row.expressTime ? scope.row.expressTime : '未收货'}}</span>
					</template>
				</el-table-column>
				<el-table-column fixed="right" label="操作" width="100">
					<template slot-scope="scope">
						<el-button size="mini" type="primary" @click="orderDetails(scope.row.id)">详情
						</el-button>
						<el-button size="mini" type="primary" :disabled="!isAuth('merchantList:refund')" v-if="scope.row.status === 2||scope.row.status === 3||scope.row.status === 4 " style="margin: 5px 0px;"
							@click="refundClick(scope.row)">退款
						</el-button>
					</template>
				</el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
				<el-pagination @size-change="handleSizeChange3" @current-change="handleCurrentChange3"
					:page-sizes="[10, 20, 30, 40, 50]" :page-size="size" :current-page="page"
					layout="total,sizes, prev, pager, next" :total="orderData.totalElements">
				</el-pagination>
			</div>
		</el-tab-pane>
		<el-tab-pane label="待发货" name="deliver">
			<div style="margin:2% 0;display: inline-block;">
				<span>订单号：</span>
				<el-input style="width: 150px;" @keydown.enter.native="select" clearable placeholder="请输入订单号"
					v-model="orderNum"></el-input>&nbsp;&nbsp;&nbsp;&nbsp;
				<span>订单类型：</span>
				<el-select v-model="orderType" placeholder="请选择订单状态" style="width:150px;" @change="select1()">
					<el-option v-for="item in orderTypes" :key="item.value" :label="item.label" :value="item.value">
					</el-option>
				</el-select>&nbsp;&nbsp;&nbsp;&nbsp;
			</div>
			<div style="display: inline-block;">
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="select1">查询
				</el-button>
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="cleans1">重置
				</el-button>
			</div>
			<el-table v-loading="tableDataLoading" :data="orderData.content">
				<el-table-column fixed prop="img" label="商品图片">
					<template slot-scope="scope">
						<img :src="scope.row.img" alt="" width="60" height="60">
					</template>
				</el-table-column>
				<el-table-column prop="title" label="商品标题" width="200">
				</el-table-column>
				<el-table-column prop="type" label="商品类型">
				</el-table-column>
				<el-table-column prop="price" label="商品价格">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{scope.row.price | numFilter}}</span>
					</template>
				</el-table-column>
				<el-table-column prop="number" label="商品个数">
				</el-table-column>
				<el-table-column prop="descrition" label="订单备注">
					<template slot-scope="scope">
						<span>{{scope.row.descrition ? scope.row.descrition : '未填写'}}</span>
					</template>
				</el-table-column>
				<el-table-column label="订单类型">
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
						<span>{{scope.row.orderNum}}</span>
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
				<el-table-column prop="payMoney" label="支付金额" width="120">
					<template slot-scope="scope">
						<span style="color: #f56c6c;cursor:pointer;">{{scope.row.payMoney | numFilter}}</span>
					</template>
				</el-table-column>
				<el-table-column prop="commissionPrice" label="佣金" width="120">
					<template slot-scope="scope">
						<span style="color: #f56c6c;">{{scope.row.commissionPrice | numFilter}}</span>
					</template>
				</el-table-column>
				<el-table-column prop="isExpress" label="是否虚拟商品" width="150">
					<template slot-scope="scope">
						<span v-if="scope.row.isExpress == 1">否</span>
						<span v-else>是</span>
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
					<template slot-scope="scope">
						<span>{{scope.row.payTime ? scope.row.payTime : '未付款'}}</span>
					</template>
				</el-table-column>
				<el-table-column prop="finishTime" label="收货时间" width="160">
					<template slot-scope="scope">
						<span>{{scope.row.finishTime ? scope.row.finishTime : '未收货'}}</span>
					</template>
				</el-table-column>
				<el-table-column fixed="right" label="操作" width="150">
					<template slot-scope="scope">
						<el-button size="mini" type="primary" :disabled="!isAuth('merchantList:deliver')"
							@click="colonelClick(scope.row)">发货
						</el-button>
						<el-button size="mini" type="primary" @click="orderDetails(scope.row.id)">详情
						</el-button>
						<el-button size="mini" type="primary" :disabled="!isAuth('merchantList:refund')" v-if="scope.row.status === 2||scope.row.status === 3||scope.row.status === 4 " style="margin: 5px 0px;"
							@click="refundClick(scope.row)">退款
						</el-button>
					</template>
				</el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
				<el-pagination @size-change="handleSizeChange3" @current-change="handleCurrentChange3"
					:page-sizes="[10, 20, 30, 40, 50]" :page-size="size" :current-page="page"
					layout="total,sizes, prev, pager, next" :total="orderData.totalElements">
				</el-pagination>
			</div>
			<!-- 发货弹框 -->
			<el-dialog title="发货" :visible.sync="dialogFormVisible3" center>
				<div style="margin-bottom: 10px;">
					<span style="width: 200px;display: inline-block;text-align: right;">快递公司：</span>
					<el-input style="width:50%;" v-model="expressName" placeholder="请输入快递公司"></el-input>
				</div>
				<div style="margin-bottom: 10px;">
					<span style="width: 200px;display: inline-block;text-align: right;">快递单号：</span>
					<el-input style="width: 50%;" v-model="expressNumber" placeholder="请输入快递单号"></el-input>
				</div>
				<div slot="footer" class="dialog-footer">
					<el-button @click="dialogFormVisible3 = false">取 消</el-button>
					<el-button type="primary" @click="deliverNoticeTo()">确 定</el-button>
				</div>
			</el-dialog>
		</el-tab-pane>
		<el-tab-pane label="待退款" name="refund">
			<div style="margin:2% 0;display: inline-block;">
				<span>订单号：</span>
				<el-input style="width: 150px;" @keydown.enter.native="select" clearable placeholder="请输入订单号"
					v-model="orderNum"></el-input>&nbsp;&nbsp;&nbsp;&nbsp;
				<span>订单类型：</span>
				<el-select v-model="orderType" placeholder="请选择订单状态" style="width:150px;" @change="select1()">
					<el-option v-for="item in orderTypes" :key="item.value" :label="item.label" :value="item.value">
					</el-option>
				</el-select>&nbsp;&nbsp;&nbsp;&nbsp;
			</div>
			<div style="display: inline-block;">
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="select1">查询
				</el-button>
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="cleans1">重置
				</el-button>
			</div>
			<el-table v-loading="tableDataLoading" :data="orderData.content">
				<el-table-column fixed prop="img" label="商品图片">
					<template slot-scope="scope">
						<img :src="scope.row.img" alt="" width="60" height="60">
					</template>
				</el-table-column>
				<el-table-column prop="title" label="商品标题" width="200">
				</el-table-column>
				<el-table-column prop="number" label="商品个数">
				</el-table-column>
				<el-table-column prop="refund" label="退款原因">
				</el-table-column>
				<el-table-column prop="descrition" label="订单备注">
					<template slot-scope="scope">
						<span>{{scope.row.descrition ? scope.row.descrition : '未填写'}}</span>
					</template>
				</el-table-column>
				<el-table-column label="订单类型">
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
						<span>{{scope.row.orderNum}}</span>
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
						<span style="color: #f56c6c;">{{scope.row.commissionPrice | numFilter}}</span>
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
					<template slot-scope="scope">
						<span>{{scope.row.payTime ? scope.row.payTime : '未付款'}}</span>
					</template>
				</el-table-column>
				<el-table-column fixed="right" label="操作" width="280">
					<template slot-scope="scope">
						<el-button size="mini" type="primary" :disabled="!isAuth('merchantList:refund')"
							@click="refundClick(scope.row)">确定退款
						</el-button>
						<el-button size="mini" type="primary" :disabled="!isAuth('merchantList:refuse')"
							@click="refuseClick(scope.row)">拒绝退款
						</el-button>
						<el-button size="mini" type="primary" @click="orderDetails(scope.row.id)">详情
						</el-button>
					</template>
				</el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
				<el-pagination @size-change="handleSizeChange3" @current-change="handleCurrentChange3"
					:page-sizes="[10, 20, 30, 40, 50]" :page-size="size" :current-page="page"
					layout="total,sizes, prev, pager, next" :total="orderData.totalElements">
				</el-pagination>
			</div>
			<!-- 拒绝退款 -->
			<el-dialog title="拒绝退款" :visible.sync="dialogFormVisible4" center>
				<div style="margin-bottom: 10px;">
					<span
						style="width: 200px;display: inline-block;text-align: right;position: relative;top: -65px;">拒绝退款理由：</span>
					<el-input style="width:50%;" type="textarea" rows="4" v-model="refusedRefund"
						placeholder="请输入拒绝退款理由"></el-input>
				</div>
				<div slot="footer" class="dialog-footer">
					<el-button @click="dialogFormVisible4 = false">取 消</el-button>
					<el-button type="primary" @click="refuseNoticeTo()">确 定</el-button>
				</div>
			</el-dialog>
		</el-tab-pane>
		<el-tab-pane label="待核销订单" name="cancel">
			<div style="margin:2% 0;display: inline-block;">
				<span>订单号：</span>
				<el-input style="width: 150px;" @keydown.enter.native="select4" clearable placeholder="请输入订单号"
					v-model="orderNum"></el-input>&nbsp;&nbsp;&nbsp;&nbsp;
				<span>手机号：</span>
				<el-input style="width: 150px;" @keydown.enter.native="select4" clearable placeholder="请输入手机号"
					v-model="mobile"></el-input>&nbsp;&nbsp;&nbsp;&nbsp;
				<span>订单类型：</span>
				<el-select v-model="orderType" placeholder="请选择订单状态" style="width:150px;" @change="select()">
					<el-option v-for="item in orderTypes" :key="item.value" :label="item.label" :value="item.value">
					</el-option>
				</el-select>&nbsp;&nbsp;&nbsp;&nbsp;
			</div>
			<div style="display: inline-block;">
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="select4">查询
				</el-button>
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="cleans4">重置
				</el-button>
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="refresh4">刷新
				</el-button>
				<el-button style='margin-left:15px;' :disabled="checkBoxData.length <= 0 " size="mini" type="primary"
					icon="document" @click="volume()">批量核销</el-button>
			</div>
			<el-table @selection-change="changeFun" v-loading="tableDataLoading" :data="tableData2.content">
				<el-table-column type="selection" fixed>
				</el-table-column>
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
				<el-table-column label="订单类型">
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
						<span>{{scope.row.orderNum}}</span>
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
						<span style="color: #f56c6c;">{{scope.row.commissionPrice | numFilter}}</span>
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
						<span style="color: #3E8EF7;" v-if="scope.row.status === 3 ">待核销</span>
						<span v-if="scope.row.status === 4 ">已核销</span>
					</template>
				</el-table-column>
				<el-table-column prop="createAt" label="创建时间" width="160">
				</el-table-column>
				<el-table-column prop="payTime" label="付款时间" width="160">
					<template slot-scope="scope">
						<span>{{scope.row.payTime ? scope.row.payTime : '未付款'}}</span>
					</template>
				</el-table-column>
				<el-table-column fixed="right" label="操作" width="120">
					<template slot-scope="scope">
						<el-button size="mini" type="primary" :disabled="!isAuth('merchantList:cancel')"
							@click="volumeOne(scope.row)">核销
						</el-button>
					</template>
				</el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
				<el-pagination @size-change="handleSizeChange4" @current-change="handleCurrentChange4"
					:page-sizes="[10, 20, 30, 40, 50]" :page-size="size" :current-page="page"
					layout="total,sizes, prev, pager, next" :total="tableData2.totalElements">
				</el-pagination>
			</div>
		</el-tab-pane>
		<el-tab-pane label="已核销订单" name="yetcancel">
			<div style="margin:2% 0;display: inline-block;">
				<span>订单号：</span>
				<el-input style="width: 150px;" @keydown.enter.native="select4" clearable placeholder="请输入订单号"
					v-model="orderNum"></el-input>&nbsp;&nbsp;&nbsp;&nbsp;
				<span>手机号：</span>
				<el-input style="width: 150px;" @keydown.enter.native="select4" clearable placeholder="请输入手机号"
					v-model="mobile"></el-input>&nbsp;&nbsp;&nbsp;&nbsp;
				<span>订单类型：</span>
				<el-select v-model="orderType" placeholder="请选择订单状态" style="width:150px;" @change="select()">
					<el-option v-for="item in orderTypes" :key="item.value" :label="item.label" :value="item.value">
					</el-option>
				</el-select>&nbsp;&nbsp;&nbsp;&nbsp;
			</div>
			<div style="display: inline-block;">
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="select4">查询
				</el-button>
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="cleans4">重置
				</el-button>
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="refresh4">刷新
				</el-button>
			</div>
			<el-table v-loading="tableDataLoading" :data="tableData2.content">
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
				<el-table-column label="订单类型">
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
						<span>{{scope.row.orderNum}}</span>
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
						<span style="color: #f56c6c;">{{scope.row.commissionPrice | numFilter}}</span>
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
						<span style="color: #3E8EF7;" v-if="scope.row.status === 3 ">待核销</span>
						<span v-if="scope.row.status === 4 ">已核销</span>
					</template>
				</el-table-column>
				<el-table-column prop="createAt" label="创建时间" width="160">
				</el-table-column>
				<el-table-column prop="payTime" label="付款时间" width="160">
					<template slot-scope="scope">
						<span>{{scope.row.payTime ? scope.row.payTime : '未付款'}}</span>
					</template>
				</el-table-column>
				<el-table-column prop="finishTime" label="核销时间" width="160">
					<template slot-scope="scope">
						<span>{{scope.row.finishTime ? scope.row.finishTime : '未收货'}}</span>
					</template>
				</el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
				<el-pagination @size-change="handleSizeChange4" @current-change="handleCurrentChange4"
					:page-sizes="[10, 20, 30, 40, 50]" :page-size="size" :current-page="page"
					layout="total,sizes, prev, pager, next" :total="tableData2.totalElements">
				</el-pagination>
			</div>
		</el-tab-pane>
		<el-tab-pane label="优惠券制作" name="coupon">
			<div style="display: inline-block;float: right;">
				<el-button style='margin-left:15px;' :disabled="!isAuth('merchantList:add')" size="mini" type="primary"
					icon="document" @click="addCoupon">添加优惠券</el-button>
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="refresh1">刷新
				</el-button>
			</div>
			<el-table v-loading="tableDataLoading" :data="couponData.content">
				<el-table-column prop="couponName" label="优惠券名称" width="150">
				</el-table-column>
				<el-table-column prop="type" label="优惠券类型" width="100">
					<template slot-scope="scope">
						<span v-if="scope.row.type==1">通用券</span>
						<span style="color: #3E8EF7;" v-if="scope.row.type==2">商品券</span>
					</template>
				</el-table-column>
				<el-table-column prop="lessMoney" label="优惠券面值" width="100">
				</el-table-column>
				<el-table-column prop="minMoney" label="优惠券最低消费" width="150">
				</el-table-column>
				<el-table-column prop="validDay" label="优惠券有效期限(天)" width="150">
				</el-table-column>
				<el-table-column prop="sort" label="排序">
				</el-table-column>
				<el-table-column label="状态">
					<template slot-scope="scope">
						<span style="color: #3E8EF7;" v-if="scope.row.status == 1">开启</span>
						<span v-if="scope.row.status == 2">关闭</span>
					</template>
				</el-table-column>
				<el-table-column prop="createTime" label="创建时间" width="150">
				</el-table-column>
				<el-table-column label="操作" width="150">
					<template slot-scope="scope">
						<el-button size="mini" type="primary" :disabled="!isAuth('merchantList:issue')"
							@click="issue(scope.$index,scope.row)">发布
						</el-button>
						<el-button size="mini" type="primary" :disabled="!isAuth('merchantList:update')"
							@click="couponUpdates(scope.$index,scope.row)">修改
						</el-button>
					</template>
				</el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
				<el-pagination @size-change="handleSizeChange5" @current-change="handleCurrentChange5"
					:page-sizes="[10, 20, 30, 40, 50]" :page-size="size" :current-page="page"
					layout="total,sizes, prev, pager, next" :total="couponData.totalElements">
				</el-pagination>
			</div>
			<!-- 添加优惠券-->
			<el-dialog title="添加优惠券" :visible.sync="dialogFormVisible5" center>
				<div style="margin-bottom: 10px;">
					<span style="width: 200px;display: inline-block;text-align: right;">优惠券类型：</span>
					<el-radio-group v-model="type">
						<!-- <el-radio :label="1">通用券</el-radio> -->
						<el-radio :label="2">商品券</el-radio>
					</el-radio-group>
				</div>
				<div v-if="type==2">
					<span style="width: 200px;display: inline-block;text-align: right;">选择商品：</span>
					<div style="position: relative;left: 30%;top: -22px; width:148px;height:148px;font-size: 28px;background-color: #fbfdff; border: 1px dashed #c0ccda;border-radius: 6px;text-align: center;line-height: 143px;cursor: pointer;"
						@click="couponshopClick()">
						<img v-if="goodsImages" :src="goodsImages" class="avatar" style="width: 138px;height: 138px;" />
						<i v-else class="el-icon-plus avatar-uploader-icon" style="font-size: 28px;color: #8c939d"></i>
					</div>
				</div>
				<div style="margin-bottom: 10px;">
					<span style="width: 200px;display: inline-block;text-align: right;">优惠券名称：</span>
					<el-input style="width:50%;" v-model="couponName" placeholder="请输入优惠券名称"></el-input>
				</div>
				<div style="margin-bottom: 10px;">
					<span style="width: 200px;display: inline-block;text-align: right;">优惠券面值：</span>
					<el-input style="width:50%;" v-model="lessMoney" type="number" :min="0" placeholder="请输入优惠券面值">
					</el-input>
				</div>
				<div style="margin-bottom: 10px;">
					<span style="width: 200px;display: inline-block;text-align: right;">优惠券最低消费：</span>
					<el-input style="width:50%;" v-model="minMoney" type="number" :min="0" placeholder="请输入优惠券最低消费">
					</el-input>
				</div>
				<div style="margin-bottom: 10px;">
					<span style="width: 200px;display: inline-block;text-align: right;">优惠券有效期限(天)：</span>
					<el-input style="width:50%;" v-model="validDay" type="number" :min="1" placeholder="请输入优惠券最低消费">
					</el-input>
				</div>
				<div style="margin-bottom: 10px;">
					<span style="width: 200px;display: inline-block;text-align: right;">排序：</span>
					<el-input style="width:50%;" v-model="sort" type="number" :min="0" placeholder="请输入排序号"></el-input>
				</div>
				<div style="margin-bottom: 10px;">
					<span style="width: 200px;display: inline-block;text-align: right;">状态：</span>
					<el-radio-group v-model="status">
						<el-radio :label="1">开启</el-radio>
						<el-radio :label="2">关闭</el-radio>
					</el-radio-group>
				</div>
				<div slot="footer" class="dialog-footer">
					<el-button @click="dialogFormVisible5 = false">取 消</el-button>
					<el-button type="primary" @click="couponNoticeTo()">确 定</el-button>
				</div>
			</el-dialog>
			<!-- 修改优惠券 -->
			<el-dialog title="修改优惠券" :visible.sync="dialogFormVisible6" center>
				<el-form :model="couponform">
					<el-form-item label="优惠券类型：" :label-width="formLabelWidth">
						<el-radio-group v-model="couponform.type">
							<el-radio :label="2">商品券</el-radio>
						</el-radio-group>
					</el-form-item>
					<el-form-item label="选择商品：" :label-width="formLabelWidth" v-if="form.type==2">
						<div style=" width:148px;height:148px;font-size: 28px;background-color: #fbfdff; border: 1px dashed #c0ccda;border-radius: 6px;text-align: center;line-height: 148px;cursor: pointer;"
							@click="shopClick()">
							<img v-if="couponform.goodsImages" :src="couponform.goodsImages" class="avatar"
								style="width: 140px;height: 140px;" />
							<i v-else class="el-icon-plus avatar-uploader-icon"
								style="font-size: 28px;color: #8c939d"></i>
						</div>
					</el-form-item>
					<el-form-item label="优惠券名称：" :label-width="formLabelWidth">
						<el-input v-model="couponform.couponName" style="width:65%;" placeholder="请输入优惠券名称"></el-input>
					</el-form-item>
					<el-form-item label="优惠券面值：" :label-width="formLabelWidth">
						<el-input v-model="couponform.lessMoney" style="width:65%;" placeholder="请输入优惠券面值"></el-input>
					</el-form-item>
					<el-form-item label="优惠券最低消费：" :label-width="formLabelWidth">
						<el-input v-model="couponform.minMoney" style="width:65%;" placeholder="请输入优惠券最低消费"></el-input>
					</el-form-item>
					<el-form-item label="优惠券有效期限(天)：" :label-width="formLabelWidth">
						<el-input v-model="couponform.validDay" style="width:65%;" placeholder="请输入优惠券有效期限(天)">
						</el-input>
					</el-form-item>
					<el-form-item label="排序：" :label-width="formLabelWidth">
						<el-input v-model="couponform.sort" style="width:65%;" placeholder="请输入排序号"></el-input>
					</el-form-item>
					<el-form-item label="状态：" :label-width="formLabelWidth">
						<el-radio-group v-model="couponform.status">
							<el-radio :label="1">开启</el-radio>
							<el-radio :label="2">关闭</el-radio>
						</el-radio-group>
					</el-form-item>
				</el-form>
				<div slot="footer" class="dialog-footer">
					<el-button @click="dialogFormVisible6 = false">取 消</el-button>
					<el-button type="primary" @click="couponNoticeTo2()">确 定</el-button>
				</div>
			</el-dialog>
			<!-- 发布优惠券-->
			<el-dialog title="发布优惠券" :visible.sync="dialogFormVisible8" center>
				<div style="margin-bottom: 10px;">
					<span style="width: 200px;display: inline-block;text-align: right;">优惠券ID：</span>
					<el-input style="width:50%;" v-model="couponId" disabled></el-input>
				</div>
				<div style="margin-bottom: 10px;">
					<span style="width: 200px;display: inline-block;text-align: right;">优惠券名称：</span>
					<el-input style="width:50%;" v-model="couponName1" disabled></el-input>
				</div>
				<div style="margin-bottom: 10px;">
					<span style="width: 200px;display: inline-block;text-align: right;">领取开启时间：</span>
					<el-date-picker style="width:200px;" v-model="startTime" align="right" type="datetime"
						format="yyyy-MM-dd HH:mm:ss" value-format="yyyy-MM-dd HH:mm:ss" placeholder="选择开始时间">
					</el-date-picker>
				</div>
				<div style="margin-bottom: 10px;">
					<span style="width: 200px;display: inline-block;text-align: right;">券领结束时间：</span>
					<el-date-picker style="width:200px;" v-model="endTime" align="right" type="datetime"
						format="yyyy-MM-dd HH:mm:ss" value-format="yyyy-MM-dd HH:mm:ss" placeholder="选择结束时间">
					</el-date-picker>
				</div>
				<div style="margin-bottom: 10px;">
					<span style="width: 200px;display: inline-block;text-align: right;">发布数量：</span>
					<el-input style="width:50%;" v-model="issueNumber" type="number" :min="0" placeholder="请输入发布数量">
					</el-input>
				</div>
				<div style="margin-bottom: 10px;">
					<span style="width: 200px;display: inline-block;text-align: right;">是否限量：</span>
					<el-radio-group v-model="isLimit">
						<el-radio :label="1">限量</el-radio>
						<el-radio :label="2">不限量</el-radio>
					</el-radio-group>
				</div>
				<div style="margin-bottom: 10px;">
					<span style="width: 200px;display: inline-block;text-align: right;">状态：</span>
					<el-radio-group v-model="status">
						<el-radio :label="1">开启</el-radio>
						<el-radio :label="2">关闭</el-radio>
					</el-radio-group>
				</div>
				<div slot="footer" class="dialog-footer">
					<el-button @click="dialogFormVisible8 = false">取 消</el-button>
					<el-button type="primary" @click="issueNoticeTo()">确 定</el-button>
				</div>
			</el-dialog>
			<!-- 选择商品弹框 -->
			<el-dialog title="商品列表" customClass="customWidth" :visible.sync="dialogFormVisible7" center>
				<div>
					<div style="margin:2% 0;display: inline-block;">
						<span>商品标题：</span>
						<el-input style="width: 150px;" @keydown.enter.native="select" clearable placeholder="请输入商品标题"
							v-model="title"></el-input>&nbsp;&nbsp;&nbsp;&nbsp;
					</div>
					<div style="display: inline-block;">
						<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="select">
							查询</el-button>
						<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="cleans">
							重置</el-button>
					</div>
					<el-table v-loading="tableDataLoading" :data="coushopData.content">
						<el-table-column prop="id" label="编号" width="80">
						</el-table-column>
						<el-table-column prop="coverImg" label="商品图片">
							<template slot-scope="scope">
								<img :src="scope.row.coverImg" alt="" width="60" height="60">
							</template>
						</el-table-column>
						<el-table-column prop="title" label="商品标题" width="200">
						</el-table-column>
						<el-table-column prop="originalPrice" label="商品原价">
						</el-table-column>
						<el-table-column prop="price" label="商品价格">
						</el-table-column>
						<el-table-column prop="memberPrice" label="会员价格">
						</el-table-column>
						<el-table-column label="操作" width="150">
							<template slot-scope="scope">
								<el-button size="mini" type="primary" @click="selectClick(scope.$index,scope.row)">选择
								</el-button>
							</template>
						</el-table-column>
					</el-table>
					<div style="text-align: center;margin-top: 10px;">
						<el-pagination @size-change="handleSizeChange8" @current-change="handleCurrentChange8"
							:page-sizes="[10, 20, 30, 40, 50]" :page-size="size" :current-page="page"
							layout="total,sizes, prev, pager, next" :total="coushopData.totalElements">
						</el-pagination>
					</div>
				</div>
			</el-dialog>
		</el-tab-pane>
		<el-tab-pane label="已发布优惠券" name="issue">
			<el-table v-loading="tableDataLoading" :data="issueData.content">
				<el-table-column prop="couponIssueId" label="id" width="80">
				</el-table-column>
				<el-table-column prop="type" label="优惠券类型">
					<template slot-scope="scope">
						<span v-if="scope.row.type==1">通用券</span>
						<span style="color: #3E8EF7;" v-if="scope.row.type==2">商品券</span>
					</template>
				</el-table-column>
				<el-table-column prop="couponName" label="优惠券名称">
				</el-table-column>
				<el-table-column label="领取日期" width="150">
					<template slot-scope="scope">
						<div>{{scope.row.createTime}}</div>
						<div>{{scope.row.endTime}}</div>
					</template>
				</el-table-column>
				<el-table-column label="发布数量">
					<template slot-scope="scope">
						<div>发布:{{scope.row.issueNumber}}</div>
						<div>剩余:{{scope.row.remainNumber}}</div>
					</template>
				</el-table-column>
				<el-table-column label="状态">
					<template slot-scope="scope">
						<span style="color: #3E8EF7;" v-if="scope.row.status == 1">开启</span>
						<span v-if="scope.row.status == 2">关闭</span>
					</template>
				</el-table-column>
				<el-table-column prop="createTime" label="创建时间" width="150">
				</el-table-column>
				<el-table-column label="操作" width="150">
					<template slot-scope="scope">
						<el-button size="mini" type="primary" :disabled="!isAuth('merchantList:update')"
							@click="amendcoupon(scope.$index,scope.row)">修改
						</el-button>
						<el-button size="mini" type="danger" :disabled="!isAuth('merchantList:delete')"
							@click="shopdeletes(scope.row)">删除
						</el-button>
					</template>
				</el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
				<el-pagination @size-change="handleSizeChange6" @current-change="handleCurrentChange6"
					:page-sizes="[10, 20, 30, 40, 50]" :page-size="size" :current-page="page"
					layout="total,sizes, prev, pager, next" :total="issueData.totalElements">
				</el-pagination>
			</div>
			<!-- 修改-->
			<el-dialog title="修改" :visible.sync="dialogFormVisible9" center>
				<el-form :model="formissue">
					<el-form-item label="状态：" :label-width="formLabelWidth">
						<el-radio-group v-model="formissue.status">
							<el-radio :label="1">开启</el-radio>
							<el-radio :label="2">关闭</el-radio>
						</el-radio-group>
					</el-form-item>
					<el-form-item label="限量：" :label-width="formLabelWidth">
						<el-radio-group v-model="formissue.isLimit">
							<el-radio :label="1">限量</el-radio>
							<el-radio :label="2">不限量</el-radio>
						</el-radio-group>
					</el-form-item>
				</el-form>
				<div slot="footer" class="dialog-footer">
					<el-button @click="dialogFormVisible9 = false">取 消</el-button>
					<el-button type="primary" @click="amendNoticeTo()">确 定</el-button>
				</div>
			</el-dialog>
		</el-tab-pane>
		<el-tab-pane label="优惠券领取记录" name="record">
			<div style="display: inline-block;">
				<div style="margin:2% 0;display: inline-block;">
					<span>优惠券名称：</span>
					<el-input style="width:200px;" @keydown.enter.native="select" clearable placeholder="请输入优惠券名称"
						v-model="couponName"></el-input>
				</div>
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="select3">查询
				</el-button>
				<el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="cleans3">重置
				</el-button>
				<el-button style='margin-left:15px;' :disabled="!isAuth('shopAdmin:add')" size="mini" type="primary"
					icon="document" @click="refresh3">刷新</el-button>
			</div>
			<el-table v-loading="tableDataLoading" :data="recordData.content">
				<el-table-column prop="couponName" label="优惠券名称" width="150">
				</el-table-column>
				<el-table-column prop="nickName" label="所属用户">
				</el-table-column>
				<el-table-column prop="lessMoney" label="优惠券面值">
				</el-table-column>
				<el-table-column prop="minMoney" label="优惠券最低消费">
				</el-table-column>
				<el-table-column prop="failureTime" label="优惠券失效时间" width="150">
				</el-table-column>
				<el-table-column label="状态">
					<template slot-scope="scope">
						<span style="color: #3E8EF7;" v-if="scope.row.status == 1">未使用</span>
						<span v-if="scope.row.status == 2">已使用</span>
						<span v-if="scope.row.status == 3">已过期</span>
					</template>
				</el-table-column>
				<el-table-column prop="createTime" label="创建时间" width="150">
				</el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
				<el-pagination @size-change="handleSizeChange7" @current-change="handleCurrentChange7"
					:page-sizes="[10, 20, 30, 40, 50]" :page-size="size" :current-page="page"
					layout="total,sizes, prev, pager, next" :total="recordData.totalElements">
				</el-pagination>
			</div>
		</el-tab-pane>
		<el-tab-pane label="店铺粉丝" name="fans">
			<el-table v-loading="tableDataLoading" :data="fansData.content">
				<el-table-column prop="id" label="编号" width="80">
				</el-table-column>
				<el-table-column prop="nickName" label="粉丝用户">
					<template slot-scope="scope">
					  <span style="color: #4f9dec;cursor: pointer;"
					    @click="updates(scope.row)" v-if="scope.row.userInfo">{{ scope.row.userInfo.nickName ? scope.row.userInfo.nickName : '未绑定' }}</span>
					</template>
				</el-table-column>
				<el-table-column prop="image_url" label="头像">
					<template slot-scope="scope">
					  <img  v-if="scope.row.userInfo" :src="scope.row.userInfo.image_url" style="width: 60px;height: 60px;"/>
					</template>
				</el-table-column>
				<el-table-column prop="createTime" label="创建时间" width="150">
				</el-table-column>
			</el-table>
			<div style="text-align: center;margin-top: 10px;">
				<el-pagination @size-change="handleSizeChangeFs" @current-change="handleCurrentChangeFs"
					:page-sizes="[10, 20, 30, 40, 50]" :page-size="size" :current-page="page"
					layout="total,sizes, prev, pager, next" :total="fansData.totalElements">
				</el-pagination>
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
				id: '',
				title: '',
				mobile: '',
				money: '',
				outMoney: '',
				orderNum: '',
				status: 0,
				isshow: true,
				isbtn: true,
				sumincome: '',
				totalnum: 0,
				income: '',
				refusedRefund: '',
				wxincome: '',
				zfbincome: '',
				yueicome: '',
				ordersum0: '',
				ordersum1: '',
				ordersum2: '',
				expressName: '',
				expressNumber: '',
				logo: '',
				storeName: '',
				img: '',
				imgs: [],
				checkBoxData: [], //多选框选择的值
				virtualData: [],
				sale: '',
				storeId: '',
				isshow: true,
				isbtn: true,
				potost: '',
				describes: '',
				merchantId: '',
				createTime: '',
				shipAddress: '',
				refundAddress: '',
				refundContact: '',
				refundMobile: '',
				shopData: [],
				classDatas: [],
				orderData: [],
				typeDatas: [],
				tableData2: '',
				name: '',
				img: '',
				logo: '',
				type: 2,
				coverImg: '',
				couponName1: '',
				couponId: '',
				startTime: '',
				endTime: '',
				goodsImages: '',
				couponName: '',
				lessMoney: 0,
				minMoney: 0,
				validDay: 1,
				sort: 0,
				issueNumber: 1,
				isLimit: 1,
				goodsIds: '',
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
				couponform: {
					couponId: '',
					type: '',
					couponName: '',
					lessMoney: '',
					goodsIds: '',
					goodsImages: '',
					minMoney: '',
					validDay: '',
					sort: '',
					status: '',
					createTime: '',
					storeId: '',
					merchantId: '',
					storeName: ''
				},
				marginStatus: '',
				marginType: '',
				endTime: '',
				issueNumber: 0,
				parentIdnum: '',
				title: '',
				title1: '',
				linkUrl: '',
				goodsId: '',
				coverImg: '',
				content: '',
				type: '',
				isJiFenGoods: 0,
				isExpress: 2,
				recordData: [],
				form: {
					merchantId: '',
					storeId: '',
					name: '',
					img: '',
					parentId: '',
					storeTypeId: ''
				},
				form1: {
					money: '',
					zhiFuBaoAccount: '',
					zhiFuBaoName: ''
				},
				info: {
					stockDate: this.getNowTime(), //日期
				},
				way: 3,
				ways: [{
					value: 3,
					label: '按天查询'
				}, {
					value: 2,
					label: '按月查询'
				}, {
					value: 1,
					label: '按年查询'
				}],
				score: '',
				typeId: '',
				value: [],
				status: 0,
				statusmain: [{
						value: 0,
						label: '全部'
					},
					{
						value: 1,
						label: '上架'
					},
					{
						value: 2,
						label: '下架'
					}
				],
				statusmain1: [{
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
					}
				],
				formissue: {
					couponId: '',
					couponIssueId: '',
					couponName: '',
					createTime: '',
					endTime: '',
					isLimit: '',
					issueNumber: '',
					remainNumber: '',
					startTime: '',
					status: '',
					type: '',
					createTime: '',
					storeId: '',
					merchantId: '',
					storeName: ''
				},
				openValue: 1,
				closeValue: 2,
				withdrawData: [],
				formLabelWidth: '200px',
				activeName: 'first',
				tableDataLoading: false,
				dialogFormVisible: false,
				dialogFormVisible1: false,
				dialogFormVisible2: false,
				dialogFormVisible3: false,
				dialogFormVisible4: false,
				dialogFormVisible5: false,
				dialogFormVisible6: false,
				dialogFormVisible7: false,
				dialogFormVisible8: false,
				dialogFormVisible9: false,
				dialogFormVisibleC:false,
				dialogFormVisibleC1:false,
				tableData: {},
				tableData2: {},
				couponData: [],
				coushopData: [],
				issueData: [],
				fansData:{},
				cationDatas:{},
				cationform: {
					id: '',
					ruleName: '',
					ruleValue: [],
					createTime: ''

				},
				releobject: {
					value: '',
					detail: '',
				},
				inputVisible: false,
				inputValues: {
					inputValue0: '',
					inputValue1: '',
					inputValue2: '',
					inputValue3: '',
					inputValue4: '',
					inputValue5: ''
				},
				ruleName: '',
				releobject: {
					value: '',
					detail: '',
				},
				ruleValue: [],
				dynamicTags: [],
				tableDataYe:{},
				cationisshow: true,
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
			// 上下架
			change(id) {
				this.$http({
					url: this.$http.adornUrl2('/goods/updateStatus'),
					method: 'get',
					params: this.$http.adornParams({
						'goodsId': id
					})
				}).then(({
					data
				}) => {
					this.$message({
						message: '操作成功',
						type: 'success',
						duration: 1500,
					})
				})
			},
			// 多选
			changeFun(val) {
				this.checkBoxData = val;
			},
			// 刷新
			refresh() {
				this.virtualSelect()
			},
			//搜索
			select2() {
				this.virtualSelect()
			},
			// 重置
			cleans2() {
				this.title = ''
				this.status = 0
				this.virtualSelect()
			},
			handleSizeChange(val) {
				this.size = val
				this.withdrawSelect()
			},
			handleCurrentChange(val) {
				this.page = val
				this.withdrawSelect()
			},
			handleSizeChange1(val) {
				this.size = val
				this.shopSelect()
			},
			handleCurrentChange1(val) {
				this.page = val
				this.shopSelect()
			},
			handleSizeChange2(val) {
				this.size = val
				this.virtualSelect()
			},
			handleCurrentChange2(val) {
				this.page = val
				this.virtualSelect()
			},
			handleSizeChange3(val) {
				this.size = val
				this.orderSelect()
			},
			handleCurrentChange3(val) {
				this.page = val
				this.orderSelect()
			},
			handleSizeChange4(val) {
				this.size = val
				this.dataSelect2()
			},
			handleCurrentChange4(val) {
				this.page = val
				this.dataSelect2()
			},
			handleSizeChange5(val) {
				this.size = val
				this.couponSelect()
			},
			handleCurrentChange5(val) {
				this.page = val
				this.couponSelect()
			},
			handleSizeChange6(val) {
				this.size = val
				this.published()
			},
			handleCurrentChange6(val) {
				this.page = val
				this.published()
			},
			handleSizeChange7(val) {
				this.size = val
				this.record()
			},
			handleCurrentChange7(val) {
				this.page = val
				this.record()
			},
			handleSizeChange8(val) {
				this.size = val
				this.coushopSelect()
			},
			handleCurrentChange8(val) {
				this.page = val
				this.coushopSelect()
			},
			handleSizeChangeFs(val) {
				this.size = val
				this.fansSelect()
			},
			handleCurrentChangeFs(val) {
				this.page = val
				this.fansSelect()
			},
			handleSizeChangeYe(val) {
				this.size = val
				this.dataSelectYe()
			},
			handleCurrentChangeYe(val) {
				this.page = val
				this.dataSelectYe()
			},
			handleChange(value) {
				this.value.forEach(element => {
					this.typeId = element;
				});
			},
			// 刷新
			refresh1() {
				this.couponSelect()
			},
			//搜索
			select() {
				this.shopSelect()
			},
			// 重置
			cleans() {
				this.title = ''
				this.status = 0
				this.typeId = ''
				this.value = []
				this.shopSelect()
			},
			//搜索
			select1() {
				this.page = 1
				this.size = 10
				this.orderSelect()
			},
			// 重置
			cleans1() {
				this.orderType = 0
				this.orderNum = ''
				this.status = 0
				this.orderSelect()
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
			//发布商品
			addShop() {
				let storeId = this.$route.query.storeId
				let merchantId = this.$route.query.merchantId
				this.$router.push({
					path: '/shopPublish2',
					query: {
						storeId: storeId,
						merchantId: merchantId
					}
				})
			},
			// 修改商品
			shopUpdates(index, rows) {
				let storeId = this.$route.query.storeId
				let merchantId = this.$route.query.merchantId
				this.$router.push({
					path: '/shopAmend2',
					query: {
						storeId: storeId,
						merchantId: merchantId,
						id: rows.id
					}
				});
			},
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
			// 日期选择
			animeDat(value) {
				let vanumber = value
				if (vanumber === 3) {
					this.moneySelect()
					this.orderSelect()
				}
				if (vanumber === 2) {
					this.moneySelect()
					this.orderSelect()
				}
				if (vanumber === 1) {
					this.moneySelect()
					this.orderSelect()
				}
			},
			timeDate() {
				this.moneySelect()
				this.orderSelect()
			},
			// 商品评价跳转
			discuss(index, rows) {
				this.$router.push({
					path: '/discuss',
					query: {
						goodsId: rows.id
					}
				});
			},
			handleClick(tab, event) {
				if (tab._props.label == '店铺信息') {
					this.dataSelect()
				}
				if (tab._props.label == '店铺收入') {
					this.moneySelect()
					this.orderSelect2()
				}
				if (tab._props.label == '提现记录') {
					this.withdrawSelect()
				}
				if (tab._props.label == '商品分类') {
					this.classifySelect()
				}
				if (tab._props.label == '商品规格') {
					this.cationSelect()
				}
				if (tab._props.label == '商品管理') {
					this.status = 0
					this.shopSelect()
					this.classSelect()
				}
				if (tab._props.label == '虚拟商品') {
					this.virtualSelect()
				}
				if (tab._props.label == '订单管理') {
					this.orderType = 0
					this.status = 0
					this.orderSelect()
				}
				if (tab._props.label == '待发货') {
					this.orderType = 0
					this.status = 2
					this.orderSelect()
				}
				if (tab._props.label == '待退款') {
					this.orderType = 0
					this.status = 6
					this.orderSelect()
				}
				if (tab._props.label == '待核销订单') {
					this.orderNum = ''
					this.mobile = ''
					this.size = 10
					this.page = 1
					this.status = 3
					this.isExpress = 3
					this.dataSelect2()
				}
				if (tab._props.label == '已核销订单') {
					this.orderNum = ''
					this.mobile = ''
					this.size = 10
					this.page = 1
					this.status = 4
					this.isExpress = 3
					this.dataSelect2()
				}
				if (tab._props.label == '优惠券制作') {
					this.couponSelect()
				}
				if (tab._props.label == '已发布优惠券') {
					this.published()
				}
				if (tab._props.label == '优惠券领取记录') {
					this.record()
				}
				if (tab._props.label == '店铺粉丝') {
					this.fansSelect()
				}
				if (tab._props.label == '余额明细') {
					this.size = 10
					this.page = 1
					this.dataSelectYe()
				}
			},
			// 收入金额统计
			moneySelect() {
				let storeId = this.$route.query.storeId
				let page = this.page - 1
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2('/orders/income'),
					method: 'get',
					params: this.$http.adornParams({
						'data': this.info.stockDate,
						'way': this.way,
						'storeId': storeId
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					this.sumincome = returnData.sum0
					this.income = returnData.sum1
					this.wxincome = returnData.pay1
					this.zfbincome = returnData.pay2
					this.yueicome = returnData.pay3
				})
			},
			// 获取店铺详情数据
			dataSelect() {
				let merchantId = this.$route.query.merchantId
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2('/self/store/findByMerchant'),
					method: 'get',
					params: this.$http.adornParams({
						'merchantId': merchantId
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					this.tableData = returnData
					this.logo = returnData.logo
					this.storeName = returnData.storeName
					this.shipAddress = returnData.shipAddress
					this.refundAddress = returnData.refundAddress
					this.refundContact = returnData.refundContact
					this.refundMobile = returnData.refundMobile
					this.sale = returnData.sale
					this.score = returnData.score
					this.describes = returnData.describes
					this.createTime = returnData.createTime
					this.marginStatus = returnData.marginStatus
					this.marginType = returnData.marginType
					this.endTime = returnData.endTime
					this.merchantId = returnData.merchantId
					this.storeId = returnData.storeId
					this.createTime = returnData.createTime
					if (returnData.img) {
						let imgs = returnData.img.split(',')
						this.imgs = imgs;
					} else {
						this.img = returnData.img;
					}

				})
			},
			//上传成功
			handleAvatarSuccess(file, fileList) {
				this.logo = file.data
			},
			// 图标上传
			handleAvatarSuccess1(file) {
				this.img = file.data;
			},
			// 图标上传
			handleAvatarSuccess2(file) {
				this.form.img = file.data;
			},
			//上传成功
			handleUploadSuccess(file, fileList) {
				this.img = file.data
				this.imgs.push(this.img)
				this.$nextTick(() => {
					this.$refs.upload.clearFiles();
				})
			},
			handleChange(file, fileList) {
				this.hideUpload = fileList.length >= this.limit;
			},
			handleRemove(file, fileList) {
				this.hideUpload = fileList.length >= this.limit;
			},
			// 删除
			clear(index) {
				this.imgs.splice(index, 1);
			},
			// 编辑
			updates() {
				this.isshow = false
				this.isbtn = false
			},
			// 取消
			cancel() {
				this.isshow = true
				this.isbtn = true
			},
			// 发货
			colonelClick(rows) {
				if (rows.isExpress == 2) {
					this.id = rows.id
					this.$http({
						url: this.$http.adornUrl2('/orders/expressVirtual'),
						method: 'get',
						params: this.$http.adornParams({
							'id': rows.id
						})
					}).then(({
						data
					}) => {
						if (data.status == 0) {
							this.$message({
								message: '操作成功',
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
				} else {
					this.dialogFormVisible3 = true
					this.id = rows.id
				}
			},
			// 发货
			deliverNoticeTo() {
				if (this.expressName == '') {
					this.$message({
						message: '请输入快递公司',
						type: 'error',
						duration: 1500
					})
					return
				}
				if (this.expressNumber == '') {
					this.$message({
						message: '请输入快递单号',
						type: 'error',
						duration: 1500
					})
					return
				}
				this.$http({
					url: this.$http.adornUrl2('/orders/express'),
					method: 'get',
					params: this.$http.adornParams({
						'id': this.id,
						'expressName': this.expressName,
						'expressNumber': this.expressNumber
					})
				}).then(({
					data
				}) => {
					if (data.status == 0) {
						this.dialogFormVisible3 = false
						let returnData = data.data;
						this.orderName = returnData.expressName
						this.orderNumber = returnData.expressNumber
						this.$message({
							message: '操作成功',
							type: 'success',
							duration: 1500,
							onClose: () => {
								this.expressName = ''
								this.expressNumber = ''
								this.orderSelect()
							}
						})
					}
				})
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
									this.orderSelect()
								}
							})
						}
						if (data.status == -1) {
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
			refuseClick(row) {
				this.ordersId = row.id
				this.dialogFormVisible4 = true
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
						this.dialogFormVisible4 = false
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
			// 保存
			saveClick() {
				this.img = this.imgs.join(',')
				this.$http({
					url: this.$http.adornUrl2('/self/store/update'),
					method: 'post',
					data: this.$http.adornParams({
						'logo': this.logo,
						'img': this.img,
						'merchantId': this.merchantId,
						'refundAddress': this.refundAddress,
						'refundContact': this.refundContact,
						'refundMobile': this.refundMobile,
						'sale': this.sale,
						'score': this.score,
						'createTime': this.createTime,
						'describes': this.describes,
						'marginStatus': this.marginStatus,
						'marginType': this.marginType,
						'endTime': this.endTime,
						'shipAddress': this.shipAddress,
						'storeId': this.storeId,
						'storeName': this.storeName
					})
				}).then(({
					data
				}) => {
					this.$message({
						message: '修改成功',
						type: 'success',
						duration: 1500,
						onClose: () => {
							this.isshow = true
							this.isbtn = true
							this.dataSelect()
						}
					})
				})
			},
			// 商户提现记录
			withdrawSelect() {
				let merchantId = this.$route.query.merchantId
				let page = this.page - 1
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2('/cash/listByMerchantId'),
					method: 'get',
					params: this.$http.adornParams({
						'page': page,
						'size': this.size,
						'merchantId': merchantId
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let returnData = data.data
					this.withdrawData = returnData
				})
			},
			// 获取商品数据
			shopSelect() {
				let storeId = this.$route.query.storeId
				let page = this.page - 1
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2('/goods/list'),
					method: 'get',
					params: this.$http.adornParams({
						'page': page,
						'size': this.size,
						'title': this.title,
						'type': this.typeId,
						'status': this.status,
						'storeId': storeId
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					this.shopData = returnData
				})
			},
			// 商品分类
			classSelect() {
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2('/goodsType/result'),
					method: 'get',
					params: this.$http.adornParams({})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					this.classDatas = returnData
				})
			},
			// 获取订单数据
			orderSelect() {
				let storeId = this.$route.query.storeId
				let page = this.page - 1
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2('/orders/list'),
					method: 'get',
					params: this.$http.adornParams({
						'page': page,
						'size': this.size,
						'orderNum': this.orderNum,
						'status': this.status,
						'storeId': storeId,
						'orderType': this.orderType
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					this.orderData = returnData
				})
			},
			// 订单成交量统计
			orderSelect2() {
				let storeId = this.$route.query.storeId
				let page = this.page - 1
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2('/orders/statistical'),
					method: 'get',
					params: this.$http.adornParams({
						'data': this.info.stockDate,
						'storeId': storeId
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					this.ordersum0 = returnData.sum0
					this.ordersum1 = returnData.sum1
					this.ordersum2 = returnData.sum2

				})
			},
			// 添加分类弹框
			addNotice() {
				this.dialogFormVisible = true
			},
			// 添加分类
			releasNoticeTo() {
				let storeId = this.$route.query.storeId
				let merchantId = this.$route.query.merchantId
				if (this.name == '') {
					this.$notify({
						title: '提示',
						duration: 1800,
						message: '请输入分类名称',
						type: 'warning'
					});
					return
				}
				if (this.img == '') {
					this.$notify({
						title: '提示',
						duration: 1800,
						message: '请上传分类图标',
						type: 'warning'
					});
					return
				}
				this.$http({
					url: this.$http.adornUrl2('/self/storeType/save'),
					method: 'post',
					data: this.$http.adornData({
						'storeId': storeId,
						'merchantId': merchantId,
						'name': this.name,
						'img': this.img
					})
				}).then(({
					data
				}) => {
					this.dialogFormVisible = false
					this.$message({
						message: '操作成功',
						type: 'success',
						duration: 1500,
						onClose: () => {
							this.name = ''
							this.img = ''
							this.classifySelect()
						}
					})
				})
			},
			// 修改弹框
			updatesclass(index, rows) {
				this.dialogFormVisible1 = true;
				this.form.merchantId = rows.merchantId;
				this.form.name = rows.name;
				this.form.storeId = rows.storeId;
				this.form.storeTypeId = rows.storeTypeId
				this.form.createTime = rows.createTime;
				this.form.img = rows.img
			},
			// 修改商品类别
			classNoticeTo() {
				if (this.form.name == '') {
					this.$notify({
						title: '提示',
						duration: 1800,
						message: '请输入分类名称',
						type: 'warning'
					});
					return
				}
				if (this.form.img == '') {
					this.$notify({
						title: '提示',
						duration: 1800,
						message: '请上传分类图标',
						type: 'warning'
					});
					return
				}
				this.$http({
					url: this.$http.adornUrl2('/self/storeType/update'),
					method: 'post',
					data: this.$http.adornData({
						'merchantId': this.form.merchantId,
						'name': this.form.name,
						'img': this.form.img,
						'storeId': this.form.storeId,
						'storeTypeId': this.form.storeTypeId,
						'createTime': this.form.createTime
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
							this.classifySelect()
						}
					})
				})
			},
			// 删除分类
			deletes(row) {
				this.$confirm(`确定删除此条信息?`, '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					this.$http({
						url: this.$http.adornUrl2('/self/storeType/delete'),
						method: 'get',
						params: this.$http.adornParams({
							'id': row.storeTypeId
						})
					}).then(({
						data
					}) => {
						this.$message({
							message: '删除成功',
							type: 'success',
							duration: 1500,
							onClose: () => {
								this.classifySelect()
							}
						})
					})
				}).catch(() => {})
			},
			// 获取分类数据
			classifySelect() {
				let storeId = this.$route.query.storeId
				let page = this.page - 1
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2('/self/storeType/list'),
					method: 'get',
					params: this.$http.adornParams({
						'page': page,
						'size': this.size,
						'storeId': storeId
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					this.typeDatas = returnData
				})
			},
			// 获取虚拟商品数据
			virtualSelect() {
				let storeId = this.$route.query.storeId
				let page = this.page - 1
				let status = 0
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2('/goods/goodsList'),
					method: 'get',
					params: this.$http.adornParams({
						'page': page,
						'size': this.size,
						'title': this.title,
						'type': this.type,
						'status': status,
						'isExpress': this.isExpress,
						'storeId': storeId,
						'isJiFenGoods': this.isJiFenGoods
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					this.virtualData = returnData
				})
			},
			// 编辑
			compile(rows) {
				this.$router.push({
					path: '/virtualAmend',
					query: {
						goodsId: rows.id
					}
				});
			},
			// 补货弹框
			program(row) {
				this.dialogFormVisible2 = true
				this.title1 = row.title
				this.coverImg = row.coverImg
				this.goodsId = row.id
			},
			// 补货
			programNoticeTo() {
				let storeId = this.$route.query.storeId
				let content = this.content.replace(/\r\n/g, ',').replace(/\n/g, ',').replace(/\s/g, ',')
				let cont = content.split(',')
				cont = [...new Set(cont)]
				this.content = cont.join(',')
				if (this.linkUrl == '') {
					this.$notify({
						title: '提示',
						duration: 1800,
						message: '请输入卡密链接',
						type: 'warning'
					});
					return
				}
				if (this.content == '') {
					this.$notify({
						title: '提示',
						duration: 1800,
						message: '请输入卡密内容',
						type: 'warning'
					});
					return
				}
				this.$http({
					url: this.$http.adornUrl2('/selfGoodsVirtual/save'),
					method: 'post',
					data: this.$http.adornData({
						'title': this.title1,
						'content': this.content,
						'linkUrl': this.linkUrl,
						'goodsId': this.goodsId,
						'coverImg': this.coverImg,
						'storeId': storeId
					})
				}).then(({
					data
				}) => {
					if (data.status == 0) {
						this.dialogFormVisible2 = false
						this.$message({
							message: data.data,
							type: 'success',
							duration: 3500,
							onClose: () => {
								this.virtualSelect()
							}
						})
					} else {
						this.$message({
							message: data.data,
							type: 'error',
							duration: 3500,
							onClose: () => {
								this.virtualSelect()
							}
						})
					}
				})
			},
			//获取优惠券数据
			couponSelect() {
				let storeId = this.$route.query.storeId
				let page = this.page - 1
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2('/selfCoupon/list'),
					method: 'get',
					params: this.$http.adornParams({
						'page': page,
						'size': this.size,
						'storeId': storeId
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					this.couponData = returnData
				})
			},
			// 添加优惠券弹框
			addCoupon() {
				this.type = 2
				this.status = 1
				this.dialogFormVisible5 = true
			},
			// 添加优惠券
			couponNoticeTo() {
				let storeId = this.$route.query.storeId
				let merchantId = this.$route.query.merchantId
				let storeName = this.$route.query.storeName
				if (this.couponName == '') {
					this.$notify({
						title: '提示',
						duration: 1800,
						message: '请输入优惠券名称',
						type: 'warning'
					});
					return
				}
				if (this.type == 2 && this.goodsImages == '') {
					this.$notify({
						title: '提示',
						duration: 1800,
						message: '请选择商品',
						type: 'warning'
					});
					return
				}
				if (this.minMoney < this.lessMoney) {
					this.$notify({
						title: '提示',
						duration: 1800,
						message: '优惠券金额不能高于最低消费金额',
						type: 'warning'
					});
					return
				}
				if (this.minMoney == this.lessMoney) {
					this.$notify({
						title: '提示',
						duration: 1800,
						message: '优惠券金额不能等于最低消费金额',
						type: 'warning'
					});
					return
				}
				this.$http({
					url: this.$http.adornUrl2('/selfCoupon/save'),
					method: 'post',
					data: this.$http.adornData({
						'couponName': this.couponName,
						'goodsIds': this.goodsIds,
						'goodsImages': this.goodsImages,
						'lessMoney': this.lessMoney,
						'minMoney': this.minMoney,
						'sort': this.sort,
						'storeId': storeId,
						'merchantId': merchantId,
						'storeName': storeName,
						'status': this.status,
						'type': this.type,
						'validDay': this.validDay,
					})
				}).then(({
					data
				}) => {
					this.$message({
						message: '操作成功',
						type: 'success',
						duration: 1500,
						onClose: () => {
							this.couponName = ''
							this.goodsImages = ''
							this.lessMoney = 0
							this.validDay = 1
							this.minMoney = 0
							this.sort = 0
							this.status = 0
							this.dialogFormVisible5 = false
							this.couponSelect()
						}
					})

				})
			},
			// 修改优惠券
			couponUpdates(index, row) {
				this.dialogFormVisible6 = true
				this.couponform.type = row.type
				this.couponform.couponId = row.couponId
				this.couponform.couponName = row.couponName
				this.couponform.lessMoney = row.lessMoney
				this.couponform.minMoney = row.minMoney
				this.couponform.sort = row.sort
				this.couponform.status = row.status
				this.couponform.validDay = row.validDay
				this.couponform.createTime = row.createTime
				this.couponform.goodsIds = row.goodsIds
				this.couponform.goodsImages = row.goodsImages
				this.couponform.storeId = row.storeId
				this.couponform.merchantId = row.merchantId
				this.couponform.storeName = row.storeName
			},
			// 修改
			couponNoticeTo2() {
				if (this.couponform.couponName == '') {
					this.$notify({
						title: '提示',
						duration: 1800,
						message: '请输入优惠券名称',
						type: 'warning'
					});
					return
				}
				if (this.couponform.type == 2 && this.couponform.goodsImages == '') {
					this.$notify({
						title: '提示',
						duration: 1800,
						message: '请选择商品',
						type: 'warning'
					});
					return
				}
				if (this.couponform.minMoney < this.couponform.lessMoney) {
					this.$notify({
						title: '提示',
						duration: 1800,
						message: '优惠券金额不能高于最低消费金额',
						type: 'warning'
					});
					return
				}
				this.$http({
					url: this.$http.adornUrl2('/selfCoupon/update'),
					method: 'post',
					data: this.$http.adornData({
						'couponId': this.couponform.couponId,
						'couponName': this.couponform.couponName,
						'goodsIds': this.couponform.goodsIds,
						'goodsImages': this.couponform.goodsImages,
						'lessMoney': this.couponform.lessMoney,
						'minMoney': this.couponform.minMoney,
						'sort': this.couponform.sort,
						'storeId': this.couponform.storeId,
						'merchantId': this.couponform.merchantId,
						'storeName': this.couponform.storeName,
						'status': this.couponform.status,
						'type': this.couponform.type,
						'validDay': this.couponform.validDay,
						'createTime': this.couponform.createTime,
					})
				}).then(({
					data
				}) => {
					this.$message({
						message: '操作成功',
						type: 'success',
						duration: 1500,
						onClose: () => {
							this.dialogFormVisible6 = false
							this.couponSelect()
						}
					})

				})
			},
			// 发布优惠券弹框
			issue(index, row) {
				this.dialogFormVisible8 = true
				this.status = 1
				this.couponId = row.couponId
				this.couponName1 = row.couponName
				this.goodsIds = row.goodsIds
				this.type = row.type
			},
			// 发布优惠券
			issueNoticeTo() {
				let storeId = this.$route.query.storeId
				let merchantId = this.$route.query.merchantId
				let storeName = this.$route.query.storeName
				if (this.startTime == '') {
					this.$notify({
						title: '提示',
						duration: 1800,
						message: '请选择优惠券领取时间',
						type: 'warning'
					});
					return
				}
				if (this.endTime == '') {
					this.$notify({
						title: '提示',
						duration: 1800,
						message: '请选择优惠券结束时间',
						type: 'warning'
					});
					return
				}
				this.$http({
					url: this.$http.adornUrl2('/selfCouponIssue/save'),
					method: 'post',
					data: this.$http.adornData({
						'couponName': this.couponName1,
						'couponId': this.couponId,
						'startTime': this.startTime,
						'endTime': this.endTime,
						'storeId': storeId,
						'merchantId': merchantId,
						'storeName': storeName,
						'issueNumber': this.issueNumber,
						'remainNumber': this.issueNumber,
						'isLimit': this.isLimit,
						'status': this.status,
						'type': this.type,
						'goodsIds': this.goodsIds
					})
				}).then(({
					data
				}) => {
					this.$message({
						message: '操作成功',
						type: 'success',
						duration: 1500,
						onClose: () => {
							this.startTime = ''
							this.endTime = ''
							this.isLimit = 1
							this.status = 1
							this.issueNumber = ''
							this.dialogFormVisible8 = false
							this.dataSelect()
						}
					})

				})
			},
			// 选择商品
			couponshopClick() {
				this.dialogFormVisible7 = true
				this.coushopSelect()
			},
			// 获取商品数据
			coushopSelect() {
				let page = this.page - 1
				let status = 0
				this.title = ''
				let storeId = this.$route.query.storeId
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2('/goods/list'),
					method: 'get',
					params: this.$http.adornParams({
						'page': page,
						'size': this.size,
						'title': this.title,
						'type': this.typeId,
						'status': status,
						'storeId': storeId
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					this.coushopData = returnData
				})
			},
			// 选择商品
			selectClick(index, row) {
				this.$http({
					url: this.$http.adornUrl2('/goods/find'),
					method: 'get',
					params: this.$http.adornParams({
						'id': row.id
					})
				}).then(({
					data
				}) => {
					this.dialogFormVisible7 = false
					let returnData = data.data;
					this.goodsIds = returnData.id
					this.goodsImages = returnData.coverImg
					this.form.goodsIds = returnData.id
					this.form.goodsImages = returnData.coverImg
				})
			},
			// 已发布优惠券
			published() {
				let storeId = this.$route.query.storeId
				let page = this.page - 1
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2('/selfCouponIssue/list'),
					method: 'get',
					params: this.$http.adornParams({
						'page': page,
						'size': this.size,
						'storeId': storeId
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					this.issueData = returnData
				})
			},
			// 删除优惠券
			shopdeletes(row) {
				this.$confirm(`确定删除此条信息?`, '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					this.$http({
						url: this.$http.adornUrl2('/selfCouponIssue/delete'),
						method: 'get',
						params: this.$http.adornParams({
							'id': row.couponIssueId
						})
					}).then(({
						data
					}) => {
						if (data && data.status == 0) {
							this.$message({
								message: '删除成功',
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
			// 修改已发布优惠券
			amendcoupon(index, row) {
				console.log(row)
				this.dialogFormVisible9 = true
				this.formissue.couponId = row.couponId
				this.formissue.couponIssueId = row.couponIssueId
				this.formissue.couponName = row.couponName
				this.formissue.createTime = row.createTime
				this.formissue.storeId = row.storeId
				this.formissue.merchantId = row.merchantId
				this.formissue.storeName = row.storeName
				this.formissue.endTime = row.endTime
				this.formissue.isLimit = row.isLimit
				this.formissue.issueNumber = row.issueNumber
				this.formissue.remainNumber = row.remainNumber
				this.formissue.startTime = row.startTime
				this.formissue.status = row.status
				this.formissue.type = row.type
				this.formissue.createTime = row.createTime
			},
			// 修改已发布优惠券
			amendNoticeTo() {
				if (this.formissue.type == '') {
					this.$notify({
						title: '提示',
						duration: 1800,
						message: '请选择状态',
						type: 'warning'
					});
					return
				}
				this.$http({
					url: this.$http.adornUrl2('/selfCouponIssue/update'),
					method: 'post',
					data: this.$http.adornData({
						'couponId': this.formissue.couponId,
						'couponIssueId': this.formissue.couponIssueId,
						'startTime': this.formissue.startTime,
						'couponName': this.formissue.couponName,
						'storeId': this.formissue.storeId,
						'merchantId': this.formissue.merchantId,
						'storeName': this.formissue.storeName,
						'endTime': this.formissue.endTime,
						'issueNumber': this.formissue.issueNumber,
						'remainNumber': this.formissue.issueNumber,
						'isLimit': this.formissue.isLimit,
						'status': this.formissue.status,
						'type': this.formissue.type,
						'createTime': this.formissue.createTime
					})
				}).then(({
					data
				}) => {
					this.$message({
						message: '操作成功',
						type: 'success',
						duration: 1500,
						onClose: () => {
							this.dialogFormVisible9 = false
							this.published()
						}
					})
				})
			},
			// 查询
			select3() {
				this.record()
			},
			// 重置
			cleans3() {
				this.couponName = ''
				this.record()
			},
			// 刷新
			refresh3() {
				this.record()
			},
			//获取已发布优惠券数据
			record() {
				let storeId = this.$route.query.storeId
				let page = this.page - 1
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2('/selfCouponUser/list'),
					method: 'get',
					params: this.$http.adornParams({
						'page': page,
						'size': this.size,
						'couponName': this.couponName,
						'storeId': storeId
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					this.recordData = returnData
				})
			},
			select4() {
				this.page = 1
				this.size = 5
				this.dataSelect2()
			},
			cleans4() {
				this.orderType = 0
				this.orderNum = ''
				this.mobile = ''
				this.dataSelect2()
			},
			refresh4() {
				this.dataSelect2()
			},
			// 获取待核销订单数据
			dataSelect2() {
				let page = this.page - 1
				let storeId = this.$route.query.storeId
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2('/orders/list'),
					method: 'get',
					params: this.$http.adornParams({
						'page': page,
						'size': this.size,
						'orderNum': this.orderNum,
						'status': this.status,
						'title': this.title,
						'storeId': storeId,
						'mobile': this.mobile,
						'isExpress': this.isExpress,
						'orderType': this.orderType
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					this.tableData2 = returnData
				})
			},
			// 批量核销
			volume(id) {
				var ids = id ? [id] : this.checkBoxData.map(item => {
					return item.id
				})
				for (var i in ids) {
					this.$http({
						url: this.$http.adornUrl2('/orders/isExpress'),
						method: 'get',
						params: this.$http.adornParams({
							'ids': ids[i]
						})
					}).then(({
						data
					}) => {
						this.$message({
							message: '批量核销成功',
							type: 'success',
							duration: 1500,
							onClose: () => {
								this.dataSelect2()
							}
						})
					})
				}
			},
			// 核销
			volumeOne(row) {
				this.$confirm(`确定核销此条信息?`, '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					this.$http({
						url: this.$http.adornUrl2('/orders/isExpress'),
						method: 'get',
						params: this.$http.adornParams({
							'ids': row.id
						})
					}).then(({
						data
					}) => {
						this.$message({
							message: '核销成功',
							type: 'success',
							duration: 1500,
							onClose: () => {
								this.dataSelect2()
							}
						})
					})
				}).catch(() => {})
			},
			// 店铺粉丝
			fansSelect(){
				let storeId = this.$route.query.storeId
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2('/selfStoreCollect/list'),
					method: 'get',
					params: this.$http.adornParams({
						'page': this.page-1,
						'size': this.size,
						'storeId': storeId
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					this.fansData = returnData
				})
			},
			// 获取规格数据
			cationSelect() {
				let storeId = this.$route.query.storeId
				let page = this.page - 1
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2('/selfGoodsRule/list'),
					method: 'get',
					params: this.$http.adornParams({
						'page': page,
						'size': this.size,
						'storeId':storeId
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					this.cationDatas = returnData
				})
			},
			// 添加规格弹框
			cationAddNotice() {
				this.dialogFormVisibleC = true
			},
			// 添加规格
			specificationTo() {
				let storeId = this.$route.query.storeId
				if (this.ruleName == '') {
					this.$message({
						title: '提示',
						type: 'error',
						duration: 1800,
						message: '请输入标题名称',
						type: 'warning'
					});
					return
				}
				if (this.ruleValue.length == 0) {
					this.$message({
						title: '提示',
						type: 'error',
						duration: 1800,
						message: '请至少添加一种规格',
						type: 'warning'
					});
					return
				}
				this.$http({
					url: this.$http.adornUrl2('/selfGoodsRule/save'),
					method: 'post',
					data: this.$http.adornData({
						'ruleName': this.ruleName,
						'ruleValue': this.ruleValue,
						'storeId':storeId
					})
				}).then(({
					data
				}) => {
					this.dialogFormVisibleC = false
					this.$message({
						message: '操作成功',
						type: 'success',
						duration: 1500,
						onClose: () => {
							this.ruleName = ''
							this.ruleValue = []
							this.cationSelect()
						}
					})
				})
			},
			// 修改弹框
			cationUpdates(rows) {
				this.dialogFormVisibleC1 = true;
				this.cationform.id = rows.id;
				this.cationform.ruleName = rows.ruleName;
				this.cationform.ruleValue = rows.ruleValue;
				this.cationform.createTime = rows.createTime;
			},
			// 修改商品规格
			cationAmendNoticeTo() {
				let storeId = this.$route.query.storeId
				this.$http({
					url: this.$http.adornUrl2('/selfGoodsRule/update'),
					method: 'post',
					data: this.$http.adornData({
						'id': this.cationform.id,
						'ruleName': this.cationform.ruleName,
						'ruleValue': this.cationform.ruleValue,
						'createTime': this.cationform.createTime,
						'storeId':storeId
					})
				}).then(({
					data
				}) => {
					this.dialogFormVisibleC1 = false
					this.$message({
						message: '操作成功',
						type: 'success',
						duration: 1500,
						onClose: () => {
							this.cationSelect()
						}
					})
				})
			},
			btnToclick1() {
				this.cationisshow = false
				this.releobject.value = ''
				this.releobject.detail = ''
			},
			speciTotwo1() {
				this.cationisshow = true
				this.releobject.value = ''
				this.releobject.detail = ''
			},
			deleterule1(index) {
				this.cationform.ruleValue.splice(index, 1);
			},
			speciTo1() {
				this.cationform.ruleValue.push({
					value: this.releobject.value,
					detail: this.releobject.detail
				});
				this.cationisshow = true
				this.releobject.value = ''
				this.releobject.detail = ''
			},
			handleClose1(tag, j, item) {
				var detailarr = item.detail.split(',')
				if (detailarr.length > 1) {
					detailarr.splice(detailarr.indexOf(tag), 1);
					item.detail = String(detailarr)
					console.log(item.detail)
				} else {
					console.log(item.detail)
					this.$message({
						title: '提示',
						type: 'error',
						duration: 1800,
						message: '规格值至少要有一个',
						type: 'warning'
					});
				}
			},
			handleInputConfirm1(index, item) {
				let inputValue = this.inputValues[index];
				if (inputValue) {
					var detailarr = item.detail.split(',')
					detailarr.push(inputValue)
					for (var i = 0; i < detailarr.length; i++) {
						for (var j = 0; j < detailarr.length; j++) {
							if (detailarr[i] == detailarr[j] && i != j) {
								detailarr.splice(j, 1);
							}
						}
					}
					item.detail = String(detailarr)
					this.inputValues[index] = '';
				}
				this.inputVisible = false;
			},
			// 删除分类
			cationDeletes(row) {
				this.$confirm(`确定删除此条信息?`, '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					this.$http({
						url: this.$http.adornUrl2('/selfGoodsRule/delete'),
						method: 'get',
						params: this.$http.adornParams({
							'id': row.id
						})
					}).then(({
						data
					}) => {
						this.$message({
							message: '删除成功',
							type: 'success',
							duration: 1500,
							onClose: () => {
								this.cationSelect()
							}
						})
					})
				}).catch(() => {})
			},
			deleterule(index) {
				this.ruleValue.splice(index, 1);
			},
			btnToclick() {
				this.cationisshow = false
				this.releobject.value = ''
				this.releobject.detail = ''
			},
			speciTotwo() {
				this.cationisshow = true
				this.releobject.value = ''
				this.releobject.detail = ''
			},
			speciTo() {
				if (this.ruleName == '') {
					this.$message({
						title: '提示',
						type: 'error',
						duration: 1800,
						message: '请输入标题名称',
						type: 'warning'
					});
					return
				}
				if (this.releobject.value == '') {
					this.$message({
						title: '提示',
						type: 'error',
						duration: 1800,
						message: '请输入规格',
						type: 'warning'
					});
					return
				}
				if (this.releobject.detail == '') {
					this.$message({
						title: '提示',
						duration: 1800,
						type: 'error',
						message: '请输入规格值',
						type: 'warning'
					});
					return
				}

				this.ruleValue.push({
					value: this.releobject.value,
					detail: this.releobject.detail
				});
				this.cationisshow = true
				this.releobject.value = ''
				this.releobject.detail = ''
			},
			handleClose(tag, j, item) {
				var detailarr = item.detail.split(',')
				if (detailarr.length > 1) {
					detailarr.splice(detailarr.indexOf(tag), 1);
					item.detail = String(detailarr)
				} else {
					this.$message({
						title: '提示',
						type: 'error',
						duration: 1800,
						message: '规格值至少要有一个',
						type: 'warning'
					});
				}
			},
			handleInputConfirm(index, item) {
				let inputValue = this.inputValues[index];
				if (inputValue) {
					var detailarr = item.detail.split(',')
					detailarr.push(inputValue)
					for (var i = 0; i < detailarr.length; i++) {
						for (var j = 0; j < detailarr.length; j++) {
							if (detailarr[i] == detailarr[j] && i != j) {
								detailarr.splice(j, 1);
							}
						}
					}
					item.detail = String(detailarr)
					this.inputValues[index] = '';
				}
				this.inputVisible = false;

			},
			// 余额明细
			dataSelectYe() {
				let merchantId = this.$route.query.merchantId
				let page = this.page - 1
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2('/self/merchantApply/selectDetails'),
					method: 'get',
					params: this.$http.adornParams({
						'page': page,
						'size': this.size,
						'merchantId': merchantId,
						'classify':0
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					this.tableDataYe = returnData

				})
			},
		},
		activated() {
			this.dataSelect()
		}
	};
</script>
<style scoped="scoped">
	.content_item {
		margin-bottom: 20px;
	}

	.content_item span {
		display: inline-block;
		width: 120px;
		text-align: left;
	}

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

	.divhove {
		position: absolute;
		width: 100%;
		height: 100%;
		left: 0;
		top: 0;
		opacity: 0;
		cursor: default;
		text-align: center;
		padding-top: 50%;
		border-radius: 6px;
		background-color: rgba(0, 0, 0, .5);
		-webkit-transition: opacity .3s;
		transition: opacity .3s;
	}

	.el-tag--medium {
		margin-right: 10px;
	}

	.divhove:hover {
		opacity: 0.8;
	}

	.text_color span {
		margin-right: 5px;
	}

	.shop_item {
		margin-top: 60px;
		padding-left: 20px;
	}
</style>
