<script>
  import { getContext, onMount, setContext } from "svelte";
  import {
    HomeIcon,
    UsersIcon,
    ToggleLeftIcon,
    ToggleRightIcon,
    LogOutIcon,
  } from "svelte-feather-icons";
  import { navigate } from "svelte-routing";
  import { isDeleteOn, userRole } from "../store";

  const signOutHandler = () => {
    localStorage.removeItem("token");
    navigate("/login", { replace: true });
  };

  let isDeleteOn_val;
  let activeRoute = "";
  let role = null;

  userRole.subscribe((value) => {
    role = value;
  });

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
      {#if role !== 0}
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
      {/if}
      <li class="nav-item" on:click={signOutHandler}>
        <span class={`nav-link`}>
          <LogOutIcon />
          Sign Out
        </span>
      </li>
    </ul>
  </div>
</nav>

<style>
  .nav-link {
    cursor: pointer;
  }
</style>
