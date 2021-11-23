<script>
  import axios from "axios";

  import Modal from "sv-bootstrap-modal";
  import { createEventDispatcher } from "svelte";
  import { BASE_SERVER } from "../constants/urls";

  const dispatch = createEventDispatcher();

  const handleModalClose = () => {
    dispatch("closemodal");
  };

  export let modalInfo;

  const headers = {
    Authorization: `Bearer ${localStorage.getItem("token")}`,
  };

  const handleSubmit = () => {
    if (modalInfo.type === "edit") {
      axios
        .put(
          `${BASE_SERVER}/api/users/${modalInfo.id}`,
          {
            username: modalInfo.username,
            password: modalInfo.password,
            role: modalInfo.role,
          },
          { headers }
        )
        .then((res) => {
          dispatch("edituser", { user: res.data });
        })
        .catch((err) => {
          alert(err.message);
        });
    } else {
      axios
        .post(
          `${BASE_SERVER}/api/users`,
          {
            username: modalInfo.username,
            password: modalInfo.password,
            role: modalInfo.role,
          },
          { headers }
        )
        .then((res) => {
          dispatch("newuser", { user: res.data.data });
        })
        .catch((err) => {
          alert(err.message);
        });
    }

    modalInfo = {
      ...modalInfo,
      isModalOpen: false,
      username: "",
      password: "",
      role: "",
    };
  };
</script>

<Modal
  bind:open={modalInfo.isModalOpen}
  dialogClasses="modal-dialog-centered"
  onClosed={handleModalClose}
>
  <div class="modal-header">
    <h5 class="modal-title" id="exampleModalLabel">
      {modalInfo.modalHeaderText}
    </h5>
    <button
      type="button"
      class="btn-close"
      data-bs-dismiss="modal"
      aria-label="Close"
      on:click={handleModalClose}
    />
  </div>
  <form on:submit|preventDefault={handleSubmit}>
    <div class="modal-body">
      <div class="container-fluid">
        <div class="mb-3">
          <label for="inputUsername" class="form-label">Username</label>
          <input
            type="text"
            class="form-control"
            id="inputUsername"
            aria-describedby="Username"
            bind:value={modalInfo.username}
          />
        </div>
        <div class="mb-3">
          <label for="inputPassword" class="form-label">Password</label>
          <input
            type="text"
            class="form-control"
            id="inputPassword"
            aria-describedby="Password"
            bind:value={modalInfo.password}
          />
        </div>
        <div class="mb-3">
          <label for="inputRole" class="form-label">Role</label>
          <select
            id="inputRole"
            class="form-select"
            aria-label="Role"
            bind:value={modalInfo.role}
          >
            <option selected>Choose an option...</option>
            <option value={0}>User</option>
          </select>
        </div>
      </div>
    </div>
    <div class="modal-footer">
      <button
        type="button"
        class="btn btn-secondary"
        data-bs-dismiss="modal"
        on:click={handleModalClose}>Close</button
      >
      <button type="submit" class="btn btn-primary">Submit</button>
    </div>
  </form>
</Modal>
