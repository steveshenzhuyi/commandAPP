<template>
  <div align="center" id="app" style="text-align: center;">
    <router-view v-if ="isRouterAlive"></router-view>
  </div>
</template>

<script>
export default {
  name: 'App',
  provide (){
    return {
      reload:this.reload
    }
  },
  data() { 
    return {
      isRouterAlive:true,
      exitAppTicker:0
    }
  },
  mounted(){
    this.backbutton()
    window.localStorage.setItem("VIDEOUSERID",904)
    var that = this
    document.addEventListener("jpush.receiveNotification", function (event) {
      var alertContent = event.extras.type
      var videoid = Number(window.localStorage.getItem("VIDEOUSERID"))
      if(alertContent == 'video' && videoid != 0){
        // that.GLOBAL.changeVideoAlert(true)
        var scheme = 'com.tencent.trtc';
        var roomnumber = Number(event.extras.roomnumber);
        appAvailability.check(scheme,
          function() {
            var sApp = startApp.set({"application":"com.tencent.trtc"}, { 
                "roomnumber":roomnumber,
                "videoid":videoid
              });
            sApp.start(function() {
            }, function(error) {
              alert(error);
            });
          },
          function() {
            alert(scheme + ' is not available');
          }
        );
      }else if(alertContent == 'ver'){
        that.checkversion()
      }
      console.log(that.GLOBAL)
    }, false)

    document.addEventListener("jpush.openNotification", function (event) {
      var alertContent = event.extras.type
      var videoid = Number(window.localStorage.getItem("VIDEOUSERID"))
      
      if(alertContent == 'video' && videoid != 0){
        that.GLOBAL.changeVideoAlert(false)
        var scheme = 'com.tencent.trtc';
        var roomnumber = Number(event.extras.roomnumber);
        appAvailability.check(scheme,
          function() {
            var sApp = startApp.set({"application":"com.tencent.trtc"}, { 
                "roomnumber":roomnumber,
                "videoid":videoid
              });
            sApp.start(function() {
            }, function(error) {
              alert(error);
            });
          },
          function() {
            alert(scheme + ' is not available');
          }
        );
      }
    }, false)

  },
  methods: {
    backbutton(){
  var that=this
    document.addEventListener("deviceready",function(){
        document.addEventListener("backbutton", function(){
      console.log(navigator)
//     //         if(this.isHomePage()){
//        // console.log("222")
                if(that.exitAppTicker == 0){
                    that.exitAppTicker++;
Toast({
  message: '再按一次返回桌面',
  position: 'bottom',
  duration: 1000
});
                    setTimeout(function(){
                            that.exitAppTicker = 0;
                    },2000);
                }else if(that.exitAppTicker == 1){
//                     navigator.app.exitApp(); //退出app
 navigator.Backbutton.goHome(function() { //进入后台
                  // console.log('go home success');
              }, function() {
                  // console.log('go home fail');
              });
                }  
// //                 }else{
// //                     history.back();
// //                 }
        }, false);
    },false);
},
    reload() {
      this.isRouterAlive = false
      this.$nextTick(function() {
        this.isRouterAlive = true
      })
    }
  },
  components: {
  }
};
</script>