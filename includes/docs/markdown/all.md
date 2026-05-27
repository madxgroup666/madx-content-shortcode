
# Cache

---

Use `[cache]` to cache page sections.

~~~
[cache name=unique_name expire='1 day']
  ...
[/cache]
~~~

It can be useful for saving query loop results to improve page load speed.

---

This feature uses the [Transients API](http://codex.wordpress.org/Transients_API) to store the content with an expiration time. When the cache expires, it is updated the next time the page is displayed.

Please note that the cache only stores the HTML output. Shortcodes with JavaScript functionality (sliders for example) may not work when cached.

### Parameters

> **name** - unique name for the cache: use lowercase alphabets, no spaces; max 40 characters

> **expire** - how often the cache is updated: *minutes*, *hours*, *days*, *years*; default is *10 minutes*

> **update** - set *true* to force update the cache

>> Note: if update is always set *true*, it will update every time and never load content from cache. Set it once, display the page to update the cache, then remove the parameter.


&nbsp;

### Cache a loop

~~~
[loop type=post count=5 cache=true expire='1 day']
  [field title-link]
  [field thumbnail]
[/loop]

~~~

A unique cache name is automatically generated based on the query parameters.

&nbsp;

## Timer

This is a little tool to measure performance.

~~~
[timer start]
  ...
[timer stop]
~~~

You can see the time it takes to render a page section, number of database queries and amount of memory used.

---

Here are the commands available:

> **start** - start the timer (displays nothing)

> **stop** - stop the timer and display time, memory used and number of queries

> **info** - display current resource info from top of page

---

You can time a loop directly.

~~~
[loop type=post timer=true]
  ...
[/loop]
~~~

This shows the info at the end of the loop.

---

#### Note

If you need more extensive measurements, [Query Monitor](https://wordpress.org/plugins/query-monitor) is recommended.
# Extras

---

## Today

To display today's date:

~~~
[today]
~~~

This uses the default date format, set in the admin panel under Settings -> General.

---

To use other formatting:

~~~
[today format=Y-m-d]
~~~

This displays a date like: `2016-08-31`

You can use it to display the time also:

~~~
[today format='Y-m-d H:i']
~~~

For details, see [the Codex: Formatting Date and Time](https://codex.wordpress.org/Formatting_Date_and_Time).

Note: shortcode parameters cannot handle a backslash, so use double slashes `//` to escape.

## Format

Use `[format]` to format a number, date, or string.

### Number

~~~
€ [format decimals=2 point=, thousands=.] [field product_price] [/format]
~~~

At least one of the following parameters must be set.

> **decimals** - number of decimal points to include; default is 0

> **point** - separator for the decimal point; default is "."

> **thousands** - separator for thousands; default is ","

> **currency** - [currency code](http://en.wikipedia.org/wiki/ISO_4217#Active_codes) (*EUR*, *USD*, ...) to use a predefined format for the above three parameters. The currency symbol is not included in the output.

> ~~~
> € [format currency=EUR][field price][/format]
> ~~~

### Date

> **date** - *default*, *relative*, or [date format](https://codex.wordpress.org/Formatting_Date_and_Time), for example *Y-m-d*

> **in=timestamp** - if the input value is a timestamp

### String

> **slugify** - The Example Title -> the_example_title

> **unslugify** - the_example_title -> The Example Title

> **ucfirst** - Uppercase first letter

> **ucwords** - Uppercase words

> **plural** - Make an English word into plural form

## Variables

This stores content in a variable with a given name:

~~~
[set any_name]Content of variable[/set]
~~~

To display it:

~~~
[get any_name]
~~~

To pass it:

~~~
[pass vars]{ANY_NAME}[/pass]
~~~

---

If you have the Math module enabled under [Settings](options-general.php?page=ccs_reference&tab=settings), these variables are shared with the `[calc]` shortcode.

## Site name and description

~~~
[field site=name]
[field site=description]
~~~

## Comment

Use `[note]` to place a comment inside the visual editor.

~~~
[note] Here is a message for myself and others. [/note]
~~~

This shortcode does not display anything, it's just a placeholder.

## Random number

Use `[random]` to display a random number in a chosen range.

*Show a random number between 1 and 8*

~~~
[random 1-8]
~~~

Use `[pass]` if you need to pass a random number to a shortcode parameter.

~~~
[pass random=1-8]
  [shortcode parameter='example-{RANDOM}.jpg']
[/pass]
~~~

# If post..

---

Use `[if]` to display content based on post conditions.

~~~
[if category=recommend]
  Must watch!
[/if]
~~~

&nbsp;

## Parameters

### Post

> **type** - post type

> **name** - post name/slug

> **author** - post author ID or user name; set to *this* for current user

> **comment_author** - comment by author ID or name; use inside comments loop

> **parent** - slug or ID of parent

> **format** - post format; if no value is set, checks if any post format exists

### Category, tag, taxonomy

> **category** - if post is in category

> **tag** - if post has tag(s)

> **taxonomy** - name of taxonomy to query

> **term** - if post has specific taxonomy term(s); if no term is set, checks if any term exists


### Field value

> ~~~
> [if field=product_color value=green]
> ~~~

> **field** - name of field to query

> **value** - if post has value(s) in the specified field; if no value is set, it will check if any field value exists

> **start/end** - use instead of **value** to check only the beginning or end of field value

> **lowercase** - set to *true* to compare lowercased version of field value

> **empty** - set to *false* when using dynamic values which could be empty, for example, with `[pass]`

> **compare** - *or* (default: any of the given values), *and* (all values), *not*

### Search field value

> ~~~
> [if field=title,content contains='some keywords']
> ~~~

> **field** - name of field(s) to search

> **contains** - words to search inside the field

> By default, it checks if *all* words exist in the field value, regardless of order or case.

> **compare=or** - search for *any* of the words

> **exact=true** - search for exact phrase

> **case=true** - search case sensitive

### Date field

~~~
[if field=date value=today]..[/if]
[if field=date before='1 week ago']..[/if]
~~~

> **value** - predefined values: today, future, past, 'future not today', 'past and today'

>> For date/time fields: now, future-time, past-time

> **before**/**after** - if field value is before/after a relative or specific date: *+10 days*, *2 weeks ago*, or *2015-02-01*

>> **field_2** - when using *before* or *after* with a relative date, you can set another field to be the reference; default is now

>> **compare** - optionally set to '<=' (before including reference) or '>=' (after)

> **date_format** - for a custom date field, default is 'Ymd'

> **in=timestamp** - compare custom date field as timestamp; same as *date_format=U*


### User field

> **user_field** - name of user field to query

> **value, start, compare** - see above for field value



### Multiple values

For category, tag, taxonomy, field, user field, or post format, you can query for multiple values.

*Science fiction **or** comedy*

~~~
[if category=sci-fi,comedy]
~~~

*Science fiction **and** comedy*

~~~
[if category=sci-fi,comedy compare=and]
~~~


### If it exists

~~~
[if attached]
  There are attachments.
[/if]
~~~

> **attached** - if the post has any attachments

> **comment** - if the post has any comments

> **image** - if the post has a featured image

> **sticky** - if post is sticky

> **gallery** - if the post has any image in the gallery field

> **field** - if the post has any value in this field

> **field=excerpt** - if the post has excerpt

> **taxonomy** - if the post has any term in this taxonomy


### Loop conditions

Use these inside the loop.

~~~
[if empty]Nothing found[/if]
[if first]First post[/if]
[if last]Last post[/if]
[if every=3]Every 3 posts[/if]
[if count=3 compare=more]After 3rd post[/if]
~~~

> **empty** - if the loop is empty

> **first, last** - if it's the first or last post found

> **count** - check current loop count; optionally set *compare* parameter: *more*, *less*, `>=`, `<=`

> **total** - check total post count, optionally with *compare* parameter

> **every** - for every number of posts in the loop; set *first* or *last* to *true* to include first/last post

>> This can be used, for example, to group four posts at a time.
>>
>> ~~~
>> [loop type=post]
>>   [if every=5 first=true]<div class="group-container">[/if]
>>   [field thumbnail]
>>   [field title]
>>   [if every=4 last=true]</div>[/if]
>> [/loop]
>> ~~~


### Not

> **not** - when the condition is not true, for example: `[if not first]`


### And

> **and** - when multiple conditions must be met

> ~~~
> [if category=apple and field=status value=ready]
> ~~~


### Else

Use `[else]` to display something when the condition is false.

~~~
[if tag=discount]
  On Sale!
[else]
  Regular Price
[/if]
~~~


## Nested

For nested conditions, use the minus prefix.

~~~
[loop type=product]
  [if category=books]
    The book [field title-link] is
    [-if field=status value=in-stock]
      in stock.
      [-else]
      not available.
    [/-if]
  [else]
    [field title-link] is not a book.
  [/if]
[/loop]
~~~

You can nest up to 5 levels.


## Other conditions

### If a field value exists

To check if a field has any value, use the *field* parameter without specifying a value.

*Display only products that have serial numbers*

~~~
[loop type=product]
  [if field=serial_number]
    Product: [field title]
    Serial #: [field serial_number]
  [/if]
[/loop]
~~~

If you specify the *value* parameter, it will check for that specific value only.



### If a taxonomy term exists

To check if there's any term in a given taxonomy, use the *taxonomy* parameter without specifying a term.

*Display tags if any exists*

~~~
[loop type=book]
  Book: [field title]
  Author: [field author_name]
  [if taxonomy=tag]
    Tags:
    [for each=tag trim=true]
      [each name-link],
    [/for]
  [else]
    There's no tag.
  [/if]
[/loop]
~~~



### Passed value

> **pass** - the value being passed

> **value** - the value to compare

> **empty=false** - return false if *pass* parameter is empty

This is for checking values passed with the `[pass]` shortcode.

~~~
[pass global=query fields=tag]
  [if pass='{TAG}' value=news empty=false]
    The value "news" was passed.
  [/if]
[/pass]
~~~

If you don't specify the value, it will check if the pass value is not empty.

~~~
[if pass='{TAG}']
  Some value was passed.
[else]
  No value was passed.
[/if]
~~~

### Variable

This is for checking variables used by `[get]`, `[set]` and `[calc]` shortcodes.

> **var** - variable to check

> **value** - the value to check

### If enclosed content exists

~~~
[if exists]
  [field optional_field]
[else]
  [field default_field]
[/if]
~~~

This will display the enclosed content if it's not empty, otherwise display the else clause.

### URL route

If the current URL is: `example.com/article/category/special`

~~~
[if route=article/category/special]
  This is a category archive of special articles.
[/if]
[if route_1=article]This is an archive of articles.[/if]
[if route_2=category]This is a category archive.[/if]
~~~

The route value supports wildcards and negatives.


> \* - will match any value

> \*\* - will match the rest of the route

> **!** - start a value with ! to match if it's not equal

---

You can also check the server host name:

~~~
[if host=example.com] Public site [/if]
[if host=staging.example.com] Staging site [/if]
[if host=localhost] Local development site [/if]
~~~

### Day of week

~~~
[if day_of_week=1]
  Today is Monday.
[/if]
~~~

Check which day of the week it is: 1~7 is Monday~Sunday.

## Switch/when

Use the following syntax to check an `[if]` condition against several values.

~~~
[switch type]
  [when post]This is a post[/when]
  [when page]This is a page[/when]
[/switch]
~~~

### Switch

The `[switch]` shortcode takes one parameter.

~~~
[switch category]
[switch tag]
[switch format]
~~~


For field or taxonomy:

~~~
[switch field=field_name]
[switch taxonomy=tax_name]
~~~

### When

Use `[when default]` for when none of the other values match.

~~~
[when default]
  This is some other post type
[/when]
~~~

To match multiple values, separate with `or`. This will check for any of them.

~~~
[when post or page]
  This is a post or page
[/when]
~~~

To check the start of a value, use *start=value*.

### Switch route

You can use switch to achieve a basic URL routing.

~~~
[switch route]
  [when product or service] Product or service archive [/when]
  [when product/* or service/*] Single product or service [/when]
  [when default] Other [/when]
[/switch]
~~~

# Is user..

---


Use `[is]` to display content based on user status.

*Display user status*

~~~
[is admin]You are an administrator.[/is]
[is author]You are the author of current post.[/is]
[is login]You are logged in.[/is]
[is logout]You are logged out.[/is]
[is user=john]You are John.[/is]
[is role=subscriber]You are a subscriber.[/is]
~~~



### Parameters

> **admin** - user has admin capability (manage options)

> **author** - user is author of current post

> **login** - user is logged in

> **logout** - user is logged out

> **user** - user name or ID - multiple values are possible: 1,3,22

> **role** - user role - default roles are *administrator*, *editor*, *author*, *contributor*, or *subscriber*

> **capable** - user capability, for example: *capable=manage_options*

>> Multiple values are possible: *role=admin,subscriber* will be true if the user is *admin* or *subscriber*.

>> See the list of available user roles and capabilities under [Dashboard -> Content -> User Roles](index.php?page=content_overview#user-roles).



### Else or not

You can use `[else]` or `[is not]` to display something when the condition is not true.

*Logged in or logged out*

~~~
[is login]
  You are logged in.
[else]
  You are not logged in.
[/is]
~~~

*User is not admin*

~~~
[is not admin]
  You are not admin.
[/is]
~~~

# Load

---


Use `[load]` to include a file into a page.

*Load a shortcode template*

~~~
[load file=template/sidebar.html]
~~~

By default, it looks for the file in the current theme directory.

*Load a file from a different location*

~~~
[load dir=views file=product/new-products.html]
~~~



### Parameters

> **dir** - load from a directory

> - *web* - http://

> - *site* - site address

> - *wordpress* - WordPress directory

> - *content* - *wp-content*

> - *theme* - *wp-content/theme*

> - *child* - *wp-content/child_theme*

> - *views* - *wp-content/views*

> **format** - set *true* to format the file with line breaks and paragraph tags

> **shortcode** - set *false* to disable shortcodes


&nbsp;

## Auto-load fields

There are special fields that are automatically loaded into the page.

### Head - CSS

Create a custom field named *css*, and the field will be included in the document head.

Inside this field, you can load a CSS file, direct styles, or meta tags.

*Load a page-specific stylesheet*

~~~
[load css=landing-page.css]
~~~

By default, it looks for the stylesheet in the *css* folder of the current theme directory.

*Load a stylesheet from a different location*

~~~
[load dir=content css=includes/css/font-awesome.min.css]
~~~

*Direct CSS*

~~~
[css]
.entry-content {
  background-color: black;
}
[/css]
~~~


&nbsp;

### Google Fonts

Use the *gfonts* parameter to include fonts from Google Fonts.

*Include fonts and apply them to page elements*

~~~
[load gfonts=Lato|Lora:400,700]
[css]
h1, h2 { font-family: Lora, serif; }
p { font-family: Lato, sans-serif; }
[/css]
~~~

This should be placed in the *css* field.


&nbsp;

### Foot - JS
Create a custom field named *js*, and the field will be included at the foot of document body.

Inside this field, you can load JS file or script.

*Load a JavaScript file*

~~~
[load js=slider.js]
~~~

By default, it looks for the file in the *js* folder of the current theme directory.

*Direct JS*

~~~
[js]
jQuery(document).ready(function($){
  console.log('Here I am!');
});
[/js]
~~~

You can also load the script in the header by placing it in the *css* field.

&nbsp;

### HTML

Create a custom field named *html*, and the field will be displayed *instead of* the post content.

This can be useful for pulling the post editor's content into a shortcode template.

*Wrap the content*

~~~
<h1>[field title]</h1>
[field author] - [field date]
[content]
~~~
# Pagination

---


To display the results of `[loop]` in pages, set the *paged* parameter.

~~~
[loop type=post paged=5]
  [field title]
  [content]
[/loop]
~~~

This defines the number of posts per page.

Optionally, set *maxpage* for the maximum number of pages.

---

Then, use `[loopage]` where you want to show the pagination.

~~~
[loopage]
~~~

There is no styling applied to the pagination. It's up to your theme's CSS.



### Parameters

> **class** - add a class to the pagination

> **anchor** - add anchor link to the page URLs

> **prev_next** - show previous/next links; default is *true*

> **prev_text** - link to previous page; default is *&amp;laquo;* or &laquo;

> **next_text** - link to next page; default is *&amp;raquo;* or &raquo;

> **show_all** - show all page numbers, instead of near the current page; default is *false*

> **end_size** - how many page numbers on start/end of pagination; default is 1

> **mid_size** - how many page numbers to either side of current page; default is 2

> **list** - set *true* to show pagination as a list; also compatible with Bootstrap

> **page** - manually set current page


### Custom query variable

The pagination URLs may conflict with existing permalinks. In that case, you can try using a custom query variable, with the *query* parameter on both `[loop]` and `[loopage]`.

~~~
[loop type=post paged=5 query=paged]
  [field title]
  [content]
[/loop]

[loopage query=paged]
~~~

The name of the variable should be something other than `page` or `p`.

### Current and total

You can display the current page and total number of pages, *after* the loop.

~~~
Current page: [loopage-now]
Total pages: [loopage-total]
~~~

The current page can also be displayed *before* the loop, using:

~~~
[loopage-now before]
~~~

### Complete example

~~~
[loop type=post paged=5]
  [field title]
[/loop]
Page [loopage-now] of [loopage-total]
[loopage prev_next=false show_all=true]
~~~

### Previous and next page

You can display links to previous/next page, separate from the page numbers.

~~~
[loopage-prev text='Go to previous page']
[loopage-next text='Go to next page']
~~~

These can only be used *after* the loop.

---

**First and last page**

If the current page is the first page, then `[loopage-prev]` will display nothing. If the current page is the last page, then `[loopage-next]` will display nothing. Set parameter *else* to display something when there is no previous/next page.

### Anchor links

If the loop is displayed further down the page, you might want to use anchor links.

~~~
<a name="start-loop"></a>
[loop type=post paged=5]
  [field title]
[/loop]
[loopage anchor=start-loop]
~~~

When you switch to another page, it will display starting at the anchor.

&nbsp;


## Permalink

To use permalinks with loop and pagination, there are three fields in the [settings](options-general.php?page=ccs_reference&tab=settings).

> 1. **Permalink slug** is the URL route to add pagination.

> 2. **Pagination slug** is added to the route above; for example, *page* will add `/page`.

> 2. When the permalink is rewritten, it overrides the **default query**. Provide a query string in the settings, to set the query to the correct template and content. For information on building a query string, see [Codex: Query Vars](https://codex.wordpress.org/WordPress_Query_Vars).

---

For example, if the loop is on a page named *hello-world*:

> 1. The permalink slug is: `hello-world`

> 2. The query string is: `pagename=hello-world`

> This will add permalinks like: *example.com/hello-world/page/2*

Multiple permalink slugs and query strings can be given, separated by comma.

---

Then, for the loop where you want pagination, add the parameter *query=default*.

~~~
[loop type=article paged=5 query=default]
  ...
[/loop]

[loopage]
~~~

---

Custom pagination permalinks do not work in archive pages, which are paginated by default.

This feature is still under testing and development. Please post feedback in the [support forum](http://wordpress.org/support/plugin/custom-content-shortcode).

&nbsp;

## Previous / next post

Use `[prev]` and `[next]` to get the previous/next post in the loop.

This can be used to display navigation links, without pagination.

~~~
[loop type=article orderby=title count=1]
  Current article: [field title]
  [prev]
    Previous: [field title-link]
  [/prev]
  [next]
    Next: [field title-link]
  [/next]
[/loop]
~~~

&nbsp;

### Order by date

Note that in the above example, the posts are ordered by title alphabetically, in ascending order. So `[prev]` means closer to "A" and `[next]` is closer to "Z".

However, by default, posts are ordered by date in descending order, from **new to old**. This means `[prev]` will get a *newer* post and `[next]` will get an *older* post, further down the loop. It's counter-intuitive, so to avoid this confusion, use `[older]` and `[newer]` when ordering by date.

```
[loop type=article]
  Current article: [field title]
  [older]
    Previous: [field title-link]
  [/older]
  [newer]
    Next: [field title-link]
  [/newer]
[/loop]
```

&nbsp;

### Outside the loop

When outside the loop, use `[prev-next]` to prepare previous/next posts.

```
[prev-next]
  [older]
    Previous: [field title-link]
  [/older]
  [newer]
    Next: [field title-link]
  [/newer]
[/prev-next]
```

Basically, `[prev-next]` creates a loop of the current post type, to make it possible to find the previous/next post in relation to the current post.

You can also pass it the same parameters as `[loop]`. This can used, for example, to get previous/next post in the same category, ordered by title.

```
[prev-next category=this orderby=title]
```

# Pass

---

Use `[pass]` to pass a field value to another shortcode's parameter.

*Display a list of posts based on field value*

~~~
[pass field=address]
  [loop type=store field=address value='{FIELD}']
    [field title-link]
  [/loop]
[/pass]
~~~

*Pass a field value to another plugin's shortcode*

~~~
[pass field=gallery]
  [isotope_gallery ids='{FIELD}']
[/pass]
~~~

&nbsp;

## Pass and if

To check the passed value, combine with an `[if]` statement.

~~~
[pass field=some_field]
  [if pass='{FIELD}' value=blue]
    It's blue.
  [/if]
[/pass]
~~~

---

If you're passing a value that could be empty, set *empty=false*.

~~~
[pass field=author_ids]
  [if user_field=id value={FIELD} empty=false]
    The ID field is not empty, and you're the user we're looking for.
  [/if]
[/pass]
~~~

This is necessary because when the *value* parameter is empty, the `[if]` statement by default only checks to see if the field exists.

## Multiple fields

Pass values from multiple fields using the *fields* parameter.

~~~
[pass fields=post_type_field,category_field]
  [loop type={POST_TYPE_FIELD} category={CATEGORY_FIELD}]
    [field title]
  [/loop]
[/pass]
~~~

This works in the same way as [field tags](options-general.php?page=ccs_reference&tab=loop#field-tags) for the `[loop]` shortcode.

## Field loop

You can loop through a comma-separated list stored in a field, and pass each item.

*Display products from a list of serial numbers*

~~~
[pass field_loop=serial_numbers]
  [loop type=product field=serial_number value={FIELD}]
    Product: [field title]
    Price: [field price]
  [/loop]
[/pass]
~~~

## User field


*Pass a single user field*

~~~
[pass user_field=twitter]
  Your twitter address: {USER_FIELD}
[/pass]
~~~

For a list of default user fields, see [the `[user]` shortcode](options-general.php?page=ccs_reference&tab=user).

*Pass several user fields*

~~~
[pass user_fields=name,email]
  Hello {NAME}, you email is {EMAIL}.
[/pass]
~~~

This works in the same way as multiple fields, described above. The replaced tag is an uppercased version of the field name.

## Taxonomy loop

You can loop through all terms in a taxonomy, and pass each item.

*Display products from each category*

~~~
[pass taxonomy_loop=category]
  Category: {TERM_NAME}
  [loop type=product category={TERM}]
    Product: [field title]
    Price: [field price]
  [/loop]
[/pass]
~~~

The available tags are: TERM, TERM_NAME and TERM_ID.

### Parameters

> **order** - *ASC* or *DESC*

> **orderby** - *id*, *count*, *slug*, *name* (default)

> **current** - Set *true* to get terms assigned to the current post only


## List loop

This is a feature to loop through a list of items.

~~~
[pass list=blue,red,green]
  {Item} products
  [loop type=product taxonomy=color term={ITEM}]
    Product: [field title]
  [/loop]
[/pass]
~~~

`{ITEM}` is replaced by the item, for example: *blue*.

`{Item}` capitalizes the first letter, for example: *Blue*.

---

For more flexibility, you can pass multiple items for each loop.

~~~
[pass list='Beautiful blue:blue,Bright red:red,Lush green:green']
  {ITEM_1} products
  [loop type=product taxonomy=color term={ITEM_2}]
    Product: [field title]
  [/loop]
[/pass]
~~~

---

There is a quick way to create a range using the `~` character.

~~~
[pass list=A~Z]
~~~

The above will pass each letter of the alphabet.

## Array field

Use the *array* parameter to display values when the field is stored as an array.

~~~
[pass array=map_field]
  Address: {ADDRESS}
  Latitude: {LAT}
  Longitude: {LNG}
  [google_map address='{ADDRESS}']
[/pass]
~~~

The tags are uppercased versions of the array keys.

If the array is not in key-value pairs but just a series of values, use the index as key, for example: `{0}`

Set *debug=true* to print the whole array content.

## Global variable

To pass a global variable, use the *global* parameter.

*Pass a global variable*

~~~
[pass global=some_var]
  Variable value: {FIELD}
[/pass]
~~~

Use the field parameter to pass an element from an array.

*Pass a query variable*

~~~
[pass global=_GET field=q]
  Current URL request: {FIELD}
[/pass]
~~~

*Multiple fields*

~~~
[pass global=_GET fields=type,category,status]
  Queries: {TYPE}, {CATEGORY}, {STATUS}
[/pass]
~~~

*If the global variable is a nested array*

~~~
[pass global=first_array field=second_array sub=field_name]
  Here it is: {FIELD}
[/pass]
~~~

### Query variables

If you set *global=query*, you can get any query variable from the URL.

~~~
[pass global=query fields=orderby,category]
  [loop type=post orderby='{ORDERBY}' category='{CATEGORY}']
    ...
  [/loop]
[/pass]
~~~

Wrap the tags in quotes in case the query variable is empty.

Please note that some query variables influence the *main* query, so it's better to use unique names.

You can use `[if pass]` to catch when the parameter is empty.

~~~
[pass global=query fields=id]
  [if pass='{ID}' empty=false]
    [loop type=member id='{ID}']
      ...
    [/loop]
  [else]
    The parameter ID is empty
  [/if]
[/pass]
~~~

In the above example, the check is needed because the loop without an ID parameter would display all posts of the given post type.

### URL route

Use `[pass route]` to pass the current URL route or its parts.

If the current URL is: `example.com/article/category/special`

~~~
[pass global=route]
  {FIELD} is: article/category/special
  {FIELD_1} is: article
  {FIELD_2} is: category
  {FIELD_3} is: special
[/pass]
~~~

## Nested pass

Create nested levels of `[pass]` by using the minus `-` prefix.

~~~
[pass field=id]
  [loop parent=this count=1]
    [-pass field=id]
      Parent ID: {FIELD}
      Child ID: {-FIELD}
    [/-pass]
  [/loop]
[/pass]
~~~

The `{FIELD}` tag also needs a minus prefix corresponding to the nest level.

This works the same way with the *fields* parameter.

# PHP

---

There are some helper functions for use in PHP templates.


> - *do_short* - quick way to write `echo do_shortcode`
> - *start_short* - start a block of shortcodes
> - *end_short* - end the block and run it

### Examples

*Run a line of shortcodes*

~~~php
do_short('Current user: [user name]');
~~~

*Run a block of shortcodes*

~~~
<?php start_short(); ?>

[loop type=post count=3]
  [field title-link]<br>
[/loop]

<?php end_short(); ?>
~~~

&nbsp;

## The loop

Use `[the-loop]` to display posts in the default query loop.

~~~
[the-loop]
  [field title]
  [content]
[/the-loop]
~~~

---

Use `[the-pagination]` to display the default pagination.

~~~
[the-pagination]
~~~

It takes the same parameters as [the `[loopage]` shortcode](options-general.php?page=ccs_reference&tab=paged).

---

To display the current page number and total page count:

~~~
Page [page-now] of [page-total]
~~~

# Raw

---


Use `[raw]` to protect a code block in the post editor from default formatting.

~~~
[raw]
  You can safely use multi-line shortcodes in this area.
  It will not be automatically formatted with <p> or <br> tags.
[/raw]
~~~

This feature can be enabled under [Settings](options-general.php?page=ccs_reference&tab=settings).

The [Raw HTML](http://wordpress.org/plugins/raw-html) plugin has the same feature, so if you have it installed, please disable this module.

&nbsp;

### How it works

The `[raw]` shortcode works differently than a normal shortcode. When the module is enabled, it works like this:

- It removes the *wpautop* and *wptexturize* filters from *the_content*
- It adds a filter called *ccs_raw_filter*, which applies *wpautop* and *wptexturize* to the content, except for sections surrounded by `[raw]..[/raw]`

And that's how it protects the section from getting automatically formatted.

&nbsp;

### Note

Themes and plugins which override the default behavior of *the_content* - for example, a visual page builder - may not be compatible with this module.

In that case, disable the module, and load the shortcode section from outside the post editor, using one of the methods described in [Getting Started](options-general.php?page=ccs_reference&tab=start#loading).

## URL

---

Use `[url]` to generate relative URLs.

*Display an image from a relative location*

~~~
<img src="[url uploads]/assets/logo.png">
~~~

This could be useful when you're migrating sites, for example, from local server to public.

Replace absolute URLs with the `[url]` shortcode, then the link doesn't depend on where the site is located.

---

### Parameters

> **site** - site address

> **wordpress** - WordPress directory

> **content** - *wp-content*

> **uploads** - *wp-content/uploads*

> **views** - *wp-content/views*

> **theme** - *wp-content/theme* - theme directory

> **child** - *wp-content/child_theme* - child theme directory


## Login / logout links
---

Use the `[url]` shortcode to display login and logout links.

*Display a login link*

~~~
<a href="[url login]">User login</a>
~~~

*Display a logout link with redirect to home*

~~~
<a href="[url logout go='home']">Logout</a>
~~~

---

### Parameters

> **login** - login link

> **logout** - logout link

> **go** - redirect after login/logout; specify URL, post slug, or *home*

---

Here is an example using both `[is]` and `[url]` to show a login/logout link based on user status.

~~~
[is logout]
  <a href="[url login go='user-profile']">Login</a>
[else]
  <a href="[url logout go='home']">Logout Link</a>
[/is]
~~~


## Redirect
---

The `[redirect]` shortcode redirects the user to another URL.

*Redirect if visitor is not logged in*

~~~
[is not login]
  [redirect go="http://example.com/guest/"]
[/is]
~~~

---

### Parameters

> **go** - redirect to URL, post slug, or *home*

> **after** - redirect after specified time; for example: *1000 ms*, *30 sec*

---

You can also specify a relative URL by wrapping it inside.

~~~
[is login]
  [redirect][url site]/user-area/[/redirect]
[/is]

~~~

The best place to do this is at the top of the page.

# Attachment

---

Use `[attached]` to loop through attachments.

*Display all attachments of the current post*

~~~
[attached orderby=title]
  [field title]
  [field image] or [content]
  [field caption]
[/attached]
~~~

*Display 3 random thumbnails from attached images*

~~~
[attached orderby=rand count=3]
  [field thumbnail]
[/attached]
~~~


### Parameters

> **count** - number of attachments to show; default is *all*

> **offset** - offset the loop by a number; for example: skip the first 3 attachments

> **orderby** - *date* (default), *title*, *rand* (random)

> **order** - *ASC* (ascending/alphabetical) or *DESC* (descending/from most recent date)

> **id** - get attachments by attachment ID(s), even if it's not attached to current post

> **field** - get attachment ID(s) from field

&nbsp;

### Attachment fields

The following fields are available for each attachment.

> **id** - ID

> **title** - title

> **caption** - caption

> **description** - description

> **url** - URL to attachment file

> **download-url** - If the attachment is a PDF file and has preview image, field *url* shows the image URL and *download-url* will get the actual file URL

> **page-url** - URL to attachment page

> **image** - display image: *&lt;img src="~"&gt;*

> **size** - image size: *thumbnail*, *medium*, *large*, *full* (default) or custom size name

>> Create a custom image size with a plugin like [Simple Image Sizes](https://wordpress.org/plugins/simple-image-sizes/), or [`add_image_size`](http://codex.wordpress.org/Function_Reference/add_image_size)

> **thumbnail** - thumbnail

> **thumbnail-url** - thumbnail URL

> **title-link** - attachment title linked to file

> **title-link-out** - attachment title linked to file in new tab



&nbsp;

### Attachments in loop

Use `[attached]` inside a loop to display attachments of specific posts.

*Display attachment thumbnails of all posts in a category*

~~~
[loop type=post category=special]
  [field title]
  [attached]
    [field thumbnail]
  [/attached]
[/loop]
~~~



&nbsp;

### If post has attachment

Use `[if attached]` to display something if the post has any attachments.

~~~
[loop type=post]
  [field title]
  [if attached]
    This post has attachments:
    [attached][field thumbnail][/attached]
  [else]
    This post has no attachments.
  [/if]
[/loop]
~~~


&nbsp;

### Specific attachment field

Use `[attached-field]` to display a single field from a specific attachment.

*Display the URL of the second attachment*

~~~
[attached-field url offset=1]
~~~

The first parameter is the field name. You can use additional parameters, which are the same as `[attached]` - for example, *offset*, *order* and *orderby*.  If you don't specify *offset*, the first attachment will be chosen.

# Comment

---


Use `[comments]` and `[comment]` to display recent comments, or those of the current post.

*Display five most recent comments*

~~~
[comments count=5]
  [comment] by [comment author] on [comment date]
[/comments]
~~~

*Display recent comments for the current post*

~~~
[comments count=3 id=this]
  [comment] by [comment author] on [comment date]
[/comments]
~~~


&nbsp;

### Parameters

Available parameters for `[comments]` are:

> **id** - set to *this* to display comments from the current post only

> **type** - post type(s) to include; default is *all*

> **count** - number of comments to show, or set to *all*

> **author** - get comments on posts by author ID or user name; set to *this* for current user

> **status** - *approve* (default), *hold* (unapproved) or set to *all*

> **exclude** - exclude post ID

> **format** - set to *false* to prevent formatting comment content

> **words/length** - trim by number of words or characters (for comment content or post title)

> **category**, **tag** - filter by category or tag

> **taxonomy**, **term**/**term_id** - filter by taxonomy term slug or ID


&nbsp;

### Fields

Inside the comments loop, use `[comment]` to display the following fields.

> **content** - comment content; default if not parameter is specified

> **date** - date of comment

> **date_format** - format the comment date, e.g., "Y-m-d". See [the codex](http://codex.wordpress.org/Formatting_Date_and_Time) for date format syntax.

> **url** - URL of the comment

> **link** - the post title linked to the comment

> **post-url** - URL of the post where the comment is

> **title** - title of the post where the comment is

> **title-link** - post title with link to the post itself

> **author** - author name

> **author-url** - author URL

> **author-link** - author name with link to URL; if no URL, then displays just name

> **avatar** - author avatar image; optional *size* parameter (default 96, max 512)

> **count** - number of comments the post has

> **counted** - shows "No comments", "1 comment" or "X comments"

> **total** - total comment count; use after the loop is finished



&nbsp;

### Count and input form

*Display comment input form*

~~~
[comment form]
~~~

*Display number of comments for the current post*

~~~
Number of comments: [comment count]
~~~

You can also display the total comment count *after* a loop is finished:

~~~
Total number of comments: [comment total]
~~~



&nbsp;

### If post has comment

Use `[if comment]` to display something if the current post has any comments or not.

*Display comment count if the post has comment*

~~~
[loop type=post category=news]
  [if comment]
    [comment count]
  [else]
    No comment yet.
  [/if]
[/loop]
~~~


&nbsp;

### If comment author

Use `[if comment_author]` to check current comment's author in a comments loop.

Combined with a post loop, you can list newest comment by *other users* on a post that the current user commented on.

~~~
[loop type=post comment_author=this]
  [comments count=1]
    [if not comment_author=this]
      New comment on [field title] by [comment author] ([comment date])
    [/if]
  [/comments]
[/loop]
~~~

&nbsp;

### Template

You can also display comments of the current post using the theme's comment template.

*Display comment list using template*

~~~
Comment list: [comment template]
~~~

By default, the comments list is displayed by *comments.php* in the theme directory. If you want to specify a different template:

~~~
[comment template=short-comments.php]
~~~

This will look for the comments template in the child theme first, then in the parent theme.

# Content

---


Use `[content]` to display content or a field from a specific post.

*Display post content by name*

~~~
[content name=hello-world]
~~~

*Display the featured image of a page*

~~~
[content type=page name=about-me field=image]
~~~

*Display a field from a custom post type*

~~~
[content type=apartment name=lux-suite-22 field=rent-per-day]
~~~



#### With loop

When inside a loop, it can be used without parameter to show each post's content.

~~~
[loop type=post count=3]
  [field title]
  [content]
[/loop]
~~~

&nbsp;

## Parameters

### Type, name, ID

> **type** - post type: *post*, *page*, or *custom post type*; default is *any*

> **name**, **title**, or **id** -  get post by *slug*, *title*, or *ID*; default is *current post*



### Field

> **field** - name of field to display; if empty, the default is the *post content*

>> You can display custom fields as well as [predefined fields](options-general.php?page=ccs_reference&tab=field#predefined-fields).

>> When displaying a field from the current post, you can use [`[field]`](options-general.php?page=ccs_reference&tab=field) as a shortcut.


### Image field

> **image** - display an image field; for example: *image=product_image*

> **in** - type of image field: *id* (default), *url*, or *object*

> **size** - size of image: *thumbnail*, *medium*, *large*, *full* (default) or [custom defined size](http://codex.wordpress.org/Function_Reference/add_image_size)

> **width**, **height** - set both to resize image by pixels; set *size* parameter with same proportion

> **image_class** - add class to the &lt;img&gt; tag

> **alt**, **title** - additional image attributes

> **out** - if field is stored as attachment ID, output image detail: *id*, *url*, *title*, *caption*, *description*

> **nopin** - set *nopin=nopin* to prevent Pinterest pinning of image


### Author field

> **meta** - display author meta: *field=author meta=user_email*

>> Author meta fields include: *user_login, user_email, display_name, first_name, last_name, description*. See [the codex](http://codex.wordpress.org/Function_Reference/get_the_author_meta) for more.


### Taxonomy

> **taxonomy** - display *category*, *tag*, or custom taxonomy of the post: *taxonomy=product_type*

>> When displaying terms of the current post, you can use [`[taxonomy]`](options-general.php?page=ccs_reference&tab=taxonomy) as a shortcut.

>> If there is more than one term, you can use [`[for/each]`](options-general.php?page=ccs_reference&tab=taxonomy#for--each) to loop through a list of terms.

> **field** - taxonomy field to display: *name* (default), *id*, *slug*, *description*, *url*, *link*, or custom taxonomy field

> **image** - custom taxonomy image field; see description above for [image field parameters](#image-field)

> **term** - get taxonomy term by ID or slug, regardless of current post

> **term_name** - get taxonomy term by name/label



### Format

> **format** - format with `<p>` and `<br>` tags; set to *true* or *false*

>> By default, post content is formatted, and fields are not.

> **texturize** - apply text transformations like smart quotes, apostrophes, dashes, ellipses, etc. See [the Codex: wptexturize](https://codex.wordpress.org/Function_Reference/wptexturize#Notes) for details.

> **words** or **length** - trim by number of words or characters

>> Trimmed content is not formatted by default; set *format=true* if you need. Also, HTML tags are stripped from trimmed content by default; you can set *html=true* to keep HTML tags - however, trimming content with HTML tags may have unexpected results.

>> **sentence** - set *true* to trim to last sentence

>> **word** - set *true* to trim to last word

>> **until** - trim to specified characters, for example: *until=...*

> **class** - add `<div>` class to the output

> **slugify** - set *true* to create a sanitized slug from field value

> **glue** - if field is array, implode with given separator; default is a comma with space after

> **escape** or **unescape** - Escape/unescape HTML special characters; this also sets *shortcode=false* unless specified otherwise

> **shortcode** - set *true/false* for shortcodes inside post content or field

> **http** - set *true* to add `http://` in front of field value, if it's not there already

> **https** - set *true* to add `https://`

> **embed** - set *true* to embed URLs like YouTube, Vimeo, etc. By default, such URLs in post content are auto-embedded.

> **filter** - set *true* to apply *the_content* filter; this may be useful when using plugins that filter the post content, for example, [Page Builder](https://wordpress.org/plugins/siteorigin-panels).

> **currency** - format as currency; see [the field section](options-general.php?page=ccs_reference&tab=field#currency) for details

> **date_format** - use a custom date format

>> For example, "d.m.Y" will display as *18.11.2013*. Please refer to [the codex](http://codex.wordpress.org/Formatting_Date_and_Time) for the date format syntax.

>> Note: instead of backslash, use double slashes to escape characters:

>>> "Y/m/d //a//t g:i A" will show as *2013/11/17 at 11:06 PM*.

>> Use *in=timestamp* to format a unix timestamp value.

### Read more

> **more** - set *true* to display content up to the &lt;!--more--&gt; tag, or an excerpt if the tag doesn't exist. This will add the text "Read More" at the end, with a link to the post. To change the text, use: *more=...*

> **dots** - set *false* to disable dots at the end of excerpt

> **link** - set *false* to disable link to the post


### Template

> **import** - set *true* to use another post's content as a template and run its shortcodes in the context of the current post; true by default when displaying a field


### Other content types

> **area** or **sidebar** - display a widget area/sidebar by *slug*, *title*, or *ID*

> **menu** - display a menu list by *slug*, *title*, or *ID*; see also [Menu loop](options-general.php?page=ccs_reference&tab=menu) and [Bootstrap tabs and navbar](options-general.php?page=ccs_reference&tab=bootstrap).

# Field

---

Use `[field]` to display a field from the current post.

*Display fields*

~~~
[field title] by [field author] was written on [field date].
~~~

---

Additional parameters can be placed after the field name. Most [parameters for `[content]`](options-general.php?page=ccs_reference&tab=content#field) can be used.

~~~
[field title words=10]
~~~


&nbsp;

## Predefined fields

### Post

> *id* - post ID

> *slug* - post slug

> *url* - post URL

> *link* - link to post URL; set parameter *text* to change link text from post title (default) to, for example, "Read More"

> *excerpt* - post excerpt; if excerpt doesn't exist, it will display post content with *words=25*. Excerpts are unformatted, and HTML tags are stripped. See also the *words* and *length* parameter for the content shortcode.

> *after-excerpt* - content after the read more tag

> *edit-url* - post edit URL

> *edit-link* - post title with link to edit URL; set parameter *text* to change link text from post title to, for example, "Edit"

> *post-status* - post status

> *post-class* - post class

> *post-type* - post type

>> *post-type-name* - singular label

>> *post-type-plural* - plural label

> *post-format* - post format

>> *post-format-name* - post format label

>> *post-format-link* - post format label with link to archive

>> *post-format-url* - post format archive URL


### Title

> *title* - post title

> *title-link* - post title with link to the post

> *title-link-out* - post title with link to the post, in new tab: *target=_blank*


### Date

> *date* - published date

> *modified* - last modified date



### Featured image

> *image* - featured image

> *image-url* - image URL

> *image-title* - image title

> *image-caption* - image caption

> *image-alt* - alternate text

> *image-description* - image description

> *image-link* - image with link to the post

> *image-link-out* - image with link to the post, in new tab: *target=_blank*

> *thumbnail* - featured image thumbnail

> *thumbnail-url* - thumbnail URL

> *thumbnail-link* - thumbnail with link to the post

> *title=true* - set *title* attribute to post title when using *image-link* and *thumbnail-link*

### Author

> *author* - post author

> *author-id* - post author ID

> *author-url* - post author URL

> *avatar* - post author avatar

### Previous / Next

> *prev-link* - previous post in the loop (title with link)

> *next-link* - next post in the loop

## Custom field

You can display custom fields as well as predefined fields listed above. If your custom field has the same name as a predefined field, set *custom=true*.


## Image field

*Display an image field*

~~~
[field image=image_field size=thumbnail]
~~~

*Display an image field (stored as URL)*

~~~
[field image=image_field in=url]
~~~

### Parameters

> **image** - display an image field; for example: *image=product_image*

> **in** - type of image field: *id* (default), *url*, or *object*

> **size** - image size: *thumbnail*, *medium*, *large*, *full* (default) or custom size name

>> Create a custom image size with a plugin like [Simple Image Sizes](https://wordpress.org/plugins/simple-image-sizes/), or [`add_image_size()`](http://codex.wordpress.org/Function_Reference/add_image_size)

> **width**, **height** - set both to resize image by pixels; set *size* parameter with same proportion

> **image_class** - add class to the &lt;img&gt; tag

> **alt**, **title** - additional image attributes

> **return** - display image field meta: *url, id, title, caption, description*


## Link to field value

Use `[link]` to create a link using field value.

~~~
[link field=image-url]
  Click here to see [field image-title].
[/link]
~~~

The default field is URL of the current post.

~~~
[loop type=post count=3]
  [link][title][/link] is the same as [field title-link]
[/loop]
~~~

### Parameters

> **id** - link to another post by ID

> **name** - link to another post by slug; set *type* to specify post type for faster query

> **field** - name of predefined or custom field; default is *url*

> **custom** if the custom field has the same name as a predefined field, set *custom=true*

> **url** - use direct URL instead of field value; can also be used with `[pass]`

> **alt**, **title**, **target**, **class** - set link attributes

> **open=new** - same as *target=_blank*

> **protocol** - add *http*, *https*, or *telnet*, plus `://` before field value, if it's not there already

> **http**, **https** - set *true* to add `http://` or `https://`

> **download** - set *true* or file name

> **mail** - set *true* to add `mailto:` before field value

## Array

Use `[array]` to loop through an array of key-value pairs stored in a field.

~~~
[array field_name]
  [field key_1]
  [field key_2]
[/array]
~~~

### Parameters

> **each** - set *true* to loop through multiple arrays of key-value pairs

> **debug** - set *true* to print the whole array and see how it's structured

> **global** - access global variable with given name

> **json** - set *true* if field value is stored as JSON string

---

Nested arrays are supported by using the minus prefix.

~~~
[array field_name]
  [-array inner_array]
    [field key_1]
  [/-array]
[/array]
~~~

For an array that is not a key-value pair, use `[field value]` to display each value.

# Loop

---

Use `[loop]` to get posts and loop through each one.

*Display five most recent posts*

~~~
[loop type=post count=5]
  [field title]
  [field date]
  [field excerpt]
[/loop]
~~~

*Display posts from a custom post type and category*

~~~
[loop type=apartment category=suite]
  Apartment: [field title]
  Rent per day: [field price]
  Description: [content]
[/loop]
~~~

&nbsp;

## Parameters

### Type, name, ID

> **type** - post type to include; if not specified, default is *any*; multiple post types possible

> ~~~
> [loop type=article,news orderby=date]
> ~~~

> **name** - post name, or "slug"; can specify multiple with comma-separated list

> **id** - post ID to include; for example: *id=1,2,3*

> **exclude** - post ID to exclude
  - *this* - exclude current post
  - *children* - exclude child posts; display top-level posts only

> **count** - number of posts to show; default is all posts found

> **offset** - offset the loop by a number of posts; for example: *offset=3* to skip the first three

> **status** - display posts by status: *any, publish, pending, draft, future, private*

> **sticky** - set *true* to include sticky posts


### Author

> **author** - show posts by author ID or login name
  - *this* - current user
  - *same* - same author as current post

> **author_exclude** - exclude posts by author ID or login name

> **role** - show posts by user role, such as *administrator*, *editor*, *subscriber*
  - *this* - current user's role

> **comment_author** - show posts which have comments by this author ID or name
  - *this* - current user
  - *same* - same author as current post

### Published date

> **year, month, day** - display posts by specific year, month, day
  - *this* - for example: *month=this* will show posts from this month

> **before** - display posts before a relative or specific date
  - Example: *before=today*, *before=2015-02-01*

> **after** - display posts after a date



### Category, tags

> **category** - display posts from one or more categories; for example: *category=sports,fashion*

> - *this* - find posts in the same category as current post

> **tag** - display posts with one or more tags; for example: tag=apples,green

> - *this* - find posts with the same tag as current post

> **compare** - for multiple categories/tags, set *compare=and* to get posts that have all terms



### Taxonomies

> **taxonomy, term** - display posts by taxonomy term

> ~~~
> [loop type=product taxonomy=product-type term=book]
>   [field title]
> [/loop]
> ~~~

> **compare** - set to *not* to exclude posts by taxonomy term; if using field compare at the same time, use *tax_compare*

---

> #### Post format

> ~~~
> [loop type=post taxonomy=format term=audio]
> ~~~

---

> #### Multiple terms

> Multiple terms may be specified, such as *term=book,lamp*.

> By default, this gets posts whose taxonomy contains *any* of the given terms.
  - Use *compare=and* to get posts whose taxonomy contains all terms
  - Use *compare=not* to get posts whose taxonomy does not contain the term(s)

---

> #### Multiple taxonomies

> ~~~
> [loop taxonomy=color term=blue relation=or taxonomy_2=size term_2=small]
> ~~~

> **relation** - additional taxonomy query, where relation is *and* (default) or *or*

> **taxonomy_2, term_2, compare_2, taxonomy_3, term_3, compare_3, ...**

### Field value

> **field** - field name

> **value** - field value(s) - if multiple, will match any: *value=this,that*

> **start** - use instead of *value* to check only the beginning of field value

> **compare** - *equal* (default), *not*, *more*, *less*, or operator like &lt; and &gt;. If using taxonomy compare at the same time, use *field_compare*

> **compare=between** - query for a range of values; for example, *value=0,100*

---

> #### Multiple fields

> ~~~
> [loop field=color value=blue relation=or field_2=size value_2=small]
> ~~~

> **relation** - additional field query, where relation is *and* (default) or *or*

> **field_2, value_2, compare_2, field_3, value_3, compare_3...**


### Date field

> **field** - field name

> **value** - field value

>> You can specify exact value, or use the following predefined values.
  - *today* - compare to today
  - *today-between* - when using date-and-time field, compare today as range (00:00:00~23:59:59)
  - *now* - if your field contains date and time
  - *future* - today and after
  - *future-time* - after now
  - *'future not today'* - from tomorrow
  - *past* - before today
  - *past-time* - before now
  - *'past and today'* - past including today

> **compare** - *equal* (default), *not*, *more*, *less*, or operator like &lt; and &gt;.

> **before, after** - used in place of *value* and *compare*; query for field values before/after a relative or specific date: *2 weeks ago*, or *2015-02-01*

> **date_format** - date format of the field value; default is 'Ymd' - for a date-and-time field, set it to 'U' or use *in=timestamp*

> #### Multiple date fields

> **field_2, value_2, compare_2, date_format_2, after_2, before_2...**


### Parent

> **parent** - display all children of a parent specified by ID or slug
  - *this* - get current post's children

>> This will include all first-level children of the current page.
>> ~~~
>> [loop type=page parent=this]
>>   [field title]
>> [/loop]
>> ~~~

  - *same* - get current post's siblings (posts which share the same parent)

### Children

> **include=children** - include child posts and descendants

>> This will include all descendants of the current page.
>> ~~~
>> [loop type=page parent=this include=children]
>>   [field title]
>> [/loop]
>> ~~~
>>
>> Children are included together with their parents. When *list=true*, they will be appended after their parent.

> **level** - set maximum level of descendants to include

>> *level=1* will get only top parents, *level=2* will include their direct children, and so on.
>>
>> ~~~
>> [loop type=page level=2]
>>   [field title]
>> [/loop]
>> ~~~
>>
>> When you set a level, there's no need to add *include=children*.

> ---
> **parent=this**
>
>> You can also use a nested loop to get each descendant level separately.
>> ~~~
>> [loop type=page orderby=title]
>>   [field title]
>>   [-loop parent=this]
>>     Child page: [field title]
>>     [--loop parent=this]
>>       Grandchild page: [field title]
>>     [/--loop]
>>   [/-loop]
>> [/loop]
>> ~~~
>> Query parameters (except *id*, *name* and *parent*) of the top loop apply to inner loops. In the example above, all loops are ordered by title.

---

> **child=this** - loop through current post's parents from the top
  - *include=this* - include current post
  - *reverse=true* - reverse order: start from current post and go up

>> For example, if the current post is a grandchild page:
>> ~~~
>> [loop child=this include=this]
>>   [if not first] > [/if][field title]
>> [/loop]
>> ~~~

>> This will display: Parent > Child > Grandchild


### Order by child date

Use *orderby=child-date* to sort posts by the most recently published children.

~~~
[loop type=page orderby=child-date]

  Parent: [field title]

  [-loop parent=this orderby=date count=1]
    Most recent child: [field title] - [field date]<br>
  [/-loop]
[/loop]
~~~

---

The following parameters are available:

> **order**

>> *DESC* - descending; new to old (default)

>> *ASC* - ascending; old to new

> **parents**

>> *true* - exclude posts which have no children; by default they are placed at the end of the result

>> *equal* - posts which have no children will be compared by their publish date



### Sorting and series

> **orderby** - order posts

> - *date* - published date (default)
> - *id*
> - *author*
> - *title*
> - *name* - post slug
> - *parent*
> - *modified*
> - *comment-date* - sort by comment date
> - *child-date* - sort by most recently published children (see above section)
> - *rand* or *random* - random order
> - *menu* - menu order
> - *field* - field value as string
> - *field_num* - field value as number

>> If the value isn't any of these, it's assumed to be the field name.

> **order** - *ASC* (ascending/alphabetical) or *DESC* (default: descending/from most recent date)

> **key** - when ordering by *field* or *field_num*, specify *key* as the name of the field to use

> **orderby_2**, **order_2**, **key_2**, ... - Order by multiple fields, up to 5; custom and default fields are supported, except *comment-date*, *rand*, *menu*, *parent*

> ---

> **series, key** - order posts by a series of field values, where *key* is the name of the field; the series can include ranges, for example: *1-15, 30-40, 42, 44*.




### Checkboxes


> **checkbox, value** - display posts whose checkbox contains the value

>> Multiple values are possible: *value=first,second*. This will return all posts with checkboxes containing any of the values, i.e., first *or* second.

>> Optionally, you can set *compare=and*, which will return all posts with checkboxes containing all of the values, i.e., first *and* second.  See example section below.

> **relation** - additional query for checkbox value, where relation is *and* (default) or *or*

> **checkbox_2, value_2, compare_2, ...**



### Format

> **trim** - set *true* to remove extra white space or comma at the end

> **clean** - set *true* to remove all &lt;p&gt; and &lt;br&gt; tags

> **strip_tags** - set *true* to remove all HTML tags inside the loop

> **allow** - strip all HTML tags except allowed, for example: *allow='&lt;a&gt;&lt;li&gt;&lt;br&gt;'*




### List

> **list** - set *true* to create a list with &lt;ul&gt;, or specify tag like *ol* or *div*

> **list_class, list_style** - add class or style to the list; classes can be separated by space or comma

> **item** - tag to wrap each loop item; default is *li*, or specify tag like *span*

> **item_class, item_style** - add class or style to each item




### Pagination

> **paged** - number of posts per page

> **maxpage** - maximum number of pages

> **page** - manually set current page

> These are used with the [[loopage] shortcode](options-general.php?page=ccs_reference&tab=paged) to create pagination.




### Cache

> **cache** - set *true* to cache the result of the loop; see [Advanced: Cache](options-general.php?page=ccs_reference&tab=cache) for details

> **expire** - how often the cache is updated: minutes, hours, days, years - default is *10 minutes*

> **update** - set *true* to force update the cache

> **timer** - set *true* to display resource info at the end of loop; see [here](options-general.php?page=ccs_reference&tab=cache#timer) for details




### Other

> **blog** - switch to given blog ID on multi-site

> **columns** - display output in columns; for example, *columns=3*; for padding: *pad='0px 10px'*

> **x** - repeat the loop x times - no query

---

When querying the *tribe_events* post type from [The Events Calendar](https://wordpress.org/support/plugin/the-events-calendar) plugin, you can use the following:

> **display** - *custom* (all events), *past* (past events), *list* (default: all future events)



## Loop exists

This is a feature to perform a query first, to check if any post matches the given parameters. It displays nothing if no post is found.

~~~
[loop exists type=post category=important orderby=title]
  <h1>Important posts</h1>
  [the-loop]
    [field title-link] by [field author]
  [/the-loop]
[/loop]
~~~

Use `[the-loop]` inside to loop through the result.

---

To display something when no post is found, use `[if empty]`.

~~~
[loop exists type=post category=important]
  ...
  [if empty]<h1>No important posts</h1>[/if]
[/loop]
~~~

## Loop count

Use `[loop-count]` to display the current index of the loop, starting from 1.

This can be useful, for example, to create unique element ID or class to wrap each post.

The shortcode can also be used to display total post count after a loop is finished.

---

If you provide query parameters, it will count the result.

~~~
Total number of posts by current user: [loop-count type=post author=this]
~~~


## Field tags

This is a feature to expand a list of fields to their values.

~~~
[loop type=post fields=title,custom_field]
  Display {TITLE} and {CUSTOM_FIELD}
[/loop]
~~~

The `{FIELD}` tags are uppercased versions of the field names. You can use [predefined fields](options-general.php?page=ccs_reference&tab=field#predefined-fields) or custom fields that you've created.

If you want to pass field values to the loop shortcode itself, use [the `[pass]` shortcode](options-general.php?page=ccs_reference&tab=pass).

# Menu

---

Use `[loop]` and the *menu* parameter to loop through menu items.

~~~
[loop menu='Menu Title']
  [field title-link]
[/loop]
~~~

### Parameters

> **menu** - get menu by title, slug or ID

> **list** - set *true* to create a list

>> **ul_class** - set `<ul>` class

>> **li_class** - set `<li>` class

### Fields

For each menu item, the following fields can be displayed.

> **title** - item title

> **title-link** - item title linked to the URL

> **url** - item URL

If the menu item is a post, you can also display other post fields.

&nbsp;

## Child menu items

Set *menu=children* to loop through child menu items.

~~~
[loop menu='Menu Title']
  Parent: [field title-link]
  [-loop menu=children]
    Child: [field title-link]
  [/-loop]
[/loop]
~~~

The child menu inherits the parent's parameters unless overridden.

## Conditions

~~~
[if first]The first menu item[/if]
[if last]The last menu item[/if]
[if children]Menu item has children[/if]
[if id=this]This menu item is the current page[/if]
~~~

## Menu item classes

You can add your own classes to the menu items.

~~~
[loop menu='Main Menu']
  <div class="page_item-[field id][if id=this] current_page_item[/if]">
    [field title-link]
  </div>
[/loop]
~~~

# Post type

---

Use `[for]` to loop through post types.

~~~
[for type=all]
  Post type: [each name]
[/for]
~~~

### Parameters

> **type** - Required: set to *all*, or a comma-separated list of post types

> **public** - Set to *true* to get only public post types

> **default** - Set to *false* to exclude default post types: post, page

> **exclude** - Exclude one or more post types by slug

&nbsp;

## Each

The `[each]` shortcode is used to display post type label or slug.

Specify the field like `[each name]` with the field name as first parameter without value.

The following fields are available.

> **name** - Post type label (singular)

> **plural** - Post type label (plural)

> **slug** - Post type slug

> **prefix** - Post type rewrite slug for use in URLs

> **url** - Post type archive URL

### Parameters

> **lower** - Set to *true* to make singular/plural name into lowercase

## Single field

There is a shortcut for displaying a single field from a post type.

*Display the archive URL of a post type*

~~~
[for type=custom-type field=url /]
~~~

The `/` before the end is a short way to close the loop.

## Conditions

You can use `[if]` for conditions inside a `[for type]` loop.

Supported conditions are: *first*, *last*, *every*, *archive* (if archive exists), and *prefix* (if rewrite slug exists).

## Loop

For each post type, you can display a post loop.

~~~
[for type=post,page,product]

  Recent [each plural lower=true]

  [loop count=3]
    [field title-link]
  [/loop]
[/for]
~~~

# Taxonomy

---


Use `[taxonomy]` to display taxonomy terms from the current post.

*Display categories*

~~~
[taxonomy category]
~~~

Optionally, you can specify the *field* parameter.

*Display categories as links*

~~~
[taxonomy category field=link]
~~~

Available fields are: *name* (default), *id*, *slug*, *description*, *url*, *link*, or custom taxonomy field.

---

If there is more than one term, they will be displayed as a comma-separated list.

You can also use [`[for/each]`](#for--each) to loop through a list of terms.

To get posts based on taxonomy terms, see [loop: taxonomies](options-general.php?page=ccs_reference&tab=loop#taxonomies).

&nbsp;

## Related posts


Use `[related]` to loop through posts related by taxonomy.

*Display posts in the same category as current post*

~~~
[related taxonomy=category count=3]
  [field title-link]
[/related]
~~~

The current post is not included in the result.

### Parameters

> **taxonomy** - *category*, *tag*, or custom taxonomy; multiple values possible

> **relation** - for multiple taxonomies, set *relation=and* to get posts that share all taxonomies

> **count** - maximum number of results

> **offset** - skip the first X number of posts

> **orderby** - order by* id*,* author*,* title*,* name*,* date* (default),* rand* (randomized)

> **order** - ASC (ascending/alphabetical) or DESC (descending/from most recent date)

> **status** - display posts by status: *any, publish, pending, draft, future, private*; multiple values possible

> **children** - include posts related by child terms - *true* or *false* (default)

> **fill** - set *true* to include unrelated posts until post count is met; must set *count* parameter to work

## For / each


This is a feature to create a loop for each category, tag, or taxonomy term.

*Display 3 most recent posts for each category*

~~~
[for each=category]
  Category: [each name]
  [loop type=post count=3]
    [field title-link]
  [/loop]
[/for]
~~~

The `[for]` shortcode loops through *all existing terms* of a given taxonomy. To limit by terms associated with the current post, set *current=true*.

The `[each]`shortcode displays the term name.

Each term is also automatically passed to the `[loop]` inside.



&nbsp;

### For

Available parameters for the **[for]** shortcode are:


> **each** - *category*, *tag*, or custom taxonomy

> **current** - set *current=true* to limit by terms associated with the current post

> **count** - limit number of terms: *count=3*

> **exclude** - exclude taxonomy term by ID or slug; *exlude=uncategorized*

> **empty** - set *true* to include terms that have no associated posts; use outside the loop

> **orderby** - order by *name* (default), *count* (post count), *id*, or *slug*

> **order** - *ASC* (ascending - default), or *DESC* (descending)

> **term/terms** - specify which term(s) to get, by ID or slug; can be a comma-separated list

> **parent** - get direct children terms by parent ID or slug

> **parents** - set *true* to get only parent terms; see section below on child terms

> **children** - set *true* to get all descendants, when using *term* or *parent*

> **trim** - set *true* to trim space or comma at the end



&nbsp;

### Each

Available parameters for the **[each]** shortcode are:

> **name** - name of category, tag, or taxonomy term

> **url** - URL of the taxonomy term archive

> **link** - name of taxonomy term with link to the archive

> **slug** - term slug

> **id** - term ID

> **count** - term's post count

You can also specify a custom taxonomy field. If no parameter is set, the term name is displayed.


&nbsp;

### Inside loop

Inside a loop, the **[for]** shortcode gets taxonomy terms from the current post in the loop.

*Display a link for each category assigned to a post*

~~~
[loop type=post]
  [field title]
  [for each=category]
    [each link]
  [/for]
[/loop]
~~~



&nbsp;

### Current post

Outside the loop, the **[for]** shortcode gets all terms that have any posts associated. Use the *current* parameter to display terms of the current post only.

*Display a link for each category assigned to the current post*

~~~
[for each=category current=true]
  [each link]
[/for]

~~~


&nbsp;

### No term

To display something if there's no term found, use **[for-else]**.


~~~
[for each=category current=true]
  [each link]
[for-else]
  No category found
[/for]

~~~


&nbsp;

### Child terms

To display child terms separately from parents, use a nested **[for]** loop.

*List parent categories, with children*

~~~
[for each=category parents=true]
  Parent: [each link]
  [-for each=child]
    Child: [each link]
  [/-for]
[/for]

~~~

You can use **[if children]** to display something only if the current term has child terms.

~~~
[for each=category]
  [if children]
    Parent: [each link]
    [-for each=child]
      Child: [each link]
    [/-for]
  [else]
    [each link]
  [/if]
[/for]
~~~


&nbsp;

### List

Use the *trim* parameter to create a list of terms. It removes extra space or comma at the end.

*Display a list of categories*

~~~
[for each=category trim=true]
  [each],
[/for]
~~~

*Trim other characters*

~~~
[for each=category trim='/']
  [each] /
[/for]
~~~


&nbsp;

### Conditions

The following `[if]` conditions can be used inside a for/each loop.

> **each** - check taxonomy term slug

> **each_field** - check taxonomy term field

> **each_value** - check taxonomy term field's value; if not set, it will check for any value

---

*Display something different for a specific term*

~~~
[for each=category]
  [if not each=uncategorized]
    Category name: [each name]
  [/if]
[/for]
~~~

*Check if a taxonomy term field has any value*

~~~
[for each=photographer]
  Name: [each name]
  [if each_field=website]
    Website: [each website]
  [/if]
[/for]
~~~


&nbsp;

### Pass each term

Use `{TERM}` to pass each term's slug to another shortcode.

~~~
[for each=category current=true]
  Category: [each name]
  [loop type=post category={TERM} list=true]
    [field title-link]
  [/if]
[/for]
~~~

There are also: `{TERM_ID}` and `{TERM_NAME}`.

---

Inside a nested `[for]` loop, add the same minus prefix to pass values from each loop. For example, to display the parent term's name:

~~~
[for each=parent_category]
  Parent category: [each name]
  [-for each=children]
    [each name] is a child of {TERM_NAME}
    [--for each=children]
      [each name] is a child of {-TERM_NAME} and grandchild of {TERM_NAME}
    [/--for]
  [/-for]
[/for]
~~~

# URL

---


Use `[url]` to generate relative URLs.

*Display an image from a relative location*

~~~
<img src="[url uploads]/assets/logo.png">
~~~

This could be useful when you're migrating sites, for example, from local server to public.

Replace absolute URLs with the `[url]` shortcode, then the link doesn't depend on where the site is located.



### Parameters

> **site** - site address

> **wordpress** - WordPress directory

> **content** - *wp-content*

> **uploads** - *wp-content/uploads*

> **views** - *wp-content/views*

> **theme** - *wp-content/theme* - theme directory

> **child** - *wp-content/child_theme* - child theme directory

&nbsp;

## Login / logout links


Use the `[url]` shortcode to display login and logout links.

*Display a login link*

~~~
<a href="[url login]">User login</a>
~~~

*Display a logout link with redirect to home*

~~~
<a href="[url logout go=home]">Logout</a>
~~~



### Parameters

> **login** - login link

> **logout** - logout link

> **go** - redirect after login/logout; specify URL, post slug, or *home*



Here is an example using both `[is]` and `[url]` to show a login/logout link based on user status.

~~~
[is logout]
  <a href="[url login go=user-profile]">Login</a>
[else]
  <a href="[url logout go=home]">Logout Link</a>
[/is]
~~~


## Redirect


The `[redirect]` shortcode redirects the user to another URL.

*Redirect if visitor is not logged in*

~~~
[is not login]
  [redirect go='http://example.com/guest/']
[/is]
~~~



### Parameters

> **go** - redirect to URL, post slug, or *home*

> **after** - redirect after specified time; for example: *1000 ms*, *30 sec*



You can also specify a relative URL by wrapping it inside.

~~~
[is login]
  [redirect][url site]/user-area/[/redirect]
[/is]

~~~

The best place to do this is at the top of the page.

# User

---



Use `[user]` to display current user's info.

~~~
ID: [user id]
Full name: [user]
Login name: [user name]
E-mail: [user email]
Author archive: [user archive-link]
~~~



### Fields

> **id** - ID

> **name** - login name

> **fullname** - display name (default)

> **email** - e-mail

> **url** - website URL

> **avatar** - avatar image

> **size** - avatar image size in pixels - default *96*, max *512*

> **registered** - registered date; optionally add parameter *format=relative*, or [date format](https://codex.wordpress.org/Formatting_Date_and_Time), for example "Y-m-d"

> **post-count** - user post count

> **archive-url** - user posts archive URL

> **archive-link** - display name linked to user posts archive

> **role** - user role

> **slug** - sanitized user name for use in URL; same as user "nicename"

> **edit-url** - user profile edit URL in the admin

> **edit-link** - user profile edit link; set *text* parameter for link text: default is "Edit Profile"

### Custom user field

You can also display a custom user field.

~~~
[user field_name]
~~~

For an image field, use the *image* parameter:

~~~
[user image=field_name size=thumbnail]
~~~

You can display other [attachment fields](options-general.php?page=ccs_reference&tab=attach#attachment-fields) from the image.

~~~
[user image=field_name field=url]
~~~


&nbsp;

## Users loop

Use `[users]` to loop through users.

*Make a list of admin users*

~~~
[users role=admin]
  Admin: [user]
  Contact: [user email]
[/users]
~~~


### Parameters

> **role** - *admin*, *editor*, *author*, *contributor*, *subscriber*; supports multiple

> **include**, **exclude** - include/exclude users by ID

> **orderby** - id, name, login (default), email, url, registered, post_count, field, field_num

> **order** - *ASC* - alphabetical (default) or *DESC* (new to old)

> **count** - limit number of returned results

> **offset** - offset results by a number

> **field** - custom field to query

> **value** - field value; multiple values possible depending on *compare*

> **compare** - *equal* (default), *not*, *in*, *not in*, *between*, *not between*, or operators like >= and <=.

> **search** - search for string match on user columns

> **search_columns** - one or more columns to search: *ID*, *login*, *nicename*, *email*, *url*

> **blog_id** - blog ID on a multisite

#### Users list

> **list** - set *true* to create a list with &lt;ul&gt;, or specify tag like *ol* or *div*

> **list_class, list_style** - add class or style to the list; classes can be separated by space or comma

> **item** - tag to wrap each loop item; default is *li*, or specify tag like *span*

> **item_class, item_style** - add class or style to each item



### Sort by user field

*Field value is string*

~~~
[users orderby=field field=twitter]
~~~

*Field value is number*

~~~
[users orderby=field_num field=position]
~~~

## User field value



Use `[if user_field]` to check if a user field has specific value, or is not empty.


*If user field has specific value*

~~~
[if user_field=school value='Home Town University']
  Hey, schoolmate!
[/if]
~~~

*If user field is not empty*

~~~
[if user_field=facebook_profile]
  <a href="[user facebook_profile]">Facebook profile</a>
[else]
  No Facebook profile
[/if]
~~~

*If user has posts*

~~~
[if user_field=post-count]
  Post count: [user post-count]
[else]
  No posts yet!
[/if]
~~~

## User condition



To display something based on user condition such as ID or role, use the [`[is]` shortcode](options-general.php?page=ccs_reference&tab=is).

# Advanced Custom Fields

---

Enable the ACF module under [Settings](options-general.php?page=ccs_reference&tab=settings). The following field types are supported:

- Text, textarea, editor, [image](options-general.php?page=ccs_reference&tab=field#image-field)
- [Checkbox, select, radio](#checkbox-select-radio), [true/false](#true-false), [date](#date-field)
- [Page link](#page-link), [relationship/post object](#relationship), [taxonomy](#related-by-taxonomy-field)
- [Gallery](#gallery), [repeater](#repeater), [flexible content](#flexible-content)
- [File field](#field-stored-as-attachment-id), [cropped image](#cropped-image), [Google map](#google-map)
- [Fields from an option page](#option-page)


&nbsp;

## Field label

To display a field's label, set *out=field-label*.

~~~
[field field_name out=field-label]
~~~

## Field key

To display a field by key (starts with *field_*), use the *key* parameter.

~~~
[field key=field_5039a99716d1d]
~~~

## Checkbox/Select/Radio

### Selection label

To display the selection's label instead of value, use the following syntax.

~~~
[field select out=label]
~~~

### Multiple selections

Use `[array]` and `[field value]` to loop through multiple selections.

~~~
[array checkboxes]
  [field value]
[array]
~~~

### Choices

Use `[array choices]` to loop through available choices.

~~~
[array choices=checkbox_field]
  Option label: [field label] or {LABEL}
  Option value: [field value] or {VALUE}
[/array]
~~~

If the field is in another post type than the current post, set parameter *type* or *name*.

## True/false

To check the value of a true/false field, use the following syntax.

~~~
[if field=true_false value=1]
  It's true.
[else]
  It's false.
[/if]
~~~

## Date field



Use the *acf_date* parameter to query the date field.


~~~
[loop type=event acf_date=date_field value=future]
~~~


Use the *acf_date* parameter to display the date field with selected formatting.


~~~
[field acf_date=date_field]
~~~



### Date and time

For fields created with ACF Date & Time Picker, it works best if you save as timestamp and use the *field* parameter for loop.

~~~
[loop type=event field=date_and_time_field value=future]
~~~

To display the field formatted, you can still use the *acf_date* parameter.

~~~
[field acf_date=date_and_time_field]
~~~


## Page link


Use the *link* parameter to display a page link field. This will display the URL of a post or archive.

~~~
[field link=page_link]
~~~


## Cropped image

With [ACF Image Crop](https://wordpress.org/plugins/acf-image-crop-add-on/) add-on, use the *cropped* parameter to display a cropped image field.

*Display cropped image*

~~~
[field cropped=field_name]
~~~

*Display cropped image URL*

~~~
[field cropped=field_name return=url]
~~~

## Google map

To display a map based on a Google map field, you need a plugin or theme that provides a shortcode, for example: [Simple Google Maps Shortcode](https://wordpress.org/plugins/simple-google-maps-short-code).

The map field is stored as an array with keys: *address*, *lat*, *lng*.

*Display the values*

~~~
[array map_field]
  Address: [field address]
  Latitude: [field lat]
  Longitude: [field lng]
[/array]
~~~

*Display the map by passing the address to a shortcode*

~~~
[pass array=map_field]
  [pw_map address='{ADDRESS}']
[/pass]
~~~

## Relationship


Use `[related]` to loop through posts in a relationship field.

~~~
[related field_name]
  [field title]
  [field thumbnail]
[/related]
~~~

### Related by users

If the related posts are users, use the `[user]` shortcode instead of `[field]`.

To get posts related to a user, use the `user_field` parameter.

~~~
[related user_field=related_posts]
~~~

## Related by taxonomy field


Use `[related taxonomy_field]` to loop through posts related by a taxonomy field.

~~~
[related taxonomy_field=field_name]
  [field title]
  [field thumbnail]
[/related]
~~~

This excludes the current post by default.

If the taxonomy field contains multiple terms, the loop will include related posts with *any* of the terms. Set *operator=and* to display related posts that have *all* terms.

### Parameters

> **count** - maximum number of results

> **orderby** - order by* id*,* author*,* title*,* name*,* date* (default),* rand* (randomized)

> **order** - ASC (ascending/alphabetical) or DESC (descending/from most recent date)


## Repeater


Use `[repeater]` to loop through each row of a repeater field.


*Display repeater fields*

~~~
[repeater field_name]
  [field title]
  [field image=image]
  [field description format=true]
[/repeater]
~~~

For an image field inside, use the *image* parameter to display the field. You can set the *size* parameter to:* thumbnail*,* medium*,* large* - default is *full*. If the image field returns as URL, set *in=url*.



### Parameters

> **count** - how many rows to loop

> **start** - which row to start; default is 1

> **row** - a specific row from the repeater field: *row=3*

> **row=rand** - a randomly selected row


### If repeater is not empty

~~~
[if field=repeater_field]
  ..Repeater field has value..
[/if]
~~~


### Display a specific row

*Display the third row*

~~~
[repeater field_name row=3]
  [field title]
[/repeater]
~~~

*Display a random row*

~~~
[repeater field_name row=rand]
~~~

*Display specific sub-fields without looping*

~~~
[repeater field_name row=1 sub=title]
[repeater field_name row=2 sub=text]
[repeater field_name row=3 sub_image=image]
~~~

This displays a sub-field from a specific row. It's used by itself without a closing tag.


### Nested repeaters

~~~
[repeater field_name]
  [-repeater inner_field_name]
    ...
  [/-repeater]
[/repeater]
~~~

To display a repeater inside a repeater, use `[-repeater]`.  Please note that the inner repeater field must have a different name than its parent.

## Flexible content

~~~
[flex flexible_content]

  [layout title_text]
    [field title]
    [field text]
  [/layout]

  [layout title_image_text]
    [field title]
    [field image=image size=thumbnail]
    [field text]
  [/layout]

  [layout gallery]
    [acf_gallery gallery_field]
      [acf_image size=thumbnail]
    [/acf_gallery]
  [/layout]

[/flex]
~~~

Multiple layouts may be specified, separated by comma. Also, *default* layout will match all.

## Gallery


For gallery fields, use `[acf_gallery]`.

*Display images with title*

~~~
[acf_gallery gallery_field]
  [field image]
  [field title]
  [field image size=thumbnail]
[/acf_gallery]
~~~

The `[field]` shortcode can display these fields: *image*, *id*, *title*, *caption*, *alt*, *url*, and *description*. When displaying the image, you can also set the *size* parameter to: *thumbnail*, *medium*, *large*. Default is full-size.

### Pass to another shortcode

You can pass the image to another shortcode's parameter.

*Each image ID or URL*

~~~
[acf_gallery gallery_field]
  [pass fields=id,url]
    [shortcode param={ID} or param={URL}]
  [/pass]
[/acf_gallery]
~~~

*All IDs from gallery - comma separated list*

~~~
[pass acf_gallery=gallery_field]
  [isotope_gallery ids='{FIELD}']
[/pass]
~~~


## In a loop


Use `[loop]` to display ACF fields from other posts.

~~~
[loop name=hello-world]
  [repeater field_name]
    [field title]
    [field image=image]
    [field description format=true]
  [/repeater]
[/loop]

~~~


## Field stored as array


If the field value is stored as an array, you can use the [`[array]` shortcode](options-general.php?page=ccs_reference&tab=field#array) to access its contents.

~~~
[array field_name]
  [field title]
  [field description]
[/array]
~~~


## Field stored as attachment ID

If the field value is an attachment ID - for example, a file upload field - you can use the [`[attached]` shortcode](options-general.php?page=ccs_reference&tab=attach) to access its contents.

~~~
[attached field=file_upload]
  [field title]
  [field description]
  <a href="[field download-url]" download>Download Link</a>
[/attached]
~~~

If the attachment is a PDF file and has preview image, field *url* shows the image URL and *download-url* will get the actual file URL.

## Option Page

To get a field from an option page, set *option=true*.

~~~
[field option_field option=true]
~~~

This parameter can be used with: `field`, `repeater`, `flex` and `acf_gallery`.

~~~
[repeater field_name option=true]
  [field inner_field]
[/repeater]
~~~

Only the parent shortcode needs the *option* parameter.

# HTML Blocks

---



The `[block]` shortcode is used as placeholders for HTML tags.

The advantage is that the tags will be visible and protected in the post editor in Visual mode.

You can enable this module under [Settings](options-general.php?page=ccs_reference&tab=settings). *Thanks to @szepeviktor for suggesting this feature.*

---

By default, it creates a `<div>`.

~~~
[block]
  ...
[/block]
~~~

You can set the *tag* parameter to create other HTML tags.

~~~
[block tag=article]
  ...
[/block]
~~~


&nbsp;

### Attributes

Any HTML attribute can be specified as parameter.

~~~
[block id=left-block class=col-md-6 style='margin-left:0']
  ...
[/block]
[block id=right-block class=col-md-6 style='margin-right:0']
  ...
[/block]
~~~


### Nested

To make nested blocks, use the minus prefix.

~~~
[block]
  [-block]
    [--block]
      ...
    [/--block]
  [/-block]
[/block]
~~~


### HTML shortcodes

Shortcodes are also provided for all major HTML tags.

~~~
[article]
  [section]
    [h1]Title[/h1]
  [/section]
  [section]
    [h1]Title[/h1]
  [/section]
[/article]
~~~

# Bootstrap

---

### Tabs

To display a menu in Bootstrap tabs or pills, use the *ul* parameter.

*Display a menu in stacked pills*

~~~
[content menu='Side Menu' ul=nav-pills-stacked]
~~~

The available values are: *nav-tabs*, *nav-pills* or *nav-pills-stacked*.

&nbsp;

### Navbar

To display a menu in a Bootstrap navbar, use the `[navbar]` shortcode.

~~~
[navbar menu='Main Menu']
  Brand
[/navbar]
~~~

The *menu* parameter is the title of the menu to be displayed. You can put text or image for the brand.

Optionally, you can set the *navclass* parameter to: *top-nav*, *navbar-fixed-top*, *navbar-fixed-bottom*, *navbar-static-top*. The default is *top-nav*. Please refer to the [Bootstrap documentation](http://getbootstrap.com/components/#navbar) for details.

# Gallery Field

---

Enable the Gallery Field module under [Settings](options-general.php?page=ccs_reference&tab=settings).

Then, select post types to use under [Admin Panel > Settings > Gallery Fields](options-general.php?page=ccs_gallery_field_settings).

---

You can add, order, edit and remove images in the field.


### Example

*Display images from the gallery field*

~~~
[attached gallery]
  Title: [field title]
  Full-size image: [field image]
  Caption: [field caption]
[/attached]
~~~

For each image, these fields can be displayed: *id*, *title*, *image*, *image-url*, *caption*, *description*,* thumbnail*, and* thumbnail-url*.

To sort by image title instead of gallery order, set parameter *orderby=title*.

&nbsp;

### In a loop

*Display gallery fields of each post in a loop*

~~~
[loop type=post category=colorful]
  Post Title: [field title]
  Description: [content]
  [attached gallery columns=4]
    [field thumbnail]
  [/attached]
[/loop]
~~~

&nbsp;

### Native or Bootstrap gallery


You can display all images in the gallery field using a native gallery or Bootstrap carousel.

*Display a native gallery or Bootstrap carousel*

~~~
[content gallery=native]
[content gallery=carousel]
~~~


You can pass the following parameters to the native gallery: *orderby*, *order*, *columns*, *size*, *link*, *include*, *exclude*. For details about the native `[gallery]` shortcode, please refer to [the codex](http://codex.wordpress.org/Gallery_Shortcode).

&nbsp;

### Individual image


The `[field]` shortcode can display individual images of the gallery field.

*Display the 3rd image in the gallery field*

~~~
[field gallery num=3]
~~~

*Get the first image's thumbnail URL*

~~~
[field gallery-url num=3 size=thumbnail]
~~~


&nbsp;

### Group

When using the `[loop]` to generate multiple Bootstrap carousels, the following will put images from each post in its own carousel.

~~~
[loop type=post fields=id]
  ...
  [content gallery=carousel group=gallery-{ID}]
[/loop]
~~~

# Math

---

Enable the Math module under [Settings](options-general.php?page=ccs_reference&tab=settings).

---

Use `[calc]` to perform safe, spreadsheet-like calculations.

~~~
Total: [calc][field price] * [field amount][/calc]
~~~

Numbers can be assigned to variables.

~~~
[calc]total = [field price] * [field amount][/calc]
~~~

When you assign a variable, it displays nothing.

These variables are shared with the `[get]` and `[set]` shortcodes described under [Advanced: Extras](options-general.php?page=ccs_reference&tab=extras#variables).

---

To use a variable:

~~~
Total: [calc]total[/calc]
Tax: [calc]total * 0.19[/calc]
~~~

The result can be formatted.

~~~
[format decimal=2][calc]total[/calc][/format]
~~~

For more information on the `[format]` shortcode, also see [Advanced: Extras](options-general.php?page=ccs_reference&tab=extras#format).

# Meta Shortcodes

---

This is a feature to create shortcodes that run other shortcodes.

It could be useful for making templates, or simplified shortcodes for use by clients.

When the module is enabled under [Settings](options-general.php?page=ccs_reference&tab=settings), a post type called `shortcode` is created.

### Name

Each post of this type becomes a shortcode whose name is the title of the post.

For example, a post called `staff` will create a `[staff]` shortcode.

When the shortcode is called, the content of the post is displayed without formatting.

### Parameters

Parameters can be passed to the shortcode as usual.

~~~
[staff position=manager]
~~~

You can use that parameter in the `shortcode` post content like this:

~~~
[loop type=staff taxonomy=position term={POSITION}]
  [field title]
[/loop]  
~~~

Shortcode parameters are passed as `{TAG}`, with an uppercased version of the parameter name.

---

To check if a parameter was passed:

~~~
[if exists]{PARAMETER}[show]The parameter was passed
[else]The parameter wasn't passed[/if]
~~~

### Content

If the shortcode is called with open/close tags:

~~~
[staff position=manager]
  [field date]
[/staff]
~~~

..the inner content is passed as `{CONTENT}`.

~~~
[loop type=staff taxonomy=position term={POSITION}]
  [field title]
  {CONTENT}
[/loop]  
~~~

### Editor

The `shortcode` post type uses a simple code editor.

In addition to code highlighting and auto-indent, there is tab auto-completion for common shortcodes and HTML tags like `[loop]` and `<div>`. For example, type `loop` and press tab to produce the both opening and closing tags.

### Global shortcodes

For efficiency, only this plugin's shortcodes are enabled inside a `shortcode` post. To use shortcodes from a theme or other plugins, surround them with `[global]..[/global]`.
# Mobile Detect

---


Enable the Mobile Detect under [Settings](options-general.php?page=ccs_reference&tab=settings).

Use `[is]` to display content based on device type.

~~~
[is mobile]
  User is on a phone or tablet.
[else]
  User is not on a mobile device.
[/is]
~~~

The parameters available are: *mobile*, *phone*, *tablet*, and *computer*.



### Examples

*Image sizes*

~~~
[is mobile]
  [field image size=medium]
[else]
  [field image size=large]
[/is]

~~~

*Stylesheets*

~~~
[is computer]
  [load css=style.css]
[else]
  [load css=style-mobile.css]
[/is]
~~~

*Redirect visitors on mobile*

~~~
[is mobile]
  [redirect][url site]/mobile/[/redirect]
[/is]
~~~

These last two would be placed in a custom field named *css*, to load in the head of the page.

&nbsp;

### Body class

There are CSS classes added to the &lt;body&gt; element, for styling purposes: *.is_phone, .isnt_phone, .is_tablet, .is_mobile,* *.is_computer* and *.isnt_computer*.

&nbsp;

### Library

Device detection is based on a lightweight PHP class, [Mobile Detect](http://mobiledetect.net) version 2.8.12.

Mobile Detect works with user-agent detection on the server side.

*It will not work if the page is cached.*

# WCK Fields and Post Types

---

## Single field

For WCK fields, use the `[field]` shortcode and specify a metabox name.


~~~
[field field-name metabox=metabox-name]
~~~

### Parameters

> **image** - display an image field: *image=field-name*

> **size** - size of image: *thumbnail*, *medium*, *large*, *full* (default) or custom defined size

> **shortcode** - set *true* to enable shortcodes inside the field.

>> Usually WCK formats a text area to create break lines, but it doesn't work well with shortcodes. When shortcodes are enabled, you'll need to insert &lt;br&gt; tags manually.


## Multiple fields

For multiple fields from the same metabox, you can use the `[metabox]` shortcode.


~~~
[metabox name=metabox-name]
  [field field-name]
  [field another-field]
[/metabox]

~~~


&nbsp;

## Repeater

Use `[repeater]` to display a repeating metabox.

*Display repeater fields*

~~~
[repeater metabox=metabox-name]
  [field field-name]
  [field another-field]
[/repeater]
~~~



### Parameters

> **metabox** - name of metabox

> **id** - post id (default is current post)

### Inside loop

*Display repeater fields from five recent posts*

~~~
[loop type=post_type count=5]
  Post Title: [field title]
  [repeater metabox=metabox-name]
    [field field-name]
    [field another-field]
  [/repeater]
[/loop]
~~~

## Conditions

Use [the `[if]` shortcode](options-general.php?page=ccs_reference&tab=if#field-value) to compare field value.

Specify the *metabox* parameter when outside a metabox or repeater loop.

~~~
[if field=field-name metabox=metabox-name value=123]
~~~
