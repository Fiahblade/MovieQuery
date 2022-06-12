<script>
	export let title;
	export let value;
	export let placeholder;
</script>

<label class="text-gray-700 dark:text-gray-200 capitalize" for={title}>{title}</label>
<input bind:value id={title} type="text" class="w-full form-input" {placeholder} />
