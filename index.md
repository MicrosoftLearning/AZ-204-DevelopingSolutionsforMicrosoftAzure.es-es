---
title: Instrucciones hospedadas en línea
permalink: index.html
layout: home
ms.openlocfilehash: 8cc220d44fe56f19385b25675c29e391b610db8e
ms.sourcegitcommit: d2d374fffa4fcbf92b9c4bdc9c9ecc470152e033
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2021
ms.locfileid: "132626151"
---
## <a name="content-directory"></a>Directorio de contenido

A continuación, se enumeran hipervínculos a cada uno de los laboratorios.

## <a name="labs"></a>Laboratorios

{% assign labs = site.pages | where_exp: "page", "page.url contains '/Instructions/Labs'" %}
| Módulo | Laboratorio |
| --- | --- |
{% for activity in labs  %}{% if activity.lab.az204Module %}| {{ activity.lab.az204Module }} | [{{ activity.lab.az204Title }}]({{ site.github.url }}{{ activity.url }}) |
{% endif %}{% endfor %}
