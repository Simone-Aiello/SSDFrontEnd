<script lang="ts">
    import Navbar from "./Navbar.svelte";
</script>

<main>
    <Navbar />
        <head>
            <meta charset="utf-8" />
            <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />
            <meta name="description" content="" />
            <meta name="author" content="" />
            <title>Secure Sea Design</title>
            <!-- Favicon-->
            <link rel="icon" type="image/x-icon" href="assets/favicon.ico" />
            <!-- Core theme CSS (includes Bootstrap)-->
            <link href="css/styles.css" rel="stylesheet" />
        </head>
        <body>
            <!-- Page Content-->
            <section>
                <div class="container px-4 px-lg-5">
                    <div class="row gx-4 gx-lg-5">
                        <div class=" header">
                            <h1 class="mt-5">Secure Sea Design</h1>
                            <p>Web application for reserving umbrella in our beautiful resorts</p>
                        </div>
                    </div>
                    <div class="row justify-content-center gx-4 gx-lg-5">
                        <img class="lido" src="src/assets/bg.JPEG" alt="">
                    </div>

                    
            </section>
        </body>
</main>

<style>
    .lido{
        width: 80%;
        height: auto;
        border-radius: 2em;
        padding: 1em;
    }

    .book{
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }

    .book button{
        width: fit-content;
        padding: 0.5em;
        font-size: 1.5em;
    }
    .header{
        display: flex;
        flex-direction: column;
        text-align: center;
        justify-content: center;
    }

    .header h1{
        font-size: 4em;
    }

    .header p{
        font-size: 1.2em;
    }
</style>