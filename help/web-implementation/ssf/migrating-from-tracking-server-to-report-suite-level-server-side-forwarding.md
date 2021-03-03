---
title: Migración del servidor de seguimiento al reenvío del lado del servidor en el nivel de grupo de informes
description: Este artículo y vídeo le mostrarán cómo habilitar el reenvío de datos de Analytics del lado del servidor a Audience Manager a nivel de grupo de informes en lugar de a nivel de servidor de seguimiento.
product: audience manager, analytics
feature: Integración de Adobe Analytics
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
role: '"Desarrollador, ingeniero de datos"'
level: Intermedio
translation-type: tm+mt
source-git-commit: a7dc335e75697a7b1720eccdadbb9605fdeda798
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 0%

---


# Migración de [!UICONTROL Tracking Server] a [!UICONTROL Report Suite] nivel [!UICONTROL Server-Side Forwarding] {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

Este artículo y vídeo le mostrarán cómo habilitar [!UICONTROL server-side forwarding] de [!DNL Analytics] datos para Audience Manager a nivel [!UICONTROL report suite] en lugar de a nivel [!UICONTROL tracking server].

## Primeros pasos {#introduction}

Si tiene Adobe Audience Manager Y Adobe Analytics, puede implementar &quot;[!UICONTROL Server-side Forwarding]&quot; de los datos [!DNL Analytics] en Audience Manager. Esto significa que, en lugar de que su página envíe 2 visitas (una a [!DNL Analytics] y otra a Audience Manager), simplemente puede enviar una visita a [!DNL Analytics] y [!DNL Analytics] reenviará esos datos a Audience Manager. Si ya lo tiene en marcha y lo ha habilitado o implementado antes de octubre de 2017, su [!UICONTROL server-side forwarding] puede estar basado en su &quot;[!UICONTROL Tracking Server]&quot;, que debe ser habilitado por el Servicio de atención al cliente de Adobe o por la Consultoría de Adobe. A partir de octubre de 2017, ahora puede configurar [!UICONTROL server-side forwarding] usted mismo y hacerlo a nivel [!UICONTROL Report Suite] (reenvío por [!UICONTROL Report Suite]). Esto tiene importantes beneficios, que se analizarán a continuación.

## [!UICONTROL Tracking Server] Reenvío  {#tracking-server-forwarding}

Su [!UICONTROL tracking server] es la ubicación a la que envía los datos de [!DNL Analytics] y también el dominio en el cual se escribe la solicitud de imagen y la cookie. Debe configurarse en DTM o [!DNL Experience Platform Launch], o en el archivo [!DNL AppMeasurement.js], y normalmente tendrá este aspecto, ya que su sitio o nombre de negocio reemplazarán a &quot;mysite&quot;:

`s.trackingServer = "mysite.sc.omtrdc.net";`

Si [!UICONTROL server-side forwarding] está configurado para reenviar en el nivel [!UICONTROL tracking server] , cualquier visita que se envíe a este [!UICONTROL tracking server] (si el servicio Experience Cloud ID también está habilitado) se reenviará a Audience Manager. Esto tenía que ser habilitado por el Servicio de atención al cliente de Adobe o por la Consultoría de Adobe. También son los que pueden desactivarlo DESPUÉS de haber cambiado al reenvío [!UICONTROL report suite], como se describe a continuación.

Si no está seguro de si [!DNL tracking server forwarding] está habilitado para usted, póngase en contacto con el Servicio de atención al cliente de Adobe o con la Consultoría de Adobe, y estos le informarán al respecto.

## [!UICONTROL Report Suite]-Nivel  [!UICONTROL Server-Side Forwarding] {#report-suite-level-server-side-forwarding}

Una de las mayores ventajas de pasar al reenvío [!UICONTROL report suite] desde el reenvío [!UICONTROL tracking server] es que ahora podrá utilizar &quot;Audience Analytics&quot;, que es la capacidad de reenviar Audience Manager [!UICONTROL segments] a Adobe Analytics para realizar un análisis detallado [!UICONTROL segment]. Esta gran función NO es compatible si todavía se encuentra en el reenvío [!UICONTROL tracking server] y no en el reenvío [!UICONTROL report suite]. Consulte más información sobre Audience Analytics en la [documentación](https://marketing.adobe.com/resources/help/en_US/analytics/audiences/).

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## Sugerencia importante {#additional-resources}

Como se indica en el vídeo anterior, una vez que haya configurado todos los [!UICONTROL report suites] para reenviar que desea reenviar a Audience Manager, debe ponerse en contacto con el Servicio de atención al cliente de Adobe o con la Consultoría de Adobe y solicitar que deshabiliten el reenvío [!UICONTROL tracking server]. No es una emergencia para usted hacer esto, porque tener tanto el reenvío [!UICONTROL tracking server] como el [!UICONTROL report suite] NO dará como resultado visitas duplicadas. Sin embargo, se recomienda tener únicamente el reenvío [!UICONTROL report suite] activado. Si deja el reenvío [!UICONTROL tracking server] activado, no solo podría reenviar datos de [!UICONTROL report suites] que no desea reenviar, sino que en el futuro, después de que usted (y todos los miembros de la empresa) hayan olvidado que el reenvío [!UICONTROL tracking server] está activado, podría pensar que los datos no se reenvían para un [!UICONTROL report suite] específico (porque no se activan en el nivel del grupo de informes), pero los datos se siguen reenviando debido a/>. [!UICONTROL tracking server] Entonces, malgastará tiempo y dinero para averiguar por qué reenvía y también pagará por las llamadas al servidor AAM que no esperaba. Por lo tanto, es una buena idea desactivar el reenvío [!UICONTROL tracking server] tan pronto como tenga todos los [!UICONTROL report suites] configurados para reenviar que tengan sentido para sus necesidades comerciales.
