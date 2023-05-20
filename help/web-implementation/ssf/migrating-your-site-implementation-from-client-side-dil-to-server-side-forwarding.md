---
title: Migre la implementación de Audience Manager del sitio del DIL del lado del cliente al reenvío del lado del servidor
description: Obtenga información sobre cómo migrar la implementación de Audience Manager AAM () del sitio desde el DIL del lado del cliente al reenvío del lado del servidor. AAM Este tutorial se aplica si tiene tanto como Adobe Analytics AAM, y si envía visitas desde la página a los usuarios mediante el uso del código de DIL (Data Integration Library), y también envía visitas desde la página a Adobe Analytics.
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: tutorial
team: Technical Marketing
kt: 1778
role: Developer, Data Engineer
level: Intermediate
exl-id: bcb968fb-4290-4f10-b1bb-e9f41f182115
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '2333'
ht-degree: 0%

---

# Migre la implementación de Audience Manager del sitio del DIL del lado del cliente al reenvío del lado del servidor {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

Este tutorial se aplica si tiene Adobe Audience Manager AAM () y Adobe Analytics AAM y si está enviando una visita de la página a un DIL de uso de la página para que la utilice en el envío de la visita de la página a la página de la aplicación de la aplicación de la página ( ). En este caso, puede usar la función de.[!DNL Data Integration Library]), y también enviar una visita de la página a Adobe Analytics. Dado que tiene ambas soluciones y que ambas forman parte de Adobe Experience Cloud, tiene la oportunidad de seguir las prácticas recomendadas de activar el reenvío del lado del servidor, lo que permite [!DNL Analytics] servidores de recopilación de datos para reenviar los datos de análisis del sitio en tiempo real al Audience Manager AAM, en lugar de hacer que el código del lado del cliente envíe una visita adicional de la página a la que se va a realizar el envío. Este tutorial le guiará por los pasos para realizar el cambio de la implementación de DIL del lado del cliente más antigua al método de reenvío del lado del servidor más reciente.

## Lado del cliente (DIL) frente a lado del servidor {#client-side-dil-vs-server-side}

Al comparar y contrastar estos dos métodos para introducir datos de Adobe Analytics AAM en los datos de los informes, puede resultar útil visualizar primero las diferencias en la siguiente imagen:

![de cliente a servidor](assets/client-side_vs_server-side_aam_implementation.png)

### Implementación del DIL del lado del cliente {#client-side-dil-implementation}

Si utiliza este método para introducir datos de Adobe Analytics AAM en el sistema de informes, tiene dos visitas provenientes de sus páginas web: Una que se dirige a [!DNL Analytics]AAM , y uno que va a (después de haber copiado el [!DNL Analytics] datos de la página web. [!UICONTROL Segments] AAM se devuelven de los usuarios a la página, donde se pueden utilizar para la personalización, entre otros. Se considera una implementación heredada y ya no se recomienda.

Además de no seguir las prácticas recomendadas, las desventajas de utilizar este método son las siguientes:

* Dos visitas procedentes de la página en lugar de una sola
* AAM el reenvío del lado del servidor es necesario para el uso compartido en tiempo real de audiencias de la a [!DNL Analytics], por lo que las implementaciones del lado del cliente no permiten esta función (y potencialmente otras funciones en el futuro)

AAM Se recomienda pasar a un método de reenvío del lado del servidor para la implementación de la.

### Implementación del reenvío del lado del servidor {#server-side-forwarding-implementation}

Como se muestra en la imagen anterior, una visita procede de la página web a Adobe Analytics. [!DNL Analytics] AAM AAM a continuación, reenvía esos datos a los visitantes en tiempo real, y los visitantes se evalúan en rasgos de la y [!UICONTROL segments], como si la visita hubiera provenido directamente de la página.

[!UICONTROL Segments] se devuelven en la misma visita en tiempo real de nuevo a [!DNL Analytics], que reenvía la respuesta a la página web para su personalización, etc.

El paso al reenvío del lado del servidor no presenta inconvenientes temporales. El Adobe recomienda encarecidamente que todas las personas que tengan ambos Audience Manager y [!DNL Analytics] utiliza este método de implementación.

## Tiene dos tareas principales {#you-have-two-main-tasks}

Hay bastante información en esta página, y todo es importante, por supuesto. Sin embargo, sí **todo se reduce a dos cosas principales que debe hacer**:

1. Cambie el código del código del DIL del lado del cliente al código de reenvío del lado del servidor
1. Gire el interruptor en la [!DNL Analytics] [!DNL Admin Console] para iniciar el reenvío real de datos (por [!UICONTROL report suite])

Si omite cualquiera de estas tareas, el reenvío del lado del servidor no funcionará correctamente. Se han agregado pasos y datos adicionales a este documento para ayudarle a realizar estos dos pasos correctamente para su configuración.

## Opciones de implementación {#implementation-options}

A medida que pasa del lado del cliente al reenvío del lado del servidor, una de las tareas que tendrá es cambiar el código al nuevo código de reenvío del lado del servidor. Esto se realiza mediante una de las siguientes opciones:

* Etiquetas de Adobe Experience Platform: opción de implementación recomendada por el Adobe para propiedades web. Verá que esto es una tarea fácil, ya que las etiquetas de Platform han hecho todo el trabajo duro por usted.
* En la página: también puede colocar el nuevo código SSF directamente en `doPlugins` función dentro de su `appMeasurement.js` , si no está utilizando (todavía) Adobe Launch
* Otros administradores de etiquetas: Se pueden tratar igual que la opción anterior (En la página), ya que aún colocará el código SSF en `doPlugins`, siempre que el otro administrador de etiquetas almacene el [!DNL AppMeasurement] código

Observaremos cada uno de estos elementos a continuación en la _Actualización del código_ sección.

## Pasos de implementación {#implementation-steps}

Los siguientes pasos describen la implementación.

### Paso 0: Requisito previo: Servicio de ID de Experience Cloud (ECID) {#step-prerequisite-experience-cloud-id-service-ecid}

El requisito previo principal para pasar al reenvío del lado del servidor es tener implementado el servicio de ID de Experience Cloud. Esto se hace más fácilmente si utiliza Experience Platform Launch, en cuyo caso solo tiene que instalar la extensión ECID y hará el resto.

Si utiliza un sistema de administración de etiquetas que no sea de Adobe o no lo utiliza, implemente un ECID para ejecutar **antes** cualquier otra solución de Adobe. Consulte la [Documentación de ECID](https://experienceleague.adobe.com/docs/id-service/using/home.html) para obtener más información. El único otro requisito previo es con respecto a las versiones del código, por lo que al aplicar las versiones más recientes del código en los siguientes pasos, estará bien.

>[!NOTE]
>
>Lea todo este documento antes de implementar. La sección &quot;Tiempo&quot; a continuación contiene información importante sobre *cuando* debe implementar cada parte, incluido ECID (si aún no se ha implementado).

### Paso 1: Registrar las opciones utilizadas actualmente del código de DIL {#step-record-currently-used-options-from-dil-code}

A medida que se prepara para pasar del código de DIL del lado del cliente al reenvío del lado del servidor, el primer paso es identificar todo lo que está haciendo con el código de DIL AAM, incluida la configuración personalizada y los datos enviados a los. Los aspectos a tener en cuenta incluyen:

* Normal [!DNL Analytics] variables, usando el `siteCatalyst.init` Módulo de DIL: no tiene que preocuparse por este, ya que su trabajo consiste únicamente en enviar la información normal [!DNL Analytics] y esto sucede simplemente porque tiene habilitado el reenvío del lado del servidor.
* Subdominio de socio: en `DIL.create` función, tome nota de la `partner` parámetro. Esto se conoce como su &quot;subdominio de socio&quot;, o a veces &quot;ID de socio&quot;, y será necesario cuando coloque el nuevo código de reenvío del lado del servidor.
* [!DNL Visitor Service Namespace] - También conocido como su &quot;[!DNL Org ID]&quot; o &quot;[!DNL IMS Org ID],&quot; también lo necesitará al configurar el nuevo código de reenvío del lado del servidor. Tome nota de ello.
* containerNSID, uuidCookie y otras opciones avanzadas: tome nota de cualquier opción avanzada adicional que esté utilizando para poder establecerla también en el código de reenvío del lado del servidor.
* AAM Variables de página adicionales: si se envían otras variables a desde la página (además de las variables normales), se debe enviar a la variable de página desde la página de [!DNL Analytics] variables gestionadas por siteCatalyst.init), deberá tomar nota de ellas para que se puedan enviar mediante el reenvío del lado del servidor (alerta de spoiler: a través de [!DNL contextData] variables).

### Paso 2: Actualizar el código {#step-updating-the-code}

Entrada [Opciones de implementación](#implementation-options) (arriba), se ofrecen varias opciones con respecto a cómo y dónde se implementa el reenvío del lado del servidor. Para que esta sección sea efectiva, necesitamos dividirla en estas secciones (con dos de ellas combinadas). Vaya al método de esta sección que mejor describa sus necesidades.

#### Etiquetas Adobe Experience Platform {#launch-by-adobe}

Vea el siguiente vídeo para obtener más información sobre cómo mover las opciones de implementación del código del DIL del lado del cliente al reenvío del lado del servidor en Experience Platform Launch.

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### Administrador de etiquetas &quot;En la página&quot; o que no sea de Adobe {#on-the-page-or-non-adobe-tag-manager}

Vea el siguiente vídeo para obtener más información sobre cómo mover las opciones de implementación del código DIL del lado del cliente al reenvío del lado del servidor en [!DNL AppMeasurement] código, residiendo en un archivo o en un sistema de administración de etiquetas que no sea de Adobe.

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### Paso 3: Activación del reenvío (por [!UICONTROL Report Suite]) {#step-enabling-the-forwarding-per-report-suite}

Hasta ahora, en este tutorial hemos dedicado todo nuestro tiempo a cambiar el código del DIL del lado del cliente al reenvío del lado del servidor. Eso está bien, porque es la parte más difícil. Esta sección, aunque verá que es muy fácil, es tan importante como actualizar el código. En este vídeo, verá cómo voltear el conmutador que habilita el reenvío real de datos de Analytics a Audience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**NOTA:** Como se indica en el vídeo, recuerde que la activación del reenvío tardará hasta 4 horas en implementarse completamente en el back-end del Experience Cloud.

## Programación {#timing}

Como recordatorio, existen dos tareas principales para pasar de un DIL del lado del cliente al reenvío del lado del servidor:

1. Actualización del código
1. Girando el interruptor en la [!DNL Analytics] [!DNL Admin Console]

Pero la pregunta es, ¿cuál haces primero? ¿Importa? Vale, lo siento, fueron dos preguntas. Pero las respuestas son... depende, y sí, depende *lata* materia. ¿Cómo es eso de vago? Vamos a descomponerlo... Pero primero una pregunta adicional que puede surgir si usted es una organización grande con numerosos sitios: ¿Tengo que hacer todo a la vez? Ese es un poco más fácil. No. Puedes hacerlo pieza por pieza.

### Un poco más profundo {#a-little-deeper-dive}

La razón por la que el tiempo y el orden importan es debido a la forma en que el reenvío _de verdad_ trabajos, que pueden resumirse en los siguientes aspectos técnicos:

* Si tiene implementado el Servicio de ID de Experience Cloud (ECID) y el conmutador en la [!DNL Analytics] [!DNL Admin Console] (&quot;el interruptor&quot;) está activado, los datos SE REENVIARÁN desde [!DNL Analytics] AAM para, incluso aunque aún no haya actualizado el código.
* Si no tiene implementado ECID, los datos no se reenviarán, aunque tenga el conmutador encendido y el código de reenvío del lado del servidor.
* El código de reenvío del lado del servidor (ya sea en etiquetas de Platform o en la página) realmente gestiona la respuesta y es necesario para completar la migración.
* Recuerde que el conmutador de reenvío del lado del servidor está habilitado por [!UICONTROL report suite], pero que el código lo gestiona la propiedad en etiquetas de Platform o el [!DNL AppMeasurement] si no utiliza etiquetas de Platform.

### Prácticas recomendadas {#best-practices}

En función de estos detalles técnicos, estas son las recomendaciones para el momento de qué hacer y cuándo:

#### Si TODAVÍA NO ha implementado ECID {#if-you-do-not-have-ecid-yet-implemented}

1. Gire el interruptor hacia dentro [!DNL Analytics] para cada [!UICONTROL report suite] que habilitará para el reenvío del lado del servidor.

   1. El reenvío aún no comienza porque no tiene ECID.

1. Por sitio, actualice el código del DIL del lado del cliente al reenvío del lado del servidor (esto podría estar en las etiquetas de Platform ) o en la página, como se describe en otra sección anterior).

   1. El reenvío ahora fluye (como ha agregado a ECID), y también debe recibir una respuesta JSON adecuada a su [!DNL Analytics] (consulte la sección Validación y solución de problemas más abajo para obtener más información).

#### Si tiene ECID implementado {#if-you-do-have-ecid-implemented}

1. Prepare y planifique para que esté listo para actualizar su código de DIL a reenvío del lado del servidor PER [!UICONTROL report suite] que habilitará para el reenvío del lado del servidor:

   1. Gire el interruptor hacia dentro [!DNL Analytics] para habilitar el reenvío del lado del servidor.

      1. El reenvío comenzará porque tiene ECID habilitado.
   1. Actualice lo antes posible el código del DIL del lado del cliente al reenvío de un solo lado (puede estar en las etiquetas de Platform o en la página, como se explica en otra sección anterior).

      1. Debe recibir una respuesta JSON adecuada a su [!DNL Analytics] baliza (consulte la [Validación y solución de problemas](#validation-and-troubleshooting) para obtener más información).


>[!NOTE]
>
>AAM Es importante realizar estos dos pasos lo más cerca posible entre sí, ya que entre los pasos 1 y 2 anteriores, se producirá una duplicación de datos en la que se va a realizar una acción de forma más rápida y eficaz. En este caso, es posible que tenga que realizar una operación de. En otras palabras, el reenvío de un solo lado habrá comenzado a enviar datos desde [!DNL Analytics] AAM a, y ya que el código de DIL AAM sigue en la página, también se producirá una visita que irá directamente de la página a la página, lo que hará que los datos se dupliquen. En cuanto actualice el código de DIL a reenvío del lado del servidor, se aliviará.

>[!NOTE]
>
>Si prefiere tener una pequeña discrepancia en los datos en lugar de una pequeña duplicación de datos, puede cambiar el orden de los pasos 1 y 2 anteriores. Al mover el código del DIL AAM al reenvío del lado del servidor, se detendría el flujo de datos en hasta que se pudiera accionar el conmutador para activar el reenvío del lado del servidor en el caso de los usuarios de la red de distribución de datos de la red. [!UICONTROL report suite]. Normalmente, los clientes prefieren duplicar ligeramente los datos en lugar de perder la oportunidad de clasificar a los visitantes en características y [!UICONTROL segments].

#### Tiempo de migración cuando tiene muchos sitios y [!UICONTROL report suites] {#migration-timing-when-you-have-many-sites-and-report-suites}

Este tema se trata brevemente en secciones anteriores, ya que la estrategia principal se puede resumir de la siguiente manera:

Migrar un sitio/[!UICONTROL report suite] (o grupo de sitios/[!UICONTROL report suites]) a la vez.

Sin embargo, esto puede resultar un poco complicado en función de algunos escenarios posibles:

* Tiene un sitio que contiene varios [!UICONTROL report suites]
* Tiene un [!UICONTROL report suite] que incluye varios sitios (como una [!UICONTROL report suite])
* Utilice una propiedad de etiquetas de Platform para cubrir varios sitios
* Tiene diferentes equipos de desarrollo para diferentes sitios

Debido a estos artículos, puede ser un poco complicado. Las mejores cosas que puedo sugerir son:

* Dedique algún tiempo a crear una estrategia para migrar al reenvío del lado del servidor, en función de los aspectos explicados anteriormente
* Se basa en el hecho de que una sola propiedad en etiquetas de Platform (o una sola [!DNL AppMeasurement] file) normalmente se asigna a 1 o 2 distintos [!UICONTROL report suites], probablemente podrá realizar un plan que funcione en estos grupos distintos uno por uno, actualizando su empresa al reenvío del lado del servidor
* Si está trabajando con la asesoría de Adobe, hable con ellos acerca de su plan de migración para que puedan ayudarle según sea necesario

## Validación y solución de problemas {#validation-and-troubleshooting}

La manera principal de comprobar que el reenvío del lado del servidor funciona es consultar la respuesta a cualquiera de sus visitas de Adobe Analytics que llegan desde la aplicación.

Si no utiliza el reenvío de datos del lado del servidor desde [!DNL Analytics] al Audience Manager, entonces no hay respuesta a la [!DNL Analytics] baliza (además de un píxel 2x2). Sin embargo, si utiliza el reenvío del lado del servidor, puede comprobar ciertos elementos en la [!DNL Analytics] solicitud y respuesta de que le informará de lo siguiente [!DNL Analytics] se comunica correctamente con el Audience Manager, reenvía la visita y obtiene una respuesta.

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

>[!WARNING]
>
>Tenga en cuenta los falsos &quot;Success&quot;. Si hay una respuesta y todo parece funcionar, asegúrese de que tiene el `stuff` en la respuesta. Si no lo hace, puede que vea un mensaje que dice `"status":"SUCCESS"`. Por muy loco que suene, esto es en realidad la prueba de que NO está funcionando correctamente.
>
>Si lo ve, significa que ha completado la actualización del código en las etiquetas de Platform o [!DNL AppMeasurement], pero que el reenvío en el [!DNL Analytics] [!DNL Admin Console] aún no ha finalizado. En este caso, debe comprobar que ha habilitado el reenvío del lado del servidor en la variable [!DNL Analytics] [!DNL Admin Console] para su [!UICONTROL report suite]. Si lo ha hecho y aún no han pasado cuatro horas, tenga paciencia, ya que puede tardar tanto en realizar todos los cambios necesarios en el servidor.


![falso éxito](assets/falsesuccess.png)

Para obtener más información acerca del reenvío del lado del servidor, consulte la [documentación](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html).
