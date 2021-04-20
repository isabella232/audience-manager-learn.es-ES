---
title: Actualización a Adobe Audience Manager DIL versión 8.0 (o superior)
description: Este artículo le proporcionará pasos y recomendaciones para actualizar el código de la biblioteca de integración de datos (DIL) de Adobe Audience Manager (AAM) a la versión 8.0 o posterior. Esto se refiere a la implementación DIL "del lado del cliente", no al reenvío de datos de Adobe Analytics del lado del servidor, y abarca DTM, Launch de Adobe e implementaciones sin una solución de administrador de etiquetas de Adobe.
feature: DIL Implementation
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
role: "Developer, Data Engineer"
level: Intermediate
translation-type: tm+mt
source-git-commit: a7dc335e75697a7b1720eccdadbb9605fdeda798
workflow-type: tm+mt
source-wordcount: '1155'
ht-degree: 1%

---


# Actualización a la versión 8.0 (o superior) de DIL de Adobe Audience Manager {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

Este artículo le proporcionará pasos y recomendaciones para actualizar el código [!DNL Data Integration Library] (DIL) de Adobe Audience Manager (AAM) a la versión 8.0 o posterior. Esto se refiere a la implementación DIL &quot;del lado del cliente&quot;, no al reenvío de datos de Adobe Analytics del lado del servidor, y abarca DTM, Launch de Adobe e implementaciones sin una solución de administrador de etiquetas de Adobe.

## Información general {#overview}

El código [!DNL Data Integration Library] (DIL) de Audience Manager le permite implementar AAM en su sitio web*. Al implementar versiones anteriores de DIL, no era necesario tener el servicio de Experience Cloud ID (ECID) de Adobe implementado también (aunque era una buena idea). A partir de la versión 8.0 de DIL, existe una fuerte dependencia de la versión 3.3 o posterior de ECID. Si implementa DIL 8.0 o posterior sin ECID 3.3, o con una versión anterior, obtendrá un error y no funcionará. Dado que existen varias formas de implementar AAM, hemos creado esta página para darle algunos pasos a seguir, así como algunas recomendaciones. A continuación, encontrará estos pasos y recomendaciones desglosados por plataforma/método de implementación. Encontrará más información sobre DIL en la [documentación](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html).

* Como se indica en la descripción de esta página, esto solo cubre las implementaciones DIL del lado del cliente, utilizadas por clientes de AAM que no tienen Adobe Analytics. Si tiene Adobe Analytics, debe utilizar el método de reenvío del lado del servidor para implementar AAM. Este método se describe en la [documentación](https://marketing.adobe.com/resources/help/en_US/reference/ssf.html).

## Duplicar y desaprobar elementos y métodos {#duplicate-and-deprecated-elements-and-methods}

En versiones anteriores de DIL y ECID, había métodos duplicados (métodos que hacen la misma función tanto en DIL como en ECID), que causaban confusión sobre cuál utilizar. Normalmente, necesitaba usar ambos y hacerlos coincidir, y ese mensaje no se comunicó bien a nuestros clientes. A partir de DIL 8.0, estos métodos y elementos duplicados quedan obsoletos en DIL y se recomienda utilizar la versión de ECID.

Por ejemplo:

* Al utilizar [!DNL DIL.create], se han desaprobado algunos elementos y en su lugar debe utilizar los elementos ECID. Estos elementos se describen en la [[!DNL DIL.create] documentación](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_create.html).
* El método de nivel de instancia [!DNL idSync] también se ha desaprobado, como se indica en la [documentación](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_idsync.html) del método.

## Sincronización de ID con un ID de cliente {#id-syncing-with-a-customer-id}

En AAM, puede sincronizar su UUID (ID de usuario único anónimo) en el equipo con un ID de cliente, de modo que pueda cargar datos sin conexión sobre ese cliente y vincularlos con su comportamiento en línea, para comprender mejor a sus clientes. En el pasado, esto se ha hecho de una de dos maneras:

* El método de nivel de instancia [!DNL idSync]
* El elemento [!DNL declaredId] en [!DNL DIL.create]

Si ha utilizado cualquiera de estos métodos antiguos para sincronizar con un ID de cliente, es muy recomendable actualizar a mediante el método [!DNL setCustomerIDs], que forma parte del servicio ECID. Encontrará más información sobre [!DNL setCustomerIDs] en la [documentación](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid_setcustomerids.html) del método.

**Sugerencia rápida:** Cuando anteriormente se utilizaban cualquiera de los métodos anteriores, se hacía referencia a AAM  [!UICONTROL Data Source] con el  [!UICONTROL Data Source] ID (también conocido como &quot;DPID&quot;). Al actualizar a [!DNL setCustomerIDs], tendrá que utilizar en su lugar el &quot;[!UICONTROL Integration Code]&quot; de AAM [!UICONTROL Data Source]. Sigue apuntando al mismo [!UICONTROL Data Source] pero es solo un identificador diferente. Esto se muestra en el siguiente vídeo.

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

En las secciones siguientes se enumeran los pasos y recomendaciones para actualizar a DIL 8.0 según el método de implementación:

## Actualización a DIL 8.0 en Adobe Experience Platform Launch {#updating-to-dil-in-experience-platform-launch}

Pasos básicos para actualizar a DIL 8.0

1. Si utiliza DIL anterior a la versión 8.0, antes de actualizar, vaya a la configuración DIL en la extensión AAM y tome nota de las opciones avanzadas que esté utilizando (para utilizarlas en un paso posterior).
1. Actualizar la extensión de AAM a la versión 8.0 o superior
1. Compruebe que la extensión del servicio Experience Cloud ID sea 3.3.0 o superior
1. Para cualquier método o elemento obsoleto (como &quot;[!DNL disableIDSyncs]&quot;) que estuviera en la extensión de AAM anterior a la 8.0 o en código personalizado para DIL, habilite los métodos ECID en la extensión ECID.

   1. (DIL) disableDestinationPublishingIframe -> (ECID) disableIdSyncs
   1. (DIL) disableIDSyncs -> (ECID) disableIdSyncs
   1. (DIL) iframeAkamaiHTTPS -> (ECID) dSyncSSLUseAkamai
   1. (DIL) declarationId -> (ECID) setCustomerIDs

1. Publicar los cambios

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## Actualización a DIL 8.0 en Adobe DTM {#updating-to-dil-in-adobe-dtm}

1. Actualice la herramienta AAM a la versión 8.0 o superior. Esta configuración de versión se encuentra en la sección &quot;General&quot; de la herramienta AAM.
1. Para cualquier método o elemento obsoleto (como &quot;disableIDSyncs&quot;) que se encuentre en el código personalizado de DIL de la herramienta AAM anterior a la versión 8.0, tenga en cuenta estos métodos (de modo que pueda agregarlos a la herramienta ECID) y, a continuación, elimínelos del [!DNL DIL code] personalizado en la herramienta AAM.
1. Actualizar la extensión del servicio Experience Cloud ID a la versión 3.3.0 o superior
1. Añada las opciones avanzadas a la herramienta ECID que ha eliminado del código personalizado de la herramienta AAM.
1. Publicar los cambios

## Actualización a DIL 8.0 sin solución de administración de etiquetas de Adobe {#additional-resources}

Si está actualizando el código directamente en la página, puede reemplazar los elementos antiguos por elementos más nuevos, excepto cuando necesite mover métodos/elementos de DIL a ECID, como se ha explicado anteriormente. En ese caso, simplemente reemplazará el método/elemento antiguo de la ubicación DIL por el método/elemento ECID en la ubicación ECID.

Lo mismo sucede con los administradores de etiquetas que no son de Adobe. Siempre que tenga las versiones anteriores en esa solución de administración de etiquetas, sustitúyalas por el nuevo código tal como se describe en los pasos siguientes.

1. Actualice la biblioteca DIL a la versión más reciente (8.0 o superior): Deberá obtener el código DIL más reciente de la Consultoría de Adobe o del Servicio de atención al cliente de Adobe, ya que actualmente no está disponible en una ubicación pública. A continuación, reemplace el antiguo código de biblioteca DIL por el nuevo código de biblioteca DIL y continúe con el siguiente paso (no se detenga ahora o tendrá problemas, ja).
1. Instale [!DNL ECID Service] o actualice la versión existente a 3.3.0 o superior. Puede descargar la última versión del servicio de Experience Cloud ID [desde nuestra página de GitHub](https://github.com/Adobe-Marketing-Cloud/id-service/releases). Si necesita ayuda con esto, consulte la [documentación](https://marketing.adobe.com/resources/help/es_ES/mcvid/) o hable con un consultor de Adobe.

1. Compruebe que cualquier método o elemento obsoleto que se encuentre en su código personalizado para DIL se mueva a los métodos ECID:

   1. (DIL) disableDestinationPublishingIframe -> (ECID) disableIdSyncs

      [Documentación](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid-disableidsync.html)

   1. (DIL) disableIDSyncs -> (ECID) disableIdSyncs

      [Documentación](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid-disableidsync.html)

   1. (DIL) iframeAkamaiHTTPS -> (ECID) idSyncSSLUseAkamai

      [Documentación](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_create.html)

   1. (DIL) declarationId -> (ECID) setCustomerIDs

      [Documentación](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid_setcustomerids.html)