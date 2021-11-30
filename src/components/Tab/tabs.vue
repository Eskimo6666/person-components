<template>
    <div class="gulu-tabs">
        <div class="gulu-tabs-nav" ref="container">
            <div
                v-for="(t,idx) in titles"
                :class="{ selected: t == selected }"
                :ref="el => { if (t == selected) resultDiv = el }"
                @click="selectItem(t)"
                :key="idx"
            >{{ t }}</div>
            <div class="gulu-tabs-nav-indicator" ref="indicator"></div>
        </div>
        <div class="gulu-tabs-content">
            <!-- 当自定义组件在相同Type的两个子组件间进行切换时，需要绑定一个key来做区分 -->
            <component :is="selectedContent" :key="selected" />
        </div>
    </div>
</template>


<script>
export default {
    methods: {
        selectItem(title) {
            this.$emit('update:selected', title)
        }
    }
}
</script>

<script  setup>
import { useSlots, defineProps, toRefs, computed, ref, watchEffect,onMounted } from 'vue';
import Tab from './tab.vue'
//对Tabs里的组件进行检查
//Tab是一个包含render属性的对象
//组件默认插槽会放在default返回的对象里，
//通过比较里面每个对象的type与Tab元素实例是否相等
//确定运行时自组件的类型
const indicator = ref(null)
const container = ref(null)
const resultDiv = ref(null)



onMounted(
    watchEffect(() => {
        const { width } = resultDiv.value.getBoundingClientRect()
        indicator.value.style.width = width + 'px'
        const { left: left1 } = resultDiv.value.getBoundingClientRect()
        const { left: left2 } = container.value.getBoundingClientRect()
        let left = left1 - left2
        indicator.value.style.left = left + 'px'
        console.log(left1 - left2)
    })
)
// onUpdated(x)

const props = defineProps({
    selected: {
        type: String
    }
})
const { selected } = toRefs(props)


console.log(selected.value);
const slots = useSlots()
let defaults = slots.default()
defaults.forEach((tag) => {
    if (tag.type !== Tab) {
        throw new Error('Tabs里面必须是Tab组件')
    }
})
const selectedContent = computed(() => {
    return defaults.filter((tag) => {
        return tag.props.title === selected.value
    })[0]
})
console.log(selectedContent, '所选项');
const titles = defaults.map((tag) => {
    return tag.props.title
})
// console.log(titles);
</script>


<style lang="scss">
$blue: #40a9ff;
$color: #333;
$border-color: #d9d9d9;
.selected {
    color: blue;
}
.gulu-tabs {
    &-nav {
        display: flex;
        color: $color;
        border-bottom: 1px solid $border-color;
        position: relative;
        &-item {
            padding: 8px 0;
            margin: 0 16px;
            cursor: pointer;
            &:first-child {
                margin-left: 0;
            }
            &.selected {
                color: $blue;
            }
        }
        &-indicator {
            position: absolute;
            height: 3px;
            background: $blue;
            left: 0;
            bottom: -1px;
            width: 100px;
            transition: all 250ms;
        }
    }
    &-content {
        padding: 8px 0;
        &-item {
            display: none;
            &.selected {
                display: block;
            }
        }
    }
}
</style>

