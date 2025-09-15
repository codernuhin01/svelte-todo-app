<script lang="ts">
  import { derived } from "svelte/store";
  import TasksForm from "./lib/tasks-form.svelte"
  import TasksList from "./lib/tasks-list.svelte"

  let msg = "Todo App"
  let tasks = $state([]);
  let totalDone = $derived(tasks.reduce((total,task)=> total + Number(task.done),0))
  let currentFilter = $state("all")
  let filteredTask = $derived.by(()=>{
    switch(currentFilter){
      case "all" : {
      return tasks;
      }
      case "done" : {
        return tasks.filter((task)=>task.done)
      }
      case "todo" : {
        return tasks.filter((task)=>!task.done)
      }
      return tasks;
    }
    
  })
  
 function addTask(newTask){
  if(newTask !== ""){

    tasks.push({
      id: crypto.randomUUID(),
      title: newTask,
      done: false,
    })

    // let storeTask = localStorage.setItem('newTask',newTask)
    // let getTask = JSON.parse(localStorage.getItem('newTask'))
    // localStorage.clear()
  }
// console.log($state.snapshot(tasks))
 }
function removeTask(id){
  const index = tasks.findIndex((task)=>task.id===id)
  tasks.splice(index,1)
}

</script>

<main>
  <h1>{msg}</h1>
  <TasksForm {addTask}/>

  {#if tasks.length}
  <p>{totalDone}/{tasks.length} Tasks completd</p>
  {:else}
  <p>Add your task in the list:</p>
  {/if}
  {#if tasks.length }
    <div class="btn-container">
    <button class="secondary" onclick={() => currentFilter = 'all'} class:contrast = {currentFilter === 'all'}>All</button>
    <button class="secondary" onclick={() => currentFilter = 'done'} class:contrast = {currentFilter === 'done'} >Done</button>
    <button class="secondary" onclick={() => currentFilter = 'todo'} class:contrast = {currentFilter === 'todo'} >Todo</button>
  </div>
  {/if}
  <TasksList tasks={filteredTask} {removeTask}/>
  
</main>

<style>
  main{
    max-width: 800px;
    margin: 1rem auto;
  }
  .btn-container{
    display: flex;
    justify-content: flex-end;
    gap: 0.5rem;
    margin-bottom: 1rem;
  }
</style>