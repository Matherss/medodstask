<template>
  <div class="body-container">
    <div class="main-cont">
      <form @submit.prevent="onSubmit">
        <div class="flex-container">
          <div>
            <p>Фамилия*:</p>
            <input type="text" v-model="v$.form.lastname.$model" />
            <div class="input-errors" v-for="(error, index) of v$.form.lastname.$errors" :key="index">
              <div class="error-msg">{{ error.$message }}</div>
            </div>
          </div>
          <div>
            <p>Имя*:</p>
            <input type="text" v-model="v$.form.firstname.$model" />
            <div class="input-errors" v-for="(error, index) of v$.form.firstname.$errors" :key="index">
              <div class="error-msg">{{ error.$message }}</div>
            </div>
          </div>
          <div>
            <p>Отчество:</p>
            <input type="text" v-model="v$.form.patronymic.$model" />
            <div class="input-errors" v-for="(error, index) of v$.form.patronymic.$errors" :key="index">
              <div class="error-msg">{{ error.$message }}</div>
            </div>
          </div>

          <div>
            <p>Дата рождения*:</p>
            <input type="text" v-model="v$.form.birthday.$model" />
            <div class="input-errors" v-for="(error, index) of v$.form.birthday.$errors" :key="index">
              <div class="error-msg">{{ error.$message }}</div>
            </div>
          </div>
          <div>
            <p>Номер телефона*:</p>
            <input type="text" v-model="v$.form.tel.$model" />
            <div class="input-errors" v-for="(error, index) of v$.form.tel.$errors" :key="index">
              <div class="error-msg">{{ error.$message }}</div>
            </div>
          </div>
          <div>
            <p>Пол:</p>
            <input type="text" />
          </div>

          <div>
            <p>Группа клиентов*:</p>
            <select multiple v-model="v$.form.clientGroup.$model">
              <option>VIP</option>
              <option>Проблемные</option>
              <option>ОМС</option>
            </select>
            <div class="input-errors" v-for="(error, index) of v$.form.clientGroup.$errors" :key="index">
              <div class="error-msg">{{ error.$message }}</div>
            </div>
          </div>
          <div>
            <p>Лечащий врач:</p>
            <select>
              <option>Иванов</option>
              <option>Захаров</option>
              <option>Чернышева</option>
            </select>
          </div>
          <div class="smscheckbox">
            <input type="checkbox" id="nosms" /><label for="nosms">Не отправлять СМС</label>
          </div>

          <div>
            <p>Индекс:</p>
            <input type="text" v-model="v$.form.index.$model" />
            <div class="input-errors" v-for="(error, index) of v$.form.index.$errors" :key="index">
              <div class="error-msg">{{ error.$message }}</div>
            </div>
          </div>
          <div>
            <p>Страна:</p>
            <input type="text" />
          </div>
          <div>
            <p>Область:</p>
            <input type="text" />
          </div>

          <div>
            <p>Город*:</p>
            <input type="text" v-model="v$.form.city.$model" />
            <div class="input-errors" v-for="(error, index) of v$.form.city.$errors" :key="index">
              <div class="error-msg">{{ error.$message }}</div>
            </div>
          </div>
          <div>
            <p>Улица:</p>
            <input type="text" />
          </div>
          <div>
            <p>Дом:</p>
            <input type="text" />
          </div>

          <div>
            <p>Тип документа*:</p>
            <select class="documentSelector" v-model="v$.form.typeOfDocument.$model">
              <option>Паспорт</option>
              <option>Свидетельство о рождении</option>
              <option>Вод. удостоверение</option>
            </select>
            <div class="input-errors" v-for="(error, index) of v$.form.typeOfDocument.$errors" :key="index">
              <div class="error-msg">{{ error.$message }}</div>
            </div>
          </div>
          <div>
            <p>Серия:</p>
            <input type="text" v-model="v$.form.seria.$model" />
            <div class="input-errors" v-for="(error, index) of v$.form.seria.$errors" :key="index">
              <div class="error-msg">{{ error.$message }}</div>
            </div>
          </div>
          <div>
            <p>Номер:</p>
            <input type="text" v-model="v$.form.number.$model" />
            <div class="input-errors" v-for="(error, index) of v$.form.number.$errors" :key="index">
              <div class="error-msg">{{ error.$message }}</div>
            </div>
          </div>

          <div>
            <p>Кем выдан:</p>
            <input type="text" />
          </div>
          <div>
            <p>Дата выдачи*:</p>
            <input type="date" v-model="v$.form.dateOfDoc.$model" />
            <div class="input-errors" v-for="(error, index) of v$.form.dateOfDoc.$errors" :key="index">
              <div class="error-msg">{{ error.$message }}</div>
            </div>
          </div>
          <div>
            <p>* - Обязательные поля</p>
            <button class="submit" @click="formChecker" name="register">Завершить регистрацию</button>
          </div>
        </div>
      </form>
      <div v-if="successForm == true" class="popup">&#10003; Регистрация прошла успешно</div>
    </div>
  </div>
</template>

<script>
import useVuelidate from "@vuelidate/core";
import { required, helpers } from "@vuelidate/validators";
export function validName(name) {
  let validNamePattern = new RegExp("[А-ЯA-Z|а-яa-z][А-ЯA-Z|а-яa-z|-]");
  if (validNamePattern.test(name) || name == "") {
    return true;
  }
  return false;
}
export function validBirthday(birthday) {
  let validBD = /^([0-2][0-9]|(3)[0-1])(\/)(((0)[0-9])|((1)[0-2]))(\/)\d{4}$/i;
  if (validBD.test(birthday)) {
    return true;
  }
  return false;
}
export function validTel(tel) {
  let validTel = /(^7)((\d{10})|(\s\(\d{3}\)\s\d{3}\s\d{2}\s\d{2}))/gm;
  if (validTel.test(tel)) {
    return true;
  }
  return false;
}
export function indexValid(idx) {
  let validIdx = /^\d+$/gm;
  if (validIdx.test(idx) || idx == "") {
    return true;
  }
  return false;
}

export default {
  name: "App",
  setup() {
    return { v$: useVuelidate() };
  },
  data() {
    return {
      successForm: false,
      form: {
        lastname: "",
        firstname: "",
        patronymic: "",
        birthday: "",
        tel: "",
        sex: "",
        clientGroup: [],
        doctor: "",
        smsCheck: false,
        index: "",
        country: "",
        region: "",
        city: "",
        street: "",
        house: "",
        typeOfDocument: "",
        seria: "",
        number: "",
        issue: "",
        dateOfDoc: ""
      }
    };
  },
  validations() {
    return {
      form: {
        lastname: {
          required: helpers.withMessage("Обязательное поле", required),
          name_validation: {
            $validator: validName,
            $message: "Должно содержать только буквы и дефис (-)"
          }
        },
        firstname: {
          required: helpers.withMessage("Обязательное поле", required),
          name_validation: {
            $validator: validName,
            $message: "Должно содержать только буквы и дефис (-)"
          }
        },
        patronymic: {
          name_validation: {
            $validator: validName,
            $message: "Должно содержать только буквы и дефис (-)"
          }
        },
        birthday: {
          required: helpers.withMessage("Обязательное поле", required),
          birthday_validation: {
            $validator: validBirthday,
            $message: "Дата в формате: ДД/ММ/ГГГГ. Пример: 01/12/1991 "
          }
        },
        tel: {
          required: helpers.withMessage("Обязательное поле", required),
          tel_validation: {
            $validator: validTel,
            $message: "Телефон в формате 79998886655"
          }
        },
        clientGroup: {
          required: helpers.withMessage("Обязательное поле", required)
        },
        city: {
          required: helpers.withMessage("Обязательное поле", required)
        },
        typeOfDocument: {
          required: helpers.withMessage("Обязательное поле", required)
        },
        dateOfDoc: {
          required: helpers.withMessage("Обязательное поле", required)
        },
        index: {
          index_validation: {
            $validator: indexValid,
            $message: "Индекс должен состоять только из цифр"
          }
        },
        seria: {
          seria_validation: {
            $validator: indexValid,
            $message: "Серия должна состоять только из цифр"
          }
        },
        number: {
          number_validation: {
            $validator: indexValid,
            $message: "Номер должен состоять только из цифр"
          }
        }
      }
    };
  },
  // computed: {
  //   lastnameErrors() {
  //     const errors = [];
  //     if (!this.$v.lastname.required) errors.push("Обязательно для заполнения");
  //     return errors;
  //   }
  // },

  components: {},
  methods: {
    formChecker() {
      setTimeout(this.v$.form.$validate, 200);
      if (this.v$.form.$silentErrors.length > 0) {
        this.successForm = false;
      } else {
        this.successForm = true;
        setTimeout(() => (this.successForm = false), 1500);
      }
    }
  }
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Ubuntu:wght@300;400;500&display=swap");
* {
  margin: 0;
  font-family: "Ubuntu", sans-serif;
  font-weight: 300;
}
.error-msg {
  color: maroon;
}
body {
  background: url("https://w.wallhaven.cc/full/k9/wallhaven-k9r6g7.jpg") center no-repeat fixed;
  background-size: cover;
  color: #333;
  font-size: 18px;
  font-family: "Raleway", sans-serif;
  min-height: 100vh;
  padding: 0;
  margin: 0 auto;
}
button.submit {
  background: rgba(255, 255, 255, 0.25);
  border: 1px solid #333;
  line-height: 1em;
  padding: 0.5em 0.5em;
  -webkit-transition: all 0.25s;
  transition: all 0.25s;
}
button:hover,
button:focus,
button:active,
button.loading {
  background: #333;
  color: #fff;
  outline: none;
}
button.success {
  background: #27ae60;
  border-color: #27ae60;
  color: #fff;
}
.popup {
  width: 300px;
  padding: 20px;
  background-color: rgb(0, 255, 85);
  color: black;
  position: fixed;
  top: calc(50% - 50%);
  left: calc(50% - 50%);
}
.main-cont {
  background: rgba(255, 255, 255, 0.6);
  margin-top: 1em;
  border-radius: 0.5em;
  box-shadow: 0 0 1em 0 rgba(51, 51, 51, 0.25);
  max-width: 880px;
  overflow: hidden;
  padding: 2em;
  z-index: 1;
  width: 98%;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 10px;
}
select {
  overflow: hidden;
  min-height: 25px;
  padding: 0.5em 0.5em;
  border-radius: 5px;
  outline: none;
  border: 1px solid rgba(0, 0, 0, 0.1);
  background: rgba(0, 0, 0, 0.3);
  &:hover {
    background: rgba(0, 0, 0, 0.3);
  }
  option:checked {
    color: white;
    background: #00a6ff;
  }
  option:hover {
    background: rgba(0, 0, 0, 0.3);
  }
}
.flex-container {
  max-width: 1140px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(auto-fit, 280px);
  grid-gap: 20px;
  justify-content: center;

  div {
    margin: 2px;
    width: 280px;
    text-align: left;
    display: flex;
    flex-direction: column;

    padding: 3px;
    p {
      margin-bottom: 2px;
    }
    input {
      height: 25px;
      outline: none;
      border: none;
      color: #00a6ff;
      padding: 0.5em 0.5em;
      border-radius: 5px;
      background: rgba(0, 0, 0, 0.3);
      transition: all 1s linear;
      &:focus {
        background: rgba(0, 0, 0, 0.6);
      }
      &[placeholder] {
        color: #c5e8fa;
      }
    }
  }
  .smscheckbox {
    display: flex;
    flex-direction: row;
    input {
      margin-right: 7px;
    }
  }
}
.body-container {
  width: 100vw;
  display: flex;
  justify-content: center;
}
@media screen and (max-width: 942px) {
  .flex-container {
    grid-gap: 15px;
  }
}
</style>
