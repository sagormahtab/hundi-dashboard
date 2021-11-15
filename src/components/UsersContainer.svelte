<script>
  import { afterUpdate, onMount } from "svelte";
  import { PlusSquareIcon } from "svelte-feather-icons";
  import axios from "axios";
  import Pagination from "./Pagination.svelte";
  import TableHead from "./TableHead.svelte";
  import UsersTableBody from "./UsersTableBody.svelte";
  import TableSkeleton from "./TableSkeleton.svelte";
  import { BASE_SERVER } from "../constants/urls";
  import UsersForm from "./UsersForm.svelte";

  let users = [];
  let paginationInfo = {
    hasPrevPage: false,
    hasNextPage: false,
    page: 1,
    limit: 10,
    totalPages: 1,
  };
  let isLoading = true;

  let modalInfo = {
    isModalOpen: false,
    modalHeaderText: "",
    type: "",
    number: "",
    paymentMethod: "Choose an option...",
    limit: "",
  };

  const theaders = ["Sl No", "Phone No", "Payment Method", "Limit"];

  const handlePaginate = async (e) => {
    if (e.detail.type === "incDec") {
      paginationInfo.page = paginationInfo.page + e.detail.value;
    } else if (e.detail.type === "direct") {
      paginationInfo.page = e.detail.value;
    }
    await getUsers();
  };

  const getUsers = async () => {
    try {
      const res = await axios.get(
        `${BASE_SERVER}/api/user?limit=${paginationInfo.limit}&page=${paginationInfo.page}`
      );
      users = res.data.docs;
      const { hasPrevPage, hasNextPage, page, limit, totalPages } = res.data;
      paginationInfo = {
        hasPrevPage,
        hasNextPage,
        page,
        limit,
        totalPages,
      };
      isLoading = false;
    } catch (err) {
      alert(err.message);
    }
  };

  const handleAddNew = () => {
    modalInfo = {
      type: "add",
      number: "",
      paymentMethod: "Choose an option...",
      limit: "",
      modalHeaderText: "Add Info",
      isModalOpen: !modalInfo.isModalOpen,
    };
  };

  const handleModalClose = () => {
    modalInfo.isModalOpen = false;
  };

  const handleChangeNumberInit = (e) => {
    modalInfo = {
      type: "edit",
      isModalOpen: !modalInfo.isModalOpen,
      modalHeaderText: "Edit Info",
      id: e.detail.id,
      number: e.detail.number,
      paymentMethod: e.detail.paymentMethod,
      limit: e.detail.limit,
    };
  };

  const handleActiveNumber = (e) => {
    axios
      .put(`${BASE_SERVER}/api/toggle/${e.detail.id}`, {
        active: !e.detail.active,
      })
      .then((res) => {
        const itemIndex = users.findIndex((num) => num._id === e.detail.id);
        users[itemIndex].active = res.data.active;
      });
  };

  const handleEditNumber = (e) => {
    const itemIndex = users.findIndex((num) => num._id === e.detail.num._id);
    users[itemIndex] = e.detail.num;
  };

  const handleNewNumber = (e) => {
    users = [...users, e.detail.num];
  };

  const handleDeleteNumber = (e) => {
    axios
      .delete(`${BASE_SERVER}/api/user/${e.detail.id}`)
      .then((res) => {
        console.log(res.data);
        const itemIndex = users.findIndex((num) => num._id === e.detail.id);
        users.splice(itemIndex, 1);
      })
      .catch((error) => {
        alert(error.message);
      });
  };

  onMount(getUsers);
</script>

<main class="col-md-9 ms-sm-auto col-lg-10 px-md-4">
  <div class="">
    <h1 class="h4 mt-3 text-center">
      Welcome <span class="text-info">user</span>, to your dashboard
    </h1>
    <div
      class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom"
    >
      <h2 class="mt-4 h4">List of users</h2>
      <button
        type="button"
        class="btn btn-sm btn-outline-secondary"
        on:click={handleAddNew}><PlusSquareIcon class="me-1" />Add New</button
      >
    </div>

    <div class="table-responsive">
      <table class="table table-striped table-sm">
        <TableHead {theaders} />
        {#if isLoading}
          <TableSkeleton />
        {:else}
          <UsersTableBody
            {users}
            on:editnumberinit={handleChangeNumberInit}
            on:activenumber={handleActiveNumber}
            on:deletenumber={handleDeleteNumber}
          />
        {/if}
      </table>
    </div>
    <UsersForm
      {users}
      {modalInfo}
      on:closemodal={handleModalClose}
      on:newnumber={handleNewNumber}
      on:editnumber={handleEditNumber}
    />
    <Pagination on:paginate={handlePaginate} {paginationInfo} />
  </div>
</main>
