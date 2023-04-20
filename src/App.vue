<template>
    <div class="main">
        <directory-side/>
        <tasks-side/>
    </div>
</template>


<script>
import DirectorySide from '@/components/DirectorySide.vue';
import TasksSide from '@/components/TasksSide.vue';
export default {
    name: 'App',
    components: {
        DirectorySide, TasksSide
    },
}
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

.main{
    display: flex;
    flex-direction: row;
    align-self: flex-end;
    
    width: 800px;
    min-height: 600px;
    
    border: 1px solid black;
}
</style>
