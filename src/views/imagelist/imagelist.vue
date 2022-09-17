<template>
	<div>
		<div style="margin-right:2%;display: inline-block;">
			<span>图片名称：</span>
			<el-input v-model="imageNames" @keydown.enter.native="select" placeholder="请输入图片名称" style="width:200px;">
			</el-input>&nbsp;&nbsp;&nbsp;
			<el-button icon="document" size="mini" style="margin: 10px 0;" type="primary" @click="select">查询</el-button>
			&nbsp;&nbsp;&nbsp;
			<el-button icon="document" size="mini" style="margin: 10px 0;" type="primary" @click="clear">重置</el-button>
		</div>
		<div style="float: right;margin-right:2%;">
			<el-button :disabled="!isAuth('imagelist:add')" icon="document" size="mini" style="margin: 10px 0;"
				type="primary" @click="addNotice">添加图片</el-button>
		</div>
		<div>
			<div v-for="(item,index) in typeDatas.content" :key="index"
				style="display: inline-block;margin: 2%;width: 16%;position: relative;">
				<el-popover placement="top-start" title="" trigger="hover">
					<img style="width: 100%; height: 200px;border-radius: 10px;object-fit: cover;" :src="item.imageUrl" alt=""
						slot="reference" @click="updates(item)">
					<img style="width: 300px; height: auto" :src="item.imageUrl" alt="">
				</el-popover>
				<div class="deletesB" style="" :disabled="!isAuth('imagelist:delete')" @click="deletes(item)">
					<!-- <icon-svg name="shanchu" class="el-icon-setting" style="font-size: 18px;"></icon-svg> -->
					<img src="../../assets/img/del.png" style="width: 100%;height: 100%;"/>
				</div>
				<div class="names" style="text-align: center;margin-top: 10px;color: #666;overflow:hidden;white-space: nowrap;text-overflow: ellipsis;-o-text-overflow:ellipsis;cursor: pointer;" @click="updates(item)">{{item.imageName}}
					<img src="../../assets/img/update.png" style="width: 15px;height: 15px;position: absolute;right: 0;"/>
				</div>
			</div>
		</div>
		<!-- <el-table v-loading="tableDataLoading" :data="typeDatas.content" row-key="id">
			<el-table-column label="编号" prop="imageId" width="80">
			</el-table-column>
			<el-table-column label="图片" prop="imageUrl">
				<template slot-scope="scope">
					<img :src="scope.row.imageUrl" alt="" height="40" width="40">
				</template>
			</el-table-column>
			<el-table-column label="名称" prop="imageName">
			</el-table-column>
			<el-table-column label="创建时间" prop="createTime">
			</el-table-column>
			<el-table-column label="操作" width="180">
				<template slot-scope="scope">
					<el-button :disabled="!isAuth('imagelist:update')" size="mini" type="primary"
						@click="updates(scope.$index, scope.row)">修改
					</el-button>
					<el-button :disabled="!isAuth('imagelist:delete')" size="mini" type="danger"
						@click="deletes(scope.row)">删除
					</el-button>
				</template>
			</el-table-column>
		</el-table> -->
		<div style="text-align: center;margin-top: 10px;">
			<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange"
				:page-sizes="[10, 20, 30,50,100]" :page-size="size" :current-page="page"
				layout="total,sizes, prev, pager, next,jumper" :total="typeDatas.totalElements">
			</el-pagination>
		</div>
		<!-- 添加图片 -->
		<el-dialog :visible.sync="dialogFormVisible" center title="添加图片">
			<div style="margin-bottom: 10px;display:flex;">
				<span style="width: 200px;display: inline-block;text-align: right;">图片：</span>
				<div
					style=" width:148px;height:148px;border: 1px dashed #c0ccda;border-radius: 6px;text-align: center;line-height: 148px;">
					<el-upload v-model="imageUrl" :on-success="handleAvatarSuccess" :show-file-list="false"
						action="https://tbshopsadmin.hnqxkjfz.com/tao/alioss/upload" class="avatar-uploader">
						<img v-if="imageUrl" :src="imageUrl" class="avatar"
							style="border-radius: 6px;width: 148px;height: 148px;" />
						<i v-else class="el-icon-plus avatar-uploader-icon"></i>
					</el-upload>
				</div>
			</div>
			<div style="margin-bottom: 10px;">
				<span style="width: 200px;display: inline-block;text-align: right;">图片名称：</span>
				<el-input v-model="imageName" placeholder="请输入图片名称" style="width:50%;"></el-input>
			</div>
			<div slot="footer" class="dialog-footer">
				<el-button @click="dialogFormVisible = false">取 消</el-button>
				<el-button type="primary" @click="releasNoticeTo()">确 定</el-button>
			</div>
		</el-dialog>
		<!-- 修改图片 -->
		<el-dialog :visible.sync="dialogFormVisible1" center title="修改图片">
			<el-form :model="form">
				<el-form-item :label-width="formLabelWidth" label="图片名称：">
					<el-input v-model="form.imageName" placeholder="请输入图片名称" style="width:65%;"></el-input>
				</el-form-item>
				<el-form-item :label-width="formLabelWidth" label="图标：">
					<div
						style=" width:148px;height:148px;border: 1px dashed #c0ccda;border-radius: 6px;text-align: center;line-height: 148px;">
						<el-upload v-model="form.imageUrl" :on-success="handleAvatarSuccess2" :show-file-list="false"
							action="https://tbshopsadmin.hnqxkjfz.com/tao/alioss/upload" class="avatar-uploader">
							<img v-if="form.imageUrl" :src="form.imageUrl" class="avatar"
								style="border-radius: 6px;width: 148px;height: 148px;" />
							<i v-else class="el-icon-plus avatar-uploader-icon"></i>
						</el-upload>
					</div>
				</el-form-item>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click="dialogFormVisible1 = false">取 消</el-button>
				<el-button type="primary" @click="amendNoticeTo()">确 定</el-button>
			</div>
		</el-dialog>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				size: 30,
				page: 1,
				imageNames: '',
				imageName: '',
				imageUrl: '',
				imageId: '',
				form: {
					imageName: '',
					imageUrl: '',
					imageId: '',
					sort: '',
					createTime: '',
				},
				formLabelWidth: '200px',
				tableDataLoading: false,
				dialogFormVisible1: false,
				dialogFormVisible: false,
				typeDatas: {},
			}
		},
		methods: {
			handleSizeChange(val) {
				this.size = val;
				this.dataSelect()
			},
			handleCurrentChange(val) {
				this.page = val;
				this.dataSelect()
			},
			// 图标上传
			handleAvatarSuccess(file) {
				this.imageUrl = file.data
			},
			// 图标上传
			handleAvatarSuccess2(file) {
				this.form.imageUrl = file.data
			},
			// 添加图片弹框
			addNotice() {
				this.imageName = ''
				this.imageUrl = ''
				this.imageId = ''
				this.dialogFormVisible = true
			},
			// 添加图片
			releasNoticeTo() {
				if (this.imageName == '') {
					this.$notify({
						title: '提示',
						duration: 1800,
						message: '请输入图片名称',
						type: 'warning'
					})
					return
				}
				if (this.imageUrl == '') {
					this.$notify({
						title: '提示',
						duration: 1800,
						message: '请上传图片',
						type: 'warning'
					})
					return
				}
				this.$http({
					url: this.$http.adornUrl2('/image/insertImageGallery'),
					method: 'post',
					data: this.$http.adornData({
						'imageId': this.imageId,
						'imageName': this.imageName,
						'imageUrl': this.imageUrl
					})
				}).then(({
					data
				}) => {
					if (data.status == 0) {
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
						this.$message({
							message: data.msg,
							type: 'warning',
							duration: 1500,
							onClose: () => {}
						})
					}
				})
			},
			// 修改弹框
			updates(rows) {
				this.dialogFormVisible1 = true
				this.form.imageName = rows.imageName
				this.form.imageId = rows.imageId
				this.form.sort = rows.sort
				this.form.createTime = rows.createTime
				this.form.imageUrl = rows.imageUrl
			},
			// 修改商品类别
			amendNoticeTo() {
				this.$http({
					url: this.$http.adornUrl2('/image/insertImageGallery'),
					method: 'post',
					data: this.$http.adornData({
						'imageName': this.form.imageName,
						'imageUrl': this.form.imageUrl,
						'imageId': this.form.imageId,
						'createTime': this.form.createTime

					})
				}).then(({
					data
				}) => {
					if (data.status == 0) {
						this.dialogFormVisible1 = false
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
							type: 'warning',
							duration: 1500,
							onClose: () => {}
						})
					}

				})
			},
			// 删除图片
			deletes(row) {
				this.$confirm(`确定删除该图片?删除后无法恢复！`, '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					this.$http({
						url: this.$http.adornUrl2('/image/deleteImageGallery'),
						method: 'post',
						params: this.$http.adornParams({
							'imageId': row.imageId
						})
					}).then(({
						data
					}) => {
						if (data.status == 0) {
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
								type: 'warning',
								duration: 1500,
								onClose: () => {}
							})
						}

					})
				}).catch(() => {})

			},

			// 获取图片数据
			dataSelect() {
				this.tableDataLoading = true
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
					this.tableDataLoading = false
					let returnData = data.data
					this.typeDatas = returnData
				})
			},
			select() {
				this.page = 1
				this.dataSelect()
			},
			clear() {
				this.page = 1
				this.imageNames = ''
				this.dataSelect()
			}
		},
		mounted() {
			this.dataSelect()
		}
	}
</script>

<style>
	.deletesB {
		position: absolute;
		right: -10px;
		top: -10px;
		/* padding: 5px; */
		/* background: #efefef; */
		/* border: 1px solid #999; */
		border-radius: 50%;
		height: 30px;
		width: 30px;
		opacity: 0.8;
	}
	.names{
		position: relative;
	}
	.names img{
		display: none ;
	}
	.names:hover img{
		display: revert;
	}
</style>
