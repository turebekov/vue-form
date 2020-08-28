<template>
    <div class="form_block">

        <div class="container">
            <h2>Форма регистрации</h2>
            <form @submit.prevent="submitForm">
                <div class="form_container">
                    <div class="field">
                        <label for="firstName">Имя</label>
                        <input type="text" v-model="user.firstName" id="firstName" name="firstName"
                               class="field-control"
                               :class="{ 'is-invalid':  $v.user.firstName.$error }"
                               @input="$v.user.firstName.$touch()"/>
                        <div v-if="$v.user.firstName.$error" class="invalid-feedback">
                            <div v-if="!$v.user.firstName.required">Введите имя
                            </div>
                        </div>

                    </div>

                    <div class="field">
                        <label for="lastName">Фамилия</label>
                        <input type="text" v-model="user.lastName" id="lastName" name="lastName"
                               class="field-control" @input="$v.user.lastName.$touch()"
                               :class="{ 'is-invalid': $v.user.lastName.$error }"/>
                        <div v-if="$v.user.lastName.$error">
                            <div v-if="!$v.user.lastName.required" class="invalid-feedback">Введите фамилию
                            </div>
                        </div>
                    </div>
                </div>


                <div class="field">
                    <label for="secondName">Отчество</label>
                    <input type="text" v-model="user.secondName" id="secondName" name="secondName"
                           class="field-control"/>
                </div>

                <div class="form_container">

                    <div class="field">
                        <label for="secondName">Дата рождения</label>
                        <input type="date" v-model="user.dateOfBirth" id="dateOfBirth" name="dateOfBirth"
                               class="field-control" @input="$v.user.dateOfBirth.$touch()"
                               :class="{ 'is-invalid':  $v.user.dateOfBirth.$error }"/>
                        <div v-if="$v.user.dateOfBirth.$error" class="invalid-feedback">
                            <span v-if="!$v.user.dateOfBirth.required">Введите день рождения</span>
                        </div>
                    </div>

                    <div class="field">
                        <label for="secondName">Номер телефона</label>
                        <input type="text" @keypress="checkEventValueType($event)" @keydown="dontRemoveFirstVal($event)"
                               v-model="user.phoneNumber" id="phoneNumber" name="phoneNumber"
                               class="field-control" @input="$v.user.phoneNumber.$touch()"
                               :class="{ 'is-invalid': $v.user.phoneNumber.$error }"/>
                        <div v-if="$v.user.phoneNumber.$error" class="invalid-feedback">
                            <span v-if="!$v.user.phoneNumber.required">Введите номер телефона</span>
                            <span v-if="!$v.user.phoneNumber.minLength">Длина номера должна составить 11 цифров</span>
                            <span v-if="!$v.user.phoneNumber.maxLength">Длина номера должна составить 11 цифров</span>
                        </div>
                    </div>

                </div>


                <div class="form_container">

                    <div class="field">
                        <label>Группа клиентов</label>
                        <select class="field-control" v-model="user.customerGroup"
                                @change="$v.user.customerGroup.$touch()" multiple>
                            <option>VIP</option>
                            <option>Проблемные</option>
                            <option>ОМС</option>
                        </select>
                        <div v-if="$v.user.customerGroup.$error" class="invalid-feedback">
                            <span v-if="!$v.user.customerGroup.required">Введите группу</span>
                        </div>
                    </div>


                    <div class="field">
                        <label for="attendingDoctor">Лечащий врач</label>
                        <select class="field-control" id="attendingDoctor" v-model="user.attendingDoctor">
                            <option disabled value="">Выберите один из вариантов</option>
                            <option>Иванов</option>
                            <option>Захаров</option>
                            <option>Чернышева</option>
                        </select>

                    </div>
                </div>

                <div class="form_container">
                    <div class="field">
                        <label for="secondName">Пол</label>
                        <input type="text" v-model="user.gender" id="gender" name="gender"
                               class="field-control"/>
                    </div>
                    <div class="field">
                        <label for="checkbox">Не отправлять СМС</label>
                        <input type="checkbox" id="checkbox" v-model="user.sendSms">
                    </div>

                </div>

                <h3>Адрес</h3>

                <div class="form_container">
                    <div class="field">
                        <label for="index">Индекс</label>
                        <input type="text" v-model="address.index" id="index" name="index"
                               class="field-control"/>
                    </div>

                    <div class="field">
                        <label for="country">Страна </label>
                        <input type="text" v-model="address.country" id="country" name="country"
                               class="field-control"/>
                    </div>
                </div>

                <div class="form_container">
                    <div class="field">
                        <label for="region">Область </label>
                        <input type="text" v-model="address.region" id="region" name="region"
                               class="field-control"/>
                    </div>

                    <div class="field">
                        <label for="city">Город</label>
                        <input type="text" v-model="address.city" id="city" name="city"
                               class="field-control" @input="$v.address.city.$touch()"
                               :class="{ 'is-invalid': $v.address.city.$error }"/>
                        <div v-if="$v.address.city.$error" class="invalid-feedback">
                            <span v-if="!$v.address.city.required">Введите город</span>
                        </div>
                    </div>
                </div>

                <div class="form_container">
                    <div class="field">
                        <label for="street">Улица </label>
                        <input type="text" v-model="address.street" id="street" name="street"
                               class="field-control"/>
                    </div>

                    <div class="field">
                        <label for="home">Дом </label>
                        <input type="text" v-model="address.home" id="home" name="home"
                               class="field-control"/>
                    </div>
                </div>

                <h3>Паспорт</h3>


                <div class="field">
                    <label for="documentType">Тип документа</label>
                    <select id="documentType" class="field-control" v-model="passport.type"
                            :class="{ 'is-invalid': $v.passport.type.$error }"
                            @change="$v.passport.type.$touch()">
                        <option disabled value="">Выберите один из вариантов</option>
                        <option>Паспорт</option>
                        <option>Свидетельство о рождении</option>
                        <option>Вод. удостоверение</option>
                    </select>
                    <div v-if="$v.passport.type.$error" class="invalid-feedback">
                        <span v-if="!$v.passport.type.required">Введите тип документа</span>
                    </div>
                </div>

                <div class="form_container">
                    <div class="field">
                        <label for="series">Серия </label>
                        <input type="text" v-model="passport.series" id="series" name="series"
                               class="field-control"/>
                    </div>

                    <div class="field">
                        <label for="number">Номер </label>
                        <input type="text" v-model="passport.number" id="number" name="number"
                               class="field-control"/>
                    </div>
                </div>

                <div class="form_container">
                    <div class="field">
                        <label for="issuedBy">Кем выдан </label>
                        <input type="text" v-model="passport.issuedBy" id="issuedBy" name="issuedBy"
                               class="field-control"/>
                    </div>

                    <div class="field">
                        <label for="dateOfIssue">Дата выдачи</label>
                        <input type="date" v-model="passport.dateOfIssue" id="dateOfIssue" name="dateOfIssue"
                               class="field-control" @input="$v.passport.dateOfIssue.$touch()"
                               :class="{ 'is-invalid': $v.passport.dateOfIssue.$error }"/>
                        <div v-if=" $v.passport.dateOfIssue.$error" class="invalid-feedback">
                            <span v-if="!$v.passport.dateOfIssue.required">Введите дату выдачу документа</span>
                        </div>
                    </div>
                </div>


                <div class="field">
                    <button>Регистрация</button>
                </div>
            </form>
        </div>
    </div>

</template>

<script>
    import {required, minLength, maxLength} from "vuelidate/lib/validators";

    export default {
        name: "FormComponent",
        data() {
            return {
                user: {
                    firstName: "",
                    lastName: "",
                    secondName: "",
                    dateOfBirth: "",
                    phoneNumber: "7",
                    gender: "",
                    customerGroup: [],
                    attendingDoctor: "",
                    sendSms: false
                },
                address: {
                    index: "",
                    country: "",
                    region: "",
                    city: "",
                    street: "",
                    home: ""
                },
                passport: {
                    type: "",
                    series: "",
                    number: "",
                    issuedBy: "",
                    dateOfIssue: ""
                },
                submitted: false
            };
        },
        validations: {
            user: {
                firstName: {required},
                lastName: {required},
                dateOfBirth: {required, minLength: minLength(6)},
                phoneNumber: {required, minLength: minLength(11), maxLength: maxLength(11)},
                customerGroup: {required},
            },
            address: {
                city: {required}
            },
            passport: {
                type: {required},
                dateOfIssue: {required}
            }
        },
        methods: {
            submitForm() {
                if (this.$v.$invalid) {
                    this.$v.$touch();
                } else {
                    alert("Новый пользователь успешно создан");

                }
            },

            checkEventValueType(evt) {
                if (evt.which !== 8 && isNaN(Number(String.fromCharCode(evt.which)))) {
                    evt.preventDefault();
                }
            },
            dontRemoveFirstVal(evt) {
                if (evt.keyCode == 8 && this.user.phoneNumber.length === 1) {
                    evt.preventDefault();
                }
            }
        }
    }
</script>
<style scoped>

    .form_block {
        padding: 4rem 2rem;
        margin-bottom: 2rem;
        background: linear-gradient(90deg, #f4f4f4 -94.12%, rgba(255, 255, 255, 0) 100%), #f0f3f9;
        border-radius: .3rem;

    }

    .container {
        width: 40%;
        margin-left: 30%;

    }

    .form_container {
        width: 100%;
        flex-direction: row;
        box-sizing: border-box;
        display: flex;
        place-content: center space-between;
        align-items: center;
    }

    .form_container > div {
        flex: 1 1 100%;
        box-sizing: border-box;
        max-width: 100%;
    }

    .field {
        display: flex;
        justify-content: flex-end;
        flex-direction: column;
        align-content: center;
        padding: .5em;
    }

    .field > label {
        display: inline-block;
        margin-bottom: .5rem;
    }

    /*.field > input {
        width: 50%;
    }
    .field > select {
        width: 51%;
    }*/
    .field > input,
    .field > button {
        padding: .5em;
    }

    .field > button {
        background: #00d1b2;
        color: white;
        border: 0;
    }

    .field-control {
        display: block;
        padding: .375rem .75rem;
        font-size: 1rem;
        line-height: 1.5;
        color: #495057;
        background-color: #fff;
        border: 1px solid #00d1b2;
        background-clip: padding-box;
        border-radius: .25rem;
        transition: border-color .15s ease-in-out, box-shadow .15s ease-in-out;
    }

    .field-control:focus {
        color: #495057;
        background-color: #fff;
        box-shadow: 0 0 0 0.125em rgba(0, 209, 178, .25);
        outline: 0;

    }

    .is-invalid {
        border-color: #dc3545;
    }

    .is-invalid:focus {
        border-color: #dc3545;
        box-shadow: 0 0 0 0.2rem rgba(220, 53, 69, .25)
    }

    .invalid-feedback {
        margin-top: .25rem;
        font-size: 80%;
        color: #dc3545;
    }

    @media screen and (min-width: 250px) {
        .container {
            width: 98%;
            margin-left: 1%;
        }
    }

    @media screen and (max-width: 600px) {
        .form_container {
            flex-flow: row wrap;
        }
    }

    @media screen and (min-width: 600px) {
        .container {
            width: 70%;
            margin-left: 15%;
        }
    }

    @media screen and (min-width: 1300px) {
        .container {
            width: 50%;
            margin-left: 25%;
        }
    }

</style>
