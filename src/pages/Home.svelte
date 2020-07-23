<main class="main">
  <div class="main__content">

    <form class="form">

      <div class="form__row">
        <label class="label__one">
          <span>X1</span>
          <input class="form__input" type="text" name="" id="" bind:value={ data.x1 }>
        </label>
        <label class="label__one">
          <span>Y1</span>
          <input class="form__input" type="text" name="" id="" bind:value={ data.y1 }>
        </label>
      </div>

      <div class="form__row">
        <label class="label__one">
          <span>X2</span>
          <input class="form__input" type="text" name="" id="" bind:value={ data.x2 }>
        </label>  
        <label class="label__one">
          <span>Y2</span>
          <input class="form__input" type="text" name="" id="" bind:value={ data.y2 }>
        </label>
      </div>

      <div class="form__large">
        <span>X</span>
        <input class="form__input" type="text" name="" id="" bind:value={ data.x }>
      </div>
      
      <input class="form__input" type="submit" value="calcular" on:click|preventDefault={ interpolar }>
      
      <div class="form__large">
        <span>Y</span>
        <input class="form__input" type="text" name="" id="" bind:value={ data.y }>
      </div>


      <div class="description">
        La interpolacion lineal es en el punto: <label>({ data.x }, { data.y })</label>
      </div>
    </form>
    
    <div class="grafico">
      <canvas class="grafico__canva" id="myChart"></canvas>
    </div>
  </div>
</main>

<style>
.main {
  display: block;
  max-width: 1024px;
  width: 92%;
  margin: 0 auto 0;
}

.main__content {
  display: grid;
  grid-template-columns: 300px 1fr;
  grid-gap: 2em;
  align-items: flex-start;
}

.form {
  display: block;
  padding-top: 2.2em;
  width: 100%;
  max-width: 300px;
}

.form__row {
  width: 100%;
  display: grid;
  grid-template-columns: 1fr 1fr;
  width: 100%;
  grid-gap: 1em;
  margin-bottom: .5em;
}

.label__one {
  background-color: transparent;
  display: grid;
  grid-template-columns: 40px 1fr;
  border: solid 1px rgba(0, 0, 0, .4);
}

.form span {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 14px;
  color: #2a2a2a;
  border-right: 1px solid rgba(0, 0, 0, .4);
}

.form__input {
  display: block;
  width: 100%;
  padding: .6em 1em;
  font-size: 14px;
  border: none;
}

.form__large {
  background-color: transparent;
  display: grid;
  width: 100%;
  grid-template-columns: 40px 1fr;
  border: solid 1px rgba(0, 0, 0, .4);
  margin-bottom: .5em;
}

.form__input[type="submit"] {
  display: block;
  width: 100%;
  cursor: pointer;
  margin-bottom: .5em;
  background-color: #3f4441;
  border: none;
  color: #ffffff;
}

.grafico {
  display: block;
  width: 100%;
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
  valoresY = [];
  valoresX = [];
  console.table(data);
  renderGraf();
}

// datas

let data = {x1: 1, y1: 6, x2: 5, y2: 2, x: 3, y: 0};
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

  // for (let i = 0; i < 3; i++) {
  //   //indice++;
  //   if (i == 0) {
  //     valoresX.push(parseFloat(data.x1));
  //   } else if (i == 1) {
  //     valoresX.push(parseFloat(data.x));
  //   } else {
  //     valoresX.push(parseFloat(data.x2));
  //   }
  //   valoresY.push(0);
  // }

  valoresX.push(indice);
  console.log(`X: ${valoresX}`);
  console.log(`Y: ${valoresY}`);
}

// FUNCION QUE BUSCA LA INTERPOLACION
function interpolarData() {
  let suma = parseFloat(data.y1);
  let division = (parseFloat(data.y2) - parseFloat(data.y1)) / (parseFloat(data.x2) - parseFloat(data.x1));
  let multiplicacion = parseFloat(data.x) - parseFloat(data.x1);

  let res = Math.abs(suma + (division * multiplicacion));
  let resTwo = res.toFixed(1); 
  data.y = parseFloat(resTwo);
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
     if (i == parseFloat(data.x)) {
       valoresY[i] = parseFloat(data.y);
     }
   }

  // for (let i = 0; i < 3; i++) {
  //   if (i == 0) {
  //     valoresY[i] = parseFloat(data.y1);
  //   } else if (i == 1) {
  //     valoresY[i] = parseFloat(data.y);
  //   } else {
  //     valoresY[i] = parseFloat(data.y2);
  //   }
  // }

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
     valuesDescendente();
   }
   if (parseFloat(data.y1) < parseFloat(data.y2)) {
     console.log('%c Es de orden ascendente', 'color:orange');
     valuesAscendente();
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

//ASCENDENTE
function valuesAscendente() {
  let resta = parseFloat(data.y1);

  for (let i = 0; i < valoresY.length; i++) {
    if (i <= parseFloat(data.y2) && i >= parseFloat(data.y1)) {
      valoresY[i] = resta + salto;
      resta = resta + salto;
      console.log(`[${i}] => ${valoresY[i]}`);
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
  interpolarData();
  addValuesX();
  addValuesDefault();
  getSalto();
  completeValues();
  deleteZero();

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
          data: valoresY, // [1, 2, 3, 4],
          pointRadius: 6,
          pointBackgroundColor: '#2a2a2a',
          pointHoverRadius: 6,
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
            suggestedMax: ((parseFloat(data.y2) > parseFloat(data.y1)) ? parseFloat(data.y2) : parseFloat(data.y1)) + 1,
            // stepSize: salto
          }
        }],
        xAxes: [{
          ticks: {
            suggestedMin: 0,
            suggestedMax: parseFloat(data.x2) + 1,
            stepSize: 1
          }
        }]
      }
    }
  })

}
</script>