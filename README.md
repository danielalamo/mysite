###SalesForce
Start by visiting `<base>/salesforce/grant_access`. You'll be prompted to authorize the application by loggin into your organization.

###Test




###Test2

###Infusion Soft
The project uses [infusionsoft](https://developer.infusionsoft.com/docs) and their [PHP-iSDK](https://github.com/infusionsoft/PHP-iSDK)
for email-marketing automation handled by `infusion_helper.php` and `Infusion.php (library wrapper)`

####the elements
In the views, the element(s) should have "infusionsoft" as a class and have a `data-infuse=""` attribute that holds
the tag name (from `infusion_helper.php`) you want to assign to the User upon a form's submit. For Example:
```html
    <a class="someclass infusionsoft" data-infuse="tools">
```
would assign `tools` tag to the infusionsoft contact.

####the forms
Then, for the form you can add a hidden input element with `name="infusion_tag"` and call the `infuse()` jQuery plugin
on it to make it listen to all the `data-infuse` elements on that page. For Example:
```html
    <form action="user/register" method="post">
      <input type="text" name="first_name">
      <input type="text" name="last_name">
      <!-- Adding "infusion_tag"-->
      <input type="hidden" name="infusion_tag" value="">
      <!-- See script below -->
    </form>
    <script>
        $(document).ready(function(){
            $('input[name="infusion_tag"]').infuse();
        });
    </script>
```
####Tracking User Landing Pages
Whithin some of the wela app controllers, you will find `infuse_url` routes which are called by InfusionSoft based on user clicks on email links. 

###Doctrine Integration
####Creating new Entities
Please read the [documentation on annotations](http://doctrine-orm.readthedocs.org/en/latest/reference/annotations-reference.html) to create new entities and [working with objects documentation](http://doctrine-orm.readthedocs.org/en/latest/reference/working-with-objects.html)

Whenever you create a new entity or update an existing one, you can sync your bd using this command:
```terminal
cd application/libraries;
vendor/doctrine/orm/bin/doctrine orm:schema-tool:update --dump-sql
vendor/doctrine/orm/bin/doctrine orm:schema-tool:update --force
```

Notice that the option  "--dump-sql" will only show the SQL statements needed to update the DB while the option "--force" actually updates the DB.


###Grunt Angular
Run `grunt uglify:angular` to get the min version of the app.
