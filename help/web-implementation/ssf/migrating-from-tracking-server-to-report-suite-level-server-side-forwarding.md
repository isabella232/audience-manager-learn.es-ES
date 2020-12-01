---
title: Migración del servidor de seguimiento al reenvío del servidor de nivel de grupo de informes
description: Este artículo y vídeo le mostrarán cómo habilitar el reenvío de datos de Analytics del lado del servidor al Audience Manager en un nivel de grupo de informes en lugar de hacerlo en un nivel de servidor de seguimiento.
product: audience manager, analytics
feature: integration with analytics
topics: null
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
translation-type: tm+mt
source-git-commit: dfd549508cc223714bdb07ac6fd2aa31e6ca5586
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 0%

---


# Migración de [!UICONTROL Tracking Server] a [!UICONTROL Report Suite] nivel [!UICONTROL Server-Side Forwarding] {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

Este artículo y vídeo le mostrarán cómo habilitar [!UICONTROL server-side forwarding] de [!DNL Analytics] datos para el Audience Manager en un nivel [!UICONTROL report suite] en lugar de hacerlo en un nivel [!UICONTROL tracking server].

## Primeros pasos {#introduction}

Si tiene Adobe Audience Manager Y Adobe Analytics, puede implementar &quot;[!UICONTROL Server-side Forwarding]&quot; de los datos [!DNL Analytics] en Audience Manager. Esto significa que, en lugar de que la página envíe 2 visitas (una a [!DNL Analytics] y otra a Audience Manager), simplemente puede enviar una visita a [!DNL Analytics] y [!DNL Analytics] reenviará esos datos al Audience Manager. Si ya tiene esto en marcha y lo ha habilitado o implementado antes de octubre de 2017, su [!UICONTROL server-side forwarding] podría basarse en su &quot;[!UICONTROL Tracking Server]&quot;, que tuvo que ser habilitado por el Servicio de atención al cliente de Adobe o por la Consultoría de Adobe. A partir de octubre de 2017, puede configurarse [!UICONTROL server-side forwarding] usted mismo y hacerlo a un nivel [!UICONTROL Report Suite] (reenviando por [!UICONTROL Report Suite]). Esto tiene importantes beneficios, que se analizarán a continuación.

## [!UICONTROL Tracking Server] Reenvío  {#tracking-server-forwarding}

Su [!UICONTROL tracking server] es la ubicación a la que está enviando los datos [!DNL Analytics] y también el dominio en el cual se escriben la solicitud de imagen y la cookie. Debe configurarse en la DTM o [!DNL Experience Platform Launch], o en el archivo [!DNL AppMeasurement.js], y tendrá el siguiente aspecto: su sitio o nombre comercial reemplazará a &quot;mysite&quot;:

`s.trackingServer = "mysite.sc.omtrdc.net";`

Si [!UICONTROL server-side forwarding] está configurado para reenviarse en el nivel [!UICONTROL tracking server], cualquier visita que se envíe a este [!UICONTROL tracking server] (SI el servicio de ID de Experience Cloud también está habilitado) se reenviará al Audience Manager. Esto tenía que ser activado por Adobe Customer Care o Adobe Consulting. También son los que pueden deshabilitarlo, DESPUÉS de haber cambiado al reenvío [!UICONTROL report suite], como se describe a continuación.

Si no está seguro de si [!DNL tracking server forwarding] está habilitado para usted, póngase en contacto con el Servicio de atención al cliente de Adobe o con la asesoría de Adobes, y deberán poder comunicárselo.

## [!UICONTROL Report Suite]-Nivel  [!UICONTROL Server-Side Forwarding] {#report-suite-level-server-side-forwarding}

Uno de los mayores beneficios de pasar al reenvío [!UICONTROL report suite] desde el reenvío [!UICONTROL tracking server] es que ahora podrá usar &quot;Audience Analytics&quot;, que es la capacidad de reenviar al Audience Manager [!UICONTROL segments] a Adobe Analytics para una análisis detallada [!UICONTROL segment]. Esta buena función NO se admite si aún se está reenviando [!UICONTROL tracking server] y no [!UICONTROL report suite]. Consulte más información sobre Audience Analytics en la [documentación](https://marketing.adobe.com/resources/help/en_US/analytics/audiences/).

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## Sugerencia importante {#additional-resources}

Como se indica en el vídeo anterior, una vez que tenga todo el [!UICONTROL report suites] configurado para reenviar que desea reenviar a Audience Manager, debe ponerse en contacto con el Servicio de atención al cliente de Adobe o con la asesoría de Adobes y hacer que deshabiliten el reenvío [!UICONTROL tracking server]. No es una emergencia que usted haga esto, porque tener tanto el reenvío [!UICONTROL tracking server] como el reenvío [!UICONTROL report suite] NO resultará en visitas de duplicado. Sin embargo, es recomendable que solo se reenvíe [!UICONTROL report suite]. Si deja el reenvío [!UICONTROL tracking server] activado, no sólo podría reenviar datos de [!UICONTROL report suites] que no desee reenviar, sino que en el futuro, después de que usted (y todos los usuarios de su compañía) olvidaron que el reenvío [!UICONTROL tracking server] está activado, podría pensar que los datos no se reenvían para un [!UICONTROL report suite] específico (porque no se encienden en el nivel del grupo de informes), pero los datos se siguen reenviando de todas maneras debido a &lt;a 4/>. [!UICONTROL tracking server] Entonces, usted perderá tiempo y dinero para averiguar por qué reenvía y también pagará por AAM llamadas al servidor que no esperaba. Por lo tanto, es una buena idea desactivar el reenvío [!UICONTROL tracking server] en cuanto tenga todo el [!UICONTROL report suites] configurado para reenviar que tenga sentido para sus necesidades comerciales.
