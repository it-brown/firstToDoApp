<template lang='pug'>
.vue-sub
    .reactive-title {{ reactiveTitle() }}
    sample
</template>

<script lang='ts'>
import Vue from 'vue';
import Component from 'vue-class-component';
import VueUtil from '@/scripts/util/VueUtil';
import RootVue from '@/components/RootVue';

/**
 * Vue Component
 */
@Component({})
export default class Sub extends RootVue {
    public title: string = 'sub';

    public beforeCreate(): void {
        // Inner Vue 登録
        VueUtil.registerComponents([]);
    }
}
</script>

<style lang='sass'>
@import '../../styles/color.sass'
@import '../../styles/common.sass'

.vue-sub
</style>
