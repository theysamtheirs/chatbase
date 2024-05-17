<script lang="ts">
    import Login from "$lib/components/Login.svelte";
    import Messages from "$lib/components/Messages.svelte";
    import { currentUser } from "$lib/pocketbase";
  
  
  </script>
  
  <div class="container p-8 mx-auto my-8 rounded-md bg-slate-300 min-h-72">
      <Login />
      
      {#if $currentUser}
      
        <Messages />
      
      {/if}
      
  </div>

  <style lang="postcss">

  </style>