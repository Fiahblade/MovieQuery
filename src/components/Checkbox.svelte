<!-- <script>
    export let label = "";
    export let bindGroup = [];
    export let value = "";

    function onChange({ target }) {
        const { value, checked } = target;
        if (checked) {
            bindGroup = [...bindGroup, value];
        } else {
            bindGroup = bindGroup.filter((item) => item !== value);
        }
    }
</script>

<label>
    <input
        type="checkbox"
        {value}
        checked={bindGroup.includes(value)}
        on:change={onChange}
    />
    {label}
</label> -->

<script>	
	export let value
	export let checked = false
</script>

<label>
  <input type="checkbox" value={value} bind:checked={checked}/>
  {value}
</label>
