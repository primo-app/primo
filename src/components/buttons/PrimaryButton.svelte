<script>
  import {createEventDispatcher} from 'svelte'
  const dispatch = createEventDispatcher();

  export let id = null
  export let variants = ''
  export let type = 'button'
  export let disabled = false
  export let icon = null
</script>

<button {id} class="primary-button {variants}" {disabled} on:click={(e) => dispatch('click', e)} {type}>
  {#if icon}
    <i class={icon}></i>
  {/if}
  <slot></slot>
</button>

<style>
  .primary-button {
    @apply w-full bg-codeblack text-gray-300 py-2 rounded-br rounded-bl font-medium transition-colors duration-100;
  }
  .primary-button:hover {
      @apply bg-gray-900;
    }
  .primary-button[disabled] {
    @apply bg-gray-600 cursor-not-allowed;
  }
</style>