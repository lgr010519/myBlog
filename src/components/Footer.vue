<template>
  <div class="footer" :class="{'fixed-footer':fixed}">
    <div class="copyright">
      <a target="_blank" href="https://beian.miit.gov.cn">Copyright © 2022 LIUGAORONG・湘ICP备2022010519号</a>
      <div>本系统由Vue+Muse-UI提供技术支持</div>
    </div>
  </div>
</template>

<script>
export default {
  props:{
    fixed:{
      type: Boolean,
      default: false,
    }
  }
}
</script>

<style lang="less" scoped>
.footer{
  text-align: center;
  font-size: 0.3rem;
  margin: 30px 0;
  .copyright{
    a{
      color: inherit;
    }
  }
}
.fixed-footer{
  position: fixed;
  bottom: 0;
  width: 100%;
  z-index: 1;
}
</style>