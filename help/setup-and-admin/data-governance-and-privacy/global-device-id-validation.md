---
title: Validación global de ID de dispositivo
description: Los identificadores de publicidad de dispositivo (es decir, iDFA, GAID, Roku ID) tienen estándares de formato que deben cumplirse para poder utilizarlos en el ecosistema de publicidad digital. Hoy en día, los clientes y socios pueden cargar ID a nuestros orígenes de datos globales en cualquier formato sin que se les notifique si el ID tiene el formato correcto. Esta función introducirá la validación de los ID de dispositivo enviados a las fuentes de datos globales para un formato adecuado y proporcionará mensajes de error cuando los ID tengan un formato incorrecto. Admitiremos la validación de los ID de iDFA, Google Advertising y Roku en el momento del lanzamiento.
feature: data governance & privacy
topics: mobile
audience: implementer, developer, architect
activity: implement
doc-type: article
team: Technical Marketing
kt: 2977
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 1%

---


# Validación global de ID de dispositivo {#global-device-id-validation}

Los identificadores de publicidad de dispositivo (es decir, iDFA, GAID, Roku ID) tienen estándares de formato que deben cumplirse para poder utilizarlos en el ecosistema de publicidad digital. Hoy en día, los clientes y socios pueden cargar ID a nuestro Global [!UICONTROL data sources] en cualquier formato sin que se les notifique si el ID tiene el formato correcto. Esta función introducirá la validación de los ID de dispositivo enviados a Global [!UICONTROL data sources] para un formato adecuado y proporcionará mensajes de error cuando los ID tengan un formato incorrecto. Admitiremos la validación para [!DNL iDFA]y [!DNL Google Advertising] en [!DNL Roku IDs] el lanzamiento.

## Información general sobre los estándares de formato {#overview-of-format-standards}

Los siguientes son los grupos globales de ID de publicidad de dispositivo que actualmente son reconocidos y admitidos por AAM. Se implementan como compartidos [!UICONTROL Data Sources] que pueden utilizar cualquier cliente o socio de datos que trabaje con datos vinculados a los usuarios de estas plataformas.

<table>
  <tr>
   <td>Plataforma </td>
   <td>ID de fuente de datos AAM </td>
   <td>Formato de ID </td>
   <td>AAM PID </td>
   <td>Notas </td>
  </tr>
  <tr>
   <td>Google Android (GAID)</td>
   <td>20914</td>
   <td>Números hexadecimales, generalmente presentados como 8-4-4-4-12<em>ejemplo, 97987bca-ae59-4c7d-94ba-ee4f19ab8c21<br/> </em> </td>
   <td>1352</td>
   <td>Este ID debe recopilarse en una referencia de formulario sin formato, sin hash o sin modificar: <a href="https://play.google.com/about/monetization-ads/ads/ad-id/">https://play.google.com/about/monetization-ads/ads/ad-id/</a></td>
  </tr>
  <tr>
   <td>Apple iOS (IDFA)</td>
   <td>20915</td>
   <td>Números hexadecimales, generalmente presentados como <em>ejemplo 8-4-4-4-12, 6D92078A-8246-4BA4-AE5B-76104861E7DC<br /> </em> </td>
   <td>3560</td>
   <td>Este ID debe recopilarse en una referencia de formulario sin formato, sin hash o sin modificar: <a href="https://support.apple.com/en-us/HT205223">https://support.apple.com/en-us/HT205223</a></td>
  </tr>
  <tr>
   <td>Roku (RIDA)</td>
   <td>121963</td>
   <td>Números hexadecimales, generalmente presentados como <em>ejemplo 8-4-4-4-12,</em> <em>fcb2a29c-315a-5e6b-bcfd-d889ba19aada</em></td>
   <td>11536</td>
   <td>Este ID debe recopilarse en una referencia de formulario sin formato, sin hash o sin modificar: <a href="https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework">https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework</a> </td>
  </tr>
  <tr>
   <td>ID de publicidad de Microsoft (MAID)</td>
   <td>389146</td>
   <td>Cadena numérica alfa</td>
   <td>14593</td>
   <td>Este ID debe recopilarse en una referencia de formulario sin formato/sin hash/sin modificar - <a href="https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid">https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.</a><br/><a href="https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx">advertisingidhttps://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx</a></td>
  </tr>
  <tr>
   <td>Samsung DUID</td>
   <td>404660</td>
   <td>Ejemplo de cadena numérica alfa, 7XCBNROQJQPYW</td>
   <td>15950</td>
   <td>Este ID debe recopilarse en una referencia de formulario sin formato, sin hash o sin modificar: <a href="https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api">https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api</a> </td>
  </tr>
</table>

## Configuración de un identificador de publicidad en la aplicación {#setting-an-advertising-identifier-in-the-app}

La configuración del ID del anunciante en la aplicación es realmente un proceso de dos pasos: primero recuperar el ID del anunciante y, después, enviarlo al Experience Cloud. A continuación encontrará vínculos para realizar estos pasos.

1. Recuperar el ID
   1. [!DNL Apple] la información sobre el [!DNL advertising ID] se puede encontrar [AQUÍ](https://developer.apple.com/documentation/adsupport/asidentifiermanager).
   1. Encontrará información sobre cómo configurar el [!DNL advertiser ID] para [!DNL Android] desarrolladores [AQUÍ](http://www.androiddocs.com/google/play-services/id.html).
1. Envíelo al Experience Cloud mediante el [!DNL setAdvertisingIdentifier] método del SDK
   1. La información de uso `setAdvertisingIdentifier` se encuentra en la [documentación](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core/identity/identity-api-reference#set-an-advertising-identifier) tanto para [!DNL iOS] como para [!DNL Android].

`// iOS (Swift) example for using setAdvertisingIdentifier:`
`ACPCore.setAdvertisingIdentifier([AdvertisingId]) // ...where [AdvertisingId] is replaced by the actual advertising ID`

## Mensajes de error de DCS para ID incorrectos  {#dcs-error-messaging-for-incorrect-ids}

Cuando se envía un ID de dispositivo global incorrecto (IDFA, GAID, etc.) en tiempo real al Audience Manager, se devuelve un código de error en la visita. A continuación se muestra un ejemplo de error devuelto porque el ID se envía como [!DNL Apple IDFA], que solo debe contener letras mayúsculas y minúsculas, y sin embargo hay una &#39;x&#39; minúscula en el ID.

![imagen de error](assets/image_4_.png)

Consulte la [documentación](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-api-reference/dcs-error-codes.html?lang=en#api-and-sdk-code) para la lista de códigos de error.

## Incorporación de ID de dispositivos globales {#onboarding-global-device-ids}

Además del envío en tiempo real de ID de dispositivos globales, también puede &quot;[!DNL onboard]&quot; (cargar) datos con los ID. Este proceso es el mismo que cuando está incorporando datos con sus ID de cliente (generalmente a través de pares clave/valor), pero simplemente usaría los ID de fuente de datos adecuados, de modo que los datos se asignen a la ID del dispositivo global. La documentación sobre el proceso de integración se encuentra en la [documentación](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/sending-audience-data/batch-data-transfer-process/batch-data-transfer-overview.html?lang=en#implementation-integration-guides). Sólo recuerde utilizar el [!UICONTROL data source] ID global, según la plataforma que utilice.

Si se envían ID de dispositivos globales incorrectos mediante el proceso de integración, los errores se mostrarán en la [[!DNL Onboarding Status Report]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reporting/onboarding-status-report.html?lang=en#reporting).

A continuación se muestra una muestra de un error que se produciría en ese informe:

![imagen de error](assets/image_5_.png)