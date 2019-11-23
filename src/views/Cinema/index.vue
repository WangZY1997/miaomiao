<template>
    <div id="mai  n" >
      <Header title="喵喵影院"/>
      <div id="content">
        <div class="cinema_menu">
          <div class="city_switch" @click="toggle(0)" :class="{'active':id===0}">
            全城 <i class="iconfont icon-triangle-down" ></i>
          </div>
          <div class="brand_switch"  @click="toggle(1)" :class="{'active':id===1}" >
            品牌 <i class="iconfont icon-triangle-down"></i>
          </div>
          <div class="feature_switch" @click="toggle(2)" :class="{'active':id===2}">
           特色 <i class="iconfont icon-triangle-down"></i>
          </div>
        </div>
         <CList/>  
        </div>
      <TabBar/>  
    
    </div>
</template>

<script>
import Header from '@/components/Header'
import TabBar from '@/components/TabBar'
import CList from '@/components/CList'
export default {
    name:'Cinema',
    data(){
      return{
        id:''
      }
   },
    components:{
      Header,
      TabBar,
      CList
    },
    methods: {
      toggle(id){
        this.id =id
      }
    },
}
</script>

<style scoped>
 #content .cinema_menu{
    width: 100%;
    height: 45px;
    border-bottom: 1px solid #e6e6e6;
    display: flex;
    justify-content: space-around;
    line-height: 45px;
} 
.active{
   border-bottom: 2px solid #ef4238;
}
</style>

