---
title: Identificación del ID de socio o subdominio de Audience Manager
description: Al implementar algunas funciones de Experience Cloud, debe saber qué es su "ID de socio" de Audience Manager (también conocido como "ID de cliente" o "subdominio"). En este vídeo, se muestran dos lugares en los que puede obtener este ID en la interfaz de usuario de Audience Manager.
feature: Implementation Basics
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 2359
role: "Developer, Data Engineer"
level: Intermediate
translation-type: tm+mt
source-git-commit: a7dc335e75697a7b1720eccdadbb9605fdeda798
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 0%

---


# Cómo identificar el subdominio de Audience Manager {#how-to-identify-your-audience-manager-partner-id-or-subdomain}

Al implementar algunas funciones de Experience Cloud, debe saber qué es Audience Manager `Subdomain` (también conocido como `client ID` o `Partner ID`). En este vídeo, se muestran dos lugares en los que puede obtener esta información en la interfaz de usuario de Audience Manager.

## Dejando de lado el final... {#giving-away-the-ending}

En caso de que prefiera simplemente saltar y encontrarlo sin ver este breve vídeo, puede encontrar su `Partner Subdomain` en dos lugares en la interfaz de usuario:

1. Si ya ha creado un [!UICONTROL rule-based] [!UICONTROL trait], haga clic en **[!UICONTROL Get Trait URL]**
   [!UICONTROL Get Trait URL] está junto a  [!UICONTROL trait] en la lista de  [!UICONTROL traits] en esa carpeta y la dirección URL incluirá su subdominio en la dirección URL.
1. Si va a la interfaz **[!UICONTROL Tools]** > **[!UICONTROL Tags]** y hace clic en **[!UICONTROL Get code]** para el contenedor, el subdominio se acerca al final de la línea de Akamai

Si no puede encontrarlo rápidamente con esas referencias rápidas, el vídeo es un breve compromiso de tiempo. :)

>[!VIDEO](https://video.tv.adobe.com/v/25922/?quality=12)

>[!IMPORTANT]
>
>Hay un ID numérico asignado a cada cliente de Adobe Experience Cloud, que suele denominarse &quot;PID&quot; o &quot;Partner ID&quot;. Este no es el ID del que hablamos en este artículo y vídeo. En su lugar, el &quot;subdominio de socio&quot;, que a veces se denomina ID de socio, suele ser una versión del nombre de cliente y es el subdominio del servidor al que se envían los datos. Por ejemplo, si su empresa es &quot;Bob&#39;s Knobs&quot; (todos los controles de puerta, por supuesto, jaja), entonces es probable que su subdominio asociado sea &quot;bobsknobs&quot;, mientras que el &quot;PID&quot; sería algo más como &quot;12345&quot;. Normalmente no necesita conocer su PID, pero es importante conocer su subdominio para poder configurar la implementación de Audience Manager.

