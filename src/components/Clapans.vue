<template>
  <div>
    <h1>{{count}}</h1> 
  <Navigation @turnOnAll="turnOnAllHere" />
  <div class="clapans">     
    <b-table class="myTable" data-filter-control="input" striped hover responsive id="myTable" sticky-header="650px" no-border-collapse head-row-variant="primary" ref="my-table" :filter="filter" :items="filtered" :fields="fields">
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
    name: 'Clapans',
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
    }},
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
            sortable: 'true',
             
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
            key: 'etazh',
            label: 'Этаж',
            sortable: 'true'
          },
          {
            key: 'status',
            label: 'Статус',
            sortable: 'true'
          },
          {
            key: 'actions',
            label: ''
          },
          {
            key: 'flat',
            label: 'Кв./Офис',
            sortable: 'true'
            
          },
          {
            key: 'id',
            label: 'ID Клапана',
            sortable: 'true'
          },
    
          {
            key: 'type',
            label: 'Физ/Юр. лицо',
            sortable: 'true'
          },
          {
            key: 'agreement_number',
            label: 'Номер Договора',
            sortable: 'true'
          },
          {
            key: 'snm',
            label: 'ФИО',
            sortable: 'true'
          },
          {
            key: 'iin',
            label: 'ИИН/БИН',
            sortable: 'true'
          },
          {
            key: 'payment',
            label: 'Способ оплаты',
            sortable: 'true'
          }
          

        ],
        items: [
           {isActive: true, shahta: "2",  type_house: "ЖК 'Есентай Сити'", city: "Алматы", raion: 'Медеуский', street: 'Кабанбай Батыр', house: '15', podezd: '2', etazh: '9', flat: '15',  id: '158626',  status: 'включен', type: 'Физическое лицо', agreement_number: "152051651", snm: "Иванов Иван Иванович", iin: "00525028156", bin: "168132138131", payment: "Карта"},
           {isActive: true, shahta: "2",  type_house: "ЖК 'Есентай Сити'", city: "Нурсултан", raion: 'Есиль', street: 'Кабанбай Батыр', house: '15',podezd: '2', etazh: '9', flat: '15',  id: '3526215',  status: 'отключен', type: 'Физическое лицо', agreement_number: "152051651", snm: "Иванов Иван Иванович", iin: "00525028156", bin: "168132138131", payment: "Карта"},
           {isActive: true, shahta: "2",  type_house: "ЖК 'Есентай Сити'", city: "Шымкент", raion: 'Абай', street: 'Кабанбай Батыр', house: '15', podezd: '2',etazh: '9', flat: '15',  id: '1616565',  status: 'включен', type: 'Физическое лицо', agreement_number: "152051651", snm: "Иванов Иван Иванович", iin: "00525028156", bin: "168132138131", payment: "Карта"},
           {isActive: true, shahta: "2",  type_house: "ЖК 'Есентай Сити'", city: "Караганда", raion: 'Есиль', street: 'Кабанбай Батыр', house: '15',podezd: '2', etazh: '9', flat: '15',  id: '651652',  status: 'включен', type: 'Физическое лицо', agreement_number: "152051651", snm: "Иванов Иван Иванович", iin: "00525028156", bin: "168132138131", payment: "Карта"},
           {isActive: true, shahta: "2",  type_house: "ЖК 'Есентай Сити'", city: "Семей", raion: 'Абай', street: 'Кабанбай Батыр', house: '15',podezd: '2', etazh: '9', flat: '15',  id: '65132156',  status: 'отключен', type: 'Физическое лицо', agreement_number: "152051651", snm: "Иванов Иван Иванович", iin: "00525028156", bin: "168132138131", payment: "Карта"},
           {isActive: true, shahta: "2",  type_house: "ЖК 'Есентай Сити'", city: "Уральск", raion: 'Райымбек', street: 'Кабанбай Батыр', house: '15',podezd: '2', etazh: '9', flat: '15',  id: '651621',  status: 'включен', type: 'Физическое лицо', agreement_number: "152051651", snm: "Иванов Иван Иванович", iin: "00525028156", bin: "168132138131", payment: "Карта"},
           {isActive: true, shahta: "2",  type_house: "ЖК 'Есентай Сити'", city: "Кокшетау", raion: 'Свобода', street: 'Кабанбай Батыр', house: '15', podezd: '2',etazh: '9', flat: '15',  id: '89456',  status: 'отключен', type: 'Физическое лицо', agreement_number: "152051651", snm: "Иванов Иван Иванович", iin: "00525028156", bin: "168132138131", payment: "Карта"},
           {isActive: true, shahta: "2",  type_house: "ЖК 'Есентай Сити'", city: "Талдыкорган", raion: 'Ленин', street: 'Кабанбай Батыр', house: '15', podezd: '2',etazh: '9', flat: '15',  id: '32151',  status: 'включен', type: 'Физическое лицо', agreement_number: "152051651", snm: "Иванов Иван Иванович", iin: "00525028156", bin: "168132138131", payment: "Карта"},
           {isActive: true, shahta: "2",  type_house: "ЖК 'Есентай Сити'", city: "Алматы", raion: 'Медеуский', street: 'Кабанбай Батыр', house: '15', podezd: '2',etazh: '9', flat: '15',  id: '"23423"',  status: 'включен', type: 'Физическое лицо', agreement_number: "152051651", snm: "Иванов Иван Иванович", iin: "00525028156", bin: "168132138131", payment: "Карта"},
           {isActive: true, shahta: "2",  type_house: "ЖК 'Есентай Сити'", city: "Тараз", raion: 'Кордай', street: 'Кабанбай Батыр', house: '15', podezd: '2',etazh: '9', flat: '15',  id: '65451',  status: 'включен', type: 'Физическое лицо', agreement_number: "152051651", snm: "Иванов Иван Иванович", iin: "00525028156", bin: "168132138131", payment: "Карта"},
           {isActive: true, shahta: "2",  type_house: "ЖК 'Есентай Сити'", city: "Тараз", raion: 'Чу', street: 'Кабанбай Батыр', house: '15', etazh: '9', flat: '15',  id: '25245',  status: 'включен', type: 'Физическое лицо', agreement_number: "152051651", snm: "Иванов Иван Иванович", iin: "00525028156", bin: "168132138131", payment: "Карта"}
          
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
      this.$set(item, "id", Math.floor(Math.random()*1000000))
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
}

  
  }

  }
</script>

<style lang="scss" scoped>
  
    $body-bg: #000;

    $body-color: rgb(105, 62, 62);

  @import "~bootstrap/scss/bootstrap.scss";
  @import '~bootstrap-vue/dist/bootstrap-vue.css';
    
    $red: #8f3333;
  .input_in_table{
    border-radius: 5px;
    width: 3.7rem;
    font-size: 12px;
    border-width: thin;
    overflow: hidden;
  }
  .clapans{
    margin-top: 1rem;
  }
  
 
  .card{
    margin-right: 1rem;
    border: none;
    width: 60%;
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