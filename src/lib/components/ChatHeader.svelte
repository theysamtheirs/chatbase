<!-- <script>
    import { onMount, onDestroy } from 'svelte';
    import { pb } from '../pocketbase';

    let newChat = '';
    
    async function createChat() {
      const data = {
        chatName: newChat,
      };
      
      const createdChat = await pb.collection('chats').create(data);
      newChat = '';
    }
</script>

<div id="chat-form" class="w-full p-4 mt-4 rounded-md bg-slate-600">
    <form on:submit|preventDefault={createChat} class="flex max-w-full gap-4 mx-auto">
      <input class="flex-grow-0 w-full mr-2 input input-bordered" placeholder="Chat name" type="text" bind:value={newChat} />
      <button class="px-8 btn" type="submit">Start</button>
    </form>
</div> -->

