<script lang="ts">
  import marked from 'marked';
  import katex from 'katex';
  import { onMount, tick } from 'svelte';

  let markdown: String = '';

  onMount(async () => {
    try {
      const stream = await fetch('Content.md');
      const string = await stream.text();
      markdown = marked(await string);
      await tick();
      const latexElements = document.getElementsByClassName('language-tex');
      for (let i = 0; i < latexElements.length; i++) {
        latexElements[i].innerHTML = katex.renderToString(latexElements[i].innerHTML, {throwOnError: false});
      }
    } catch (e) {
      console.log(e);
    }
  });
</script>

<main>
  {@html markdown}
</main>

<svelte:head>
  <link
    rel="stylesheet"
    href="https://cdn.jsdelivr.net/npm/katex@0.12.0/dist/katex.min.css"
    integrity="sha384-AfEj0r4/OFrOo5t7NnNe46zW/tFgW6x/bCJG8FqQCEo3+Aro6EYUG4+cU+KJWu/X"
    crossorigin="anonymous"
  />
</svelte:head>

<style lang="scss">
  :global(body) {
    margin: 0;
    padding: 0;
  }
  main {
    // text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
