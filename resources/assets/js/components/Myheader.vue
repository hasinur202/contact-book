
<template>
    <nav class="navbar" role="navigation" aria-label="main navigation">
    <div class="navbar-brand">
        <a class="navbar-item" href="">
            <img src="" alt="Hasin" width="112" height="28">
        </a>

        <div class="navbar-menu is-active">

            <router-link to="/home" class="navbar-item">Home</router-link>
            <router-link to="/about" class="navbar-item">About</router-link>

        </div>

        <a role="button" class="navbar-burger" aria-label="menu" aria-expanded="false">
        <span aria-hidden="true"></span>
        <span aria-hidden="true"></span>
        <span aria-hidden="true"></span>
        </a>
    </div>
    </nav>
</template>