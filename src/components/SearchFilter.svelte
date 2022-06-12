<script>
  import { createEventDispatcher } from "svelte";
  import Checkbox from "../components/Checkbox.svelte";
  import Textbox from "./Textbox.svelte";
  import Button from "../components/Button.svelte";

  const dispatch = createEventDispatcher();
  function search() {
    // if year empty set to default value (null)
    if (filterOptions.year === null || filterOptions.year.length === 0) {
      filterOptions.year = null;
    }

    // if year empty set to default value (null)
    let formatedSelectedGenres = genres.filter((o, i) => selectedGenres[i]).toString();

    if (formatedSelectedGenres === null || formatedSelectedGenres.length === 0) {
      filterOptions.genres = null;
    } else {
      // fill in genres
      filterOptions.genres = formatedSelectedGenres;
    }

    console.log(filterOptions);

    dispatch("search", filterOptions);
  }

  function reset() {
    filterOptions = {
      title: "",
      plot: "",
      year: null,
      genres: null,
    };
  }

  let filterOptions = {
    title: "story",
    plot: "toy",
    year: 2010,
    genres: null,
  };

  const genres = [
    "Action",
    "Adventure",
    "Animation",
    "Biography",
    "Comedy",
    "Crime",
    "Documentary",
    "Drama",
    "Family",
    "Fantasy",
    "History",
    "Horror",
    "Mystery",
    "Music",
    "Musical",
    "Romance",
    "Sci-Fi",
    "Short",
    "Sport",
    "Thriller",
    "War",
    "Western",
  ];

  let selectedGenres = genres.map(() => false);

  let expanded = false;
</script>

<form on:submit|preventDefault={search} on:reset|preventDefault={reset}>
  <Textbox bind:value={filterOptions.title} title="name" placeholder="What movie are you searching for?" />

  {#if expanded}
    <Textbox bind:value={filterOptions.plot} title="plot" placeholder="What's the movie about?" />
    <Textbox bind:value={filterOptions.year} title="release year" placeholder="What year is it from?" />

    <label class="text-gray-700 dark:text-gray-200 capitalize" for="">genres</label>
    <div class="grid grid-cols-3 gap-1">
      {#each genres as genre, i}
        <div><Checkbox value={genre} bind:checked={selectedGenres[i]} /></div>
      {/each}
    </div>
  {/if}
  <div class="flex justify-center">
    <button type="button" on:click={() => (expanded = !expanded)} class="mt-2 px-5 py-2 font-semibold text-blue-600 capitalize rounded-md hover:text-gray-500">{expanded ? "less" : "more"} options</button>
    <button type="reset" class="mt-2 px-5 py-2 font-semibold text-red-500 capitalize rounded-md hover:text-red-700">reset query</button>
  </div>

  <button type="submit" class="w-full mt-2 px-5 py-2 font-semibold text-white capitalize bg-blue-600 rounded-lg hover:bg-blue-500 ">search 🔎</button>
</form>

<!-- <div>
  <p> 
    <i>
      You searching for content with "{filterOptions.title}" with mentions of
      "{filterOptions.plot}" in the year: "{filterOptions.year}" with
      genre(s): "{genres.filter((o, i) => selectedGenres[i])}".
    </i>
  </p>
</div> -->
