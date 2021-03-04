<script>
  import tryCatch from "./api";
  import { onMount } from "svelte";
  import { hackerNewsTopStoryUrls, hackerNewsDetails } from "./stores";
  import "bootstrap/dist/css/bootstrap.min.css";
  import { Spinner } from "sveltestrap";
  const getStoryUrls = async () => {
    let urls = [];
    let top30 = (
      await tryCatch(
        "https://hacker-news.firebaseio.com/v0/topstories.json?print=pretty"
      )
    ).slice(0, 30);
    top30.forEach((id) =>
      urls.push(`https://hacker-news.firebaseio.com/v0/item/${id}.json`)
    );
    hackerNewsTopStoryUrls.set(urls);
    return urls;
  };
  const setStoryInfo = async () => {
    let urls = await getStoryUrls();
    let details = await Promise.all(
      urls.map((url) => fetch(url).then((response) => response.json()))
    );
    hackerNewsDetails.set(details);
  };
  onMount(setStoryInfo);
</script>

<div>
  <h1>Top 30 Stories</h1>
  {#await $hackerNewsDetails}
    <div>
      <Spinner color="primary" />
    
    <p>Awaiting details...</p>
</div>
  {:then storyObject}
    <div class="grids">
      {#each storyObject as detail, index (detail.id)}
        <div class="aCard">
          <div class="flexedHeader">
            <a target="_blank" href={detail.url}>{index + 1}. {detail.title} </a>
            <a target="_blank" class="refURL" href={detail.url}>({detail.url}</a>
          </div>
          <div class="score">
            <p>
                Score: {detail.score} | by {detail.by}
            </p>
          </div>
        </div>
      {/each}
    </div>
  {/await}
</div>

<style>

    .grids{
        display: grid;
        grid-template-columns: 1fr; 
        gap: 15px;
        margin: 0 auto 1rem auto;
        justify-items: center;
        padding-bottom: 2rem;
    }
    .aCard{
        border: 1px rgba(128, 128, 128, 0.657) solid;
        border-radius: 10px;
        width: 80vw;
    }
    .aCard p {
        margin: 0;
    }
  .flexedHeader {
    align-items: left;
    border-radius: 10px 10px 0 0;
    padding: .25rem;
    background-color: rgb(8, 105, 189);
    border-bottom: 2px rgba(128, 128, 128, 0.589) solid;
  }
  .flexedHeader a{
      color: #f2f2f2;
  }
  .refURL {
    font-size: 0.7rem;
  }
  .score {
    padding: .25rem;
  }
  @media screen and (max-width: 900px){
     .grids{
         grid-template-columns: 1fr;
     }
     .aCard{
         width: 90vw;
     }
  }

  @media screen and (max-width: 645px) {
    h1 {
      font-size: 1.5rem;
    }
    a, p{
        font-size: .75rem;
    }
    .refURL{
        font-size: .5rem;
    }
  }
</style>
