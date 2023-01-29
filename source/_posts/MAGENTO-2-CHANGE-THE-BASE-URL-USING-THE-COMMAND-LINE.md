---
title: 'MAGENTO 2: CHANGE THE BASE-URL USING THE COMMAND LINE'
date: 2017-03-10 14:21:45
tags:
    - 电商
    - Magento 2
categories:
  - 技术

---
The following Magento 2’s CLI command will update the Magento base-url and the base-url-secure values.

Go to the Magento’s root directory, then type within the console:

    php bin/magento setup:store-config:set --base-url="http://localhost:8080/"
Replacing http://localhost:8080/ with your new base-url.

You may want to change the base-url-secure also:

    php bin/magento setup:store-config:set --base-url-secure="https://localhost:8080/"
Note: both base-url and base-url-secure values must contain the URL’s scheme, http:// or https://, and a trailing slash /.

Then clear the cache:

    php bin/magento cache:flush
Troubleshooting
Clear current values from the database
Can happen that the above command doesn’t works as expected and you still have some url pointing to the old base-url. In these cases you have to clear some values in your db.

Open the Magento 2 database with your favorite MySQL tool then go to the core_config_data table.

Search for rows having these values in the column path (note that there could be more than one row for each value):

- "web/unsecure/base_url"
- "web/secure/base_url"
Delete these rows (Magento will recreate them).

Now you can set the base-url value using the above CLI command.

Single-Store Mode option enabled
If you have the Single-Store Mode option enabled this could bring to some problem with setting the base-url with the command line.

In this case you should modify the base-url using only the command line and not the Magento’s Admin Panel. If you already saved the Base url field value using the Admin Panel you should clear values within the Magento’s core_config_data table as described above.
