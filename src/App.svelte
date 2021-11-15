<script>
  import { Router, Link, Route } from "svelte-routing";
  import Header from "./components/Header.svelte";
  import Sidebar from "./components/Sidebar.svelte";
  import Home from "./pages/Home.svelte";
  import Login from "./pages/Login.svelte";

  export let url = "";
</script>

<main>
  <Header />
  <Sidebar />
  <Router {url}>
    <div>
      <Route exact path="/">
        <div class="container-fluid">
          <div class="row">
            <Home />
          </div>
        </div>
      </Route>
      <Route exact path="login" component={Login} />
    </div>
  </Router>
</main>

<style>
</style>
