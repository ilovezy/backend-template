<template>
  <div class='panel'>
    <div class='panel-title'
         v-if='title'>{{title}}
      <span class='panel-subtitle' v-if='subTitle'>
        {{subTitle}}
      </span>
    </div>
    <div class='panel-body'>
      <slot></slot>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'cc-panel',
    props: {
      title: {
        type: String,
        default: ''
      },
      subTitle: {
        type: String,
        default: ''
      },
    },
    data() {
      return {}
    },
    mounted() {
    },
    beforeDestroy() {

    },
    methods: {}
  }
</script>

<style lang='scss'
       scoped>
  @import "src/styles/variables.scss";
  .panel {
    border: 1px solid $border-color;
    margin-bottom: 20px;
    .panel-title {
      border-bottom: 1px solid $border-color;
      padding: 20px;
      color: #1989FA;
      font-size: 18px;
      font-weight: bold;
      background: #F5FBFF;
      position: relative;
      &:before {
        content: '';
        display: inline-block;
        width: 3px;
        height: 20px;
        background: #1989FA;
        position: absolute;
        top: 50%;
        transform: translateY(-50%);
        left: 0;
      }
      .panel-subtitle {
        font-size: 14px;
        color: #999;
        margin-left: 20px;
      }
    }
    .panel-body {
      padding: 20px;
      &:after {
        visibility: hidden;
        display: block;
        font-size: 0;
        content: " ";
        clear: both;
        height: 0;
      }
    }
  }

</style>
