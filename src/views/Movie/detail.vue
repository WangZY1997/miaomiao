<template>
    <div id="detailcontainer" class="slide_enter_active">
        <Header title="影片详情">
            <i class="iconfont icon-fanhui-copy-copy" @touchstart="handleToBack"></i></Header>  
            <Loading v-if="isLoading"></Loading>
        <div v-else id="content" class="contentDetail">
            <div class="detail_list">
                <div class="detail_list_bg"></div>
                <div class="detail_list_filter"></div>
                <div class="detail_list_content">
                    <div class="detail_list_img">
                        <img :src="movieDetail.albumImg" alt="">
                    </div>
                    <div class="detail_list_info">
                        <h2>{{movieDetail.nm}}</h2>
                        <p>{{movieDetail.enm}}</p>
                        <p>{{movieDetail.sc}}分</p>
                        <p>{{movieDetail.cat}}</p>
                        <p>{{movieDetail.src}}/{{movieDetail.dur}}分钟</p>
                        <p>{{movieDetail.pubDesc}}</p>

                    </div>
                </div>

            </div>
            <div class="detail_intro">{{movieDetail.dra}}</div>
            <div class="detail_player swiper-container" ref="detail_player">
                <ul class="swiper-wrapper">
                    <li v-for="(item,index) in movieDetail.photos"  :key="index"  class="swiper-slide">
                        <div>
                            <img :src="item | setWH('148.208')" alt="" >
                        </div>
                        <p>{{movieDetail.dir}}</p>
                        <p>{{movieDetail.star}}</p>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>
<script>
import Header from '@/components/Header'
export default {
    name:'Detail',
    data(){
        return{
            movieDetail :{},
            isLoading:true
        }
    },
    components:{
        Header,
        
    },
    props:['movieid'],
    methods:{
        handleToBack(){
            this.$router.back()
        }
    },
    mounted() {
        // console.log(this.movieid);
        this.axios.get('/api/detailmovie?movieId='+this.movieid).then((res)=>{
            var msg = res.data.msg;
            // console.log(res);
            if(msg === 'ok'){
                this.isLoading = false 
                this.movieDetail  = res.data.data.detailMovie ;
                setTimeout(()=>{
                
                this.$nextTick(()=>{
                    new Swiper(this.$refs.detail_player,{
                        slidesPerView:'auto',
                        freeMode:true,
                        freeModeSticky:true
                    })
                })
               
                },2000)
            
                
            }
        })
    },

}
</script>

<style  scoped>
#detailcontainer{
    position: absolute;
    left: 0;
    top:0;
    z-index: 100;
    width: 100%;
    min-height: 100%;
    background: white;
}
#content.contentDetail{display:block;margin-bottom: 0}
#detailcontainer.slide_enter_active{animation:.3s sliderMove}
@keyframes sliderMove {
    0% { transform:translateX(100%)}
    100%{transform:translateX(0)}
    }

#content .detail_list{height: 200px; width: 100%;position: relative; overflow: hidden;}
.detail_list .detail_list_bg{width: 100%;height: 100%;filter: blur(20px);background-color: gray;}
.detail_list .detail_list_filter{width: 100%;height: 100%;background-color: #40454d ;opacity: 0.5;}
.detail_list .detail_list_content{display: flex;width: 100%;height: 100%;position: absolute;left: 0;top: 0;z-index: 100}
.detail_list .detail_list_img{width: 108px;height: 150px;border: 1px solid #f0f2f3;margin: 20px}
.detail_list .detail_list_img img{width: 100%; height: 100%;}
.detail_list .detail_list_info{margin-top: 20px;}
.detail_list .detail_list_info h2{font-size: 20px;color: white;font-weight: normal;line-height: 40px}
.detail_list .detail_list_info p{color:white;line-height: 20px;font-size: 14px;color:#ccc}

#content .detail_intro{padding: 10px}
#content .detail_player{margin: 20px}
.detail_player .swiper-slide{width: 70px;margin-right: 20px;text-align: center;font-size: 14px}
.detail_player  .swiper-slide img{width: 100%;margin-bottom: 5px}
.detail_player  .swiper-slide p:nth-of-type(2){color: #999}

</style>