<template>
    <form @submit.prevent="translateIt" class="well">
        <textarea
            cols="30"
            rows="5"
            class="form-control"
            placeholder="Write here..."
            v-model="toTranslate"
        ></textarea>
        <select v-model="translateLanguage" class="form-control">
            <option v-for="(value, key) in languages" :value="key" :key="key">
                {{ value }}
            </option>
        </select>
        <br />
        <div class="text-left">
            <strong>Original Language : </strong>{{ detectedLang }}
        </div>
        <br />
        <button type="submit" class="btn btn-primary btn-block">
            Translate
        </button>
    </form>
</template>
<script>
import axios from "axios";
export default {
    data() {
        return {
            toTranslate: "",
            translateLanguage: "",
            languages: {},
            detectedLang: "",
        };
    },
    methods: {
        translateIt() {
            let url =
                "https://translate.yandex.net/api/v1.5/tr.json/translate?key=trnsl.1.1.20210722T203515Z.7e43f72c497dcf66.91cdf8fc08a40ef7d64747e887ed1e9a0f575cb5&text=" +
                this.toTranslate +
                "&lang=" +
                this.translateLanguage;
            axios
                .get(url)
                .then((response) => {
                    console.log(response);
                    let languagePeer = response.data.lang;
                    let lang = languagePeer.slice(0, 2);
                    this.detectedLang = this.languages[lang];
                    this.$emit("translatedEvent", response.data.text[0]);
                })
                .catch((error) => console.log(error));
        },
    },
    created() {
        axios
            .get(
                "https://translate.yandex.net/api/v1.5/tr.json/getLangs?key=trnsl.1.1.20210722T203515Z.7e43f72c497dcf66.91cdf8fc08a40ef7d64747e887ed1e9a0f575cb5&ui=en"
            )
            .then((response) => {
                console.log(response);
                this.languages = response.data.langs;
            })
            .catch((error) => console.log(error));
    },
};
</script>
<style scoped>
</style>