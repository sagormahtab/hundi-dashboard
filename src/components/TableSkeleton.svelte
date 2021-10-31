<!-- copied from copepen. found after searching "bootstrap skeleton" on google -->

<tbody class="container loading-skeleton">
  {#each [1, 2, 3, 4, 5] as num, i (i)}
    <tr key={num._id}>
      <td>{i + 1}</td>
      <td>01235468571</td>
      <td>bkash</td>
      <td>25000</td>
      <td> Some dummy text </td>
    </tr>
  {/each}
</tbody>

<style>
  .container {
    margin-top: 4em;
    margin-bottom: 4em;
  }
  .loading-skeleton tr,
  .loading-skeleton td {
    color: transparent;
    appearance: none;
    -webkit-appearance: none;
    background-color: #eee;
    border-color: #eee;
  }
  .loading-skeleton tr::placeholder,
  .loading-skeleton td::placeholder {
    color: transparent;
  }
  @keyframes loading-skeleton {
    from {
      opacity: 0.4;
    }
    to {
      opacity: 1;
    }
  }
  .loading-skeleton {
    pointer-events: none;
    animation: loading-skeleton 1s infinite alternate;
  }
</style>
