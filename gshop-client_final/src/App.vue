<template>
  <div id="app">
    <router-view/>
    <FooterGuide v-show="$route.meta.showFooter"/>
  </div>
</template>

<script>
  import {mapActions} from 'vuex'
  import FooterGuide from './components/FooterGuide/FooterGuide.vue'
  import {mapState} from 'vuex'
  export default {
    props: {
      food: Number,
      afood: Number,
    },

    mounted () {

      var sww=this.$store
      function baiduPosition(cid){
        var geolocation = new BMap.Geolocation();
        geolocation.getCurrentPosition(function(r){
          if(this.getStatus() == BMAP_STATUS_SUCCESS){
            var position = {
              lng: r.point.lng,
              lat: r.point.lat
            }
            if(cid == 'sort'){
              sort(position);
            } else {
              positions(position, cid);
            }
            var l=r.point.lng
            var a=r.point.lat
            console.log("经度"+l);
            console.log("纬度"+a);
            sww.dispatch('getAddress',{ food: l,afood:a})
          }
          else {
            //alert('获取当前位置失败,请确定您开启了定位服务');
          }
        },{enableHighAccuracy: true});
      }
      baiduPosition(35)
      function positions(json, cid) {
        return "o";
        //可以获取到了地理位置，跳转页面，然后在跳转的页面在获取经纬度的值
        //window.location.href = "./index.php?i=5&amp;c=entry&amp;do=list&amp;m=weilive&amp;cid=" + cid + "&amp;lng=" + json['lng'] + "&amp;lat=" + json['lat'];
        //alert(json['lng']);
      }

      this.getUserInfo()
    },

    methods: {
      ...mapActions(['getUserInfo']),

    },

    components: {
      FooterGuide
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .app
    width 100%
    height 100%
    background #f5f5f5
</style>
