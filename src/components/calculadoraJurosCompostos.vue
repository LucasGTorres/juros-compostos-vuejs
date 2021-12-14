<template>
  <div class="d-flex justify-content-center">
    <form class="border m-2 p-2" style="width: 50vw">
      <div class="row">
        <div class="col">
          <label for="exampleInputEmail1">Aporte Inicial</label>
          <div class="input-group flex-nowrap col">
            <span class="input-group-text">R$</span>
            <input
              type="text"
              class="form-control shadow-none"
              v-money="money"
              v-model="data.valorInicial"
            />
          </div>
        </div>

        <div class="col">
          <label for="exampleInputEmail1">Aporte Mensal</label>
          <div class="input-group flex-nowrap col">
            <span class="input-group-text">R$</span>
            <input
              type="text"
              class="form-control shadow-none"
              v-money="money"
              v-model="data.aporteMensal"
            />
          </div>
        </div>
      </div>

      <div class="row mt-2">
        <div class="col">
          <label for="exampleInputEmail1">Taxa de Juros</label>
          <div class="input-group flex-nowrap col">
            <span class="input-group-text">%</span>
            <input
              type="text"
              class="form-control shadow-none"
              v-money="money"
              v-model="data.juros"
            />
          </div>
        </div>

        <div class="col">
          <label for="exampleInputEmail1">Periodo (Meses)</label>
          <div class="input-group flex-nowrap col">
            <input
              type="number"
              class="form-control shadow-none"
              v-model="data.periodo"
            />
          </div>
        </div>
      </div>

      <div class="row mt-2">
        <div class="col" id="limpar">
          <button type="button" class="btn btn-secondary shadow-none form-control">
            Limpar
          </button>
        </div>

        <div class="col">
          <button type="button" @click="postData()" class="btn btn-primary shadow-none form-control">
            Calcular
          </button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import { VMoney } from "v-money";
import axios from 'axios';

export default {
  data() {
    return {
      price: 123.45,
      money: {
        decimal: ",",
        thousands: ".",
        precision: 2,
        masked: false /* doesn't work with directive */,
      },
      data: {"valorInicial": 0, "aporteMensal": 0, "juros": 0, "periodo": 0},
    };
  },
  methods: {
      postData(){

          this.data.valorInicial = this.data.valorInicial.replaceAll(".", "").replaceAll(",", ".")
          this.data.aporteMensal = this.data.aporteMensal.replaceAll(".", "").replaceAll(",", ".")
          this.data.juros = this.data.juros.replaceAll(".", "").replaceAll(",", ".")
          
          axios.post("https://api-juros-compostos.herokuapp.com/api/juros-compostos", this.data).then((resultado) => {
              alert("Aporte: " + resultado.data.montanteSemJuros + "Juros: " + resultado.data.jurosTotal + "Total: " + resultado.data.montanteComJuros )
          })
      }
  },

  directives: { money: VMoney },
};
</script>