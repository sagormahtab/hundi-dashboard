<script>
  import { Router, Link, Route } from "svelte-routing";
  import Dashboard from "./components/Dashboard.svelte";
  import Header from "./components/Header.svelte";
  import Login from "./components/Login.svelte";

  export let url = "";
</script>

<main>
  <Header />
  <div class="container-fluid">
    <div class="row">
      <Router {url}>
        <div>
          <Route exact path="/"><Dashboard /></Route>
          <Route exact path="login" component={Login} />
        </div>
      </Router>
    </div>
  </div>
</main>

<style>
</style>
