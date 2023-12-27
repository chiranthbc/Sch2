<!-- +Layout.svelte -->
<script>
</script>

<div class="container">
  <div class="sideBar">
    <h1><a href="/">Home</a></h1>
    <h1><a href="/Task">Task</a></h1>
    <h1><a href="/scheduler">Scheduler</a></h1>
    <h1><a href="/Jobs">Jobs</a></h1>
  </div>
  <div class="content">
    <slot />
  </div>
</div>

<style>
  .container {
    display: grid;
    grid-template-columns: min-content 1fr; /* Sidebar is set to a minimum width, content area fills the rest */
    gap: 20px; /* Adjust the gap between the sidebar and content */
  }

  .sideBar {
    /* Your sidebar styles */
    width: 200px; /* Adjust as needed */
    background-color: #f0f0f0; /* Sample background color */
    position: sticky;
    top: 0;
  }

  .content {
    /* Other styles for your content area */
    background-color: #ffffff; /* Sample background color */
  }

  .sideBar a {
    color: #333; /* Adjust the link color */
    text-decoration: none;
    display: block; /* Make the links fill the entire width of the sidebar */
    padding: 10px 20px; /* Add padding for better readability */
    transition: color 0.3s ease; /* Smooth transition effect */
  }
  .sideBar a:hover {
    color: #6c757d; /* Adjust the hover color */
    background-color: #f0f0f0; /* Background color on hover */
  }
  h1 {
    margin-bottom: 50px;
  }
</style>
