<template>
    <UploadBlock>
        <div class="block_header">
            <div class="block_header_num">
                {{ upload.id }}
            </div>

            <div class="block_header_heading" v-html="upload.event"></div>

            <div class="block_header_close">
                <button class="btn btn-close" @click="$emit('close')">X</button>
            </div>
        </div>
        <div class="block_body">
            <p class="block_body_notice">
                <i class="fa-solid fa-lightbulb-on" style="color: #6f6eab;"></i>
                Если после клика загрузка не пошла, проверьте не блокирует ли браузер
                скачивание архива
            </p>
            <p>
                <span class="text-bold">Ссылка для скачивания архива Выгрузки (.zip):</span><br>
                <a class="link" href="#">https://seenday.com/mdmsLKJhhds</a>
                <button class="span-link" @click="copyLink('https://seenday.com/mdmsLKJhhds')">
                    скопировать ссылку
                </button>

                <i v-if="linkCoppied" style="color: green;" class="fa-solid fa-check"></i>
            </p>
        </div>
        <div class="block_footer">
            <button class="btn btn-close btn-lg" @click="$emit('close')">Закрыть</button>
        </div>

    </UploadBlock>
</template>

<script setup>
const props = defineProps({
    selectedId: String,
})
const { selectedId } = props;

const linkCoppied = ref(false);
const data = ref({})

onMounted(() => {
    const params = {
        page: 'pages:unload',
        event: 'get',
        unload_id: selectedId
    }
    useAPIFetch('e.scripts', { params }).then((res) => {
        data.value = res.data;
    }).catch((err) => console.log(err));
})

const upload = computed(() => {
    if (data.value && data.value.value) {
        const parsedData = JSON.parse(data.value.value);

        if (parsedData.response.status === 1) {
            return parsedData.response.data[0] ?? [];
        }
    }
    return [];
})

function copyLink(link) {
    if (navigator.clipboard) {
        navigator.clipboard.writeText(link).then(() => {
            linkCoppied.value = true;
            setTimeout(() => {
                linkCoppied.value = false;
            }, 1500)
        }, function (err) {
            console.err('Error to copy link: ', err);
        });
    } else {
        // TODO: document.execCommand('copy')
        console.log('Your browser does not support navigator')
    }
}
</script>

<style lang="scss">
.block_header {
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    width: 100%;
    display: grid;
    grid-template-areas:"a a b b b b b c";

    &_num {
        padding: 10px;
        background-color: purple;
        color: white;
        grid-area: a;
        text-align: center;
    }

    &_heading {
        background-color: #f5f6f6;
        padding: 10px;
        width: 100%;
        grid-area: b;
    }

    &_close {
        background-color: red;
        padding-top: 3px;
        padding-bottom: 3px;
        display: none;
        grid-area: c;
        text-align: center;

        @include start-at("xslg") {
            display: block;
        }
    }
}

.block_body {
    &_notice {
        margin-top: 15px;
        margin-bottom: 15px;
        padding: 10px;
        background-color: #e5e5e5;
        border-radius: 3px;
        margin-top: 55px;
    }
}

.block_footer {
    padding-top: 15px;
    text-align: right;

    @include start-at("xslg") {
        display: none;
    }
}

.span-link {
    --muted_color: darkgray;
    color: var(--muted_color);
    border: none;
    border-bottom: 2px dotted var(--muted_color);
    background: transparent;
    padding: 0;
    margin-left: 20px;
}

.btn {
    border: none;
    padding: 10px 7px;
    background-color: inherit;
    color: white;
}

.btn-close {
    background-color: red;
    padding-top: 10px;
    padding-bottom: 10px;
}

.btn-lg {
    padding-left: 30px;
    padding-right: 30px;
}
</style>