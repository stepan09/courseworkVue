<template>
<div class="container">
  <div class="row">
    <div class="col-md-8">
      <input type="text" v-model="search" class="form-control" placeholder="Пошук">
    </div>
    <div class="col-md-4">
      <button type="button" @click="showModal = true" class="btn btn-primary" data-toggle="modal" data-target="#exampleModalCenter">
        Додати
      </button>
    </div>
  </div>
  <b-card  v-for="competition in filteredList" :key="competition.competitionId" v-bind:data="competition" :title="competition.name"
           img-src="http://treningclub.by/wp-content/uploads/2015/01/business_start2.jpg"
           img-alt="Image"
           img-top
           tag="article"
           style="max-width: 20rem;"
           class="mb-2">
    <p class="card-text">
    <p>Старт : {{ competition.startDate}}</p>
    <p>Фініш : {{ competition.finishDate}}</p>
    <p>Організатор: {{competition.organizer.lastName}} {{competition.organizer.firstName}}</p>
    <p>Вид спорту: {{competition.sportKind.name}} </p>
    <b-form-select class="mb-2 mr-sm-2 mb-sm-0"
                   :value="null"
                   id="inlineFormCustomSelectPref" placeholder="Види спорту">
      <option disabled slot="first" :value="null">Спортзали</option>
      <option v-for="gym in competition.gyms">
        {{gym.name}}
      </option>
      <option disabled :value="null">Стадіони</option>
      <option v-for="stadium in competition.stadiums">
        {{stadium.name}}
      </option>
    </b-form-select>
    </p>
    <b-button variant="success" @click="showUpdateModals(competition)">Змінити</b-button>
    <b-button @click="deleteCompetition(competition.competitionId)" variant="danger">Видалити</b-button>
  </b-card>

  <!-- Modal -->
  <div v-if="showModal">
    <transition name="modal">
      <div class="modal-mask">
        <div class="modal-wrapper">
          <div class="modal-dialog">
            <div class="modal-content">
              <div class="modal-header">
                Додати
                <button type="button" class="close" @click="showModal=false">
                  <span aria-hidden="true">&times;</span>
                </button>

              </div>
              <div class="modal-body">

                <div class="form-group">
                  <label>Назва</label>
                  <input v-model="formAdd.name" type="text" class="form-control" placeholder="Введіть прізвище">
                </div>

                <div class="form-group">
                  <label>Старт</label>
                  <input v-model="formAdd.startDate" type="date" class="form-control">
                </div>

                <div class="form-group">
                  <label>Фініш</label>
                  <input v-model="formAdd.finishDate" type="date" class="form-control">
                </div>

                <div class="form-group">
                  <label class="my-1 mr-2">Організатори</label>
                  <select class="custom-select my-1 mr-sm-2" v-model="formAdd.organizer">
                    <option v-for="organizer in organizers" :value="organizer">
                      {{organizer.lastName}} {{organizer.firstName}}
                    </option>
                  </select>
                </div>

                <div class="form-group">
                  <label class="my-1 mr-2">Види спорту</label>
                  <select class="custom-select my-1 mr-sm-2" v-model="formAdd.sportKind">
                    <option v-for="sportKind in sportKinds" :value="sportKind">
                      {{sportKind.name}}
                    </option>
                  </select>
                </div>

                <div class="form-group">
                  <label class="my-1 mr-2">Стадіони</label>
                  <select class="custom-select my-1 mr-sm-2" v-model="formAdd.stadiums" multiple>
                    <option v-for="stadium in stadiums" :value="stadium">
                      {{stadium.name}}
                    </option>
                  </select>
                </div>

                <!--<div class="form-group">
                  <label class="my-1 mr-2">Тенісні корти</label>
                  <select class="custom-select my-1 mr-sm-2" v-model="formAdd.courts" multiple>
                    <option v-for="court in courts" :value="court">
                      {{court.name}}
                    </option>
                  </select>
                </div>-->

                <div class="form-group">
                  <label class="my-1 mr-2">Спортзали</label>
                  <select class="custom-select my-1 mr-sm-2" v-model="formAdd.gyms" multiple>
                    <option v-for="gym in gyms" :value="gym">
                      {{gym.name}}
                    </option>
                  </select>
                </div>

                <button class="btn btn-default" @click="showModal=false">Скасувати</button>
                <button class="btn btn-success" @click="addCompetition">Зберегти</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </transition>
  </div>

<div v-if="showUpdateModal">
    <transition name="modal">
      <div class="modal-mask">
        <div class="modal-wrapper">
          <div class="modal-dialog">
            <div class="modal-content">
              <div class="modal-header">
                Додати
                <button type="button" class="close" @click="showUpdateModal=false">
                  <span aria-hidden="true">&times;</span>
                </button>

              </div>
              <div class="modal-body">

                <div class="form-group">
                  <label>Назва</label>
                  <input v-model="formAdd.name" type="text" class="form-control" placeholder="Введіть прізвище">
                </div>

                <div class="form-group">
                  <label>Старт</label>
                  <input v-model="formAdd.startDate" type="date" class="form-control">
                </div>

                <div class="form-group">
                  <label>Фініш</label>
                  <input v-model="formAdd.finishDate" type="date" class="form-control">
                </div>

                <div class="form-group">
                  <label class="my-1 mr-2">Організатори</label>
                  <select class="custom-select my-1 mr-sm-2" v-model="formAdd.organizer">
                    <option v-for="organizer in organizers" :value="organizer">
                      {{organizer.lastName}} {{organizer.firstName}}
                    </option>
                  </select>
                </div>

                <div class="form-group">
                  <label class="my-1 mr-2">Види спорту</label>
                  <select class="custom-select my-1 mr-sm-2" v-model="formAdd.sportKind">
                    <option v-for="sportKind in sportKinds" :value="sportKind">
                      {{sportKind.name}}
                    </option>
                  </select>
                </div>

                <div class="form-group">
                  <label class="my-1 mr-2">Стадіони</label>
                  <select class="custom-select my-1 mr-sm-2" v-model="formAdd.stadiums" multiple>
                    <option v-for="stadium in stadiums" :value="stadium">
                      {{stadium.name}}
                    </option>
                  </select>
                </div>

                <!--<div class="form-group">
                  <label class="my-1 mr-2">Тенісні корти</label>
                  <select class="custom-select my-1 mr-sm-2" v-model="formAdd.courts" multiple>
                    <option v-for="court in courts" :value="court">
                      {{court.name}}
                    </option>
                  </select>
                </div>-->

                <div class="form-group">
                  <label class="my-1 mr-2">Спортзали</label>
                  <select class="custom-select my-1 mr-sm-2" v-model="formAdd.gyms" multiple>
                    <option v-for="gym in gyms" :value="gym">
                      {{gym.name}}
                    </option>
                  </select>
                </div>

                <button class="btn btn-default" @click="showUpdateModal=false">Скасувати</button>
                <button class="btn btn-success" @click="updateCompetition(formAdd)">Зберегти</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </transition>
  </div>

</div>
</template>

<script>
  import axios from 'axios'
    export default {
        name: "Competition",
      data: () => ({
        search: '',
        competitions: [],
        organizers: [],
        sportKinds: [],
        stadiums: [],
        courts: [],
        gyms: [],
        formAdd: {},
        showModal: false,
        showUpdateModal: false,
      }),
      created() {
          this.fetchCompetitions();
          this.fetchOrganizers();
          this.fetchSportKinds();
          this.fetchStadiums();
          this.fetchCourts();
          this.fetchGyms();
      },
      methods: {
        fetchCompetitions() {
          axios.get('http://localhost:8080/api/competitions').then((response) => {
            this.competitions = response.data;
          })
        },
        fetchOrganizers() {
          axios.get('http://localhost:8080/api/organizers').then((response) => {
            this.organizers = response.data;
          })
        },
        fetchSportKinds() {
          axios.get('http://localhost:8080/api/sport-kinds').then((response) => {
            this.sportKinds = response.data;
          })
        },
        fetchStadiums() {
          axios.get('http://localhost:8080/api/stadiums').then((response) => {
            this.stadiums = response.data;
          })
        },
        fetchCourts() {
          axios.get('http://localhost:8080/api/courts').then((response) => {
            this.courts = response.data;
          })
        },
        fetchGyms() {
          axios.get('http://localhost:8080/api/gyms').then((response) => {
            this.gyms = response.data;
          })
        },
        addCompetition() {
          this.showModal = false;
          axios.post('http://localhost:8080/api/competitions/', this.formAdd).then(() => {
            this.fetchCompetitions();
          });
          this.formAdd = {};
        },
        updateCompetition(competition) {
          this.showUpdateModal = false;
          axios.put('http://localhost:8080/api/competitions/' + competition.competitionId, competition).then(() => {
            this.fetchCompetitions();
          });
          this.formAdd = {};
        },
        deleteCompetition(id) {
          axios.delete('http://localhost:8080/api/competitions/' + id).then(() => {
            this.fetchCompetitions();
          })
        },
        showUpdateModals(coach) {
          this.showUpdateModal = true;
          this.formAdd = coach;
        }
      },
      computed: {
        filteredList() {
          return this.competitions.filter(competition => {
            return competition.name.toLowerCase().includes(this.search.toLowerCase())
          })
        }
      }
    }
</script>

<style scoped>
  .container {
    margin-top: 20px;
  }

  .card {
    display: inline-block;
    margin: 8px;
  }

  .modal-mask {
    position: absolute;
    z-index: 9998;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, .5);
    display: table;
    transition: opacity .3s ease;
  }

  .modal-wrapper {
    display: table-cell;
    vertical-align: middle;
  }
</style>
