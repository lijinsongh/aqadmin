<template>
	<div class="components-container main-cont">
		<div style="display: inline-block;font-size:18px;margin-bottom: 15px;">
			<a href="#" @click="prev" style="text-decoration:none;font-size: 14px;">
				<icon-svg name="jiantou" style="width: 1.2em;height: 1.2em;position: relative;top: 0.3em;"></icon-svg>
				返回
			</a>
			<span style="display: inline-block;margin: 0 15px;color: #D9D9D9;">|</span>
			<span>商品修改</span>
		</div>
		<div style="position: relative;">
			<el-row>
				<el-col :span="6">
					<span>商品标题：</span>
					<el-input style="width: 200px;" class="margin15" placeholder="请输入商品标题" v-model="title" autosize>
					</el-input>
				</el-col>
				<el-col :span="6">
					<span>商品分类：</span>
					<el-cascader class="margin15" v-model="value" :options="typeDatas" placeholder="请选择商品分类"
						:show-all-levels="false" :change-on-select="true" :props="{ expandTrigger: 'hover' }"
						@change="handleChange1">
					</el-cascader>
				</el-col>
				<el-col :span="6">
					<span>店铺分类：</span>
					<el-select v-model="storeTypeId" class="margin15" placeholder="请选择店铺分类" style="width:200px;">
						<el-option v-for="(item,index) in storeTypeIds" :key="item.index" :label="item.label"
							:value="item.value">
						</el-option>
					</el-select>
				</el-col>
				<!-- <el-col :span="6" >
				<span>商户号	：&nbsp;&nbsp;</span>
				<el-input style="width:200px;" class="margin15" placeholder="请输入商户号"  v-model="merchants" autosize></el-input>
			</el-col> -->
				<el-col :span="6">
					<span>商品原价：</span>
					<el-input style="width:200px;" class="margin15" placeholder="请输入商品原价" type="number" :min="0"
						:controls="false" v-model="originalPrice" autosize></el-input>
				</el-col>

			</el-row>
			<el-row>
				<el-col :span="6">
					<span>商品价格：</span>
					<el-input style="width:200px;" class="margin15" placeholder="请输入商品价格" type="number" :min="0"
						:controls="false" v-model="price" autosize></el-input>
				</el-col>
				<el-col :span="6">
					<span>会员价格：</span>
					<el-input style="width:200px;" class="margin15" placeholder="请输入会员价格" type="number" :min="0"
						:controls="false" v-model="memberPrice" autosize></el-input>
				</el-col>
				<!-- <el-col :span="6">
				<span>商品佣金：</span>
				<el-input style="width:200px;" class="margin15" placeholder="例0.1 等于10%" type="number" :min="0" :max="10" :controls="false" v-model="commissionPrice" autosize></el-input>
			</el-col> -->
				<el-col :span="6">
					<span>商品销量：</span>
					<el-input style="width:200px;" class="margin15" placeholder="请输入商品销量" type="number" :min="0"
						:controls="false" v-model="sales" autosize></el-input>
				</el-col>
				<el-col :span="6">
					<span>商品状态：</span>
					<el-select v-model="status" class="margin15" placeholder="请选择商品类型" style="width:200px;">
						<el-option v-for="(item,index) in statusmain2" :key="item.index" :label="item.label"
							:value="item.value">
						</el-option>
					</el-select>
				</el-col>

			</el-row>
			<el-row>
				<!-- <el-col :span="6">
					<span>生产日期：</span>
					<el-date-picker class="margin15" style="width:200px;"  format="yyyy-MM-dd" value-format="yyyy-MM-dd" v-model="productionDate" type="date"
						placeholder="选择生产日期">
					</el-date-picker>
				</el-col>
				<el-col :span="6">
					<span>到期时间：</span>
					<el-date-picker class="margin15" style="width:200px;" format="yyyy-MM-dd" value-format="yyyy-MM-dd" v-model="expirationDate" type="date"
						placeholder="选择到期时间">
					</el-date-picker>
				</el-col> -->
				<el-col :span="6">
					<span>必买理由：</span>
					<el-input style="width:200px;" class="margin15" placeholder="请输入必买理由" v-model="buyReason" autosize>
					</el-input>
				</el-col>
        <el-col :span="6">
        	<span>是否发货：</span>
        	<el-select v-model="isExpress" class="margin15" placeholder="请选择商品是否发货" style="width:200px;">
        		<el-option v-for="(item,index) in isExpressmain" :key="item.index" :label="item.label"
        			:value="item.value">
        		</el-option>
        	</el-select>
        </el-col>
        <el-col :span="6">
        	<span>商品品牌：</span>
        	<el-select v-model="brandId" class="margin15" placeholder="请选择商品品牌" style="width:200px;">
        		<el-option v-for="(item,index) in  brandIds" :key="item.index" :label="item.label"
        			:value="item.value">
        		</el-option>
        	</el-select>
        </el-col>
        <el-col :span="6">
        	<span>商品佣金：</span>
        	<el-input style="width:200px;" class="margin15" placeholder="例0.1 等于10%" type="number" :min="0"
        		:controls="false" v-model="commissionPrice" autosize></el-input>
        	<span
        		style="display: inline-block; width:200px; color: #f56c6c;font-size: 13px;margin-left: 75px;margin-top: 2px;">例如商品价格为100元，商品佣金设置为10%，该商品实际收入为90元。</span>
        </el-col>
				<el-col :span="6" style="margin-top: 20px;">
				<span>精选好物：</span>
				<el-radio-group v-model="isSelect">
				    <el-radio :label="1">是</el-radio>
				    <el-radio :label="0">否</el-radio>
				</el-radio-group>
			</el-col>
				<el-col :span="6"  style="margin-top: 20px;">
				<span>首页商品：</span>
				<el-radio-group v-model="homeGoods">
				    <el-radio :label="1">是</el-radio>
				    <el-radio :label="0">否</el-radio>
				</el-radio-group>
			</el-col>
				<el-col :span="6"  style="margin-top: 20px;">
				<span>每日推荐：</span>
				<el-radio-group v-model="isRecommend">
				    <el-radio :label="1">是</el-radio>
				    <el-radio :label="0">否</el-radio>
				</el-radio-group>
			</el-col>

			</el-row>
      <div style="display: flex;align-items: center;margin: 2% 0;">
        <span style="display: inline-block;text-align: right;">商品封面图：</span>
        <div
          style=" width:148px;height:148px;background-color: #fbfdff;border: 1px dashed #c0ccda;border-radius: 6px;text-align: center;line-height: 148px;">
          <el-upload class="avatar-uploader" v-model="coverImg" action="https://sanshengadmin.hnqxkjfz.com/tao/alioss/upload"
            :show-file-list="false" :on-success="handleAvatarSuccess1">
            <img v-if="coverImg" :src="coverImg" class="avatar" style="width: 140px;height: 140px;" />
            <i v-else class="el-icon-plus avatar-uploader-icon" style="font-size: 28px;color: #8c939d"></i>
          </el-upload>
        </div>
      </div>
      <div style="display: flex;align-items: center;margin: 2% 0;flex-flow: wrap;">
        <span style="display: inline-block;text-align: right;">商品轮播图：</span>
        <div class="imgs" v-for="(item,index) in photos" :key="index">
        	<img width="100%" class="images" height="100%" :src="item" alt="">
        	<span class="dels">
        		<i class="el-icon-delete" @click="dels(index)"></i>
        	</span>
        </div>
        <div>
          <el-upload action="https://sanshengadmin.hnqxkjfz.com/tao/alioss/upload" class="avatar-uploader"
            :on-success="handleUploadSuccess" :show-file-list="false">
            <i class="el-icon-plus avatar-uploader-icon" style="font-size: 28px;color: #8c939d;width:148px;height:148px;background-color: #fbfdff;border: 1px dashed #c0ccda;border-radius: 6px;text-align: center;line-height: 148px;"></i>
          </el-upload>
        </div>
      </div>
			<!-- <div style="display: flex;align-items: center;margin: 2% 0;">
				<span style="display: inline-block;text-align: right;">商品封面图：</span>
				<div
					style=" width:148px;height:148px;background-color: #fbfdff; border: 1px dashed #c0ccda;border-radius: 6px;text-align: center;line-height: 148px;">
					<img v-if="coverImg" :src="coverImg" class="avatar" style="width: 148px;height: 148px;"
						@click="imgBtn(1)" />
					<i v-else class="el-icon-plus avatar-uploader-icon" style="font-size: 28px;color: #8c939d"
						@click="imgBtn(1)"></i>
				</div>
			</div>
			<div style="display: flex;align-items: center;margin: 2% 0;flex-flow: wrap;">
				<span style="display: inline-block;text-align: right;">商品轮播图：</span>
				<div class="imgs" v-for="(item,index) in photos" :key="index">
					<img width="100%" class="images" height="100%" :src="item" alt="">
					<span class="dels">
						<i class="el-icon-delete" @click="dels(index)"></i>
					</span>
				</div>
				<div
					style=" width:148px;height:148px;background-color: #fbfdff; border: 1px dashed #c0ccda;border-radius: 6px;text-align: center;line-height: 148px;">
					<i class="el-icon-plus avatar-uploader-icon" style="font-size: 28px;color: #8c939d"
						@click="imgBtn(2)"></i>
				</div>
			</div> -->
			<div style="display: flex;align-items: center;margin: 2% 0;">
				<span style="display: inline-block;text-align: right;">商品规格：</span>
				<el-radio-group v-model="guige" @change="guigeChange">
					<el-radio :label="0">单规格</el-radio>
					<el-radio :label="1">多规格</el-radio>
				</el-radio-group>
				<div v-if="guigeshow" style="margin-left: 20px;">
					<el-select v-model="ruleId" placeholder="请选择商品规格" style="width:200px;"
						@change="selectTrigger(ruleId)">
						<el-option v-for="(item,index) in specifdata" :key="item.index" :label="item.ruleName"
							:value="item.id">
						</el-option>
					</el-select>
				</div>
			</div>
			<div v-if="ruleValue" style="margin-left: 70px;">
				<div class="ruleitem" v-for="(item,index) in ruleValue" :key="index">
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
				<div style="margin-top:20px;" v-if="ruleshow">
					<div v-if="isshow" class="btn_specif">
						<el-button type="primary" @click="btnToclick()">添加新规格</el-button>
						<el-button type="success" @click="create()">立即生成</el-button>
					</div>
				</div>
				<div style="margin-top:10px;">
					<div v-if="!isshow" style="margin-top: 20px;">
						<div style="display: inline-block;">
							<span style="width:80px;display: inline-block;text-align: right;">规格：</span>
							<el-input style="width:50%;" v-model="releobject.value" placeholder="例:颜色"></el-input>
						</div>
						<div style="display: inline-block;">
							<span style="width:80px;display: inline-block;text-align: right;">规格值：</span>
							<el-input style="width:50%;" v-model="releobject.detail" placeholder="例:黑色"></el-input>
						</div>
						<div style="display: inline-block;">
							<el-button type="primary" @click="speciTo()">确 定</el-button>
							<el-button @click="speciTotwo()">取 消</el-button>
						</div>
					</div>
				</div>
			</div>
			<div style="display: flex;margin-top: 20px;" v-if="valueshow">
				<span>商品属性：</span>
				<div class="property_table">
					<table style="width: 100%;">
						<tr style="display:flex;text-align: left;">
							<th style="width:120px;" v-for="item in headerData">{{item}}</th>
						</tr>
					</table>
					<el-table :data="valueData">
						<el-table-column v-if="detailJson.length == 1 || detailJson.length == 0" prop="value0"
							width="120">
						</el-table-column>
						<el-table-column v-if="detailJson.length == 2" prop="value0" width="120">
						</el-table-column>
						<el-table-column v-if="detailJson.length == 2" prop="value1" width="120">
						</el-table-column>
						<el-table-column v-if="detailJson.length == 3" prop="value0" width="120">
						</el-table-column>
						<el-table-column v-if="detailJson.length == 3" prop="value1" width="120">
						</el-table-column>
						<el-table-column v-if="detailJson.length == 3" prop="value2" width="120">
						</el-table-column>
						<el-table-column v-if="detailJson.length == 4" prop="value0" width="120">
						</el-table-column>
						<el-table-column v-if="detailJson.length == 4" prop="value1" width="120">
						</el-table-column>
						<el-table-column v-if="detailJson.length == 4" prop="value2" width="120">
						</el-table-column>
						<el-table-column v-if="detailJson.length == 4" prop="value3" width="120">
						</el-table-column>
						<el-table-column v-if="detailJson.length == 5" prop="value0" width="120">
						</el-table-column>
						<el-table-column v-if="detailJson.length == 5" prop="value1" width="120">
						</el-table-column>
						<el-table-column v-if="detailJson.length == 5" prop="value2" width="120">
						</el-table-column>
						<el-table-column v-if="detailJson.length == 5" prop="value3" width="120">
						</el-table-column>
						<el-table-column v-if="detailJson.length == 5" prop="value4" width="120">
						</el-table-column>
						<el-table-column prop="skuImg" width="100">
							<template slot-scope="scope">
								<div class="imgWrap"
									style=" width:60px;height:60px;background-color: #fbfdff; border: 1px dashed #c0ccda;border-radius: 6px;text-align: center;line-height: 60px;">
									<!-- <img v-if="scope.row.skuImg" :src="scope.row.skuImg"
										@click="imgBtn(6,scope.$index)" class="avatar"
										style="border-radius: 6px;width:60px;height: 60px;" />
									<i v-else class="el-icon-plus avatar-uploader-icon"
										style="font-size: 28px;color: #8c939d" @click="imgBtn(6,scope.$index)"></i> -->
                    <el-upload class="avatar-uploader" v-model="scope.row.skuImg"
                      action="https://sanshengadmin.hnqxkjfz.com/tao/alioss/upload" :show-file-list="false"
                      :on-success="handleAvatarSuccess">
                      <img v-if="scope.row.skuImg" :src="scope.row.skuImg" class="avatar"
                        style="border-radius: 6px;width:50px;height: 50px;" @click="curRowIndex=scope.$index" />
                      <i v-else class="el-icon-plus avatar-uploader-icon" @click="curRowIndex=scope.$index"></i>
                    </el-upload>
								</div>
							</template>
						</el-table-column>
						<el-table-column prop="skuOriginalPrice" width="120">
							<template slot-scope="scope">
								<div>
									<el-input type="text" v-model="scope.row.skuOriginalPrice"
										style="width:60px;font-size:14px;">
									</el-input>
								</div>
							</template>
						</el-table-column>
						<el-table-column prop="skuPrice" width="120">
							<template slot-scope="scope">
								<div>
									<el-input type="text" v-model="scope.row.skuPrice"
										style="width:60px;font-size:14px;">
									</el-input>
								</div>
							</template>
						</el-table-column>
						<el-table-column prop="memberPrice" width="120">
							<template slot-scope="scope">
								<div>
									<el-input type="text" v-model="scope.row.memberPrice"
										style="width:60px;font-size:14px;">
									</el-input>
								</div>
							</template>
						</el-table-column>
						<el-table-column prop="stock" width="120">
							<template slot-scope="scope">
								<div>
									<el-input type="text" v-model="scope.row.stock" style="width:60px;font-size:14px;">
									</el-input>
								</div>
							</template>
						</el-table-column>
						<el-table-column prop="sales" width="120">
							<template slot-scope="scope">
								<div>
									<el-input type="text" v-model="scope.row.sales" style="width:60px;font-size:14px;">
									</el-input>
								</div>
							</template>
						</el-table-column>
						<el-table-column width="120">
							<template slot-scope="scope">
								<el-button :disabled="!isAuth('userList:delete')" size="mini" type="danger"
									@click="deleter(scope.$index)">删除
								</el-button>
							</template>
						</el-table-column>
					</el-table>
				</div>
			</div>
			<div v-if="valueshow1">
				<span style="position: relative;top: -120px;">商品属性：</span>
				<div class="property_table" style="display: inline-block;">
					<table style="width: 100%;">
						<tr style="display:flex;text-align: left;">
							<th style="width:120px;" v-for="item in headerData2">{{item}}</th>
						</tr>
					</table>
					<el-table :data="sku">
						<el-table-column prop="skuImg" width="150">
							<template slot-scope="scope">
								<div class="imgWrap"
									style=" width:60px;height:60px;background-color: #fbfdff; border: 1px dashed #c0ccda;border-radius: 6px;text-align: center;line-height: 60px;">
									<!-- <img v-if="scope.row.skuImg" :src="scope.row.skuImg" @click="imgBtn(5)"
										class="avatar" style="border-radius: 6px;width:60px;height: 60px;" />
									<i v-else class="el-icon-plus avatar-uploader-icon"
										style="font-size: 28px;color: #8c939d" @click="imgBtn(5)"></i> -->
                    <el-upload class="avatar-uploader" v-model="scope.row.skuImg"
                      action="https://sanshengadmin.hnqxkjfz.com/tao/alioss/upload" :show-file-list="false"
                      :on-success="handleAvatarSuccess3">
                      <img v-if="scope.row.skuImg" :src="scope.row.skuImg" class="avatar"
                        style="border-radius: 6px;width:50px;height: 50px;" @click="curRowIndex=scope.$index" />
                      <i v-else class="el-icon-plus avatar-uploader-icon" @click="curRowIndex=scope.$index"></i>
                    </el-upload>
								</div>
							</template>
						</el-table-column>
						<el-table-column prop="skuOriginalPrice" width="120">
							<template slot-scope="scope">
								<div>
									<el-input type="text" v-model="scope.row.skuOriginalPrice"
										style="width:60px;font-size:14px;">
									</el-input>
								</div>
							</template>
						</el-table-column>
						<el-table-column prop="skuPrice" width="120">
							<template slot-scope="scope">
								<div>
									<el-input type="text" v-model="scope.row.skuPrice"
										style="width:60px;font-size:14px;">
									</el-input>
								</div>
							</template>
						</el-table-column>
						<el-table-column prop="memberPrice" width="120">
							<template slot-scope="scope">
								<div>
									<el-input type="text" v-model="scope.row.memberPrice"
										style="width:60px;font-size:14px;">
									</el-input>
								</div>
							</template>
						</el-table-column>
						<el-table-column prop="stock" width="120">
							<template slot-scope="scope">
								<div>
									<el-input type="text" v-model="scope.row.stock" style="width:60px;font-size:14px;">
									</el-input>
								</div>
							</template>
						</el-table-column>
						<el-table-column prop="sales" width="120">
							<template slot-scope="scope">
								<div>
									<el-input type="text" v-model="scope.row.sales" style="width:60px;font-size:14px;">
									</el-input>
								</div>
							</template>
						</el-table-column>
					</el-table>
				</div>
			</div>
			<div style="height: 38px;line-height: 38px;margin:20px 0;">
				<span>是否包邮：</span>
				<el-radio-group v-model="positage" @change="agreeChange">
					<el-radio :label="0">是</el-radio>
					<el-radio :label="1">否</el-radio>
					<el-input v-if="priceshow" style="width:200px;margin-left: 10px;" placeholder="请输入邮费" type="number"
						v-model="postagePrice" autosize></el-input>
				</el-radio-group>
			</div>
		</div>
		<!-- <div class="tinymce-content" style="display: flex;">
			<div style="width:90px;">商品详情：</div>
			<quill-editor ref="myTextEditor" v-model="descrition" :options="quillOption"
				style="padding-bottom: 50px;height: 300px;width: 72%;display: inline-table;margin-bottom: 60px;">
			</quill-editor>
		</div> -->
		<div class="tinymce-content" style="display: flex;position: relative;">
			<div style="width:90px;">商品详情：</div>
			<quill-editor ref="myTextEditor" v-model="descrition" :options="quillOption"
				style="padding-bottom: 50px;height: 300px;width: 72%;display: inline-table;margin-bottom: 60px;">
			</quill-editor>
		</div>
		<div style="text-align: center;margin-top:30px;">
			<el-button style='margin:0 20px 0 0;' size="mini" type="primary" icon="document" @click="prev">返回上一页
			</el-button>
			<el-button size="mini" type="primary" icon="document" @click="artiReleass">保存
			</el-button>
		</div>
	</div>
</template>

<script>


	import {
		quillEditor
	} from 'vue-quill-editor'
	import 'quill/dist/quill.core.css'
	import 'quill/dist/quill.snow.css'
	import 'quill/dist/quill.bubble.css'
	import quillConfig from './quill-config.js'
	export default {
		name: 'news',
		components: {
			quillEditor
		},
		data() {
			return {
				type: '',
				typeId: '',
				title: '',
				merchants: '',
				originalPrice: '',
				price: '',
				memberPrice: '',
				coverImg: '',
				guige: '',
				commissionPrice: '',
				postagePrice: '',
				img: '',
				buyReason: '',
				sales: '',
				status: '',
				storeTypeId: '',
				storeTypeIds: '',
				storeId: '',
				brandId: '',
				brandIds: [],
				typeDatas: [],
				detailJson: [],
				imgs: [],
				value: [],
				potost: '',
				attr: [],
				statusmain2: [{
						value: 1,
						label: '上架'
					},
					{
						value: 2,
						label: '下架'
					}
				],
				isExpress: '',
				isExpressmain: [{
						value: 1,
						label: '普通商品需要发货'
					},
					{
						value: 2,
						label: '虚拟商品无需发货'
					},
					{
						value: 3,
						label: '门店核销无需发货'
					}
				],
				positage: 0,
				valueshow: true,
				valueshow1: true,
				browse: 0,
				priceshow: false,
				specifdata: [],
				ruleValue: [],
				headerData: [],
				valueData: [],
				sku: [],
				releobject: {
					value: '',
					detail: ''
				},
				isshow: true,
				ruleshow: false,
				inputValues: {
					inputValue0: '',
					inputValue1: '',
					inputValue2: '',
					inputValue3: '',
					inputValue4: '',
					inputValue5: ''
				},
				ruleId: '',
				specif: '',
				propshow: false,
				guigeshow: true,
				attrName: '',
				headerData2: [],
				attrValue: [],
				skuImg: '',
				isSelect: 0,
				homeGoods: 0,
				isRecommend: 0,
				isShow: false,
				hideUpload: false,
				limit: 1,
				hasChange: false,
				hasInit: false,
				tinymceId: 'tinymceId',
				height: 435,
				toolbar: [],
				menubar: 'file edit insert view format table',
				languageTypeList: {
					'en': 'en',
					'zh': 'zh_CN'
				},
				artiCletypes: [],
				// 富文本内容
				quillOption: quillConfig,
				descrition: '',
				curRowIndex: null,
				productionDate: '',
				expirationDate: '',
				imgType: 1,
				imgIndex: '',
				// 富文本组件配置
				// quillOption: {
				// 	theme: "snow", // 主题
				// 	placeholder: "",
				// 	modules: {
				// 		toolbar: {
				// 			container: [
				// 				["bold", "italic", "underline", "strike"],
				// 				["blockquote", "code-block"],
				// 				[{
				// 					header: 1
				// 				}, {
				// 					header: 2
				// 				}],
				// 				[{
				// 					list: "ordered"
				// 				}, {
				// 					list: "bullet"
				// 				}],
				// 				[{
				// 					script: "sub"
				// 				}, {
				// 					script: "super"
				// 				}],
				// 				[{
				// 					indent: "-1"
				// 				}, {
				// 					indent: "+1"
				// 				}],
				// 				[{
				// 					direction: "rtl"
				// 				}],
				// 				[{
				// 					size: ["small", false, "large", "huge"]
				// 				}],
				// 				[{
				// 					header: [1, 2, 3, 4, 5, 6, false]
				// 				}],
				// 				[{
				// 					color: []
				// 				}, {
				// 					background: []
				// 				}],
				// 				[{
				// 					font: []
				// 				}],
				// 				[{
				// 					align: []
				// 				}],
				// 				["clean", "link", "image", "video"],
				// 				["addBtn"], //新添加的工具
				// 			], // 自定义工具栏选项

				// 			handlers: {
				// 				addBtn: () => { //新增按钮绑定事件 自定义  调用vue实例上的method   这里注意名字要和上面新增按钮的名字一样
				// 					this.btnMethod();
				// 				},
				// 			},
				// 		},
				// 	},
				// 	initButton: function() {
				// 		//在使用的页面中初始化按钮样式
				// 		const addBtn = document.querySelector(".ql-addBtn");
				// 		addBtn.style.cssText =
				// 			"width:80px; border:none;padding-bottom:30px;font-size:10px;line-height: 26px;";
				// 		addBtn.className = "iconfont icon-reset";
				// 		addBtn.textContent = '图片库'
				// 	},
				// },
				// 图片库数据
				size: 10,
				page: 1,
				imageNames: '',
				typeDatasImg: {},
				imageUrlList: [],
				photos: [],
				centerDialogVisible:false,
				centerDialogVisible1:false,
				centerDialogVisible2:false,
			}
		},
		methods: {
			btnMethod() {
				this.imgIndex = 3
				this.centerDialogVisible1 = true
				this.dataSelectImg()

			},
			// 图标上传
			handleAvatarSuccess(file) {
				if (this.curRowIndex == 0) {
					this.skuImg = file.data;
					this.valueData[0].skuImg = this.skuImg
					return
				} else {
					this.skuImg = file.data;
					this.valueData[this.curRowIndex].skuImg = this.skuImg
				}
			},
			// 图标上传
			handleAvatarSuccess3(file) {
				this.skuImg = file.data;
				this.sku[0].skuImg = this.skuImg
			},
			// 是否包邮
			agreeChange(val) {
				if (val == 1) {
					this.priceshow = true
					this.postagePrice = ''
				} else {
					this.priceshow = false
					this.postagePrice = 0
				}
			},
			// 是否多规格
			guigeChange(val) {
				if (val == 0) {
					this.ruleId = 0
					this.dansku()
					this.valueshow1 = true
					this.valueshow = false
					this.guigeshow = false
					this.attr = []
					this.ruleshow = false
					this.ruleValue = []
					this.dansku()
				}
				if (val == 1) {
					this.ruleId = ''
					this.sku = []
					this.guigeshow = true
					this.valueshow1 = false
				}
			},
			// 获取单规格的sku
			dansku() {
				if (this.originalPrice == '') {
					this.originalPrice = '0'
				}
				if (this.price == '') {
					this.price = '0'
				}
				if (this.memberPrice == '') {
					this.memberPrice = '0'
				}
				this.$http({
					url: this.$http.adornUrl2('/goods/onlyFormatAttr'),
					method: 'get',
					params: this.$http.adornParams({
						'originalPrice': this.originalPrice,
						'price': this.price,
						'memberPrice': this.memberPrice,
						'coverImg': this.coverImg,
					})
				}).then(({
					data
				}) => {
					let returnData = data.data;
					this.sku = returnData.value
					this.headerData2 = returnData.header
					this.valueshow = false
					this.propshow = true
					this.guigeshow = false
				})
			},
			echo(goodsId) {
				this.$http({
					url: this.$http.adornUrl2('/goods/formatAttr'),
					method: 'get',
					params: this.$http.adornParams({
						'goodsId': goodsId,
					})
				}).then(({
					data
				}) => {
					let returnData = data.data;
					this.sku = returnData.value
					this.headerData2 = returnData.header
					this.valueshow = false
					this.propshow = true
					this.guigeshow = false
				})
			},
			handleChange1(value) {
				this.value.forEach(element => {
					this.typeId = element;
				});
			},
			// 获取品牌
			brandSelect() {
				let page = this.page - 1
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2('/selfGoodsBrand/result'),
					method: 'get',
					params: this.$http.adornParams({})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					this.brandIds = returnData
				})
			},
			// 删除
			clear(index) {
				this.imgs.splice(index, 1);
			},
			handleChange(file, fileList) {
				this.hideUpload = fileList.length >= this.limit;
			},
			handleRemove(file, fileList) {
				this.hideUpload = fileList.length >= this.limit;
			},
			handleAvatarSuccess1(file, fileList) {
				this.coverImg = file.data
			},
			//上传成功
			handleUploadSuccess(file, fileList) {
				this.photos.push(file.data)
			},
			init() {

				let id = this.$route.query.id
				this.$http({
					url: this.$http.adornUrl2('/goods/find'),
					method: 'get',
					params: this.$http.adornParams({
						'id': id
					})
				}).then(({
					data
				}) => {
					let returnData = data.data;
					let imgs = returnData.img.split(',')

					this.title = returnData.title
					this.id = returnData.id;
					this.imgs = imgs;
          this.photos = imgs
					this.buyReason = returnData.buyReason
					this.coverImg = returnData.coverImg
					this.title = returnData.title;
					this.commissionPrice = returnData.commissionPrice,
						this.originalPrice = returnData.originalPrice;
					this.typeId = returnData.typeId;
					this.storeId = returnData.storeId
					this.merchantId = returnData.merchantId
					this.merchants = returnData.merchants;
					this.price = returnData.price;
					this.sales = returnData.sales;
					this.brandId = returnData.brandId;
					this.storeTypeId = returnData.storeTypeId;
					this.memberPrice = returnData.memberPrice;
					this.descrition = returnData.descrition;
					this.productionDate = returnData.productionDate
					this.expirationDate = returnData.expirationDate
					this.status = returnData.status;
					this.createAt = returnData.createAt;
					this.isSelect = returnData.isSelect;
					this.homeGoods = returnData.homeGoods;
					this.isRecommend = returnData.isRecommend;
					this.isExpress = returnData.isExpress;
					this.createAt = returnData.createAt;
					this.browse = returnData.browse
					let label = JSON.parse(returnData.type.parentId)
					let value = JSON.parse(returnData.type.id)
					this.value = [label, value]
					this.postagePrice = returnData.postagePrice

					if (this.postagePrice > 0) {
						this.positage = 1
						this.priceshow = true
					} else {
						this.positage = 0
						this.priceshow = false
					}

					this.attr = returnData.attr
					if (this.attr.length > 0) {
						this.guige = 1
						this.Triggerselect()
						this.create2()
						this.attrName = returnData.attr[0].attrName
						this.attrValue = returnData.attr[0].attrValue
						this.ruleId = returnData.attr[0].ruleId
						this.goodsId = returnData.attr[0].goodsId
						this.id = returnData.attr[0].id
					} else {
						this.guige = 0
						this.echo(this.id)
						this.ruleId = 0
						this.sku = returnData.sku
					}
				})
			},
			// 返回上一级
			prev() {
				this.$router.back()
			},
			// 修改商品
			artiReleass() {
				let id = this.$route.query.id
				if (this.imgs == '') {
					this.img = this.potost
					this.img = this.img.substr(0, this.img.length - 1)
				} else {
					this.img = this.potost + this.imgs
				}
				if (this.sku.length == 0) {
					this.$notify({
						title: '提示',
						duration: 1800,
						message: '请选择商品规格',
						type: 'warning'
					});
					return
				} else {
					this.$http({
						url: this.$http.adornUrl2('/goods/update'),
						method: 'post',
						data: this.$http.adornData({
							'id': id,
							'img': this.photos.toString(),
							'title': this.title,
							'typeId': this.typeId,
							'merchants': this.merchants,
							'originalPrice': this.originalPrice,
							'price': this.price,
							'memberPrice': this.memberPrice,
							'sales': this.sales,
							'storeId': this.storeId,
							'merchantId': this.merchantId,
							'status': this.status,
							'storeTypeId': this.storeTypeId,
							'coverImg': this.coverImg,
							'descrition': this.descrition,
							'brandId': this.brandId,
							'commissionPrice': this.commissionPrice,
							'isSelect': this.isSelect,
							'homeGoods': this.homeGoods,
							'buyReason': this.buyReason,
							'isRecommend': this.isRecommend,
							'isExpress': this.isExpress,
							'postagePrice': this.postagePrice,
							'sku': this.sku,
							'attr': this.attr,
							'createAt': this.createAt,
							'browse': this.browse,
							'productionDate': this.productionDate,
							'expirationDate': this.expirationDate
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
									this.$router.push({
										path: '/merchIncome',
										query: {
											merchantId: this.merchantId,
											storeId: this.storeId
										}
									})
								}
							})
						} else {
							this.$message({
								message: data.msg,
								type: 'warning',
								duration: 1500,
								onClose: () => {}
							})
						}

					})
				}

			},

			// 店铺分类
			commerSelect() {
				let storeId = this.$route.query.storeId
				this.tableDataLoading = true
				this.$http({
					url: this.$http.adornUrl2('/self/storeType/result'),
					method: 'get',
					params: this.$http.adornParams({
						'storeId': storeId
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading = false
					let returnData = data.data;
					this.storeTypeIds = returnData
				})
			},
			// 商品规格
			specifSelect() {
				let storeId = this.$route.query.storeId
				this.$http({
					url: this.$http.adornUrl2('/goods/info'),
					method: 'get',
					params: this.$http.adornParams({
						'storeId':storeId
					})
				}).then(({
					data
				}) => {
					let returnData = data.data;
					this.specifdata = returnData;
				})
			},
			// 商品规格
			Triggerselect() {
				let id = this.$route.query.id
				this.$http({
					url: this.$http.adornUrl2('/goods/findAttrValue'),
					method: 'get',
					params: this.$http.adornParams({
						'goodsId': id
					})
				}).then(({
					data
				}) => {
					let returnData = data.data;
					this.ruleValue = returnData.attrValue
					this.attrName = returnData.attrName
					this.ruleshow = true
				})
			},
			// 删除商品属性
			deleter(index) {
				let length = this.sku.length
				if (length == 1) {
					this.$message({
						title: '提示',
						type: 'error',
						duration: 1800,
						message: '属性至少要有一个',
						type: 'warning'
					});
					return
				} else {
					this.sku.splice(index, 1);
				}
			},
			// 查询商品规格
			selectTrigger(val) {
				this.$http({
					url: this.$http.adornUrl2('/selfGoodsRule/find'),
					method: 'get',
					params: this.$http.adornParams({
						'id': val
					})
				}).then(({
					data
				}) => {
					let returnData = data.data;
					this.ruleValue = returnData.ruleValue
					this.attrName = returnData.ruleName
					this.ruleshow = true
					this.ruleId = returnData.id
					this.create()
				})
			},
			deleterule(index) {
				this.ruleValue.splice(index, 1);
			},
			btnToclick() {
				this.isshow = false
				this.releobject.value = ''
				this.releobject.detail = ''
			},
			speciTotwo() {
				this.isshow = true
				this.releobject.value = ''
				this.releobject.detail = ''
			},
			speciTo() {
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
				this.isshow = true
				this.releobject.value = ''
				this.releobject.detail = ''
			},
			handleClose(tag, j, item) {
				var detailarr = item.detail.split(',')
				detailarr.splice(detailarr.indexOf('tag'), 1);
				if (detailarr.length < 1) {
					this.$message({
						title: '提示',
						type: 'error',
						duration: 1800,
						message: '规格值至少要有一个',
						type: 'warning'
					});
					return
				} else {
					item.detail = String(detailarr)
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
			// 初始化立即生成
			create2() {
				let id = this.$route.query.id
				this.$http({
					url: this.$http.adornUrl2('/goods/formatAttr'),
					method: 'get',
					params: this.$http.adornParams({
						'goodsId': id
					})
				}).then(({
					data
				}) => {
					let returnData = data.data;
					this.headerData = returnData.header
					this.valueData = returnData.value
					this.valueshow = true
					this.valueshow1 = false
					this.sku = returnData.value
					this.detailJson = returnData.value[0].detailJson.split(',')
				})
			},
			// 立即生成
			create() {
				this.attr = []
				this.attr.push({
					attrName: this.attrName,
					attrValue: this.ruleValue,
					ruleId: this.ruleId
				})
				if (this.originalPrice == '') {
					this.originalPrice = '0'
				}
				if (this.price == '') {
					this.price = '0'
				}
				if (this.memberPrice == '') {
					this.memberPrice = '0'
				}
				this.$http({
					url: this.$http.adornUrl2(
						`/goods/isFormatAttr?coverImg=${this.coverImg}&originalPrice=${this.originalPrice}&price=${this.price}&memberPrice=${this.memberPrice}`
					),
					method: 'post',
					data: this.attr[0]
				}).then(({
					data
				}) => {
					let returnData = data.data;
					this.headerData = returnData.header
					this.valueData = returnData.value
					this.valueshow = true
					this.valueshow1 = false
					this.sku = returnData.value
					this.detailJson = returnData.value[0].detailJson.split(',')
				})
			},
			// 商品分类
			dataSelect() {
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
					this.typeDatas = returnData
				})
			},
			// 商城图片弹框
			imgBtn(index, ind) {
				this.curRowIndex = ind
				this.imgIndex = index
				this.centerDialogVisible = true
			},
			// 图片库选项弹框
			imageBtn() {
				if (this.imgIndex == 1 || this.imgIndex == 5 || this.imgIndex == 6) {
					this.centerDialogVisible2 = true
				} else {
					this.centerDialogVisible1 = true
				}
				this.dataSelectImg()
			},
			// 删除轮播图
			dels(index) {
				this.photos.splice(index, 1);
				console.log(this.photos)
			},
			// 图片库数据
			handleSizeChange(val) {
				this.size = val;
				this.dataSelectImg()
			},
			handleCurrentChange(val) {
				this.page = val;
				this.dataSelectImg()
			},

			// 获取图片数据
			dataSelectImg() {
				this.tableDataLoading4 = true
				this.$http({
					url: this.$http.adornUrl2('/image/selectImageGalleryList'),
					method: 'get',
					params: this.$http.adornParams({
						'page': this.page - 1,
						'size': this.size,
						'imageName': this.imageNames
					})
				}).then(({
					data
				}) => {
					this.tableDataLoading4 = false
					let returnData = data.data
					this.typeDatasImg = returnData
				})
			},
			select() {
				this.page = 1
				this.dataSelectImg()
			},
			clear() {
				this.page = 1
				this.imageNames = ''
				this.dataSelectImg()
			},
			// 点击确认
			querenBtn(row) {
				if (this.imgIndex == 1) {
					this.coverImg = row.imageUrl
					this.centerDialogVisible = false
					this.centerDialogVisible2 = false
				}
				if (this.imgIndex == 5) {
					this.sku[0].skuImg = row.imageUrl
					this.centerDialogVisible = false
					this.centerDialogVisible2 = false
				}
				if (this.imgIndex == 6) {
					this.valueData[this.curRowIndex].skuImg = row.imageUrl
					this.centerDialogVisible = false
					this.centerDialogVisible2 = false
				}

			},
			// 选择图片
			changeFun(val) {
				console.log('多选', val)
				var imageUrlList = []
				for (var i in val) {
					imageUrlList.push(val[i].imageUrl)

				}
				this.imageUrlList = imageUrlList
				console.log('imageUrlList', imageUrlList)

			},
			// 多图确认
			querenBtnDuo() {
				console.log('this.imgIndex', this.imgIndex)
				if (this.imgIndex == 3) {
					for (var i in this.imageUrlList) {
						console.log('富文本图片····', i)
						var divP = '<p><img src="' + this.imageUrlList[i] + '" /></p>'
						this.descrition += divP
						console.log('this.descrition', this.descrition, '```', divP)
					}

				} else {

					for (var i in this.imageUrlList) {
						this.photos.push(this.imageUrlList[i])
					}
				}

				this.centerDialogVisible = false
				this.centerDialogVisible1 = false
				this.imageUrlList = []
			}
		},
		mounted() {
			this.init()
			this.dataSelect()
			this.specifSelect()
			this.brandSelect()
			this.commerSelect()
			// this.quillOption.initButton();
		},

	}
</script>

<style scoped="scoped">
	.chesks .el-checkbox__label{
		width: 100%!important;
	}
	.main-cont {
		max-width: 100%;
		min-width: 80%;
		padding-bottom: 5%;
		background-color: #fff;
	}

	.tinymce-container {
		position: relative;
		line-height: normal;
	}

	.tinymce-container>>>.mce-fullscreen {
		z-index: 10000;
	}

	.tinymce-textarea {
		visibility: hidden;
		z-index: -1;
	}

	.hide .el-upload--picture-card {
		display: none;
	}

	.margin15 {
		margin-right: 15px;
		margin-top: 10px;
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

	.ruleitem {
		padding-left: 10px;
		margin-top: 15px;
	}

	.ruleitem .ruleicon {
		position: relative;
		top: 3px;
		left: 2px;
		width: 1.2em;
		height: 1.2em;
	}

	.el-tag+.el-tag {
		margin-left: 10px;
	}

	.button-new-tag {
		height: 32px;
		line-height: 31px;
		padding-top: 0;
		padding-bottom: 0;
		border-radius: 4px;
		border-top-left-radius: 0;
		border-bottom-left-radius: 0;
		background-color: #f5f7fa;
		position: relative;
		left: -5px;
		border-color: #dcdfe6;
		border: 1px solid #dcdfe6;
	}

	.input-new-tag {
		width: 90px;
		margin-left: 10px;
		vertical-align: bottom;
	}

	.el-input--small .el-input__inner {
		border-top-right-radius: 0;
		border-bottom-right-radius: 0;
	}

	.el-table--enable-row-transition .el-table__body td {
		text-align: center;
	}

	.imgWrap .avatar-uploader .el-upload {
		width: 60px;
	}

	.el-table .cell {
		text-align: center !important;
	}

	.el-tag--medium {
		margin-right: 10px;
	}
  .imgs {
  	position: relative;
  	border-radius: 6px;
  	width: 148px;
  	height: 148px;
  	margin-right: 10px;
  	display: inline-block;
  }

  .dels {
  	position: absolute;
  	top: 0;
  	left: 0;
  	display: none;
  }

  .dels .el-icon-delete {
  	line-height: 148px;
  	padding-left: 58px;
  	font-size: 25px;
  	color: #fff;
  }

  .imgs:hover .dels {
  	width: 100%;
  	height: 100%;
  	background: #000;
  	display: block;
  	opacity: 0.5;
  }
</style>
