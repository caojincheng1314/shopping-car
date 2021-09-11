<template>
 <div class="header-container" :style="{color:color,backgroundColor:bgcolor,'font-size':fsize+'px'}">{{title}}</div>
</template> 
<script>
export default {
 name: 'EsHeader', 
 props:{
     title:{
         type:String,
         default:'es-header',
     },
     color:{
         type:String,
         default:'#fff',
     },
     bgcolor:{
         type:String,
         default:'#007bff',
     },
     fsize:{
         type:Number,
         default:12,
     }
 }
 }
</script> 
<style lang="less" scoped>
.header-container {
    position: fixed;
    top: 0;
    left: 0;
    height: 45px;
    line-height: 45px;
    width: 100%;
    z-index: 999;
    text-align: center;

}
</style>