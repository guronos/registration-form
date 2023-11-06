<template>
    <div class="wrapper">
        <div class="modal">
            <div class="modal__title">Регистрация</div>
            <hr />
            Заполните Ваши данные
            <div class="modal__main">
                <div v-for="component in componentData" :key="component.id">
                    <component
                        :is="component.nameComponent"
                        v-bind="component.props"
                    />
                </div>
            </div>
            <hr />

        </div>
    </div>
</template>

<script>
import VInput from "@/components/UI/VInput.vue";

export default {
    name: "RegistrationForm",
    components: {
        VInput
    },
    data() {
        return {
            componentData: {
                userName: {
                    id: 1,
                    nameComponent: 'VInput',
                    props: {
                        type: 'text',
                        placeholder: 'Имя'
                    }
                },
                userEmail: {
                    id: 2,
                    nameComponent: 'VInput',
                    props: {
                        type: 'text',
                        placeholder: 'Email'
                    }
                },
                userRole: {
                    id: 3,
                    nameComponent: 'DropDown',
                    props: {
                        placeholder: 'Должность'
                    }
                },
                userPassword: {
                    id: 4,
                    nameComponent: 'VInput',
                    props: {
                        type: 'text',
                        placeholder: 'Пароль'
                    }
                },
                userPasswordRepeat: {
                    id: 5,
                    nameComponent: 'VInput',
                    props: {
                        type: 'text',
                        placeholder: 'Повторите пароль'
                    }
                },
            }
        }
    }
}
</script>
<style scoped>
.wrapper {
    width: 100vw;
    height: 100vh;
    background-color: #2c3e5017;
    display: flex;
    justify-content: center;
    align-items: center;
}
.modal {
    background-color: #e8d4d4;
    width: 40vw;
    height: 40vh;
    border-radius: 12px;
}
.modal__title {
    padding: 20px;
}
</style>