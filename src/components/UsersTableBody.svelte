<script>
  import { createEventDispatcher } from "svelte";
  import { EditIcon, Trash2Icon } from "svelte-feather-icons";
  import { isDeleteOn } from "../store";

  const dispatch = createEventDispatcher();
  const handleChange = (id, username, password, role) => {
    dispatch("edituserinit", { id, username, password, role });
  };

  const handleDelete = (id) => {
    dispatch("deleteuser", { id });
  };

  let isDeleteOn_val;

  isDeleteOn.subscribe((value) => {
    isDeleteOn_val = value;
  });

  export let users;
</script>

<tbody>
  {#if users.length > 0}
    {#each users as user, i (user._id)}
      <tr>
        <td>{i + 1}</td>
        <td>{user.username}</td>
        <td>{user.password}</td>
        <td>{user.role == 0 ? "user" : "admin"}</td>
        <!-- <td style={{}} onClick={() => deleteUser(user.id)}>Change</td> -->
        <td>
          {#if isDeleteOn_val}
            <button
              type="button"
              class="btn btn-sm btn-outline-secondary"
              on:click={() => handleDelete(user._id)}
              ><Trash2Icon class="me-1" />Delete</button
            >
          {:else}
            <button
              type="button"
              class="btn btn-sm btn-outline-secondary"
              on:click={() =>
                handleChange(user._id, user.username, user.password, user.role)}
              ><EditIcon class="me-1" />Change</button
            >
          {/if}
        </td>
      </tr>
    {/each}
  {/if}
</tbody>

<style>
  tr {
    vertical-align: middle;
  }
</style>
