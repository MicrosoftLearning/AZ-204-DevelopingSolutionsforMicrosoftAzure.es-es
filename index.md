---
title: Instrucciones hospedadas en línea
permalink: index.html
layout: home
---

## Directorio de contenido

A continuación, se enumeran hipervínculos a cada uno de los laboratorios.

## Laboratorios

{% assign labs = site.pages | where_exp: "page", "page.url contains '/Instructions/Labs'" %}
| Módulo | Laboratorio |
| --- | --- |
{% for activity in labs  %}{% if activity.lab.az204Module %}| {{ activity.lab.az204Module }} | [{{ activity.lab.az204Title }}]({{ site.github.url }}{{ activity.url }}) |
{% endif %}{% endfor %}

