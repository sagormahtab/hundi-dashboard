<script>
  import HomeAdminBtnGrp from "./HomeAdminBtnGrp.svelte";
  export let numbers;
</script>

<tbody>
  {#if numbers.length > 0}
    {#each numbers as num, i (i)}
      <tr key={num._id} style={{ background: !num.active ? "#e74c3c" : "" }}>
        <td>{i + 1}</td>
        <td>{num.number}</td>
        <td>{num.paymentMethod}</td>
        <td>{num.limit}</td>
        <!-- <td style={{}} onClick={() => deleteNum(num.id)}>Change</td> -->
        <td>
          <HomeAdminBtnGrp />
        </td>
      </tr>
    {/each}
  {/if}
</tbody>

<style>
  tr {
    vertical-align: middle;
  }
</style>
