<template>
    <div class="left-side">
        <!--Показать все папки с задачами-->
        <button
            class="btn-directory all-tasks"
            @click="showAllTasks"
        >
            <img src="@/assets/image/all-tasks.png">
            <span>Все задачи</span>
        </button>
        
        <!--Список папок-->
        <div class="all-directories">
            <tasks-directory
                @showTargetTasks="showTargetTasks"
                @deleteDirectory="deleteDirectory"
                v-for="directory in directories" :key="directory.id"
                :directory="directory"
            >
                <span>{{ directory.title }}</span>
            </tasks-directory>
        </div>
        
        <!--Добавить папку с задачами-->
        <button
            class="add-directory"
            @click="addDirectory"
        >
            <img src="@/assets/image/add-directory.png">
            <span>Добавить папку</span>
            
            <!--Диалоговое окно-->
            <div
                class="modal-add-directory"
            >
                <button class="close-modal">
                    <img src="@/assets/image/close-modal.png" alt="X">
                </button>
                <input
                    placeholder="Название папки"
                    type="text"
                    name=""
                    id=""
                >
                <!--choose-color-->
                <button
                    class="add-directory-btn"
                >
                    Добавить
                </button>
            </div>
        </button>
    </div>
</template>

<script>
import TasksDirectory from '@/components/UI/TasksDirectory.vue';

export default {
    components: {
        TasksDirectory
    },
    data(){
        return{
            directories:[
                {id: 1, markcolor: '#f09797', title: 'Покупки', tasks:[
                        {description: 'Макарошки'},
                        {description: 'Картошки'},
                        {description: 'Биткоины'}
                    ]},
                {id: 2, markcolor: '#97b8f0', title: 'Фронтенд', tasks:[
                        {description: 'Изучить JavaScript'},
                        {description: 'Изучить паттерны проектирования'},
                        {description: 'ReactJS Hooks (useState, useReducer, useEffect и т.д.)'},
                        {description: 'Redux (redux-observable, redux-saga)'}
                    ]},
                {id: 3, markcolor: '#97f0aa', title: 'Фильмы и сериалы', tasks:[
                        {description: 'Люцифер'},
                        {description: 'Флэш'},
                        {description: 'Главный герой'}
                    ]},
                {id: 4, markcolor: '#e8cf89', title: 'Книги', tasks:[
                        {description: 'Основы прототипирования в Figma'},
                        {description: 'Богатый папа, бедный папа'},
                        {description: 'По соображениям совести'}
                    ]},
                {id: 5, markcolor: '#e681b3', title: 'Личное', tasks:[
                        {description: 'Пригласить девушку в кино в сб'},
                        {description: 'Записаться в зал'},
                        {description: 'Записаться в СПА'}
                    ]},
            ]
        }
    },
    methods:{
        showAllTasks(){
            console.log("showAllTasks")
        },
        showTargetTasks(){
            console.log("showTargetTasks")
        },
        deleteDirectory(directory){
            console.log("deleteDirectory")
            this.directories = this.directories.filter(d => d.id !== directory.id)

        },
        addDirectory(){
            console.log("addDirectory")
        }
    },
};
</script>

<style scoped>
/* Сторона с папками (Общее) */
.left-side{
    display: flex;
    flex-direction: column;
    gap: 25px;
    
    width: 200px;
    height: auto;
    padding: 30px 20px;
    
    background-color: #fcf2f2;
}
/* End */


/* Все папки (begin) */
.all-directories{
    display: flex;
    flex-direction: column;
}
/* Все папки (end) */


/* Добавление папки (begin) */
/* Main button (begin) */
.add-directory{
    position: relative;

    width: 100%;
    height: 40px;
    
    cursor: pointer;
    padding: 0 10px;
    border: none;
    background: none;
    
    color: #878787;
    text-align: left;
    font-size: 16px;
}

.add-directory img{
    margin-right: 10px;
}
/* Main button (End) */


/* Modal window (begin) */
.modal-add-directory{
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 13px;
    
    position: absolute;
    width: 220px;
    height: 150px;
    padding: 17px;
    
    cursor: default;
    background: #FFFFFF;
    box-shadow: 0px 6px 10px rgba(0, 0, 0, 0.05);
    border-radius: 10px;
}

.close-modal{
    position: absolute;
    top: -10px;
    right: -10px;
    
    width: 20px;
    height: 20px;
    border: none;
    background: none;
}

.modal-add-directory input{
    width: 200px;
    height: 32px;
    padding: 7px 10px;
    
    border: 1px solid #EFEFEF;
    border-radius: 4px;
    
    font-size: 16px;
}
/* Modal window (end) */
/* Добавление папки (end) */
</style>
