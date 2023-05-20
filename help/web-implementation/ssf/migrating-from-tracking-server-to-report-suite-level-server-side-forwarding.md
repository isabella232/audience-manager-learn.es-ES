---
title: Migración del servidor de seguimiento al reenvío del lado de servidor en el nivel de grupo de informes
description: Obtenga información sobre cómo habilitar el reenvío de datos de Adobe Analytics del lado del servidor al Audience Manager en el nivel de grupo de informes en lugar de en el nivel de servidor de seguimiento.
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
role: Developer, Data Engineer
level: Intermediate
exl-id: 08b81e52-a28a-43e4-a284-df2460a43016
source-git-commit: 4adaade180545bcf5f911b7453a7e9939e2ed178
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 0%

---

# Migración del servidor de seguimiento al reenvío del lado de servidor en el nivel de grupo de informes {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

Este artículo y vídeo le muestran cómo habilitar el reenvío del lado del servidor de [!DNL Analytics] Datos para el Audience Manager a la vez [!UICONTROL report suite] nivel en lugar de en un [!UICONTROL tracking server] nivel.

## Primeros pasos {#introduction}

Si tiene Adobe Audience Manager AND Adobe Analytics, puede implementar el reenvío del lado del servidor de [!DNL Analytics] datos al Audience Manager. Esto significa que, en lugar de que la página envíe dos visitas (una a [!DNL Analytics] y uno a Audience Manager), puede enviar una visita a [!DNL Analytics], y [!DNL Analytics] reenviará esos datos al Audience Manager.

Si ya lo tiene en funcionamiento y lo tenía habilitado o implementado antes de octubre de 2017, el reenvío del lado del servidor podría basarse en [!UICONTROL Tracking Server], que tenía que ser activada por el Servicio de atención al cliente de Adobe o por la Asesoría de Adobe. Desde octubre de 2017, ahora puede configurar el reenvío del lado del servidor por su cuenta y hacerlo en el nivel de grupo de informes (reenvío por grupo de informes). Hay beneficios significativos en esto, que se discuten más adelante.

## [!UICONTROL Tracking server] reenvío {#tracking-server-forwarding}

Su [!UICONTROL tracking server] es la ubicación a la que envía su [!DNL Analytics] datos, y también el dominio en el que se escribe la solicitud de imagen y la cookie. Debe configurarse en la DTM o [!DNL Experience Platform Launch], o en el [!DNL AppMeasurement.js] y normalmente lucirán de esta manera, con el nombre de su sitio o empresa reemplazando &quot;misitio&quot;:

`s.trackingServer = "mysite.sc.omtrdc.net";`

Si el reenvío del lado del servidor está configurado para el reenvío en la [!UICONTROL tracking server] nivel, cualquier visita que se envíe a este [!UICONTROL tracking server] (SI el servicio de ID de Experience Cloud también está activado) se reenviará al Audience Manager. Esto tenía que ser habilitado por el Servicio de atención al cliente de Adobe o la Asesoría de Adobe. También son ellos los que pueden deshabilitarlo, DESPUÉS de que haya cambiado a [!UICONTROL report suite] reenvío, tal como se describe a continuación.

Si no está seguro de si [!DNL tracking server forwarding] está habilitado para usted, póngase en contacto con el Servicio de atención al cliente de Adobe o con la Asesoría de Adobe, y deben poder informarle.

## [!UICONTROL Report-suite]Reenvío de nivel de servidor {#report-suite-level-server-side-forwarding}

Uno de los mayores beneficios de mudarse a [!UICONTROL report suite] reenvío desde [!UICONTROL tracking server] el reenvío es que ahora podrá utilizar &quot;Audience Analytics&quot;, que es la capacidad de reenviar Audience Manager [!UICONTROL segments] Vuelva a Adobe Analytics para realizar un análisis detallado de los segmentos. Esta buena función NO es compatible si aún se encuentra en [!UICONTROL tracking server] reenvío y no [!UICONTROL report suite] reenvío. Consulte más información sobre Audience Analytics en la [documentación](https://experienceleague.adobe.com/docs/analytics/integration/audience-analytics/mc-audiences-aam.html).

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## Sugerencia importante {#additional-resources}

Como se indica en el vídeo anterior, una vez que tenga todas las [!UICONTROL report suites] configurado para el reenvío que desea reenviar a Audience Manager, debe ponerse en contacto con el Servicio de atención al cliente de Adobe o con la Asesoría de Adobe y hacer que deshabiliten la variable [!UICONTROL tracking server] reenvío. No es una emergencia para usted hacer esto, porque tener ambos [!UICONTROL tracking server] reenvío y [!UICONTROL report suite] el reenvío no genera visitas duplicadas. Sin embargo, es recomendable tener solo [!UICONTROL report suite] reenvío en.

Si te vas [!UICONTROL tracking server] reenvío activado, no solo podría reenviar datos de [!UICONTROL report suites] que no desea que se le reenvíe, pero en el futuro, después de que usted (y todos los miembros de su empresa) haya olvidado que [!UICONTROL tracking server] reenvío está activado, podría pensar que los datos no se reenvían para un específico [!UICONTROL report suite]. Esto se debe a que no está activado en el nivel de grupo de informes, pero los datos se siguen reenviando de todos modos debido a la variable [!UICONTROL tracking server]. AAM Entonces perderá tiempo y dinero averiguando por qué reenvía y también pagando por llamadas al servidor que no esperaba. Por lo tanto, es aconsejable deshabilitarlo [!UICONTROL tracking server] reenvío tan pronto como tenga todas las [!UICONTROL report suites] configure para que avancen y tengan sentido para sus necesidades empresariales.
