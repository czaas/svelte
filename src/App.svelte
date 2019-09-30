<script>
  let users = [
    {
      name: "John",
      id: new Date().toISOString() + 1
    },
    { name: "Bob", id: new Date().toISOString() + 2 },
    { name: "Jack", id: new Date().toISOString() + 3 }
  ];
  let newUser = "";
  let clonedUser = null;

  function deleteUser(id) {
    users = users.filter(user => user.id !== id);
  }
  function addUser() {
    if (newUser.trim("").length > 0) {
      users = [...users, { name: newUser, id: new Date().toISOString() }];
      newUser = "";
    }
  }
  function editUser(id) {
    const existingEdited = {...clonedUser};
    users = users.map(user => {
      if (user.id === id) {
        clonedUser = user;
        return {
          ...user,
          isEditing: true
        };
      }
      const isAlreadyBeingEdited = existingEdited && existingEdited.id === user.id;
      return {
        ...(isAlreadyBeingEdited ? existingEdited : user),
        isEditing: false,
      };
    });
  }
  function cancelEdit(id) {
    users = users.map(user => {
      if (user.id === id) {
        return {
          ...clonedUser,
          isEditing: false
        };
      }
      return user;
    });
    clonedUser = null;
  }
  function saveUser(id) {
    users = users.map(user => {
      if (user.id === id) {
        return {
          ...user,
          isEditing: false
        };
      }
      return user;
    });
  }
</script>

<form on:submit|preventDefault={addUser}>
  <label>
    Add New User
    <input bind:value={newUser} />
  </label>
  <button type="submit">Save</button>
</form>
<ul>
  {#each users as { name, isEditing, id } (id)}
    <li>
      {#if isEditing}
        <input bind:value={name} />
        <br />
        <button on:click={() => saveUser(id)}>Save</button>
        <button on:click={() => cancelEdit(id)}>Cancel</button>
      {:else}
        {name}
        <br />
        <button on:click={() => deleteUser(id)}>delete</button>
        <button on:click={() => editUser(id)}>Edit</button>
      {/if}
    </li>
  {/each}
</ul>
