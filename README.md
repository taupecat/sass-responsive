sass-responsive
===============

A library of variables, mixins and formulas geared towards Responsive Web Design using Sass.


## Breakpoints

Breakpoints, coupled with media queries, are the crux of responsive web design.  Breakpoints are the points in your design where something changes, and those changes are made in CSS, along the lines of:

    @media only all and (max-width: 800px) {
        /* Your new CSS here */
    }

Setting your breakpoints as variables in one place keeps them organized and consistent throughout your CSS.

## Contexts

Fluid grids, another key aspect of responsive web design, rely on percentages, rather than fixed pixels, to denote horizontal measurement.  Yet, your web designers are likely to give you a Photoshop document where everything is in fixed, unchanging pixels.

Contexts are the variable in the equation:

    $percentage = $target / $context

So, for example, if you have a comp where the main column occupies 600 pixels of a total 960 pixel width, you would figure out the percentage by calculating something like:

    62.5% = 600px / 960px

where `960px` is the context. (We'll make actual functions you can use in Sass further down in this document.) Often you're using the same value over and over as the context for a variety of situations, so like the breakpoints, you can keep them all here for easy reference and better consistency.

## Mixins

### responsive

The main mixin that you'll use for most of your responsive Sass coding.  It can either be used outside of any other styling blocks, as such:

    $breakpoint: "max-width: 900px";

    @include responsive($breakpoint) {
        .some-selector {
            background-color: #FFFFFF;
            color: #000000;
        }
    }

**Output:**

    @media only all and (max-width: 900px) {
        .some-selector {
            background-color: #FFFFFF;
            color: #000000;
        }
    }

or inside a styling block, taking advantage of Sass' "[media query bubbling](http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html#media)," like so:

    $breakpoint: "max-width: 900px";

    .some-selector {
        float: left;
        
        @include responsive($breakpoint) {
            float: none;
        }
    }

**Output:**

    .some-selector {
        float: left;
    }
    
    @media only all and (max-width: 900px) {
        .some-selector {
            float: none;
        }
    }

### hidpi

HiDPI displays (you might know this better as Retina displays) are the high pixel density displays found in devices such as the iPhone 5.  When using background images, you can specify a larger image file, then shrink it down using the `background-sizing` property, to achieve crisper looking graphics than you would get with standard graphics.  To easily facilitate this, use the `hidpi` mixin like so:

    .some-selector {
        background-image: bg.png;

        @include hidpi {
            background-image: bg@2x.png;
            background-size: 50% auto;
        }
    }

**Output:**

    .some-selector {
        background-image: bg.png;
    }
    
    @media only all and (-webkit-min-device-pixel-ratio: 1.3),
		only all and (min--moz-device-pixel-ratio: 1.3),
		only all and (-o-min-device-pixel-ratio: 1.3/1),
		only all and (min-device-pixel-ratio: 1.3),
		only all and (min-resolution: 1.3dppx) {

        .some-selector {
		    background-image: bg@2x.png;
		    background-size: 50% auto;
		}
	}

### retina

If you prefer to use the Apple proprietary term "retina," I've made it an alias to the hidpi mixin, and it is used the same way.

### rem

Fixed pixels are bad! Relative rems are good! But some browsers (ahem, IE<9) don't understand rems. So if you have to support such ancient technology, but still want to use rems, use this mixin to return the same property in _both_ pixel and rem formats.

    .some-selector {
        @include rem(font-size, 13px);
    }

**Output:**

    .some-selector {
        font-size: 13px;
        font-size: 0.8125rem;
    }

_(assuming a default font size of 16px)_

Note: this will not work for properties that take multiple values, such as the padding shorthand, for example. For that, you would need to explicitly state a pixel version, followed by a rem version:

    .some-selector {
        padding: 16px 0 32px 0;
        padding: cr(16px) 0 cr(32px) 0;
    }
    
**Output:**

    .some-selector {
        padding: 16px 0 32px 0;
        padding: 1rem 0 2rem 0;
    }

_(assuming a default font size of 16px)_

## Functions

### calc-rem()

A way to calculate `rem` (relative em units), based on a context. I've set the default context to be a default font size, specified as 16px, and since most often you are going to use the font size set on the `<HTML>` element, you are unlikely to ever need to add the context to this particular function.

To change your default font size, set the variable $default-font-size elsewhere in your Sass stylesheets.

### cr()

Simply a shorthand alias for calc-rem(), used exactly the same way.


### calc-percent()

The same basic function as calc-rem(), but for percentages instead. This is used mainly in horizontal measurements.  Unlike calc-rem(), you will always need to specify a context.  As illustrated above, you can set a variety of often-used $context variables above to make your site more consistent.

### cp()

A shorthand alias for calc-percent().

