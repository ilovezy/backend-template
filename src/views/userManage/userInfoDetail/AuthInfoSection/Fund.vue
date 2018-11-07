<!--TODO-->
<template>
  <div class='main-section'
       v-loading='loading'>
    <div v-if='showResult'>
      <div class='no-data'
           style='padding: 150px;text-align: center;font-size: 20px;color: #999;'
           v-if='isEmpty'>
        暂无数据
      </div>

      <div v-else
           class='section-info-container'>
        <cc-panel title='个人基本信息'
                  :subTitle='"信息获取时间：" + user_info.createAt'>
          <el-row :gutter="20">
            <el-col :span="8">
              <div class='simple-text'>姓名：{{user_info.real_name}}</div>
              <div class='simple-text'>手机号：{{user_info.mobile}}</div>
              <div class='simple-text'>身份证号：{{user_info.id_card}}</div>
              <div class='simple-text'>性别：{{user_info.gender}}</div>
            </el-col>
            <el-col :span="8">
              <div class='simple-text'>当前缴存城市名称：TODO</div>
              <div class='simple-text'>当前缴存企业名称：TODO</div>
            </el-col>
          </el-row>
        </cc-panel>

        <cc-panel title='账户基本信息'>
          <el-row :gutter="20">
            <el-col :span="8">
              <div class='simple-text'>账户余额： {{user_info.fund_balance}}</div>
              <div class='simple-text'>月缴金额（元）：{{user_info.monthly_total_income}}</div>
              <div class='simple-text'>基数（元）：{{user_info.base_number}}</div>
              <div class='simple-text'>最后缴费日期：{{user_info.last_pay_date}}</div>
            </el-col>
            <el-col :span="8">
              <div class='simple-text'>公积金存缴状态：{{user_info.pay_status}}</div>
              <div class='simple-text'>单位月缴金额（元）：{{user_info.monthly_corporation_income}}</div>
              <div class='simple-text'>个人月缴金额（元）：{{user_info.monthly_customer_income}}</div>
              <div class='simple-text'>单位月缴比例：{{user_info.corporation_ratio}}</div>
              <div class='simple-text'>个人月缴比例：{{user_info.customer_ratio}}</div>
            </el-col>
            <el-col :span="8">
              <div class='simple-text'>开户日期：{{user_info.begin_date}}</div>
              <div class='simple-text'>最早缴费日期：TODO</div>
            </el-col>
          </el-row>
        </cc-panel>

        <cc-panel title='贷款信息 TODO'>
          <el-row :gutter="20">
            <el-col :span="8">
              <div class='simple-text'>贷款人姓名：{{user_info.user_name}}</div>
              <div class='simple-text'>贷款人联系电话：{{user_info.certified ? '经过认证' : '未经过认证'}}</div>
            </el-col>
            <el-col :span="8">
              <div class='simple-text'>贷款状态：{{user_info.register_time}}</div>
              <div class='simple-text'>承办银行：{{user_info.register_time}}</div>
              <div class='simple-text'>贷款类型：{{user_info.register_time}}</div>
              <div class='simple-text'>当前贷款购房地址：{{user_info.register_time}}</div>
              <div class='simple-text'>贷款金额：{{user_info.register_time}}</div>
              <div class='simple-text'>贷款期数：{{user_info.register_time}}</div>
            </el-col>
            <el-col :span="8">
              <div class='simple-text'>月还款金额：{{user_info.register_time}}</div>
              <div class='simple-text'>贷款开始日期：{{user_info.register_time}}</div>
              <div class='simple-text'>贷款结束日期：{{user_info.register_time}}</div>
              <div class='simple-text'>还款方式：{{user_info.register_time}}</div>
              <div class='simple-text'>每月还款日：：{{user_info.register_time}}</div>
            </el-col>
          </el-row>
        </cc-panel>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        loading: false,
        isEmpty: false,
        showResult: false,

        user_info: {},
      }
    },

    props: {
      memberId: {
        default: ''
      }
    },

    created() {
      this.getDetail()
    },

    methods: {
      formatMoney,
      getDetail() {
        const self = this
        self.loading = true
        AXIOS.post('/auth/moxieInfo/fundData', {
          memberId: Number(this.memberId)
        }).then(res => {
          if (_.isEmpty(res)) {
            self.isEmpty = true
          } else {
            self.user_info = res.user_info || {}
          }
          self.showResult = true
          self.loading = false
        }).catch((res) => {
          self.showResult = true
          self.loading = false
        })
      }
    }
  }
</script>

<style lang="scss"
       scoped>
  .main-section {
    padding-bottom: 50px;
  }
</style>
