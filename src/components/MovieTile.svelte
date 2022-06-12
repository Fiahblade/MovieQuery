<script>
    import { openModal } from "svelte-modals";
    import MoreDetailsModal from "./MoreDetailsModal.svelte";

    export let _id;
    export let title;
    export let year;
    export let poster;

    function showMovieDetails() {
        openModal(MoreDetailsModal, { id:_id });
    }
</script>

<button on:click={showMovieDetails} target="_blank" class="w-48">
    <img src={poster} alt="Movie poster of {title}" class="object-cover object-center w-full rounded-md h-72" />
    <div class="m-2">
        <h2 class="font-semibold">{title}</h2>
        <span class="block text-xs font-medium tracking-widest italic">{year}</span>
    </div>
</button>
