<script>
  import { rul } from "./Ruleset";
  import BaseServiceList from "./BaseServiceList.svelte";
  import SpecialBonus from "./SpecialBonus.svelte";

  import { Link, Intro, LinksPage, Value, LinksList } from "./Components";

  export let facility;
  let size = 1;

  $: {
    size = facility.size || 1;
    console.log(size);
    console.info(facility);
  }
</script>

<style>
  .dropdown-content {
    background: black;
    padding: 5px;
  }
</style>

<table class="main-table">
  <tr>
    <td colspan="2" class="table-header">{rul.str("Facility")}</td>
  </tr>
  {#each Object.entries(facility).sort((a, b) =>
    a[0] > b[0] && a[0] != 'storageTiles' ? 1 : -1
  ) as [key, prop]}
    {#if !['type', 'battlescapeTerrainData', 'craftInventoryTile', 'deployment'].includes(key)}
      <tr>
        <td class="padding-right">
          {@html rul.str(key)}
        </td>
        <td>
          {#if ['buildCostItems'].includes(key)}
            {#each Object.keys(prop).sort() as field, i}
              {#if i != 0}
                <br />
              {/if}
              <Link href={field} />
              : {prop[field].build} / {prop[field].refund}
            {/each}
          {:else if ['storageTiles'].includes(key)}
            <div class="dropdown is-hoverable">
              <div class="dropdown-trigger">
                <button
                  class="button"
                  aria-haspopup="true"
                  aria-controls="dropdown-tiles">
                  {rul.str("Expand")}
                </button>
              </div>
              <div class="dropdown-menu" id="dropdown-tiles" role="menu">
                <div class="dropdown-content" style="columns:6">
                  <LinksList items={prop} vertical={true} />
                </div>
              </div>
            </div>
          {:else if ['provideBaseFunc', 'requiresBaseFunc', 'forbiddenBaseFunc'].includes(key)}
            <BaseServiceList items={prop} vertical={true} />
          {:else if ['spriteFacility', 'spriteShape'].includes(key)}
            <div class="tight" style="columns:{size};width:{32 * size}px;zoom:2;">
              {#each { length: size } as _, y}
                {#each { length: size } as _, x}
                  <img
                      class="sprite"
                      alt="X"
                      src={rul.specialSprite('baseSprite', prop * 1 + x * size + y)} />
                {/each}
              {/each}
            </div>
          {:else}
            <Value val={prop} />
          {/if}
        </td>
      </tr>
    {/if}
  {/each}
</table>
