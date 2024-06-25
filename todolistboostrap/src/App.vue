<template>

  <head>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet"
      integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
  </head>
  <header class=" bg-dark-subtle"> 
    <div v-if="!selectedList">
      <div class=" p-2 bg-dark-subtle">
        <div class="d-flex justify-content-between">
          <h1>{{ titolo }}</h1>
          <button class="btn btn-primary" @click="popup()">+</button>
        </div>
      </div>

      <div v-if="showModale" class="modal d-block " tabindex="-1">
        <div role="document" class="modal-dialog-centered ">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title">Inserisci il nome della lista</h5>
              <button type="button" class="btn-close btn" @click="chiudipopup()"></button>
            </div>
            <div class="modal-body">
              <input type="text" v-model="nomeLista" required id="testo" class="form-control" />
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-primary" @click="inserisciLista()" :disabled="!nomeLista">
                Inserisci lista
              </button>
            </div>
          </div>
        </div>
      </div>


      <div v-if="liste.length" v-for="(lista, i) in liste" class="md-flex">

        <div class="d-flex p-2 bg-dark-subtle justify-content-between fs-4 text">
          <p @click="selectedList = lista"> {{ lista.nomeLista }} <span class="fw-bold">({{ lista.contenutoLista.length
            > 1 ? 'ci sono '
            + lista.contenutoLista.length + ' elementi' : 'c\'è ' + lista.contenutoLista.length + ' elemento' }} nella
              lista) </span></p>
          <button @click="rimuoviLista(i)" class="btn btn-danger "> x </button>
        </div>


      </div>
    </div>

    <div v-else>

      <div v-if="showModale2" class="modal d-block " tabindex="-1">
        <div role="document" class="modal-dialog-centered ">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title">Inserisci il nome della task</h5>
              <button type="button" class="btn-close btn" @click="ChiudiPopupTask()"></button>
            </div>
            <div class="modal-body">
              <input type="text" v-model="nomeTask" required id="testo" class="form-control" />
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-primary" @click="inserisciTask()" :disabled="!nomeTask">
                Inserisci task
              </button>
            </div>
          </div>
        </div>
      </div>
      <div class="p-2 bg-dark-subtle ">
        <div class="d-flex justify-content-between d-flex ">
          <h1>{{ selectedList.nomeLista }}</h1>
          <button class="btn btn-primary" @click="PopupTask()">+</button>
        </div>
       <br>
          <div class="d-flex justify-content-between fs-4 text" v-for="(task, index) in selectedList.contenutoLista" :key="index">
            <p>{{ task.nomeTask }} </p>
            <button @click="rimuoviTask(index)" class="btn btn-danger">x</button>
          </div>
       
      </div>
      <div>
        <button @click="selectedList = null" class="btn btn-danger">Torna all' elenco delle liste</button>
      </div>
    </div>
    <div class="p-2 bg-dark-subtle " ></div>
  </header>
</template>

<script>
export default {
  data() {
    return {
      titolo: "Benvenuto, crea la tua lista",
      nomeLista: '',
      nomeTask: '',
      showModale: false,
      showModale2: false,
      liste: [],
      selectedList: null,
      NumTask: 0,
    };
  },
  methods: {
    popup() {
      this.nomeLista = '';
      this.showModale = true;
    },
    inserisciLista() {
      this.liste.push({ nomeLista: this.nomeLista, contenutoLista: [] });
      this.showModale = false;
      this.nomeLista = '';
      localStorage.setItem('liste', JSON.stringify(this.liste));
    },
    rimuoviLista(index) {
      this.liste.splice(index, 1);
      localStorage.setItem('liste', JSON.stringify(this.liste));
    },
    chiudipopup() {
      this.showModale = false;
    },
    PopupTask() {
      this.nomeTask = '';
      this.showModale2 = true;
    },
    ChiudiPopupTask() {
      this.showModale2 = false;
    },
    inserisciTask() {
      if (this.selectedList) {
        this.selectedList.contenutoLista.push({ nomeTask: this.nomeTask });
        this.showModale2 = false;
        this.nomeTask = '';
        localStorage.setItem('liste', JSON.stringify(this.liste));
      }
    },
    rimuoviTask(index) {
      this.selectedList.contenutoLista.splice(index, 1);
      localStorage.setItem('liste', JSON.stringify(this.liste));
    }
  },
  mounted() {
    if (localStorage.getItem('liste') != null) {
      this.liste = JSON.parse(localStorage.getItem('liste'));
    }
  }
};
</script>

<style scoped>
header{
  height: 100vh;
}
.modal {
  background: rgba(0, 0, 0, 0.5);
}
</style>
