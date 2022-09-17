<template>
  <div>
    <div style="margin:2% 0;display: inline-block;">
      <span>店铺名称：</span>
      <el-input style="width: 160px;" @keydown.enter.native="select" clearable placeholder="请输入店铺名称" v-model="storeName"></el-input>&nbsp;&nbsp;&nbsp;&nbsp;
	  <span>状态：</span>
	  <el-select v-model="status" placeholder="请选择店铺类型" style="width:150px;" @change="selectTrigger(status)">
	    <el-option v-for="item in statusnum" :key="item.value" :label="item.label" :value="item.value">
	    </el-option>
	  </el-select>
	</div>
    <div style="display: inline-block;">
      <el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="select">查询</el-button>
      <el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="cleans">重置</el-button>
    </div>
    <el-table
      v-loading="tableDataLoading"
      :data="tableData.content">
	  <el-table-column
	    fixed
	    prop="id"
	    label="id"
	    width="80">
	  </el-table-column>
	  <el-table-column
	    prop="storeName"
	    label="店铺名称"
	    width="120">
	  </el-table-column>
	  <el-table-column
	    prop="nickName"
	    label="用户昵称">
		<template slot-scope="scope">
			<span>{{scope.row.nickName ? scope.row.nickName : '无'}}</span>
		</template>
	  </el-table-column>
	  <el-table-column
	    prop="phone"
	    label="手机号">
	  </el-table-column>
      <el-table-column
        prop="img"
        label="投诉图片">
		<template slot-scope="scope">
		  <div v-if="scope.row.img == null || scope.row.img == ''">
		    暂无图片
		  </div>
		  <div v-else-if="scope.row.img.includes(',')" style="display:flex;flex-wrap: wrap;">
		     <div v-for="item in scope.row.img.split(',')" style="margin: 2px;">
		       <el-popover placement="top-start" title="" trigger="hover">
		           <img style="width: 50px; height: 50px" :src="item" alt="" slot="reference">
		           <img style="width: 200px; height: 200px" :src="item" alt="">
		        </el-popover>
		     </div>
		  </div>
		  <div v-else>
		    <el-popover placement="top-start" title="" trigger="hover">
		        <img style="width: 50px; height: 50px" :src="scope.row.img" alt="" slot="reference">
		        <img style="width: 200px; height: 200px" :src="scope.row.img" alt="">
		     </el-popover>
		  </div>
		</template>
      </el-table-column>
      <el-table-column
        prop="content"
        label="投诉内容">
			<template slot-scope="scope">
				<span>{{scope.row.content}}</span>
			</template>
      </el-table-column>
		<el-table-column
			prop="refundReason"
			label="驳回原因">
			<template slot-scope="scope">
				<span>{{scope.row.refundReason ? scope.row.refundReason : '未驳回'}}</span>
			</template>
		</el-table-column>
		<el-table-column
			prop="status"
			label="状态">
			<template slot-scope="scope">
				<span style="color: #3E8EF7;" v-if="scope.row.status == 1">待处理</span>
				<span v-if="scope.row.status == 2">投诉驳回</span>
				<span v-if="scope.row.status == 3">商家处理中</span>
				<span v-if="scope.row.status == 4">商家已处理</span>
				<span v-if="scope.row.status == 5">总后台已处理</span>
			</template>
		</el-table-column>
		<el-table-column
			prop="createTime"
			label="创建时间"
			width="160">
		</el-table-column>
		<el-table-column
	    label="操作"
	    width="100"
		fixed="right">
			<template slot-scope="scope">
				<el-button
					size="mini"
					type="primary"
					:disabled="!isAuth('complaint:dispose')"
					v-if="scope.row.status == 1 || scope.row.status == 4"
					@click="merchLook(scope.row)">处理
				</el-button>
				<el-button
					v-else
					size="mini"
					type="primary"
					disabled>处理
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
        :total="tableData.totalElements">
      </el-pagination>
    </div>
	<!-- 处理投诉 -->
	<el-dialog title="处理投诉" :visible.sync="dialogFormVisible" center>
		<div style="margin-bottom: 10px;">
			<span style="width: 200px;display: inline-block;text-align: right;">状态：</span>
			<el-radio-group v-model="status" @change="rejectChange">
			    <el-radio :label="2">投诉驳回</el-radio>
			    <el-radio :label="3">商家去处理</el-radio>
			</el-radio-group>
		</div>
		<div style="margin-bottom: 10px;" v-if="isshow">
			<span style="width: 200px;display: inline-block;text-align: right;position: relative;top: -65px;">驳回原因：</span>
			<el-input style="width:50%;" type="textarea" :rows="4" v-model="refundReason" placeholder="请输入驳回原因"></el-input>
		</div>
		<div style="margin-bottom: 10px;" v-if="!isshow">
			<span style="width: 200px;display: inline-block;text-align: right;">商家处理：</span>
			<span>发送给商家处理</span>
		</div>
	  <div slot="footer" class="dialog-footer">
	    <el-button @click="dialogFormVisible = false">取 消</el-button>
	    <el-button type="primary" @click="releasNoticeTo()">确 定</el-button>
	  </div>
	</el-dialog>
	<!-- 处理投诉 -->
	<el-dialog title="处理投诉" :visible.sync="dialogFormVisible1" center>
		<div style="margin-bottom: 10px;">
			<span style="width: 200px;display: inline-block;text-align: right;">状态：</span>
			<el-radio-group v-model="status">
			    <el-radio :label="3">继续处理</el-radio>
			    <el-radio :label="5">审核通过</el-radio>
			</el-radio-group>
		</div>
	  <div slot="footer" class="dialog-footer">
	    <el-button @click="dialogFormVisible1 = false">取 消</el-button>
	    <el-button type="primary" @click="releasNoticeTo2()">确 定</el-button>
	  </div>
	</el-dialog>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        size: 5,
        page: 1,
		id:'',
        storeName:'',
		isshow:false,
		status:0,
		refundReason:'',
		statusnum: [
		  {
		    value: 0,
		    label: '全部'
		  },
		  {
		    value: 1,
		    label: '待处理'
		  }, {
		    value: 2,
		    label: '投诉驳回'
		  }, {
		    value: 3,
		    label: '商家处理中'
		  }, {
		    value: 4,
		    label: '商家已处理'
		  },
		  {
		    value: 5,
		    label: '总后台已处理'
		  }
		],
        formLabelWidth:'200px',
        tableDataLoading: true,
        dialogFormVisible:false,
		dialogFormVisible1:false,
        tableData: [],
      }
    },
    methods: {
		// 驳回
		rejectChange(val){
			if(val == 2){
				this.isshow = true
			}else{
				this.isshow = false
			}
		},
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
		this.status = 0
        this.storeName = ''
        this.dataSelect()
      },
	  // 处理
	  merchLook(row){
		if(row.status == 1){
			this.status = 2
			this.isshow = true
			this.dialogFormVisible = true 
			this.id = row.id
		}else{
			this.status = 3
			this.dialogFormVisible1 = true 
			this.id = row.id
		}
	  },
	  // 处理
	  releasNoticeTo(){
		if(this.status == 2){
			if(this.refundReason == ''){
				this.$message({
				  message: '请输入驳回原因',
				  type: 'error',
				  duration: 1500
				})
				return
			}
			this.$http({
			  url: this.$http.adornUrl2('/self/storeComplain/deal'),
			  method: 'get',
			  params: this.$http.adornParams({
			    'id':this.id,
			    'refundReason': this.refundReason,
				'status':this.status
			  })
			}).then(({data}) => {
			  if(data.status == 0){
				  this.$message({
						message: '处理成功',
						type: 'success',
						duration: 1500,
						onClose: () => {
						this.dialogFormVisible = false
						 this.status = 0
						 this.dataSelect()
						}
				  })
			  }
			})  
		}else{
			this.$http({
			  url: this.$http.adornUrl2('/self/storeComplain/deal'),
			  method: 'get',
			  params: this.$http.adornParams({
			    'id':this.id,
				'status':this.status
			  })
			}).then(({data}) => {
			  if(data.status == 0){
				  this.$message({
					message: '处理成功',
					type: 'success',
					duration: 1500,
					onClose: () => {
					this.dialogFormVisible = false
					 this.status = 0
					 this.dataSelect()
					}
				  })
			  }
			})  
		}
		
	  },
	  // 处理
	  releasNoticeTo2(){
	  	this.$http({
	  	  url: this.$http.adornUrl2('/self/storeComplain/deal'),
	  	  method: 'get',
	  	  params: this.$http.adornParams({
	  	    'id':this.id,
	  		'status':this.status
	  	  })
	  	}).then(({data}) => {
	  	  if(data.status == 0){
	  		  this.$message({
				message: '处理成功',
				type: 'success',
				duration: 1500,
				onClose: () => {
				this.dialogFormVisible1 = false
				 this.status = 0
				 this.dataSelect()
				}
	  		  })
	  	  }
	  	}) 	
	  },
      // 获取数据列表
      dataSelect () {
        let page = this.page-1
        this.tableDataLoading = true
        this.$http({
          url: this.$http.adornUrl2('/self/storeComplain/list'),
          method: 'get',
          params: this.$http.adornParams({
            'page':page,
            'size':this.size,
            'storeName': this.storeName,
			'status':this.status
          })
        }).then(({data}) => {
          this.tableDataLoading = false
          let returnData = data.data
          this.tableData = returnData
        })
      }
    },
    mounted () {
      this.dataSelect()
    }
  }
</script>

<style scoped="scoped">
  .el-button + .el-button {
    margin-left: 0 !important;
    margin-top: 5px !important;
  }
</style>
