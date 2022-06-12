<script>
    import Nav from "../components/Nav.svelte";
</script>

<section>
    <Nav />

    <div class="container px-6 py-16 mx-auto text-center">
        <div class="max-w-lg mx-auto">
            <h1 class="text-3xl font-bold text-gray-800 dark:text-white md:text-4xl">Simple Movie Searcher made with GraphQL.</h1>
            <p class="my-3 text-gray-500 dark:text-gray-300">A Web Topics Advanced 2022 Project by Robin.</p>
            <a href="/search" class="px-6 py-2 text-sm font-medium leading-5 text-center text-white capitalize bg-blue-600 rounded-lg hover:bg-blue-500 md:mx-0 md:w-auto focus:outline-none">Start searching 🔎</a>
        </div>

        <div class="flex justify-center mt-10">
            <div class="w-full h-64 bg-blue-600 rounded-xl md:w-4/5" />
        </div>
    </div>
</section>
