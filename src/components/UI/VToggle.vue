<template>
    <div class="toggle-wrapper" @click="handlerToggle">
        <span class="toggle">
            <input type="checkbox" class="toggle-button" :checked="active" />
            <span class="circle" :class="{ active: active }"></span>
        </span>
        <div class="text">
            <label for="toggle-button" class="text__main">{{ text }}</label>
            <div v-if="description" class="description">{{ description }}</div>
        </div>
    </div>
</template>
<script>
export default {
    name: 'VToggle',
    props: {
        activeStatus: {
            default: false,
        },
        text: {
            required: true,
        },
        description: {
            default: false,
        },
    },
    data() {
        return {
            active: null,
        }
    },
    mounted() {
        this.active = this.activeStatus
    },
    methods: {
        handlerToggle() {
            this.active = !this.active
            this.$emit('clickToggle', this.active)
        },
    },
}
</script>
<style scoped>
.toggle-wrapper {
    cursor: pointer;
    display: flex;
}

.toggle {
    position: relative;
}

.toggle-button {
    display: inline-block;
    width: 40px;
    height: 20px;
    margin: 0;
    vertical-align: top;
    background: #cdced8;
    border: 1px solid #bbc1e1;
    border-radius: 30px;
    outline: none;
    cursor: pointer;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
}

.toggle-button:checked {
    background: #3586ff;
    transition: 0.3s;
}

.circle {
    display: inline-block;
    position: absolute;
    left: 0;
    top: 1px;
    width: 18px;
    height: 18px;
    background-color: #fdfdfd;
    border-radius: 50%;
    transform: translateX(0);
    transition: 0.3s;
}

.active {
    transform: translateX(21px);
    transition: 0.3s;
}

.text {
    margin-left: 6px;
    user-select: none;
}

.text__main {
    display: inline-block;
    font-size: 16px;
    font-weight: 500;
    line-height: 19px;
    text-align: left;
}

.description {
    font-size: 14px;
    font-weight: 400;
    line-height: 19px;
    letter-spacing: -0.0015em;
    text-align: left;
    color: #696977;
    margin-top: 6px;
    cursor: default;
}
</style>
