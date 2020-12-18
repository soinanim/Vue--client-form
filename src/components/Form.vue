<template>
<div class="container">
    <form class="form-fields" @submit.prevent="checkForm">
      <div v-show="step === 1" class="step">
        <!--ФИО-->
        <div class="form-group">
          <label for="surname">Фамилия</label>
          <input 
            id="surname" 
            placeholder="Ваша фамилия"
            class="form-control"
            :class="$v.form.surname.$error ? 'is-invalid' : '' "
            v-model.trim="form.surname"
          >
          <p v-if="$v.form.surname.$dirty && !$v.form.surname.required" class="invalid-message">
            Обязательное поле
          </p>
          <p v-if="$v.form.surname.$dirty && !$v.form.surname.minLength" class="invalid-message">
            Здесь должно быть больше 5 символов
          </p>
          
          <label for="name">Имя</label>
          <input 
            id="name"
            placeholder="Ваше Имя" 
            class="form-control"
            :class="$v.form.name.$error ? 'is-invalid' : '' "
            v-model.trim="form.name" 
          >
          <p v-if="$v.form.name.$dirty && !$v.form.name.required" class="invalid-message">
            Обязательное поле
          </p>
          <p v-if="$v.form.name.$dirty && !$v.form.name.minLength" class="invalid-message">
            Здесь должно быть больше 5 символов
          </p>

          <label for="patronymic">Отчество</label>
          <input
            id="patronymic"
            placeholder="Ваше отчество"
            class="form-control"
            :class="$v.form.patronymic.$error ? 'is-invalid' : '' "
            v-model.trim="form.patronymic" 
          >
          <p v-if="$v.form.patronymic.$dirty && !$v.form.patronymic.required" class="invalid-message">
            Обязательное поле
          </p>
          <p v-if="$v.form.patronymic.$dirty && !$v.form.patronymic.minLength" class="invalid-message">
            Здесь должно быть больше 5 символов
          </p>      
        </div>

        <div class="form-group">
          <label for="date">Дата рождения</label>
          <input
            id="date"
            placeholder="Дата рождения"
            class="form-control"
            :class="$v.form.date.$error ? 'is-invalid' : '' "
            v-model.trim="form.date" 
          >
          <p v-if="$v.form.date.$dirty && !$v.form.date.required" class="invalid-message">
            Обязательное поле
          </p>
          <p v-if="$v.form.date.$dirty && !$v.form.date.minLength" class="invalid-message">
            Здесь должно не менее 8 символов
          </p>      

          <label for="number">Номер телефона</label>
          <input
            id="number"
            placeholder="Номер телефона"
            class="form-control" 
            :class="$v.form.number.$error ? 'is-invalid' : '' "
            v-model.trim="form.number"
          >   
          <p v-if="$v.form.number.$dirty && !$v.form.number.required" class="invalid-message">
            Обязательное поле
          </p>
          <p v-if="$v.form.number.$dirty && !$v.form.number.minLength" class="invalid-message">
            Здесь должно быть 11 символов
          </p>   
          <p v-if="$v.form.number.$dirty && !$v.form.number.maxLength" class="invalid-message">
            Здесь должно быть 11 символов
          </p>      
        </div>

        <div class="form-radio">
          <label class="form-radio--tittle">Пол: </label>
          <label class="radio">
            <input type="radio" class="radio__real" value="male" 
            name="exampleRadios" id="male" 
            v-model="form.gendere"
            >
            <span class="radio__fake"></span>
            <span class="radio__title">Мужской</span>

          </label>
          <label class="radio">
            <input type="radio" class="radio__real" value="female" 
            name="exampleRadios" id="female" 
            v-model="form.gendere"
            >
            <span class="radio__fake"></span>
            <span class="radio__title">Женский</span>

          </label>
        </div>

        <div class="form-group">
          <label for="client">Группа клиентов</label>
          <select id="client" 
            class="form-control" 
            v-model="form.client" multiple 
          >
            <option
              v-for="(client, index) in clients"
              :value="client.value"
              :key="index"
            > 
              {{ client.label }}
            </option>

          </select>

          <label for="doctor">Лечащий врач</label>
          <select id="doctor" 
            class="form-control" 
            v-model="form.doctor"
          >
            <option
              v-for="(doctor, index) in doctors"
              :value="doctor.value"
              :key="index"
            >
              {{ doctor.label }}
            </option>
          </select>
        </div>


        <div class="form-check">
          <label class="checkbox">    
            <input type="checkbox" class="checkbox__real" id="notification" 
            v-model="form.disagreeWithSendMessage"
            >
            <span class="checkbox__fake"></span>
            <span class="checkbox__title">Не отправлять СМС</span>
          </label>
        </div>
        
        <div class="buttons">
        <button @click="nextStep" type="button" class="btn">
          Следующий шаг
        </button>
        </div>

      </div>

      <div v-show="step === 2" class="step">

        <div class="form-group">   
          <label for="index">Индекс</label> 
          <input
            id="index"
            placeholder="Индекс"
            class="form-control" 
            :class="$v.form.index.$error ? 'is-invalid' : '' "
            v-model.trim="form.index"
          >   
          <p v-if="$v.form.index.$dirty && !$v.form.index.minLength" class="invalid-message">
            Здесь должно быть 6 символов
          </p>   
          <p v-if="$v.form.index.$dirty && !$v.form.index.maxLength" class="invalid-message">
            Здесь должно быть 6 символов
          </p>      

          <label for="country">Страна</label>
          <input id="country" placeholder="Страна проживания"
           class="form-control" v-model="form.country"
          >

          <label for="region">Область</label>
          <input id="region" placeholder="Область"
           class="form-control" v-model="form.region"
          >

          <label for="city">Город</label>
          <input id="city" placeholder="Город"
           class="form-control"
           :class="$v.form.city.$error ? 'is-invalid' : '' "
           v-model="form.city"
          >
          <p v-if="$v.form.name.$dirty && !$v.form.name.required" class="invalid-message">
            Обязательное поле
          </p>

          <label for="street">Улица</label>
          <input id="street" placeholder="Улица"
           class="form-control" v-model="form.street"
          >
          <label for="house">Дом</label>
          <input id="house" placeholder="Дом"
           class="form-control" v-model="form.house"
          >
        </div>

        <div class="buttons">
        <button @click="backStep" type="button" class="btn">
          Назад
        </button>

        <button @click="nextStep" type="button" class="btn">
          Следующий шаг
        </button>
        </div>

      </div> 

      <div v-show="step === 3" class="step">

        <div class="form-group">   

          <label for="index">Серия</label> 
          <input
            id="index"
            placeholder="Серия"
            class="form-control" 
            :class="$v.form.index.$error ? 'is-invalid' : '' "
            v-model.trim="form.index"
          >   
          <p v-if="$v.form.index.$dirty && !$v.form.index.minLength" class="invalid-message">
            Здесь должно быть 6 символов
          </p>   
          <p v-if="$v.form.index.$dirty && !$v.form.index.maxLength" class="invalid-message">
            Здесь должно быть 6 символов
          </p>      

          <label for="country">Номер</label>
          <input id="country" placeholder="Номер"
           class="form-control" v-model="form.country"
          >

          <label for="region">Кем выдан</label>
          <input id="region" placeholder="Кем выдан"
           class="form-control" v-model="form.region"
          >

          <label for="date">Дата выдачи</label>
          <input
            id="date"
            placeholder="Дата выдачи"
            class="form-control"
            :class="$v.form.date.$error ? 'is-invalid' : '' "
            v-model.trim="form.date" 
          >
          <p v-if="$v.form.date.$dirty && !$v.form.date.required" class="invalid-message">
            Обязательное поле
          </p>
          <p v-if="$v.form.date.$dirty && !$v.form.date.minLength" class="invalid-message">
            Здесь должно не менее 8 символов
          </p>  
          
        </div>

        <div class="buttons">
        <button @click="backStep" type="button" class="btn">
          Назад
        </button>

        <button type="submit" class="btn">
          Зарегистрироваться
        </button>
        </div>


      </div> 
    </form>
  </div>
</template>

<script>
import { validationMixin } from 'vuelidate'
import { required, minLength, maxLength } from 'vuelidate/lib/validators'

export default {
  mixins: [validationMixin],
  name: "Form",
  data() {
    return {
      step: 1,
      form: {
        surname: '',
        name: '',
        patronymic: '',
        date: '',
        number: '',
        client: ['Vip'],
        doctor: 'Ivanov',
        gendere: 'male',
        disagreeWithSendMessage: false,
        index: '',
        country: '',
        region: '',
        city: '',
      },
      clients: [
        {
          label: 'VIP',
          value: 'Vip'
        },
        {
          label: 'Проблемные',
          value: 'Problematic'
        },
        {
          label: 'ОМС',
          value: 'OMS'
        },
      ],
      doctors: [
        {
          label: 'Иванов',
          value: 'Ivanov'
        },
        {
          label: 'Захаров',
          value: 'Zaharov'
        },
        {
          label: 'Чернышева',
          value: 'Chernysheva'
        } 
      ],

    } 
  },

  validations: {
    form:{
      surname: { required, minLength: minLength(5) },
      name: { required, minLength: minLength(5) },
      patronymic: { required, minLength: minLength(5) },
      date: { required, minLength: minLength(8) },
      number: { required, minLength: minLength(11), maxLength: maxLength(11) },
      index: {  minLength: minLength(6), maxLength: maxLength(6) },
      city: { required },

    }
  },

  methods: {
    checkForm(){
      this.$v.form.$touch()
      if (this.$v.form.$error) {
        console.log('Валидация прошла успешно')
      }
    },
    nextStep() {
      this.step++
    },
    backStep() {
      this.step--
    }
  }
};
</script>

<style lang="sass" scoped>
@import url(https://fonts.googleapis.com/css?family=Roboto:300);

$font-stack:    'Roboto', sans-serif

$text-size: 18px
$text-size--general: 16px
$text-size--msg: 12px
$text-color: #666666  
$text-color--important: #2e2e2e
$text-color--msg:  #ff0000

$color-general: #bfbfbf38
$color-hover: #b4b7c5
$color-btn--border: #4e5365
$color-btn--hover: #96a0e8
$color-invalid: #da0d0dfc 
/*$color-bg-btn: #85cc85

*
  box-sizing: border-box

.container
  display: flex
  justify-content: center
  margin: 0 auto
  padding-top: 20px
  max-width: 400px

  
form
  width: 100%
  font-family: $font-stack
  font-size: $text-size
  color: $text-color

.step > * + *
  margin: 40px 0 30px 0


/* Form group with text input */
.form-control
  display: block
  font-family: inherit
  width: 100%
  margin-top: 7px
  padding: 10px
  border: 0
  border-radius: 10px
  box-shadow: 0px 0px 11px 4px $color-general
  transition: .3s box-shadow

.form-control:hover 
  box-shadow: 0 0 15px 4px $color-hover

.form-control:focus 
  outline:none !important

.is-invalid
  box-shadow: 0px 0px 8px 2px $color-invalid



.form-group label
  display: flex
  color: $text-color--important
  margin-top: 20px


/* Text for invalid input */
.invalid-message
  font-family: inherit 
  font-size: $text-size--msg
  color: $text-color--msg


/* Checkbox radio input */ 
.form-radio
  display: flex
  justify-content: space-between


.form-radio--tittle
  color: $text-color--important

/*.form-radiocheck-label 
  font-size: $text-size--general
  margin-right: 30px
  cursor: pointer

/*.form-radiocheck-input 
  margin: 20px 10px 
.radio


.radio__real
  width: 1px
  height: 1px
  position: absolute
  opacity: 0
  appearance: none  /* отображение элемента */


.radio__fake
  position: relative
  display: inline-block
  vertical-align: bottom
  margin-right: 10px
  width: 20px
  height: 20px
  background-color: #ffffff
  border: 1px solid #777777
  border-radius: 50%


.radio__real:checked + .radio__fake
  background-color: $color-general


.radio__real:checked + .radio__fake::before
  position: absolute
  top: 50%
  left: 50%
  content: ""
  border-radius: 50%
  width: 8px
  height: 8px
  background: #3c3c3c
  transform: translate(-50%, -50%)

.radio__title
  font-size: $text-size--general
  margin-right: 30px
  cursor: pointer

/* Checkbox disagree with message */
.form-check
  display: flex

.checkbox
  cursor: pointer

.checkbox__real
  width: 1px
  height: 1px
  position: absolute
  opacity: 0
  appearance: none


.checkbox__fake
  position: relative
  display: inline-block
  vertical-align: bottom
  margin-right: 10px
  width: 20px
  height: 20px
  background-color: transparent
  border: 1px solid #777777
  border-radius: 2px

.checkbox__real:checked + .checkbox__fake
  background-color: $color-general

.checkbox__real:checked + .checkbox__fake::before
  position: absolute
  top: 50%
  left: 50%
  content: ""
  width: 18px
  height: 18px
  background-image: url(../assets/check.svg)
  background-size: initial
  background-position: center
  transform: translate(-50%, -50%)  

.checkbox__title      
  font-size: $text-size--general
  margin-left: 10px


/*.form-check-label
  font-size: $text-size--general
  margin-left: 10px
  cursor: pointer

/* Submit button */

.buttons
  display: flex
  justify-content: space-between


.btn
  padding: 0 20px
  height: 50px
  border: 1px solid $color-btn--border
  border-radius: 4px
  background-color: transparent
    
  font-family: inherit
  text-align: center
  font-size: $text-size--general
  line-height: 1
  color: $text-color--important

  cursor: pointer
  transition: all 0.2s ease-in

.btn:hover
  border: 1px solid $color-btn--hover
  color: $color-btn--hover

</style>