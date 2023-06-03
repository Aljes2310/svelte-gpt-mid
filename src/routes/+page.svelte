<script>

    let texto;
    let requestimagen;
    let promise = Promise.resolve([]); //crea una promesa que luego se reemplaza con el valor asignado
    let promise2 = Promise.resolve([])
    async function fetchtext () {
        const response= await fetch("https://api.openai.com/v1/completions", {
            method: "POST",
        headers:{  
            Authorization: `Bearer ${import.meta.env.VITE_API_KEY}`, //importando por medio de VITE
            "Content-Type": "application/json"},

            body: JSON.stringify({
                model: "text-davinci-003",
                prompt: texto,
                max_tokens: 1024,
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
                Authorization: `Bearer ${import.meta.env.VITE_API_KEY}`, 
            "Content-Type": "application/json"},

            body: JSON.stringify({
                prompt: requestimagen,
                n:1,
                size:"256x256"
            })
        })

        const img = await res.json();
        const first_img= img["data"][0]["url"]

        console.log(first_img)

        if (res.ok) {
        return first_img
    } else {
        throw new Error("error")
    }

    

    };

    function handleclick2() {
        promise2=fetchimagenes()
    }



</script>

<div class="flex justify-center sm:py-10 mt-5 sm:gap-x-10 items-center gap-x-2">
    <h1 class="sm:text-5xl text-3xl font-mono">Preguntale a la IA</h1>
    <img src="imegi.png" class="sm:h-20 sm:w-20 h-10 w-10" alt=""/>
</div>

<div class="flex sm:flex-row flex-col justify-center gap-x-36 mx-20">


    <!-- pregunta left -->
    <div class="flex flex-col gap-y-10 justify-center items-center mt-10 basis-1/2 mb-7">
    <h2 class="sm:text-3xl text-slate-700 font-mono"> Que te gustaria saber</h2>
    <input id="texto" placeholder="" bind:value={texto} class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"> 
    <button type="button" on:click={handleclick} class="shadow bg-purple-500 hover:bg-purple-400 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded">
        Preguntale a gpt
    </button>

<!-- respuesta -->


    <label for="message" class="block text-sm font-medium text-gray-900 dark:text-white font-mono">Respuesta: </label>
    {#await promise}
<textarea id="message" rows="4" class="block p-2.5 w-full h-96 text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" >
....waiting
</textarea>
{:then data} 
<textarea id="message" rows="4" class="block p-2.5 w-full h-96 text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" >
{data}</textarea>
{/await}

</div>



    <!-- rightt -->
    <div class="flex flex-col gap-y-10 items-center mt-10 basis-1/2 mb-9">
        <h2 class="sm:text-3xl text-slate-700 justify-center font-mono"> Describe la imagen que quieres crear</h2>
        <input id="imagen" placeholder="" bind:value={requestimagen} class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"> 
    <button on:click={handleclick2} class="shadow bg-purple-500 hover:bg-purple-400 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded">
    Crear imagen dall-e
    </button>
       



            <label for="message" class="block mb-10 text-sm font-medium text-gray-900 dark:text-white font-mono">Imagen: </label>
            {#await promise2}
            <p>
    ....waiting
            </p>
    {:then first_img} 
        <img src={first_img} alt=""/>
    {/await}
    



        </div>

</div>

<!-- 
<div class="flex flex-row">
    <div class= "flex flex-col basis-1/2 ml-36 px-36 justify-center items-center">
        <label for="message" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Respuesta: </label>
        {#await promise}
<textarea id="message" rows="4" class="block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" >
....waiting
</textarea>
{:then data} 
<textarea id="message" rows="4" class="block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" >
    {data}</textarea>
{/await}
    </div>

</div>
 -->




