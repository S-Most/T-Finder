<script>
	import Technieken from "../components/Technieken.svelte"
	let input = "";
	$: technieken = getTechnieken(input);

	async function getTechnieken(keyword) {
		const res = await fetch("/static/result.json");
		let technieken = await res.json();

		let newTechniques = []
		technieken = technieken
			.filter(techniek => techniek.technique_name.toUpperCase().includes(keyword.toUpperCase()))
			.sort((a, b) => 
				a.technique_name.localeCompare(b.technique_name) || 
				a.module_name.localeCompare(b.module_name) || 
				a.level - b.level)
			.filter(techniek => {
				const duplicateName = newTechniques.includes(techniek.technique_name);
				if (!duplicateName) {
					newTechniques.push(techniek.technique_name);
					return true;
				}
				return false;
			});
			
		if (res.ok) {
			return technieken;
		} else {
			throw new Error(technieken);
		}
	}
</script>
	
<h1>Jarvis Technieken:</h1>

<input bind:value={input} on:keyup={() => getTechnieken(input)}>

<Technieken {technieken}/>
