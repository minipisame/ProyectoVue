<template>
				<div>
        <div id="encabezado">
		<h1>Proyecto Vue.js - David Medina Vega</h1>
		</div>
		<div id="inpu">
    <input v-model="message" v-on:keyup.enter="add" placeholder="¿Que quieres recordar?">

    </div>


    <div class="container">
		<div class="row">
		<div class="col-sm-4">{{completadas}} Tareas pendientes de un total de {{this.todos.length}}</div>
  <div class="col-sm-3"> <a href="#" v-on:click="borrarT" >X Borrar tareas completadas</a></div>
</div>
 
</div>
		 <transition-group name="notas" tag="p">
		<div class="note"  v-for="(todo) in array_order" v-bind:key="todo" > 
		
		<div>

				<img v-if="todo.completada" id="tick" v-on:click="cambiarE(todo)" src="../assets/tick.png"></img>
				<img v-else  id="circulo" v-on:click="cambiarE(todo)" src="../assets/circulo.gif"></img>
				<h1 v-bind:class="{ titulo: todo.completada }">{{todo.Tarea}}</h1>
			 
</div>
	Prioridad:
	<button v-if="todo.prioridad == 'Zlow'" type="button" class="btn btn-info btn-sm">Low</button>
	<button v-else type="button" class="btn btn-secondary btn-sm" v-on:click="priority(todo,'Zlow')">Low</button>
		<span>&nbsp;</span>
		<button v-if="todo.prioridad == 'Normal'" type="button" class="btn btn-primary btn-sm">Normal</button>
	<button v-else type="button" class="btn btn-secondary btn-sm" v-on:click="priority(todo,'Normal')">Normal</button>
	<span>&nbsp;</span>
<button v-if="todo.prioridad == 'High'" type="button" class="btn btn-danger btn-sm">High</button>
	<button v-else type="button" class="btn btn-secondary btn-sm" v-on:click="priority(todo,'High')">High</button>
<span>&nbsp;</span>
		
	
		
			<img id="clock" src="../assets/si-glyph-clock.svg"/> Añadido hace {{ todo.fecha_creacion | moment("from", new Date()) }} 
			<button class="borrar"  v-on:click="borrar(todo)"></button>

</div>

  </transition-group>
</div>

</template>

<script>
export default {
  name: 'notes',
  data(){
    return{
    message:'',
			todos_filter:[],
			todos:[]
		
    }
  },
  methods: {
    add: function () {
    	this.todos.push({Tarea:this.message,prioridad:"High",fecha_creacion:new Date(),completada:true})
		localStorage.setItem("todos",JSON.stringify(this.todos));
		this.message = '';
		
    },
 
	borrar: function(todo) {
				this.todos.splice(todo,1);
				localStorage.setItem("todos",JSON.stringify(this.todos));
        },
	borrarT:function(){
		let fin = this.todos.filter(function(todo){
			return !todo.completada;
		});
		this.todos = fin;
		localStorage.setItem("todos",JSON.stringify(this.todos));
	},
	cambiarE:function(todo){
		todo.completada=!todo.completada;
		
	},

	priority:function(todo,prioridad){
		todo.prioridad = prioridad;

}
		},
    computed:{
	completadas: function(){
		let completadas = this.todos.filter(function(todo){
			return !todo.completada;
		});
		return completadas.length;

	},
	array_order: function(){

	let ordenadas = _.orderBy(this.todos,'prioridad');
		return ordenadas;

	},
	


},
mounted(){
    if (localStorage.getItem("todos")) {
      try {
        this.todos = JSON.parse(localStorage.getItem("todos"));
   
        
      } catch (e) {
        localStorage.removeItem("todos");
      }
    }


	}
}
 

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.titulo{
 color:#43B232;
   text-decoration: line-through;
   margin-left:10px;
}
input{
	height: 100px;
	width:80%;
	border:1px solid black;
	border-radius:15px 15px 15px 15px;
}

h1{
    display:inline;
    
}
#circulo{
	
	height:30px;
	width:30px;
}

#tick{
	height:30px;
	width:30px;
}
#encabezado{
	margin-bottom: 50px;
}
#inpu{
	margin-bottom:50px;
}
.container{
	margin-bottom:50px;
}
a {
  color: orange;
}
.note{
	border: 1px solid white;
	width:80%;
	margin-left:10%;
}
.borrar{
	background-color:red;
	background-image:url(../assets/si-glyph-circle-remove.svg);
	height:40px;
	width:40px;
}
#clock{
	width:20px;
	height:20px;
}

.notas-enter-active, .notas-leave-active {
  transition: all 1s;
}
.notas-enter, .notas-leave-to /* .notas-leave-active below version 2.1.8 */ {
  opacity: 0;
  transform: translateY(30px);
}
</style>
