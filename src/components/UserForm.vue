<template lang="pug">
  form#create-profile(
    value="create",
    @submit.prevent="createUser",
    v-model="$v.form.$model"
  )
    .grid-wrapper
      h1.form-title Карта пациента
      .bio-wrapper
        h3 Основное:
        InputValid(
          :input-item="inputs.surname"
        )

        label( for="surname") Фамилия*: 
        input(
          id="surname"
          v-model.trim="$v.bio.surname.$model"
          type="text"
          name="surname"
        )
        div.notice
          span.notice-error(v-if="$v.bio.surname.$error && !$v.bio.surname.required") Обязательное поле
          span.notice-error(v-if="$v.bio.surname.$error && !$v.bio.surname.minLength") Не менее {{$v.bio.surname.$params.minLength.min}} символов
        label( for="name") Имя*: 
        input(
          id="name"
          v-model.trim="$v.bio.name.$model"
          type="text"
          name="name"
        )
        div.notice
          span.notice-error(v-if="$v.bio.name.$error && !$v.bio.name.required") Обязательное поле
          span.notice-error(v-if="$v.bio.name.$error && !$v.bio.name.minLength") Не менее {{$v.bio.name.$params.minLength.min}} символов
        label( for="patronym") Отчество: 
        input(
          id="patronym"
          v-model="$v.bio.patronym.$model"
          type="text"
          name="patronym"
        )
        div.notice
          span.notice-error(v-if="$v.bio.patronym.$error && !$v.bio.patronym.minLength") Не менее {{$v.bio.patronym.$params.minLength.min}} символов
        label( for="dob") Дата рождения*: 
        input(
          id="dob"
          v-model.trim="$v.bio.dob.$model"
          type="text"
          name="dob"
        )
        div.notice
          span.notice-error(v-if="$v.bio.dob.$error && !$v.bio.dob.numeric") Только цифры!
          span.notice-error(v-if="$v.bio.dob.$error && !$v.bio.dob.required") Обязательное поле
        label( for="phone") Номер телефона*: 
        input(
          id="phone"
          v-model.trim="$v.bio.phone.$model"
          type="text"
          name="phone"
        )
        div.notice
          span.notice-error(v-if="$v.bio.phone.$error && !$v.bio.phone.required") Обязательное поле
          span.notice-error(v-if="$v.bio.phone.$error && !$v.bio.phone.numeric") Только цифры!
          span.notice-error(v-if="$v.bio.phone.$error && $v.bio.phone.numeric && !$v.bio.phone.minLength") Не менее {{$v.bio.phone.$params.minLength.min}} символов
        span Пол:
        .gender-radio
          input(
            id="gendermale"
            v-model.trim="$v.bio.gender.$model"
            type="radio"
            name="gender"
            value="male"
          )
          label(for="gendermale") муж.
          input(
            id="genderfemale"
            v-model.trim="$v.bio.gender.$model"
            type="radio"
            name="gender"
            value="female"
          )
          label(for="genderfemale") жен.
        div.notice
        label(
          for="group"
          :class="{ 'invalid': $v.bio.group.$error }"
          ) Группа клиентов*:
        select(
          id="group"
          v-model.trim="$v.bio.group.$model"
          type="text"
          name="group"
          multiple="on"
        )
          option VIP
          option Проблемные
          option ОМС
        div.notice
          span.notice-error(v-if="$v.bio.group.$error && !$v.bio.group.required") Обязательное поле

        label( for="doctor") Лечащий врач: 
        select(
          id="doctor"
          v-model.trim="$v.bio.doctor.$model"
          type="text"
          name="doctor"
        )
          option Иванов
          option Захаров
          option Чернышева
        div.notice

        label(for="sendsms") Не отправлять СМС: 
        input(
          id="sendsms"
          v-model.trim="$v.bio.sendsms.$model"
          type="checkbox"
          name="sendsms"
        )
        div.notice
      .address-wrapper
        h3 Адрес:
        label(for="addrIndex") Индекс: 
        input(
          id="addrIndex"
          v-model="address.index"
          type="text"
          name="addrIndex"
          size="10"
        )
        label(for="country") Страна: 
        input(
          id="country"
          v-model="address.country"
          type="text"
          name="country"
        )
        label(for="region") Область: 
        input(
          id="region"
          v-model="address.region"
          type="text"
          name="region"
        )
        label(for="city") Город*: 
        input(
          id="city"
          v-model="$v.address.city.$model"
          type="text"
          name="city"
        )
        label( for="street") Улица: 
        input(
          id="street"
          v-model="address.street"
          type="text"
          name="street"
        )
        label( for="house") Дом: 
        input(
          id="house"
          v-model="address.house"
          type="text"
          name="house"
        )
      .passport-wrapper
        h3 Паспорт:
        label( for="typeDocument") Тип документа*: 
        select(
          id="typeDocument"
          v-model="passport.type"
          type="text"
          name="typeDocument"
        )
          option Паспорт
          option Свидетельство о рождении
          option Вод. удостоверение
        label( for="docSerial") Серия: 
        input(
          id="docSerial"
          v-model="$v.passport.serial.$model"
          type="text"
          name="docSerial"
        )
        label( for="docNumber") Номер: 
        input(
          id="docNumber"
          v-model="passport.number"
          type="text"
          name="docNumber"
        )
        label( for="issueBy") Кем выдан: 
        input(
          id="issueBy"
          v-model="passport.issueBy"
          type="text"
          name="issueBy"
        )
        label( for="issueDate") Дата выдачи*: 
        input(
          id="issueDate"
          v-model="$v.passport.issueDate.$model"
          type="text"
          name="issueDate"
        )
      input.submit-create(type="submit" value="Сохранить")
</template>

<script>
import { required, minLength, numeric } from 'vuelidate/lib/validators'
import InputValid from './InputValid.vue'


export default {
  components: {
    InputValid
  },
  data() {
    return {
      inputs: {
        surname: {
          type: "text",
          name: "surname",
          title: "Фамилия",
          validators: [
            "required",
            ["minLength", 2],
            "alpha"
          ]
        }
      },
      bio: {
        name: "",
        surname: "",
        patronym: "",
        dob: "",
        phone: "7",
        gender: "",
        group: [],
        doctor: [],
        sendsms: false,
      },
      address: {
        index: "",
        country: "",
        region: "",
        city: "",
        street: "",
        house: ""
      },
      passport: {
        type: [],
        serial: "",
        number: "",
        issueBy: "",
        issueDate: "",

      },
      form: false,
      submitErr: false
    }
  },
  methods: {
    createUser: function() {
      console.log('Hey!')
      this.$v.$touch()
      if (this.$v.$invalid) {
        this.submitErr = true
      } else {
        this.submitErr = false
      }
    }
  },
  validations: {
    bio: {
      name: {
        required,
        minLength: minLength(2)
      },
      surname: {
        required,
        minLength: minLength(2)
      },
      patronym: {
        minLength: minLength(2)
      },
      dob: {
        required,
        numeric
      },
      gender: {

      },
      phone: {
        required,
        minLength: minLength(11),
        numeric
      },
      group: {
        required
      },
      doctor: {

      },
      sendsms: {

      }
    },
    address: {
      index: {
        numeric
      },
      country: {},
      region: {},
      city: {
        required
      },
      street: {},
      house: {}
    },
    passport: {
      type: {
        required
      },
      serial: {},
      number: {
        numeric
      },
      issueBy: {},
      issueDate: {
        required
      },

    },
    form: ['bio', 'address', 'passport'],
  }
}
</script>

<style lang="scss" scoped>
#create-profile {
  &.form--error {
    color: red;
    h1 {
      font-style: italic;
    }
  }
  input[type="text"] {
    height: 30px;
    border: 0;
    background: none;
    font-size: 14px;
    border-bottom: 2px solid rgba($color: #000000, $alpha: .3);
    outline: none;
    &:focus {
      border-bottom-color: rgba($color: #000000, $alpha: .6);
    }
  }
  .notice-error {
    padding: 0;
    margin: 0;
    margin-left: 10px;
    color: red;
    font-size: 12px;
  }
}
.grid-wrapper {
  display: grid;
  grid-template-columns: repeat(12, minmax(82px, 1fr));
  grid-gap: 1em;
  align-items: baseline;
  .form-title {
    grid-column: 1/-1;
  }
  .submit-create {
    grid-column: 1/-1;
    justify-self: center;
  }
  .bio-wrapper {
    display: grid;
    grid-area: 2/1/4/7;
    grid-template-columns: repeat(3, 1fr);
    grid-gap: 1em;
    align-items: end;
    h3 {
      grid-column: 1/-1;
      justify-self: start;
    }
    label {
      justify-self: start;
      margin-left: 1em;
      &[for="group"] {
        align-self: baseline;
      }
    }
    input {
      justify-self: stretch;
      &#sendsms {
        justify-self: start;
      }
    }

    // #group {
    //   grid-row: span 2
    // }
    .notice {
      justify-self: start;
    }
  }
  .passport-wrapper {
    display: grid;
    grid-column: 7/-1;
    grid-template-columns: repeat(2, 1fr);
    h3 {
      grid-column: 1/-1;
      justify-self: start;
    }
  }
  .address-wrapper {
    display: grid;
    grid-column: 7/-1;
    grid-template-columns: repeat(2, 1fr);
    h3 {
      grid-column: 1/-1;
      justify-self: start;
    }
  }
}

h1.form-title::after {
  content: "";
  position: absolute;
  top: 80px;
  right: 25%;
  width: 50%;
  height: 0px;
  border: 1px solid rgb(110, 77, 77);
}
input.submit-create {
  width: 100px;
  height: 40px;
}

h3 {
  text-decoration: underline;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: block;
  margin: 5px 10px;
}
a {
  color: #42b983;
}

</style>
