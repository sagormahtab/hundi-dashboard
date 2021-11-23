<script>
  import { Router, Route, navigate } from "svelte-routing";
  import Home from "./pages/Home.svelte";
  import Login from "./pages/Login.svelte";
  import Users from "./pages/Users.svelte";
  import Sidebar from "./components/Sidebar.svelte";
  import Header from "./components/Header.svelte";

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
          <Header />
          <Sidebar />
          <Home />
        </div>
      </div>
    </Route>
    <Route exact path="/users">
      <Header />
      <Sidebar />
      <Users />
    </Route>
    <Route exact path="login" component={Login} />
  </Router>
</main>

<style>
</style>
