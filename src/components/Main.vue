<template>
  <div class="container jumbotron">
    <h1 class="display-4">Hızlı Yazma Yarışması</h1>
    <p class="lead">Ne kadar hızlı klavye kullandığını test et.</p>
    <hr class="my-4">

    <div class="card">
      <div class="card-body">
        <span v-for="(word,key) in words" :key="key" v-bind:class="key!=0 || 'writing-word'" class="words m-2">{{ word }}</span>
      </div>
    </div>

    <div class="card">
      <div class="card-body bg-secondary">
        <div class="input-group">
          <input type="text" class="form-control">
          <div class="input-group-append" id="button-addon4">
            <button class="btn btn-light" disabled type="button">1:00</button>
            <button class="btn btn-light" type="button">Yenile</button>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
  export default{
    data(){
      return{
        words:["uçmak","elma","portakal","flappy","codeigniter"]
      }
    }
  }
</script>

<style>
  .words{
    font-size:25px;
    font-weight:400;
  }
  .writing-word{
    background-color:slategrey;
    color:white;
    padding:5px;
    border-radius:5px;
  }
</style>
