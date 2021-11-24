<script>
  import { onMount } from "svelte";
  import { PlusSquareIcon } from "svelte-feather-icons";
  import axios from "axios";
  import Pagination from "./Pagination.svelte";
  import TableHead from "./TableHead.svelte";
  import NumbersTableBody from "./NumbersTableBody.svelte";
  import TableSkeleton from "./TableSkeleton.svelte";
  import { BASE_SERVER } from "../constants/urls";
  import NumbersForm from "./NumbersForm.svelte";
  import jwt_decode from "jwt-decode";
  import { navigate } from "svelte-routing";

  let numbers = [];
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

  const token = localStorage.getItem("token");
  if (!token) {
    alert("Please login again");
    navigate("/login", { replace: true });
  }

  const { user } = jwt_decode(token);
  if (!user || !user.username) {
    alert("Please login again");
    navigate("/login", { replace: true });
  }
  let role = "";
  role = user.role;

  const headers = {
    Authorization: `Bearer ${localStorage.getItem("token")}`,
  };

  const theaders = ["Sl No", "Phone No", "Payment Method", "Limit"];

  const handlePaginate = async (e) => {
    if (e.detail.type === "incDec") {
      paginationInfo.page = paginationInfo.page + e.detail.value;
    } else if (e.detail.type === "direct") {
      paginationInfo.page = e.detail.value;
    }
    await getNumbers();
  };

  let getNumbersURL = "";
  if (role === 0) {
    getNumbersURL = `${BASE_SERVER}/api/active?limit=${paginationInfo.limit}&page=${paginationInfo.page}`;
  } else {
    getNumbersURL = `${BASE_SERVER}/api/numbers?limit=${paginationInfo.limit}&page=${paginationInfo.page}`;
  }

  const getNumbers = async () => {
    try {
      const res = await axios.get(getNumbersURL, { headers });
      numbers = res.data.docs;
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
      if (err.response?.status === 401) {
        alert("Please Login again");
        navigate("/login", { replace: true });
      } else {
        alert(err.message);
      }
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
      .put(
        `${BASE_SERVER}/api/numbers/${e.detail.id}`,
        {
          active: !e.detail.active,
        },
        { headers }
      )
      .then((res) => {
        const itemIndex = numbers.findIndex((num) => num._id === e.detail.id);
        numbers[itemIndex].active = res.data.active;
      });
  };

  const handleEditNumber = (e) => {
    const itemIndex = numbers.findIndex((num) => num._id === e.detail.num._id);
    numbers[itemIndex] = e.detail.num;
  };

  const handleNewNumber = (e) => {
    numbers = [...numbers, e.detail.num];
  };

  const handleDeleteNumber = (e) => {
    axios
      .delete(`${BASE_SERVER}/api/numbers/${e.detail.id}`, { headers })
      .then((res) => {
        numbers = numbers.filter((num) => num._id !== e.detail.id);
      })
      .catch((error) => {
        alert(error.message);
      });
  };

  const handleReduceNumber = (e) => {
    const itemIndex = numbers.findIndex((num) => num._id === e.detail.id);
    numbers[itemIndex].limit = e.detail.limit;
  };

  onMount(getNumbers);
</script>

<main class="col-md-9 ms-sm-auto col-lg-10 px-md-4">
  <div class="">
    <h1 class="h4 mt-3 text-center">
      Welcome <span class="text-info">{user.username}</span>, to your dashboard
    </h1>
    <div
      class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom"
    >
      <h2 class="mt-4 h4">List of numbers</h2>
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
          <NumbersTableBody
            {numbers}
            {role}
            on:editnumberinit={handleChangeNumberInit}
            on:activenumber={handleActiveNumber}
            on:deletenumber={handleDeleteNumber}
            on:reducenumber={handleReduceNumber}
          />
        {/if}
      </table>
    </div>
    <NumbersForm
      {numbers}
      {modalInfo}
      on:closemodal={handleModalClose}
      on:newnumber={handleNewNumber}
      on:editnumber={handleEditNumber}
    />
    <Pagination on:paginate={handlePaginate} {paginationInfo} />
  </div>
</main>
