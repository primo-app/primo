<script>
  import {ContextMenu} from '../misc'

</script>

<div class="relative">
  <button class="rounded-sm px-4 py-2 bg-gray-200 font-semibold outline-none">
    <svg class="fill-current h-3 w-3" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><title>Menu</title><path d="M0 3h20v2H0V3zm0 6h20v2H0V9zm0 6h20v2H0v-2z"/></svg>
  </button>
  <ContextMenu on:click={() => alert('clicked context menu')}/>
</div>
