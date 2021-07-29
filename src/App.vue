<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col form-inline">
        <b-form-input  v-model="newTask" placeholder="Enter new Task" @keyup.enter="add"></b-form-input> 
        <b-button class="ml-2" variant="primary" @click="add">Add</b-button>
        <b-button class="ml-2" variant="primary" @click="saveTable">Save Table as..</b-button>
        </div>
    </div>
    

    <div class="row mt-3">
      <div class="col-md-3">
        <div class="p-2 alert alert-danger">
          <h3>Backlog</h3>
          <draggable class="list-group board-column" :list="arrBacklog" group="tasks">
            <div class="list-group-item" v-for="element in arrBacklog" :key="element.name">
              {{element.name}}
            </div>
          </draggable>
        </div>
      </div> 

      <div class="col-md-3">
        <div class="p-2 alert alert-dark">
          <h3>In Progress</h3>
          <draggable class="list-group board-column" :list="arrInProgress" group="tasks">
            <div class="list-group-item" v-for="element in arrInProgress" :key="element.name">
              {{element.name}}
            </div>
          </draggable>
        </div>
      </div> 

      <div class="col-md-3">
        <div class="p-2 alert alert-info">
          <h3>Tested</h3>
          <draggable class="list-group board-column" :list="arrTested" group="tasks">
            <div class="list-group-item" v-for="element in arrTested" :key="element.name">
              {{element.name}}
            </div>
          </draggable>
        </div>
      </div> 

      <div class="col-md-3">
        <div class="p-2 alert alert-success">
          <h3>Done</h3>
          <draggable class="list-group board-column" :list="arrDone" group="tasks">
            <div class="list-group-item" v-for="element in arrDone" :key="element.name">
              {{element.name}}
            </div>
          </draggable>
        </div>
      </div> 
  </div>
  </div>
</template>

<script>
import draggable from "vuedraggable";

export default {
  name: 'App',
  components: {
    draggable
  },

  data(){
    return{
      newTask: "",
      arrBacklog: [
        {name: "Wake up"},
        {name: "Do the first step"},
        {name: "Complete half of the list"},
        {name: "Rest"}
      ],
      arrInProgress: [],
      arrTested:  [],
      arrDone:  []
    }
  },
  methods:{
    add(){
      if(this.newTask){
        this.arrBacklog.push({name: this.newTask});
        this.newTask= "";
      }
    },
    saveTable() {
            var a = "Backlog:" + JSON.stringify(this.arrBacklog) + "In Progress:" + JSON.stringify(this.arrInProgress) + "Tested:"  + JSON.stringify(this.arrTested) + "Done:" + JSON.stringify(this.arrDone);
            download(a);
        }
  }

}

function download(data, filename, type) {
    var file = new Blob([data], {type: type});
    if (window.navigator.msSaveOrOpenBlob) // IE10+
        window.navigator.msSaveOrOpenBlob(file, filename);
    else { // Others
        var a = document.createElement("a"),
                url = URL.createObjectURL(file);
        a.href = url;
        a.download = filename;
        document.body.appendChild(a);
        a.click();
        setTimeout(function() {
            document.body.removeChild(a);
            window.URL.revokeObjectURL(url);  
        }, 0); 
    }
}

</script>

<style>


  .board-column{
    min-height: 300px;
  }
</style>
