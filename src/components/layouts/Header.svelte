<script>
    import Nav from "../../components/Nav.svelte";
</script>

<section>
    <Nav />

    <div class="container px-6 py-16 mx-auto text-center">
        <div class="max-w-lg mx-auto">
            <h1 class="text-3xl font-bold text-gray-800 dark:text-white md:text-4xl">Simple Movie Searcher made with GraphQL.</h1>
            <p class="my-3 text-gray-500 dark:text-gray-300">A Web Topics Advanced 2022 Project by Robin.</p>
            <a href="/search" class="px-6 py-2 text-sm font-medium leading-5 text-center text-white capitalize bg-blue-600 rounded-lg hover:bg-blue-500 md:mx-0 md:w-auto focus:outline-none">Start searching 🔎</a>
        </div>


        <div class="flex space-x-4 justify-center mt-10">
            <div class="w-48">
                <img
                    src="./coverMovie1.jpg"
                    alt="Movie poster of Toy Story 2"
                    class="object-cover object-center w-full rounded-md h-72"
                />
                <div class="m-2">
                    <h2 class="font-semibold">Toy Story 2</h2>
                    <span class="block text-xs font-medium tracking-widest italic">1999</span>
                </div>
            </div>
            <div target="_blank" class="w-48">
                <img src="./coverMovie2.jpg" alt="Movie poster of Dredd" class="object-cover object-center w-full rounded-md h-72" />
                <div class="m-2">
                    <h2 class="font-semibold">Dredd</h2>
                    <span class="block text-xs font-medium tracking-widest italic">2012</span>
                </div>
            </div>
            <div target="_blank" class="w-48">
                <img src="./coverMovie3.jpg" alt="Movie poster of 127 Hours" class="object-cover object-center w-full rounded-md h-72" />
                <div class="m-2">
                    <h2 class="font-semibold">127 Hours</h2>
                    <span class="block text-xs font-medium tracking-widest italic">2010</span>
                </div>
            </div>
        </div>
    </div>
</section>
