# Posts

### [Installing RDKit on Windows 10 (and bonus track :))](./rdkit_install_post.md)
Published: January 15, 2020

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>. Published: {{ post.date | date_to_string: "ordinal", "US" }}
    </li>
  {% endfor %}
</ul>
