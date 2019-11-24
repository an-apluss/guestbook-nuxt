<template>
  <div class="container">
    <div class="alert alert-danger text-center mt-5" v-if="error">
      <strong>Unsuccessful!</strong> {{ errorMessage }}
    </div>
    <div v-else>
      <div class="alert alert-success text-center mt-5" v-if="saved">
        <strong>Success!</strong> {{ successMessage }}
      </div>

      <div class="mt-5" id="signature-form">
        <h4 class="text-center mb-3">Edit the guestbook</h4>
        <form method="post" @submit.prevent="onSubmit">
          <div class="form-row">
            <div class="form-group col-md-4">
              <label for="first_name">Firstname</label>
              <input type="text"
                class="form-control input-deco" 
                name="first_name"
                v-model="signature.first_name"
                required>
              <span class="help-block text-danger"></span>
            </div>
            <div class="form-group col-md-4">
              <label for="last_name">Lastname</label>
              <input type="text"
                class="form-control input-deco"
                placeholder="Lastname"
                name="last_name"
                v-model="signature.last_name"
                required>
              <span class="help-block text-danger"></span>  
            </div>
            <div class="form-group col-md-4">
              <label for="email">E-mail</label>
              <input type="email"
                class="form-control input-deco"
                placeholder="Email"
                name="email"
                v-model="signature.email"
                required>
              <span class="help-block text-danger"></span>
            </div>
          </div>
          <div class="form-group">
            <label for="message">Message</label>
            <textarea class="form-control input-deco"
              id="message"
              placeholder="Please enter your message here..."
              name="message"
              v-model="signature.message"
              rows="5"
              required></textarea>
          </div>
          <div class="form-group">
            <button type="submit" class="btn btn-submit btn-lg">UPDATE</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {

  async asyncData({ params }) {
    try {
      const { data } = await axios.get(`https://online-guestbook.herokuapp.com/api/signatures/${params.signatureId}`)
      return { 
        error: false,
        saved: false,
        successMessage: null,
        signature: {
          id: data.data.id,
          first_name: data.data.first_name,
          last_name: data.data.last_name,
          email: data.data.email,
          message: data.data.message
        }
      } 
    } catch ({response}) {
      return {
        error: true,
        errorMessage: response.data.error
      }
    }
  },

  methods: {
    onSubmit() {
      this.saved = false;
      axios.post(`https://online-guestbook.herokuapp.com/api/signatures/${this.signature.id}`, this.signature)
        .then(({data}) => {
          const { status } = data
          console.log(data.message)
          if (status === 'success') {
            this.successMessage = data.message
            this.setSuccessMessage(data)
          }
        })
    },

    setSuccessMessage(data) {
      this.saved = true;
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
