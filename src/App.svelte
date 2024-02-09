<script>
  import svelteLogo from './assets/svelte.svg'
  import viteLogo from '/vite.svg'
  import Counter from './lib/Counter.svelte'
  import TRON from './lib/TRON.svelte'

  let data = {
    "name": "john", // missing type defaults to string as single-line textinput
    "int.age": 21,
    "expr.bio": '(name + age).toUpperCase()',
    "table.jobs": [
        ["str.company", "int.year"],
				["google", 2004],
				["whatsapp", 2024]
    ],
    "list.education": [3, false, "hello"], // items can be of different types
    "map.tags": {'int.foo': 23}
	};
</script>

<main>
  <h2>TRON: Typed Reactive Object Notation</h2>
  <p>TRON is the "spreadsheet" (really a malleable miniapp) format we really wanted:</p>
  
  <sl-tree>
    <sl-tree-item>It's just a static file (you can send via email or store it in GDrive)</sl-tree-item>
    <sl-tree-item>It's easily parseable: It's just a plain-text JSON</sl-tree-item>
    <sl-tree-item>It's typed: Prevents the biggest problem with spreadsheets: Users entering invalid data. TRON's types even support unit conversion.</sl-tree-item>
    <sl-tree-item>It's reactive: Build and send tweakable mini-apps</sl-tree-item>
    <sl-tree-item>It's user-friendly: Rich collection of types and their editing widgets</sl-tree-item>
    <sl-tree-item>It's developer-friendly: Formulas just use Javascript. No need to learn a custom scripting language.</sl-tree-item>
  </sl-tree>

  <sl-divider></sl-divider>

  <sl-split-panel disabled>
    <div
      slot="start"
      style="padding: 2em"
    >
      <h3>Here is a raw .tron file:</h3>
      <sl-textarea filled  readonly resize="auto" value={JSON.stringify(data, null, 2)}></sl-textarea>
    </div>
    <div
      slot="end"
      style="padding: 2em"
    >
      <h3>And here is the reactive document/miniapp users see:</h3>
      <sp-theme scale="large" color="light">
        <TRON hideLabel={true} type='map' name='data' value={data} />
      </sp-theme>
    </div>
  </sl-split-panel>
  

</main>


