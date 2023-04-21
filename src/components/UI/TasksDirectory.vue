<template>
    <button
        class="btn-directory"
        @click="$emit('showTargetTasks')"
    >
        <slot></slot>
        <button
            class="delete-directory"
            @click="$emit('deleteDirectory', directory)"
        >
            <img src="@/assets/image/delete-directory.png" alt="X">
        </button>

    </button>
</template>

<script>
export default {
    name: 'tasks-directory',
    props:{
        directory:{
            type: Object
        }
    }
};
</script>

<style>
.btn-directory{
    display: flex;
    align-items: center;
    gap: 10px;

    width: 160px;
    height: 37px;

    padding: 0 10px;
    border: none;
    background: none;
    
    text-align: left;
    font-size: 16px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;

}
.btn-directory:hover{
    min-height: 37px;
    height: auto;

    background-color: white;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.03);
    border-radius: 4px;

    white-space: normal;
}

.btn-directory .delete-directory{
    display: none;
}

.btn-directory:hover .delete-directory{
    display: block;
    margin-left: auto;

    cursor: pointer;
    background: none;
    border: none;
}
</style>
