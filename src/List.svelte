<script>
  import { quintOut } from 'svelte/easing';
	import { crossfade } from 'svelte/transition';
  import { tweened } from 'svelte/motion';
	import { cubicOut } from 'svelte/easing';

  import { v4 as uuidv4 } from 'uuid';
  import Task from "./Task.svelte"

  let todoTasks = [];
  let doneTasks = [];

	const progress = tweened(0, {
		duration: 400,
		easing: cubicOut
	});

  $: {
    let taskAmount = todoTasks.length + doneTasks.length;
    if(taskAmount > 0) {
      progress.set(doneTasks.length / taskAmount);
    }
    else{
      progress.set(0);
    }
  }

  const [send, receive] = crossfade({
		duration: d => Math.sqrt(d * 200),

		fallback(node, params) {
			const style = getComputedStyle(node);
			const transform = style.transform === 'none' ? '' : style.transform;

			return {
				duration: 600,
				easing: quintOut,
				css: t => `
					transform: ${transform} scale(${t});
					opacity: ${t}
				`
			};
		}
	});

  const handleKeyUp = (event) => {
    if(event.code === "Enter" || event.code === "NumpadEnter"){
      todoTasks = [...todoTasks, {
        text: event.target.value,
        done: false,
        id: uuidv4()
      }];
      event.target.value = "";
    }
  };

  const handleTaskDelete = (event) => {
    if(event.detail.done){
      doneTasks = doneTasks.filter(task => task.id !== event.detail.id);
    }
    else {
      todoTasks = todoTasks.filter(task => task.id !== event.detail.id);
    }
  };

  const handleTaskStatusChange = (event) => {
    if(event.detail.done) {
      doneTasks = [...doneTasks, {
        text: event.detail.text,
        done: true,
        id: event.detail.id
      }];
      todoTasks = todoTasks.filter(task => task.id !== event.detail.id);
    }
    else {
      todoTasks = [...todoTasks, {
        text: event.detail.text,
        done: false,
        id: event.detail.id
      }];
      doneTasks = doneTasks.filter(task => task.id !== event.detail.id);
    }
  };

</script>

<main>
  <div class="new-task">
    <input type="text" placeholder="Add a task" on:keyup={handleKeyUp}/>
    <div>
      <progress value={$progress}></progress>
    </div>
    <!-- <div class="progress-bar">
      <div style="background-color: darkgreen; width: {progress}%; height: 1em; z-index: 10;"></div>
    </div> -->
  </div>
  <div class="board">
    <div class="left">
      {#each todoTasks as {text, done, id}}
        <div in:receive="{{key: id}}" out:send="{{key: id}}">
          <Task {text} {done} {id} on:delete={handleTaskDelete} on:statuschange={handleTaskStatusChange}/>
        </div>
      {/each}
    </div>

    <div class="right">
      {#each doneTasks as {text, done, id}}
        <div in:receive="{{key: id}}" out:send="{{key: id}}">
          <Task {text} {done} {id} on:delete={handleTaskDelete} on:statuschange={handleTaskStatusChange}/>
        </div>
      {/each}
    </div>

  </div>
</main>


<style>

  .board {
		display: grid;
		grid-template-columns: 1fr 1fr;
	}
  input[type="text"] {
    text-align: center;
    position: center;
    font-size: 2em;
  }

  progress {
		display: block;
		width: 100%;
    background-color: lightgrey;
	}

  .new-task {
    border-bottom: 4px solid #383838;
  }
</style>