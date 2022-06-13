<script>
  import { GraphQLClient, gql } from "graphql-request";

  import { page } from "$app/stores";
  import Nav from "../../components/Nav.svelte";
  import Breadcrumbs from "../../components/Breadcrumbs.svelte";

  let movie = "";

  let errorText = "";
  LoadMovie().catch((error) => {
    console.error(error);
    errorText = "Movie not found";
  });

  async function LoadMovie() {
    const endpoint = "https://eu-central-1.aws.realm.mongodb.com/api/client/v2.0/app/moviequerygraphql-roato/graphql";

    const graphQLClient = new GraphQLClient(endpoint, {
      headers: {
        apiKey: import.meta.env.VITE_API_KEY,
      },
    });

    const query = gql`
      query getMovie($id: ObjectId) {
        movie(query: { _id: $id }) {
          title
          year

          fullplot
          poster
          rated
          cast
          released
          runtime
          type
          year
          directors
          genres
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
    console.log(movie);

    if (movie.poster === null) {
      movie.poster = "";
    }
  }

  function handleCoverError(e) {
    e.target.src = "../cover404.svg";
  }
</script>

<Nav />
<Breadcrumbs pageName={movie.title} pageLocation={$page.params.id} />

<section class="flex flex-col mx-auto max-w-lg text-center">
  <div class="flex-auto w-64 self-center">
    <img src={movie.poster} on:error={handleCoverError} alt="Movie poster of {movie.title}" class="object-cover object-center w-full aspect-auto rounded-lg" />
  </div>
  <div class="mt-1">
    <h2 class="font-semibold text-xl">{movie.title}</h2>
    <span class="block text-xs font-medium tracking-widest italic ">{movie.year}</span>
  </div>

  <div class="mt-4 italic">
    <p>{movie.fullplot}</p>
  </div>
  <div class="mt-4">
    {#if movie.directors}
      <h3 class="font-semibold text-lg capitalize">director</h3>
      {#each movie.directors as director}
        <p>{director} </p>
      {/each}
    {/if}

    {#if movie.cast}
      <h3 class="font-semibold text-lg capitalize">cast</h3>
      {#each movie.cast as castMember}
        <p>{castMember}</p>
      {/each}
    {/if}

    {#if movie.genres}
      <h3 class="font-semibold text-lg capitalize">genres</h3>
      {#each movie.genres as genre}
        <p>{genre}</p>
      {/each}
    {/if}

    {#if movie.runtime}
      <h3 class="font-semibold text-lg capitalize">runtime</h3>
      <p>{movie.runtime} minutes</p>
    {/if}

    {#if movie.rated}
      <h3 class="font-semibold text-lg capitalize">rated</h3>
      <p>{movie.rated}</p>
    {/if}
  </div>
</section>
