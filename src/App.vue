<template>
    <div class="main">
        <div class="left-side">
            <div class="content-container">
                <!--Показать все папки с задачами-->
                <button
                    v-if="directories.length != 0"
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
                        {{ directory.title }}
                    </tasks-directory>
                </div>
                <!--Добавить папку с задачами-->
                <button
                    class="add-directory"
                    @click="showModalWindow"
                >
                    <img src="@/assets/image/add-directory.png" alt="">
                    Добавить папку
                </button>
                <!--Диалоговое окно-->
                <div
                    v-if="modalVisibility"
                    class="modal-add-directory"
                >
                    <!--Закрыть диалоговое окно-->
                    <button
                        class="close-modal"
                        @click="showModalWindow"
                    >
                        <img src="@/assets/image/close-modal.png" alt="X">
                    </button>
                    <!--Название папки-->
                    <input
                        placeholder="Название папки"
                        type="text"
                        v-bind:value="nameNewDirectory"
                        @input="inputTitle"
                    >
                    <!--Выбор цвета марки-->
                    <div class="color-container">
                        <choose-color
                            v-for="color in defaultColor" :key="color.id"
                            :current-color="color"
                            @chooseColor="chooseColor"
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
        
        <tasks-side
            :directories="activeDirectory"
            :modalVisibility="modalVisibility"
            :renameVisibility="renameVisibility"
            :btnTaskVisibility="btnTaskVisibility"
            @changeTask="changeTask"
            @deleteTask="deleteTask"
            @addNewTask="addNewTask"
        />
    </div>
</template>


<script>
import TasksSide from '@/components/TasksSide.vue';
import TasksDirectory from '@/components/UI/TasksDirectory.vue';
import ChooseColor from '@/components/UI/ChooseColor.vue';
export default {
    name: 'App',
    components: {
        TasksSide, TasksDirectory, ChooseColor,
    },
    data(){
        return{
            directories:[
                {status: false, markcolor: '#f09797', title: 'Покупки', tasks:[
                        {description: 'Макарошки', checked: false},
                        {description: 'Картошки', checked: false},
                        {description: 'Биткоины', checked: false}
                    ]},
                {status: false, markcolor: '#97b8f0', title: 'Фронтенд', tasks:[
                        {description: 'Изучить JavaScript', checked: false},
                        {description: 'Изучить паттерны проектирования', checked: false},
                        {description: 'ReactJS Hooks (useState, useReducer, useEffect и т.д.)', checked: false},
                        {description: 'Redux (redux-observable, redux-saga)', checked: false}
                    ]},
                {status: false, markcolor: '#97f0aa', title: 'Фильмы и сериалы', tasks:[
                        {description: 'Люцифер', checked: false},
                        {description: 'Флэш', checked: false},
                        {description: 'Главный герой', checked: false}
                    ]},
                {status: false, markcolor: '#e8cf89', title: 'Книги', tasks:[
                        {description: 'Основы прототипирования в Figma', checked: false},
                        {description: 'Богатый папа, бедный папа', checked: false},
                        {description: 'По соображениям совести', checked: false}
                    ]},
                {status: false, markcolor: '#e681b3', title: 'Личное', tasks:[
                        {description: 'Пригласить девушку в кино в сб', checked: false},
                        {description: 'Записаться в зал', checked: false},
                        {description: 'Записаться в СПА', checked: false}
                    ]},
            ],
            defaultColor:[
                {color: '#C9D1D3', status: false}, {color: '#42B883', status: false},
                {color: '#64C4ED', status: false}, {color: '#FFBBCC', status: false},
                {color: '#B6E6BD', status: false}, {color: '#C355F5', status: false},
                {color: '#09011A', status: false}, {color: '#FF6464', status: false},
            ],
            nameNewDirectory: '',
            colorNewDirectory: '',
            activeDirectory: [],
            modalVisibility: false,
            btnTaskVisibility: true,
            renameVisibility: true,
        }
    },
    methods:{
        showAllTasks(){
            this.directories.forEach(el => el.status = false)
            this.activeDirectory = []
            this.activeDirectory = [
                ...this.directories
            ]
            this.btnTaskVisibility = false
            this.renameVisibility = false
        },
        showTargetTasks(directory){
            this.activeDirectory = []
            this.activeDirectory.push(directory)
            this.directories.forEach(el => el.status = false)
            directory.status = true;
            this.btnTaskVisibility = true
            this.renameVisibility = true
        },
        showModalWindow(){
            this.modalVisibility = !this.modalVisibility
            if(this.activeDirectory.length != this.directories.length)
            {
                this.renameVisibility = !this.renameVisibility
                this.btnTaskVisibility = !this.btnTaskVisibility
            }
        },
        deleteDirectory(directory){
            this.directories = this.directories.filter(d => d.title !== directory.title)
    
            // список задач текущей папки удаляется, если текущую папку удаляют
            if (this.activeDirectory.length == 1 && directory.title === this.activeDirectory[0].title)
                this.activeDirectory = []
            
            // список всех задач обновляется, если удаляют папку во время просмотра всех задач
            if (this.directories.length < this.activeDirectory.length)
                this.activeDirectory = [
                    ...this.directories
                ]
        },
        inputTitle(event){
            this.nameNewDirectory = event.target.value
        },
        chooseColor(clickedColor){
            this.defaultColor.forEach(el => el.status = false)
            clickedColor.status = true
            this.colorNewDirectory = clickedColor.color
        },
        addDirectory(){
            if(!this.colorNewDirectory && !this.nameNewDirectory)
                alert('Введите название папки и выберите цвет метки.')
            else if(!this.nameNewDirectory)
                alert('Введите название папки.')
            else if(!this.colorNewDirectory)
                alert('Выберите цвет метки.')
            else{
                this.directories.push({status: false, markcolor: this.colorNewDirectory, title: this.nameNewDirectory, tasks: []})
                this.defaultColor.forEach(el => el.status = false)
                this.nameNewDirectory = ''
                this.colorNewDirectory = ''
                this.showModalWindow()
            }
        },
        changeTask(emitTask){
            emitTask.checked = !emitTask.checked
        },
        deleteTask(emitTask, emitDirectory){
            emitDirectory.tasks = emitDirectory.tasks.filter(t => t.description !== emitTask.description)
        },
        addNewTask(newDescription, currentTasks){
            if (!newDescription)
                alert('Введите описание задачи')
            else{
                currentTasks.push({description: newDescription, checked: false})
            }
        }
    },
}
</script>

<style lang="scss">
@font-face {
    font-family: 'Lato500';
    src: url('~@/assets/fonts/Lato-Bold.ttf') format('ttf');
    font-style: normal;
}
@font-face {
    font-family: 'Lato600';
    src: url('~@/assets/fonts/Lato-Black.ttf') format('ttf');
    font-weight: 900;
    font-style: normal;
}
@font-face {
    font-family: 'Montserrat';
    src: url('~@/assets/fonts/Lato-Black.ttf') format('ttf');
    font-weight: 700;
    font-style: normal;
}

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

span, p{
    font-family: 'Lato500', sans-serif;
    font-weight: 500;
    font-size: 16px;
    line-height: 19px;
    letter-spacing: 0.2px;
}
button, ::placeholder{
    font-family: 'Lato600', sans-serif;
    font-weight: 600;
    font-size: 14px;
    line-height: 17px;
    letter-spacing: 0.15px;
}
h2{
    font-family: 'Montserrat', sans-serif;
    font-weight: 700;
}

.main{
    display: flex;
    flex-direction: row;
    align-self: flex-end;
    
    width: 800px;
    min-height: 600px;
    
    border: 1px solid black;
}

/* Сторона с папками (Общее) */
.left-side{
    min-width: 200px;
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
    z-index: 10;
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
    
    cursor: pointer;
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
    
    &::placeholder{
         color: #C7C7C7;
     }
}

.color-container{
    display: flex;
    gap: 5px;
}

.add-directory-btn{
    width: 200px;
    height: 37px;
    
    cursor: pointer;
    background: #4DD599;
    border-radius: 4px;
    border: none;
    
    color: #FFFFFF;
    font-size: 14px;
}
/* Modal window (end) */
/* Добавление папки (end) */
</style>
