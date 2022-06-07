<script>
  import { GraphQLClient, gql } from "graphql-request";
  import Checkbox from "../components/Checkbox.svelte";
  import { writable } from "svelte/store";

  //https://realm.mongodb.com/groups/6299d27491c275417bbbac89/apps/6299d4df2f932d4c6d81e5c2/graphql/explore

  // function onSubmit(e) {
  //   const formData = new FormData(e.target);

  //   // const data = {};
  //   // for (let field of formData) {
  //   //   const [key, value] = field;
  //   //   data[key] = value;
  //   // }
  //   // console.log(data)

  //   console.log(formData)
  // }

  // export const movieQuery = writable({
  //   year: 2011,
  //   year_gt: 0
  // });

  function handleSubmit() {
    //	alert(`answeredith "${searchVariables.year}"`);
    searchVariables.pagination.skip = 0;
    loadedMovieCount = 0;
    searchResults = [];
    searchMovies();
  }

  function handleShowMore() {
    //	alert(`answeredith "${searchVariables.year}"`);
    searchVariables.pagination.skip = loadedMovieCount;
    searchMovies();
  }

  let btnShowMoreVisible = false;

  let genres = [
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

  let searchVariables = {
    // yearGt: 2016,
    title: "story",
    plot: "toy",
    year: 2010,
    genres: null,
    pagination: {
      skip: 0,
      limit: 20,
    },
    //   yearGt: 2010,
  };

  let loadedMovieCount = 0;
  let searchResults = [];

  async function searchMovies() {
    const endpoint =
      "https://eu-central-1.aws.realm.mongodb.com/api/client/v2.0/app/moviequerygraphql-roato/graphql";

    //todo put this apikey in an env variable
    const graphQLClient = new GraphQLClient(endpoint, {
      headers: {
        apiKey:
          "60hswkvSMNXEE4AuNsL6xCPveFerg9ifW4dLUZLcAaLZzTUWiZTI8hVVcSNLaqXV",
      },
    });

    // doel zoeken op titel, plot, jaar & genre
    // leuk extra: vanaf jaar, voor jaar
    const query = gql`
      query getMovies(
        $title: String
        $plot: String
        $year: Int
        $genres: String
        $pagination: PaginationInput
      ) {
        results: search(
          input: {
            title: $title
            plot: $plot
            year: $year
            genres: $genres
            pagination: $pagination
          }
        ) {
          title
          year
          genres
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

    // if year empty set to default value (null)
    if (searchVariables.year === null || searchVariables.year.length === 0) {
      searchVariables.year = null;
    }

    // if year empty set to default value (null)
    let formatedSelectedGenres = genres
      .filter((o, i) => selectedGenres[i])
      .toString();

    if (formatedSelectedGenres === null || formatedSelectedGenres.length === 0) {
      searchVariables.genres = null;
    } else {
      // fill in genres
      searchVariables.genres = formatedSelectedGenres;
    }

    console.log(searchVariables);

    let data = await graphQLClient.request(query, searchVariables);

    if (data.results == null || data.results.length === 0) {
      console.log("No movies found.");
    }

    let foundMovies = data.results.length;

    if (foundMovies > 0) {
      loadedMovieCount += foundMovies;
      searchResults = searchResults.concat(data.results);
    }

    if (foundMovies < searchVariables.pagination.limit) {
      btnShowMoreVisible = false;
    } else {
      btnShowMoreVisible = foundMovies > 0;
    }


    //console.log(data.results);
    console.log(loadedMovieCount);
    console.log(btnShowMoreVisible);

    //console.log(JSON.stringify(data, undefined, 2))
  }

  async function getAllMovies() {
    const endpoint =
      "https://eu-central-1.aws.realm.mongodb.com/api/client/v2.0/app/moviequerygraphql-roato/graphql";

    const graphQLClient = new GraphQLClient(endpoint, {
      headers: {
        apiKey:
          "60hswkvSMNXEE4AuNsL6xCPveFerg9ifW4dLUZLcAaLZzTUWiZTI8hVVcSNLaqXV",
      },
    });

    const query = gql`
      {
        movies {
          _id
          cast
          countries
          directors
          fullplot
          genres
          languages
          lastupdated
          metacritic
          num_mflix_comments
          plot
          poster
          rated
          released
          runtime
          title
          type
          writers
          year
        }
      }
    `;

    let data = await graphQLClient.request(query);

    return data.movies;

    //console.log(JSON.stringify(data, undefined, 2))
  }

  //let movies = searchMovies();

  //main().catch((error) => console.error(error))
</script>

<h1>MovieQuery</h1>
<h2>Search</h2>

<form on:submit|preventDefault={handleSubmit}>
  <input bind:value={searchVariables.title} />
  <input bind:value={searchVariables.plot} />
  <input bind:value={searchVariables.year} />
  <hr />

  {#each genres as genre, i}
    <Checkbox value={genre} bind:checked={selectedGenres[i]} />
  {/each}
  <hr />

  <button type="submit">Submit</button>
</form>

<div>
  <p>
    <i>
      You searching for content with "{searchVariables.title}" with mentions of
      "{searchVariables.plot}" in the year: "{searchVariables.year}" with
      genre(s): "{genres.filter((o, i) => selectedGenres[i])}".
    </i>
  </p>
</div>
<h2>Results</h2>

<!-- <input bind:value={searchVariables.year} placeholder="enter year">
<p>Hello {searchVariables.year || 'stranger'}!</p> -->
<!-- <form on:submit|preventDefault={onSubmit}>
  <label for="year">Year</label>
  <input name="year" id="year" type="number" bind:value={$movieQuery.year} />
  <button type="submit">Submit</button>
</form> -->

{#each searchResults as movie}
  <p>{movie.title} {movie.year} - <small>{movie.genres}</small></p>
{/each}

{#if btnShowMoreVisible}
  <form on:submit|preventDefault={handleShowMore}>
    <button type="submit">Show More</button>
  </form>
{/if}

<!-- <h2>All movies</h2>
{#await getAllMovies()}
  <p>.. loading</p>
{:then movies}
  {#each movies as movie, i}
    <p>{movie.title}</p>
  {/each}
{:catch e}
  {e}
{/await} -->
<style>
  .hidden {
    opacity: 0;
    visibility: hidden;
  }
  .btnShowMore.expanded {
    opacity: 1;
    visibility: visible;
  }
</style>
