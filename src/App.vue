<template>
    <div class="container-fluid">
        <h1 class="text-center">Simple Translator</h1>
        <h5 class="text-center">with Vue.js & Yandex Api</h5>
        <br />
        <div class="row">
            <div class="col-md-12">
                <div class="col-md-6">
                    <ul class="nav nav-pills">
                        <li @click="chooseLang($event)">
                            <a id="tr" class="language" :class="isActive">
                                Türkish
                            </a>
                        </li>
                        <li @click="chooseLang($event)">
                            <a id="en" class="language" href="#"> English </a>
                        </li>
                        <li @click="chooseLang($event)">
                            <a id="de" class="language" href="#"> German </a>
                        </li>
                        <li @click="chooseLang($event)">
                            <a
                                id="choosenFromSelect"
                                class="language"
                                href="#"
                                >{{ translatedLang }}</a
                            >
                        </li>
                    </ul>
                    <TranslateForm
                        @translatedEvent="translatedText = $event"
                        @historyEvent="history.push($event)"
                        @translateLangEvent="translatedLang = $event"
                        :choicedLang="choicedLang"
                    />
                </div>
                <div class="col-md-6 well">
                    <h3 class="text-success text-center">
                        <span v-if="!translatedText">Translation</span>
                        {{ translatedText }}
                    </h3>
                </div>
            </div>
        </div>
        <div class="row search">
            <SearchHistory :history="history" />
        </div>
    </div>
</template>
<script>
import TranslateForm from "./components/TranslateForm";
import SearchHistory from "./components/SearchHistory";

function styleTabs(id) {
    // remove active
    let items = document.getElementsByClassName("language");
    for (let i = 0; i < items.length; i++) {
        const element = items[i];
        element.classList.remove("actived");
        if (element.id == id) {
            element.classList.add("actived");
        }
    }
}

export default {
    data() {
        return {
            translatedText: "",
            history: [],
            choicedLang: "tr",
            isActive: "actived",
            translatedLang: "",
        };
    },
    components: {
        TranslateForm,
        SearchHistory,
    },
    methods: {
        chooseLang(event) {
            this.choicedLang = event.target.id;
            // styling for active
            styleTabs(event.target.id);
        },
    },
    watch: {
        translatedLang: () => {
            styleTabs("choosenFromSelect");
        },
    },
};
</script>
<style>
body {
    background-color: #dedede;
}

.actived {
    background-color: #2c3e50 !important;
    color: white !important;
}
.nav {
    background-color: #ecf0f1;
    display: flex;
    justify-content: flex-end;
    padding: 15px 0 0 0;
}
.row {
    margin: 0;
}
.search {
    margin-left: 30px;
    margin-right: 25px;
}
</style>