<script>
  import { createEventDispatcher } from 'svelte';

  export let text = "Sample text";
  export let done = false;
  export let id;

  const dispatch = createEventDispatcher();

  const handleStatusChange = () => {
    dispatch("statuschange", {text: text, done: done, id: id});
  };

  const handleDelete = () => {
    confirm(`Delete "${text}" Task?`) && dispatch('delete', {id: id, done: done});
  };

</script>

<main>
  <div class="task-container">
    <div class="task-text">
      <input type="text" bind:value={text}>
    </div>
    <div class="task-controls">
      <input type="checkbox" class="checkbox" bind:checked={done} on:change={handleStatusChange}>
      <button class="delete-button" on:click={handleDelete}></button>
    </div>
  </div>
</main>

<style>
  .task-container {
    display: flex;
    margin-top: 1em;
    justify-content: center;
    align-items: center;
  }


  .task-text {
    text-align: center;
    position: center;
    font-size: 2em;
  }
  .task-controls {
    display: flex;
    flex-direction: row;
  }

  input[type=checkbox] {
    -ms-transform: scale(2); /* IE */
    -moz-transform: scale(2); /* FF */
    -webkit-transform: scale(2); /* Safari and Chrome */
    -o-transform: scale(2); /* Opera */
    transform: scale(2);
    padding: 10px;
    margin-left: 1em;
  }

  .delete-button {
    -ms-transform: scale(2.4); /* IE */
    -moz-transform: scale(2.4); /* FF */
    -webkit-transform: scale(2.4); /* Safari and Chrome */
    -o-transform: scale(2.4); /* Opera */
    transform: scale(2.4);
    margin-left: 1.2em;
		background: no-repeat 50% 50% url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath fill='%23676778' d='M12,2C17.53,2 22,6.47 22,12C22,17.53 17.53,22 12,22C6.47,22 2,17.53 2,12C2,6.47 6.47,2 12,2M17,7H14.5L13.5,6H10.5L9.5,7H7V9H17V7M9,18H15A1,1 0 0,0 16,17V10H8V17A1,1 0 0,0 9,18Z'%3E%3C/path%3E%3C/svg%3E");
  }

</style>