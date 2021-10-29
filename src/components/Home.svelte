<script>
  import { EditIcon, EyeOffIcon, PlusSquareIcon } from "svelte-feather-icons";
  import Pagination from "./Pagination.svelte";
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
        <thead>
          <tr>
            <th scope="col">Sl No.</th>
            <th scope="col">Phone No</th>
            <th scope="col">Payment Method</th>
            <th scope="col">Limit</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>1,001</td>
            <td>random</td>
            <td>data</td>
            <td>placeholder</td>
            <td>
              <div class="btn-group me-2">
                <button type="button" class="btn btn-sm btn-outline-secondary"
                  ><EditIcon class="me-1" />Change</button
                >
                <button type="button" class="btn btn-sm btn-outline-secondary"
                  ><EyeOffIcon class="me-1" />Disable</button
                >
              </div>
            </td>
          </tr>
          <tr>
            <td>1,002</td>
            <td>placeholder</td>
            <td>irrelevant</td>
            <td>visual</td>
            <td>layout</td>
          </tr>
        </tbody>
      </table>
    </div>
    <Pagination />
  </div>
</main>

<style>
  tr {
    vertical-align: middle;
  }
</style>
