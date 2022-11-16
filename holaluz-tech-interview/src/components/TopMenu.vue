<template>
    <header class="navbar__container">
        <section class="menu__container">
            <article class="logo__container">
            <i class="fa-solid fa-bars"></i>
            <img src="../assets/images/logo-gradient.svg" alt="Logo">
            </article>
            <article class="menu__article">
                <p>Tarifas de luz</p>
                <p>Placas Solares</p>
                <p>Empresas</p>
                <p>Productores</p>
                <p>Blog</p>
            </article>
        </section>
        <section class="contact__container">
            <a href="tel:900670707"><i class="fa-solid fa-phone"></i>900 67 07 07</a>
            <button>Contratar</button>
            <button>Zona Clientes</button>
            <p>luz 100% verde</p>
        </section>
    </header>
</template>

<script>

export default {
    name: 'TopMenu',
    components: {
    // Flex
  },
}

</script>

<style scoped>
img{
    width: 7rem;
}
.navbar__container{
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 100%;
    height: 4.5rem;
    background-color: white;
}
.menu__container{
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 35rem;
    font-size: 1.5rem;
    margin: 0 1rem;

}
.logo__container{
    display: flex;
    justify-content: space-evenly;
    align-items: center;
    width: 10rem;
    color: #e5007e
}
.contact__container{
    display: flex;
    align-items: center;
    width: 30rem;
    justify-content: space-between;
    color: #56b98e;
    font-size: 0.9rem;
    font-weight: 600;
    margin: 0 1rem 0;
}
.contact__container button:nth-child(2){
    background-color:#e5007e;
    color: white;
    width: 8rem;
    height: 2rem;
    font-weight: 700;
    border-radius: 2rem;
    font-size: 1rem;
    border: #e5007e 1px solid
}
.contact__container button:nth-child(3){
    background-color:hsl(327, 100%, 98%);
    color: #e5007e;
    width: 8rem;
    height: 2rem;
    font-weight: 600;
    border-radius: 2rem;
    font-size: 1rem;
    border: #e5007e 1px solid
}
.menu__article{ 
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 0.8rem;
    font-weight: 600;
    width: 25rem;
    margin: 0 1rem;
}
a{
    color: #e5007e;
    font-weight: 700;
}

</style>