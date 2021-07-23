<template>
    <div class="well text-center" v-if="history.length > 0">
        <h3>Search History</h3>
        <ul class="list-group text-left">
            <li
                class="list-group-item"
                v-for="(item, i) in history"
                :key="i"
                :value="item"
            >
                <div>
                    <strong>{{ item.from }} => {{ item.to }}</strong>
                </div>
                <br />
                <div>
                    <strong class="text-success">{{
                        item.translateText
                    }}</strong>
                    <span class="text-info">{{ item.translatedText }}</span>
                </div>
            </li>
        </ul>
    </div>
</template>
<script>
export default {
    props: ["history"],
};
</script>
<style scoped>
.list-group-item {
    margin-bottom: 5px;
}
</style>