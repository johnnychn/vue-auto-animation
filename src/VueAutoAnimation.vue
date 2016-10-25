<template>
    <div v-el:box :style="style" :class="class">
        <vue-dynamic-animation v-el:dom @click="showInfo" :normal="normal" :enter="enter"
                               :leave="leave" v-show="show">
            <slot></slot>
        </vue-dynamic-animation>
    </div>
</template>


/*!
* vue-auto-animation v0.0.1 (https://github.com/johnnyGoo/vue-auto-animation)
* Author: Johnny chen
*
* Copyright 2013-2015 Johnny chen
*/
<script>
    import VueDynamicAnimation from './vue-dynamic-animation.js';
    var count = 0;
    if (!window.Smart) {
        throw 'vue-auto-animation required smart.js'
    }
    var Smart = window.Smart;
    var Css = Smart.Css;
    var Event = Smart.Event;
    var Utils = Smart.Utils;
    var _ = Smart._;






    // 注册
    export default {
        components: {
            VueDynamicAnimation: VueDynamicAnimation
        },
        // 声明 props
        props: {
            style: {
                type: Object,
                default: function () {
                    return {}
                }
            },
            class: {
                type: Object,
                default: function () {
                    return {}
                }
            },
            normal: {
                type: Object,
                default: function () {
                    return {}
                }
            },
            enter: {
                type: Object,
                default: function () {
                    return {}
                }
            }, leave: {
                type: Object,
                default: function () {
                    return {}
                }
            }
        },
        data: function () {
            count++;
            return {
                show: true, size: {width: 0, height: 0}, windowSize: {}
            }
        },
        methods: {
            _update: function () {
                var size = this.$els.box.getBoundingClientRect();
                var show=(size.bottom>=0&&size.bottom<=this.windowSize.height+size.height)&&(size.right>=0&&size.right<=this.windowSize.width+size.width)
                if(this.show !=show){
                    this.show=show;
                }
            },
            showInfo: function (e) {
                //  this.show = !this.show;
                console.log(this.$els.dom.getBoundingClientRect())

            }
        },
        computed: {},
        ready: function () {
            var me = this;
            // this._update();
           setInterval(this._update, 100);
            var size = this.$els.box.getBoundingClientRect();
            this.size.width = size.width;
            this.size.height = size.height;
            Css.smartCss(this.$els.box, {width: size.width, height: size.height});
            //console.log(this.size);
            //  setTimeout(this._update, 2000)


            function onResize() {
                me.windowSize = Utils.windowSize();
            }


            var rm_widow = Event.windowEvent('resize', function () {
                onResize();
                me._update();
                // rm_widow();
            })
            var rm_scoll = Event.bindEvent(window, 'scroll', function () {
                me._update();
             //   console.log('scroll')
            });
            onResize()

        }
    }


</script>