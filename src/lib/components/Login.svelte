<script lang="ts">
  import { currentUser, pb } from '../pocketbase';

  let username: string;
  let password: string;

  async function login() {
    await pb.collection('users').authWithPassword(username, password);
  }

  async function signUp() {
    try {
      const data = {
        username,
        password,
        passwordConfirm: password,
        name: 'Sheldon',
      };
      const createdUser = await pb.collection('users').create(data);
      await login();
    } catch (err) {
      console.error(err)
    }
  }

  function signOut() {
    pb.authStore.clear();
  }

</script>

<div class="mx-auto my-4 rounded-md bg-base-700">
    <h1 class="text-3xl font-bold text-center text-pink-400">SamChat</h1>
    {#if $currentUser}
      <div class="flex justify-between max-w-full p-4 rounded bg-slate-200">
          <p class=" text-slate-800">
            Signed in as <span class="font-bold text-pink-500">{$currentUser.username}</span>
            <button on:click={signOut} class="ml-2 btn btn-ghost btn-sm">Sign Out</button>
          </p>
      </div>
    {:else}
      <form on:submit|preventDefault class="max-w-full">
        <div class="mb-4">
            <label class="w-full mb-2 form-control">
                <div class="label">
                  <span class="label-text text-slate-800">Username</span>
                </div>
                <input
                    placeholder="Username"
                    type="text"
                    bind:value={username}
                    class="input input-bordered"
                    id="username"
                 />
            </label>
            <label class="w-full form-control">
                <div class="label">
                  <span class="label-text text-slate-800">Create password</span>
                </div>
                <input
                  placeholder="Password"
                  type="password"
                  bind:value={password}
                  class="input input-bordered"
                  id="password"
                />
            </label>
        </div>
        <div class="flex gap-4">
            <button class="btn btn-primary grow" on:click={signUp}>Sign Up</button>
            <button class="btn btn-secondary grow" on:click={login}>Login</button>
        </div>
      </form>
    {/if}
</div>
