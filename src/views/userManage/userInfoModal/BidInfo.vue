<template>
  <div>
    <div class='detail-info'
         v-loading='detailLoading'>
      <div class='detail-item-wrap'>
        <div class='detail-item'>
          <div class='number'>{{totalReturnedAmount}} 元</div>
          <div class='text'>待还余额</div>
        </div>

        <div class='detail-item'>
          <div class='number'>{{totalLoanOverdueAmount}} 元</div>
          <div class='text'>逾期金额</div>
        </div>

        <div class='detail-item'>
          <div class='number'>{{totalLoanAmount}} 元</div>
          <div class='text'>累计借款金额</div>
        </div>

        <div class='detail-item'>
          <div class='number'>{{totalCounts}} 次</div>
          <div class='text'>累计发起借款</div>
        </div>

        <div class='detail-item'>
          <div class='number'>{{totalSuccessCounts}} 次</div>
          <div class='text'>成功借款</div>
        </div>
      </div>
    </div>
    <el-table
      :data="list"
      tooltip-effect="dark"
      style="width: 100%;"
      max-height='600'
      v-loading="listLoading">
      <el-table-column
        prop="id"
        label="交易订单号"
        width='120'>
      </el-table-column>
      <el-table-column
        prop="createAt"
        label="借款成功时间"
        width='180'
        show-overflow-tooltip>
      </el-table-column>
      <el-table-column
        prop="amount"
        label="借款金额"
        width='130'>
      </el-table-column>
      <el-table-column
        prop='rewardAmount'
        label="打赏金额"
        width='130'
        show-overflow-tooltip>
      </el-table-column>
      <el-table-column
        prop='realAmount'
        label="实际到账金额"
        width='130'
        show-overflow-tooltip>
      </el-table-column>
      <el-table-column
        prop='trueName'
        label="投资人姓名"
        width='160'
        show-overflow-tooltip>
      </el-table-column>

      <el-table-column
        label="还款状态"
        show-overflow-tooltip>
        <template slot-scope="scope">
          {{statusMap[scope.row.status] || scope.row.status}}
        </template>
      </el-table-column>
    </el-table>
    <div class="pagination-container">
      <el-pagination background
                     v-if='showPagination'
                     @size-change="handleSizeChange"
                     @current-change="handleCurrentChange"
                     :current-page="listQuery.page + 1"
                     :page-sizes="[10,20,30, 50]"
                     :page-size="listQuery.size"
                     layout="total, sizes, prev, pager, next, jumper"
                     :total="total">
      </el-pagination>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        detailLoading: false,

        showPagination: true,
        list: [],
        total: 0,
        listLoading: false,
        listQuery: {
          page: 0,
          size: 10,
        },
        statusMap: {
          collecting: '募集中',
          collected: '募集完成',
          repayed: '已还款',
          overdue: '逾期',
          fail: '流标',
        },

        totalReturnedAmount: 0,
        totalLoanOverdueAmount: 0,
        totalLoanAmount: 0,
        totalCounts: 0,
        totalSuccessCounts: 0,
      }
    },

    props: {
      memberId: {
        default: ''
      }
    },
    mounted() {
      this.getDetail()
      this.getList()
    },

    methods: {
      getDetail() {
        const self = this
        self.detailLoading = true
        AXIOS.post('/auth/userInfo/userTotalCountIndex', {
          memberId: self.memberId
        }).then(res => {
          self.totalReturnedAmount = res.totalReturnedAmount || 0
          self.totalLoanOverdueAmount = res.totalLoanOverdueAmount || 0
          self.totalLoanAmount = res.totalLoanAmount || 0
          self.totalCounts = res.totalCounts || 0
          self.totalSuccessCounts = res.totalSuccessCounts || 0
          self.detailLoading = false
        }).catch((res) => {
          self.detailLoading = false
        })
      },

      getList() {
        const self = this
        self.listLoading = true
        self.listQuery.memberId = self.memberId
        AXIOS.post('/auth/userInfo/bidInfoIndex', self.listQuery).then(res => {
          self.list = res.content || []
          self.total = res.totalElements || 0
          self.listLoading = false
        }).catch((res) => {
          self.listLoading = false
        })
      },
      handleSizeChange(val) {
        this.listQuery.size = val
        this.getList()
      },

      handleCurrentChange(val) {
        this.listQuery.page = val - 1
        this.getList()
      },
    }
  }
</script>

<style lang="scss"
       scoped>
  @import "./modalStyle.scss";
</style>
