<script>

    let texto;
    let requestimagen;
    let promise = Promise.resolve([]); //crea una promesa que luego se reemplaza con el valor asignado

    async function fetchtext () {
        const response= await fetch("https://api.openai.com/v1/completions", {
            method: "POST",
        headers:{  
            Authorization: `Bearer ${import.meta.env.VITE_API_KEY}`, //importando por medio de VITE
            "Content-Type": "application/json"},

            body: JSON.stringify({
                model: "text-davinci-003",
                prompt: texto,
                max_tokens: 100,
                temperature: 0.8
        })
    })

    const datos= await response.json()
    const data= datos["choices"][0]["text"]
    console.log(data)

    if (response.ok) {
        return data
    } else {
        throw new Error("error")
    }

    };


    function handleclick() {
        promise=fetchtext()
    }
    
    async function fetchimagenes() {
        const res= await fetch("https://api.openai.com/v1/images/generations", {
            method: "POST",
            headers: {
                Authorization: `Bearer ${API_KEY}`, 
            "Content-Type": "application/json"},

            body: JSON.stringify({
                prompt: requestimagen,
                n:2,
                size:"1024x1024"
            })
        })

        const img = res.json();
        console.log(img)
    }

</script>


<div class="flex justify-center sm:py-10">
    <h1 class="text-5xl font-serif">Preguntale a la IA</h1>
</div>

<div class="flex flex-col gap-y-10 justify-center items-center mt-10">
<h2 class="text-3xl text-slate-700"> Que te gustaria saber</h2>
<input id="texto" placeholder="prompt" bind:value={texto}> 
<button type="button" on:click={handleclick} class="shadow bg-purple-500 hover:bg-purple-400 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded">
	Preguntale a gpt
</button>
</div>


<br>

<input id="imagen" placeholder="imagen" bind:value={requestimagen}> 
<button on:click={fetchimagenes}>
    Muestrame imagenes
</button>

<h1>Esto tenemos que enviar {texto}</h1>

<br>




    {#await promise}
    <p>...waiting</p>
    {:then data} 
    <p>{data}</p>
    {/await}

    


