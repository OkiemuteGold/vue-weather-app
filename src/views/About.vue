<template>
    <div class="about">
        <main>
            <h1>About App</h1>
            <p>
                This is a weather application made with Vue js. App uses
                openwweathermap API.
            </p>
            <p>
                App allows you to search by <strong><em>City</em></strong
                >, <strong><em>Country</em></strong
                >, or <strong><em>Zip code</em></strong
                >.
            </p>
            <p>
                Simply type in the input box, and either press
                <strong><em>Enter</em></strong> key or click the
                <strong><em>Icon</em></strong> to search weather condition of
                any location.
            </p>
        </main>
    </div>
</template>

<style scoped>
.about {
    height: 100vh;
    max-width: 800px;
    margin: auto;
    text-align: center;
}
main {
    padding: 100px 20px;
}
h1 {
    margin-bottom: 25px;
}
main p {
    line-height: 26px;
    margin-bottom: 15px;
}
</style>
