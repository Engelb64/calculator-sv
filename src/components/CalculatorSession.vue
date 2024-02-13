<template>
  <div class="calculator-body" :class="theme">
    <div class="r-screen">

      <label class="switch">
        <input type="checkbox" @click="changeMode">
        <div class="slider round"></div>
      </label>

      <li class="r-list">
        <ul><strong>Usuarios:</strong> {{ usernames }}</ul>
        <ul><strong>Valores Ingresados:</strong> {{ values }} </ul>
        <ul><strong>Resultado:</strong> {{ result }} </ul>
      </li>
    </div>
    <div class="separator"></div>
    <div class="functions">
      <div class="icons-functions">
        <div>
          <i class="fa-solid fa-trash fa-2x icon" :class="{ 'fa-bounce': isBouncing }" @click="clearData"></i>
        </div>
      </div>
      <div></div>
      <div></div>
    </div>
    <div class="c-input">
      <Form @submit="recordOperation">

        <Field v-model="username" type="text" name="username" :rules="validateField" v-slot="{ field }">
          <input type="text" placeholder="Username" v-bind="field">
        </Field>
        <ErrorMessage class="msg-e" name="username" />

        <Field v-model="op1" type="text" name="op1" :rules="validateField" v-slot="{ field }">
          <input type="text" placeholder="Operando 1" v-bind="field">
        </Field>
        <ErrorMessage class="msg-e" name="op1" />

        <Field v-model="op2" type="text" name="op2" :rules="validateField" v-slot="{ field }">
          <input type="text" placeholder="Operando 2" v-bind="field">
        </Field>
        <ErrorMessage class="msg-e" name="op2" />


        <Field v-model="op3" type="text" name="op3" v-slot="{ field }">
          <input type="text" placeholder="Operando 3" v-bind="field">
        </Field>

        <button>Procesar</button>
      </Form>
    </div>
  </div>
</template>

<script>

  import { Form, Field, ErrorMessage } from 'vee-validate';

  export default {
    components: {
      Field,
      Form,
      ErrorMessage,
    },
    name: 'CalculatorSession',
    data() {
      return {
        darkTheme: true,
        username: '',
        op1: '',
        op2: '',
        op3: '',
        values: '',
        result: '',
        usernames: '',
        isBouncing: false
      };
    },
    methods: {
      validateField(value) {
        if (!value) {
          return 'Este campo es obligatorio';
        }

        return true;
      },
      recordOperation() {

        this.username = toCamelCase(this.username);

        console.log('Valor del nombre de Usuario:', this.username);
        console.log('Valor del Operando 1:', this.op1);
        console.log('Valor del Operando 2:', this.op2);
        console.log('Valor del Operando 3:', this.op3);

        let operationKey = `${this.op1}-${this.op2}-${this.op3}`;

        let storeUsers = JSON.parse(sessionStorage.getItem(operationKey)) || [];

        if (!storeUsers.includes(this.username)) {
          storeUsers.push(this.username);
          sessionStorage.setItem(operationKey, JSON.stringify(storeUsers));
        } else {
          console.log('El nombre ya existe en el array almacenado en sessionStorage.');
        }

        this.usernames = storeUsers.join(", ");

        this.values = this.op1 + ',' + this.op2;

        if (this.op3) {
          this.values = this.values + ',' + this.op3
        }

        if (!isNaN(this.op1) && !isNaN(this.op2) || !isNaN(this.op1) && !isNaN(this.op2) && !isNaN(this.op3)) {
          console.log('numeric');
          this.result = parseFloat(this.op1) + parseFloat(this.op2);

          if (parseFloat(this.op3)) {
            this.result = this.result + parseFloat(this.op3)
          }

        } else {
          this.result = this.op1 + this.op2 + this.op3;
        }

      },
      clearData() {
        this.isBouncing = true;


        this.result = '';
        this.usernames = '';
        this.values = '';

        // Después de un tiempo, detén el efecto de rebote
        setTimeout(() => {
          this.isBouncing = false;
        }, 1000);

      },
      changeMode() {
        this.darkTheme = !this.darkTheme;
        console.log(this.darkTheme)
      }
    },
    computed: {
      theme() {
        return this.darkTheme ? 'dark-theme' : 'light-theme';
      }
    }
  }

  function toCamelCase(text) {
    return text.toLowerCase().replace(/(?:^\w|[A-Z]|\b\w)/g, function (word, index) {
      return index === 0 ? word.toUpperCase() : word.toLowerCase();
    }).replace(/\s+/g, '');
  }

</script>

<style scoped>

h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

input {
  display: block;
  margin-bottom: 10px;
  margin-top: 10px;
  padding: 5px;
  font-size: 20px;
  border-width: 0px;
  background-color: #1d2127;
  color: #ffffff;
  border-radius: 10px;
  text-align: center;
}

input::placeholder {
  color: #ffffff;
  text-align: center;
}

button {
  margin: 20px;
  background-color: #937CE6;
  border-radius: 34px;
  font-size: 20px;
  padding: 4%;
  height: auto;
  width: 200px;
  border: none;
  color: #ffffff;
  cursor: pointer;
}


.light-theme {
  background-color: #C9BAFF !important;
  color: black !important;
}

.light-theme input {
  background-color: #937CE6 !important;
}

.light-theme button {
  background-color: #6344D4 !important;
}

.light-theme button {
  background-color: #6344D4 !important;
}

.light-theme .r-screen {
  background-color: #e4e4e4 !important;
}

.light-theme .icons-functions {
  color: #6344D4;
}

.light-theme .icon {
  border-color: #C9BAFF;
}

.msg-e {
  color: red
}

.calculator-body {
  text-align: -webkit-center;
  background-color: #2c3e50;
  color: white;
  max-width: 360px;
  max-height: 780px;
  position: relative;
  width: 100%;
  margin: auto;
  /*padding: 1%;*/
  border-radius: 10px;
  border-color: white;
}

.r-screen {
  border-radius: 10px 10px 0px 0px;
  background-color: #171C22;
  font-family: 'Inter', sans-serif;
  font-weight: 300;
}

.r-list {
  margin-top: 20px;
  padding: 1.5%;
  max-width: 240px;
  text-align: left;
}

.separator {
  height: 6px;
  background-color: #A430FF;
  background: linear-gradient(90deg, rgba(164, 48, 255, 1) 0%, rgba(243, 24, 173, 1) 75%, rgba(255, 33, 113, 1) 100%);
  margin: 0 0px;
  flex: 1;
}

.icons-functions {
  margin: 15px;
  text-align: right;
  color: #C9BAFF;
}

.icon {
  cursor: pointer;
  padding: 5px;
  border-radius: 15%;
  border-color: #C9BAFF;

}

.c-input {
  width: 90%;
  text-align: -webkit-center;
}

.switch {
  margin: 10px;
  position: absolute;
  display: block;
  width: 50px;
  height: 25px;
  text-align: left;
}

.switch input {
  display: none;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #6344D4;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 18px;
  width: 18px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

input:checked+.slider {
  background-color: #101010;
}

input:focus+.slider {
  box-shadow: 0 0 1px #101010;
}

input:checked+.slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}

.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}
</style>
