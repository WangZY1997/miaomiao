<template>
  <div class="movie_body" ref="movie_body">
    <Loading v-if="isLoading"/>
    <Scroller v-else :handleToScroll='handleToScroll' :handleToTouchEnd="handleToTouchEnd">
      <ul>
        <li class="pullDown">{{pullDownMsg}}</li>
        <li v-for="item in movieList" :key="item.id">
          <div class="pic_show" @tap="handleToDetail">
            <img :src="item.img | setWH('128.180')" alt />
          </div>
          <div class="info_list">
            <h2>{{item.nm}}</h2>
            <p>
              观众评
              <span class="grade">{{item.sc}}</span>
            </p>
            <p>主演：{{item.star}}</p>
            <p>{{item.showInfo}}</p>
          </div>
          <div class="btn_mail">购票</div>
        </li>
      </ul>
    </Scroller>
  </div>
</template>   
<script>
export default {
  data() {
    return {
      movieList: [],
      pullDownMsg: "",
      isLoading:true,
      preCityId:-1
    };
  },
  name: "NowPlaying",
  activated() {
    // console.log(123);
     //在mounted生命周期中是不允许在刷新页面后再次执行请求
     var cityId =  this.$store.state.city.id
    //  console.log(cityId);
     if(this.preCityId === cityId){
       return;
     }
      this.isLoading = true
    this.axios.get("/api/movieOnInfoList?cityId="+cityId).then(res => {
      // console.log(res);
      if (res.data.msg === "ok") {
        this.movieList = res.data.data.movieList;
        this.isLoading = false
        this.preCityId = cityId
        //    this.$nextTick(()=>{
        //       var scroll = new BScroll(this.$refs.movie_body,{
        //            tap:true,
        //            probeType:1
        //        });
        //        scroll.on('scroll',(pos)=>{
        //         //    console.log('scroll');
        //         if(pos.y>30){
        //         this.pullDownMsg = '正在更新中'
        //         }

        //        });
        //        scroll.on('touchEnd',(pos)=>{
        //             // console.log('scrollend');
        //             if(pos.y>30){
        //                  this.axios.get('/api/movieOnInfoList?cityId=11').then(res=>{
        //                      if(res.data.msg === 'ok'){
        //                         this.pullDownMsg = '更新成功';
        //                         setTimeout(()=>{
        //                     this.movieList = res.data.data.movieList;
        //                     this.pullDownMsg = ''
        //                         },1000)

        //                      }

        //                  });

        //             }
        //        })
        //    })
      }
    });
  },
  methods: {
    handleToDetail() {
      console.log(1);
    },
    handleToScroll(pos) {
      if (pos.y > 30) {
        this.pullDownMsg = "正在更新中";
      }
    },
    handleToTouchEnd(pos) {
      if (pos.y > 30) {
        this.axios.get("/api/movieOnInfoList?cityId=11").then(res => {
          if (res.data.msg === "ok") {
            this.pullDownMsg = "更新成功";
            setTimeout(() => {
              this.movieList = res.data.data.movieList;
              this.pullDownMsg = "";
            }, 1000);
          }
        });
      }
    }
  }
};
</script>

<style scoped>
#content .movie_body {
  flex: 1;
  overflow: auto;
}
.movie_body ul {
  margin: 0 12px;
  overflow: hidden;
}
.movie_body ul li {
  margin-top: 12px;
  display: flex;
  align-items: center;
  border-bottom: 1px solid #e6e6e6;
  padding-bottom: 10px;
}
.movie_body .pic_show {
  width: 64px;
  height: 90px;
}
.movie_body .pic_show img {
  width: 100%;
}

.movie_body .info_list {
  margin-left: 10px;
  flex: 1;
  position: relative;
}
.movie_body .info_list h2 {
  font-size: 17px;
  line-height: 24px;
  width: 150px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
.movie_body .info_list p {
  font-size: 13px;
  color: #666;
  line-height: 22px;
  width: 200px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
.movie_body .info_list .grade {
  font-weight: 700;
  color: #faaf00;
  font-size: 15px;
}
.movie_body .info_list img {
  width: 50px;
  position: abslute;
  right: 10px;
  top: 5px;
}
.movie_body .btn_mail,
.movie_body .btn_pre {
  width: 47px;
  height: 27px;
  line-height: 28px;
  text-align: center;
  background-color: #f03d37;
  color: #fff;
  border-radius: 4px;
  font-size: 12px;
  cursor: pointer;
}
.movie_body .btn_pre {
  background-color: #3c9fe6;
}

.movie_body .pullDown {
  margin: 0;
  padding: 0;
  border: none;
}
</style>
    
  