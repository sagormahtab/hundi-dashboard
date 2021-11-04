<script>
  import Modal from "sv-bootstrap-modal";
  let isOpen = false;
</script>

<Modal bind:open={isOpen}>
  <div class="modal-header">
    <h5 class="modal-title" id="exampleModalLabel">Edit Info</h5>
    <button
      type="button"
      class="btn-close"
      data-bs-dismiss="modal"
      aria-label="Close"
    />
  </div>
  <div class="modal-body">
    <div class="container-fluid">
      <form>
        <div class="mb-3">
          <label for="inputPhone" class="form-label">Phone No</label>
          <input
            type="text"
            class="form-control"
            id="inputPhone"
            aria-describedby="Phone No"
          />
        </div>
        <div class="mb-3">
          <label for="inputPayMethod" class="form-label">Payment Method</label>
          <select
            id="inputPayMethod"
            class="form-select"
            aria-label="Payment Method"
          >
            <option selected>Choose an option...</option>
            <option value="bkash">Bkash</option>
            <option value="nagad">Nagad</option>
            <option value="roket">Roket</option>
          </select>
        </div>
        <div class="mb-3">
          <label for="inputLimit" class="form-label">Limit</label>
          <input
            type="text"
            class="form-control"
            id="inputLimit"
            aria-describedby="Limit"
          />
        </div>
        <button type="submit" class="btn btn-primary">Submit</button>
      </form>
    </div>
  </div>
  <div class="modal-footer">
    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal"
      >Close</button
    >
    <button type="button" class="btn btn-primary">Save changes</button>
  </div>
</Modal>
