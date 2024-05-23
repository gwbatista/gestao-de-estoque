<template>
  <div>
    <h3 class="titulo">Painel</h3>
    <hr>
    
    <v-container>
      <v-row>
        <v-col cols="16" md="8">
          <div class="periodo-datas">
            <p class="title-data">Selecione o período de datas:</p>
            <div class="campo-data">
              <label for="start-date" class="label-text">Início:</label>
              <input type="date" id="start-date" v-model="startDate" class="input-date">
              <label for="end-date" class="label-text">Término:</label>
              <input type="date" id="end-date" v-model="endDate" class="input-date">
              <v-btn class="button-buscar" color="primary" @click="searchDatabase">Buscar</v-btn>
            </div>
          </div>
        </v-col>
        <v-col cols="12" md="4" class="mt-4">
          <v-radio-group class="font-weight-bold" label="Tipo de Gráfico" inline v-model="selectedChartType">
            <v-radio label="Pizza" value="pie"></v-radio>
            <v-radio label="Barra" value="bar"></v-radio>
          </v-radio-group>
        </v-col>
      </v-row>
    </v-container>

    <div class="graficos">
      <div class="grafico-1">
        <p class="texto-grafico-1">GESTÃO DE ESTOQUE</p>
        <PieChartWorklist :selectedChartType="selectedChartType" />
      </div>
      
    </div>
  </div>
</template>

<script>
  import axios from 'axios';

  import PieChartWorklist from '@/components/PieChartWorklist.vue';

  import { data as worklistData } from '@/util/chartConfigWorklist';

  export default {
    components: {
      PieChartWorklist
    },
    data() {
      return {
        selectedChartType: 'pie', // Valor padrão é 'pizza'
        selectedDateRange: null,
        menu: false,
        startDate: null,
        endDate: null,
        worklistData: worklistData,
      };
    },
    methods: {
      async searchDatabase() {
        try {

          if (!this.startDate || !this.endDate) {
            alert('Por favor, selecione um período de datas válido.');
            return;
          }

          // Formatar as datas para o formato yyyyMMdd
          const formattedStartDate = this.startDate.split('-').join('');
          const formattedEndDate = this.endDate.split('-').join('');

          const response = await axios.post('http://localhost:4000/data/get-data-dashboard', {
            startDate: formattedStartDate,
            endDate: formattedEndDate,
          });

          console.log(`StartDate: ${formattedStartDate} EndDate: ${formattedEndDate}`);
          console.log('Resposta do backend:', response.data);

          // Atualiza os dados dos gráficos com os dados recebidos do backend
          this.data.datasets[0].data = [
            parseInt(response.data.inStockCounts),
            parseInt(response.data.outOfStockCounts),
            parseInt(response.data.reservedCounts)
          ];

          this.renderChart();

          console.log('Quantidade de produtos em estoque:', this.inStockCount);
          console.log('Quantidade de produtos sem estoque:', this.outOfStockCount);
          console.log('Quantidade de produtos reservados:', this.reservedCount);

        } catch (error) {
          console.error('Erro ao fazer a requisição: ', error);
        }
      }
    },
    computed: {
      fromDateDisp() {
        return this.selectedDateRange;
        // format/do something with date
      },
    },
  };
</script>

<style scoped>
  .titulo {
    margin-bottom: 5px;
  }

  .button-buscar {
    margin-left: 50px;
    font-size: 12px;
  }

  .title-data {
    font-weight: bold;
    font-size: 15px;
    margin-bottom: 20px;
  }

  .periodo-datas {
    display: flex;
    flex-direction: column;
    align-items: center; /* Centralizar o conteúdo */
    padding: 10px; /* Adicionar espaço interno */
  }

  .campo-data {
    display: flex;
    align-items: center; /* Centralizar os itens */
    flex-wrap: wrap;
    justify-content: center;
  }

  .campo-data label {
    margin-right: 10px;
    margin-left: 20px;
  }

  .input-date {
    border: 1px solid #ccc; 
    border-radius: 5px; 
    padding: 5px; 
    font-size: 12px;
  }
  .label-text {
    font-size: 12px;
    font-weight: bold;
  }
  .graficos {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
  }

  .grafico-1 {
    margin-top: 45px;
    display: flex;
    flex-direction: column;
    gap: 35px; 
  }
  .texto-grafico-1 {
    margin-left: 70px;
    font-weight: bold;
  }

</style>
