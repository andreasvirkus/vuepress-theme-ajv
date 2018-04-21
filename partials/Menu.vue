<template>
  <nav id="menu" class="menu" role="navigation">
    <button class="menu__handle" title="Open menu"><span>Menu</span></button>
    <div class="menu__inner">
        <ul class="menu__list">
          <li v-for="(page, i) in pages"
							:key="i"
              class="menu__item"
              aria-current="page">
            <a :href="post.path" class="menu__link">{{ post.title }}</a>
          </li>
        </ul>
    </div>
    <div class="morph-shape" data-morph-open="M300-10c0,0,295,164,295,410c0,232-295,410-295,410" data-morph-close="M300-10C300-10,5,154,5,400c0,232,295,410,295,410">
      <svg width="100%" height="100%" viewBox="0 0 600 800" preserveAspectRatio="none">
        <path fill="none" d="M300-10c0,0,0,164,0,410c0,232,0,410,0,410"/>
      </svg>
    </div>
  </nav>
</template>

<script>
// Replace morph-shape snap.svg with tweenlite or some other lighter SVG morph lib
// Look into vue-tweezing!
  export default {

  }

  (function() {
    function SVGMenu(el, options) {
        this.el = el;
        this.init();
    }

    SVGMenu.prototype.init = function() {
        this.trigger = this.el.querySelector('button.menu__handle');
        this.shapeEl = this.el.querySelector('div.morph-shape');

        var s = Snap(this.shapeEl.querySelector('svg'));
        this.pathEl = s.select('path');
        this.paths = {
            reset : this.pathEl.attr('d'),
            open : this.shapeEl.getAttribute('data-morph-open'),
            close : this.shapeEl.getAttribute('data-morph-close')
        };

        this.isOpen = false;
        this.initEvents();
    };

    SVGMenu.prototype.initEvents = function() {
        var menu = this,
            isBlogPost,
            menuLinks = document.querySelectorAll('#menu .menu__link');

        menu.trigger.addEventListener('click', menu.toggle.bind(menu));

        // Close menu if opened and reassign current nav link
        [].forEach.call(menuLinks, function(menuLink) {
            menuLink.addEventListener('click', function(e) {
                menu.isOpen && menu.toggle();
            });
        });

        // TODO: Add back ajax navigation once I settle on a page transition animation.

        // document.addEventListener('pjax:complete', function() {
        //     menu.isOpen && menu.toggle();
        //
        //     isBlogPost = true;
        //
        //     // Loop through nav links, find one whose href matches page slug
        //     [].forEach.call(menuLinks, function(link) {
        //         link.parentElement.removeAttribute('aria-current');
        //
        //         if (link.getAttribute('href') === location.pathname) {
        //             link.parentElement.setAttribute('aria-current', 'page');
        //             isBlogPost = false;
        //         }
        //     });
        //
        //     if (isBlogPost) {
        //         document.getElementById('menu')
        //             .querySelector('[href="/thoughts/"]')
        //             .parentElement.setAttribute('aria-current', 'page');
        //     }
        // });
    };

    SVGMenu.prototype.toggle = function() {
        var self = this;

        if (this.isOpen) {
            self.el.classList.remove('menu--anim');
            setTimeout(function() {
                self.el.classList.remove('menu--open');
            }, 250);
        } else {
            self.el.classList.add('menu--anim');
            setTimeout(function() {
                self.el.classList.add('menu--open');
            }, 250);
        }

        this.pathEl.stop().animate({
            'path': this.isOpen
                ? this.paths.close
                : this.paths.open
            }, 350, mina.easeout, function() {
                self.pathEl.stop().animate({ 'path' : self.paths.reset },
                    800,
                    mina.elastic
                );
            });

        this.isOpen = !this.isOpen;
    };

    new SVGMenu(document.getElementById('menu'));

    // new Pjax({
    //     selectors: ["title", "main"],
    //     cacheBust: false,
    //     switches: {
    //         'main': Pjax.switches.outerHTML
    //     }
    // });
})();
</script>

<style>
.menu {
	position: fixed;
	width: 300px;
	top: 3.5em;
	left: 0;
	z-index: 5;
	transition: transform 0.6s;
	color: transparent;
}
.menu::after {
	content: '';
	width: 140vw;
	height: 110vh;
	position: absolute;
	top: -5em;
	left: 0;
	transition: ease-in background-color .1s;
	z-index: -1;
	pointer-events: none;
}

.menu__inner {
	width: calc(100% + 25px);
	padding: 0 140px 2em 0;
	overflow-y: auto;
	height: 80vh;
	position: relative;
	z-index: 5;
}

.menu__list {
	list-style: none;
	padding-left: 50px;
	margin: 0;
	width: 180px;
}

.menu__item {
	margin-bottom: 2em;
	font-weight: 600;
	cursor: pointer;
}
.menu__item::before {
	content: ':';
	position: relative;
	display: inline-block;
	top: -1px;
	left: -3px;
	font-weight: 700;
	height: 11px;
	width: 4px;
}

.menu__link {
	position: relative;
	outline: none;
	text-transform: uppercase;
	color: var(--content-color);
	cursor: pointer;
	transition: color 0.1s ease-in-out, opacity 0.1s;
	opacity: 0.6;
}

.menu__item:hover .menu__link {
	opacity: 1;
}
.menu__item[aria-current="page"] .menu__link {
	opacity: 1;
}

.menu__item:first-child::before {
	color: #A6D865;
}
.menu__item:nth-child(2)::before {
	color: #DBD253;
}
.menu__item:nth-child(3)::before {
	color: #F484D4;
}
.menu__item:nth-child(4)::before {
	color: #EF835F;
}
.menu__item:nth-child(5)::before {
	color: #8F6EF4;
}
.menu__item:nth-child(6)::before {
	color: #00A0B0;
}

.menu__handle {
	display: none;
}

.morph-shape {
	position: absolute;
	width: 240px;
	height: 80vh;
	top: 0;
	right: 0;
}

.morph-shape path {
	stroke: var(--menu-color);
	stroke-width: 5px;
}

@media screen and (min-width: 67em) {
	.morph-shape {
		pointer-events: none;
	}
}

@media screen and (max-width: 67em) {
	.menu.menu--anim li {
		will-change: transform;
		transform: translateX(0);
	}

	.menu__item {
		will-change: transform;
		transform: translate3d(-160px, 0, 0);
		transition: transform 0.6s;
	}

	.menu__item:first-child {
		transition-delay: 0.3s;
	}
	.menu__item:nth-child(2) {
		transition-delay: 0.25s;
	}
	.menu__item:nth-child(3) {
		transition-delay: 0.2s;
	}
	.menu__item:nth-child(4) {
		transition-delay: 0.15s;
	}
	.menu__item:nth-child(5) {
		transition-delay: 0.1s;
	}
	.menu__item:nth-child(6) {
		transition-delay: 0.05s;
	}

	.menu__handle {
		position: absolute;
		top: 3px;
		right: 70px;
		background: transparent;
		width: 30px;
		height: 24px;
		padding: 0;
		border: none;
		outline: none;
		z-index: 9;
		cursor: pointer;
		pointer-events: all;
	}

	.menu__handle::before,
	.menu__handle::after,
	.menu__handle span {
		background-color: var(--menu-color);
		position: absolute;
		height: 3px;
		left: 0;
	}

	.menu__handle::before,
	.menu__handle::after {
		content: '';
		top: 50%;
		transform-origin: 50% 50%;
		transition: transform 0.25s;
	}

	.menu__handle span {
		width: 55%;
		text-indent: 200%;
		color: transparent;
	}

	.menu__handle::before {
		transform: translate3d(0, -8px, 0);
		width: 100%;
	}

	.menu__handle::after {
		transform: translate3d(0, 8px, 0);
		width: 75%
	}

	.menu--open .menu__handle span {
		opacity: 0;
	}

	.menu--open .menu__handle::before {
		transform: rotate3d(0, 0, 1, 45deg);
	}

	.menu--open .menu__handle::after {
		transform: rotate3d(0, 0, 1, -45deg);
		width: 100%;
	}

	.menu:not(.menu--open) {
		pointer-events: none;
	}

	.menu {
		top: 2em;
		transform: translateX(-160px);
	}
	.menu.menu--open {
		transform: translateX(0);
	}
	.menu__handle {
		display: block;
	}
	.menu__handle span::before {
		content: '';
		background-color: rgba(255, 255, 255, 0.9);
		position: absolute;
		top: -20px;
		left: -7px;
		right: -7px;
		bottom: -20px;
		z-index: -1;
		width: 45px;
	}

	/* Menu background overlay */
	.menu--open::after {
		background-color: rgba(255, 255, 255, 0.9);
		pointer-events: inherit;
	}
	.menu.menu--open + main {
		opacity: 0.6;
		pointer-events: none;
	}
}
</style>
