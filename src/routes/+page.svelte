<script lang="ts">
    import Login from "$lib/components/Login.svelte";
    import Messages from "$lib/components/Messages.svelte";
    import { currentUser } from "$lib/pocketbase";
  
  
  </script>
  
  <div class="container mx-auto my-12 ">
      <h1>Pocketchat</h1>
      
      <Login />
      
      {#if $currentUser}
      
        <Messages />
      
      {/if}
      
  </div>