===========
Jekyll Tips
===========

.. role:: md(code)
  :language: md

Front matter
------------

Post template:

.. code:: md
  
  ---
  layout: post
  title: "Post Template"
  excerpt_separator: "<!--more-->"
  tags:
    - post
  categories:
    - templates
  ---
  
Page template:

.. code:: md

  ---
  layout: page
  title: "Page Template"
  sidebar_link: true
  ---
  
Defaults are set in ``_config.yml``:

.. code:: yml

  defaults:
    -
      scope:
        path: ""
        type: "posts"
      values:
        layout: "default"
        comments: false
        permalink: "url"
        published: true

Images
------

In ``_includes/images.html``, add the following:

.. code:: html

  <div class="image-wrapper" >
    {% if include.url %}
    <a href="{{ include.url }}" title="{{ include.title }}" target="_blank">
    {% endif %}
    
      <img src="{{ site.url }}/{{ include.img }}" alt="{{ include.title }}"/>
    
    {% if include.url %}
    </a>
    {% endif %}
  
    {% if include.caption %}
      <p class="image-caption">{{ include.caption }}</p>
    {% endif %}
  </div>
  
In ``_sass/images.scss`` or however styling is overridden in your theme, add the following:

.. code:: scss
 
  @import "hydeout";

  .image-wrapper {
    text-align: center;

    .image-caption {
      color: $your-grey;
      margin-top: 0;
      font-size: 85%;
    }
  }
  
Where ``$your-grey`` is a predefined colour variable. Finally, add an image using the following:

.. code:: md

  {% include image.html
             img="images/image.png"
             title="title"
             caption="caption"
             url="https://example.com" %}

*Taken from <https://superdevresources.com/image-caption-jekyll/>*.

Hyperlinks
----------

Linking to another post:

.. code:: md

  [name]({{ site.baseurl }}{% post_url yyyy-mm-dd-post-file-name %})
