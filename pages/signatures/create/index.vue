<template>
  <div class="container">
    <div class="alert alert-success text-center mt-5" v-if="saved">
      <strong>Success!</strong> {{ successMessage }}
    </div>

    <div class="container mt-5" id="signature-form">
      <h4 class="text-center mb-3">Sign the guestbook</h4>
      <form method="post" @submit.prevent="onSubmit">
        <div class="form-row">
          <div class="form-group col-md-4">
            <label for="first_name">Firstname</label>
            <input type="text"
              class="form-control input-deco" 
              placeholder="Firstname"
              name="first_name"
              v-model="signature.first_name"
              required>
            <span class="help-block text-danger" v-if="errors.first_name">{{ errors.first_name[0] }}</span>
          </div>
          <div class="form-group col-md-4">
            <label for="last_name">Lastname</label>
            <input type="text"
              class="form-control input-deco"
              placeholder="Lastname"
              name="last_name"
              v-model="signature.last_name"
              required>
            <span class="help-block text-danger" v-if="errors.last_name">{{ errors.last_name[0] }}</span>  
          </div>
          <div class="form-group col-md-4">
            <label for="email">E-mail</label>
            <input type="email"
              class="form-control input-deco"
              placeholder="Email"
              name="email"
              v-model="signature.email"
              required>
            <span class="help-block text-danger" v-if="errors.email">{{ errors.email[0] }}</span>
          </div>
        </div>
        <div class="form-group">
          <label for="message">Message</label>
          <textarea class="form-control input-deco"
            id="message"
            placeholder="Please enter your message here..."
            rows="5"
            name="message"
            v-model="signature.message"
            required></textarea>
          <span class="help-block text-danger" v-if="errors.message">{{ errors.message[0] }}</span>
        </div>
        <div class="form-group">
          <button type="submit" class="btn btn-submit btn-lg">SIGN</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      errors: [],
      saved: false,
      successMessage: null,
      signature: {
        first_name: null,
        last_name: null,
        email: null,
        message: null
      }
    };
  },

  methods: {
    onSubmit() {
      this.saved = false;
      axios.post('https://online-guestbook.herokuapp.com/api/signatures', this.signature)
        .then(({data}) => {
          const { status } = data
          if (status === 'success') {
            this.successMessage = data.message
            this.setSuccessMessage()
          }
        })
        .catch(({response}) => {
          this.setErrors(response)
        })
    },

    setErrors(response) {
      this.errors = response.data.error;
    },

    setSuccessMessage() {
      this.reset();
      this.saved = true;
    },

    reset() {
      this.errors = []
      this.signature = {first_name: null, last_name: null, email: null, message: null};
    }
  }
}
</script>

<style>
  .btn-submit, 
  .btn-submit:hover {
    background-color: orangered;
    color: #ffffff;
  }

  #signature-form {
    border-radius: 5px;
    background-color: #1e2885;
    color: #ffffff;
    padding: 30px;
    box-shadow: 1px 1px 1px 1px #202b8d;
  }

  label {
    font-size: 0.9em;
  }

  .input-deco {
    border-color: #1e2885;
    color: #1e2885;
  }
</style>
