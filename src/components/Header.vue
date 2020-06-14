<template>
    <section class="section">
        <img class="logo" alt="Rezgo logo" src="../assets/rezgo_logo.png">

        <h1 class="intro-header is-size-2 has-text-weight-bold">Discover amazing things to do </h1>
        <h3 class="subheader is-size-5 ">Meet people all over the world while trying something new. <br>
            Engaging activities that go beyond the typical tour or class. </h3>
    </section>
</template>

<style scoped>
section.section{
    background: #ADA996;  /* fallback for old browsers */
    background: -webkit-linear-gradient(to right, #EAEAEA, #DBDBDB, #F2F2F2);  /* Chrome 10-25, Safari 5.1-6 */
    background: linear-gradient(to right, #EAEAEA, #DBDBDB, #F2F2F2); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
}
img.logo{
    margin-bottom:2rem;
}
h1.intro-header{
    line-height:1;
    margin-bottom:1rem;
}
</style>

<script>
    export default {
        name:'Header'
    }
</script>