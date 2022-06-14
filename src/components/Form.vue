<template>
<div class="form align-self-center">
  <h1 class="form_header">Form</h1>
  <div class="form_body">
    <div class="row">
      <div class="col-12 col-xl pt-3">
          <label>Imię</label>
          <input class="form-control" v-model="formObject.name"/>
          <div v-show="errors.name" class="form_validate">
            {{errors.name}}
          </div>
      </div>
      <div class="col-12 col-xl pt-3">
          <label>Hasło</label>
          <input class="form-control" type="password" v-model="formObject.password"/>
          <div v-show="errors.password" class="form_validate">
            {{errors.password}}
          </div>
      </div>
    </div>
    <div class="row">
      <div class="col-24 col-xl pt-3">
          <label>Wiadomść</label>
          <textarea class="form-control" rows="5" v-model="formObject.description"></textarea>
          <div v-show="errors.description" class="form_validate">
            {{errors.description}}
          </div>
      </div>
    </div>
    <div class="row">
      <div class="col-12 col-xl pt-3">
        <button type="submit" class="btn btn-primary" @click="chekForm()">Wyślij</button>
      </div>
    </div>

    <div :class="['mt-3', 'note_' + note.type]">
      <div class="col-12 col-xl">
        <p>{{note.message}}</p>
      </div>
    </div>
  </div>
</div> 
</template>

<script>
  import axios from 'axios';
  const API_URL = "https://task-front-1.free.beeceptor.com";
  export default {
    data() {
      return {
        formObject: {},
        errors: {},
        note: {}
      }
    },
    methods: {
      async sentForm(data = false) {
        var data = {
          'name' :this.formObject.name,
          'password' :this.formObject.password,
          'description' :this.formObject.description,
        };

        try {
          await axios.post(API_URL, data);
          this.note = {
            type: 'success',
            message: 'Pomyślnie wysłano dane z formularza.'
          };
        } catch (error) {
          this.note = {
            type: 'error',
            message: error.response.data.error.message
          };
        }
      },
      chekForm() {
        this.errors = {};

        if (!this.formObject.name) {
          this.errors.name = "Imię jest wymagane.";
        }
        if (!this.formObject.password) {
          this.errors.password = 'Hasło jest wymagane.';
        }
        if (!this.formObject.description) {
          this.errors.description = 'Treść wiadomości jest wymagana.';
        }

        if (Object.getPrototypeOf(this.errors) === Object.prototype) {
          this.sentForm();
        }
      },
    },
  }

</script>


<style>
.form_validate{
  margin-top: 0.25rem;
  font-size: 0.875em;
  color: #dc3545;
}

.note_error{
  padding: 5px;
  color: #dc3545;
  background-color: #decccc;
  text-align: center;
}


.note_succes{
  padding: 5px;
  color: #2fb54e;
  background-color: #a2dbaf;
  text-align: center;
}
</style>
