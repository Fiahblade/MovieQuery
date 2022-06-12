<script>
	export let title;
	export let placeholder;
	export let value;
</script>

<label class="text-gray-700 dark:text-gray-200 capitalize" for={title}>{title}</label>
<input bind:value id={title} type="text" class="w-full my-1 py-3 pl-10 pr-4 text-gray-700 bg-white border rounded-md dark:bg-gray-800 dark:text-gray-300 dark:border-gray-600 focus:border-blue-500 dark:focus:border-blue-500 focus:outline-none focus:ring" {placeholder} />
