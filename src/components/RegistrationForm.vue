<template>
    <div class="wrapper">
        <div class="modal">
            <div class="modal__title">Регистрация</div>
            <hr />
            <div v-if="!responseCreateUser">
                <div class="modal__main">
                    <span class="main__desctiption">
                        Заполните Ваши данные
                    </span>
                    <div class="modal__writable">
                        <template v-for="(component, key) in componentData">
                            <component
                                v-if="key !== 'userRole'"
                                class="component_margin-y"
                                :key="component.id"
                                :is="component.nameComponent"
                                v-bind="component.props"
                                @setInput="component.setInput($event)"
                            />
                            <div
                                v-else-if="key === 'userRole'"
                                class="writable_alone"
                                :key="component.id"
                            >
                                <component
                                    class="component_margin-y"
                                    :key="component.id"
                                    :is="component.nameComponent"
                                    v-bind="component.props"
                                    @selectItem="component.changeData($event)"
                                />
                            </div>
                        </template>
                    </div>
                </div>
                <hr />
                <div class="modal__actions">
                    <div>
                        <VToggle
                            :text="'Хотите чтобы Ваш профиль видели другие участники платформы?'"
                            :activeStatus="showProfile"
                            :description="'Включает профиль для просмотра другими пользователями по ссылке'"
                            @clickToggle="showProfile = $event"
                        />
                    </div>
                    <div class="modal__actions_last-child">
                        <VCheckbox
                            :text="checkboxData"
                            :active-status="publicStatus"
                            @clickCheckbox="publicStatus = $event"
                        />
                        <button class="modal__btn" @click="validateAll">
                            Зарегистрироваться
                        </button>
                    </div>
                </div>
            </div>
            <div v-else class="result-created">
                <div v-if="!successCreated">
                    Регистрация якобы прошла успешно
                </div>
                <div v-if="successCreated">Регистрация прошла успешно</div>
            </div>
        </div>
    </div>
</template>

<script>
import VInput from '@/components/UI/VInput.vue'
import DropDown from '@/components/UI/DropDown.vue'
import VToggle from '@/components/UI/VToggle.vue'
import VCheckbox from '@/components/UI/VCheckbox.vue'

export default {
    name: 'RegistrationForm',
    components: {
        VCheckbox,
        VToggle,
        VInput,
        DropDown,
    },
    data() {
        return {
            API_URL_MOCK: 'http:localhost:3000/',
            componentData: {
                userName: {
                    id: 1,
                    nameComponent: 'VInput',
                    props: {
                        type: 'text',
                        placeholder: 'Имя',
                        model: '',
                        error: false,
                        errorText: '',
                    },
                    setInput(data) {
                        this.props.error = false
                        this.props.model = data
                    },
                },
                userEmail: {
                    id: 2,
                    nameComponent: 'VInput',
                    props: {
                        type: 'email',
                        placeholder: 'Email',
                        model: '',
                        error: false,
                        errorText: '',
                    },
                    setInput(data) {
                        this.props.error = false
                        this.props.model = data
                    },
                },
                userRole: {
                    id: 3,
                    nameComponent: 'DropDown',
                    props: {
                        placeholder: 'Должность',
                        dropList: [
                            {
                                value: 1,
                                name: 'Администратор',
                            },
                            {
                                value: 2,
                                name: 'Участник',
                            },
                            {
                                value: 3,
                                name: 'Пользователь',
                            },
                            {
                                value: 4,
                                name: 'Ученик',
                            },
                        ],
                        model: '',
                        error: false,
                        errorText: '',
                    },
                    changeData(value) {
                        this.props.error = false
                        this.props.model = value
                    },
                },
                userPassword: {
                    id: 4,
                    nameComponent: 'VInput',
                    props: {
                        type: 'password',
                        placeholder: 'Пароль',
                        model: '',
                        error: false,
                        errorText: '',
                    },
                    setInput(data) {
                        this.props.error = false
                        this.props.model = data
                        const secondaryPassword = this.getSecondaryPassword()
                        if (
                            data &&
                            secondaryPassword &&
                            data !== secondaryPassword
                        ) {
                            this.props.error = true
                            this.props.errorText = 'Пароли не совпадают'
                        } else this.setStatusValidate()
                    },
                    getSecondaryPassword: () => {
                        return this.componentData.userPasswordRepeat.props.model
                    },
                    setStatusValidate: () => {
                        this.validatePassword = true
                    },
                },
                userPasswordRepeat: {
                    id: 5,
                    nameComponent: 'VInput',
                    props: {
                        type: 'password',
                        placeholder: 'Повторите пароль',
                        model: '',
                        error: false,
                        errorText: '',
                    },
                    setInput(data) {
                        this.props.error = false
                        this.props.model = data
                        const firstPassword = this.getFirstPassword()
                        if (data && firstPassword && data !== firstPassword) {
                            this.props.error = true
                            this.props.errorText = 'Пароли не совпадают'
                        } else this.setStatusValidate()
                    },
                    getFirstPassword: () => {
                        return this.componentData.userPassword.props.model
                    },
                    setStatusValidate: () => {
                        this.validatePassword = true
                    },
                },
            },
            showProfile: true,
            publicStatus: true,
            checkboxData: [
                {
                    type: 'text',
                    value: 'Регистрируясь, Вы соглашаетесь  с ',
                },
                {
                    type: 'link',
                    value: 'политикой конфиденциальности ',
                    link: 'https://google.com',
                },
                {
                    type: 'text',
                    value: 'и обработкой ',
                },
                {
                    type: 'link',
                    value: 'персональных данных',
                    link: 'https://google.com',
                },
            ],
            responseCreateUser: null,
            validatePassword: false,
            successCreated: false,
        }
    },
    methods: {
        async createdUser() {
            try {
                const res = await fetch(`${this.API_URL_MOCK}created`, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json; charset=utf-8',
                    },
                    body: JSON.stringify({
                        public: this.showProfile,
                        username: this.componentData.userName.props.model,
                        email: this.componentData.userEmail.props.model,
                        role: this.componentData.userRole.props.model,
                        password: this.componentData.userPassword.props.model,
                        password_repeat:
                            this.componentData.userPasswordRepeat.props.model,
                    }),
                })
                this.responseCreateUser = await res.json()
                this.successCreated = true
            } catch (e) {
                console.error(e)
                this.responseCreateUser = []
                this.successCreated = false
            }
        },
        validateAll() {
            if (!this.publicStatus) return
            let status = this.validatePassword
            for (const key in this.componentData) {
                if (!this.componentData[key].props.model) {
                    this.componentData[key].props.error = true
                    this.componentData[key].props.errorText =
                        'Необходимо заполнить поле!'
                    status = false
                }
            }
            if (status) this.createdUser()
        },
    },
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
    background-color: #ffffff;
    width: 960px;
    min-height: 40vh;
    border-radius: 12px;
    padding-bottom: 40px;
}

.modal__title {
    padding: 20px;
    font-size: 19px;
    font-weight: 700;
    line-height: 27px;
    letter-spacing: -0.0035em;
    text-align: left;
    color: #000000;
}

.hr {
    color: #d9d9d9;
}

.modal__main {
    padding: 20px;
}

.main__desctiption {
    font-size: 16px;
    font-weight: 500;
    line-height: 19px;
}

.modal__writable {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    margin-top: 0.5rem;
}

.input_wrapper,
.drop-down__wrapper {
    width: 49%;
}

.writable_alone {
    width: 100%;
    display: flex;
    justify-content: flex-end;
}

.component_margin-y {
    margin: 0.5rem 0;
}

.modal__actions {
    padding: 20px;
}

.modal__actions_last-child {
    display: flex;
    justify-content: space-between;
    margin-top: 20px;
}

.modal__btn {
    width: 300px;
    height: 40px;
    background-color: rgba(73, 122, 218, 0.2);
    color: #497ada;
    font-size: 12px;
    font-weight: 400;
    line-height: 19px;
    border: none;
    border-radius: 8px;
    cursor: pointer;
}

.modal__btn:disabled {
    opacity: 0.3;
}

.result-created {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 10rem;
}

.checkbox-wrapper {
    width: 60%;
}
</style>
