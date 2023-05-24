<script>
	import { onMount } from 'svelte';

	let name = '';
	let type = 'Folder';
	let folder = '';
	let items = [];
	let folders = [];

	window.addEventListener('message', (event) => {
  // Check if the message is coming from the trusted origin (the first application)
  if (event.origin === 'http://localhost:65266/') { // Replace with the origin of the first application
    // Get the username and password values from the message
    const username = event.data.username;
    const password = event.data.password;

    // Store the values in the second application's localStorage
    localStorage.setItem('username', username);
    localStorage.setItem('password', password);
  }
});
  
	function handleSubmit() {
  if (name.trim() === '') {
    alert('Please enter a name');
    return;
  }

  if (type === 'File' && folder.trim() === '') {
    alert('Please select a folder for the file');
    return;
  }

  const item = {
    name,
    type,
    folder
  };

  items = [...items, item];

  if (type === 'Folder') {
    folders = [...folders, name]; // Add the new folder name to the list of folders
  }

  resetForm();
}
function handleLogout() {
	  navigateToLogin();
	}
	function navigateToLogin() {
    // Redirect to the login page
    window.location.href = 'http://localhost:65266/';
  }

	function resetForm() {
	  name = '';
	  type = 'Folder';
	  folder = '';
	}
  
	function handleCancel() {
	  resetForm();
	}
  
	function handleTypeChange(event) {
	  type = event.target.value;
	  folder = '';
	}
  
	function handleFolderChange(event) {
	  folder = event.target.value;
	}
  </script>
  
  <main>
	<h1>Add Name</h1>
	<form on:submit|preventDefault={handleSubmit}>
	  <label>
		Name:
		<input type="text" bind:value={name} />
	  </label>
	  <label>
		Type:
		<select bind:value={type} on:change={handleTypeChange}>
		  <option value="Folder">Folder</option>
		  <option value="File">File</option>
		</select>
	  </label>
	  {#if type === 'File'}
		<label>
		  Select Folder:
		  <select bind:value={folder} on:change={handleFolderChange}>
			<option value="">None</option>
			{#each folders as folderOption}
			  <option value={folderOption}>{folderOption}</option>
			{/each}
		  </select>
		</label>
	  {/if}
	  <div>
		<button type="submit">Save</button>
		<button type="button" on:click={handleCancel}>Cancel</button>
	  </div>
	</form>
	<div>
	  <button class=".logoutButton" on:click="{handleLogout}">Logout</button>
	</div>
  
	{#if items.length > 0}
	  <h2>Items:</h2>
	  <ul>
		{#each items as item}
		  <li>{item.name} - {item.type} {#if item.type === 'File'}in {item.folder}{/if}</li>
		{/each}
	  </ul>
	{/if}
  </main>
  
  <style>
	main {
	  max-width: 600px;
	  margin: 0 auto;
	  padding: 2rem;
	}
  /* CSS for the form */
form {
  margin-bottom: 20px;
}

label {
  display: block;
  margin-bottom: 10px;
}

input[type="text"],
select {
  width: 100%;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
  background-color: #4caf50;
  color: #fff;
  border: none;
}

button[type="button"] {
  background-color: #ccc;
  margin-right: 10px;
}

/* CSS for responsiveness */
@media only screen and (max-width: 600px) {
  input[type="text"],
  select {
    width: 100%;
    margin-bottom: 10px;
  }

  button {
    width: 100%;
    margin-bottom: 10px;
  }
}

/* CSS for the logout button */
div:last-child {
  margin-top: 20px;
}

button .logoutButton {
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
  background-color: #f44336;
  color: #fff;
  border: none;
}

/* CSS for the list */
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

ul li {
  margin-bottom: 5px;
}

  </style>
  