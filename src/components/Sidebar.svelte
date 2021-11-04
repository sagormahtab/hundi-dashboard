<script>
  import { HomeIcon, UsersIcon } from "svelte-feather-icons";
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
    </ul>
  </div>
</nav>

<style>
  .nav-link {
    cursor: pointer;
  }
</style>
