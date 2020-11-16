---
title: Prácticas recomendadas en páginas SPA al enviar datos a AAM
description: En este documento, describiremos varias prácticas recomendadas que debe seguir y tener en cuenta a medida que envía datos desde Aplicaciones de una sola página (SPA) a Adobe Audience Manager (AAM). Este documento se centrará en el uso de Launch by Adobe, que es el método de implementación recomendado.
feature: implementation basics
topics: spa
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1390
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '576'
ht-degree: 0%

---


# Prácticas recomendadas en páginas SPA al enviar datos a AAM {#using-best-practices-on-spa-pages-when-sending-data-to-aam}

En este documento, describiremos varias prácticas recomendadas que debe seguir y tener en cuenta a medida que envía datos de [!UICONTROL Single Page Applications] (SPA) a Adobe Audience Manager (AAM). Este documento se centra en el uso [!UICONTROL Experience Platform Launch], que es el método de implementación recomendado.

## Notas iniciales

* Los siguientes elementos supondrán que se utiliza [!DNL Platform Launch] para implementar en el sitio. Las consideraciones seguirían existiendo si no se utiliza [!DNL Platform Launch], pero tendría que adaptarlas al método de implementación.
* Todos los SPA son diferentes, por lo que es posible que tenga que modificar algunos de los siguientes elementos para adaptarlos mejor a sus necesidades, pero queremos compartir con usted algunas prácticas recomendadas; cosas que debe tener en cuenta a medida que envía datos de SPA páginas al Audience Manager.

## Diagrama sencillo de trabajo con SPA y AAM en Experience Platform Launch {#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}

![spa para jamón en [!DNL launch]](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>Como se ha indicado, este es un diagrama simplificado de cómo se gestionan SPA páginas en una implementación de Adobe Audience Manager (sin Adobe Analytics) mediante [!DNL Platform Launch]. Como pueden ver, es bastante directo, con la gran decisión de comunicar un cambio de vista (o una acción) a [!DNL Platform Launch].

## Activación [!DNL Launch] desde la página SPA {#triggering-launch-from-the-spa-page}

Dos de los métodos más comunes para activar una regla en [!DNL Platform Launch] (y por lo tanto enviar datos al Audience Manager) son:

* Configuración de eventos personalizados de JavaScript (consulte el ejemplo [AQUÍ](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html) con Adobe Analytics)
* Uso de un [!UICONTROL Direct Call Rule]

En este ejemplo de Audience Manager, vamos a usar un [!UICONTROL Direct Call rule] in [!DNL Launch] para activar la visita que se va a entrar en Audience Manager. Como verá en las siguientes secciones, esto resulta muy útil al configurar el [!UICONTROL Data Layer] en un nuevo valor, de modo que el [!UICONTROL Data Element] en [!DNL Platform Launch].

## Página de demostración {#demo-page}

Hemos creado una pequeña página de demostración que muestra cómo cambiar un valor en el [!DNL data layer] y enviarlo a AAM, como puede hacer en una página SPA. Esta funcionalidad se puede modelar para realizar cambios más detallados que sean necesarios. Puede encontrar esta página de demostración [AQUÍ](https://aam.enablementadobe.com/SPA-Launch.html).

## Al configurar la variable [!DNL data layer] {#setting-the-data-layer}

Como se mencionó anteriormente, cuando se carga contenido nuevo en la página o cuando alguien realiza una acción en el sitio, el [!DNL data layer] contenido debe configurarse dinámicamente en el encabezado de la página ANTES de que [!DNL Launch] se llame y ejecute el [!UICONTROL rules], de modo que [!DNL Platform Launch] pueda recoger los nuevos valores del sitio [!DNL data layer] e insertarlos en el Audience Manager.

Si va al sitio de demostración enumerado arriba y mira el origen de la página, verá:

* El [!DNL data layer] está en el encabezado de la página, antes de la llamada a [!DNL Platform Launch]
* El JavaScript del vínculo de SPA simulado cambia el [!UICONTROL Data Layer]y LUEGO llama [!DNL Platform Launch] (la llamada _satellite.track()). Si estaba utilizando eventos personalizados de JavaScript en lugar de esto [!UICONTROL Direct Call Rule], la lección es la misma. Primero cambie el [!DNL data layer]y luego llame a [!DNL Launch].

>[!VIDEO](https://video.tv.adobe.com/v/23322/?quality=12)

## Recursos adicionales {#additional-resources}

* [SPA debate en los foros de Adobe](https://forums.adobe.com/thread/2451022)
* [Sitios de Arquitectura de referencia para mostrar cómo implementar SPA en Launch by Adobe](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [Uso de las prácticas recomendadas al rastrear SPA en Adobe Analytics](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [Sitio de demostración utilizado para este artículo](https://aam.enablementadobe.com/SPA-Launch.html)
