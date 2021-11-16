<script>
  import axios from "axios";
  import { navigate } from "svelte-routing";
  import { BASE_SERVER } from "../constants/urls";

  let username = "";
  let password = "";
  const onSubmitHandler = () => {
    axios
      .post(`${BASE_SERVER}/api/login`, { username, password })
      .then((res) => {
        localStorage.setItem("token", res.data.token);
        username = "";
        password = "";
        navigate("/", { replace: true });
      })
      .catch((error) => {
        if (error.response) {
          alert(error.response.data.error);
        } else if (error.request) {
          alert(error.request);
        } else {
          alert("Error", error.message);
        }
      });
  };
</script>

<main class="form-wrapper">
  <div class="form-signin text-center">
    <form on:submit|preventDefault={onSubmitHandler}>
      <h1 class="h3 mb-3 fw-normal">Sign in first</h1>

      <div class="form-floating">
        <input
          type="text"
          name="username"
          bind:value={username}
          class="form-control"
          id="floatingInput"
          placeholder="username"
        />
        <label for="floatingInput">User name</label>
      </div>
      <div class="form-floating">
        <input
          type="password"
          name="password"
          bind:value={password}
          class="form-control"
          id="floatingPassword"
          placeholder="Password"
        />
        <label for="floatingPassword">Password</label>
      </div>

      <button class="w-100 btn btn-lg btn-primary" type="submit">Sign in</button
      >
    </form>
    <p class="mt-5 mb-3 text-muted">
      &copy;{new Date().getFullYear()} All rights reserved
    </p>
  </div>
</main>

<style>
  .form-wrapper {
    min-height: 100vh;
    display: flex;
    align-items: center;
    padding-top: 40px;
    padding-bottom: 40px;
    background-color: #f5f5f5;
  }
  .form-signin {
    width: 100%;
    max-width: 330px;
    padding: 15px;
    margin: auto;
  }

  .form-signin .form-floating:focus-within {
    z-index: 2;
  }

  /* .form-signin input[type="email"] {
    margin-bottom: -1px;
    border-bottom-right-radius: 0;
    border-bottom-left-radius: 0;
  } */

  .form-signin input[type="password"] {
    margin-bottom: 10px;
    border-top-left-radius: 0;
    border-top-right-radius: 0;
  }
</style>
