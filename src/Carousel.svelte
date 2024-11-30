<script>
    export let items = []; // Items dinámicos para el carrusel

    let carouselContainer;

    const scroll = (direction) => {
        const scrollAmount = 300;
        if (carouselContainer) {
            carouselContainer.scrollBy({ left: direction === 'left' ? -scrollAmount : scrollAmount, behavior: 'smooth' });
        }
    };
</script>

<div class="carousel-container">
    <button class="control left" on:click={() => scroll('left')}>←</button>
    <button class="control right" on:click={() => scroll('right')}>→</button>

    <div class="carousel" bind:this={carouselContainer}>
        {#each items as item}
            <div class="item">
                <a href={item.link} target="_blank" rel="noopener noreferrer">
                    <img src={item.image} alt={item.title} />
                </a>
                <div class="text">
                    <div class="title">{item.title}</div>
                    <div class="description">{item.description}</div>
                </div>
            </div>
        {/each}
    </div>
</div>

<style>
    /* Contenedor principal del carrusel */
    .carousel-container {
        position: relative;
        width: 96%;
        height: 400px;
        overflow: hidden;
        padding: 0 40px;
    }

    /* Contenedor del carrusel */
    .carousel {
        display: flex;
        gap: 2rem;
        overflow-x: auto;
        scroll-behavior: smooth;
        padding: 1rem;
        scrollbar-width: none; /* Oculta la barra de desplazamiento */
        overflow-y: hidden; /* Evita el scroll vertical */
        margin-top: 1px;
    }

    .carousel::-webkit-scrollbar {
        display: none; /* Oculta la barra de desplazamiento en WebKit */
    }

    /* Estilos de los ítems */
    .item {
        position: relative;
        flex: 0 0 200px;
        height: 300px;
        border-radius: 10px;
        overflow: hidden;
        transition: transform 0.3s ease, box-shadow 0.3s ease;
    }

    .item img {
        width: 100%;
        height: 100%;
        object-fit: cover; /* Ajusta la imagen al contenedor */
        display: block; /* Asegura que las imágenes sean clicables */
    }

    /* Hover: agranda el ítem */
    .item:hover {
        transform: scale(1.2);
        box-shadow: 0 4px 15px rgba(250, 250, 249, 0.5);
        z-index: 5; /* Prioridad visual */
    }

    /* Estilo del texto superpuesto */
    .text {
        position: absolute;
        bottom: 0;
        left: 0;
        width: 100%;
        padding: 0.8rem;
        background: #DDE2B2;
        color: #383838;
        padding-bottom: 25px;
        transform: translateY(100%);
        transition: transform 0.3s ease-in-out;
    }

    /* Hover: muestra el texto */
    .item:hover .text {
        transform: translateY(0);
    }

    .title {
        font-size: 16px;
        font-weight: bold;
    }

    .description {
        font-size: 0.8rem;
        margin-top: 0.3rem;
    }

    /* Botones de control */
    .control {
        position: absolute;
        top: 50%;
        transform: translateY(-50%);
        background: rgba(165, 184, 46, 0.7);
        color: white;
        border: none;
        border-radius: 50%;
        width: 40px;
        height: 40px;
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;
        z-index: 10;
        transition: background-color 0.3s ease;
    }

    /* Hover: cambia el color del botón */
    .control:hover {
        background: rgba(165, 184, 46, 1);
    }

    .control.left {
        left: 0;
    }

    .control.right {
        right: 0;
    }
</style>