<script>
  import Modal from "sv-bootstrap-modal";
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  const handleModalClose = () => {
    dispatch("closemodal");
  };

  // export let isModalOpen;
  // export let modalHeaderText;
  export let modalInfo;
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
  <form>
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
            <option value="bkash">Bkash</option>
            <option value="nagad">Nagad</option>
            <option value="rocket">Rocket</option>
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
