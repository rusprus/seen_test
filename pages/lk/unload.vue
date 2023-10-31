<template>
    <div class="main">
        <div class="tasks">
            <div class="block tasks__header" @click="getListUpload">
                <div class="text-bold">
                    <h1>Выгрузка</h1>
                </div>
                <div class="text-bold">
                    <h2>Выполняет работу:</h2>
                </div>
                <div>- Собирает фотографии из заказов пользователей.</div>
                <div>- Выгружает по папкам.</div>
            </div>

            <UnloadItem v-for="item in unloads" :key="item.id" :item="item" class="block card" :data-color="item.status" @click="isShow = !isShow" />
        </div>


        <div v-if="!isShow">
            <div class="info ">Для того чтобы посмотреть информацию <span class="text-bold">выгрузки </span>, а также ее скачать, нажмите на требуюмую выгрузку в столбце слева.</div>
        </div>

        <template v-if="isShow">
            <div class="notice" data-color="light-purple">
                <div class="block">
                    <div class="text-bold notice__block-h2">
                        <h2>Ссылка для скачивания архива Выгрузки (.zip):</h2>
                        <div>
                            <div class="link">{{ hrefUnloadItem }}</div>
                            <a class="span-link" @click="pushHrefToBuff(hrefUnloadItem)">cкопировать ссылку:</a>
                        </div>
                    </div>

                </div>
            </div>
        </template>
    </div>
</template>

<script>
import { useAPIFetch } from '@/composables/useAPIFetch.ts'
import UnloadItem from '@/components/UnloadItem.vue'

export default {

    data() {
        return {
            url: 'https://dev-cabinet.seenday.com/e.scripts?page=pages:unload&event=get',
            unloads: [],
            isShow: false,
            hrefUnloadItem: "https://dev-cabinet.seenday.com/e.scripts?page=pages:unload&event=get&unload_id=2175"
        }
    },

    methods: {


        async getListUpload() {
            const list = await useAPIFetch(this.url)
            this.unloads = JSON.parse(list?.data?._rawValue)?.response?.data
        },

        pushHrefToBuff(hrefItem) {
            navigator.clipboard.writeText(hrefItem);
        }
    },
    components: {
        UnloadItem
    },

    mounted() {
        this.getListUpload()
    }

}


</script>

<style>
.main {
    display: flex;
    font-size: 14px;
    line-height: 1.2;
    gap: 15px;
    padding: 15px;

}

.tasks {
    display: flex;
    flex-direction: column;
    gap: 15px;
    width: 35%;
}

.tasks__header {
    cursor: pointer;
}

.notice {
    flex: 1;
}

.info {
    padding: 15px;
    background-color: var(--color_secondary);
    flex: 1;
    border-radius: 5px;
}

@media (max-width: 769px) {

    .tasks,
    .notice {
        width: 100%;
    }

    .main {
        display: block;
    }
}

.block {
    padding: 15px;
}

.block-h1 {
    font-size: 18px;
}

@media (max-width: 769px) {
    .block-h1 {
        font-size: 20px;
    }
}


.block-h2 {}

.notice__block-h2 {}

.card {
    padding: 15px;
    /* border-left: 10px var(--color_success) solid; */
    border-radius: 3px;
    font-size: 14px;
}

.text-bold {
    font-weight: bold;
}


[data-color="green"] {
    /* background-color: green; */
    border-left: 10px var(--color_success) solid;
}

[data-color="red"] {
    /* background-color: light-purple; */
    border-left: 10px var(--color_danger) solid;
}
</style>