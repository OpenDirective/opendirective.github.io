---
layout: post
title: JSON Data File Collections in Jekyll
---

<div class="message">While Jekyll and liquid provide very limited support for collections, it's surprisingly easy to mange a set of JSON files so they can be readily and efficiently accessed on the client.
</div>

## Requirements

We have three similar projects under development at the W3C WAI. Each provides information on a set of items (course, authoring tools and evaluations tools) and users of our Jekyll SSG generated web 'app' wil be able to sort and filter the items.

A form is provided to submit new items that will undergo a review process before eventually being added to the website dataset for future access in the app.

So that's only two pages, assuming no pagination.

## Architecture

The need for client side manipulation of the item list view (filter and sort) suggests the JSON data be made available to the client.

It would be possible to keep all state in the DOM and manipulate that directly. It would also be possible to use the now common pattern of always rendering a view of the data in the client(made popular by React).

However, as this is a low complexity app other architectures might be suitable, for example using matching data and HTMLin the client.

As the content will ony rarely change and Jekyll is a SSG it makes sense to pre-generate as much as possible, both data and HTML. Then custom client side behaviour (ie javascript code) can be minimised.

A final observation is that as we are using a SSG, data and HTML can be pre render and directly included in pages, rather than fetched a runtime in the client.

The need for scalability and pagination might impact these decisions, but initially data sets are small. But this post highlight an option that might be non obvious in terms of approach and implementation in Jekyll.

## Outline

The approach is as follows:

- each item has it's own json data file in a folder with the others
- the JSON files have a top level object (hash) s owe can access various fields
- generate a single JSON structure of the items as an array
- Jekyll is used to general a single JSON structure of items
- html for the items can be created in as similar way
- the data and initial HTML are directly embedded into the HTML files
- we can use Jekyll `includes` to modularise the code

At build time we would like to:

- access the files as a collection
- sort the collection on a specific item field - eg name
- generate the JSON array from the files
- iterate over the collection, to generate HTML for example

## Creating the JSON structure

The Jekyll documentation on using [data files](https://jekyllrb.com/docs/datafiles/) is a little lacking and some experimentation is needed. But it turns out to be really easy to generate the JSON from all the files.

### Tools

Liquid has the concept of `arrays`, but provides a very restricted set of `filters` (functions) that create and operate on them. It also has the `[n]` access operator, plus `first` and `last` methods. Jekyll adds a few more useful filters.

Jekyll also adds `hashes` (objects) and filters that work on them. But hashes are created for you in standard variables and there is no way to create or manipulate them.

But we can do what we need to with these limited programming constructs.

### Attempt

Given all our JSON files live in the `_data` directory, an initial attempt might be as trivial as using the variable `site.data` to access them as a collection. Then we can try things like

```liquid
{{ site.data | sort: 'name' }}
or
{{ site.data.items | jsonify }}
and
{% for item in side.data %} ... {{ item.name }} ... {% endfor %}

```

But alas results are unexpected. With a little use of the `inspect` filter it turns out that:

- site.data is a hash keyed by data filename and with values that are file contents are being parsed as json
- the `jsonify` filter also shows this structure
- you can iterate over the keys and otherwise treat as an array

### Solution

The solution is straight forward. As we don't care about the filenames we can convert the hash values to an array. Then all the operation work as required.

```liquid
# create an empty array
{%- assign itemValues = "" | split: "," -%}

# add the hash values to the array
{%- for item in site.data -%}
    {% assign itemValues = item[1] -%}
    # note push is non mutating
    {% assign itemValues = itemValues | push: itemValue -%}
{%- endfor -%}```

Note there's no "return" form includes so the output is any variables we set. itemValues in this case.

Now you can do the things we tried before:

```liquid
{{ itemValues | sort: "name" }}
{{ itemValues | jsonify }}
{% for item in itemValues %} ... {{ item.name }} ... {% endfor %}
```

As a final optimisation you can put the code in an `include`, passing the data (you can use subfolders of `_data` organise) and a sort key.

```liquid
{% include sort-data-folder.liquid data=site.data sortKey="name" %}
```
