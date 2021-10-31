<script>
  import { onMount } from "svelte";
  import { EditIcon, EyeOffIcon, PlusSquareIcon } from "svelte-feather-icons";
  import axios from "axios";
  import Pagination from "./Pagination.svelte";
  import TableHead from "./TableHead.svelte";
  import TableBody from "./TableBody.svelte";
  import TableSkeleton from "./TableSkeleton.svelte";
  import { BASE_SERVER } from "../constants/urls";

  let numbers = [];
  let isLoading = true;
  const theaders = ["Sl No", "Phone No", "Payment Method", "Limit"];

  const getNumbers = () => {
    axios.get(`${BASE_SERVER}/api/numbers?limit=50&page=1`).then((res) => {
      numbers = res.data.docs;
      isLoading = false;
    });
  };

  onMount(getNumbers);
</script>

<main class="col-md-9 ms-sm-auto col-lg-10 px-md-4">
  <div class="">
    <h1 class="h4 mt-3 text-center">
      Welcome <span class="text-info">user</span>, to your dashboard
    </h1>
    <div
      class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom"
    >
      <h2 class="mt-4 h4">List of numbers</h2>
      <button type="button" class="btn btn-sm btn-outline-secondary"
        ><PlusSquareIcon class="me-1" />Add New</button
      >
    </div>

    <div class="table-responsive">
      <table class="table table-striped table-sm">
        <TableHead {theaders} />
        {#if isLoading}
          <TableSkeleton />
        {:else}
          <TableBody {numbers} />
        {/if}
      </table>
    </div>
    <Pagination />
  </div>
</main>
