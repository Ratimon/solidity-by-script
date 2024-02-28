<script  lang="ts">

    import Header from './Header.svelte';
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
    
  
  </script>
    
  <Header links={headLinks} class="bg-base-200 "></Header>
  <!-- <Footer links={footLinks}></Footer> -->