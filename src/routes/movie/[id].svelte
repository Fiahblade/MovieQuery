<script>
  import { GraphQLClient, gql } from "graphql-request";
  import { page } from "$app/stores";
  import MovieTile from "../../components/MovieTile.svelte";
  import Nav from "../../components/Nav.svelte";

  let movie = "";

  let errorText = "";
  LoadMovie().catch((error) => {
    console.error(error);
    errorText = "Movie not found";
  });

  async function LoadMovie() {
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
      query getMovie($id: ObjectId) {
        movie(query: { _id: $id }) {
          title
          year
          plot
          fullplot
          poster
          rated
          released
          runtime
          type
          year
        }
      }
    `;

    let parameters = {
      id: $page.params.id,
    };

    let data = await graphQLClient.request(query, parameters);

    if (data.movie === null) {
      errorText = "Movie not found";
      return;
    }

    movie = data.movie;

    if (movie.poster === null) {
      movie.poster = "";
    }
  }

  function handleCoverError(e) {
    e.target.src = "../cover404.svg";
  }
</script>

<Nav />

{#if movie}
  <section class="flex flex-wrap gap-4 place-content-center mt-4 sm:mt-8 lg:mt-12">
    <img src={movie.poster} on:error={handleCoverError} alt="Movie poster of {movie.title}" class="object-cover object-center w-full rounded-md h-72" />
    <div class="m-2">
      <h2 class="font-semibold">{movie.title}</h2>
      <span class="block text-xs font-medium tracking-widest italic">{movie.year}</span>
    </div>
  </section>
{:else}
  <p>{errorText}</p>
{/if}
