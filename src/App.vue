<template>
  <div class="container mt-4">
    <div class="col-sm-4 mx-auto">

      <h2 class="reg-title">Регистрация</h2>
      <form @submit.prevent="userRegister" novalidate>
        <div v-show="step === 1">

          <div v-if="regMessage" class="alert alert-success" role="alert">
             Вы успешно зарегистрировались!
          </div>

          <div class="form-group">
              <label for="name">Ваше имя</label>

              <input @blur="$v.formReg.name.$touch()"
                     :class="status($v.formReg.name)"
                     v-model.trim="formReg.name"
                     type="text" class="form-control" id="name">

              <div class="invalid-feedback" v-if="!$v.formReg.name.required">{{ reqText }}</div>
              <div class="invalid-feedback" v-if="!$v.formReg.name.alpha">{{ alphaText }}</div>
          </div>

          <div class="form-group">
              <label for="surname">Ваша фамилия</label>

              <input @blur="$v.formReg.surname.$touch()"
                     :class="status($v.formReg.surname)"
                     v-model.trim="formReg.surname"
                     type="text" class="form-control" id="surname">

              <div class="invalid-feedback" v-if="!$v.formReg.surname.required">{{ reqText }}</div>
              <div class="invalid-feedback" v-if="!$v.formReg.surname.alpha">{{ alphaText }}</div>
          </div>

          <div class="form-group">
              <label for="lastname">Ваше отчество</label>

              <input @blur="$v.formReg.lastname.$touch()"
                     :class="status($v.formReg.lastname)"
                     v-model.trim="formReg.lastname"
                     type="text" class="form-control" id="lastname">

              <div class="invalid-feedback" v-if="!$v.formReg.lastname.required">{{ reqText }}</div>
              <div class="invalid-feedback" v-if="!$v.formReg.lastname.alpha">{{ alphaText }}</div>
          </div>

          <div class="form-group">
                  <label for="year">Год рождения</label>

                  <select @blur="$v.formReg.year.$touch()"
                          :class="status($v.formReg.year)"
                          v-model="formReg.year"
                          id="year" class="custom-select">
                      <option disabled value="">Выберите</option>
                      <option v-for="(year, index) in years" 
                              :value="year"
                              :key="index">{{ year }}</option>
                  </select>

                  <div class="invalid-feedback" v-if="!$v.formReg.year.required">{{ reqText }}</div>
              </div>

          <div class="form-group">
                  <label for="phone">Телефон</label>
                  
                  <input @blur="$v.formReg.phone.$touch()"
                         :class="status($v.formReg.phone)"
                         v-model.trim="formReg.phone"
                         type="numeric" class="form-control" id="phone" placeholder="+7-9XX-XXX-XX-XX">
                    
                  <div class="invalid-feedback" v-if="!$v.formReg.phone.numeric">{{ numeric }}</div>
                  <div class="invalid-feedback" 
                       v-if="!$v.formReg.phone.minLength">{{ minLengthText }}</div>
              </div>

          <div class="form-group">
                  <label for="gender">Пол</label>

                  <select @blur="$v.formReg.gender.$touch()"
                          :class="status($v.formReg.gender)"
                          v-model="formReg.gender"
                          id="gender" class="custom-select" placeholder="Выберите">
                      <option value="1">Муж</option>
                      <option value="2">Жен</option>
                  </select>

                  <div class="invalid-feedback" v-if="!$v.formReg.gender.or">{{ or }}</div>
              </div>

          <div class="form-group">
                  <label for="clients">Группа клиентов</label>
                  <select @blur="$v.formReg.clients.$touch()"
                          :class="status($v.formReg.clients)"
                          v-model="formReg.clients"
                          id="clients" class="custom-select" size="2" placeholder="Выберите">
                      <option value="1">VIP</option>
                      <option value="2">Проблемные</option>
                      <option value="3">ОМС</option>
                  </select>

                  <div class="invalid-feedback" v-if="!$v.formReg.clients.or">{{ or }}</div>
              </div>

          <div class="form-group">
                  <label for="doctors">Лечащий врач</label>
                  <select @blur="$v.formReg.doctors.$touch()"
                          :class="status($v.formReg.doctors)"
                          v-model="formReg.doctors"
                          id="doctors" class="custom-select" size="2" placeholder="Выберите">
                      <option value="1">Иванов</option>
                      <option value="2">Захаров</option>
                      <option value="3">Чернышева</option>
                  </select>

                  <div class="invalid-feedback" v-if="!$v.formReg.doctors.or">{{ or }}</div>
              </div>

              <label>
                <input type="checkbox" v-model='checkboxes' value='SMS' name="options" id="option1" autocomplete="off" checked> Получать SMS
              </label>
              

          <div class="form-group">
              <label for="email">Email</label>

              <input @blur="$v.formReg.email.$touch()"
                     :class="status($v.formReg.email)"
                     v-model.trim="formReg.email"
                     type="text" class="form-control" id="email">

              <div class="invalid-feedback" v-if="!$v.formReg.email.required">{{ reqText }}</div>
              <div class="invalid-feedback" v-if="!$v.formReg.email.email">Пожалуйста введите Email адрес</div>
          </div>

          <button @click="step++" :disabled="disabledBtn1"
                  type="button" class="btn btn-primary">Следующий шаг</button>

        </div>

        <transition name="slide-fade">
          <div v-show="step === 2">

              <div class="form-group">
                <label for="inputZip">Индекс</label>
                <input type="text" class="form-control" id="inputZip">
              </div>

              <div class="form-group">
                  <label for="country">Страна</label>

                  <input @blur="$v.formReg.country.$touch()"
                         :class="status($v.formReg.country)"
                         v-model.trim="formReg.country"
                         type="text" class="form-control" id="country">

                  <div class="invalid-feedback" v-if="!$v.formReg.country.alpha">{{ alphaText }}</div>
              </div>

              <div class="form-group">
                  <label for="region">Область</label>

                  <input @blur="$v.formReg.region.$touch()"
                         :class="status($v.formReg.region)"
                         v-model.trim="formReg.region"
                         type="text" class="form-control" id="region">

                  <div class="invalid-feedback" v-if="!$v.formReg.region.alpha">{{ alphaText }}</div>
              </div>

              <div class="form-group">
                  <label for="city">Город</label>

                  <input @blur="$v.formReg.city.$touch()"
                         :class="status($v.formReg.city)"
                         v-model.trim="formReg.city"
                         type="text" class="form-control" id="city">

                  <div class="invalid-feedback" v-if="!$v.formReg.city.alpha">{{ alphaText }}</div>
              </div>

              <div class="form-group">
                  <label for="street">Улица</label>

                  <input @blur="$v.formReg.street.$touch()"
                         :class="status($v.formReg.street)"
                         v-model.trim="formReg.street"
                         type="text" class="form-control" id="street">

                  <div class="invalid-feedback" v-if="!$v.formReg.street.alpha">{{ alphaText }}</div>
              </div>

              <div class="form-group">
                  <label for="house">Дом</label>

                  <input @blur="$v.formReg.house.$touch()"
                         :class="status($v.formReg.house)"
                         v-model.trim="formReg.house"
                         type="text" class="form-control" id="house">

                  <div class="invalid-feedback" v-if="!$v.formReg.house.alpha">{{ alphaText }}</div>
              </div>

              <button @click="step--" type="button" class="btn btn-light mr-2">Назад</button>
              <button @click="step++" :disabled="disabledBtn2"
                      type="button" class="btn btn-primary">Следующий шаг</button>

          </div>
        </transition>

        <transition name="slide-fade">
          <div v-show="step === 3">

              <div class="form-group">
                  <label for="doctype">Тип документа</label>
                  <select @blur="$v.formReg.doctype.$touch()"
                          :class="status($v.formReg.doctype)"
                          v-model="formReg.doctype"
                          id="doctype" class="custom-select" size="2" placeholder="Выберите">
                      <option value="1">Паспорт</option>
                      <option value="2">Свидетельство о рождении</option>
                      <option value="3">Вод. удостоверение</option>
                  </select>

                  <div class="invalid-feedback" v-if="!$v.formReg.doctype.or">{{ or }}</div>
              </div>

              <div class="form-group">
                  <label for="series">Серия</label>
                  
                  <input @blur="$v.formReg.series.$touch()"
                         :class="status($v.formReg.series)"
                         v-model.trim="formReg.series"
                         type="numeric" class="form-control" id="series" placeholder="XXXX">
                    
                  <div class="invalid-feedback" v-if="!$v.formReg.series.numeric">{{ numeric }}</div>
                  <div class="invalid-feedback" 
                       v-if="!$v.formReg.series.minLength">{{ minLengthText }}</div>
              </div>

              <div class="form-group">
                  <label for="nom">Номер</label>
                  
                  <input @blur="$v.formReg.nom.$touch()"
                         :class="status($v.formReg.nom)"
                         v-model.trim="formReg.nom"
                         type="numeric" class="form-control" id="nom" placeholder="XXXXXX">
                    
                  <div class="invalid-feedback" v-if="!$v.formReg.nom.numeric">{{ numeric }}</div>
                  <div class="invalid-feedback" 
                       v-if="!$v.formReg.nom.minLength">{{ minLengthText }}</div>
              </div>

              <div class="form-group">
                  <label for="career">Кем выдан</label>
                  
                  <input @blur="$v.formReg.career.$touch()"
                         :class="status($v.formReg.career)"
                         v-model.trim="formReg.career"
                         type="text" class="form-control" 
                         id="career" placeholder="Отделом УФМС/МВД...">
                    
                  <div class="invalid-feedback" v-if="!$v.formReg.career.alpha">{{ alphaText }}</div>
              </div>

              <div class="form-group">
                  <label for="issue">Дата выдачи</label>
                  
                  <input @blur="$v.formReg.issue.$touch()"
                         :class="status($v.formReg.issue)"
                         v-model.trim="formReg.issue"
                         type="text" class="form-control" 
                         id="issue" placeholder="XX.XX.XXXX">
                    
                  <div class="invalid-feedback" v-if="!$v.formReg.issue.alpha">{{ alphaText }}</div>
              </div>

              <div class="form-group">
                  <label for="password">Пароль</label>
                  
                  <input @blur="$v.formReg.password.$touch()"
                         :class="status($v.formReg.password)"
                         v-model.trim="formReg.password"
                         type="text" class="form-control" id="password">
                    
                  <div class="invalid-feedback" v-if="!$v.formReg.password.required">{{ reqText }}</div>
                  <div class="invalid-feedback" 
                       v-if="!$v.formReg.password.minLength">{{ minLengthText }}</div>
              </div>

              <div class="form-group">
                  <label for="passwordConfirm">Подтверждение пароля</label>
                  
                  <input @blur="$v.formReg.passwordConfirm.$touch()"
                         :class="status($v.formReg.passwordConfirm)"
                         v-model.trim="formReg.passwordConfirm"
                         type="text" class="form-control" id="passwordConfirm">
                  
                  <div class="invalid-feedback" 
                       v-if="!$v.formReg.passwordConfirm.sameAs">{{ passwordConfirmText }}</div>
              </div>

              <button @click="step--" type="button" class="btn btn-light mr-2">Назад</button>
              <button :disabled="disabledBtnFinish"
                      type="submit" class="btn btn-primary">Зарегистрироваться</button>

          </div>
        </transition>
      </form>

    </div>
  </div>
</template>

<script>
import { email, required, minLength, helpers, sameAs, numeric, or } from 'vuelidate/lib/validators'

const alpha = helpers.regex('alpha', /^[a-zA-Zа-яёА-ЯЁ]*$/)

export default {
   data() {
     return {
        checkboxes: [],
        step: 1,
        regMessage: false,
        years: [],
        yearEnd: 2020,
        reqText: 'Поле обязательно для заполнения',
        alphaText: 'Запрещены цифры, пробелы и другие символы',
        minLengthText: 'Минимальная длина 6 символов!',
        passwordConfirmText: 'Пароли не совпадают',
        formReg: {
          email: '',
          or: '',
          clients: '',
          doctors: '',
          numeric: '',
          name: '',
          surname: '',
          lastname: '',
          country: '',
          city: '',
          region: '',
          street: '',
          house: '',
          year: '',
          doctype: '',
          series: '',
          nom: '',
          career: '',
          issue:'',
          password: '',
          passwordConfirm: ''
        }
     }
   },
   computed: {
    disabledBtn1() {
       return this.$v.formReg.name.$invalid || 
              this.$v.formReg.surname.$invalid || 
              this.$v.formReg.email.$invalid ||
              this.$v.formReg.lastname.$invalid ||
              this.$v.formReg.phone.$invalid ||
              this.$v.formReg.gender.$invalid ||
              this.$v.formReg.clients.$invalid ||
              this.$v.formReg.doctors.$invalid ||
              this.$v.formReg.year.$invalid 
    },
    disabledBtn2() {
       return this.$v.formReg.country.$invalid || 
              this.$v.formReg.city.$invalid ||
              this.$v.formReg.region.$invalid ||
              this.$v.formReg.street.$invalid ||
              this.$v.formReg.house.$invalid
    },
    disabledBtnFinish() {
       return this.$v.formReg.career.$invalid || 
              this.$v.formReg.password.$invalid || 
              this.$v.formReg.passwordConfirm.$invalid ||
              this.$v.formReg.doctype.$invalid ||
              this.$v.formReg.series.$invalid ||
              this.$v.formReg.nom.$invalid ||
              this.$v.formReg.issue.$invalid
    }
   },
   methods: {
    status(validation) {
       return {
         'is-invalid': validation.$error,
         'error': validation.$error
       }
    },
    userRegister() {
        console.group()
          console.log('Вы успешно зарегистрированны!')
          console.log('Ваше имя: ' + this.formReg.name)
          console.log('Ваша фамилия: ' + this.formReg.surname)
          console.log('Ваше отчество: ' + this.formReg.lastname)
          console.log('Email: ' + this.formReg.email)
          console.log('Phone: ' + this.formReg.phone)
          console.log('Gender: ' + this.formReg.gender)
          console.log('Clients: ' + this.formReg.clients)
          console.log('Лечащий врач: ' + this.formReg.doctors)
          console.log('Страна: ' + this.formReg.country)
          console.log('Область: ' + this.formReg.region)
          console.log('Город: ' + this.formReg.city)
          console.log('Улица: ' + this.formReg.street)
          console.log('Дом: ' + this.formReg.house)
          console.log('Тип документа: ' + this.formReg.doctype)
          console.log('Год рождения: ' + this.formReg.year)
          console.log('Серия: ' + this.formReg.series)
          console.log('Номер: ' + this.formReg.nom)
          console.log('Кем выдан: ' + this.formReg.career)
          console.log('Дата выдачи: ' + this.formReg.issue)
          console.log('Пароль: ' + this.formReg.password)
        console.groupEnd()

        this.reset()
        
    },
    reset() {
        // сбросить шаг и показать сообщение о регистрации
        this.step = 1;
        this.regMessage = true;

        // убрать сообщение о регистрации
        setTimeout(() => {
          this.regMessage = false
        }, 3000)

        // сбросить все поля
        for (let input in this.formReg) {
            this.formReg[input] = ''
        }

        // сбросить валидацию
        this.$v.$reset()
    }
  },
  validations: {
      formReg: {
          email: {
            email,
            required
          },
          name: {
            required,
            alpha
          },
          surname: {
            required,
            alpha
          },
          lastname: {
            required,
            alpha
          },
          phone: {
            numeric
          },
          gender: {
            or
          },
          clients: {
            or
          },
          doctors: {
            or
          },
          country: {
            alpha
          },
          region: {
            alpha
          },
          city: {
            alpha
          },
          street: {
            alpha
          },
          house: {
            alpha
          },
          year: {
            required
          },
          doctype: {
            or
          },
          career: {
            alpha
          },
          series: {
            numeric
          },
          nom: {
            numeric
          },
          issue: {
            alpha
          },
          password: {
            required,
            minLength: minLength(6)
          },
          passwordConfirm: {
            sameAs: sameAs('password')
          }
      }
  },
  created() {
      for (let i = this.yearEnd; i >= 1800; i--) this.years.push(i)
  }
}
</script>

<style>
body {
  background-color: #f1f1f1;
}

form {
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 10px 10px 45px -31px rgba(0, 0, 0, 0.75);
}

.error {
  background-color: #fdd;
}

.reg-title {
  color: #5d5d5d;
  font-size: 24px;
  margin-bottom: 18px;
  padding-left: 20px;
}

.slide-fade-enter-active {
  transition: all 0.3s ease;
}

.slide-fade-enter {
  transform: translateX(10px);
  opacity: 0;
}
</style>
