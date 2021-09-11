<template>
    <div class="footer-container">
        <div class="custom-control custom-checkbox">
           <input type="checkbox" class="custom-control-input" id="fullCheck" :checked="isfull" @change="onCheckBoxChange">
           <label class="custom-control-label" for="fullCheck">全选</label>
        </div>
        <!-- 合计 -->
        <div>
            <span>合计</span>
            <span class="amount">￥{{amount.toFixed(2)}}</span>
        </div>
        <button type="button" class="btn btn-primary" :disabled="total === 0">结算（{{total}}）</button>
    </div>
</template>
<script>
export default {
    name:'EsFooter',
    props:{
        amount:{
            type: Number,
            default: 0,
        },
        total:{
            type: Number,
            default: 0,
        },
        isfull:{
            type: Boolean,
            default: false,
        },
    },
    methods:{
        // 监听复选框选中状态变化的事件
        onCheckBoxChange(e) {
            // e.target.checked; // 复选框最新的选中状态
            // console.log(e.target.checked);
            // 触发自定义事件
            this.$emit('fullChange',e.target.checked)
        },
    },
    // 声明自定义事件
    emits: ['fullChange'],
}
</script>
<style lang="less" scoped>
.footer-container {
    position: fixed;
    bottom: 0;
    left: 0;
    height: 50px;
    width: 100%;
    // line-height: 50px;
    // 内部元素的对齐方式
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: white;
    border-top: 1px solid #efefef;
    padding: 0 10px;
    z-index: 999;
}
.amount {
    color: red;
    font-weight: bold;
}
.btn-primary {
    height: 38px;
    border-radius: 19px;
    min-width: 90px;
}
</style>
