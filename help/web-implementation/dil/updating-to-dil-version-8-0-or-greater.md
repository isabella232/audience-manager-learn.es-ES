---
title: Actualización a la versión 8.0 (o buena) del DIL de Adobe Audience Manager
description: Este artículo le proporcionará pasos y recomendaciones para actualizar el código de Data Integration Library (DIL) de Adobe Audience Manager (AAM) a la versión 8.0 o posterior. Esto se refiere a la implementación de DIL "del lado del cliente", no al reenvío de datos de Adobe Analytics por parte del servidor, y abarcará DTM, Launch by Adobe e implementaciones sin ninguna solución de administración de etiquetas de Adobe.
feature: dil implementation
topics: null
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
translation-type: tm+mt
source-git-commit: dfd549508cc223714bdb07ac6fd2aa31e6ca5586
workflow-type: tm+mt
source-wordcount: '1149'
ht-degree: 1%

---


# Actualización a la versión 8.0 (o buena) del DIL de Adobe Audience Manager {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

Este artículo le proporcionará pasos y recomendaciones para actualizar el código de Adobe Audience Manager (AAM) [!DNL Data Integration Library] (DIL) a la versión 8.0 o posterior. Esto se refiere a la implementación de DIL &quot;del lado del cliente&quot;, no al reenvío de datos de Adobe Analytics por parte del servidor, y abarcará DTM, Launch by Adobe e implementaciones sin ninguna solución de administración de etiquetas de Adobe.

## Información general {#overview}

El código [!DNL Data Integration Library] (DIL) del Audience Manager le permite implementar AAM en su sitio Web*. Al implementar versiones anteriores de DIL, no era necesario tener implementado también el servicio de ID de Experience Cloud (ECID) de Adobe (aunque era una muy buena idea). A partir de la versión 8.0 del DIL, existe una dependencia estricta de la versión 3.3 o posterior del ECID. Si implementa DIL 8.0 o posterior sin ECID 3.3, o con una versión anterior, obtendrá un error y no funcionará. Dado que existen varias formas de implementar AAM, hemos creado esta página para darle algunos pasos a seguir, así como algunas recomendaciones. A continuación encontrará estos pasos y recomendaciones desglosados por plataforma o método de implementación. Encontrará más información sobre DIL en la [documentación](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html).

* Como se indica en la descripción de esta página, esto solo cubrirá las implementaciones de DIL &quot;del lado del cliente&quot;, utilizadas por AAM clientes que no tienen Adobe Analytics. Si tiene Adobe Analytics, debe utilizar el método de reenvío del lado del servidor para implementar AAM. Este método se describe en la [documentación](https://marketing.adobe.com/resources/help/en_US/reference/ssf.html).

## Métodos y elementos obsoletos y duplicado {#duplicate-and-deprecated-elements-and-methods}

En versiones anteriores de DIL y ECID, había métodos duplicados (métodos que hacen la misma función tanto en el DIL como en el ECID), lo que causaba confusión respecto a cuál utilizar. Normalmente, necesitabas utilizarlos y hacerlos coincidir, y ese mensaje no se comunicaba bien a nuestros clientes. A partir de DIL 8.0, estos métodos y elementos de duplicado han quedado obsoletos en DIL y se recomienda utilizar la versión ECID.

Por ejemplo:

* Al utilizar [!DNL DIL.create], algunos elementos han quedado obsoletos y en su lugar debe utilizar los elementos ECID. Estos elementos se mencionan en la [[!DNL DIL.create] documentación](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_create.html).
* El método de nivel de [!DNL idSync] instancia también ha quedado obsoleto, como se indica en la [documentación](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_idsync.html)del método.

## Sincronización de ID con un ID de cliente {#id-syncing-with-a-customer-id}

En AAM, puede sincronizar su UUID (ID de usuario único anónimo) en el equipo con un ID de cliente, de modo que pueda cargar datos sin conexión sobre ese cliente y vincularlos con su comportamiento en línea, para comprender mejor a sus clientes. En el pasado, esto se ha hecho de dos maneras:

* El método [!DNL idSync] de nivel de instancia
* El [!DNL declaredId] elemento de [!DNL DIL.create]

Si ha estado utilizando cualquiera de estos métodos anteriores para sincronizar con un ID de cliente, se recomienda encarecidamente actualizar a mediante el [!DNL setCustomerIDs] método , que forma parte del servicio ECID. Encontrará más información sobre [!DNL setCustomerIDs] este tema en la [documentación](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid_setcustomerids.html)del método.

**Sugerencia rápida:** Cuando anteriormente se utilizaban cualquiera de los métodos anteriores, se hacía referencia al AAM [!UICONTROL Data Source] con el [!UICONTROL Data Source] ID (también denominado &quot;DPID&quot;). Al actualizar a [!DNL setCustomerIDs], deberá utilizar el &quot; [!UICONTROL Data Source]&quot;[!UICONTROL Integration Code]del AAM. Sigue apuntando a lo mismo [!UICONTROL Data Source] pero es simplemente un identificador diferente. Esto se muestra en el siguiente video.

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

Las secciones siguientes lista los pasos y recomendaciones para la actualización a DIL 8.0 según el método de implementación:

## Actualización a DIL 8.0 en Adobe Experience Platform Launch {#updating-to-dil-in-experience-platform-launch}

Pasos básicos para la actualización a DIL 8.0

1. Si está utilizando el DIL anterior a la versión 8.0, antes de realizar la actualización, vaya a la configuración del DIL en la extensión AAM y tome nota de las opciones avanzadas que esté utilizando (para utilizarlas en un paso posterior)
1. Actualice la extensión de AAM a la versión 8.0 o buena
1. Verifique que la extensión del servicio de ID de Experience Cloud sea la versión 3.3.0 o buena
1. Para cualquier método o elemento obsoleto (como &quot;[!DNL disableIDSyncs]&quot;) que se encuentre en la extensión de AAM anterior a 8.0 o en el código personalizado para DIL, habilite los métodos ECID en la extensión ECID.

   1. (DIL) disableDestinationPublishingIframe -> (ECID) disableIdSyncs
   1. (DIL) disableIDSyncs -> (ECID) disableIdSyncs
   1. (DIL) iframeAkamaiHTTPS -> (ECID) dSyncSSLUseAkamai
   1. (DIL) declareId -> (ECID) setCustomerIDs

1. Publicar los cambios

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## Actualización a DIL 8.0 en la DTM de Adobe {#updating-to-dil-in-adobe-dtm}

1. Actualice la herramienta de AAM a la versión 8.0 o buena. Esta configuración de versión se encuentra en la sección &quot;General&quot; de la herramienta AAM.
1. Para cualquier método o elemento obsoleto (como &quot;disableIDSyncs&quot;) que se encuentre en el código personalizado de la herramienta de AAM anterior a la versión 8.0 para el DIL, tenga en cuenta los mismos (para que pueda agregarlos a la herramienta ECID) y, a continuación, elimínelos de los elementos personalizados [!DNL DIL code] en la herramienta de AAM.
1. Actualice la extensión del servicio de ID de Experience Cloud a la versión 3.3.0 o buena
1. Añada las opciones avanzadas a la herramienta ECID que ha eliminado del código personalizado de la herramienta AAM.
1. Publicar los cambios

## Actualización a DIL 8.0 sin solución de administración de etiquetas Adobe {#additional-resources}

Si está actualizando el código directamente en la página, puede reemplazar elementos antiguos por elementos más nuevos, excepto cuando necesite mover métodos/elementos de DIL a ECID, como se analizó anteriormente. En ese caso, simplemente reemplazará el método/elemento antiguo en la ubicación del DIL por el método/elemento ECID en la ubicación ECID.

Lo mismo sucede con los administradores de etiquetas que no son de Adobe. Siempre que tenga las versiones anteriores en esa solución de administración de etiquetas, reemplácela por el nuevo código como se describe en los pasos siguientes.

1. Actualice la biblioteca de DIL a la versión más reciente (8.0 o buena): deberá obtener el código de DIL más reciente de Adobe Consulting o de Adobe Customer Care, ya que actualmente no está disponible en una ubicación pública. A continuación, simplemente reemplace el antiguo código de biblioteca del DIL por el nuevo código de biblioteca del DIL y continúe con el paso siguiente (no se detenga ahora o se encuentre con problemas, ja).
1. Instale [!DNL ECID Service] o actualice la versión existente a 3.3.0 o buena. Puede descargar la última versión [del servicio de ID de Experience Cloud desde nuestra página](https://github.com/Adobe-Marketing-Cloud/id-service/releases)de GitHub. Si necesita ayuda con esto, consulte la [documentación](https://marketing.adobe.com/resources/help/es_ES/mcvid/) o póngase en contacto con un consultor de Adobe.

1. Compruebe que todos los métodos o elementos obsoletos del código personalizado para DIL se muevan a los métodos ECID:

   1. (DIL) disableDestinationPublishingIframe -> (ECID) disableIdSyncs

      [Documentación](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid-disableidsync.html)

   1. (DIL) disableIDSyncs -> (ECID) disableIdSyncs

      [Documentación](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid-disableidsync.html)

   1. (DIL) iframeAkamaiHTTPS -> (ECID) idSyncSSLUseAkamai

      [Documentación](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_create.html)

   1. (DIL) declareId -> (ECID) setCustomerIDs

      [Documentación](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid_setcustomerids.html)