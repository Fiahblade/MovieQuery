<script>
  import Header from "../components/Header.svelte";
</script>

<Header />

<section class="bg-white dark:bg-gray-900">
  <div class="container px-6 py-10 mx-auto">
    <h1 class="text-3xl font-semibold text-center text-gray-800 lg:text-4xl dark:text-white capitalize">technology & software</h1>

    <div class="grid grid-cols-1 gap-8 mt-8 xl:mt-12 xl:gap-16 md:grid-cols-2 xl:grid-cols-3">
      <div class="flex flex-col items-center p-6 space-y-3 text-center bg-gray-100 rounded-xl dark:bg-gray-800">
        <h1 class="text-2xl font-semibold text-gray-700 capitalize dark:text-white">SvelteKit as a framework</h1>

        <p class="text-gray-500 dark:text-gray-300">SvelteKit is a framework for building web applications of all sizes, with a beautiful development experience and flexible filesystem-based routing.</p>
      </div>

      <div class="flex flex-col items-center p-6 space-y-3 text-center bg-gray-100 rounded-xl dark:bg-gray-800">
        <h1 class="text-2xl font-semibold text-gray-700 capitalize dark:text-white">GraphQL as query language</h1>

        <p class="text-gray-500 dark:text-gray-300">GraphQL is a query language for APIs and a runtime for fulfilling those queries with your existing data.</p>
      </div>

      <div class="flex flex-col items-center p-6 space-y-3 text-center bg-gray-100 rounded-xl dark:bg-gray-800">
        <h1 class="text-2xl font-semibold text-gray-700 capitalize dark:text-white">MongoDB for our data</h1>

        <p class="text-gray-500 dark:text-gray-300">MongoDB is a document database with the scalability and flexibility that you want with the querying and indexing that you need.</p>
      </div>

      <div class="flex flex-col items-center p-6 space-y-3 text-center bg-gray-100 rounded-xl dark:bg-gray-800">
        <h1 class="text-2xl font-semibold text-gray-700 capitalize dark:text-white">Tailwind CSS for rapid styling</h1>

        <p class="text-gray-500 dark:text-gray-300">A utility-first CSS framework packed with classes like flex, pt-4, text-center and rotate-90 that can be composed to build any design, directly in your markup.</p>
      </div>

      <div class="flex flex-col items-center p-6 space-y-3 text-center bg-gray-100 rounded-xl dark:bg-gray-800">
        <h1 class="text-2xl font-semibold text-gray-700 capitalize dark:text-white">Hosted by Cloudflare Pages</h1>

        <p class="text-gray-500 dark:text-gray-300">Cloudflare Pages is a JAMstack platform for frontend developers to collaborate and deploy websites.</p>
      </div>
      <div class="flex flex-col items-center p-6 space-y-3 text-center bg-gray-100 rounded-xl dark:bg-gray-800">
        <h1 class="text-2xl font-semibold text-gray-700 capitalize dark:text-white">Visual Studio Code</h1>

        <p class="text-gray-500 dark:text-gray-300">My precious.</p>
      </div>
    </div>
  </div>
</section>
