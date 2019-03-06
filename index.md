---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
---
<h2>{{ site.data.navigation.docs_list_title }}</h2>
Ben Sims Esquire, Software Engineer, Part III CASM, welcomes you to this homepage 
<ul>
   {% for item in site.data.navigation.docs %}
      <li><a href="{{ item.url }}">{{ item.title }}</a></li>
   {% endfor %}
</ul>
