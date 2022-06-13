<script context="module">
    export function load({ error, status }) {
        return {
            props: {
                message: error.message,
                status: status,
            },
        };
    }
</script>

<script>
    import Error from "../components/layouts/Error.svelte";

    export let message;
    export let status;
</script>

<Error {message} {status} />
