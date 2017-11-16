<template>
  <div>
    <input class="test">
  </div>
</template>

<script>
  export default {
    data () {
      return {
      }
    },
    watch: {},
    mounted () {
      let _this = this
      window.onpageshow = function (event) {  // ios 专用,每次加载页面时触发
        let payOrderId = $cache.get('payOrderId', '')
        $cache.del('payOrderId')
        if (payOrderId) {
          _this.$router.push({
            name: 'Payment',
            query: {orderId: payOrderId, channel: _this.isTakeout ? 1 : 0}
          })
        }
      }
      window.onhashchange = (e) => {
        if (e.oldURL.search(/type10/g > 0)) {
          this.benefitStatus = false
        }
      }
    },
    // android 专用
    beforeRouteEnter (to, from, next) {
      next(vm => {
        document.title = vm.$store.getters.shopName
        if (from.name === 'newDetail') {
          vm.isDetail = true
        } else if (from.name === null) {
          vm.isLoading = false
          let payOrderId = $cache.get('payOrderId', '')
          $cache.del('payOrderId')
          if (payOrderId) {
            vm.$router.push({
              name: 'Payment',
              query: {orderId: payOrderId, channel: vm.isTakeout ? 1 : 0}
            })
          }
        } else if (from.name === 'Payment') {
          vm.isLoading = false
          vm.$router.push({name: 'index', query: {channel: vm.isTakeout ? 1 : 0}})
        }
      })
    }
  }
</script>

<style type="text/scss" lang="scss" scoped>
</style>
