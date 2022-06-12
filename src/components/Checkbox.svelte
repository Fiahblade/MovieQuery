<script>
  export let value;
  export let checked = false;
</script>

<label for="genre_{value.toLowerCase()}">
  <input bind:checked id="genre_{value.toLowerCase()}" type="checkbox" {value} />
  {value}
</label>
