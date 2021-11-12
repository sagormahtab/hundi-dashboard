<script>
  import { Router, Link, Route } from "svelte-routing";
  import Dashboard from "./components/Dashboard.svelte";
  import Header from "./components/Header.svelte";
  import Login from "./components/Login.svelte";

  export let url = "";
</script>

<main>
  <Router {url}>
    <div>
      <Route exact path="/">
        <div class="container-fluid">
          <div class="row">
            <Header />
            <Dashboard />
          </div>
        </div>
      </Route>
      <Route exact path="login" component={Login} />
    </div>
  </Router>
</main>

<style>
</style>
