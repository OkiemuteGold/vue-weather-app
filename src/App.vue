<template>
    <div id="app">
        <div id="nav">
            <router-link to="/">Home</router-link>
            <router-link to="/about">About</router-link>
        </div>
        <transition name="page" mode="out-in">
            <router-view />
        </transition>
    </div>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@300;500;700&display=swap");

/* @import url("https://fonts.googleapis.com/css2?family=Lato:wght@300;700&display=swap"); */

*,
body {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

:root {
    --text-color: #fff;
}

body {
    font-family: "montserrat", sans-serif;
    overflow: hidden;
}

#app {
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    background: linear-gradient(
        to bottom right,
        rgba(219, 215, 215, 0.4),
        rgba(0, 0, 0, 0.25)
    );
    color: #2c3e50;
}

#nav {
    position: absolute;
    left: 0%;
    right: 0%;
    text-align: center;
    padding: 30px 0;
    /* background-color: #fff; */
    z-index: 1;
}

#nav a {
    font-weight: bold;
    color: #2c3e50;
    text-decoration: none;
    padding: 8px 10px;
    border-radius: 3px;
}

#nav a.router-link-exact-active {
    color: #108f56;
    background-color: #fff;
}

.page-enter,
.page-leave-to {
    opacity: 0;
    transform: translateY(-30px);
}

.page-enter-active,
.page-leave-active {
    transition: all 0.3s ease;
}

.page-enter-to,
.page-leave {
    opacity: 1;
    transform: translateY(0);
}
</style>
