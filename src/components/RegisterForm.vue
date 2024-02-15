<template>
    <form @submit.prevent="submitForm">
      <div>
        <label for="username">Username:</label>
        <input id="username" v-model="form.username" type="text" @blur="validateUsername" />
        <p v-if="errors.username">Username must be at least 2 characters long and contain no spaces.</p>
      </div>
  
      <div>
        <label for="email">Email:</label>
        <input id="email" v-model="form.email" type="email" @blur="validateEmail" />
        <p v-if="errors.email">Please enter a valid email.</p>
      </div>
  
      <div>
        <label for="password">Password:</label>
        <input id="password" v-model="form.password" type="password" @blur="validatePassword" />
        <p v-if="errors.password">Password must be at least 6 characters long.</p>
      </div>
  
      <div>
        <label for="confirmPassword">Confirm Password:</label>
        <input id="confirmPassword" v-model="form.confirmPassword" type="password" @blur="validateConfirmPassword" />
        <p v-if="errors.confirmPassword">Passwords must match.</p>
      </div>
  
      <button type="submit" :disabled="hasErrors">Submit</button>
    </form>
  </template>
  
  <script>
  import { ref, computed } from 'vue'
  
  export default {
    setup() {
      const form = ref({
        username: '',
        email: '',
        password: '',
        confirmPassword: ''
      })
  
      const errors = ref({
        username: false,
        email: false,
        password: false,
        confirmPassword: false
      })
  
      const validateUsername = () => {
        errors.value.username = form.value.username.length < 2 || /\s/.test(form.value.username)
      }
  
      const validateEmail = () => {
        const emailRegex = /\S+@\S+\.\S+/
        errors.value.email = !emailRegex.test(form.value.email)
      }
  
      const validatePassword = () => {
        errors.value.password = form.value.password.length < 6
      }
  
      const validateConfirmPassword = () => {
        errors.value.confirmPassword = form.value.password !== form.value.confirmPassword
      }
  
      const submitForm = () => {
        validateUsername()
        validateEmail()
        validatePassword()
        validateConfirmPassword()
  
        if (!hasErrors.value) {
            alert('Form submitted!')
        }
      }
  
      const hasErrors = computed(() => Object.values(errors.value).some(error => error))
  
      return { form, errors, submitForm, hasErrors, validateUsername, validateEmail, validatePassword, validateConfirmPassword }
    }
  }
  </script>