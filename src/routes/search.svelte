<script>
  import { GraphQLClient, gql } from "graphql-request";
  import { Modals, closeModal } from 'svelte-modals'
  import MovieTile from "../components/MovieTile.svelte";
  import Nav from "../components/Nav.svelte";
  import Search from "../components/SearchFilter.svelte";

  let filterOptions = {};

  let pagination = {
    skip: 0,
    limit: 20,
  };

  function handleSearch(event) {
    filterOptions = event.detail;

    pagination.skip = 0;
    loadedMovieCount = 0;
    searchResults = [];

    searchMovies();
  }

  function handleShowMore() {
    pagination.skip = loadedMovieCount;
    searchMovies();
  }

  let btnShowMoreVisible = false;

  let loadedMovieCount = 0;
  let searchResults = [];

  async function searchMovies() {
    const endpoint = "https://eu-central-1.aws.realm.mongodb.com/api/client/v2.0/app/moviequerygraphql-roato/graphql";

    //todo put this apikey in an env variable
    const graphQLClient = new GraphQLClient(endpoint, {
      headers: {
        apiKey: "60hswkvSMNXEE4AuNsL6xCPveFerg9ifW4dLUZLcAaLZzTUWiZTI8hVVcSNLaqXV",
      },
    });

    // doel zoeken op titel, plot, jaar & genre
    // leuk extra: vanaf jaar, voor jaar
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

    // works
    //  const query = gql`
    //    query {
    //       results: search(input: {title: "the", year: 2002}) {
    //       title,
    //       year
    //     }
    //    }
    //  `;

    //  const query = gql`
    //    query getMovies( $year: Int, $yearGt: Int) {
    //      movies(query: { year: $year, year_gt: $yearGt }) {
    //        title
    //        year
    //      }
    //     titleContains(input:"the") {
    //       title
    //     }
    //    }
    //  `;
    // const query = gql`
    //   query getMovies($title: String, $year: Int, $yearGt: Int) {
    //     titleContains: "test" {
    //       title
    //       year
    //     }
    //   }
    // `;

    console.log(pagination);

    filterOptions.pagination = pagination;
    console.log(filterOptions.pagination);

    let data = await graphQLClient.request(query, filterOptions);

    if (data.results == null || data.results.length === 0) {
      console.log("No movies found.");
    }
      console.log(data.results);

    let foundMovies = data.results.length;
    if (foundMovies > 0) {
      loadedMovieCount += foundMovies;

      data.results.forEach((movie) => {
        if (movie.poster === null) {
          movie.poster = "cover404.svg";
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

    /// console.log(searchResults);
    console.log(loadedMovieCount);
    console.log(btnShowMoreVisible);

    //console.log(JSON.stringify(data, undefined, 2))
  }

  //main().catch((error) => console.error(error))
</script>

<Nav />

<section class="grid mx-auto max-w-lg">
  <Search on:search={handleSearch} />
</section>

<!-- <section class="grid grid-cols-4 gap-4 p-6"> -->
<section class="flex flex-wrap gap-4 place-content-center mt-4 sm:mt-8 lg:mt-12">
  {#each searchResults as movie}
    <MovieTile {...movie} />
  {:else}
    <h3>Fetching Data...</h3>
  {/each}
  <!-- {#each Array(9) as _, i}
    <a href="">
      <div class="rounded-sm">
        <img src="https://m.media-amazon.com/images/M/MV5BMjE1MDU1MDA2Nl5BMl5BanBnXkFtZTcwNTQ2Mzk2NQ@@._V1_SY1000_SX677_AL_.jpg" alt="" class="object-cover object-center w-full rounded-md h-72 dark:bg-gray-500" />
        <div class="m-2">
          <h2 class="font-semibold">!Women Art Revolution 1</h2>
          <span class="block text-xs font-medium tracking-widest italic dark:text-violet-400">2010</span>
        </div>
      </div>
    </a>
  {/each} -->
</section>

<section class="grid place-content-center my-5">
  {#if btnShowMoreVisible}
    <form on:submit|preventDefault={handleShowMore}>
      <button type="submit" class="px-5 py-2 font-semibold text-gray-100 transition-colors bg-gray-900 rounded-md hover:bg-gray-700">Show More</button>
    </form>
  {/if}
</section>


<Modals>
  <div
    slot="backdrop"
    class="backdrop"
    on:click={closeModal}
  />
</Modals>

<style>
  .backdrop {
    position: fixed;
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    background: rgba(0,0,0,0.50);
  }
</style>