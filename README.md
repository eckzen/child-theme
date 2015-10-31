https://thethemefoundry.com/blog/wordpress-child-theme/
###How to create a Child Theme

A child theme consists of at least one directory (the child theme directory) and two files (style.css and functions.php), which you will need to create:

+ The child theme directory
+ style.css
+ functions.php

Create a child theme inside wp-content/themes

    theme-child

Create a stylesheet with this format

    /*
     Theme Name:   Twenty Fifteen Child
     Theme URI:    http://example.com/twenty-fifteen-child/
     Description:  Twenty Fifteen Child Theme
     Author:       John Doe
     Author URI:   http://example.com
     Template:     twentyfifteen
     Version:      1.0.0
     License:      GNU General Public License v2 or later
     License URI:  http://www.gnu.org/licenses/gpl-2.0.html
     Tags:         light, dark, two-columns, right-sidebar, responsive-layout, accessibility-ready
     Text Domain:  twenty-fifteen-child
    */

The final step is to enqueue the parent and child theme stylesheets.

The correct method of enqueuing the parent theme stylesheet is to add a wp_enqueue_scripts action and use wp_enqueue_style() in your child theme's functions.php.

You will therefore need to create a functions.php in your child theme directory.

    add_action( 'wp_enqueue_scripts', 'theme_enqueue_styles' );
    function theme_enqueue_styles() {
        wp_enqueue_style( 'parent-style', get_template_directory_uri() . '/style.css' );
    }

Your child theme's stylesheet will usually be loaded automatically. If it is not, you will need to enqueue it as well. Setting 'parent-style' as a dependency will ensure that the child theme stylesheet loads after it.

    function theme_enqueue_styles() {
        $parent_style = 'parent-style';
        wp_enqueue_style( $parent_style, get_template_directory_uri() . '/style.css' );
        wp_enqueue_style( 'child-style',
            get_stylesheet_directory_uri() . '/style.css',
            array( $parent_style )
        );
    }
    add_action( 'wp_enqueue_scripts', 'theme_enqueue_styles' );