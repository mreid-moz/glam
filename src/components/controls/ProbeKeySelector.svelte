<script>
  import { CaretDown } from '@graph-paper/icons';
  import { FloatingMenu, MenuList, MenuListItem } from '@graph-paper/menu';
  import { tooltip as tooltipAction } from '@graph-paper/core/actions';

  export let options;
  export let currentKey;
  export let active;

  let button;
  let width;

  options.sort();

  function toggle() {
    active = !active;
  }

  function setValue(event) {
    currentKey = event.detail.key;
    active = false;
  }
</script>

<style>
  .activating-button {
    padding: var(--space-1h);
    padding-left: var(--space-base);
    padding-right: var(--space-base);
    font-size: var(--text-01);
    margin: 0;
    text-align: left;
    /* min-width: var(--space-16x); */
    background-color: white;
    display: grid;
    grid-auto-flow: column;
    width: max-content;
    grid-column-gap: var(--space-base);
    color: var(--subhead-gray-02);
    border-radius: var(--space-1h);
  }
  .menu-list-item__title {
    font-size: var(--text-01);
  }

  .active {
    background-color: rgba(0, 0, 0, 0.2);
    color: var(--subhead-gray-02);
  }
</style>

<div class="menu-button" bind:this={button}>
  <button
    class="activating-button"
    on:click={toggle}
    class:active
    use:tooltipAction={{
      text: 'this probe has multiple keys associated with it',
    }}>
    <div>
      {#each options as opt}
        <div
          style="
            visibility: {opt === currentKey
            ? 'visible'
            : 'hidden'};
            height: {opt === currentKey
            ? 'inherit'
            : 0};
          ">
          {opt}
        </div>
      {/each}
    </div>
    <CaretDown size="16" />
  </button>
</div>

{#if active}
  <FloatingMenu
    bind:width
    offset={1}
    on:cancel={() => {
      active = false;
    }}
    parent={button}>
    <MenuList on:selection={setValue}>
      {#each options as key, i}
        <MenuListItem {key} value={key}>
          <div class="menu-list-item__title">{key}</div>
        </MenuListItem>
      {/each}
    </MenuList>
  </FloatingMenu>
{/if}
