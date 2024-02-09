<script>
	export let type;
	export let name;
	export let value;
	export let hideLabel = false;
	export let ctx = normalize(value);

    $: console.log({ctx});

	function normalize(obj){
        // Remove types from the key names so that they can be easily used in expressions
		// Check if the input is an object
        if (typeof obj !== 'object' || obj === null) {
            return obj;
        }

        // Iterate over the object keys
        return Object.keys(obj).reduce((newObj, key) => {
            // Convert key to uppercase
            const normalizedKey = get_type_and_name(key).name;

            // If the value is an object, recursively call the function
            const value = obj[key];
            newObj[normalizedKey] = typeof value === 'object' && value !== null ? normalize(value) : value;

            return newObj;
        }, Array.isArray(obj) ? [] : {}); // Preserve array structure if the original object is an array
	}
	
	function get_type_and_name(key){
		if(key.includes(".")){
			return { type: key.split(".")[0], name: key.split(".")[1]};
		} else {
			return { type: 'str', name: key};
		}
	}

	function exprEval(expr, ctx){
        console.log({expr}, {ctx});
		const func = new Function(...Object.keys(ctx), `return ${expr};`);
		return func(...Object.values(ctx));
	}
</script>

{#if type == 'map'}
	{hideLabel ? '' : name}
	{#each Object.keys(value) as key}
		<svelte:self 
			type={get_type_and_name(key).type} 
			name={get_type_and_name(key).name} 
            {ctx}
			value={value[key]} />
		<br/>
	{/each}
{:else if type == 'table'}
	<sp-table density="compact" emphasized >
		<sp-table-head>
			{#each value[0] as column}
				<sp-table-head-cell>{get_type_and_name(column).name}</sp-table-head-cell>
			{/each}
		</sp-table-head>
		<sp-table-body>
			{#each value.slice(1) as row}
            <sp-table-row>
				{#each row as cell, index}
                    <sp-table-cell>
						<svelte:self
							hideLabel={true}
							type={get_type_and_name(value[0][index]).type} 
							name={get_type_and_name(value[0][index]).name} 
                            {ctx}
							value={cell} />
                    </sp-table-cell>
				{/each}
            </sp-table-row>
			{/each}
		</sp-table-body>
	</sp-table>
{:else if type == 'list'}
	{get_type_and_name(name).name}
	{#each value as item}
    <sl-input value={item}></sl-input>
	{/each}
{:else if type == 'str'}
	<sl-input clearable filled label={hideLabel ? '' : name} {value}></sl-input>
{:else if type == 'int'}
	<sl-input clearable filled type="number" label={hideLabel ? '' : name} {value}></sl-input>
{:else if type == 'expr'}
    <sl-input readonly label={hideLabel ? '' : name} value={exprEval(value, ctx)}></sl-input>
{/if}
