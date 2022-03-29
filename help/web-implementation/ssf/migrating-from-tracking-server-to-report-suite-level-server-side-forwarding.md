---
title: Migración del servidor de seguimiento al reenvío del lado del servidor en el nivel de grupo de informes
description: Obtenga información sobre cómo habilitar el reenvío de datos de Adobe Analytics del lado del servidor a un Audience Manager a nivel de grupo de informes en lugar de a nivel de servidor de seguimiento.
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

# Migración del servidor de seguimiento al reenvío del lado del servidor en el nivel de grupo de informes {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

Este artículo y vídeo le mostrarán cómo habilitar el reenvío del lado del servidor de [!DNL Analytics] Datos para el Audience Manager a [!UICONTROL report suite] en lugar de en un [!UICONTROL tracking server] nivel.

## Primeros pasos {#introduction}

Si tiene Adobe Audience Manager Y Adobe Analytics, puede implementar el reenvío del lado del servidor de la variable [!DNL Analytics] datos para el Audience Manager. Esto significa que, en lugar de que la página envíe dos visitas (una a [!DNL Analytics] y uno al Audience Manager), puede enviar una visita a [!DNL Analytics]y [!DNL Analytics] reenviará esos datos al Audience Manager.

Si ya lo tiene en funcionamiento y lo ha habilitado o implementado antes de octubre de 2017, el reenvío del lado del servidor puede basarse en su [!UICONTROL Tracking Server], que deben ser activados por el Servicio de atención al cliente de Adobe o la Consultoría de Adobe. A partir de octubre de 2017, ahora puede configurar el reenvío del lado del servidor y hacerlo a nivel de grupo de informes (reenvío por grupo de informes). Esto tiene importantes beneficios, que se examinan a continuación.

## [!UICONTROL Tracking server] reenvío {#tracking-server-forwarding}

Su [!UICONTROL tracking server] es la ubicación a la que envía su [!DNL Analytics] y también el dominio en el que se escribe la solicitud de imagen y la cookie. Debe configurarse en DTM o [!DNL Experience Platform Launch]o en la variable [!DNL AppMeasurement.js] y normalmente se verán así: su sitio o nombre comercial reemplazarán a &quot;mysite&quot;:

`s.trackingServer = "mysite.sc.omtrdc.net";`

Si el reenvío del lado del servidor está configurado para reenviarse en la variable [!UICONTROL tracking server] , cualquier visita que se envíe a esta [!UICONTROL tracking server] (SI el servicio de ID de Experience Cloud también está habilitado) se reenviará al Audience Manager. Esto tenía que ser habilitado por el Servicio de atención al cliente de Adobe o la Consultoría de Adobe. También son los que pueden desactivarlo, DESPUÉS de haber cambiado a [!UICONTROL report suite] como se describe a continuación.

Si no está seguro si [!DNL tracking server forwarding] está habilitado para usted, póngase en contacto con el servicio de atención al cliente de Adobe o con la asesoría de Adobe, y deben poder informarle al respecto.

## [!UICONTROL Report-suite]-Reenvío del lado del servidor {#report-suite-level-server-side-forwarding}

Uno de los mayores beneficios de pasar a [!UICONTROL report suite] reenviar desde [!UICONTROL tracking server] el reenvío es que ahora podrá utilizar &quot;Audience Analytics&quot;, que es la capacidad de reenviar Audience Manager [!UICONTROL segments] vuelva a Adobe Analytics para obtener un análisis detallado de los segmentos. Esta buena función NO es compatible si aún se encuentra en [!UICONTROL tracking server] reenvío y no [!UICONTROL report suite] reenvío. Consulte más información sobre el Audience Analytics en la [documentación](https://experienceleague.adobe.com/docs/analytics/integration/audience-analytics/mc-audiences-aam.html).

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## Sugerencia importante {#additional-resources}

Como se indica en el vídeo anterior, una vez que tenga todas las [!UICONTROL report suites] configurado para reenviar que desea reenviar a Audience Manager, debe ponerse en contacto con el servicio de atención al cliente de Adobe o con el servicio de consultoría de Adobe y hacer que deshabiliten el [!UICONTROL tracking server] reenvío. No es una emergencia para usted hacer esto, porque tener ambos [!UICONTROL tracking server] reenvío y [!UICONTROL report suite] el reenvío no produce visitas duplicadas. Sin embargo, es recomendable tener [!UICONTROL report suite] reenvío activado.

Si se va [!UICONTROL tracking server] reenvío activado, no solo podría reenviar datos desde [!UICONTROL report suites] que no desea reenviar, pero en el futuro, después de que usted (y todos los miembros de su empresa) hayan olvidado que [!UICONTROL tracking server] el reenvío está activado, puede pensar que los datos no se reenvían para un [!UICONTROL report suite]. Esto se debe a que no está activado en el nivel de grupo de informes, pero los datos se siguen reenviando de todos modos debido al [!UICONTROL tracking server]. Entonces, malgastará tiempo y dinero para averiguar por qué reenvía y también pagará por AAM llamadas al servidor que no esperaba. Por lo tanto, es aconsejable desactivar [!UICONTROL tracking server] reenvío en cuanto tenga todos los [!UICONTROL report suites] configure para que el reenvío tenga sentido según sus necesidades comerciales.
