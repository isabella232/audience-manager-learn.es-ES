---
title: Actualización a la versión 8.0 de Adobe Audience Manager DIL (o buena)
description: En este artículo se explican los pasos y recomendaciones para actualizar el código de Data Integration Library (DIL) de Adobe Audience Manager AAM () a la versión 8.0 o posterior. Esto hace referencia a la implementación de DIL del lado del cliente, no al reenvío de datos de Adobe Analytics del lado del servidor, y cubre DTM, Launch by Adobe e implementaciones sin la solución de administrador de etiquetas de Adobe.
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

# Actualización a la versión 8.0 (o buena) de DIL de Adobe Audience Manager {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

En este artículo se explican los pasos y las recomendaciones para actualizar Adobe Audience Manager AAM () [!DNL Data Integration Library] Código (DIL) de la versión 8.0 o posterior de. Esto hace referencia a la implementación de DIL del lado del cliente, no al reenvío de datos de Adobe Analytics del lado del servidor, y cubre DTM, Launch by Adobe e implementaciones sin la solución de administrador de etiquetas de Adobe.

## Información general {#overview}

del Audience Manager [!DNL Data Integration Library] El código (DIL AAM) le permite implementar el código en su sitio web*. Al implementar versiones anteriores de DIL, no era necesario tener implementado también el servicio de ID de Experience Cloud (ECID) de Adobe (aunque era una muy buena idea). A partir de la versión 8.0 de DIL, existe una dependencia estricta de la versión 3.3 o posterior de ECID. Si implementa DIL 8.0 o posterior sin ECID 3.3 o con una versión anterior, obtendrá un error y no funcionará. AAM Dado que hay varias formas de implementar la aplicación, hemos creado esta página para darle algunos pasos que debe seguir, así como algunas recomendaciones. A continuación, se muestran estos pasos y recomendaciones desglosados por plataforma o método de implementación. Encontrará más información sobre DIL en la [documentación](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=en).

* Como se indica en la descripción de esta página, esto solo cubre las implementaciones de DIL AAM del lado del cliente, utilizadas por clientes que no tienen Adobe Analytics y que no son de la misma manera que se utilizan en el lado del cliente. Si tiene Adobe Analytics AAM, debe utilizar el método de reenvío del lado del servidor para implementar la implementación de la función de reenvío de la parte del servidor de la interfaz de usuario de la aplicación de. Este método se describe en la [documentación](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html).

## Elementos y métodos duplicados y obsoletos {#duplicate-and-deprecated-elements-and-methods}

En versiones anteriores de DIL y ECID, había métodos duplicados (métodos que realizan la misma función tanto en DIL como en ECID), lo que causaba confusión sobre cuál usar. Normalmente, necesitaba utilizar ambos y hacerlos coincidir, y ese mensaje no se comunicaba bien a nuestros clientes. A partir de DIL 8.0, estos métodos y elementos duplicados han quedado obsoletos en DIL y se recomienda utilizar la versión de ECID.

Por ejemplo:

* Al utilizar [!DNL DIL.create], algunos elementos se han desaprobado y debe utilizar los elementos ECID en su lugar. Estos elementos se llaman en la variable [[!DNL DIL.create] documentación](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html).
* El [!DNL idSync] el método de nivel de instancia también se ha desaprobado, como se llama en el [documentación](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-instance-methods.html).

## Sincronización de ID con un ID de cliente {#id-syncing-with-a-customer-id}

AAM En, puede sincronizar su UUID (ID único de usuario anónimo) en el equipo con un ID de cliente, de modo que pueda cargar datos sin conexión sobre ese cliente y vincularlos con su comportamiento en línea para comprender mejor a sus clientes. En el pasado, esto se ha hecho de una de las dos maneras siguientes:

* El [!DNL idSync] método de nivel de instancia
* El [!DNL declaredId] elemento en [!DNL DIL.create]

Si ha estado utilizando cualquiera de estos métodos antiguos para sincronizarse con un ID de cliente, se recomienda encarecidamente que actualice a con el [!DNL setCustomerIDs] , que forma parte del servicio ECID. Más información sobre [!DNL setCustomerIDs] está disponible en el [documentación](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html).

**Sugerencia rápida:** AAM Cuando anteriormente utilizaba cualquiera de los métodos anteriores, hacía referencia a la [!UICONTROL Data Source] con el [!UICONTROL Data Source] ID (también conocido como &quot;DPID&quot;). Al actualizar a [!DNL setCustomerIDs]AAM , tendrá que usar la opción de configuración de la [!UICONTROL Data Source]&quot;s&quot;[!UICONTROL Integration Code]&quot; en su lugar. Todavía apunta a lo mismo [!UICONTROL Data Source] pero es solo un identificador diferente. Esto se muestra en el siguiente vídeo.

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

En las siguientes secciones se enumeran los pasos y recomendaciones para actualizar a DIL 8.0 según el método de implementación:

## Actualización a DIL 8.0 en Adobe Experience Platform tags {#updating-to-dil-in-experience-platform-launch}

Pasos básicos para actualizar a DIL 8.0

1. Si está utilizando un DIL anterior a 8.0, antes de actualizar, vaya a la configuración del DIL AAM en la extensión de la extensión de la extensión y tome nota de cualquier opción avanzada que esté utilizando (para utilizar en un paso posterior).
1. AAM Actualice la extensión de la a la versión 8.0 o buena.
1. Compruebe que la extensión del servicio de ID de Experience Cloud sea de la versión 3.3.0 o buena.
1. Para cualquier método o elemento obsoleto (como `disableIDSyncs`AAM ) que estaban en su extensión anterior a la 8.0 o en el código personalizado para DIL, habilite los métodos ECID en la extensión ECID.

   1. (DIL) `disableDestinationPublishingIframe` -> (ECID) `disableIdSyncs`
   1. (DIL) `disableIDSyncs` -> (ECID) `disableIdSyncs`
   1. (DIL) `iframeAkamaiHTTPS` -> (ECID) `dSyncSSLUseAkamai`
   1. (DIL) `declaredId` -> (ECID) `setCustomerIDs`

1. Publique los cambios.

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## Actualización a DIL 8.0 en Adobe DTM {#updating-to-dil-in-adobe-dtm}

1. AAM Actualice la herramienta de a la versión 8.0 o buena. AAM Esta configuración de versión se encuentra en la sección &quot;General&quot; de la herramienta de.
1. Para cualquier método o elemento obsoleto (como `disableIDSyncs`AAM ) que estaban en el código personalizado de la herramienta de creación de informes anterior a la versión 8.0 de para DIL, anótelos (para que pueda agregarlos a la herramienta ECID) y, a continuación, elimínelos del código personalizado de [!DNL DIL code] AAM en la herramienta de.
1. Actualice la extensión del servicio de ID de Experience Cloud a la versión 3.3.0 o buena
1. AAM Añada las opciones avanzadas a la herramienta ECID que ha eliminado del código personalizado de la herramienta de.
1. Publicación de los cambios

## Actualización a DIL 8.0 sin solución de Adobe de Tag Management {#additional-resources}

Si actualiza el código directamente en la página, puede reemplazar los elementos más antiguos por elementos más nuevos, excepto cuando necesite mover métodos/elementos de DIL a ECID, como se ha indicado anteriormente. En ese caso, simplemente reemplazará el método/elemento antiguo en la ubicación del DIL con el método/elemento ECID en la ubicación ECID.

Lo mismo ocurre con los administradores de etiquetas que no son de Adobe. Siempre que tenga las versiones antiguas en esa solución de administración de etiquetas, sustitúyalo por el código nuevo tal como se describe en los pasos siguientes.

1. Actualice la biblioteca del DIL a la última versión (8.0 o buena): necesitará obtener el código del DIL más reciente de la asesoría de Adobe o del Servicio de atención al cliente de Adobe, ya que actualmente no está disponible en una ubicación pública. A continuación, simplemente sustituya el código antiguo de la biblioteca de DIL por el nuevo código de la biblioteca de DIL y continúe con el siguiente paso (no se detenga ahora o tendrá problemas, ha).
1. Instalar [!DNL ECID Service] o actualizar la versión existente a la 3.3.0 o buena. Puede descargar la versión más reciente del servicio de ID de Experience Cloud [desde nuestra página de GitHub](https://github.com/Adobe-Marketing-Cloud/id-service/releases). Si necesita ayuda con esto, consulte la [documentación](https://experienceleague.adobe.com/docs/id-service/using/home.html) o hable con un consultor de Adobe.

1. Compruebe que todos los métodos o elementos obsoletos del código personalizado de DIL se muevan a los métodos ECID:

   1. (DIL) `disableDestinationPublishingIframe` -> (ECID) `disableIdSyncs`

      [Documentación](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) `disableIDSyncs` -> (ECID) `disableIdSyncs`

      [Documentación](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) `iframeAkamaiHTTPS` -> (ECID) `idSyncSSLUseAkamai`

      [Documentación](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html)

   1. (DIL) `declaredId` -> (ECID) `setCustomerIDs`

      [Documentación](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html)
