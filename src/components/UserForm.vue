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
          v-for="input in inputs.bio"
          :item="input"
          :key="input.name"
        )
      .address-wrapper
        h3 Адрес:
        InputValid(
          v-for="input in inputs.address"
          :item="input"
          :key="input.name"
        )
      .passport-wrapper
        h3 Паспорт:
        InputValid(
          v-for="input in inputs.passport"
          :item="input"
          :key="input.name"
        )
      input.submit-create(type="submit" value="Сохранить")
</template>

<script>
import * as vuelidate from 'vuelidate/lib/validators'
import InputValid from './InputValid.vue'

export default {
  components: {
    InputValid
  },
  data() {
    return {
      childata: {},
      inputs: {
        bio: {
          surname: {
            type: "text",
            name: "surname",
            title: "Фамилия*:",
            listener: this.getChildData,
            validators: {
              required: true,
              minLength: 2,
              alpha: true
            }
          },
          name: {
            type: "text",
            name: "name",
            title: "Имя*:",
            listener: this.getChildData,
            validators: {
              required: true,
              minLength: 2,
              alpha: true
            }
          },
          patronym: {
            type: "text",
            name: "patronym",
            title: "Отчество:",
            listener: this.getChildData,
            validators: {
              minLength: 2,
              alpha: true
            }
          },
          dob: {
            type: "text",
            name: "dob",
            title: "Дата рождения*:",
            listener: this.getChildData,
            validators: {
              required: true,
              numeric: true
            }
          },
          gender: {
            type: "radio",
            name: "gender",
            title: "Пол:",
            listener: this.getChildData
          },
          phone: {
            type: "text",
            name: "phone",
            title: "Имя*:",
            listener: this.getChildData,
            validators: {
              required: true,
              minLength: 11,
              numeric: true
            }
          },
          group: {
            type: "multiselect",
            name: "group",
            title: "Группа клиентов*:",
            options: [
              "VIP",
              "Проблемные",
              "ОМС"
            ],
            listener: this.getChildData,
            validators: {
              required: true
            }
          },
          doctor: {
            type: "select",
            name: "doctor",
            title: "Лечащий врач*:",
            options: [
              "Иванов",
              "Захаров",
              "Чернышева"
            ],
            listener: this.getChildData
          },
          sendsms: {
            type: "checkbox",
            name: "sendsms",
            title: "Не отправлять смс:",
            listener: this.getChildData
          }
        },
        address: {
          index: {
            type: "text",
            name: "index",
            title: "Индекс:",
            listener: this.getChildData,
            validators: {
              numeric: true
            }
          },
          country: {
            type: "text",
            name: "country",
            title: "Страна:",
            listener: this.getChildData,
            validators: {
              alpha: true
            }
          },
          region: {
            type: "text",
            name: "region",
            title: "Область:",
            listener: this.getChildData,
            validators: {
              alpha: true
            }
          },
          city: {
            type: "text",
            name: "city",
            title: "Город*:",
            listener: this.getChildData,
            validators: {
              required: true,
              alpha: true
            }
          },
          street: {
            type: "text",
            name: "street",
            title: "Улица:",
            listener: this.getChildData,
            validators: {
              alphaNum: true
            }
          },
          house: {
            type: "text",
            name: "house",
            title: "Дом:",
            listener: this.getChildData
          }
        },
        passport: {
          type: {
            type: "text",
            name: "type",
            title: "Тип документа:",
            listener: this.getChildData,
            validators: {
              required: true
            }
          },
          serial: {
            type: "text",
            name: "serial",
            title: "Серия:",
            listener: this.getChildData
          },
          number: {
            type: "text",
            name: "number",
            title: "Номер:",
            listener: this.getChildData,
            validators: {
              numeric: true
            }
          },
          issueBy: {
            type: "text",
            name: "issueBy",
            title: "Кем выдан:",
            listener: this.getChildData,
            validators: {
              numeric: true
            }
          },
          issueDate: {
            type: "text",
            name: "issueDate",
            title: "Дата выдачи:",
            listener: this.getChildData,
            validators: {
              required: true
            }
          }
        }
      },
      form: false,
      submitErr: false
    }
  },
  methods: {
    getChildData (childata) {
      this.childata[childata[0]] = childata[1]
    },
    createUser: function() {
      console.log('Vuelidators: ', vuelidate)
      this.$v.$touch()
      if (this.$v.$invalid) {
        this.submitErr = true
      } else {
        this.submitErr = false
      }
    }
  },
  validations() {
    const arr = []
    for (let child in this.childata) {
      arr.push(child)
    }
    return {
      form: arr
    }
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
}
.grid-wrapper {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
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
    display: flex;
    flex-direction: column;
    grid-column: 1/2;
  }
  .passport-wrapper {
    display: flex;
    flex-direction: column;
    grid-column: 2/-1;
  }
  .address-wrapper {
    display: flex;
    flex-direction: column;
    grid-column: 2/-1;
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
