<template>
  <div>
    <div style="margin:2% 0;display: inline-block;">
      <span>店铺名称：</span>
      <el-input style="width: 160px;" @keydown.enter.native="select" clearable placeholder="请输入店铺名称"
        v-model="storeName"></el-input>&nbsp;&nbsp;&nbsp;&nbsp;
    </div>
    <div style="display: inline-block;">
      <el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="select">查询</el-button>
      <el-button style='margin-left:15px;' size="mini" type="primary" icon="document" @click="cleans">重置</el-button>
    </div>
    <el-table v-loading="tableDataLoading" :data="tableData.content">
      <el-table-column fixed prop="storeName" label="店铺名称" width="120">
      </el-table-column>
      <el-table-column prop="logo" label="店铺logo">
        <template slot-scope="scope">
          <span v-if="scope.row.logo == null">暂无图片</span>
          <img v-else :src="scope.row.logo" alt="" width="40" height="40">
        </template>
      </el-table-column>
      <el-table-column prop="shipAddress" label="店铺发货地址">
        <template slot-scope="scope">
          <span>{{scope.row.shipAddress ? scope.row.shipAddress : '未绑定'}}</span>
        </template>
      </el-table-column>
      <el-table-column prop="refundAddress" label="店铺退货地址">
        <template slot-scope="scope">
          <span>{{scope.row.refundAddress ? scope.row.refundAddress : '未绑定'}}</span>
        </template>
      </el-table-column>
      <el-table-column prop="refundContact" label="店铺退货收货人">
        <template slot-scope="scope">
          <span>{{scope.row.refundContact ? scope.row.refundContact : '未绑定'}}</span>
        </template>
      </el-table-column>
      <el-table-column prop="refundMobile" label="店铺退货手机号">
        <template slot-scope="scope">
          <span>{{scope.row.refundMobile ? scope.row.refundMobile : '未绑定'}}</span>
        </template>
      </el-table-column>
      <el-table-column prop="score" label="店铺评分">
      </el-table-column>
      <el-table-column prop="banStatus" label="状态" width="80">
        <template slot-scope="scope">
          <span style="color: #3E8EF7;" v-if="scope.row.banStatus == 1">正常</span>
          <span style="color: #F56C6C;" v-if="scope.row.banStatus == 2">封禁</span>
        </template>
      </el-table-column>
      <el-table-column prop="createTime" label="创建时间">
      </el-table-column>
      <el-table-column label="操作" width="100" fixed="right">
        <template slot-scope="scope">
          <el-button size="mini" type="primary" @click="merchLook(scope.row)">查看
          </el-button>
          <el-button size="mini" type="primary" @click="merchDetails(scope.row)">详情
          </el-button>
          <el-button size="mini" type="primary" v-if="scope.row.banStatus == 2"
            :disabled="!isAuth('merchantList:archive')" @click="deblocking(scope.row)">解封
          </el-button>
          <el-button size="mini" type="danger" :disabled="!isAuth('merchantList:close')" v-if="scope.row.banStatus == 1"
            @click="deblocking(scope.row)">封禁
          </el-button>
        </template>
      </el-table-column>
    </el-table>
    <div style="text-align: center;margin-top: 10px;">
      <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :page-sizes="[10, 20, 30, 50, 100]"
        :page-size="size" :current-page="page" layout="total,sizes, prev, pager, next" :total="tableData.totalElements">
      </el-pagination>
    </div>
    <!-- 查看弹框-->
    <el-dialog title="查看商家" :visible.sync="dialogFormVisible" center>
      <el-form :model="form">
        <el-form-item label="店铺名称：" :label-width="formLabelWidth">
          <el-input v-model="form.storeName" v-if="isshop" disabled style="width:65%;"></el-input>
          <el-input v-model="form.storeName" v-if="!isshop" style="width:65%;"></el-input>
        </el-form-item>
        <el-form-item label="店铺logo：" :label-width="formLabelWidth">
          <img v-if="isshop" :src="form.logo" class="avatar" style="border-radius: 6px;width: 148px;height: 148px;" />
          <div v-if="!isshop"
            style=" width:148px;height:148px;border: 1px dashed #c0ccda;border-radius: 6px;text-align: center;line-height: 148px;">
            <el-upload class="avatar-uploader" v-model="form.logo"
              action="https://tbshopsadmin.hnqxkjfz.com/tao/alioss/upload" :show-file-list="false"
              :on-success="handleAvatarSuccess">
              <img v-if="form.logo" :src="form.logo" class="avatar"
                style="border-radius: 6px;width: 148px;height: 148px;" />
              <i v-else class="el-icon-plus avatar-uploader-icon"></i>
            </el-upload>
          </div>
        </el-form-item>
        <el-form-item label="店铺发货地址：" :label-width="formLabelWidth">
          <el-input v-model="form.shipAddress" v-if="isshop" disabled style="width:65%;"></el-input>
          <el-input v-model="form.shipAddress" v-if="!isshop" style="width:65%;"></el-input>
        </el-form-item>
        <el-form-item label="店铺退货地址：" :label-width="formLabelWidth">
          <el-input v-model="form.refundAddress" v-if="isshop" disabled style="width:65%;"></el-input>
          <el-input v-model="form.refundAddress" v-if="!isshop" style="width:65%;"></el-input>
        </el-form-item>
        <el-form-item label="店铺退货收货人：" :label-width="formLabelWidth">
          <el-input v-model="form.refundContact" v-if="isshop" disabled style="width:65%;"></el-input>
          <el-input v-model="form.refundContact" v-if="!isshop" style="width:65%;"></el-input>
        </el-form-item>
        <el-form-item label="店铺退货手机号：" :label-width="formLabelWidth">
          <el-input v-model="form.refundMobile" v-if="isshop" disabled style="width:65%;"></el-input>
          <el-input v-model="form.refundMobile" v-if="!isshop" style="width:65%;"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer" v-if="!isshop">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="disposeNoticeTo()">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        size: 10,
        page: 1,
        storeName: '',
        banStatus: '',
        form: {
          storeId: '',
          merchantId: '',
          storeName: '',
          logo: '',
          shipAddress: '',
          refundAddress: '',
          refundContact: '',
          refundMobile: '',
          createTime: ''
        },
        isshop: true,
        formLabelWidth: '200px',
        createTimeStart: '',
        createTimeEnd: '',
        refundReason: '',
        auditTime: '',
        auditYears: '',
        isshow: true,
        tableDataLoading: true,
        dialogFormVisible: false,
        tableData: [],
      }
    },
    methods: {
      handleSizeChange(val) {
        this.size = val
        this.dataSelect()
      },
      handleCurrentChange(val) {
        this.page = val
        this.dataSelect()
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
        this.dataSelect()
      },
      // 图标上传
      handleAvatarSuccess(file) {
        this.form.logo = file.data;
      },
      // 商户详情跳转
      merchDetails(rows) {
        this.$router.push({
          path: '/merchIncome',
          query: {
            merchantId: rows.merchantId,
            storeId: rows.storeId,
            storeName: rows.storeName
          }
        });
      },
      // 封禁/解封
      deblocking(row) {
        if (row.banStatus == 1) {
          this.banStatus = 2
        } else {
          this.banStatus = 1
        }
        this.$confirm(`确定执行此操作?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$http({
            url: this.$http.adornUrl2('/self/store/ban'),
            method: 'get',
            params: this.$http.adornParams({
              'storeId': row.storeId,
              'banStatus': this.banStatus
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
            }
          })
        }).catch(() => {})
      },
      // 查看
      merchLook(row) {
        this.dialogFormVisible = true
        this.form.storeId = row.storeId
        this.form.merchantId = row.merchantId
        this.form.storeName = row.storeName
        this.form.logo = row.logo
        this.form.shipAddress = row.shipAddress
        this.form.refundAddress = row.refundAddress
        this.form.refundContact = row.refundContact
        this.form.refundMobile = row.refundMobile
        this.form.createTime = row.createTime
      },
      // 获取数据列表
      dataSelect() {
        let page = this.page - 1
        this.tableDataLoading = true
        this.$http({
          url: this.$http.adornUrl2('/self/store/list'),
          method: 'get',
          params: this.$http.adornParams({
            'page': page,
            'size': this.size,
            'storeName': this.storeName
          })
        }).then(({
          data
        }) => {
          this.tableDataLoading = false
          let returnData = data.data
          this.tableData = returnData
        })
      }
    },
    mounted() {
      this.dataSelect()
    }
  }
</script>

<style scoped="scoped">
  .el-button+.el-button {
    margin-left: 0 !important;
    margin-top: 5px !important;
  }
</style>
