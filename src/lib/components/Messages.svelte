<script lang="ts">
    import { onMount, onDestroy } from 'svelte';
    import { currentUser, pb } from '../pocketbase';
    import ChatHeader from "$lib/components/ChatHeader.svelte";
  
    let newMessage: string;
    let messages = [];
    let unsubscribe: () => void;
    let currentChat = 'f9d2gzqbae0or5f';
  
    onMount(async () => {
      // Get initial messages
      const resultList = await pb.collection('messages').getList(1, 50, {
        sort: 'created',
        expand: 'user',
      });
      messages = resultList.items;
  
      // Subscribe to realtime messages
      unsubscribe = await pb
        .collection('messages')
        .subscribe('*', async ({ action, record }) => {
          if (action === 'create') {
            // Fetch associated user
            const user = await pb.collection('users').getOne(record.user);
            record.expand = { user };
            messages = [...messages, record];
          }
          if (action === 'delete') {
            messages = messages.filter((m) => m.id !== record.id);
          }
        });
    });
  
    // Unsubscribe from realtime messages
    onDestroy(() => {
      unsubscribe?.();
    });
  
    async function sendMessage() {
      const data = {
        text: newMessage,
        user: $currentUser.id,
        chat: currentChat,
      };
      const createdMessage = await pb.collection('messages').create(data);
      newMessage = '';
    }
  </script>

  <div class="flex flex-col">
    
    <ChatHeader />
    
    <div class="flex-grow p-8 overflow-y-auto rounded-md bg-slate-50">
        {#each messages.filter(message => message.chat === currentChat) as message (message.id)}
            <div class="p-4 mb-8 transition-transform rounded-md hover:bg-orange-200 hover:scale-105  {message.expand?.user?.id === $currentUser.id ? 'bg-blue-200' : 'bg-slate-100'}">
            <p class="mb-2 text-xl text-slate-800">{message.text}</p>
            <small class="{message.expand?.user?.id === $currentUser.id ? 'text-pink-500 font-medium' : 'text-slate-500'}">
                @{message.expand?.user?.username} - @{message.created}
            </small>
            </div>
      {/each}
    </div>
    <div id="message-form" class="w-full p-4 mt-4 rounded-md bg-slate-600">
      <form on:submit|preventDefault={sendMessage} class="flex max-w-full gap-4 mx-auto">
        <input class="flex-grow-0 w-full mr-2 input input-bordered" placeholder="Message" type="text" bind:value={newMessage} />
        <button class="px-8 btn" type="submit">Send</button>
      </form>
    </div>
  </div>