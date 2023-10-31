<template>
    <div class="card">
        <p><span v-html="item.task_date"></span></p>
        <p>Статус задачи: <span class="text-bold">{{ item.status_text }}</span></p>
        <p>ID выгрузки: <span class="text-bold">{{ item.id }}</span> </p>
        <p> <span v-html="item.event"></span></p>
        <p>Размер выгрузки: <span class="text-bold">{{ item?.size }}</span></p>
    </div>
</template>
<script>
export default {
    props: ['item'],
}
</script>
<style></style>

