The WordPress Template Hierarchy Everywhere
===========================================

This plugin is an implementation of [the "template hierarchy" concept of WordPress](https://developer.wordpress.org/themes/basics/template-hierarchy/) 
for any template.

It redefines some internal methods often used in templates to use the "template hierarchy" concept
everywhere: for header, footer, sidebar, search form, sub-templates and assets.


## Usage

### Some new concepts & methods

The plugin first introduces a new concept: the ability to include a template passing it some arguments.
This concept is just an enhancement of the [`get_template_part($slug, $name = null)`](https://developer.wordpress.org/reference/functions/get_template_part/) 
internal function, with a third argument as an array of variables which will be loaded in the template:

     get_template_part_fetch($slug, $name = null, $args = array())

Actually, the arguments are extracted in the current environment using [the native `extract()` PHP function](http://php.net/manual/function.extract.php).

The plugin also defines a function to get an asset URI (a CSS or JS file for instance), searching it first in
an optional child theme (if it exists) and falling back to the parent theme. This allows theme developers to
let the final user overwrite one of the theme assets without modifying the original file (please note that this
is not the same as overwritting a CSS rule in the child theme's `style.css` file as the original file of the
parent theme will not be loaded in our case).

This new function is:

     get_asset_uri($name)

### The template hierarchy in action

The smart stuff of the plugin is to introduce a new `get_template_part_hierarchical()` function on the same model
as the classic `get_template_part()` with an implementation of the template hierarchy concept:

     get_template_part_hierarchical($slug, $name = null, $include = true)

This function will search a template file following the same rules as for "root templates" (natively concerned by
the template hierarchy) and finally fallback to the default WordPress behavior.

As for the enhanced version of including a template fetching it arguments described above, an enhanced version of
this new function also exists:

     get_template_part_hierarchical_fetch($slug, $name = null, $include = true, $args = array())

### The template hierarchy aliases

As the core of WordPress does not allow to use its *hooks* to do so, you will have to use some new
methods in replacement of common ones:

| original function    | new function                     |
|----------------------|----------------------------------|
| get_template_part()  | get_template_part_hierarchical() |
| get_footer()         | get_footer_hierarchical()        |
| get_header()         | get_header_hierarchical()        |
| get_sidebar()        | get_sidebar_hierarchical()       |
| comments_template()  | comments_template_hierarchical() |
| get_search_form()    | get_search_form_hierarchical()   |


## Installation

1.  Upload the plugin files to the `/wp-content/plugins/wp-template-hierarchy-everywhere` directory,
    or install the plugin through the WordPress plugins screen directly.
1.  Activate the plugin through the 'Plugins' screen in WordPress
1.  You're done

You can also embed a standalone version of this library in a plugin or a theme (internal tests will avoid multi-loading).

Please see the *releases* of the repository to get last version (the `master` branch can have few commits forward but
the last release is the best choice).

NOTE - For those who are [Composer](http://getcomposer.org/) users, the package is registered
as `picas/wp-template-hierarchy-everywhere`.


## Note for developers

As you will see, I do NOT follow the WordPress coding rules but the PSR (<http://www.php-fig.org/>).
The WordPress rules just sucks.
Period.


## Support

Is you find a bug, please use the [support panel](https://gitlab.com/e-picas/wp-template-hierarchy-everywhere/issues) 
of the repository. You may first search in existing issues to check if the bug you figured has not been
reported yet.


## License

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.

This plugin is made "avec amour" from Paris, France.

    WP_Template_Hierarchy_Everywhere
    Copyright (c) 2016, Pierre Cassat
