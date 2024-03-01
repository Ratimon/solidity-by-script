<script  lang="ts">

    import { createEventDispatcher } from 'svelte';

    import hljs from './highlightjs';

    import Header from './Header.svelte';
    import CopyIcon from '$lib/ui/icons/CopyIcon.svelte';
    import CheckIcon from '$lib/ui/icons/CheckIcon.svelte';
    // import Footer from './Footer.svelte';

    import type { KindedOptions, Kind, Contract, OptionsErrorMessages } from '$lib/wizard';
    import { ContractBuilder, buildGeneric, printContract, sanitizeKind, OptionsError } from '$lib/wizard';
    import { postConfig } from './post-config';

    import { injectHyperlinks } from '$lib/ui/utils/inject-hyperlinks';

    

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

    const dispatch = createEventDispatcher();

    export let initialTab: string | undefined = 'ERC20';
    export let tab: Kind = sanitizeKind(initialTab);

    $: {
      tab = sanitizeKind(tab);
      dispatch('tab-change', tab);
    };

    let allOpts: { [k in Kind]?: Required<KindedOptions[k]> } = {};
    let errors: { [k in Kind]?: OptionsErrorMessages } = {};

    let contract: Contract = new ContractBuilder('MyToken');

    $: opts = allOpts[tab];

    $: {
      if (opts) {
        try {
          contract = buildGeneric(opts);
          errors[tab] = undefined;
        } catch (e: unknown) {
          if (e instanceof OptionsError) {
            errors[tab] = e.messages;
          } else {
            throw e;
          }
        }
      }
    }

$: code = printContract(contract);
$: highlightedCode = injectHyperlinks(hljs.highlight('solidity', code).value);

const language = 'solidity';


    let copied = false;
    const copyHandler = async () => {
      await navigator.clipboard.writeText(code);
      copied = true;
      if (opts) {
        await postConfig(opts, 'copy', language);
      }
      setTimeout(() => {
        copied = false;
      }, 1000);
    };
    
  
  </script>
    
  <Header links={headLinks} class="bg-base-200 "></Header>

  <!-- <div class="container flex flex-col gap-4 p-4"> -->

  <div class="flex flex-col gap-4 p-4">


    <div class="flex flex-row justify-between">
      <div class="overflow-hidden">

            <ul class="menu menu-horizontal bg-base-200">
              <li>
                <button class:selected={tab === 'ERC20'} on:click={() => tab = 'ERC20'}>
                  ERC20
                </button>
              </li>
              <li>
                <button class:selected={tab === 'ERC721'} on:click={() => tab = 'ERC721'}>
                  ERC721
                </button>
              </li>
              <li>
                <button class:selected={tab === 'ERC1155'} on:click={() => tab = 'ERC1155'}>
                  ERC1155
                </button>
              </li>
              <li>
                <button class:selected={tab === 'Governor'} on:click={() => tab = 'Governor'}>
                  Governor
                </button>
              </li>
              <li>
                <button class:selected={tab === 'Custom'} on:click={() => tab = 'Custom'}>
                  Custom
                </button>
              </li>
            </ul>
            
      </div>

      <div class="action flex flex-row gap-2 shrink-0">

        <button class="action-button min-w-[165px]" on:click={copyHandler}>

          <div class="flex justify-between">
            {#if copied}
              <CheckIcon />Copied
            {:else}
              <CopyIcon />Copy to Clipboard
            {/if}

          </div>
        </button>

    
      </div>
    </div>

  </div>