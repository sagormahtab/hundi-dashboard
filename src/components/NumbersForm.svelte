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
          `${BASE_SERVER}/api/numbers/${modalInfo.id}`,
          {
            number: modalInfo.number,
            paymentMethod: modalInfo.paymentMethod,
            limit: modalInfo.limit,
          },
          { headers }
        )
        .then((res) => {
          dispatch("editnumber", { num: res.data });
        })
        .catch((err) => {
          alert(err.message);
        });
    } else {
      axios
        .post(
          `${BASE_SERVER}/api/numbers`,
          {
            number: modalInfo.number,
            paymentMethod: modalInfo.paymentMethod,
            limit: modalInfo.limit,
          },
          { headers }
        )
        .then((res) => {
          dispatch("newnumber", { num: res.data });
        })
        .catch((err) => {
          alert(err.message);
        });
    }

    modalInfo = {
      ...modalInfo,
      isModalOpen: false,
      number: "",
      paymentMethod: "",
      limit: "",
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
          <label for="inputPhone" class="form-label">Phone No</label>
          <input
            type="text"
            class="form-control"
            id="inputPhone"
            aria-describedby="Phone No"
            bind:value={modalInfo.number}
          />
        </div>
        <div class="mb-3">
          <label for="inputPayMethod" class="form-label">Payment Method</label>
          <select
            id="inputPayMethod"
            class="form-select"
            aria-label="Payment Method"
            bind:value={modalInfo.paymentMethod}
          >
            <option selected>Choose an option...</option>
            <option value="Bkash">Bkash</option>
            <option value="Nagad">Nagad</option>
            <option value="Rocket">Rocket</option>
          </select>
        </div>
        <div class="mb-3">
          <label for="inputLimit" class="form-label">Limit</label>
          <input
            type="text"
            class="form-control"
            id="inputLimit"
            aria-describedby="Limit"
            bind:value={modalInfo.limit}
          />
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
