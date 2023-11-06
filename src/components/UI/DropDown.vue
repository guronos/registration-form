<template>
    <!--
    По сути компонент обертка, возможно этот компонент лишний в текущей редакции,
    однако компонент может служить фабрикой подготовки данных, например если ключи в объектах списков разные по проекту.
    Или из выбранной сущности необходимо получить уникальные данные.
    По-хорошему я бы сделал на инпут и дропдаун разные компоненты при использовании на большом проекте,
    в текущем - неоправданно много лишнего кода, но при необходимости маштабирования перепись компонента будет проще.
    -->
    <div class="drop-down__wrapper">
        <v-input
            behavior="dropDown"
            :placeholder="placeholder"
            :dropList="dropList"
            :error="error"
            :errorText="errorText"
            @selectItem="$emit('selectItem', $event.value)"
        />
    </div>
</template>
<script>
import VInput from '@/components/UI/VInput.vue'

export default {
    name: 'DropDown',
    components: {
        VInput,
    },
    props: {
        placeholder: {
            default: 'Выберите значение',
        },
        dropList: {
            default: [],
        },
        error: {
            default: false,
        },
        errorText: {
            default: 'Проверьте корректность заполнения поля',
        },
    },
}
</script>
<style scoped></style>
