<script>
  import { GraphQLClient, gql } from "graphql-request";
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
    //	alert(`answeredith "${variables.year}"`);
    searchMovies();  
  }

  let variables = {
    // yearGt: 2016,
    title: "the",
    year: 2010,
    yearGt: 2010,
  };

  let searchResults = [];

  async function searchMovies() {
    const endpoint =
      "https://eu-central-1.aws.realm.mongodb.com/api/client/v2.0/app/moviequerygraphql-roato/graphql";

    const graphQLClient = new GraphQLClient(endpoint, {
      headers: {
        apiKey:
          "60hswkvSMNXEE4AuNsL6xCPveFerg9ifW4dLUZLcAaLZzTUWiZTI8hVVcSNLaqXV",
      },
    });
     const query = gql`
        query getMovies($title: String, $year: Int) {
          results: search(input: {title: $title, year: $year}) {
          title,
          year
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

    let data = await graphQLClient.request(query, variables);

    if (data.results == null || data.results.length === 0) {
      console.log("No movies found.");
    }

    searchResults = data.results;
    console.log(data.results);

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
<h2>Search movies</h2>

<h1>Form</h1>

<form on:submit|preventDefault={handleSubmit}>
  <!-- <select bind:value={selected} on:change="{() => answer = ''}">
		{#each questions as question}
			<option value={question}>
				{question.text}
			</option>
		{/each}
	</select> -->

  <input bind:value={variables.title} />
  <input bind:value={variables.year} />

  <button type="submit">Submit</button>
</form>

<!-- <input bind:value={variables.year} placeholder="enter year">
<p>Hello {variables.year || 'stranger'}!</p> -->
<!-- <form on:submit|preventDefault={onSubmit}>
  <label for="year">Year</label>
  <input name="year" id="year" type="number" bind:value={$movieQuery.year} />
  <button type="submit">Submit</button>
</form> -->

{#each searchResults as movie}
  <p>{movie.title} {movie.year}</p>
{/each}

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
