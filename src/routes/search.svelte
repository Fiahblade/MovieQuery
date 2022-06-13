<script>
  import { GraphQLClient, gql } from "graphql-request";
  import { Modals, closeModal } from "svelte-modals";
  import MovieTile from "../components/MovieTile.svelte";
  import Nav from "../components/Nav.svelte";
  import Search from "../components/SearchFilter.svelte";

  let filterOptions = {};

  let pagination = {
    skip: 0,
    limit: 20,
  };

  let btnShowMoreVisible = false;
  let loadedMovieCount = 0;
  let searchResults = [];
  let errorMessage = "";

  function handleSearch(event) {
    filterOptions = event.detail;

    pagination.skip = 0;
    loadedMovieCount = 0;
    searchResults = [];

    searchMovies().catch((error) => {
      console.error(error);
      errorMessage = "Fetching Data...";
    });
  }

  function handleShowMore() {
    pagination.skip = loadedMovieCount;
    searchMovies().catch((error) => console.error(error));
  }

  async function searchMovies() {
    const endpoint = "https://eu-central-1.aws.realm.mongodb.com/api/client/v2.0/app/moviequerygraphql-roato/graphql";

    const graphQLClient = new GraphQLClient(endpoint, {
      headers: {
        apiKey: import.meta.env.VITE_API_KEY,
      },
    });

    const query = gql`
      query getMovies($title: String, $plot: String, $year: Int, $genres: String, $pagination: PaginationInput) {
        results: search(input: { title: $title, plot: $plot, year: $year, genres: $genres, pagination: $pagination }) {
          _id
          title
          year
          poster
        }
      }
    `;

    filterOptions.pagination = pagination;

    let data = await graphQLClient.request(query, filterOptions);

    if (data.results == null || data.results.length === 0) {
      errorMessage = "No movies found.";
      console.log("No movies found.");
    }

    let foundMovies = data.results.length;
    if (foundMovies > 0) {
      loadedMovieCount += foundMovies;

      data.results.forEach((movie) => {
        if (movie.poster === null) {
          movie.poster = "";
        }

        searchResults.push(movie);
      });

      // weird svelte hack to refresh
      searchResults = searchResults;
    }

    if (foundMovies < pagination.limit) {
      btnShowMoreVisible = false;
    } else {
      btnShowMoreVisible = foundMovies > 0;
    }
  }
</script>

<Nav />

<section class="grid mx-auto max-w-lg">
  <Search on:search={handleSearch} />
</section>

<section class="flex flex-wrap gap-4 place-content-center mt-4 sm:mt-8 lg:mt-12">
  {#each searchResults as movie (movie._id)}
    <MovieTile {...movie} />
  {:else}
    <h3>{errorMessage}</h3>
  {/each}
</section>

<section class="grid place-content-center my-5">
  {#if btnShowMoreVisible}
    <form on:submit|preventDefault={handleShowMore}>
      <button type="submit" class="px-5 py-2 font-semibold text-gray-100 transition-colors bg-gray-900 rounded-md hover:bg-gray-700">Show More</button>
    </form>
  {/if}
</section>

<Modals>
  <div slot="backdrop" class="backdrop" on:click={closeModal} />
</Modals>

<style>
  .backdrop {
    position: fixed;
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    background: rgba(0, 0, 0, 0.5);
  }
</style>
