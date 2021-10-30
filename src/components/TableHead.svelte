<script>
  export let theaders;
</script>

<thead>
  <tr>
    {#each theaders as theader (theader)}
      <th scope="col">{theader}</th>
    {/each}
  </tr>
</thead>
