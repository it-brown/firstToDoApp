<template lang='pug'>
.vue-create-task
        .head
            section.task-creation
                input-form-item(ref='formItem' :title='formItems[0].title' :type='formItems[0].type'
                    @apply-from-child='addTodo')
            section.ongoing-task-title
                p.ongoing-title Ongoing Tasks
        .content
            section.todo-task
                task-item(v-for='todo, i in todos' :key='i' :uniqueID='i' :todo='todo' :imgs='todoImgs'
                    @editTask='editTask' @deleteTask='deleteTask' @moveTask='moveToOngoingTask')
            section.ongoing-task
                task-item(v-for='ongoingTodo, j in ongoingTodos' :key='j' :uniqueID='j' :todo='ongoingTodo' :imgs='ongoingTodoImgs'
                    @editTask='editTask' @deleteTask='deleteTask' @moveTask='moveToOngoingTask' @archiveTask='archiveTask')
</template>

<script lang='ts'>
import Vue from 'vue';
import Component from 'vue-class-component';
import VueUtil from '@/scripts/util/VueUtil';
import TaskItem from '@/components/common/TaskItem.vue';
import InputFormItem from '@/components/common/InputFormItem.vue';
import InputFormItemProps from '@/components/common/InputFormItemProps';

export interface todoProps {
    id: number,
    time_stamp: Date,
    name: string,
    doing: boolean,
    editing: boolean,
}
/**
 * Vue Component
 */
@Component({
})
export default class CreateTask extends Vue {
    private uniqueID: number = 0;
    private todoImgs: Imgs = {
        right: require('@/resources/img/right.png'),
        trash: require('@/resources/img/trash-can.png'),
        begin: require('@/resources/img/right-arrow.png')
    }
    private ongoingTodoImgs: Imgs = {
        right: require('@/resources/img/right.png'),
        trash: require('@/resources/img/trash-can.png'),
        done: require('@/resources/img/fireworks.png')
    }
    private todos: todoProps[] = [];
    private ongoingTodos: todoProps[] = [];
    private archivedTodos: todoProps[] = [];
    private formItems: InputFormItemProps[] = [
        {title: 'Task Name', type: 'text'}
    ];
    addTodo(val: string) {
        const time: Date = new Date();
        this.todos.push({
            id: this.todos.length,
            time_stamp: time,
            name: val,
            doing: false,
            editing: false
        })
        console.log(this.todos);
    }
    moveToOngoingTask (id: number) {
        const target: todoProps = this.todos[id];
        if(target.editing == true) {alert('finish editing bofore you move you task');return;}
        target.doing = !target.doing;
        this.ongoingTodos.push(this.todos[id]);
        console.log(this.ongoingTodos);
        this.deleteTask(id);

    }
    checkIfDoing(text: string | undefined) {
        if(text == 'ongoing') {
            console.log("doing")
            return true
        }
        console.log("not doing")
        return false;
    }
    deleteTask(id: number, text?: string) {
        if(this.checkIfDoing(text) == true) {
            const target: todoProps = this.ongoingTodos[id];
            this.ongoingTodos = this.ongoingTodos.filter(function(v) {
                return v != target;
            })
            console.log('ongoingTodo deleted!');
        } else if (this.checkIfDoing(text) == false) {
            const target: todoProps = this.todos[id];
            this.todos = this.todos.filter(function(v) {
                return v != target;
            })
            console.log('todo deleted!');
        }
    }
    editTask(id: number, text?: string) {
        if(this.checkIfDoing(text) == true) {
            const target: todoProps = this.ongoingTodos[id];
            target.editing = !target.editing;
            console.log('ongoingTodo edited!');
        } else if (this.checkIfDoing(text) == false) {
            const target: todoProps = this.todos[id];
            target.editing = !target.editing;
            console.log('todo edited');
        }
    }
    archiveTask(id: number, text: string) {
        const target: todoProps = this.ongoingTodos[id];
        target.doing = !target.doing;
        this.archivedTodos.push(this.ongoingTodos[id]);
        console.log('archivedTodos -> ', this.archivedTodos);
        this.deleteTask(id, text);
        console.log('ongoingTodos -> ', this.ongoingTodos);
    }
}
</script>

<style lang='sass' scoped>
@import '../styles/color.sass'
@import '../styles/common.sass'
@import '../styles/Todo.sass'

.vue-create-task
        .head
            height: 80px
            width: 100vw
            display: flex
            align-items: center
            border-bottom: 1px dashed grey

            & > section.task-creation
                flex: 3

            & > section.ongoing-task-title
                flex: 2
                display: flex
                align-items: center
                justify-content: center

        .content
            height: calc(100vh - 80px)
            display: flex
            align-items: left

            & > section.todo-task
                flex: 3
                display: flex
                flex-wrap: wrap

            & > section.ongoing-task
                flex: 2
                display: flex
                flex-direction: column
                align-items: center

.hidden
    display: none
</style>
