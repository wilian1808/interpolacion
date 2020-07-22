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

// funcion que llama al evento submit del formulario
function interpolar() {
  console.table(data);
  renderGraf();
  addValuesX();
  addValuesDefault();
  getSalto();
  completeValues();
  valuesDescendente();
  deleteZero();
}

// datas

let data = {x1: 1, y1: 4, x2: 5, y2: 2, x: 3};
let valoresX = [];
let valoresY = [];
let salto = 0;

// 1RO AGREGAMOS TODOS LOS VALORES PARA EL EJE X
function addValuesX() {
  let indice = 0;
  for (let i = 0; i <= parseFloat(data.x2); i++) {
    indice++;
    valoresX.push(i);
    valoresY.push(0);
  }

  valoresX.push(indice);
  console.log(`X: ${valoresX}`);
  console.log(`Y: ${valoresY}`);
}

// SEGUNDO ASIGNAMOS VALORES DE LAS COORDENADAS AL ARRAY Y
function addValuesDefault() {
  for (let i = 0; i < valoresY.length; i++) {
    if (i == parseFloat(data.x1)) {
      valoresY[i] = parseFloat(data.y1);
    }
    if (i == parseFloat(data.x2)) {
      valoresY[i] = parseFloat(data.y2);
    }
  }

  console.log(`Nuevo array: ${valoresY}`);
}

// TERCERO NECESITAMOS EL VALOR DEL SALTO QUE TOMAREMOS PARA ASIGNAR LOS VALORES DE Y
function getSalto() {
  let y = Math.abs(parseFloat(data.y1) - parseFloat(data.y2));
  let x = Math.abs(parseFloat(data.x1) - parseFloat(data.x2));
  let res = Math.abs(y / x);
  salto = res;

  console.log(`El salto es de: ${salto}`);
}

// CUARTO TERMINAMOS DE ASIGNAR LOS VALORES DE LOS DEMAS PUESTOS DEL ARRAY Y
// antes de eso tenemos que determinar el orden de los valores si van en forma ascendente o descendente
function completeValues() {
  if (parseFloat(data.y1) > parseFloat(data.y2)) {
    console.log('%c Es de orden descendente', 'color:orange');
  }
  if (parseFloat(data.y1) < parseFloat(data.y2)) {
    console.log('%c Es de orden ascendente', 'color:orange');
  }
}

// DESCENDENTE
function valuesDescendente() {
  let resta = parseFloat(data.y1);
  for (let i = 0; i < valoresY.length; i++) {
    if (i <= parseFloat(data.y1) && i >= parseFloat(data.y2)) {
      valoresY[i] = resta - salto;
      resta = resta - salto;
      console.log(i + ' ' + valoresY[i]);
    }
  }

  console.log(`Valores totales: ${valoresY}`);
}

// ULTIMO QUITAMOS TODOS LOS VALORES MENORES QUE EL 1X
function deleteZero() {
  for (let i = 0; i < parseFloat(data.x1); i++) {
    valoresY[i] = null;
  }
}

// funcion que renderiza el grafico
function renderGraf() {
  let ctx = document.getElementById('myChart').getContext('2d');

  let chart = new Chart(ctx, {
    type: 'line',
    data: {
      labels: valoresX, // x
      datasets: [
        {
          label: 'interpolacion lineal',
          backgroundColor: 'transparent',
          borderColor: '#2a2a2a',
          data: valoresY.reverse(), // [1, 2, 3, 4],
          pointRadius: 5,
          pointBackgroundColor: 'red',
          lineTension: 0
        }
      ]
    },
    options: {
      responsive: true,
      scales: {
        yAxes: [{
          ticks: {
            suggestedMin: 0,
            suggestedMax: ((parseFloat(data.y2) > parseFloat(data.y1)) ? parseFloat(data.y2) : parseFloat(data.y1)) + 1
          }
        }],
      }
    }
  })

}
</script>