<template>
<div>
  <Navigation/>
  <div class="master_hub">
    <b-table class="myTable" striped hover responsive id="myTable" sticky-header="800px" no-border-collapse head-row-variant="primary" ref="my-table" :filter="filter" :items="filtered" :fields="fields">
      <template slot="top-row" slot-scope="{ fields }">
      <td v-for="field in fields" :key="field.key">
        <input class="input_in_table"  v-model="filters[field.key]" :placeholder="'Поиск'">
      </td>
    </template>
      <template class="info" v-slot:cell(actions)="row">
            <b-button size="sm" @click="toggleRowDetails(row, 'status')">
              {{ row.detailsShowing ? 'Скрыть' : 'Редактировать'}} 
            </b-button>
          </template>

          <template v-slot:row-details="row">
            <b-card class="information">
              <div>
              <b-row
                v-for="(detail, i) in detailsMask"
                :key="`detail-${i}`"
                class="mb-2" 
              >
              Статус: {{ row.item[detail] }} 
              
              </b-row>
              
              <b-row>
              <b-button  size="sm-3"  class="onButton"  @click="rowOn(row.item); addHistory()" >Включить</b-button>
              <b-button  size="sm-3"  class="offButton"  @click="rowOff(row.item); addHistory()" >Отключить</b-button>
              <b-button  size="sm-3" class="mr-2" @click="changeId(row.item); addHistory()" >Поменять</b-button>
              <b-button  size="sm-3" class="mr-2" @click="downloadCSVData" >Загрузить Историю</b-button>
              </b-row>
              <b-row>
                <div class="comment">

              <b-form-textarea
                    id="textarea"
                
                    placeholder="Комментировать..."
                    size="sm-3"
                    rows="2"
                    max-rows="4"
                  ></b-form-textarea>
                  <b-button class="addComment" size="sm">Добавить комментарий</b-button>
                </div>
              </b-row>
               <b-row>
                 <h3>История:</h3>
                 <br>
                <p v-for="h in history" :key="h">{{h}}, <br></p>
              </b-row>
              </div>
            </b-card>
          </template>
    </b-table>
  </div>
</div>
</template>

<script>
import Navigation from '@/components/Navigation.vue'

  export default {
    name: 'MasterHub',
    components: {Navigation},
    computed: {
    filtered () {
      const filtered = this.items.filter(item => {
        return Object.keys(this.filters).every(key =>
            String(item[key]).includes(this.filters[key]))
      })
      return filtered.length > 0 ? filtered : [{
        id: '',
        issuedBy: '',
        issuedTo: ''
      }]
    }
  },
    data() {
      return {  
        filters: {
          id: '',
          issuedBy: '',
          issuedTo: ''
        },
        filter: null,
        history:[],
        count: "",
        detailsMask: [],
        fields: [
          {
            key: 'city',
            label: 'Город',
            sortable: 'true'
          },
          {
            key: 'raion',
            label: 'Район',
            sortable: 'true'
          },
          {
            key: 'street',
            label: 'Улица',
            sortable: 'true'
          },
          {
            key: 'type_house',
            label: 'Наименование строения',
            sortable: 'true'
          },
          {
            key: 'house',
            label: 'Дом',
            sortable: 'true'
          },
          
          {
            key: 'podezd',
            label: 'Подъезд',
            sortable: 'true'
          },
          {
            key: 'shahta',
            label: 'Шахта',
            sortable: 'true'
          },
          {
            key: 'id_master_hub',
            label: 'ID МастерХаба',
            sortable: 'true'
          },
          {
            key: 'status',
            label: 'Статус',
            sortable: 'true'
          },
          {
            key: 'actions',
            label: '',
          },

        ],
        items: [
          { isActive: true, type_house: "ЖК 'Есентай Сити'", city: "Алматы", raion: 'Есиль', street: 'Кабанбай Батыр', house: '15', podezd: "2", shahta: "1", id_master_hub: '1521544',  status: 'включен',},
          { isActive: true, type_house: "ЖК 'Есентай Сити'", city: "Алматы", raion: 'Есиль', street: 'Кабанбай Батыр', house: '15', podezd: "2", shahta: "1", id_master_hub: '1521544',  status: 'включен',},
          { isActive: true, type_house: "ЖК 'Есентай Сити'", city: "Алматы", raion: 'Есиль', street: 'Кабанбай Батыр', house: '15', podezd: "2", shahta: "1", id_master_hub: '1521544',  status: 'включен',},
          { isActive: true, type_house: "ЖК 'Есентай Сити'", city: "Алматы", raion: 'Есиль', street: 'Кабанбай Батыр', house: '15', podezd: "2", shahta: "1", id_master_hub: '1521544',  status: 'включен',},
          { isActive: true, type_house: "ЖК 'Есентай Сити'", city: "Алматы", raion: 'Есиль', street: 'Кабанбай Батыр', house: '15', podezd: "2", shahta: "1", id_master_hub: '1521544',  status: 'включен',},
          { isActive: true, type_house: "ЖК 'Есентай Сити'", city: "Алматы", raion: 'Есиль', street: 'Кабанбай Батыр', house: '15', podezd: "2", shahta: "1", id_master_hub: '1521544',  status: 'включен',},
          { isActive: true, type_house: "ЖК 'Есентай Сити'", city: "Алматы", raion: 'Есиль', street: 'Кабанбай Батыр', house: '15', podezd: "2", shahta: "1", id_master_hub: '1521544',  status: 'включен',},
          { isActive: true, type_house: "ЖК 'Есентай Сити'", city: "Алматы", raion: 'Есиль', street: 'Кабанбай Батыр', house: '15', podezd: "2", shahta: "1", id_master_hub: '1521544',  status: 'включен'}
      
        ]
  }
  },
  methods: {
    toggleRowDetails(row, data) {
      this.detailsMask = data.split(', ')
      row.toggleDetails()
    },
    rowOn(item) {
      this.$set(item, "status", "включен")
    },
    rowOff(item) {
      this.$set(item, "status", "отключен")
    },
    changeId(item){
      this.$set(item, "id_master_hub", Math.floor(Math.random()*1000000))
    },
    turnOnAllHere(data){
      this.count = data;
    }, 
    
    addHistory(){
      this.history.push("Hello")
    },
    downloadCSVData() {
    let csv = 'Put,Column,Titles,Here\n';
    this.history.forEach((row) => {
            csv += row.join(',');
            csv += "\n";
    });
 
    const anchor = document.createElement('a');
    anchor.href = 'data:text/csv;charset=utf-8,' + encodeURIComponent(csv);
    anchor.target = '_blank';
    anchor.download = 'история.csv';
    anchor.click();
}}
  }
</script>

<style scoped>
.input_in_table{
    border-radius: 5px;
    width: 3.7rem;
    font-size: 12px;
    border-width: thin;
    overflow: hidden;
  }
  
  .master_hub{
    margin-top: 1rem;
  }
  .card{
    margin-right: 1rem;
    border: none;
  }

  .onButton{
    margin-right: 1rem;
    background-color: #629e6c;
  }
  .offButton{
    margin-right: 1rem;
    background-color: #f57575;
  }
  
  .comment{
    display: flex;
    margin-top: 2rem;
  }
  .addComment{
    margin-left: 1rem;
  }
  .information > div{
    margin-left: 1rem;
  }

  p{
    margin-top: 1.5rem;
  }
  h3{
    margin-top: 1rem;
  }
  
  
  .myTable{
    border-radius: 1rem;
    border-style: solid;
    border-width: thin;
    box-shadow: 0px 2px 18px 0px rgba(44, 45, 46, 0.5);
    display: block;
  }
</style>