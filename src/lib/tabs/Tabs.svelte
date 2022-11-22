<script lang="ts">
	export let tabs = []
  export let activeTab = 0;

  let ul;
  let lis = [];
  let startIndex = 0;
  let firstVisibleIndex = -1;
  let lastVisibleIndex = -1;

  const isVisible = (element) => {
    const { bottom: elementBottom } = element.getBoundingClientRect();
    const { bottom: containerBottom } = ul.getBoundingClientRect();
    return elementBottom === containerBottom;
  }

  const updateVisibleTabsIndexes = () => {

    let firstTime = true;

    for(let i = 0, len = lis.length; i < len; i++) {
      if( isVisible(lis[i]) ) {
        if( firstTime ) {
          firstVisibleIndex = i;
          firstTime = false;
        }
        lastVisibleIndex = i;
      }
    }
  }

  const moveLeft = () => {
    lis[firstVisibleIndex - 1].classList.remove("hidden");
    updateVisibleTabsIndexes();
  }
  const moveRight = () => {
    lis[firstVisibleIndex].classList.add("hidden");
    updateVisibleTabsIndexes();
  }
  const onResize = () => {
    lis.forEach(e => e.classList.remove("hidden"));
    updateVisibleTabsIndexes();
  }

  $: if( lis.length ) updateVisibleTabsIndexes()
</script>

<svelte:window on:resize={onResize} />

<div class="container">
  {#if firstVisibleIndex > 0}
    <div on:click={moveLeft} style="padding-top: 8px; transform: rotate(180deg);">
      <svg width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M6.5 4L11.5 9L6.5 14" stroke="#212121" stroke-width="2" stroke-miterlimit="10" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </div>
  {/if}

  <ul bind:this={ul} class="tabrow">
    {#each tabs as tab, idx}
      <li bind:this={lis[idx]} class:selected={idx === activeTab} on:click={() => (activeTab = idx)}>
        {tab.name}
      </li>
    {/each}
  </ul>

  {#if lastVisibleIndex < lis.length - 1}
    <div on:click={moveRight} style="padding-top: 8px;">
      <svg width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M6.5 4L11.5 9L6.5 14" stroke="#212121" stroke-width="2" stroke-miterlimit="10" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </div>
  {/if}
</div>

<div class="content">
  <svelte:component this={tabs[activeTab].content} />
</div>

<style>
  :global(.hidden) {
    display: none !important;
  }
  .container {
    display: flex;
    justify-content: space-between;
  }
  .tabrow {
    text-align: center;
    list-style: none;
    padding: 0;
    line-height: 35px;
    height: 37px;
    width: 100%;
    overflow: hidden;
    font-size: 12px;
    position: relative;
    margin: 0px;
  }

  .tabrow li {
    border: 1px solid #AAA;
    background: #D1D1D1;
    background: linear-gradient(top, #ECECEC 50%, #D1D1D1 100%);
    display: inline-block;
    position:relative;
    user-select: none;
    z-index: 0;
    border-bottom-left-radius: 6px;
    border-bottom-right-radius: 6px;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.4), inset 0 1px 0 #FFF;
    text-shadow: 0 1px #FFF;
    margin: 0 -5px;
    padding: 0 30px;
  }

  .tabrow a {
    color: #555;
    text-decoration: none;
  }

  .tabrow li.selected {
    background: #FFF;
    color: #333;
    z-index: 2;
    border-top-color: #FFF;
    font-weight: bold;
  }

  .tabrow:before {
    position: absolute;
    content: "";
    width: 100%;
    top: 0;
    left: 0;
    border-top: 1px solid #AAA;
    z-index: 1;
  }

  .tabrow li:before,
  .tabrow li:after {
    border: 1px solid #AAA;
    position: absolute;
    top: -1px;
    width: 6px;
    height: 6px;
    content: "";
  }

  .tabrow li:before {
    left: -7px;
    border-top-right-radius: 6px;
    border-width: 1px 1px 0px 0px;
    box-shadow: 2px 0px 0 #ECECEC;
  }

  .tabrow li:after {
    right: -7px;
    border-top-left-radius: 6px;
    border-width: 1px 0px 0px 1px;
    box-shadow: -2px 0px 0 #ECECEC;
  }

  .tabrow li.selected:before {
    box-shadow: 2px 0px 0 #FFF;
  }

  .tabrow li.selected:after {
    box-shadow: -2px 0px 0 #FFF;
  }

  .content {
    display: block;
    border: 1px solid #e0e0e0;
    padding: 10px 15px;
    background-color: #fff;
  }
</style>
