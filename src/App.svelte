<script>
  import "bootstrap/dist/css/bootstrap.min.css";
  import Stories from "./viewStories.svelte";
</script>

<body>
  <main>
    <h1>Hacker News</h1>
    <Stories />
  </main>
</body>

<style>
  body {
    background-color: #f4f4f4;
  }
  main {
    margin: 0 auto;
  }

  h1 {
    color: #001aff;
    text-transform: uppercase;
    font-size: 4rem;
    font-weight: 100;
    font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }

  @media screen and (max-width: 645px) {
    h1 {
      font-size: 2rem;
    }
  }
</style>
