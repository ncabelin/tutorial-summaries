# CUSTOM THEMEs FOR WORDPRESS
1. Create static design for your site (HTML5 and CSS3 / Bootstrap)

2. Install Wordpress :

* in Linux Ubuntu

* in Local desktop :
1. Make sure you have MAMP installed, create Database
2. create copy of 'wp-config-sample' to 'wp-config'.
3. edit wp-config, insert database name, username and password
* (optional) Install certbot for Let's Encrypt

_local server, do MAMP, then configure database name then adduser
	copy it to wp-config.php_

3. Install Underscore barebones theme
* Go to https://underscores.me/
* download theme
* Install in your admin panel

4. Make sure you can FTP to /wp-content/themes/< your-theme >
		and copy assets folder to theme directory. Set your pages to homepage
		in settings on wordpress admin

5. a. Edit header.php
Leave the following code in the original header.php :
```
<!doctype html>
<html <?php language_attributes(); ?>>
<head>
	<meta charset="<?php bloginfo( 'charset' ); ?>">
	<meta name="viewport" content="width=device-width, initial-scale=1">
	<link rel="profile" href="http://gmpg.org/xfn/11">

	<?php wp_head(); ?>
</head>

<body <?php body_class(); ?>>
<div id="page" class="site">
	<a class="skip-link screen-reader-text" href="#content"><?php esc_html_e( 'Skip to content', 'andalusiahoa' ); ?></a>

```
		put 'navigation' section in it,
		and if you want dynamic menus, delete nav and replace with :
```
<?php
	wp_nav_menu( array(
			'menu_location' => 'primary',
			'container' => 'nav',
			'container_class' => 'navbar-collapse collapse',
			'menu_class' => 'nav navbar-nav navbar-right'
		)
	);
?>
```
b. Edit footer.php
replace all relative paths to include :
```
<?php bloginfo('stylesheet_directory'); ?>
```
or template_directory

c. create page-home.php (page-< whatever >)
		copy page.php, then leave only get_header and get_footer

6. Install Advanced Custom Field and Custom Post Types UI PLUGINs
a. ACF - how to use :
1. on php file use ACF through variables :
```
$variable = get_field('variable_name');

// then use it in the html section :

<?php echo $variable; ?>
```
2. on images :

		ex. $variable_img = get_field('variable_img');

				then use url to get url

				<?php echo $variable_img['url'] ?>


b. Custom Post UI -

	7. Contact Form 7 - install

				copy html of contact form, delete form tags
				convert to CF7 friendly syntax like so :
```
<div class="form-group">
  <label for="name" class="sr-only">Name</label>
  [text* name class:form-control id:name placeholder "Name"]
</div>
<div class="form-group">
  <label for="email" class="sr-only">E-mail</label>
  <input type="text" class="form-control" placeholder="E-mail">
  [email* email class:form-control id:email placeholder "E-mail"]
</div>
<div class="form-group">
  <label for="phone" class="sr-only">Phone</label>
  <input type="phone" class="form-control" placeholder="Phone">
  [text* phone class:form-control id:phone placeholder "Phone"]
</div>
<div class="form-group">
  <label for="message" class="sr-only">Message</label>
  <textarea placeholder="Message" class="form-control" rows="8"></textarea>
  [textarea* message class:form-control id:message placeholder "Message"]
</div>
<button class="btn btn-success btn-block btn-lg">Submit</button>
[submit class:btn class:btn-success class:btn-block class:btn-lg "Submit"]
```
* update email with [brackets]

8. Style the current page in nav :
- update style.css with :
```
.navbar-nav > li.current_page_item a,
.navbar-nav > li.current_page_parent a {
	color: white;
}
```

9. For Custom Post UIs :
```
<?php $loop = new WP_Query( array(‘post_type’ => ‘<NAME OF POST UI>’, ‘orderly’ => ‘post_id’, ‘order’ => ‘ASC’ ) ); ?>

<?php while( $loop->have_posts() ) : $loop->the_post(); ?>
```
		enter on html <?php the_field(‘’); ?>

10. Edit 'wp-config.php' and put this is at the end of the file.
```
define('FS_METHOD', 'direct');
```
# Uploading to live site / server after live-site Wordpress install
1. Compress your theme folder '.zip'
2. Go to your admin page and upload theme zip file
3. Activate
4. Install Plugins (Contact Form 7, Custom Post UIs)
5. Export database :
* go to phpmyadmin then click on database
* click 'database name' then click 'export'
* click 'custom' then click output to 'file' downloading it by clicking 'go'

6. Go to your live site's wp-config.php and take note of the 'database name'
* go to your live site's phpmyadmin, find that database name and 'drop' all of it

7. Go to your local site's wp-config.php and take note of the 'table_prefix'
* then go to the live site's wp-config.php and make sure it is the same by editing it.
* go to live site's phpmyadmin, and import your local database

8. Go to [Search and Replace DB Github](https://github.com/interconnectit/Search-Replace-DB)
* download the zip file and unzip in your local machine, copy this folder onto the root folder of your live site
* go to your live site + '/Search-Replace-DB-master' in your browser fill up the search fields e.g. http://localhost:8888 and https://mysite.com and do a DRY RUN, check and see then do the LIVE update. afterwards DELETE the Search and Replace tool.

9. If you want to change the login logo, add the plugin Login logo
10. If you want Google analytics you can add a tracking ID from Google and put it in a plugin you can add (Yoast Google Analytics)
