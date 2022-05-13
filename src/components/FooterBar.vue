<template>
    <footer>
        <div class="content">
            <p class="no-interaction">Made with <span class="pulse">❤️</span></p>
            <p> <a href="https://rapidapi.com/dot357/api/qr-code-generator-with-colors">Free API</a> / <a href="https://linkedin.com/in/emrecaneskimez"><strong>Emre Can Eskimez (dot357)</strong></a></p>
        </div>
    </footer>
</template>


<style scoped>
footer{
    
    padding: 4rem 0 8rem 0;

}

footer .content {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    position: relative;
}

@keyframes pulser {
    0% {font-size: 1rem;}
    
    50% {font-size: 1.2rem;}
    100% {font-size: 1rem;}
}


.pulse{
    position: absolute;
    top:-2px;
    margin-left: 5px;
   
    display: inline-block;
    animation-name: pulser;
    animation-duration: 2s;
    animation-timing-function: cubic-bezier(0.075, 0.82, 0.165, 1);
    animation-iteration-count: infinite;
}

.no-interaction{
    pointer-events: none;
    user-select: none;
    -moz-user-select: none;
    -webkit-user-select: none;
}


footer a {
    color: inherit;
}
</style>