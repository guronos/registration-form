<template>
    <div class="checkbox-wrapper" @click="handlerCheckbox">
        <input type="checkbox" class="checkbox-button" :checked="active" />
        <label
            v-if="typeof text === 'string'"
            for="checkbox-button"
            class="text"
            >{{ typeof text }}</label
        >
        <label
            v-if="typeof text === 'object'"
            for="checkbox-button"
            class="text"
        >
            <span>
                <template v-for="(wordShort, idx) in text">
                    <span v-if="wordShort.type === 'text'" :key="idx">{{
                        wordShort.value
                    }}</span>
                    <a
                        v-else-if="wordShort.type === 'link'"
                        class="link"
                        :href="wordShort.link"
                        :key="idx"
                        >{{ wordShort.value }}</a
                    >
                </template>
            </span>
        </label>
    </div>
</template>
<script>
export default {
    name: 'VCheckbox',
    props: {
        activeStatus: {
            default: false,
        },
        text: {
            default: false,
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
        handlerCheckbox() {
            this.active = !this.active
            this.$emit('clickCheckbox', this.active)
        },
    },
}
</script>
<style scoped>
.checkbox-wrapper {
    cursor: pointer;
    display: flex;
}

.checkbox-button {
    position: absolute;
    z-index: -1;
    opacity: 0;
}

.checkbox-button + label {
    display: inline-flex;
    margin-top: 0;
    user-select: none;
}

.checkbox-button + label::before {
    content: '';
    display: inline-block;
    width: 21px;
    height: 17px;
    border: 1px solid #adb5bd;
    border-radius: 5px;
    margin-right: 0.5rem;
    background-repeat: no-repeat;
    background-position: center center;
    background-size: 50% 50%;
}

.checkbox-button:not(:disabled):not(:checked) + label:hover::before {
    border-color: #b3d7ff;
}

.checkbox-button:not(:disabled):active + label::before {
    background-color: #b3d7ff;
    border-color: #b3d7ff;
}

.checkbox-button:focus + label::before {
    box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.25);
}

.checkbox-button:focus:not(:checked) + label::before {
    border-color: #80bdff;
}

.checkbox-button:checked + label::before {
    border-color: #0b76ef;
    background-color: #0b76ef;
    background-image: url('data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iOSIgaGVpZ2h0PSI3IiB2aWV3Qm94PSIwIDAgOSA3IiBmaWxsPSJub25lIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciPgo8cGF0aCBkPSJNMC4zMjQ0NjMgNC40MDc2N0MwLjExNDMyNyA0LjIxNjgyIDAuMDA1NzA2NzYgMy45NjI5NSAwLjAwMDIxMzQ4IDMuNzA3MzRDLTAuMDA1MjA2NTUgMy40NTE4IDAuMDkyNTczOCAzLjE5MzkgMC4yOTQyODcgMi45OTUxNkMwLjQ5NTg1NCAyLjc5NjU2IDAuNzY0MjkyIDIuNjkzNzcgMS4wMzQ0OSAyLjY4ODY0QzEuMzA0ODMgMi42ODMzNyAxLjU3NzM3IDIuNzc1NTcgMS43ODc2NSAyLjk2NjU1TDMuNjAzMTQgNC42MTg3M0w3LjIxNzY1IDAuMjc2MjgxTDcuMzQ3MTQgMC4zNjk3MjhMNy4yMTc1IDAuMjc1ODY2QzcuMjI3ODMgMC4yNjMxODkgNy4yMzk1NSAwLjI1MjM4MyA3LjI1MjU5IDAuMjQzNTg1QzcuNDY3NjMgMC4wNjc0Mjk1IDcuNzM3NTMgLTAuMDEyMDkzNSA4LjAwMTM2IDAuMDAxNDgzNjNWMC4wMDEyMDY1NEw4LjAxNDI1IDAuMDAyMzE0ODhDOC4yNzk2MSAwLjAxODkzOTkgOC41Mzg4MiAwLjEzMDQ2NiA4LjczMDIgMC4zMzI0NkM4LjkyNDg5IDAuNTM3ODQ4IDkuMDEzMTQgMC43OTg3MjIgOC45OTg0MiAxLjA1MzcxSDguOTk4NzJMOC45OTc1NCAxLjA2NTlDOC45ODAyNiAxLjMxMjM3IDguODY2MzYgMS41NTMyMiA4LjY1OTk2IDEuNzMzMTJMNC4zMjIxOCA2LjcyNjg1TDQuMzIyNDcgNi43MjcxM0M0LjMxNDM0IDYuNzM2OSA0LjMwNTE5IDYuNzQ1MjggNC4yOTUyMiA2Ljc1MjYyQzQuMDkyMTIgNi45MjEzIDMuODM1NjIgNy4wMDQwMSAzLjU4MDU5IDYuOTk5ODVDMy4zMjM0MyA2Ljk5NTYzIDMuMDY2NzggNi45MDMxNSAyLjg2NzEyIDYuNzIxNjZMMC4zMjQ0NjMgNC40MDc2N1oiIGZpbGw9IndoaXRlIi8+Cjwvc3ZnPgo=');
}

.checkbox-button:disabled + label::before {
    background-color: #e9ecef;
}

.text {
    display: inline-block;
    font-size: 14px;
    font-weight: 400;
    line-height: 19px;
    letter-spacing: -0.0015em;
    text-align: left;
}

.link {
    text-decoration: none;
    color: #3586ff;
}
</style>
