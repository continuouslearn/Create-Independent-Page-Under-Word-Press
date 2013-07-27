Create-Independent-Page-Under-Word-Press
========================================

create an independent page under word press CMS

--------------------------------------

*Step 1*
  
  Create a new page, say called structure_search.php under the root of Wordpress
  
*Step 2*

  Merge index.php, wp-blog-header.php, wp-load.php, wordpress/wp-includes/template-loader.php
  into structure_search.php
  
*Step 3*

  structure_search.php looks like
  
  ```
<?php
define('WP_USE_THEMES', true);

if ( !isset($wp_did_header) ) {

        $wp_did_header = true;

        //require_once( dirname(__FILE__) . '/wp-load.php' );
define( 'ABSPATH', dirname(__FILE__) . '/' );

error_reporting( E_CORE_ERROR | E_CORE_WARNING | E_COMPILE_ERROR | E_ERROR | E_WARNING | E_PARSE | E_USER_ERROR | E_USER_WARNING | E_RECOVERABLE_ERROR );

if ( file_exists( ABSPATH . 'wp-config.php') ) {

        /** The config file resides in ABSPATH */
        require_once( ABSPATH . 'wp-config.php' );

} elseif ( file_exists( dirname(ABSPATH) . '/wp-config.php' ) && ! file_exists( dirname(ABSPATH) . '/wp-settings.php' ) ) {

        /** The config file resides one level above ABSPATH but is not part of another install */
        require_once( dirname(ABSPATH) . '/wp-config.php' );

} else {
        if ( strpos($_SERVER['PHP_SELF'], 'wp-admin') !== false )
                $path = 'setup-config.php';
        else
                $path = 'wp-admin/setup-config.php';

        define( 'WPINC', 'wp-includes' );
        define( 'WP_CONTENT_DIR', ABSPATH . 'wp-content' );
        require_once( ABSPATH . WPINC . '/load.php' );
        require_once( ABSPATH . WPINC . '/version.php' );

        wp_check_php_mysql_versions();
        wp_load_translations_early();

        require_once( ABSPATH . WPINC . '/functions.php' );

        // Die with an error message
        $die  = __( "There doesn't seem to be a <code>wp-config.php</code> file. I need this before we can get started." ) . '</p>';
        $die .= '<p>' . __( "Need more help? <a href='http://codex.wordpress.org/Editing_wp-config.php'>We got it</a>." ) . '</p>';
        $die .= '<p>' . __( "You can create a <code>wp-config.php</code> file through a web interface, but this doesn't work for all server setups. The safest way is to manually create the file." ) . '</p>';
        $die .= '<p><a href="' . $path . '" class="button button-large">' . __( "Create a Configuration File" ) . '</a>';

        wp_die( $die, __( 'WordPress &rsaquo; Error' ) );
}


        wp();

//var_dump($_GLOBAL);

       // require_once( ABSPATH . WPINC . '/template-loader.php' );
if ( defined('WP_USE_THEMES') && WP_USE_THEMES )
        do_action('template_redirect');

// Halt template load for HEAD requests. Performance bump. See #14348
if ( 'HEAD' === $_SERVER['REQUEST_METHOD'] && apply_filters( 'exit_on_http_head', true ) )
        exit();

// Process feeds and trackbacks even if not using themes.
if ( is_robots() ) :
        do_action('do_robots');
        return;
elseif ( is_feed() ) :
        do_feed();
        return;
elseif ( is_trackback() ) :
        include( ABSPATH . 'wp-trackback.php' );
        return;
endif;

if ( defined('WP_USE_THEMES') && WP_USE_THEMES ) :
        $template = false;
        if     ( is_404()            && $template = get_404_template()            ) :
        elseif ( is_search()         && $template = get_search_template()         ) :
        elseif ( is_tax()            && $template = get_taxonomy_template()       ) :
        elseif ( is_front_page()     && $template = get_front_page_template()     ) :
        elseif ( is_home()           && $template = get_home_template()           ) :
        elseif ( is_attachment()     && $template = get_attachment_template()     ) :
                remove_filter('the_content', 'prepend_attachment');
        elseif ( is_single()         && $template = get_single_template()         ) :
        elseif ( is_page()           && $template = get_page_template()           ) :
        elseif ( is_category()       && $template = get_category_template()       ) :
        elseif ( is_tag()            && $template = get_tag_template()            ) :
        elseif ( is_author()         && $template = get_author_template()         ) :
        elseif ( is_date()           && $template = get_date_template()           ) :
        elseif ( is_archive()        && $template = get_archive_template()        ) :
        elseif ( is_comments_popup() && $template = get_comments_popup_template() ) :
        elseif ( is_paged()          && $template = get_paged_template()          ) :
        else :
                $template = get_index_template();
        endif;
        if ( $template = apply_filters( 'template_include', $template ) )
        {
            //echo "template : $template";
              //  include( $template ); // customize this line
                include("/wordpress/wp-content/themes/twentyeleven/ss.php");
        }
        return;
endif;
}

  ```

*step 4*

  Customize structure_search.php
  about line 96,  include your own file here. 
  It depends which template you choose, you need to put your own file under your chosen template folder.
  For example, I choose template twentyeleven, I will create my own php/js file under this template folder.
```
  include("/wordpress/wp-content/themes/twentyeleven/myownphpfile.php");
```

*step 5*

  Create your own php/JS/HTML file under your  template folder
  All WP templates is under /wordpress/wp-content/themes/ folder.
  
```
<?php
get_header(); ?>

                <div id="primary">
                        <div id="content" role="main">

               // all your php js html css code here


                        </div><!-- #content -->
                </div><!-- #primary -->

<?php get_sidebar(); ?>
<?php get_footer(); ?>

```
  
*last step*
  
  Create  a menu link to this page
  You need to create a page, for example: search, then WP will give a link like:
  http://yourdomain.com/wordpress/search
  
  You want link this url to your page
  http://yourdomain.com/wordpress/structure_search.php
  
  Then add a RewriteRule into .htaccess
```
RewriteRule search http://yourdomain.com/wordpress/structure_search.php [L]
```

That's it. Enjoy!

  
