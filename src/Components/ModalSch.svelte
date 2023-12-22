<!-- ModalForm.svelte -->
<script>
    import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();
  let values = {
    schName: '',
    taskName:'',
    frequency:'',
    runEvery:'',
    time:'',
    failureAttempts:'',
    status:'',
  };


  function handleSubmit() {
    const formValues = { ...values };
    dispatch('submit', formValues); // Emitting the submit event with form values
    values = {  schName: '',
    
    taskName:'',
    frequency:'',
    runEvery:'',
    time:'',
    failureAttempts:'',
    status:'',
  }
}
</script>

<div class="modal-form">
  <form on:submit|preventDefault={handleSubmit}>

    <h3>Create Schedule</h3>
    <label for="schName">Schedule Name</label>
    <input type="text" name="schName" id="schName" bind:value={values.schName}><br>

    <label for="taskName">Task Name</label>
    <input type="text" name="rtaskName" id="taskName"bind:value={values.taskName}><br>

    <label for="frequency" style="margin-right: 20px;">Frequency:</label>
    <select id="frequency" name="frequency" bind:value={values.frequency}>
        <option value="weekly">Weekly</option>
        <option value="Daily">Daily</option>
        <option value="OnlyThisTime">OnlyThisTime</option>
    </select> <br><br>

    <label for="frequency">Run every:</label>
   <input type="checkbox" id="monday" name="day" value="monday" bind:group={values.runEvery}>
    <label for="monday">Mon</label>

    <input type="checkbox" id="tuesday" name="day" value="tuesday"bind:group={values.runEvery}>
    <label for="tuesday">Tues</label>

    <input type="checkbox" id="wednesday" name="day" value="wednesday"bind:group={values.runEvery}>
    <label for="wednesday">Wed</label>

    <input type="checkbox" id="thursday" name="day" value="thursday"bind:group={values.runEvery}>
    <label for="thursday">Thurs</label>

    <input type="checkbox" id="friday" name="day" value="friday"bind:group={values.runEvery}>
    <label for="friday">Fri</label>

    <input type="checkbox" id="saturday" name="day" value="saturday"bind:group={values.runEvery}>
    <label for="saturday">Sat</label>

    <input type="checkbox" id="sunday" name="day" value="sunday"bind:group={values.runEvery}>
    <label for="sunday">Sun</label> <br><br>

    <label for="Time" style="margin-right: 20px;">Time:</label>
    <input type="time" name="Time" id="Time" bind:value={values.time}><br><br>

    <label for="failureAttempts">No of attempts on failure</label>
    <input type="number" name="failureAttempts" id="failureAttempts" bind:value={values.failureAttempts}> 
    
    <label for="Status">Status</label>

    <label for="Active">
      <input type="radio" name="Status" id="status" value="Active" bind:group={values.status}>
     Active
    </label>

    <label for="Medium">
      <input type="radio"  name="Status" id="status "value="Inactive" bind:group={values.status}>
      Inactive
    </label>

    <label for="Low">
      <input type="radio"  name="Status" id="status" value="Active for period" bind:group={values.status}>
      Active For Period
    </label><br><br>

   

    

    <button type="submit">Submit</button><br>
  </form>
</div>

<!-- ModalForm.svelte -->
<style>
  .modal-form {
    padding: 20px;
    background-color:  #f2f2f2;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    font-family: Arial, sans-serif;
    
    margin: auto;
  }

  .modal-form h3 {
    margin-top: 0;
    font-size: 24px;
    color: #0a0a0a;
  }

  .modal-form label {
    display: inline-block;
    margin-bottom: 5px;
    font-weight: bold;
    color: #0a0a0a;
  }

  .modal-form input[type="text"],
  /* .modal-form input[type="datetime-local"], */
  .modal-form input[type="number"] {
    width: calc(100% - 22px); /* Adjust input width accounting for padding and border */
    padding: 8px;
    margin-bottom: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 16px;
    box-sizing: border-box;
  }

  .modal-form input[type="radio"] {
    margin-right: 10px;
    display: flex;
    justify-content:space-around;
  }

  .modal-form button[type="submit"] {
    display: block;
    width: 100%;
    padding: 10px;
    border: none;
    border-radius: 4px;
    background-color: #007bff;
    color: #fff;
    font-size: 16px;
    cursor: pointer;
  }

  .modal-form button[type="submit"]:hover {
    background-color: #0056b3;
  }

  /* Additional CSS for radio buttons in a single line */
  .modal-form label input[type="radio"] {
    display: inline-block;
    vertical-align: baseline;
  }
</style>
