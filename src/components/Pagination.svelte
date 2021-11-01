<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  export let paginationInfo;
  const handlePrevious = () => {
    dispatch("prevpage");
  };
</script>

<nav aria-label="Page navigation example">
  <ul class="pagination justify-content-end">
    <li
      class={`page-item ${!paginationInfo.hasPrevPage ? "disabled" : ""}`}
      on:click={handlePrevious}
    >
      <a class="page-link" href="/">Previous</a>
    </li>
    {#each new Array(paginationInfo.totalPages).fill("dk") as tp, i (i)}
      <li class={`page-item ${paginationInfo.page === i + 1 ? "active" : ""}`}>
        <a class="page-link" href="/">{i + 1}</a>
      </li>
    {/each}
    <li class={`page-item ${!paginationInfo.hasNextPage ? "disabled" : ""}`}>
      <a class="page-link" href="/">Next</a>
    </li>
  </ul>
</nav>
