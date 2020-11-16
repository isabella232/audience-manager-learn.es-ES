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


# Migración de [!UICONTROL Tracking Server] a [!UICONTROL Report Suite]nivel [!UICONTROL Server-Side Forwarding] {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

Este artículo y vídeo le mostrarán cómo habilitar [!UICONTROL server-side forwarding] los datos para [!DNL Analytics] el Audience Manager en un [!UICONTROL report suite] nivel en lugar de en un [!UICONTROL tracking server] nivel.

## Primeros pasos {#introduction}

Si tiene Adobe Audience Manager Y Adobe Analytics, puede implementar &quot;[!UICONTROL Server-side Forwarding]&quot; de los [!DNL Analytics] datos en Audience Manager. Esto significa que, en lugar de que la página envíe 2 visitas (una a [!DNL Analytics] y otra a Audience Manager), simplemente puede enviar una visita a [!DNL Analytics]y [!DNL Analytics] reenviar los datos al Audience Manager. Si ya tiene esto en marcha, y si lo tenía habilitado/implementado antes de octubre de 2017, [!UICONTROL server-side forwarding] podría estar basado en su &quot;[!UICONTROL Tracking Server]&quot;, que tenía que ser activado por el Servicio de atención al cliente de Adobe o por la asesoría de Adobes. A partir de octubre de 2017, [!UICONTROL server-side forwarding] puede configurarse y hacerlo a [!UICONTROL Report Suite] nivel (reenvío POR [!UICONTROL Report Suite]). Esto tiene importantes beneficios, que se analizarán a continuación.

## [!UICONTROL Tracking Server] Reenvío {#tracking-server-forwarding}

Su ubicación [!UICONTROL tracking server] es la ubicación a la que está enviando [!DNL Analytics] los datos, y también el dominio en el cual se escriben la solicitud de imagen y la cookie. Debe configurarse en la DTM o [!DNL Experience Platform Launch], o en el [!DNL AppMeasurement.js] archivo, y tendrá el siguiente aspecto: su sitio o nombre comercial reemplazará a &quot;mysite&quot;:

`s.trackingServer = "mysite.sc.omtrdc.net";`

Si [!UICONTROL server-side forwarding] se configura para reenviar en el [!UICONTROL tracking server] nivel, cualquier visita que se envíe a esto [!UICONTROL tracking server] (SI el servicio de ID de Experience Cloud también está habilitado) se reenviará al Audience Manager. Esto tenía que ser activado por Adobe Customer Care o Adobe Consulting. También son ellos los que pueden desactivarlo, DESPUÉS de haber cambiado a [!UICONTROL report suite] reenvío, como se describe a continuación.

Si no está seguro de si [!DNL tracking server forwarding] está habilitado para usted, póngase en contacto con el Servicio de atención al cliente de Adobe o con el servicio de consultoría de Adobe, y estos le informarán al respecto.

## [!UICONTROL Report Suite]-Nivel [!UICONTROL Server-Side Forwarding] {#report-suite-level-server-side-forwarding}

Una de las mayores ventajas de pasar al [!UICONTROL report suite] reenvío desde el [!UICONTROL tracking server] reenvío es que ahora podrá utilizar &quot;Audience Analytics&quot;, que es la capacidad de reenviar al Audience Manager [!UICONTROL segments] a Adobe Analytics para obtener una [!UICONTROL segment] análisis detallada. Esta buena función NO es compatible si aún está en [!UICONTROL tracking server] reenvío y no [!UICONTROL report suite] reenvío. Consulte más información sobre Audience Analytics en la [documentación](https://marketing.adobe.com/resources/help/en_US/analytics/audiences/).

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## Sugerencia importante {#additional-resources}

Como se indica en el vídeo anterior, una vez que tenga todo el [!UICONTROL report suites] conjunto configurado para reenviar que desea reenviar al Audience Manager, debe ponerse en contacto con el Servicio de atención al cliente de Adobe o con la asesoría de Adobes y hacer que deshabiliten el [!UICONTROL tracking server] reenvío. No es una emergencia para usted hacer esto, porque tener tanto [!UICONTROL tracking server] reenvío como [!UICONTROL report suite] reenvío NO resultará en visitas de duplicado. Sin embargo, es recomendable que solo se [!UICONTROL report suite] reenvíe. Si deja [!UICONTROL tracking server] el reenvío activado, no sólo puede que reenvíe datos de los [!UICONTROL report suites] que no desea reenviar, sino que en el futuro, después de que usted (y todos los usuarios de la compañía) hayan olvidado que el reenvío está activado, puede que piense que los datos no se reenvían para un [!UICONTROL tracking server] (ya que no se activan en el nivel del grupo de informes), pero los datos se siguen reenviando de todos modos debido al [!UICONTROL report suite] [!UICONTROL tracking server]. Entonces, usted perderá tiempo y dinero para averiguar por qué reenvía y también pagará por AAM llamadas al servidor que no esperaba. Por lo tanto, es una buena idea desactivar el [!UICONTROL tracking server] reenvío tan pronto como tenga todo el [!UICONTROL report suites] conjunto para avanzar que tenga sentido para sus necesidades comerciales.
