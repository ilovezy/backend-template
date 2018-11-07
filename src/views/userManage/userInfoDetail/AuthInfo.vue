<!--TODO-->
<template>
  <div class="app-container auth-info">
    <div class='tab-main-container'>
      <div class='tab-header-wrap'>
        <div class='tab-item'
             :class='currentTab == "basic" && "active"'
             @click='setItem("basic")'>基本信息
        </div>
        <div class='tab-item'
             :class='currentTab == "operator" && "active"'
             @click='setItem("operator")'>运营商信息
        </div>
        <div class='tab-item'
             :class='currentTab == "ali-pay" && "active"'
             @click='setItem("ali-pay")'>支付宝信息
        </div>
        <div class='tab-item'
             :class='currentTab == "j-d" && "active"'
             @click='setItem("j-d")'>京东信息
        </div>
        <div class='tab-item'
             :class='currentTab == "fund" && "active"'
             @click='setItem("fund")'>公积金信息
        </div>
        <div class='tab-item'
             :class='currentTab == "social-security" && "active"'
             @click='setItem("social-security")'>社保信息
        </div>
      </div>

      <div class='tab-panel-wrap'>
        <transition name="fade-transform"
                    mode="out-in">
          <basic :memberId='memberId'
                 v-if='currentTab == "basic"'></basic>
          <operator :memberId='memberId'
                    v-if='currentTab == "operator"'></operator>
          <ali-pay :memberId='memberId'
                   v-if='currentTab == "ali-pay"'></ali-pay>
          <j-d :memberId='memberId'
               v-if='currentTab == "j-d"'></j-d>
          <fund :memberId='memberId'
                v-if='currentTab == "fund"'></fund>
          <social-security :memberId='memberId'
                           v-if='currentTab == "social-security"'></social-security>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
  import Basic from './AuthInfoSection/Basic'
  import Operator from './AuthInfoSection/Operator'
  import AliPay from './AuthInfoSection/AliPay'
  import JD from './AuthInfoSection/JD'
  import Fund from './AuthInfoSection/Fund'
  import SocialSecurity from './AuthInfoSection/SocialSecurity'

  export default {
    components: {
      Basic,
      Operator,
      AliPay,
      JD,
      Fund,
      SocialSecurity,
    },
    data() {
      return {
        memberId: '',
        currentTab: 'basic'
      }
    },

    created() {
      let params = this.$route.params || {}
      this.memberId = params.id
    },

    methods: {
      setItem(tabName) {
        this.currentTab = tabName
      }
    }
  }
</script>

<style lang="scss"
       scoped>
  .auth-info {
    .tab-main-container {
      background: #fff;
      box-shadow: 0px 2px 12px 0px rgba(0, 0, 0, 0.06);
      .tab-header-wrap {
        display: flex;
        align-items: center;
        justify-content: space-around;
        height: 50px;
        line-height: 50px;
        border-bottom: 1px solid #eaeaea;
        .tab-item {
          cursor: pointer;
          transition: all 0.3s ease-in-out;
          flex: 1;
          text-align: center;
          &:hover{
            background: #F5FBFF;
          }
          &.active {
            background: #1989FA;
            color: #fff;
          }
        }
      }

      .tab-panel-wrap {
        padding: 20px;
        min-height: 600px;
      }
    }
  }
</style>
