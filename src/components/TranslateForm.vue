<template>
    <form @submit.prevent="translateIt" class="well">
        <button
            @click="deleteTextArea()"
            class="delete-btn btn btn-danger btn-sm"
        >
            X
        </button>
        <textarea
            cols="30"
            rows="5"
            class="form-control"
            placeholder="Write here..."
            v-model="toTranslate"
            @keydown.enter="translateIt()"
        >
        </textarea>
        <select v-model="translateLanguage" class="form-control">
            <option value="" disabled selected>Select Language</option>
            <option v-for="(value, key) in languages" :value="key" :key="key">
                {{ value }}
            </option>
        </select>
        <br />
        <div class="text-left">
            <strong v-if="detectedLang">Original Language : </strong
            >{{ detectedLang }}
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
            translateLanguage: "", // the code of the translate language
            languages: {},
            detectedLang: "",
        };
    },
    props: ["choicedLang"],
    methods: {
        translateIt() {
            let languageToTranslate = "";
            this.translateLanguage.length > 0
                ? (languageToTranslate = this.translateLanguage)
                : (languageToTranslate = this.choicedLang);
            let url =
                "https://translate.yandex.net/api/v1.5/tr.json/translate?key=trnsl.1.1.20210722T203515Z.7e43f72c497dcf66.91cdf8fc08a40ef7d64747e887ed1e9a0f575cb5&text=" +
                this.toTranslate +
                "&lang=" +
                languageToTranslate;
            axios
                .get(url)
                .then((response) => {
                    console.log(response);
                    // find the original language
                    let languagePeer = response.data.lang;
                    let lang = languagePeer.split("-");
                    // save the original language as detectedLang
                    this.detectedLang = this.languages[lang[0]];
                    // find the translation language
                    let translateLang = this.languages[lang[1]];
                    // emit the translation to show it
                    this.$emit("translatedEvent", response.data.text[0]);
                    // show history
                    let history = {
                        from: this.detectedLang,
                        to: translateLang,
                        translateText: this.toTranslate,
                        translatedText: response.data.text[0],
                    };

                    this.$emit("historyEvent", history);
                    this.translateLanguage.length > 0
                        ? this.$emit(
                              "translateLangEvent",
                              this.languages[this.translateLanguage]
                          )
                        : "";
                })
                .catch((error) => console.log(error));
        },
        deleteTextArea() {
            this.toTranslate = "";
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
    watch: {
        translateLanguage: () => {
            console.log(
                "I can detect when option is selected but I cant take the translate Language"
            );
            // console.log(this.translateLanguage);
        },
    },
};
</script>
<style scoped>
.delete-btn {
    margin-top: 0;
    margin-bottom: 5px;
    position: relative;
    float: right;
    bottom: -40px;
}
</style>