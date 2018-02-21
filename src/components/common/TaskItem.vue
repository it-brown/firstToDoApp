<template lang='pug'>
.vue-task-item
        p.task-title(:class='{ hidden: todo.editing}' @click='triggerEditTask(uniqueID)')
            | {{ todo.name }}
        input.task-title(v-if='todo.editing' v-on:keyup.enter='triggerEditTask(uniqueID)'
            v-model='todo.name')
        .edit-bar
            .toggle-edit-bar
                img(:src='imgs["right"]')
            .erase-button
                img(:src='imgs["trash"]' @click='triggerDeleteTask(uniqueID)')
            .move-to-ongoing(v-if='imgs["begin"]')
                img(:src='imgs["begin"]' @click='triggerMoveTask(uniqueID)')
            .move-to-ongoing(v-if='imgs["done"]')
                img(:src='imgs["done"]' @click='triggerArchiveTask(uniqueID)')

</template>

<script lang='ts'>
import Vue from 'vue';
import Component from 'vue-class-component';
import VueUtil from '@/scripts/util/VueUtil';
import todoProps from '@/components/CreateTask.vue';

/**
 * Vue Component
 */
@Component({
    props:  {
        uniqueID: Number,
        imgs: Object,
        todo: Object
    }
})
export default class TaskItem extends Vue {
    check() {
        if(this.$props.todo.doing == true) {
            return true;
        }else {
            console.log("else");
            return false;
        }
    }
    triggerEditTask(key: number) {
        if(this.check() == false) {
            console.log(this.$props)
            this.$emit('editTask', this.$props.uniqueID);
        }else if (this.check() == true){
            console.log(this.$props.uniqueID);
            this.$emit('editTask', this.$props.uniqueID, 'ongoing');
        }
    }
    triggerDeleteTask(key: number) {
        if(this.check() == false) {
            this.$emit('deleteTask', this.$props.uniqueID);
        }else if (this.check() == true){
            console.log(this.$props.uniqueID);
            this.$emit('deleteTask', this.$props.uniqueID, 'ongoing');
        }
    }
    triggerMoveTask(key: number) {
        if(this.check() == false) {
            this.$emit('moveTask', this.$props.uniqueID);
        }else if (this.check() == true){
            this.$emit('moveTask', this.$props.uniqueID, 'ongoing');
        }
    }
    triggerArchiveTask(key: number) {
        this.$emit('archiveTask', this.$props.uniqueID, 'ongoing');
    }
}
</script>

<style lang='sass' scoped>
.vue-task-item
    display: flex
    width: calc(100vw * 3 / 5  / 2 - 6px)
    height: 50px
    display: flex
    border: 1px solid black
    overflow: hidden
    margin: 3px
    border-radius: 3px

    & > .task-title
        flex: 5
        overflow: scroll

    & > .edit-bar
        flex: 2
        display: flex
        $a: calc(100vw * 3 / 5  / 2 * 2 / 7 * 2 / 3)
        transform: translateX($a)

        & >  .toggle-edit-bar, .move-to-ongoing, .erase-button
            display: flex
            justify-content: center
            align-items: center
            flex: 1
            border-left: 1px dashed black
    & > .edit-bar:hover
        transform: translateX(0)
        .toggle-edit-bar
            display: none
.hidden
    display: none
</style>
