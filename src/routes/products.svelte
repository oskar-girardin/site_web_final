<script>
    import Nav from "../components/nav.svelte";
    import ProductWidget from "../components/product_widget.svelte";
    import supabase from "$lib/db.js";

    async function getData() {
        let { data, error } = await supabase
            .from('products')
            .select()
        if (error) throw new Error(error.message)
        return data;
    }


</script>

<svelte:head>
    <title>Our Products</title>
</svelte:head>


<div class="sticky z-10 top-0 left-0 w-full">
    <Nav></Nav>
</div>

<body class="">
    <div class="grid place-items-center py-12">
        <div class="w-1/2">
            <h1 class="text-center text-yellow-300 font-semibold tracking-tight text-3xl">
                Our selection of products at the forefront of Tech and Innovation!
            </h1>
        </div>
    </div>

    <div class="flex justify-center flex-wrap gap-6">

        {#await getData()}
            <p>Fetching data...</p>
        {:then data}

            {#each data as product}

                <ProductWidget imgUrl={product.image_url} price={product.price} title={product.title} description={product.description} weight={product.weight}
                size={product.size} battery={product.battery_autonomy} uselessness={product.uselessness} location={product.production_location} productId={product.product_id}></ProductWidget>
    
            {/each}

        {:catch error}
        <p>Something went wrong while fetching the data:</p>
        <pre>{error}</pre>
        {/await}

    </div>

    <div class="mt-10 grid place-items-center gap-10 pt-10 pb-14">
        <p class="text-4xl text-yellow-300 font-semibold">Do you have a product in mind that is even more useless?</p>
        <div>
            <a href="/contact" class="bg-yellow-300 p-4 uppercase text-blue-500 rounded-md shadow-lg font-semibold">
                Tell us more!
            </a>
        </div>
    </div>

</body>


