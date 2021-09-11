<template>
    <div class="goods-container">
        <!-- 左侧图片区域 -->
    <div class="left">
        <div class="custom-control custom-checkbox">
            <input type="checkbox" class="custom-control-input" :id="id" :checked="checked"
            @change="onCheckBoxChange">
            <label class="custom-control-label" :for="id">
                <!-- 商品的缩略图 -->
                <img :src="thumb" alt="商品图片" class="thumb" />
            </label>
        </div>
    </div>

    <!-- 右侧信息区域 -->
    <div class="right">
      <!-- 商品名称 -->
      <div class="top">{{title}}</div>
      <div class="bottom">
        <!-- 商品价格 -->
        <div class="price">￥{{price.toFixed(2)}}</div>
        <!-- 商品数量 -->
        <div class="count">
            <es-counter :num="count" :min="1" @numChange="getNumber"></es-counter>
        </div>
      </div>
    </div>
    </div>
</template>
<script>
import EsCounter from '../es-counter/EsCounter.vue';
export default {
   name: 'EsGoods',
   // 封装自定义属性id
   emits:['stateChange','countChange'],
   methods:{
        // 监听复选框选中状态变化的事件
       onCheckBoxChange(e) {
           this.$emit('stateChange',{
               id: this.id,
               value: e.target.checked,
           });
       },
        // 监听数量变化的事件
        getNumber(num) {
            // console.log(num);
            // 触发自定义事件，向外传递数据对象{id，value}
            this.$emit('countChange',{
                // 商品id
                id:this.id,
                // 最新的数量
                value: num,
            })
        },
   },
   props:{
       id:{
           type: [Number,String],
           require: true,
       },
       thumb:{
           type: String,
           require: true,
       },
       title:{
           type: String,
           require: true,
       },
       price: {
           type: Number,
           require: true,
       },
       count:{
           type: Number,
           require: true,
       },
       checked:{
           type: Boolean,
           require: true,
       },  
   },
   components:{
       EsCounter,
   },
}
</script>
<style lang="less" scoped>
.goods-container {
  display: flex;
  padding: 10px;
   // 最终生成的选择器为 .goods-container + .goods-container
  // 在 css 中，（+）是“相邻兄弟选择器”，表示：选择紧连着另一元素后的元素，二者具有相同的父元素。
  + .goods-container {
        border-top: 1px solid #efefef;
    }
    // 左侧图片的样式
    .left {
       margin-right: 10px;
         // 商品图片
        .thumb {
            display: block;
            width: 100px;
            height: 100px;
            background-color: #efefef;
        }
    }
    // 右侧商品名称、单价、数量的样式
    .right {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        flex: 1;
        .top {
            font-weight: bold;
        }
        .bottom {
            display: flex;
            justify-content: space-between;
            align-items: center;
            .price {
                color: red;
                font-weight: bold;
            }
        }
    }
}
.custom-control-label::after,
.custom-control-label::before {
    top: 3.4rem;
}
</style>
