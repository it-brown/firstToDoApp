<template lang='pug'>
.vue-input-form-item
    label {{ $props.title }}
    input(v-if='$props.type == "text"' v-model='todo' v-on:keyup.enter='apply')
</template>

<script lang='ts'>
import Vue from 'vue';
import Component from 'vue-class-component';
import VueUtil from '@/scripts/util/VueUtil';

/**
 * Vue Component
 */
@Component({
    props: {
        title: String,
        type: String
    }
})
export default class InputFormItem extends Vue {
    private todo: string = 'hoge';
    apply() {
        this.$emit('apply-from-child', this.todo);
    }
}
</script>

<style lang='sass' scoped>
.vue-input-form-item
    display: flex
    padding: 10px

    label
        display: flex
        align-items: flex-end
        justify-content: center
        flex: 1
        margin-right: 8px

    input
        display: flex
        flex: 2
        padding: 8px
</style>
