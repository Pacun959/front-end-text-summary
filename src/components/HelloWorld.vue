<template>
  <div>
    <h1>Text Summarization</h1>

    <!-- Create a form to get text from the user -->
    <form @submit.prevent="fetchData">
      <label for="text">Nhập văn bản:</label><br>
      <textarea id="text" name="text" rows="9" cols="80" v-model="text"></textarea><br>
      <label for="output_sentences">Số từ tối đa:</label><br>
      <input type="number" id="output_sentences" name="output_sentences" v-model="output_sentences"><br>
      <button type="submit">Tóm tắt</button>
    </form>

    <div v-if="loading">Loading...</div>

    <div v-if="error" style="color: red;">{{ error }}</div>

    <div v-if="data">
      <h2>Tóm tắt:</h2>

        <p style="
    border: green 5px solid;
    text-align: justify;
    padding: 5px;
">{{ data.microsoft.result }}</p>

    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      text: '',  // Added to bind to the input field
      output_sentences: 40,  // Set a default value or adjust as needed
      data: null,  // Changed to null for better checking
      loading: false,
      error: null,
    };
  },
  methods: {
    fetchData() {
      this.loading = true;
      this.error = null;
      const headers = {
        'Content-Type': 'application/json',
        'accept': 'application/json',
        'Authorization': 'Bearer ' + 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoiY2NhNDVkYTgtYTY0NS00MDk0LTkxZTUtNDQ4ZmY1ZjVmNTkzIiwidHlwZSI6ImFwaV90b2tlbiJ9.J14YIoOnbQ9I2wDjI3BfYZZ8MLeHwo6YUp9sqpVr3QY',  // Replace with your actual token
      };

      // Set request body using the data property
      const requestBody = {
        text: this.text,
        providers: 'microsoft',
        language: 'vi',
        response_as_dict: 1,
        attributes_as_list: 0,
        show_original_response: 0,
        settings: {},
        output_sentences: this.output_sentences,
      };

      // Make the HTTP request
      axios.post('https://api.edenai.run/v2/text/summarize', requestBody, { headers })
        .then(response => {
          this.data = response.data;
          console.log(response.data);
          this.loading = false;
        })
        .catch(error => {
          this.error = 'Error fetching data: ' + error;
          this.loading = false;
        });
    },
  },
};
</script>

<style>
/* Add your styles here if needed */
</style>
