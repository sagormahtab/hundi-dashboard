<script>
  import { getContext, onMount, setContext } from "svelte";
  import {
    HomeIcon,
    UsersIcon,
    ToggleLeftIcon,
    ToggleRightIcon,
  } from "svelte-feather-icons";
  import { isDeleteOn } from "../store";

  let isDeleteOn_val;

  isDeleteOn.subscribe((value) => {
    isDeleteOn_val = value;
  });

  const handleToggleDelete = () => {
    isDeleteOn.update((n) => !n);
  };
</script>

<nav
  id="sidebarMenu"
  class="col-md-3 col-lg-2 d-md-block bg-light sidebar collapse"
>
  <div class="position-sticky pt-3">
    <ul class="nav flex-column">
      <li class="nav-item">
        <span class="nav-link active" aria-current="page">
          <HomeIcon />
          Dashboard
        </span>
      </li>
      <li class="nav-item">
        <span class="nav-link">
          <UsersIcon />
          Users
        </span>
      </li>
      <li class="nav-item" on:click={handleToggleDelete}>
        {#if isDeleteOn_val}
          <span class="nav-link">
            <ToggleRightIcon class="text-primary" />
            Disable Delete
          </span>
        {:else}
          <span class="nav-link">
            <ToggleLeftIcon />
            Activate Delete
          </span>
        {/if}
      </li>
    </ul>
  </div>
</nav>

<style>
  .nav-link {
    cursor: pointer;
  }
</style>
