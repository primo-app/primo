<script>
  import { onMount, tick } from 'svelte';
  import { find, unionBy } from 'lodash';
  import Block from './Layout/Block.svelte';
  import {
    pages,
    wrapper as siteWrapper,
    symbols,
    fields as siteFields,
  } from '../../stores/data/draft';
  import {
    id,
    wrapper as pageWrapper,
    content,
    fields as pageFields,
  } from '../../stores/app/activePage';

  $: pageExists = findPage($id, $pages);
  function findPage(id, pages) {
    const [root] = id.split('/');
    const rootPage = find(pages, ['id', root]);
    const childPage = rootPage ? find(rootPage?.pages, ['id', id]) : null;
    return childPage || rootPage;
  }

  function hydrateInstance(block, symbols) {
    const symbol = find(symbols, ['id', block.symbolID]);
    // overwrite the symbol field values
    return {
      ...block,
      value: {
        ...symbol.value,
        fields: symbol.value.fields.map((field) => {
          const originalField = find(block.value.fields, ['id', field.id]) ||
            find(symbol.value.fields, ['id', field.id]) || { value: '' };
          return {
            ...field,
            fields:
              field.type === 'repeater' ? field.fields : originalField.fields,
            value: originalField.value,
          };
        }),
      },
    };
  }

  let element;

  // Disable the links on the page that don't navigate to a page within primo
  // TODO: prevent navigating away from site
  // prevent navigating to pages that don't exist
  async function disableLinks(_) {
    if (!element) return;
    setTimeout(() => {
      element.querySelectorAll('a').forEach((link) => {
        if (window.location.host !== link.host) {
          link.setAttribute('data-tinro-ignore', '');
          link.onclick = (e) => {
            e.preventDefault();
            window.open(link.href, '_blank');
          };
        }
      });
    }, 100);
  }
  $: disableLinks($content);

</script>

<div
  bind:this={element}
  class="primo-page"
  style="border-top: 48px solid rgb(20,20,20)">
  {#if pageExists}
    {#each $content as block, i (block.id)}
      {#if block.symbolID}
        <Block
          block={hydrateInstance(block, $symbols, $pageFields, $siteFields)}
          {i} />
      {:else}
        <Block {block} {i} />
      {/if}
    {/each}
  {/if}
  {@html $pageWrapper.below.final}
  {@html $siteWrapper.below.final}
</div>
