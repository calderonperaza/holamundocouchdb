<template>
  <v-container>
    <v-row>
      <v-col md6>
        <h2>Lista de tareas</h2>
        <div v-for="(item, key) in listatareas" :key="key">
        <v-card color="grey">
          <v-card-text><h2>{{item.doc.nombre}}</h2>
          {{item.doc.descripcion}}        
          </v-card-text>
          <v-card-actions>
            <v-btn color="accent" small @click.stop="modificarShow=true; tareaSelectedIndex=key;">Modificar</v-btn>
            <v-btn color="error" small @click="eliminarTarea(key)">Eliminar</v-btn>
          </v-card-actions>
        </v-card><br>
        </div>
        
      </v-col>
      <v-col md6>
        <h2>Acciones</h2>
        <v-card pa-3> 
        <v-card-text>
          <h2 dark>Agregar Nuevo</h2>
          <v-text-field
            v-model="newtarea.nombre"
            label="Nombre"
            required            
          ></v-text-field>
          <v-textarea rows="3"
            v-model="newtarea.descripcion"
            label="Descripcion"
                        
          ></v-textarea>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" @click="agregarTarea">Agregar</v-btn>
          <v-btn color="secondary" @click="newtarea.nombre=''; newtarea.descripcion=''">Cancelar</v-btn>
        </v-card-actions>
        </v-card>
        
      </v-col>
    </v-row>
    <v-dialog v-model="modificarShow" justify-center max-width="400" >
      <v-card color="blue lighten-5">
        <v-card-title>Modificar Tarea</v-card-title>
        <v-card-text>
          <div v-if="tareaSelectedIndex>-1">
          <v-text-field
            v-model="listatareas[tareaSelectedIndex].doc.nombre"
            label="Nombre"
            required            
          ></v-text-field>
          <v-textarea rows=2
            v-model="listatareas[tareaSelectedIndex].doc.descripcion"
            label="Descripcion"                        
          ></v-textarea>
          </div>          
        </v-card-text>
        <v-card-actions> <v-spacer></v-spacer>
        <v-btn @click="modificarShow=false; modificarTarea();"
          color="success darken-2">Guardar</v-btn>
        <v-btn @click="modificarShow=false" color="success darken-2">Cerrar</v-btn>   
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script lang="ts">
  import Vue from 'vue'
  import axios from 'axios'

  export default Vue.extend({
    name: 'HelloWorld',
    
    data: () => ({
      listatareas: [],
      modificarShow:false,
      tareaSelectedIndex:-1,
      newtarea:{
        nombre:"",
        descripcion:""
      }      
    }),
    methods:{
      cargardatos(){
        axios.get("http://127.0.0.1:5984/tareas/_all_docs?include_docs=true").then((result) => {
        //console.log(result.data);
        this.listatareas=result.data.rows;
        this.tareaSelectedIndex=-1;
    })
      },
      agregarTarea(){
        axios.post("http://127.0.0.1:5984/tareas", this.newtarea).then((result) => {
        this.cargardatos();
        this.newtarea.nombre="";
        this.newtarea.descripcion="";
        });
      },
      eliminarTarea(id: string | number){
        console.log(this.listatareas[id].doc._rev);
        
        axios.delete("http://127.0.0.1:5984/tareas/"+ this.listatareas[id].id + 
        "?rev=" + this.listatareas[id].doc._rev
        ).then(
          ()=>{
            this.cargardatos();
          }
        );
        
      },
      modificarTarea(){
        if(this.tareaSelectedIndex>0){
          let Ltarea=this.listatareas[this.tareaSelectedIndex];
          axios.put("http://127.0.0.1:5984/tareas/"+ Ltarea.id, Ltarea.doc
          ).then(()=>{
            this.cargardatos();
          }
        );

        }
      },
    },
    created(){
      this.cargardatos();
    }    
  })
</script>
