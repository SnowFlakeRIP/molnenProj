<template>
    <div class="right-side">
        <h2
            v-if="directories.length == 0"
            class="zero-tasks"
        >
            Задачи отсутствуют
        </h2>
        <show-tasks
            v-for="directory in directories" :key="directory.title"
        >
            <!--Заголовок-->
            <div class="rename-title" v-if="inputRenameVisibility">
                <input
                    type="text"
                    v-bind:value="newNameDirectory"
                    @input="inputRename"
                >
                <img
                    src="@/assets/image/ok.png"
                    alt="OK"
                >
            </div>
            <div class="title-container" else>
                <h2
                    class="title"
                    :style="`color: ${directory.markcolor}`"
                >{{ directory.title }}</h2>
                <img
                    v-if="renameVisibility"
                    @click="renameDirectory"
                    src="@/assets/image/rename-title.png"
                    alt="Редактировать"
                >
            </div>
            
            
            <div class="line"></div>
            
            <!--Задачи-->
            <div class="tasks-container">
                <check-task
                    v-for="task in directory.tasks" :key="task.description"
                    :current-task="task"
                >
                    <div class="task-container">
                        <div class="non-checked-image"
                             v-if="task.checked === false"
                             @click="$emit('changeTask', task)"
                        ></div>
                        <div class="checked-image"
                             v-else
                             @click="$emit('changeTask', task)"
                        ></div>
                        <span>{{ task.description }}</span>
                        <button
                            class="delete-task"
                            @click="$emit('deleteTask', task, directory)"
                        >
                            <img src="@/assets/image/delete-directory.png" alt="X">
                        </button>
                    </div>
                </check-task>
                
                <!--Кнопка для записи новой задачи-->
                <button
                    class="add-task-btn"
                    v-if="btnTaskVisibility"
                    @click="openAddTaskContainer"
                >
                    <img
                        src="@/assets/image/add-task.png"
                        alt="+"
                    >
                    <span>Новая задача</span>
                </button>
                
                <!--Окно записи новой задачи-->
                <div
                    class="add-task-container"
                    v-if="addTaskVisibility"
                >
                    <input
                        type="text"
                        placeholder="Текст задачи"
                        v-bind:value="nameNewTask"
                        @input="inputTask"
                    >
                    
                    <div class="add-btns">
                        <button
                            class="add-task"
                            @click="$emit('addNewTask', nameNewTask, directory.tasks); checkAddTaskContainer();"
                        >
                            Добавить задачу
                        </button>
                        <button
                            class="close-add-task-container"
                            @click="closeAddTaskContainer"
                        >
                            Отмена
                        </button>
                    </div>
                </div>
            </div>
        </show-tasks>
    </div>
</template>

<script>
import ShowTasks from '@/components/UI/ShowTasks.vue';
import CheckTask from '@/components/UI/CheckTask.vue';

export default {
    name: 'tasks-side',
    data(){
        return{
            addTaskVisibility: false,
            newNameDirectory: '',
            nameNewTask: '',
            inputRenameVisibility: false
        }
    },
    components:{
        ShowTasks, CheckTask
    },
    props:{
        directories:{
            type: Array
        },
        modalVisibility:{
            type: Boolean
        },
        btnTaskVisibility:{
            type: Boolean
        },
        renameVisibility:{
            type: Boolean
        },
    },
    methods:{
        openAddTaskContainer(){
            this.addTaskVisibility = true
        },
        closeAddTaskContainer(){
            this.addTaskVisibility = false
            this.nameNewTask = ''
        },
        checkAddTaskContainer(){
              if(this.nameNewTask)
                  this.closeAddTaskContainer()
        },
        renameDirectory(){
            this.inputRenameVisibility = true
            this.newNameDirectory = this.directories[0].title
        },
        inputTask(event){
            this.nameNewTask = event.target.value
        },
        inputRename(event){
            this.newNameDirectory = event.target.value
        }
    }
};
</script>

<style lang="scss" scoped>
.right-side{
    position: relative;
    
    width: 100%;
    height: auto;
    
    padding: 55px;
}

.zero-tasks{
    position: absolute;
    top: 45%;
    left: 25%;
    
    font-size: 32px;
    line-height: 39px;
    color: #C9D1D3;
}

.title-container{
    display: flex;
    align-items: baseline;
    gap: 15px;
    
    .title{
        margin-bottom: 20px;
        
        font-size: 32px;
        line-height: 39px;
        color: #64C4ED;
    }
    
    img{
        width: 15px;
        height: 15px;
        
        cursor: pointer;
    }
}

.line{
    width: 100%;
    height: 1px;
    margin-bottom: 30px;
    
    background: #F2F2F2;
}

.tasks-container{
    display: flex;
    flex-direction: column;
    gap: 20px;
    
    margin-bottom: 40px;
    
    .task-container{
        display: flex;
        gap: 15px;
        align-items: center;
        
        .non-checked-image{
            width: 20px;
            height: 20px;
        
            cursor: pointer;
            background: url(@/assets/image/non-checked.png);
    
            &:hover{
                background: url(@/assets/image/hover-checked.png);
            }
        }
    
        .checked-image{
            width: 20px;
            height: 20px;
        
            cursor: pointer;
            background: url(@/assets/image/checked.png);
        }
    }
    
    .add-task-btn{
        display: flex;
        gap: 17px;
        align-items: center;
        
        margin: 3px 0 0 3px;
    
        cursor: pointer;
        border: none;
        background: none;
        
        color: #B4B4B4;
        
        img{
            width: 14px;
            height: 14px;
        }
    }
}

.add-task-container{
    display: flex;
    flex-direction: column;
    gap: 16px;
    
    input{
        width: 415px;
        height: 38px;
        padding: 8.5px 14px;
    
        background: #FFFFFF;
        border: 1px solid #EFEFEF;
        border-radius: 4px;
    
        font-size: 14px;
        
        &::placeholder{
            color: #C7C7C7;
        }
    }
    
    .add-btns{
        display: flex;
        gap: 9px;
        
        .add-task{
            width: 145px;
            height: 34px;
    
            cursor: pointer;
            color: #FFFFFF;
            background: #4DD599;
            border-radius: 4px;
            border: none;
        }
        
        .close-add-task-container{
            width: 90px;
            height: 34px;
    
            cursor: pointer;
            color: #9C9C9C;
            background: #F4F6F8;
            border-radius: 4px;
            border: none;
        }
    }
}
</style>
