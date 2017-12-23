Overview
====================
This folder contains MySQL migration files that you can utilize to quickly create and seed your development database. The purpose of these files is to give you a starting point for developing new and existing sites without having to constantly generate dummy content.

All the files in this folder are prefixed with "boostrap-" except one file which is named "wordpress.sql". The "wordpress.sql" is the foundation of every bootstrap file - it is responsible for creating and configuring the entire default WordPress environment so you don't have to. The rest of the files, those prefixed with "bootstrap-" are site-specific MySQL files that allow you to quickly import data for a given site.

These files are versioned controlled so it's generally not a good idea to make changes or modifications to these files - once they are created there aren't many reasons to update them. Of course if something needs to be fixed you should do so, or if a new content type is created that should be added, but don't make a habit of updating these files.

Creating a new WordPress site
-------------------------------------------------
When the time comes to create a new WordPress site you should do the following.

1. Save a copy of your entire database somewhere safe using the `mysqldump` command or your GUI of choice.
2. Next, import the `wordpress.sql` file into your database. This will reset your environment to the default state which is what we want!
3. Create a new site from the Network Admin screen and set the appropriate configuration settings. **Note:** only configure the necessary options. **DO NOT** enable any themes or activate any plugins; you'll add those in the bootstrap file.
4. Open your new site in a new tab/window and make sure everything looks as it should. The default theme should be displayed, the title should say the name of your site, etc.
5. Open your MySQL client or open up a terminal and run a mysqldump on the entire database saving the result to the `wordpress.sql` file.
6. Drop all the tables from your database and import the "wordpress.sql" file to test that there are no errors in the file. Once everything looks fine in MySQL, check the site out in your browser to make sure WordPress is happy.
7. Commit your changes with a message stating that you've added a new site to the default configuration.
8. If you are not planning to immediately create a bootstrap file then you should push your changes to a remote branch and submit a pull-request to integrate your updates into the master branch.

Creating a new bootstrap file
-------------------------------------------------
When the time comes to create a new bootstrap file you should do the following.

1. Save a copy of your entire database somewhere safe using the `mysqldump` command or your GUI of choice.
2. Next, import the `wordpress.sql` file into your database. This will wipe out everything (which is why we made a backup). **Do not skip this state - doing so may lead to inconsistencies and issues down the line.**
3. At this point you should have already added your site to the `wordpress.sql` file so your site should already be configured. If this is not the case, start from step #3 under "Creating a new WordPress site".
4. Enable the theme used for the site and activate any necessary plugins.
5. Configure menus, widgets, theme settings and add content to the site as need.
6. Once the site looks fleshed out, go to your database and select all the tables that belong to your site (e.g. wpms_#_posts, wpms_#_postmeta, etc.) in addition to any tables that were created for your site by any plugins you activated. Export these tables as a mysqldump to a file named after your site with the prefix `bootstap-`.
7. Commit your changes with a message stating that you've added a new bootstrap configuration.
8. Push your changes to a remote branch and submit a pull-request to integrate your updates into the master branch.
