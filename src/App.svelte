<script>
  import { Router, Route, navigate } from "svelte-routing";
  import Home from "./pages/Home.svelte";
  import Login from "./pages/Login.svelte";
  import Users from "./pages/Users.svelte";

  export let url = "";

  let redirect = true;
  const token = localStorage.getItem("token");
  if (!token) {
    redirect = true;
  } else {
    redirect = false;
  }

  $: {
    if (redirect) {
      navigate("/login", { replace: true });
    }
  }
</script>

<main>
  <Router {url}>
    <Route exact path="/">
      <div class="container-fluid">
        <div class="row">
          <Home />
        </div>
      </div>
    </Route>
    <Route exact path="users" component={Users} />
    <Route exact path="login" component={Login} />
  </Router>
</main>

<style>
</style>
