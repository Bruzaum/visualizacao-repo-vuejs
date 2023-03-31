<!-- eslint-disable vue/no-multiple-template-root -->
<template>
  <link rel="stylesheet" href="https://unicons.iconscout.com/release/v4.0.8/css/line.css">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Open+Sans:wght@300;500;700&display=swap" rel="stylesheet">

  <div class="container" id="Geral">
    <header>
      <div>
          <div class="header">
              <h1 class="header-txt">
                  <i class="uil uil-github"></i>
                  <span class="header-txt--strong">Github</span> profiles
              </h1>
          </div>
      </div>
      
    </header>

    <main>
      <div class="container input-username">
        <div class="input-group">
          <input
            id="input-group"
            type="text"
            class="form-control"
            placeholder="Escreva seu username"
            aria-label="username"
            aria-describedby="button-addon2"
          />
          <button
            class="btn btn-outline-secondary"
            type="button"
            id="button-addon2"
            @click="$emit('ShowView'), $emit('getUser')"
          >
            <i class="uil uil-search"></i>
          </button>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'BuscaRepo',
  emits: ['ShowView', 'getUser'],
}
</script>

<style scoped>
  * {
      box-sizing: border-box;
      color: #586069;
      list-style-type: none;
      margin: 0;
      padding: 0;
      font-weight: 400;
  }
  .header {
      background-color: #5c646d;
      display: flex;
      align-items: center;
      justify-content: space-between;
      height: 80px;
      padding: 1rem 2rem;
      min-width: 100%;
  }
  .header-txt {
      letter-spacing: 1px;
      font-weight: 400;
      color: #fff;
      font-size: 1.8rem;
  }
  .header-txt--strong{
      font-weight: 700;
      color: #fff;
      font-size: 2rem!important;
  }
  .uil-github{
      margin-right: 0.2em;
      font-weight: 700;
      color: #fff;
      font-size: 2.5rem!important;
  }
  .input-username {
      display: flex;
      justify-content: center;   
      padding-top: 2rem;
  }
  .input-group {
      width: 400px;
  }
  .input-group input {
      height: 40px;
      width: 87%;
      border-radius: 4px;
      padding: 1rem;
  }
  .input-group button {
      height: 40px;
      width: 10%;
      border-radius: 4px;
      font-weight: 700;
      cursor: pointer;
  }
</style>