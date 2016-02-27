Internal Wordpress templating system improvements
=================================================

The library basically embeds two new features:

-   the ability to test if we are currently displaying a *blog* page (anything about posts) or not (any other page like
    a "page" post type, the search page, the 404 etc);
-   an update of the templating fallback system to let you customize the PHP files used to build parts of the whole page
    in a complete logic, including custom post types, post formats and any known page type for Wordpress.


Using template hierarchy for template parts
-------------------------------------------

The `get_template_part_hierarchical()` is built on the same model as the internal `get_template_part()`:

    get_template_part_hierarchical( string $slug, string $name = null )

Basically, it will try to include first the requested template part file filtered using the basic template hierarchy
of Wordpress, which means something like:

    <slug>-<name>-<page type>-<current object info>.php
    <slug>-<name>-<page type>.php
    <slug>-<name>.php
    <slug>-<page type>.php
    <slug>.php

For example, if you want to include the generic `partials/navbar` file from a "product" post type single page, 
it will search this template in the following order:

-   partials/navbar-single-product-<product slug>.php
-   partials/navbar-single-product-<product ID>.php
-   partials/navbar-single-product.php
-   partials/navbar-single.php
-   partials/navbar-product.php
-   partials/navbar.php

The rules are the same if you specify a "name" argument in your initial call, let's say "footer":

-   first, the "navbar-footer" template is searched:
    -   partials/navbar-footer-single-product-<product slug>.php
    -   partials/navbar-footer-single-product-<product ID>.php
    -   partials/navbar-footer-single-product.php
    -   partials/navbar-footer-single.php
    -   partials/navbar-footer-product.php
    -   partials/navbar-footer.php
-   then it fallbacks to the generic "navbar" template:
    -   partials/navbar-single-product-<product slug>.php
    -   partials/navbar-single-product-<product ID>.php
    -   partials/navbar-single-product.php
    -   partials/navbar-single.php
    -   partials/navbar-product.php
    -   partials/navbar.php


Include sub-templates with arguments
------------------------------------

The first added function is the ability to pass arguments to the template inclusion system of Wordpress: the
[`get_template_part()`](https://developer.wordpress.org/reference/functions/get_template_part/) function. This
is done by defining the new `get_template_part_fetch()` function, with the following signature:

    get_template_part_fetch($slug, $name = null, $args = array())

It uses the core function, first extracting the arguments in the environment to let them available in the included
template.

Example:

    # my-template-1.php
    
    <?php get_template_part_fetch('my-template-part-2', '', array('my_var'=>'my value')); ?>

    
    # my-template-2.php
    
    <?php echo $my_var; ?> // this will output: 'my value'
