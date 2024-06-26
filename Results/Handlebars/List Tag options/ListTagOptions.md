
# Tags display options #

This is a few options for displaying tags in a list layout.

Out of the box the tags looks like this:

![Outofthebox](/Results/Handlebars/List%20Tag%20options/asserts/OutOfTheBox_tags.png)

## Simple layout ##
The List Tags Simple option looks like this by adding a div and a class handle the CSS:


![Simple](/Results/Handlebars/List%20Tag%20options/asserts/simple_tags.png)


## Advanced layout ##
The List Tags Advanced option looks like this by adding a div and a class handle the CSS:

The List Tags Advanced option looks like this using the same css class as the simple option and making it a link:

```html
<div class="tagstest">
    <i class="ms-Icon ms-Icon--MapPin" aria-hidden="true"></i>
    <a href="?q=Tags:'{{trim tag}}'" data-interception="off">{{tag}}</a>
</div>

```

![Advanced](/Results/Handlebars/List%20Tag%20options/asserts/advanced_tags.png)

