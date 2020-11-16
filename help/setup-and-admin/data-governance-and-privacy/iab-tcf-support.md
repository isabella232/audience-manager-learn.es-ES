---
title: Compatibilidad con IAB TCF 2.0 en Audience Manager
description: Adobe le proporciona los medios para administrar y comunicar las opciones de privacidad de sus usuarios a través de la funcionalidad de inclusión y a través del complemento Audience Manager a la compatibilidad con Transparencia y Consentimiento de IAB Framework 2.0 (TCF 2.0). Este artículo funciona junto con la documentación para ayudarle a comprender el complemento Audience Manager a IAB TCF y cómo funciona junto con el objeto Opt-in de Adobe y su proveedor de gestión de consentimiento (CMP).
feature: data governance & privacy
topics: null
audience: implementer, architect
activity: implement
doc-type: technical video
team: Technical Marketing
thumbnail: 26434.jpg
kt: 5027
translation-type: tm+mt
source-git-commit: df8afb50ed3971dc47e6506d31a8222a7f488b25
workflow-type: tm+mt
source-wordcount: '1123'
ht-degree: 0%

---


# Compatibilidad con IAB TCF 2.0 en Audience Manager {#iab-tcf-support-in-audience-manager}

Adobe le proporciona los medios para administrar y comunicar las opciones de privacidad de sus usuarios a través de la funcionalidad de inclusión y a través del complemento Audience Manager a la compatibilidad con Transparencia y Consentimiento de IAB Framework 2.0 (TCF 2.0). Este artículo funciona junto con la documentación para ayudarle a comprender el complemento Audience Manager a IAB TCF y cómo funciona junto con el objeto Opt-in de Adobe y su proveedor de gestión de consentimiento (CMP). Para obtener más información sobre la IAB, visite su sitio Web en [https://www.iabeurope.eu/](https://www.iabeurope.eu/).

## Primer paso: Comprender la inclusión del ECID {#first-step-understand-ecid-s-opt-in}

Para comprender cómo trabajar con el TCF de IAB, primero debe comprender [!DNL Opt-in] la funcionalidad, que forma parte de la biblioteca del servicio de ID de Experience Cloud (ECID). Si no está familiarizado con el funcionamiento de Opt-in, consulte [este útil artículo](https://docs.adobe.com/content/help/en/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html) primero. También debe revisar la [documentación](https://docs.adobe.com/content/help/es-ES/id-service/using/implementation/opt-in-service/optin-overview.html)de participación. Una vez que haya pasado por esos recursos, vuelva a esta página y continúe.

## The Audience Manager Plug-In for IAB TCF {#the-audience-manager-plug-in-for-iab-tcf}

Ahora que tiene al menos una comprensión básica de cómo funciona el servicio de inclusión, el Audience Manager puede crear una capa en su [!DNL IAB Transparency and Consent Framework (TCF)] compatibilidad, que se realiza mediante un complemento en el objeto de inclusión.

El complemento Audience Manager para IAB TCF amplía la funcionalidad de la opción de inclusión y permite a los clientes AAM evaluar, cumplir y reenviar las opciones de privacidad de los usuarios a los socios intermedios de acuerdo con IAB TCF. Proporciona un estándar que los controladores de datos (es decir, usted como cliente de Adobe) y los proveedores (DMPs, DSP, SSPs, Servidores de publicidad, etc.) puede utilizarse para comprender el consentimiento en el entorno de consentimiento.

## Habilitación de TCF IAB {#enabling-iab-tcf}

Habilitar el complemento Audience Manager para IAB TCF es fácil si utiliza Adobe Experience Platform Launch, ya que es una casilla de verificación sencilla, como se muestra en el breve vídeo a continuación:

>[!VIDEO](https://video.tv.adobe.com/v/26433/?quality=12)

Como alternativa, si no utiliza Launch, puede `isIabContext=true` habilitarlo al crear una instancia del Visitante de Experience Cloud. Esto inicia el flujo TCF de IAB, es decir, agrega otro paso a la recopilación de consentimiento, usando el TCF de IAB para la consulta de la cadena TC de IAB y lo devuelve a Opt-in, que a su vez se comunica con las soluciones Experience Cloud.

## Cadena TC IAB {#iab-tcf-consent-string}

Uno de los estándares que la IAB ofrece es una &quot;cadena de consentimiento&quot; (también conocida como &quot;DaisyBit&quot;), que en realidad es dos listas juntas:

1. Propósitos: **¿Para qué** se concede el consentimiento?
1. Proveedores: **¿A quién** se da el consentimiento?

### Fines {#purposes}

Con IAB TCF 2.0, hay diez &quot;propósitos&quot; para reunir el consentimiento (qué pueden hacer los proveedores con los datos del visitante). Adobe Audience Manager no requiere los diez, sino que sólo requiere el consentimiento para los siguientes fines, además del consentimiento del proveedor:

* **Objetivo 1:** Almacenar y/o acceder a información en un dispositivo;
* **Objetivo 10:** Desarrollar y mejorar los productos;
* **Objetivo especial 1:** Garantizar la seguridad, evitar fraudes y depurar.

Esta es la primera parte de la cadena TC de IAB y se registra como 1 y 0, lo que determina si se aprueba o no ese propósito o actividad.

>[!NOTE]
>
>De acuerdo con las regulaciones de IAB, el objetivo especial 1 (garantizar la seguridad, evitar fraudes y depurar) siempre está aceptado y los usuarios no pueden oponerse a él.

### Proveedores {#vendors}

Otra parte de la cadena TC de la IAB es una larga lista de varios cientos de proveedores, de modo que los visitantes se pueden presentar con una lista de proveedores aplicables que tienen etiquetas en el sitio y pueden elegir qué proveedores utilizar. Los proveedores mantienen su punto en la lista. Por ejemplo, el número de proveedor de Adobe Audience Manager en esta lista es 565. Si ese número de la lista tiene un &quot;1&quot;, el Audience Manager puede realizar los objetivos aprobados desde la parte delantera de la lista. Si el punto de AAM tiene un &quot;0&quot;, no puede hacer nada con los datos.

**Para que el Audience Manager pueda proporcionar una interfaz de usuario para que los clientes puedan utilizar el TCF de IAB para elegir estos fines y proveedores, o para aprobar o desaprobar toda actividad, debe utilizar un socio de CMP registrado en TCF de IAB o crear uno que admita TCF de IAB y esté registrado en el TCF de IAB.**

## Inclusión: Traducción entre la IAB y las soluciones de Adobe {#opt-in-translating-between-iab-and-adobe-solutions}

Una de las ventajas de utilizar el TCF de la IAB es que los propósitos estándar mencionados anteriormente probablemente den al usuario final una idea más de lo que está aprobando que una lista de soluciones de Adobe. Es posible que los usuarios finales no sepan lo que significa &quot;aprobar&quot; a un Audience Manager [!DNL Target], pero &quot;Almacenar y/o acceder a información en un dispositivo&quot; o &quot;Desarrollar y mejorar productos&quot; es probablemente más fácil de entender y de aceptar.

Para que el Audience Manager pueda ser aprobado (es decir, para traducir los propósitos de la IAB para la participación en la votación AAM &quot;sí&quot;), los objetivos 1 y 10, enumerados anteriormente, deben recibir el consentimiento del usuario final. Si alguno de estos no está aprobado, o si un vendedor no está aprobado, no AAM ejecutar activaciones de píxeles ni establecer cookies. También es bueno saber que muchos clientes simplemente eligen proporcionar al usuario final una interfaz de usuario &quot;todo o nada&quot; que, por supuesto, permitiría o no el uso de Audience Manager (y las otras soluciones Experience Cloud).

Hay información buena en la [documentación](https://marketing.adobe.com/resources/help/en_US/aam/aam-iab-plugin.html) sobre cómo se aplica el flujo del complemento Audience Manager para IAB TCF a casos de uso tanto del publicador como del anunciante.

## IAB: Envío de consentimiento descendente {#iab-sending-consent-downstream}

Cuando se utiliza el complemento Audience Manager para IAB TCF, las opciones de consentimiento del usuario también se envían a las sincronizaciones de ID de nivel de plataforma (de terceros) para socios que están presentes en la Lista Global de Proveedores, de modo que el socio tenga la información de consentimiento del usuario y pueda actuar en consecuencia. Esta información se envía en dos variables:

* gdpr = 1
* gdpr_permission = cadena de consentimiento [codificado]

La advertencia es que si el usuario se encuentra en el contexto de IAB y no da su consentimiento (o da su consentimiento negativo), entonces el Audience Manager no recopila la cadena TC de IAB y, como tal, descarta las llamadas. Así que, en ese caso... no se pasa el consentimiento en sentido descendente.

## Demostración{#demo}

En el siguiente vídeo, vea cómo las cookies y las señalizaciones de ECID y las soluciones se ven afectadas por las selecciones de selección de usuarios de IAB.

>[!VIDEO](https://video.tv.adobe.com/v/26434/?quality=12)

Para obtener información más detallada sobre el complemento Audience Manager para IAB TCF 2.0, incluido cómo implementar y probar, casos de uso y flujo de trabajo, consulte la [documentación](https://docs.adobe.com/content/help/en/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html).
