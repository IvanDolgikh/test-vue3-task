<template>
    <div class="custom-select">
        <v-select v-model="selectedOption" clearable label="Сортировка" :items="options" variant="outlined"></v-select>
    </div>
</template>

<script setup>
import { inject, ref, watch } from 'vue';

const firstList = inject('firstList');
const secondList = inject('secondList');
const lastList = inject('lastList');

const options = ref(['по возрастанию рейтинга', 'по убыванию рейтинга', 'без сортировки'])

const selectedOption = ref(null)

const props = defineProps({
    options: {},
});

let cards = ref([]);
let copiedCards = ref([])

function getLocalCards() {
    switch (props.options.id) {
        case 1:
            cards = firstList;
            copiedCards.value = firstList.value.slice();
            break;
        case 2:
            cards = secondList;
            copiedCards.value = secondList.value.slice();
            break;
        case 3:
            cards = lastList;
            copiedCards.value = lastList.value.slice();
            break;
    }
}


watch(selectedOption, newValue => {
    getLocalCards()
    if (newValue === 'по возрастанию рейтинга') {
        return cards.value.sort((a, b) => a.rating.rate - b.rating.rate)
    } else if (newValue === 'по убыванию рейтинга') {
        return cards.value.sort((a, b) => b.rating.rate - a.rating.rate)
    } else if (newValue === 'без сортировки') {
        return copiedCards.value
    }
})

</script>

<style lang="scss" scoped>
.custom-select {
    .v-field__input {
        max-height: 46px;
    }
}
</style>