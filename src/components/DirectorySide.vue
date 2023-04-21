<template>
    <div class="left-side">
        <div class="content-container">
            <!--Показать все папки с задачами-->
            <button
                class="btn-directory all-tasks"
                @click="showAllTasks"
            >
                <img src="@/assets/image/all-tasks.png" alt="">
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
                    <div class="mark-style" :style="`background-color: ${directory.markcolor}`"></div>
                    <span>{{ directory.title }}</span>
                    
                </tasks-directory>
            </div>
            <tasks-side style="display: none"
                        :directory-task="activeDirectory"
            ></tasks-side>
    
            <!--Добавить папку с задачами-->
            <button
                class="add-directory"
                @click="showModalWindow"
            >
                <img src="@/assets/image/add-directory.png" alt="">
                <span>Добавить папку</span>
            </button>
            <!--Диалоговое окно-->
            <div
                class="modal-add-directory"
            >
                <!--Закрыть диалоговое окно-->
                <button class="close-modal">
                    <img src="@/assets/image/close-modal.png" alt="X">
                </button>
                <!--Название папки-->
                <input
                    placeholder="Название папки"
                    type="text"
                    name=""
                    id=""
                >
                <!--Выбор цвета марки-->
                <div class="color-container">
                    <choose-color
                        v-for="color in defaultColor" :key="color.id"
                        :current-color="color"
                        @eraseTargetColor="eraseTargetColor"
                    />
                </div>
                <!--Добавить новую папку-->
                <button
                    class="add-directory-btn"
                    @click="addDirectory"
                >
                    Добавить
                </button>
            </div>
        </div>
    </div>
</template>

<script>
import TasksDirectory from '@/components/UI/TasksDirectory.vue';
import ChooseColor from '@/components/UI/ChooseColor.vue';
import TasksSide from '@/components/TasksSide.vue';
export default {
    components: {
        TasksDirectory, ChooseColor, TasksSide
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
            ],
            defaultColor:[
                {id: 1, color: '#C9D1D3', status: true}, {id: 2, color: '#42B883', status: false},
                {id: 3, color: '#64C4ED', status: false}, {id: 4, color: '#FFBBCC', status: false},
                {id: 5, color: '#B6E6BD', status: false}, {id: 6, color: '#C355F5', status: false},
                {id: 7, color: '#09011A', status: false}, {id: 8, color: '#FF6464', status: false},
            ],
            nameNewDirectory: '',
            colorNewDirectory: '',
            activeDirectory: {},
        }
    },
    methods:{
        showAllTasks(){
            console.log("showAllTasks")
        },
        showTargetTasks(directory){
            console.log(directory)
            console.log("showTargetTasks")
            this.activeDirectory = {
                ...directory,
            }
        },
        showModalWindow(){
            console.log("showModalWindow")
        },
        deleteDirectory(directory){
            console.log("deleteDirectory")
            this.directories = this.directories.filter(d => d.id !== directory.id)
        },
        eraseTargetColor(){
          console.log("ok")
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
    width: 200px;
    height: auto;
    padding: 30px 20px;
    
    background-color: #fcf2f2;
}

.content-container{
    display: flex;
    flex-direction: column;
    gap: 25px;
    
    position: relative;
    height: auto;
}
/* End */


/* Все папки (begin) */
.all-directories{
    display: flex;
    flex-direction: column;
    
    height: auto;
}

.mark-style{
    flex-shrink: 0;
    
    width: 10px;
    height: 10px;
    border-radius: 10px;
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
    bottom: -150px;
    left: 10px;

    width: 235px;
    height: 150px;
    padding: 17px;
    
    cursor: default;
    background: #FFFFFF;
    box-shadow: 0 6px 10px rgba(0, 0, 0, 0.05);
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

.color-container{
    display: flex;
    gap: 5px;
}

.add-directory-btn{
    width: 200px;
    height: 37px;
    
    background: #4DD599;
    border-radius: 4px;
    border: none;
    
    color: #FFFFFF;
    font-size: 14px;
}
/* Modal window (end) */
/* Добавление папки (end) */
</style>
