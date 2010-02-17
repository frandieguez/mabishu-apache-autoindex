# mabishu-apache-autoindex: An autoindex theme for Apache autoindex module with Mabishu Studio branding 

Index-Style is a set of html, css and image files designed to work together with
the mod_autoindex module to make the default Apache file listings look a little
nicer. The UI design is based almost entirely on the great work done by the guys
at [Repos-Style](http://www.reposstyle.com/), although the code itself is largely
done from scratch (as mod_autoindex doesn’t support XSLT).

## Installation

First, get a copy of the "include" folder – the easiest way is to change to
the document root of the domain you want to style, and check it out from
subversion (in my case the domain will be download.recurser.com ) :

 $ cd /var/www/YOUR.VHOST.LOCAL
 $ git clone http://github.com/frandieguez/mabishu-apache-autoindex.git

This should create an ‘include’ folder in the parent of your document root. Next, you
need to configure Apache to use the "includes" files to style your directory
listings. I use the following config, which you’ll need to adjust to match your
particular setup – the ‘icons’ and 'includes' alias and the ‘mod_autoindex’ 
section are the main areas to pay attention to:

 # Virtualhost example configuration file with autoindex theme support
 <VirtualHost *:80>
    ServerName your.vhost.local

    DocumentRoot /var/www/your.vhost.local/public_html

	# Define where is the theme and icons directory
    Alias /icons/ /var/www/your.vhost.local/include/icons/
	Alias /include/ /var/www/your.vhost.local/include/
	
    <Directory "/var/www/your.vhost.local/public_html">
        AllowOverride All
        Order allow,deny
        Allow from all

		# Tell Apache to add theme support to autoindex
        <IfModule mod_autoindex.c>
            Options Indexes FollowSymLinks
            IndexOptions +FancyIndexing 
            IndexOptions +VersionSort 
            IndexOptions +HTMLTable 
            IndexOptions +FoldersFirst 
            IndexOptions +IconsAreLinks 
            IndexOptions +IgnoreCase 
            IndexOptions +SuppressDescription 
            IndexOptions +SuppressHTMLPreamble 
            IndexOptions +XHTML 
            IndexOptions +IconWidth=16 
            IndexOptions +IconHeight=16 
            IndexOptions +NameWidth=*
            IndexOrderDefault Descending Name
            HeaderName /include/header.html
            ReadmeName /include/footer.html
        </ifModule>
 
    </Directory>
 </VirtualHost>
You’ll need mod_autoindex for any of this to work – it should be installed by 
default with Apache in most linux distributions.

#Disclaimer

The formatting relies on a few javascript hacks which may or may not work exactly
as intended if the output of your apache directory listings differs a lot from
what is expected. If the output appears strange, try playing around with the 
javascript formatting in header.html, or drop me a line if you need a hand.
