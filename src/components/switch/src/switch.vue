<template>
    <div :class="`csSwitch ${disabled ? 'csSwitch--disabled': ''} ${isActive ? 'csSwitch-active' : ''}`" :style="`font-size: ${circleSize}; background-color: ${isActive ? activeParentsNodeColor : unActiveParentsNodeColor}; `" @click="changeActive">
        <div :class="`csSwitch-circle ${isActive ? 'csSwitch-active' : ''}`" :style="`width: ${circleSize}; height: ${circleSize}; background-color: ${isActive ? activeNodeColor : unActiveNodeColor}; transform: translateX(${isActive ? '100%' : '0%'})`" @webkitTransitionEnd="animationEnd($event)" @transitionend="animationEnd($event)">

        </div>
    </div>
</template>

<script>
    export default {
        name: 'csSwitch',
        data() {
            return {}
        },
        props: {
            isActive: {
                type: Boolean,
                default: false
            },
            size: {
                type: String | Number,
                default: '0.6rem'
            },
            activeParentsNodeColor: {
                type: String,
                default: ''
            },
            activeNodeColor: {
                type: String,
                default: '#fff'
            },
            unActiveParentsNodeColor: {
                type: String,
                default: '#fff'
            },
            unActiveNodeColor: {
                type: String,
                default: '#fff'
            },
            disabled: {
                type: Boolean,
                default: false
            },
            asyncChange: {
                type: Boolean,
                default: false
            }
        },
        model: {
            prop: 'isActive',
            event: 'change'
        },
        computed: {
            circleSize() {
                return typeof this.size === 'string' ? this.size : (typeof this.size === 'number' ? `${this.size}px` : '0.6rem')
            }
        },
        methods: {
            /**
             点击改变switch状态
             @param
             @return
             */
            changeActive() {
                if(!this.disabled) {
                    if(!this.asyncChange) {
                        this.$emit('change', !this.isActive)
                    }
                    this.$emit('click')
                }
            },
            /**
             动画结束时触发
             @param
             @return
             */
            animationEnd() {
                if(this.isActive) {
                    this.$emit('opened')
                } else {
                    this.$emit('closed')
                }
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
    .csSwitch {
        box-sizing: content-box;
        display: inline-block;
        position: relative;
        padding: 0.02rem;
        border: 0.02rem solid rgba(0, 0, 0, 0.1);
        border-radius: 1em;
        width: 2em;
        height: 1em;
        overflow: hidden;
        transition-property: all;
        transition-duration: .3s;
    .csSwitch-circle {
        position: absolute;
        top: 0.03rem;
        left: 0.03rem;
        transition-duration: .3s;
        transition-property: all;
        transition-timing-function: ease-in-out;
        border-radius: 50%;
        box-shadow: 0 3px 1px 0 rgba(0, 0, 0, 0.05), 0 2px 2px 0 rgba(0, 0, 0, 0.1), 0 3px 3px 0 rgba(0, 0, 0, 0.05);
    }
    }
    .csSwitch-active {
        background-color: var(--color);
    }
    .csSwitch--disabled {
        cursor: not-allowed;
        opacity: 0.5;
    }
</style>