<script>
  import _ from 'lodash'
  import {createEventDispatcher} from 'svelte'
  const dispatch = createEventDispatcher() 

  import fieldTypes from '../../stores/app/fieldTypes'
  import {Card} from '../misc'

  export let field

</script>

<Card title={field.label}>
  {#each field.fields as subfield}
    <div class="group-item">
      <svelte:component this={_.find($fieldTypes, ['id', subfield.type]).component} field={subfield} on:input />
    </div>
  {/each}
</Card>

<style>
  .group-item {
    @apply bg-gray-900 mb-1 p-2;
  }
</style>