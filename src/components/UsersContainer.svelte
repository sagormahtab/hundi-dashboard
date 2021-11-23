<script>
  import { onMount } from "svelte";
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
    username: "",
    password: "",
    role: "Choose an option...",
  };

  const theaders = ["Sl No", "Username", "Password", "Role"];

  const headers = {
    Authorization: `Bearer ${localStorage.getItem("token")}`,
  };

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
        `${BASE_SERVER}/api/users?limit=${paginationInfo.limit}&page=${paginationInfo.page}`,
        { headers }
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
      username: "",
      password: "",
      role: "Choose an option...",
      modalHeaderText: "Add Info",
      isModalOpen: !modalInfo.isModalOpen,
    };
  };

  const handleModalClose = () => {
    modalInfo.isModalOpen = false;
  };

  const handleChangeUserInit = (e) => {
    modalInfo = {
      type: "edit",
      isModalOpen: !modalInfo.isModalOpen,
      modalHeaderText: "Edit Info",
      id: e.detail.id,
      username: e.detail.username,
      password: e.detail.password,
      role: e.detail.role,
    };
  };

  const handleEditUser = (e) => {
    const itemIndex = users.findIndex((user) => user._id === e.detail.user._id);
    users[itemIndex] = e.detail.user;
  };

  const handleNewUser = (e) => {
    users = [...users, e.detail.user];
  };

  const handleDeleteUser = (e) => {
    axios
      .delete(`${BASE_SERVER}/api/users/${e.detail.id}`, { headers })
      .then((res) => {
        users = users.filter((user) => user._id !== e.detail.id);
      })
      .catch((error) => {
        alert(error.message);
      });
  };

  onMount(getUsers);
</script>

<main class="col-md-9 ms-sm-auto col-lg-10 px-md-4">
  <div class="">
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
            on:edituserinit={handleChangeUserInit}
            on:deleteuser={handleDeleteUser}
          />
        {/if}
      </table>
    </div>
    <UsersForm
      {users}
      {modalInfo}
      on:closemodal={handleModalClose}
      on:newuser={handleNewUser}
      on:edituser={handleEditUser}
    />
    <Pagination on:paginate={handlePaginate} {paginationInfo} />
  </div>
</main>
