---
title: Actualización a Adobe Audience Manager DIL versión 8.0 (o buena)
description: Este artículo le proporcionará pasos y recomendaciones para actualizar el código de Data Integration Library (DIL) de Adobe Audience Manager (AAM) a la versión 8.0 o posterior. Esto se refiere a la implementación de DIL del "lado del cliente", no al reenvío de datos de Adobe Analytics del lado del servidor, y abarca DTM, Launch by Adobe e implementaciones sin solución de administración de etiquetas de Adobe.
feature: DIL Implementation
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
role: Developer, Data Engineer
level: Intermediate
exl-id: 8c1e6ed5-0f21-427b-a681-0ecb020a0e60
source-git-commit: 62b43b5627dabf754cf821f974a56c60989ef7ef
workflow-type: tm+mt
source-wordcount: '1122'
ht-degree: 1%

---

# Actualización a la versión 8.0 (o buena) del DIL de Adobe Audience Manager {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

Este artículo le proporcionará pasos y recomendaciones para actualizar Adobe Audience Manager (AAM) [!DNL Data Integration Library] (DIL) a la versión 8.0 o posterior. Esto se refiere a la implementación de DIL del &quot;lado del cliente&quot;, no al reenvío de datos de Adobe Analytics del lado del servidor, y abarca DTM, Launch by Adobe e implementaciones sin solución de administración de etiquetas de Adobe.

## Información general {#overview}

del Audience Manager [!DNL Data Integration Library] (DIL) permite implementar AAM en el sitio web*. Al implementar versiones anteriores de DIL, no era necesario tener implementado también el Servicio de ID de Experience Cloud (ECID) de Adobe (aunque era una buena idea). A partir de la versión 8.0 de DIL, existe una fuerte dependencia de la versión 3.3 o posterior de ECID. Si implementa DIL 8.0 o posterior sin ECID 3.3, o con una versión anterior, obtendrá un error y no funcionará. Dado que hay varias formas de implementar AAM, hemos creado esta página para darle algunos pasos a seguir, así como algunas recomendaciones. A continuación, encontrará estos pasos y recomendaciones desglosados por plataforma/método de implementación. Encontrará más información sobre el DIL en la [documentación](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=en).

* Como se indica en la descripción de esta página, esto solo cubre las implementaciones de DIL del lado del cliente, que utilizan los clientes AAM que no tienen Adobe Analytics. Si tiene Adobe Analytics, debe utilizar el método de reenvío del lado del servidor para implementar AAM. Este método se describe en la sección [documentación](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html).

## Duplicar y desaprobar elementos y métodos {#duplicate-and-deprecated-elements-and-methods}

En versiones anteriores de DIL y ECID, había métodos duplicados (métodos que hacen la misma función tanto en el DIL como en el ECID), lo que causaba confusión sobre cuál utilizar. Normalmente, necesitaba usar ambos y hacerlos coincidir, y ese mensaje no se comunicó bien a nuestros clientes. A partir de DIL 8.0, estos métodos y elementos duplicados han quedado obsoletos en DIL y se recomienda utilizar la versión de ECID.

Por ejemplo:

* Al usar [!DNL DIL.create], algunos elementos se han quedado obsoletos y en su lugar debe utilizar los elementos ECID. Estos elementos se llaman en la variable [[!DNL DIL.create] documentación](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html).
* La variable [!DNL idSync] el método de nivel de instancia también ha quedado obsoleto, como se indica en el informe [documentación](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-instance-methods.html).

## Sincronización de ID con un ID de cliente {#id-syncing-with-a-customer-id}

En AAM, puede sincronizar su UUID (ID de usuario único anónimo) en el equipo con un ID de cliente, de modo que pueda cargar datos sin conexión sobre ese cliente y vincularlos con su comportamiento en línea, para comprender mejor a sus clientes. En el pasado, esto se ha hecho de una de dos maneras:

* La variable [!DNL idSync] método de nivel de instancia
* La variable [!DNL declaredId] elemento en [!DNL DIL.create]

Si ha estado utilizando cualquiera de estos métodos antiguos para sincronizar con un ID de cliente, es muy recomendable actualizar a mediante el uso de [!DNL setCustomerIDs] que forma parte del servicio ECID. Más información sobre [!DNL setCustomerIDs] está disponible en el [documentación](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html).

**Sugerencia rápida:** Cuando anteriormente usaba cualquiera de los métodos anteriores, hacía referencia al AAM [!UICONTROL Data Source] con la variable [!UICONTROL Data Source] ID (también conocido como &quot;DPID&quot;). Al actualizar a [!DNL setCustomerIDs], deberá usar la AAM [!UICONTROL Data Source]s &quot;[!UICONTROL Integration Code]&quot; en su lugar. Sigue apuntando a lo mismo [!UICONTROL Data Source] pero es solo un identificador diferente. Esto se muestra en el siguiente vídeo.

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

En las secciones siguientes se enumeran los pasos y recomendaciones para actualizar a DIL 8.0 según el método de implementación:

## Actualización al DIL 8.0 en las etiquetas de Adobe Experience Platform {#updating-to-dil-in-experience-platform-launch}

Pasos básicos para la actualización a DIL 8.0

1. Si utiliza un DIL anterior a la versión 8.0, antes de actualizar, vaya a la configuración del DIL en la extensión de AAM y tome nota de las opciones avanzadas que esté utilizando (para utilizarlas en un paso posterior).
1. Actualice la extensión de AAM a la versión 8.0 o buenas.
1. Compruebe que la extensión del servicio de ID de Experience Cloud sea de la versión 3.3.0 o buena.
1. Para cualquier método o elemento obsoleto (como `disableIDSyncs`) que estaban en la extensión de AAM anterior a 8.0 o en código personalizado para DIL, habilite los métodos ECID en la extensión ECID.

   1. (DIL) `disableDestinationPublishingIframe` -> (ECID) `disableIdSyncs`
   1. (DIL) `disableIDSyncs` -> (ECID) `disableIdSyncs`
   1. (DIL) `iframeAkamaiHTTPS` -> (ECID) `dSyncSSLUseAkamai`
   1. (DIL) `declaredId` -> (ECID) `setCustomerIDs`

1. Publique los cambios.

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## Actualización al DIL 8.0 en la DTM de Adobe {#updating-to-dil-in-adobe-dtm}

1. Actualice la herramienta AAM a la versión 8.0 o buenas. Esta configuración de versión se encuentra en la sección &quot;General&quot; de la herramienta AAM.
1. Para cualquier método o elemento obsoleto (como `disableIDSyncs`) que estaban en el código personalizado de la herramienta de AAM anterior a la versión 8.0 para el DIL, tenga en cuenta estos (para que pueda agregarlos a la herramienta ECID) y luego eliminarlos de la variable [!DNL DIL code] en la herramienta AAM.
1. Actualizar la extensión del servicio de ID de Experience Cloud a la versión 3.3.0 o buena
1. Agregue las opciones avanzadas a la herramienta ECID que ha eliminado del código personalizado de la herramienta AAM.
1. Publicar los cambios

## Actualización al DIL 8.0 sin solución Tag Management de Adobe {#additional-resources}

Si está actualizando el código directamente en la página, puede reemplazar los elementos antiguos por elementos más nuevos, excepto cuando necesite mover métodos/elementos de DIL a ECID, como se ha explicado anteriormente. En ese caso, simplemente reemplazará el método/elemento antiguo en la ubicación del DIL por el método/elemento ECID en la ubicación ECID.

Lo mismo sucede con los administradores de etiquetas que no son de Adobe. Siempre que tenga las versiones anteriores en esa solución de administración de etiquetas, sustitúyalas por el nuevo código tal como se describe en los pasos siguientes.

1. Actualice la biblioteca de DIL a la versión más reciente (8.0 o buena). Deberá obtener el código de DIL más reciente de la asesoría de Adobe o del Servicio de atención al cliente de Adobe, ya que actualmente no está disponible en una ubicación pública. A continuación, reemplace el antiguo código de biblioteca del DIL por el nuevo código de biblioteca del DIL y continúe con el siguiente paso (no se detenga ahora o tendrá problemas, ja).
1. Instalar [!DNL ECID Service] o actualice su versión existente a 3.3.0 o buena. Puede descargar la última versión del servicio de ID de Experience Cloud [desde nuestra página de GitHub](https://github.com/Adobe-Marketing-Cloud/id-service/releases). Si necesita ayuda con esto, consulte la [documentación](https://experienceleague.adobe.com/docs/id-service/using/home.html) o hablar con un consultor de Adobe.

1. Compruebe que cualquier método o elemento obsoleto que se encuentre en el código personalizado para el DIL se mueva a los métodos ECID:

   1. (DIL) `disableDestinationPublishingIframe` -> (ECID) `disableIdSyncs`

      [Documentación](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) `disableIDSyncs` -> (ECID) `disableIdSyncs`

      [Documentación](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) `iframeAkamaiHTTPS` -> (ECID) `idSyncSSLUseAkamai`

      [Documentación](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html)

   1. (DIL) `declaredId` -> (ECID) `setCustomerIDs`

      [Documentación](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html)
