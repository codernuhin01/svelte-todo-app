<script lang="ts">
  let newTask = $state("");
  let {addTask} = $props();

  function taskSubmitted(e){
    e.preventDefault()
  
    addTask(newTask)
    newTask = ""
  }
</script>

<form onsubmit={taskSubmitted}>
  <label for="">
    Create your new task:
    <input name="newTask" bind:value={newTask} type="text">
  </label>

  <div class="btn-container">
    <button>Add</button>
  </div>
</form>

<style>
  form{
  margin-bottom: 2rem;
}
.btn-container{
  display: flex;
  justify-content: end;
}
</style>