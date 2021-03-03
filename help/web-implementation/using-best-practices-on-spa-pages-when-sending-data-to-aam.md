---
title: Prácticas recomendadas en páginas de SPA al enviar datos a AAM
description: En este documento, describiremos varias prácticas recomendadas que debe seguir y tener en cuenta a la hora de enviar datos desde aplicaciones de una sola página (SPA) a Adobe Audience Manager (AAM). Este documento se centrará en el uso de Launch por Adobe, que es el método de implementación recomendado.
feature: Conceptos básicos de implementación
topics: spa
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1390
topic: SPA
role: '"Desarrollador, ingeniero de datos"'
level: Con experiencia
translation-type: tm+mt
source-git-commit: a7dc335e75697a7b1720eccdadbb9605fdeda798
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 0%

---


# Prácticas recomendadas en páginas de SPA al enviar datos a AAM {#using-best-practices-on-spa-pages-when-sending-data-to-aam}

En este documento, describiremos varias prácticas recomendadas que debe seguir y tener en cuenta a la hora de enviar datos de [!UICONTROL Single Page Applications] (SPA) a Adobe Audience Manager (AAM). Este documento se centra en el uso de [!UICONTROL Experience Platform Launch], que es el método de implementación recomendado.

## Notas iniciales

* Los elementos siguientes asumirán que está utilizando [!DNL Platform Launch] para implementar en el sitio. Las consideraciones seguirían existiendo si no utiliza [!DNL Platform Launch], pero tendría que adaptarlas a su método de implementación.
* Todas las SPA son diferentes, por lo que es posible que tenga que modificar algunos de los siguientes elementos para adaptarlos a sus necesidades, pero quisimos compartir algunas prácticas recomendadas con usted; cosas que debe tener en cuenta a la hora de enviar datos desde páginas de SPA a Audience Manager.

## Diagrama sencillo del trabajo con SPA y AAM en Experience Platform Launch {#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}

![spa para aam en  [!DNL launch]](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>Como se ha indicado, este es un diagrama simplificado de cómo se gestionan las páginas de SPA en una implementación de Adobe Audience Manager (sin Adobe Analytics) mediante [!DNL Platform Launch]. Como puede ver, es bastante directo, con la gran decisión de cómo va a comunicar un cambio de vista (o una acción) a [!DNL Platform Launch].

## Activación [!DNL Launch] desde la página de SPA {#triggering-launch-from-the-spa-page}

Dos de los métodos más comunes para activar una regla en [!DNL Platform Launch] (y, por lo tanto, enviar datos a Audience Manager) son:

* Configuración de eventos personalizados de JavaScript (consulte el ejemplo [HERE](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html) con Adobe Analytics)
* Uso de [!UICONTROL Direct Call Rule]

En este ejemplo de Audience Manager, utilizamos un [!UICONTROL Direct Call rule] en [!DNL Launch] para activar la visita que entra en Audience Manager. Como verá en las secciones siguientes, esto resulta realmente útil al configurar el [!UICONTROL Data Layer] en un nuevo valor, de modo que el [!UICONTROL Data Element] pueda utilizarlo en [!DNL Platform Launch].

## Demostración de página {#demo-page}

Hemos creado una pequeña página de demostración que muestra cómo cambiar un valor en [!DNL data layer] y enviarlo a AAM, como puede hacer en una página de SPA. Esta funcionalidad se puede modelar para realizar cambios más detallados y necesarios. Puede encontrar esta página de demostración [HERE](https://aam.enablementadobe.com/SPA-Launch.html).

## Al configurar la variable [!DNL data layer] {#setting-the-data-layer}

Como se ha mencionado, cuando se carga contenido nuevo en la página o cuando alguien realiza una acción en el sitio, el [!DNL data layer] debe configurarse de forma dinámica en el encabezado de la página ANTES de que se llame a [!DNL Launch] y ejecute el [!UICONTROL rules], de modo que [!DNL Platform Launch] pueda recoger los nuevos valores del [!DNL data layer] y colocarlos en Audience Manager.

Si va al sitio de demostración enumerado arriba y mira el origen de la página, verá:

* El [!DNL data layer] está en el encabezado de la página, antes de la llamada a [!DNL Platform Launch]
* El JavaScript en el vínculo SPA simulado cambia el [!UICONTROL Data Layer] y LUEGO llama a [!DNL Platform Launch] (la llamada _satellite.track() ). Si estaba utilizando eventos personalizados de JavaScript en lugar de [!UICONTROL Direct Call Rule], la lección es la misma. Primero cambie [!DNL data layer] y luego llame a [!DNL Launch].

>[!VIDEO](https://video.tv.adobe.com/v/23322/?quality=12)

## Recursos adicionales {#additional-resources}

* [Debate sobre la SPA en los foros de Adobe](https://forums.adobe.com/thread/2451022)
* [Sitios de arquitectura de referencia para mostrar cómo implementar SPA en Launch por Adobe](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [Prácticas recomendadas al rastrear SPA en Adobe Analytics](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [Sitio de muestra utilizado para este artículo](https://aam.enablementadobe.com/SPA-Launch.html)
