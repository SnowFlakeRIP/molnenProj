<template>
    <div class="right-side">
        <div class="title-container">
            <h2 class="title">{{ directoryTask }}</h2>
            <img src="" alt="Редактировать">
        </div>
        <div class="line">asd</div>
        <div class="tasks-container">

        </div>
    </div>
</template>

<script>
export default {
    name: 'tasks-side',
    props:{
        directoryTask:{
            type: Object,
            default() {
                return {}
            },
        }
    }
};
</script>

<style scoped>

</style>
