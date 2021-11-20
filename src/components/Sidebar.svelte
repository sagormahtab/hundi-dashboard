<script>
  import { getContext, onMount, setContext } from "svelte";
  import {
    HomeIcon,
    UsersIcon,
    ToggleLeftIcon,
    ToggleRightIcon,
  } from "svelte-feather-icons";
  import { navigate } from "svelte-routing";
  import { isDeleteOn } from "../store";

  let isDeleteOn_val;
  let activeRoute = "";

  isDeleteOn.subscribe((value) => {
    isDeleteOn_val = value;
  });

  const handleToggleDelete = () => {
    isDeleteOn.update((n) => !n);
  };

  const handleChangeRoute = (route) => {
    activeRoute = route;
    navigate(`/${activeRoute}`, { replace: true });
  };

  onMount(() => {
    if (window.location.pathname === "/") {
      activeRoute = "";
    } else {
      activeRoute = window.location.pathname.substring(
        1,
        window.location.pathname.length
      );
    }
  });
</script>

<nav
  id="sidebarMenu"
  class="col-md-3 col-lg-2 d-md-block bg-light sidebar collapse"
>
  <div class="position-sticky pt-3">
    <ul class="nav flex-column">
      <li class="nav-item" on:click={() => handleChangeRoute("")}>
        <span
          class={`nav-link ${activeRoute === "" ? "active" : ""}`}
          aria-current="page"
        >
          <HomeIcon />
          Dashboard
        </span>
      </li>
      <li class="nav-item" on:click={() => handleChangeRoute("users")}>
        <span class={`nav-link ${activeRoute === "users" ? "active" : ""}`}>
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
