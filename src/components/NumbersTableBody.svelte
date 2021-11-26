<script>
  import { createEventDispatcher } from "svelte";
  import {
    EditIcon,
    EyeOffIcon,
    EyeIcon,
    Trash2Icon,
    MinusSquareIcon,
    SaveIcon,
    XSquareIcon,
  } from "svelte-feather-icons";
  import axios from "axios";
  import { isDeleteOn, reduceInputShow } from "../store";
  import { BASE_SERVER } from "../constants/urls";

  const dispatch = createEventDispatcher();
  let reduceInputShow_val = false;
  let reduceInputId = "";
  let reduceAmount = 0;
  let isButtonDisabled = false;

  reduceInputShow.subscribe((value) => {
    reduceInputShow_val = value;
  });

  const handleChange = (id, number, paymentMethod, limit) => {
    dispatch("editnumberinit", { id, number, paymentMethod, limit });
  };

  const handleActive = (id, active) => {
    dispatch("activenumber", { id, active });
  };

  const handleDelete = (id) => {
    dispatch("deletenumber", { id });
  };

  const handleCancel = () => {
    reduceInputShow.set(false);
    reduceInputId = "";
  };

  const toggleReduceInput = (id) => {
    reduceInputId = id;
    reduceInputShow.set(true);
  };

  const headers = {
    Authorization: `Bearer ${localStorage.getItem("token")}`,
  };

  const handleReduce = (id) => {
    isButtonDisabled = true;
    axios
      .post(
        `${BASE_SERVER}/api/limit/${id}`,
        { amount: reduceAmount },
        { headers }
      )
      .then((res) => {
        isButtonDisabled = false;
        dispatch("reducenumber", { id, limit: res.data.limit });
      })
      .catch((error) => {
        isButtonDisabled = false;
        alert(error.message);
      });
  };

  let isDeleteOn_val;

  isDeleteOn.subscribe((value) => {
    isDeleteOn_val = value;
  });

  export let numbers;
  export let role;
</script>

<tbody>
  {#if numbers.length > 0}
    {#each numbers as num, i (num._id)}
      <tr style="background: {!num.active ? '#e74c3c' : ''}">
        <td>{i + 1}</td>
        <td>{num.number}</td>
        <td>{num.paymentMethod}</td>
        {#if reduceInputShow_val && reduceInputId === num._id}
          <td>
            <input
              type="text"
              class="form-control"
              id="inputReduce"
              aria-describedby="Reduce Amount"
              bind:value={reduceAmount}
            />
          </td>
        {:else}
          <td>{num.limit}</td>
        {/if}
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
          {:else if role === 0}
            {#if reduceInputShow_val && reduceInputId === num._id}
              <div class="btn-group me-2">
                <button
                  type="button"
                  disabled={isButtonDisabled}
                  class="btn btn-sm btn-outline-secondary"
                  on:click={() => handleReduce(num._id)}
                  ><SaveIcon class="me-1" /><span class="btn-text-hide"
                    >Save</span
                  ></button
                >
                <button
                  type="button"
                  disabled={isButtonDisabled}
                  class="btn btn-sm btn-outline-secondary"
                  on:click={handleCancel}
                  ><XSquareIcon class="me-1" /><span class="btn-text-hide"
                    >Cancel</span
                  ></button
                >
              </div>
            {:else}
              <button
                type="button"
                class="btn btn-sm btn-outline-secondary"
                on:click={() => toggleReduceInput(num._id)}
                ><MinusSquareIcon class="me-1" />Reduce</button
              >
            {/if}
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

  @media (max-width: 575px) {
    .btn-text-hide {
      display: none;
    }
  }
</style>
