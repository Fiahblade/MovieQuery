<script>
	export let title;
	export let value;
	export let placeholder;

	function onType(e) {
		// if (e.data == "+" || e.data == "-") {
		// 	e.preventDefault();
		// 	value = e.target.value;
		// 	return;
		// }

		if (isNaN(e.data)) {
			e.preventDefault();
			value = e.target.value;
			return;
		}

		console.log(5);
	}
</script>

<label class="text-gray-700 dark:text-gray-200 capitalize" for={title}>{title}</label>
<input bind:value on:input|preventDefault={onType} id={title} type="number" min="1900" max="2100" step="1" class="w-full form-input" {placeholder} />
<!-- <input bind:value on:input={onType} id={title} type="text"  pattern="[0-9\.]+" class="w-full form-input" {placeholder} /> -->
