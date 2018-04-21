<template>
    <footer>
        <social-links />
        <p>Proudly generated with <a href="{{ site.generatorurl }}" title="{{ generatortitle }}">{{ site.generatorname }}</a>.</p>
        <p>Safely hosted on <a href="{{ site.hosturl }}" title="{{ hosttitle }}">{{ site.hostname }}</a>.</p>
    </footer>
</template>

<style>
footer {
    margin-bottom: 1.5rem;
}
footer p {
    text-align: right;
    padding-right: 0.8em;
    margin: 0;
}
.social__link {
    position: relative;
}
.social__link::after {
    content: '';
    position: absolute;
    width: 3em;
    height: 3em;
    transition: border-color .2s;
    border: 1px dashed transparent;
    top: -0.65em;
    left: -0.62em;
    z-index: 0;
}
.social__link:hover::after {
    border-color: #444;
}

@media screen and (min-width: 35em) {
    footer {
        padding-right: 3em;
    }
}

@media screen and (max-width: 35em) {
    .social {
        padding-right: 1em;
    }
}
</style>