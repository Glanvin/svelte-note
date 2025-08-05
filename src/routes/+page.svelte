<script lang="ts">
	import pb from "$lib/index.js";
	import { onDestroy, onMount } from "svelte";
    import '$lib/style.css'

    type Note = {
        id?: string,
        title: string,
        content: string,
        created?: string,
        updated?: string
    }

    let notes: Note[] = [];

    let newNote: Note = {
        title: '',
        content: '',
    };

    onMount(async () => {
        const result = await pb.collection('user_notes').getFullList<Note>({
            sort: '-updated',
        });
        notes = result;
    });

    async function addNote() {
        await pb.collection('user_notes').create(newNote)
        newNote.title = '';
        newNote.content = '';
    }
</script>

<div class="container">
  <div class="grid">
    {#each notes as note}
      <div class="grid-item">
        <h2 class="note-title">{note.title}</h2>
        <p class="note-content">{note.content}</p>
      </div>
    {/each}
  </div>
  
  <div class="note-form">
    <button on:click={addNote}>Add note</button>
    <input bind:value={newNote.title} placeholder="Title" />
    <input bind:value={newNote.content} placeholder="Content" />
  </div>
</div>




