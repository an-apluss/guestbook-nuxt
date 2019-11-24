<template>
  <div class="container mt-5">
    <div class="alert alert-success text-center mt-5" v-if="saved">
      <strong>Success!</strong> {{ successMessage }}
    </div>
    <div class="row mt-5">
      <div :key="signature.id" class="mr-4" v-for="signature of signatures">
        <div class="card card-deco" style="width: 20rem;">
          <div class="card-body">
            <h5 class="card-title">{{ signature.last_name }} {{ signature.first_name }}</h5>
            <p class="card-subtitle mt-2">{{ signature.email }}</p>
            <p class="card-text my-4">{{ signature.message }}</p>
            <nuxt-link class="mr-4 buttn btn-view text-white px-2 py-1" :to="`signatures/${signature.id}/view`">View</nuxt-link>
            <nuxt-link class="mr-4 buttn btn-view text-white px-2 py-1" :to="`signatures/${signature.id}/edit`">Edit</nuxt-link>
            <a class="mr-4 buttn btn-delete text-white px-2 py-1" @click.prevent="remove(signature.id)">Delete</a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import _ from 'lodash'

export default {
  async asyncData() {
    return {
      error: false,
      saved: false, 
      successMessage: null, 
      signatures: []
    }
  },

  created() {
    this.fetchSignatures()
  },

  methods: {
    async fetchSignatures() {
      const { data } = await axios.get('https://online-guestbook.herokuapp.com/api/signatures')
      this.signatures = data.data
    },

    async remove(signatureId) {
      this.saved = false

      if(confirm('Are you sure you want to delete this signature?')) {
        const { data } = await axios.delete(`https://online-guestbook.herokuapp.com/api/signatures/${signatureId}`)
        const { status } = data

        if (status === 'success') {
          this.signatures = _.remove(this.signatures, function (signature) {
            return signature.id !== signatureId;
          })
          this.saved = true
          this.successMessage = data.message
        }
      }
    }
  }
}
</script>

<style scoped>
 .card-deco {
    border-radius: 5px;
    background-color: #1e2885;
    color: #ffffff;
    box-shadow: 1px 1px 1px 1px #202b8d;
  }

  .buttn {
   border-radius: 5px; 
   text-decoration: none;
  }

  .btn-view {
    border: 1px solid orangered;
  }

  .btn-delete {
    border: 1px solid red;
    cursor: pointer;
  }
</style>>