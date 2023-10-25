<template>
    <main class="upload">
        <section class="upload_info">
            <UploadBlock class="upload_cards-main_block">

                <h3 class="upload_cards-main_block-heading"><span class="text-bold">Выгрузка</span></h3>
                <p><span class="text-bold">Выполняет работу:</span></p>
                <p>- Собирает фотографии из заказов пользователей.</p>
                <p>- Выгружает по папкам.</p>

            </UploadBlock>
        </section>

        <section class="upload_details" data-color="light-purple">
            <div class="upload_details_wrapper">
                <UploadBlockDetails v-if="Number(selectedId)" :selected-id="selectedId" @close="selectedId = null"
                    :key="selectedId" />
                <UploadNotice v-else>
                    Для того, чтобы просмотреть информацию о выгрузке, а также ее скачать, нажмите на требуемую выгрузку в
                    столбце слева.
                </UploadNotice>
            </div>
        </section>

        <section class="upload_cards">
            <UploadCard v-for="upload in uploads" :key="upload.id"
                :class="{ 'color_success': (upload.status == 'green'), 'color_danger': (upload.status == 'red') }"
                @click="showSelectedUploadDetails(upload.id)">

                <p v-if="upload.task_date" v-html="upload.task_date"></p>
                <p v-if="upload.status_text">Статус задачи: <span class="text-bold">{{ upload.status_text }}</span></p>
                <p v-if="upload.id">ID Выгрузки: <span class="text-bold">{{ upload.id }}</span></p>
                <p v-if="upload.event" v-html="upload.event"></p>
                <p v-if="upload.size">Размер выгрузки: <span class="text-bold">{{ upload.size }}</span></p>

            </UploadCard>
        </section>
    </main>
</template>

<script setup>
const selectedId = ref(null)

const { data } = await useAPIFetch('e.scripts', {
    params: {
        page: 'pages:unload',
        event: 'get',
    }
}).catch((err) => console.log(err));
const uploads = computed(() => {
    if (data && data.value) {
        const parsedData = JSON.parse(data.value);

        if (parsedData.response.status === 1) {
            return parsedData.response.data;
        }
    }
    return [];
})

function showSelectedUploadDetails(id) {
    selectedId.value = id;
}
</script>

<style lang="scss">
.upload {
    position: relative;
    display: grid;
    grid-gap: 15px;
    grid-template-columns: 1rf;

    @include start-at("xslg") {
        grid-template-columns: 35% calc(65% - 15px);
    }


    &_cards {
        /* Hide scrollbar for IE, Edge and Firefox */
        // -ms-overflow-style: none; /* IE and Edge */
        // scrollbar-width: none; /* Firefox */

        @include start-at("xslg") {
            // max-height: 100vh;
            // overflow: auto;
        }

        &-main_block {
            &-heading {
                font-size: 18px;

                @include start-at("xslg") {
                    font-size: 20px;
                }
            }
        }
    }

    /* Hide scrollbar for Chrome, Safari and Opera */
    // &_cards::-webkit-scrollbar {
        // display: none;
    // }

    &_details {
        position: -webkit-sticky;
        position: sticky;
        top: 0;
        z-index: 9999;
        margin-top: 15px;
        grid-area: "details";

        @include start-at("xslg") {
            margin-top: 0;
        }
    }
}
</style>
  