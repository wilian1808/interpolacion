<main class="main">
  <form class="form">
    <label class="form__label"> x1
      <input class="form__input" type="text" name="" id="" bind:value={ data.x1 }>
    </label>
    <label class="form__label"> y1
      <input class="form__input" type="text" name="" id="" bind:value={ data.y1 }>
    </label>
    <label class="form__label"> x2
      <input class="form__input" type="text" name="" id="" bind:value={ data.x2 }>
    </label>
    <label class="form__label"> y2
      <input class="form__input" type="text" name="" id="" bind:value={ data.y2 }>
    </label>
    <label class="form__label"> x
      <input class="form__input" type="text" name="" id="" bind:value={ data.x }>
    </label>
    <input class="form__input" type="submit" value="calcular" on:click|preventDefault={ interpolar }>
  </form>

  <canvas id="myChart"></canvas>
</main>

<style>
.form {
  display: block;
}

.form__input {
  display: block;
  font-size: 14px;
  padding: 1em .8em;
}

.form__label {
  background-color: #ffeeee;
  display: flex;
  width: 320px;
}
</style>

<script>
import Chart from 'chart.js';
import { onMount } from 'svelte';

// componenet functions

onMount(async () => {
  console.log('%c Home Cargado', 'color:orange');
})

// datas

let data = {x1: 1, y1: 6, x2: 5, y2: 2, x: 3};
let valoresX = [];
let valoresY = [];

// funciones

function interpolar() {
  console.table(data);
  renderGraf();
}

function asignarValoresX() {
  let final = (data.x1 > data.x2) ? data.x1 : data.x2;
  let inicio = (data.x1 < data.x2) ? data.x1 : data.x2;

  for (inicio; inicio <= final; inicio++) {
    valoresX.push(inicio);
  }
}

function asignarValoresY() {
  let final = (data.y1 > data.y2) ? data.y1 : data.y2;
  let inicio = (data.y1 < data.y2) ? data.y1 : data.y2;

  for (inicio; inicio <= final; inicio++) {
    valoresY.push(inicio);
  }

  console.log(valoresY);
}

function renderGraf() {
  let ctx = document.getElementById('myChart').getContext('2d');

  asignarValoresX();
  asignarValoresY();

  let chart = new Chart(ctx, {
    type: 'line',
    data: {
      labels: valoresX, // x
      datasets: [
        {
          label: 'interpolacion lineal',
          backgroundColor: 'transparent',
          borderColor: '#2a2a2a',
          data: valoresY.reverse() // [1, 2, 3, 4],
        }
      ]
    },
    options: {}
  })
}
</script>
