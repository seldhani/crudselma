<template>
    <div class="login-container">
      <h2>Login</h2>
      <form @submit.prevent="handleSubmit">
        <div class="form-group">
          <label for="username">Username:</label>
          <div class="input-container">
            <i class="fas fa-user icon"></i>
            <input type="text" v-model="username" class="form-control" id="username" required />
          </div>
        </div>
        <div class="form-group">
          <label for="password">Password:</label>
          <div class="input-container">
            <i class="fas fa-lock icon"></i>
            <input type="password" v-model="password" class="form-control" id="password" required />
          </div>
        </div>
        <button type="submit" class="btn btn-primary">Login</button>
      </form>
      <p v-if="error" class="text-danger">{{ error }}</p>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        username: '',
        password: '',
        error: ''
      };
    },
    methods: {
      handleSubmit() {
        const validUsername = 'selrmdh';
        const validPassword = 'makanmie';
        if (this.username === validUsername && this.password === validPassword) {
          this.$emit('login-success');
        } else {
          this.error = 'Invalid username or password';
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .login-container {
    max-width: 400px;
    margin: auto;
    padding: 2em;
    background: #f9f9f9;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    text-align: center;
  }
  
  h2 {
    margin-bottom: 1em;
    color: #333;
  }
  
  .form-group {
    margin-bottom: 1.5em;
    text-align: left;
  }
  
  .input-container {
    position: relative;
  }
  
  .input-container .icon {
    position: absolute;
    left: 10px;
    top: 50%;
    transform: translateY(-50%);
    color: #999;
  }
  
  .form-control {
    width: 100%;
    padding: 10px 10px 10px 40px;
    border: 1px solid #ccc;
    border-radius: 5px;
    box-sizing: border-box;
  }
  
  .form-control:focus {
    border-color: #66afe9;
    outline: none;
  }
  
  .btn-primary {
    background-color: #007bff;
    border: none;
    padding: 10px 20px;
    border-radius: 5px;
    color: #fff;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  .btn-primary:hover {
    background-color: #0056b3;
  }
  
  .text-danger {
    color: red;
    margin-top: 1em;
  }
  </style>
  