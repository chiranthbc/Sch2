<script lang="ts">
  interface Task {
    taskName: string;
    taskDesc: string;
    createdBy: string;
    dateTime: string;
    priorityLevel: string;
  }
  import ModalTask from "./ModalTask.svelte";
  import { taskList } from "../lib/taskStore";
  let formModal = false;

  let tasks: Task[] = [];

  taskList.subscribe((value) => (tasks = value));
  let editIndex: number | null = null; // Track the index of the task being edited

  type FormEvent = {
    detail: {
      taskName: string;
      taskDesc: string;
      createdBy: string;
      dateTime: string;
      priorityLevel: string;
    };
  };

  // Initialize the values variable
  let values: Partial<Task> = {}; // Use Partial to allow incomplete object

  function handleFormSubmit(event: FormEvent) {
    const { taskName, taskDesc, createdBy, dateTime, priorityLevel } =
      event.detail;

    const formattedDateTime = new Date(dateTime).toLocaleString("en-US", {
      dateStyle: "short", //  (long, medium, short, full)
      timeStyle: "short", // (long, medium, short)
    });

    const updatedTask = {
      taskName,
      taskDesc,
      createdBy,
      dateTime: formattedDateTime,
      priorityLevel,
    };

    if (editIndex !== null) {
      // If editIndex is not null, update existing task

      tasks[editIndex] = updatedTask;
      editIndex = null; // Reset editIndex after editing

      console.log("Task Edited:", tasks);
    } else {
      // If editIndex is null, add a new task

      tasks = [...tasks, updatedTask];
      console.log("New Task Added:", tasks);
    }

    taskList.set(tasks);
    formModal = false; // Close the modal after submission
  }

  function deleteTask(index: number) {
    tasks = tasks.filter((_, i) => i !== index);
    console.log("Task deleted:", tasks);
    taskList.set(tasks);
  }

  function editTask(index: number) {
    // Set the editIndex and populate form with existing task details

    editIndex = index;

    const taskToEdit = tasks[index];
    // Open modal for editing
    formModal = true;
    // Update form values with task details for editing

    values = {
      taskName: taskToEdit.taskName,
      taskDesc: taskToEdit.taskDesc,
      createdBy: taskToEdit.createdBy,
      dateTime: taskToEdit.dateTime,
      priorityLevel: taskToEdit.priorityLevel,
    };
  }

  function handleRunTask() {
    console.log("Tasks:", tasks); // Log the tasks array
  }
</script>

<div class="task-manager">
  <div class="header">
    <h1><i class="fa-solid fa-calendar-days"></i> Task Lists</h1>
    <div class="header-buttons">
      <button on:click={() => (formModal = true)} class="add-task-button"
        >Add New Task</button
      >
      <button>Task Job History</button>
    </div>
    <div class="modal">
      {#if formModal}
        <ModalTask on:submit={handleFormSubmit} />
      {/if}
    </div>
  </div>

  {#if !formModal}
    <div class="task-manager-header">
      <h2>Task Name</h2>
      <h2>Task Description</h2>
      <h2>CreatedBy</h2>
      <h2>DateTime</h2>
      <h2>Priority Level</h2>
      <h2>Edit</h2>
      <h2>Action</h2>
    </div>
    <div class="task-manager-list">
      <!-- Displaying Tasks -->
      {#each tasks as task, index (task)}
        <!-- Use a unique variable for each task's selection -->

        <div class="task-manager-list-item" key={task.taskName}>
          <h2>{task.taskName}</h2>
          <h2>{task.taskDesc}</h2>
          <h2>{task.createdBy}</h2>
          <h2>{task.dateTime}</h2>
          <h2>{task.priorityLevel}</h2>
          <div class="edit">
            <button
              on:click={() => deleteTask(index)}
              class="delete-task-button"
              ><i class="fa-solid fa-trash-can"></i></button
            >
            <button on:click={() => editTask(index)} class="edit-task-button"
              ><i class="fa-solid fa-pen-to-square"></i></button
            >
          </div>

          <div class="run-task">
            <button on:click={handleRunTask}>Run Task</button>
          </div>
        </div>
      {/each}
    </div>
  {/if}
</div>

<style>
  .task-manager {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    overflow: hidden;
  }

  .header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 20px;
    margin-right: 20px;
    margin-left: 20px;
  }

  .header h1 {
    font-weight: bold;
    font-size: 24px;
    display: flex;
    align-items: center;
  }

  .header h1 i {
    margin-right: 5px;
  }
  .header-buttons {
    display: flex;
    gap: 10px;
  }
  .header-buttons button {
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    background-color: #3498db;
    color: #fff;
    font-size: 16px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }

  .header-buttons button:hover {
    background-color: #2980b9;
  }

  .task-manager-header {
    display: grid; /* Use grid for more precise control */
    grid-template-columns: repeat(7, 1fr); /* Create 4 equally spaced columns */
    background-color: rgb(190, 196, 203);
    margin-left: 10px;
    margin-right: 10px;
    padding: 8px; /* Add padding for spacing */
    font-weight: bold;
    font-size: 16px;
    gap: 10px;
  }

  .task-manager-list-item {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    align-items: center;
    padding: 8px;
    font-weight: normal;
    border-bottom: 1px solid #ccc;
    font-size: 14px;
    gap: 10px;
  }

  .delete-task-button {
    background-color: #ff6347;
    color: white;
    border: none;

    border-radius: 4px;
    cursor: pointer;
    width: fit-content;
    transition: background-color 0.3s ease;
    padding: 10px;
    margin-right: 5px;
  }
  .edit-task-button {
    background-color: #ff6347;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    padding: 10px;

    transition: background-color 0.3s ease; /* Smooth transition on hover */
  }
  .edit {
    position: relative;
    display: inline-block;
  }

  .delete-task-button:hover {
    background-color: #ff7f50;
  }
  .edit-task-button:hover {
    background-color: #ff7f50;
  }

  .modal {
    position: fixed;
    top: 40%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 30%;
    height: 40%;
    z-index: 9999;
  }
  /* overflow: auto; 
    z-index: 9999; */
  /* .job-dropdown {
    
    position: relative;
    display: inline-block;
  } */

  .run-task {
    display: flex;
    justify-content: flex-start;
    align-items: flex-start;
  }
  .run-task button {
    /* Adjust button styles as needed */
    background-color: #008cba;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    padding: 8px 16px;
    transition: background-color 0.3s ease; /* Smooth transition on hover */
  }

  .run-task button:hover {
    background-color: #005f73;
  }

  /* Additional styles as needed */
</style>
