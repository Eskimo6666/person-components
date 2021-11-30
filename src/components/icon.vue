<template>
    <img :src="currentMode" :style="{width:width, height:height}" alt='' />
</template>

<script>

const theme = window.theme || 'light'
export default {
    name: 'icon',
    data() {
        return {
            normal: '',
            black: '',
        }
    },
    created() {
        this.initIcon()
    },
    props: {
        //greenRed
        colorMode:{
            default:false,
            type:Boolean
        },
        //theme
        themeMode:{
            default:true,
            type:Boolean
        },
        //图片名称
        name: {
            type: String,
            default: 'search'
        },
        //宽度
        width:{
            type:String,
            default:'1.3rem'
        },
        //高度
        height:{
            type:String,
            default:'1.3rem'
        }
    },
    methods: {
        initIcon() {
            const files = require.context('../assets', true, /\.png$/)
            files.keys().forEach(element => {
                let iconName = element.slice(2, element.length - 4)
                if (iconName === `icon_${this.name}`) {
                    this.normal = require(`../assets/icon_${this.name}.png`)
                } else if (iconName === `icon_${this.name}_black`) {
                    this.black = require(`../assets/icon_${this.name}_black.png`)
                }
            });
        }
    },
    computed: {
        //Icon
        currentMode() {
            return theme === 'black' ? this.black : this.normal
        }
    }

}
</script>
