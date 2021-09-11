<template>
  <div class="app-container">
    <h1>App根组件</h1>
  <hr>
  <es-header title="购物车案例"></es-header>
  <es-footer :total="total" :amount="amount" @fullChange="onFullStateChange"></es-footer>
  <es-goods v-for="item in goodslist" :key="item.id" :id="item.id"
  :thumb="item.goods_img" :title="item.goods_name" :price="item.goods_price"
  :count="item.goods_count" :checked="item.goods_state" @stateChange="onGoodsStateChange" 
  @countChange="onGoodsCountChange"></es-goods>
  </div>
</template>
<script>
// import { defineComponent } from '@vue/composition-api'
import EsHeader from './components/es-header/EsHeader.vue';
import EsFooter from './components/es-footer/EsFooter.vue';
import EsGoods from './components/es-good/EsGoods.vue';
export default {
  name:'MyApp',
  data() {
    return {
      // 商品列表的数据
      goodslist:[],
    }
  },
  created() {
      this.getGoodsList();
    },
  methods:{
    // 请求列表的数据
    async getGoodsList(){
      // 1.通过组件实例this访问到全局挂载的$http属性，并发起Ajax数据请求
      const {data:res} = await this.$http.get('/api/cart');
      // 2.判断请求是否成功
      if (res.status !== 200) return alert('请求商品列表数据失败！');
      // 3.将请求成功的数据 存储到data中，供页面渲染期间使用
      // console.log(res);
      this.goodslist = res.list;
    },
    // 监听全选按钮状态的变化
    onFullStateChange(isFull) {
      // console.log(isFull);
      // 更新每件商品的选中状态
      this.goodslist.forEach(x=>x.goods_state = isFull);
    },
    // 监听商品选中状态变化的事件
    onGoodsStateChange(e) {
      // 1.根据id进行查找
      const findResult = this.goodslist.find(x=>x.id === e.id);
      if (findResult) {
        // 2.找到对应商品，则更新其选中状态
        findResult.goods_state = e.value;
      }
    },
    // 监听商品数量变化的事件处理函数
    onGoodsCountChange(e) {
      // console.log(e);
      // 1.根据id进行查找
      const findResult = this.goodslist.find(x=>x.id === e.id);
      // 2.找到对应商品，则更新其选中状态
      findResult.goods_count = e.value;
    }
  },
  // 定义计算属性
  computed:{
    // 已勾选商品的总价
    amount() {
      // 1.定义商品总价格
      let a = 0;
      // 2.循环累加商品总价格
      this.goodslist.filter(x=>x.goods_state).forEach(x=>{
        a += x.goods_price * x.goods_count
      });
      // 3.返回计算结果
      return a;
    },
    // 已勾选商品总数量
    total() {
      // 1.定义商品总数量
      let t = 0;
      // 2.循环累加商品数量
      this.goodslist.filter(x=>x.goods_state).forEach(x=>t += x.goods_count);
      // 3.返回计算结果
      return t;
    }
  },
  components:{
    EsHeader,
    EsFooter,
    EsGoods,
  }
}
</script>
<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>

