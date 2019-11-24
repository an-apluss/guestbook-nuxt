<template>
  <div class="container mt-5">
    <div class="alert alert-danger text-center mt-5" v-if="error">
      <strong>Unsuccessful!</strong> {{ errorMessage }}
    </div>
    <div class="card mb-3 card-deco" v-else>
      <div class="card-body">
        <h3 class="card-title mb-4">{{ signature.last_name }} {{ signature.first_name }}</h3>
        <h5 class="reomve-mb"><label for="email" style="color: orangered;">Email Address</label></h5>
        <h6 class="card-text remove-mt">{{ signature.email }}</h6>
        <h5 class="reomve-mb mt-4"><label for="message" style="color: orangered;">Message</label></h5>
        <p class="card-text remove-mt">{{ signature.message }}</p>
        <p class="card-text"><small class="text-muted">Signed on {{ signature.created_at }}</small></p>
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
        signature: data.data
      } 
    } catch ({response}) {
      return {
        error: true,
        errorMessage: response.data.error
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

  .reomve-mb {
    margin-bottom: 0;
  }

  .remove-mt {
    margin-top: 0;
  }
</style>