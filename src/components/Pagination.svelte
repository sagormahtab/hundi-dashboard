<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  export let paginationInfo;
  const handlePaginate = (value, type) => {
    dispatch("paginate", { value, type });
  };
</script>

<nav aria-label="Page navigation example">
  <ul class="pagination justify-content-end">
    <li
      class={`page-item ${!paginationInfo.hasPrevPage ? "disabled" : ""}`}
      on:click={() => handlePaginate(-1, "incDec")}
    >
      <span class="page-link">Previous</span>
    </li>
    {#each Array(paginationInfo.totalPages) as _, i (i)}
      <li
        class={`page-item ${paginationInfo.page === i + 1 ? "active" : ""}`}
        on:click={() => handlePaginate(i + 1, "direct")}
      >
        <span class="page-link">{i + 1}</span>
      </li>
    {/each}
    <li
      class={`page-item ${!paginationInfo.hasNextPage ? "disabled" : ""}`}
      on:click={() => handlePaginate(1, "incDec")}
    >
      <span class="page-link">Next</span>
    </li>
  </ul>
</nav>

<style>
  .page-link {
    cursor: pointer;
  }
</style>
