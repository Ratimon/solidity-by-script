<script  lang="ts">

    import Header from './Header.svelte';
    import OverflowMenu from '$lib/ui/components/OverflowMenu.svelte';
    import type { KindedOptions, Kind, Contract, OptionsErrorMessages } from '$lib/wizard';
    import { ContractBuilder, buildGeneric, printContract, sanitizeKind, OptionsError } from '$lib/wizard';
    // import Footer from './Footer.svelte';

    // in case you want to use the multiple pages app 
    const headLinks = [
        {pathname: '/', title: 'Home'},
        {pathname: '/docs', title: 'Documentation'},
        {pathname: '/github', title: 'github'},
        {pathname: '/twitter', title: 'Twitter'},
    ];

    // const footLinks = [
    //     {pathname: '/', title: 'Home'},
    //     {pathname: '/docs', title: 'Documentation'},
    //     {pathname: '/github', title: 'github'},
    //     {pathname: '/twitter', title: 'Twitter'},
  	// ];

    export let initialTab: string | undefined = 'ERC20';
    export let tab: Kind = sanitizeKind(initialTab);
    
  
  </script>
    
  <Header links={headLinks} class="bg-base-200 "></Header>

  <div class="container flex flex-col gap-4 p-4">

    <div class="header flex flex-row justify-between">
        <div class="tab overflow-hidden">
            <OverflowMenu>
              <button class:selected={tab === 'ERC20'} on:click={() => tab = 'ERC20'}>
                ERC20
              </button>
              <button class:selected={tab === 'ERC721'} on:click={() => tab = 'ERC721'}>
                ERC721
              </button>
              <button class:selected={tab === 'ERC1155'} on:click={() => tab = 'ERC1155'}>
                ERC1155
              </button>
              <button class:selected={tab === 'Governor'} on:click={() => tab = 'Governor'}>
                Governor
              </button>
              <button class:selected={tab === 'Custom'} on:click={() => tab = 'Custom'}>
                Custom
              </button>
            </OverflowMenu>
          </div>
    </div>

  </div>

  <!-- <Footer links={footLinks}></Footer> -->

<!-- <style lang="postcss">
  .container {
    background-color: var(--gray-1);
    border: 1px solid var(--gray-2);
    border-radius: 10px;
    min-width: 32rem;
  }

  .header {
    font-size: var(--text-small);
  }

</style> -->
