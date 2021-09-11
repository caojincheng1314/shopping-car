<template>
    <div class="counter-container">
        <!-- 数量 -1 按钮 -->
        <button type="button" class="btn btn-light btn-sm" @click="onSubClick">-</button>
        <!-- 输入框 -->
        <input type="number" class="form-control form-control-sm ipt-num" v-model.number.lazy="number" />
        <!-- 数量 +1 按钮 -->
        <button type="button" class="btn btn-light btn-sm" @click="onAddClick">+</button>
    </div>
</template>
<script>

export default {
    name:'EsCounter',
    props:{
        // 数量值
        num:{
            type: Number,
            default: 0,
        },
        // 最小数量值
        min:{
            type: Number,
            // min 属性的值默认为 NaN，表示不限制最小值
            default: NaN,
        },
    },
    data() {
        return {
            number: this.num,
        }
    },
    methods:{
        onSubClick() {
            // min值存在且-1后小于min
            if(!isNaN(this.min) && this.number - 1 < this.min) return
            this.number -= 1;
        },
        onAddClick() {
            this.number += 1;
        },
    },
    emits:['numChange'],
    watch:{
        // 监听number数值的变化
        number(newval) {
            // 1.将输入的新值转换为整数
            const parseResult = parseInt(newval);
            // 2.如果转换的结果不是数字，或小于1，则强制number的值等于1
            if(isNaN(parseResult) || parseResult < 1) {
                this.number = 1;
                return
            }
            // 3.如果新值为小数，则把转换结果赋值给number
            if (String(newval).indexOf('.') !== -1) {
                this.number = parseResult;
                return
            }
            // console.log(this.number);
            // 触发numChange自定义事件,把最新的number数值传递给组件的使用者
            this.$emit('numChange',this.number);
        },      
    }
}
</script>
<style lang="less" scoped>
    .counter-container {
        display: flex;
        // 按钮的样式
        .btn {
            width: 25px;
        }
        // 输入框的样式
        .ipt-num {
            width: 34px;
            text-align: center;
            margin: 0 4px;
        }
    }
</style>
