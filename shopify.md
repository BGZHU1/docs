# Developing with the FINDMINE Shopify Demo Store

The FINDMINE Shopify demo store has the FINDMINE javascript module live on its product pages, and can be used for development purposes.

For example, perhaps you want to simulate a scrape of an unseen item for end-to-end testing of scrape functionality with some development code.

**Note: You will need an active shopify administrator account in order to configure the demo store. If you do not have an active shopify administrator account, email angela.fox@findmine.com**

### Delete the item you intend to scrape

First, you will need to delete the shopify store item from the dev.findmine.com/admin page. This means that when you visit the item, it will be 'unseen', triggering a scrape.

### Tunneling on Dev1

Then, configure your local instance of FINDMINE that you want to test by tunneling on dev1.

### Configure Shopify

First, navigate to the shopify admin portal, by visiting ```https://dev-findmine.myshopify.com/admin/``` 

Once you have logged in, select ```Online Store``` from the Shopify menu on the left hand side of the page.

Next, select ```Themes``` from the ```Online Store``` sub menu.

Click the ```more``` button (with three dots), and select the option ```Edit HTML/CSS``` 

Select the file ```theme.liquid``` from the ```Layout``` directory

Adjust the following code block to point it to your tunneling IP address. For example, if you are tunneling on port 11000, you should change this block from this:

```shell
{% if template contains "product" %}
{{ 'https://dev.findmine.com/static/css/findmine.css' | stylesheet_tag }}
{{ 'https://dev.findmine.com/findmine-latest.js' | script_tag }}
{% endif %}
```

to this:

```shell
{% if template contains "product" %}
{{ 'https://dev11000.findmine.com/static/css/findmine.css' | stylesheet_tag }}
{{ 'https://dev11000.findmine.com/findmine-latest.js' | script_tag }}
{% endif %}
```

### Modifying ```match()``` and ```render()```

If you want to modify the parameters passed in the ```match()``` and ```render()``` functions, select the file ```product.liquid``` from the ```Templates``` directory in the Themes menu.

On ```line 175``` of the file ```product.liquid``` you will see the ```findmine.match()``` function, where you can modify the parameters as desired.

### Conclusion

If you have any questions or issues with this documentation, please email angela.fox@findmine.com.



