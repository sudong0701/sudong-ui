<template>
    <div class="csSwipeCellBox" style="width: 100%; overflow-x: hidden">
        <div class="csSwipeCell" ref="csSwipeCell">
            <div class="csSwipeCell_left" ref="csSwipeCell_left">
                <slot name="left">

                </slot>
            </div>
            <div class="csSwipeCell_center" @touchstart="touchStart($event)" @touchmove="touchMove($event)" @touchend="touchEnd($event)" @touchleave="touchLeave($event)" @touchenter="touchEnter($event)">
                <slot>

                </slot>
            </div>
            <div class="csSwipeCell_right" ref="csSwipeCell_right">
                <slot name="right"></slot>
            </div>
            <!--<div class="csSwipeCell_box">-->
            <!---->
            <!--</div>-->
        </div>
    </div>
</template>

<script>
    let startPageX, startPageY, currentPageX
    export default {
        name: 'csSwipeCell',
        data() {
            return {
                isDisabled: false,
                leftWidth: '',
                rightWidth: '',
                cellStatus: 'init',   //单元格的状态 init 初始、 left 左、 right 右
            }
        },
        props: {

        },
        mounted() {
            const csSwipeCell_left = this.$refs.csSwipeCell_left
            const csSwipeCell_right = this.$refs.csSwipeCell_right
            this.leftWidth = csSwipeCell_left.clientWidth
            this.rightWidth = csSwipeCell_right.clientWidth
        },
        methods: {
            /**
             触摸移动事件
             @param {dom} e 触摸开始事件参数
             @return
             */
            touchStart(e) {
                this.isDisabled = false
                startPageX = currentPageX = e.targetTouches[0].pageX
                startPageY = e.targetTouches[0].pageY
            },
            /**
             触摸移动事件
             @param {dom} e 触摸移动事件参数
             @return
             */
            touchMove(e) {
                const movePageX = e.targetTouches[0].pageX
                const movePageY = e.targetTouches[0].pageY
                if(this.isDisabled) {   //如果为禁用状态则不处理
                    return
                }
                //如果Y轴移动的距离大于X轴则不进行处理 或距离小于30px
                if(Math.abs(movePageY - startPageY) > Math.abs(movePageX - startPageX) && currentPageX === startPageX ) {   //如果首次move事件Y轴距离大于X轴距离则把表格设置为禁用状态
                    this.isDisabled = true
                    return
                }
                currentPageX = movePageX   //将当前的currentPageX设置为movePageX
                //右滑时的不处理情况
                if((movePageX - startPageX >= this.leftWidth && this.cellStatus === 'init') || (this.cellStatus === 'left' && movePageX > startPageX) || (this.cellStatus === 'right' && movePageX - startPageX >= (this.leftWidth + this.rightWidth))) {
                    return
                }
                //左滑的不处理情况
                if((startPageX - movePageX >= this.rightWidth && this.cellStatus === 'init') ||  (this.cellStatus === 'right' && movePageX < startPageX) || (this.cellStatus === 'left' && startPageX - movePageX >= (this.rightWidth + this.leftWidth))) {
                    return
                }
                const csSwipeCell = this.$refs.csSwipeCell
                let initPageX
                if(this.cellStatus === 'init') {
                    initPageX = 0
                } else if(this.cellStatus === 'left') {
                    initPageX = this.leftWidth
                }else if(this.cellStatus === 'right') {
                    initPageX = -this.rightWidth
                }
                csSwipeCell.style.transition=`-webkit-transform 0 ease-out`;
                csSwipeCell.style.transform=`translate(${e.targetTouches[0].pageX - startPageX + initPageX}px, 0)`;
            },
            /**
             触摸事件结束事件
             @param {dom} e 触摸结束事件参数
             @return
             */
            touchEnd(e) {
                const endPageX = e.changedTouches[0].pageX
                const endPageY = e.changedTouches[0].pageY
                if(this.isDisabled) {
                    return
                }
//                //如果Y轴移动的距离大于30则不进行处理
//                if(Math.abs(endPageY - startPageY) >30) {
//                    return
//                }
                //点击事件不进行处理
                if(endPageX === startPageX) {
                    return
                }else if(endPageX - startPageX > 0) {   //向右滑
                    const csSwipeCell = this.$refs.csSwipeCell
                    if(this.cellStatus === 'init'){
                        csSwipeCell.style.transition=`-webkit-transform 300ms ease-out`;
                        csSwipeCell.style.transform=`translate(${this.leftWidth}px, 0)`;
                        setTimeout(()=> {
                            this.cellStatus = 'left'
                        }, 300)
                    } else if(this.cellStatus === 'right'){
                        if(endPageX - startPageX > (this.rightWidth + this.leftWidth * 0.6)) {
                            csSwipeCell.style.transition=`-webkit-transform 300ms ease-out`;
                            csSwipeCell.style.transform=`translate(${this.leftWidth}px, 0)`;
                            setTimeout(()=> {
                                this.cellStatus = 'left'
                            }, 300)
                        } else {
                            csSwipeCell.style.transition=`-webkit-transform 300ms ease-out`;
                            csSwipeCell.style.transform=`translate(0, 0)`;
                            setTimeout(()=> {
                                this.cellStatus = 'init'
                            }, 300)
                        }
                    }
                } else {   //向左滑
                    const csSwipeCell = this.$refs.csSwipeCell
                    if(this.cellStatus === 'init'){
                        csSwipeCell.style.transition=`-webkit-transform 300ms ease-out`;
                        csSwipeCell.style.transform=`translate(${-this.rightWidth}px, 0)`;
                        setTimeout(()=> {
                            this.cellStatus = 'right'
                        }, 300)
                    } else if(this.cellStatus === 'left'){
                        if(startPageX - endPageX > (this.leftWidth + this.rightWidth * 0.6)) {
                            csSwipeCell.style.transition=`-webkit-transform 300ms ease-out`;
                            csSwipeCell.style.transform=`translate(${-this.rightWidth}px, 0)`;
                            setTimeout(()=> {
                                this.cellStatus = 'right'
                            }, 300)
                        } else {
                            csSwipeCell.style.transition=`-webkit-transform 300ms ease-out`;
                            csSwipeCell.style.transform=`translate(0, 0)`;
                            setTimeout(()=> {
                                this.cellStatus = 'init'
                            }, 300)
                        }
                    }
                }
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
    .csSwipeCellBox {
        width: 100%;
        overflow-x: hidden;
        .csSwipeCell {
            position: relative;
            width: 100%;
            //overflow: hidden;
            .csSwipeCell_left {
                position: absolute;
                top: 0; left: 0;
                height: 100%;
                transform: translate(-100%, 0);
                z-index: 2;
            }
            .csSwipeCell_center {
                width: 100%
            }
            .csSwipeCell_right {
                position: absolute;
                top: 0; right: 0;
                height: 100%;
                transform: translate(100%, 0);
                z-index: 2;
            }
        }
    }
</style>