<template lang='pug'>
.vue-index
    create-task
</template>

<script lang='ts'>
import Vue from 'vue';
import Component from 'vue-class-component';
import VueUtil from '@/scripts/util/VueUtil';
import RootVue from '@/components/RootVue';
import CreateTask from '@/components/CreateTask.vue';
import TaskItem from '@/components/common/TaskItem.vue';
import InputFormItem from '@/components/common/InputFormItem.vue';

/**
 * Vue Component
 */
@Component({})
export default class Index extends RootVue {
    public title: string = 'index';

    protected beforeCreate(): void {
        // Inner Vue 登録
        VueUtil.registerComponents([CreateTask, TaskItem, InputFormItem,]);
    }
}
</script>

<style lang='sass'>
@import '../../styles/color.sass'
@import '../../styles/common.sass'
@import '../../styles/Todo.sass'

.vue-index
    height: $index-vue-height
    width: $index-vue-width
</style>
