<script>
  import { GraphQLClient, gql } from "graphql-request";
  import { closeModal } from "svelte-modals";

  export let isOpen;
  
  export let id;
  export let title;
  export let year;

  let movie = {
    title: "",
    fullplot: "",
    cast: [],
  };
  LoadMovie().catch((error) => console.error(error));

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
          fullplot
          cast
        }
      }
    `;

    let parameters = {
      id: id,
    };

    let data = await graphQLClient.request(query, parameters);
    movie = data.movie;
  }
</script>

{#if isOpen}
  <div role="dialog" class="modal">
    <div class="relative p-4 w-full max-w-2xl h-full md:h-auto contents">
      <div class="relative bg-gray-600 shadow rounded-lg text-white">
        <div class="flex justify-between items-start p-4 rounded-t border-b">
          <h3 class="text-xl font-semibold ">{title} - {year}</h3>
          <button on:click={closeModal} type="button" class="text-white bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm p-1.5 ml-auto inline-flex items-center">
            <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
              <path
                fill-rule="evenodd"
                d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
                clip-rule="evenodd"
              />
            </svg>
          </button>
        </div>

        <div class="p-6 space-y-6">
          <p class="text-base leading-relaxed ">
            {movie.fullplot}
          </p>
          <p class="text-base leading-relaxed ">
            {#if movie.cast}
              <p>
                Cast:
                {#each movie.cast as castMember}
                  <span>{castMember} </span>
                {/each}
              </p>
            {/if}
          </p>
        </div>

        <div class="flex items-center p-6 space-x-2 rounded-b border-t border-gray-200">
          <a href="movie/{id}" target="_blank" class=" bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center">More info</a>
        </div>
      </div>
    </div>
  </div>
{/if}

<style>
  .modal {
    position: fixed;
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    pointer-events: none;
  }

  .contents {
    display: flex;
    pointer-events: auto;
  }
</style>
