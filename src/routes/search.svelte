<script>
  import { GraphQLClient, gql } from "graphql-request";
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
          title
          year
          plot
          genres
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

    let foundMovies = data.results.length;

    if (foundMovies > 0) {
      loadedMovieCount += foundMovies;
      searchResults = searchResults.concat(data.results);
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

<section class="container mx-auto max-w-lg ">
  <Search on:search={handleSearch} />
</section>

<h2>Results</h2>

<!-- <input bind:value={searchVariables.year} placeholder="enter year">
<p>Hello {searchVariables.year || 'stranger'}!</p> -->
<!-- <form on:submit|preventDefault={onSubmit}>
  <label for="year">Year</label>
  <input name="year" id="year" type="number" bind:value={$movieQuery.year} />
  <button type="submit">Submit</button>
</form> -->
<div class="flex flex-wrap">
  {#each searchResults as movie}
    <MovieTile {...movie} />
  {/each}
</div>

{#if btnShowMoreVisible}
  <form on:submit|preventDefault={handleShowMore}>
    <button type="submit" class="px-5 py-2 font-semibold text-gray-100 transition-colors bg-gray-900 rounded-md hover:bg-gray-700">Show More</button>
  </form>
{/if}
