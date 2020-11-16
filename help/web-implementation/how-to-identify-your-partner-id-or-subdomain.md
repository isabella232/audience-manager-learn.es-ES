---
title: Cómo identificar su ID de socio Audience Manager o subdominio
description: Al implementar algunas funciones de Experience Cloud, debe saber qué es su "ID de socio" de Audience Manager (también denominado a veces "ID de cliente" o "Subdominio"). En este vídeo, le mostraremos dos lugares en los que puede obtener este ID en la interfaz de usuario del Audience Manager.
feature: implementation basics
topics: null
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 2359
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '331'
ht-degree: 0%

---


# Cómo identificar el subdominio Audience Manager {#how-to-identify-your-audience-manager-partner-id-or-subdomain}

Al implementar algunas funciones de Experience Cloud, debe saber qué es su Audience Manager `Subdomain` (también conocido a veces como su `client ID` o `Partner ID`). En este vídeo, le mostraremos dos lugares donde puede obtener esta información en la interfaz de usuario del Audience Manager.

## Dejando de lado el final... {#giving-away-the-ending}

En caso de que prefiera simplemente saltar y encontrarlo sin ver este breve vídeo, puede encontrar el vídeo `Partner Subdomain` en dos lugares en la interfaz de usuario:

1. Si ya ha creado un [!UICONTROL rule-based] [!UICONTROL trait], haga clic en **[!UICONTROL Get Trait URL]**
   [!UICONTROL Get Trait URL] está junto a la [!UICONTROL trait] en la lista de [!UICONTROL traits] en esa carpeta y la dirección URL incluirá su subdominio en la dirección URL.
1. Si va a la interfaz **[!UICONTROL Tools]** > **[!UICONTROL Tags]** y hace clic **[!UICONTROL Get code]** para el contenedor, el subdominio se encuentra al final de la línea Akamai

Si no puede encontrarlo rápidamente con esas referencias rápidas, el vídeo es un compromiso de poco tiempo. :)

>[!VIDEO](https://video.tv.adobe.com/v/25922/?quality=12)

>[!IMPORTANT]
>
>Hay un ID numérico asignado a cada cliente del Adobe Experience Cloud, y esto suele conocerse como &quot;PID&quot; o ID de socio. Esta no es la identificación de la que hablamos en este artículo y vídeo. En su lugar, el &quot;subdominio del socio&quot;, al que a veces se hace referencia como ID del socio, es generalmente una versión del nombre del cliente y es el subdominio del servidor al que se envían los datos. Por ejemplo, si tu compañía es &quot;Bob&#39;s Knobs&quot; (todos los controles de puerta, por supuesto, jaja), entonces es probable que tu subdominio asociado sea &quot;bobsknobs&quot;, mientras que el &quot;PID&quot; sería algo más como &quot;12345&quot;. No suele ser necesario conocer el PID, pero es importante conocer el subdominio para poder configurar la implementación de Audience Manager.

