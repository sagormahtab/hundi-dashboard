<script>
  import { createEventDispatcher, getContext } from "svelte";
  import {
    EditIcon,
    EyeOffIcon,
    EyeIcon,
    Trash2Icon,
  } from "svelte-feather-icons";
  import { isDeleteOn } from "../store";

  const dispatch = createEventDispatcher();
  const handleChange = (id, number, paymentMethod, limit) => {
    dispatch("editnumberinit", { id, number, paymentMethod, limit });
  };

  const handleActive = (id, active) => {
    dispatch("activenumber", { id, active });
  };

  const handleDelete = (id) => {
    dispatch("deletenumber", { id });
  };

  let isDeleteOn_val;

  isDeleteOn.subscribe((value) => {
    isDeleteOn_val = value;
  });

  export let numbers;
</script>

<tbody>
  {#if numbers.length > 0}
    {#each numbers as num, i (num._id)}
      <tr style="background: {!num.active ? '#e74c3c' : ''}">
        <td>{i + 1}</td>
        <td>{num.number}</td>
        <td>{num.paymentMethod}</td>
        <td>{num.limit}</td>
        <!-- <td style={{}} onClick={() => deleteNum(num.id)}>Change</td> -->
        <td>
          {#if isDeleteOn_val}
            <button
              type="button"
              class="btn btn-sm btn-outline-secondary {!num.active
                ? 'text-white border-white'
                : ''}"
              on:click={() => handleDelete(num._id)}
              ><Trash2Icon class="me-1 text-danger" />Delete</button
            >
          {:else}
            <div class="btn-group me-2">
              <button
                type="button"
                class="btn btn-sm btn-outline-secondary {!num.active
                  ? 'text-white border-white'
                  : ''}"
                on:click={() =>
                  handleChange(
                    num._id,
                    num.number,
                    num.paymentMethod,
                    num.limit
                  )}><EditIcon class="me-1" />Change</button
              >
              <button
                type="button"
                class="btn btn-sm btn-outline-secondary {!num.active
                  ? 'text-white border-white'
                  : ''}"
                on:click={() => handleActive(num._id, num.active)}
              >
                {#if !num.active}
                  <EyeIcon class="me-1" /> Enable
                {:else}
                  <EyeOffIcon class="me-1" /> Disable
                {/if}
              </button>
            </div>
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
