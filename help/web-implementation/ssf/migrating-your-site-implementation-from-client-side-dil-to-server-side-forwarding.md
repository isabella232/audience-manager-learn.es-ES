---
title: Migración de la implementación de AAM del sitio desde el DIL del cliente al reenvío del lado del servidor
description: Este tutorial se aplica a usted si tiene Adobe Audience Manager (AAM) y Adobe Analytics, y actualmente está enviando una visita desde la página a AAM mediante el código "DIL" (Data Integration Library) y también enviando una visita desde la página a Adobe Analytics. Dado que dispone de ambas soluciones y que ambas forman parte del Adobe Experience Cloud, tiene la oportunidad de seguir la práctica recomendada de activar "Reenvío del lado del servidor (SSF)", que permite a los servidores de recopilación de datos de Analytics reenviar datos de análisis del sitio en tiempo real al Audience Manager, en lugar de hacer que el código del cliente envíe una visita adicional de la página a AAM. Este tutorial le guiará por los pasos para realizar el cambio de la implementación anterior de "DIL del lado del cliente" al nuevo método de "reenvío del lado del servidor".
product: audience manager, analytics
feature: integration with analytics
topics: null
audience: implementer
activity: implement
doc-type: tutorial
team: Technical Marketing
kt: 1778
translation-type: tm+mt
source-git-commit: 133279f589bd58aef36a980c2b7248ae00fd9496
workflow-type: tm+mt
source-wordcount: '2319'
ht-degree: 0%

---


# Migración de la implementación de AAM del sitio del [!DNL Client-Side] DIL al [!DNL Server-Side Forwarding] {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

Este tutorial se aplica a usted si tiene Adobe Audience Manager (AAM) y Adobe Analytics, y actualmente está enviando una visita desde la página a AAM mediante el código &quot;DIL&quot; ([!DNL Data Integration Library]) y también enviando una visita desde la página a Adobe Analytics. Dado que tiene ambas soluciones y que ambas forman parte del Adobe Experience Cloud, tiene la oportunidad de seguir la práctica recomendada de activar &quot;[!DNL Server-Side Forwarding] (SSF)&quot;, que permite a los servidores de recopilación de datos reenviar los datos de análisis del sitio en tiempo real al Audience Manager, en lugar de hacer que el [!DNL Analytics] [!DNL client-side] código envíe una visita adicional de la página a AAM. Este tutorial le guiará por los pasos para pasar de la implementación &quot;[!DNL Client-Side DIL]&quot; anterior al método &quot;[!DNL Server-Side forwarding]&quot; más reciente.

## [!DNL Client-Side] (DIL) vs. [!DNL Server-Side] {#client-side-dil-vs-server-side}

Al comparar y contrastar estos dos métodos de conversión de datos de Adobe Analytics en AAM, podría resultar útil primero visualizar las diferencias en la siguiente imagen:

![cliente a servidor](assets/client-side_vs_server-side_aam_implementation.png)

### [!DNL Client-side] Implementación de DIL {#client-side-dil-implementation}

Si utiliza este método para AAM los datos de Adobe Analytics, significa que tiene dos visitas provenientes de las páginas Web: Uno va a [!DNL Analytics]y otro a AAM (después de haber copiado los [!DNL Analytics] datos en la página Web. [!UICONTROL Segments] se devuelven de AAM a la página, donde se pueden utilizar para la personalización, etc. Se considera una implementación &quot;heredada&quot; y ya no se recomienda.

Más allá del hecho de que esto no sigue las optimizaciones, las desventajas de utilizar este método incluyen:

* Dos visitas provenientes de la página en lugar de una sola
* [!UICONTROL Server-Side Forwarding] es necesario para compartir en tiempo real las audiencias de AAM a [!DNL Analytics], por lo que [!DNL Client-side] las implementaciones no permiten esta función (y posiblemente otras funciones en el futuro)

Se recomienda pasar a un [!UICONTROL Server-Side Forwarding] método de implementación AAM.

### [!UICONTROL Server-Side Forwarding]Implementación{#server-side-forwarding-implementation}

Como se muestra en la imagen anterior, una visita llega de la página Web a Adobe Analytics. [!DNL Analytics] luego reenvía esos datos a AAM en tiempo real, y los visitantes se evalúan en AAM [!UICONTROL traits] y [!UICONTROL segments], como si la visita hubiera venido directamente de la página.

[!UICONTROL Segments] se devuelven en la misma visita en tiempo real a [!DNL Analytics], que envía la respuesta a la página Web para su personalización, etc.

No hay una desventaja de tiempo para pasar al reenvío del lado del servidor. Recomendamos encarecidamente que cualquier usuario que tenga Audience Manager y [!DNL Analytics] utilice este método de implementación.

## Tiene DOS Tareas Principales {#you-have-two-main-tasks}

Hay bastante información en esta página, y todo es importante, por supuesto. Sin embargo, **todo se reduce a dos cosas principales que hay que hacer**:

1. Cambiar el código de [!DNL Client-Side] código DIL a [!UICONTROL Server-Side Forwarding] código
1. Girar el conmutador en el [!DNL Analytics] inicio [!DNL Admin Console] para el reenvío real de datos (por [!UICONTROL report suite])

Si omite cualquiera de estos dos, SSF no funcionará correctamente. Se han agregado pasos y datos adicionales a este documento para ayudarle a realizar estos dos pasos correctamente en su configuración.

## Opciones de implementación {#implementation-options}

A medida que se mueve de [!DNL client-side] a [!DNL server-side], una de las tareas que tendrá es cambiar el código al nuevo [!UICONTROL Server-Side Forwarding] código. Esto se realiza con una de las siguientes opciones:

* Adobe Experience Platform Launch: nuestra opción de implementación recomendada para propiedades web. Verán que esta es una tarea muy fácil, como [!DNL Launch] ha hecho todo lo duro por ustedes.
* En la página: también puede colocar el nuevo código SSF directamente en la `doPlugins` función dentro del [!DNL appMeasurement.js] archivo, si todavía no está utilizando Adobe Launch
* Otros administradores de etiquetas: se pueden tratar igual que la opción anterior (En la página), ya que se seguirá colocando el código SSF en `doPlugins`, siempre que el otro administrador de etiquetas almacene el [!DNL AppMeasurement]

Veremos cada una de estas opciones en la sección Actualización del código.

## Pasos de la implementación {#implementation-steps}

### Paso 0: Requisito previo: Servicio de ID de Experience Cloud (ECID) {#step-prerequisite-experience-cloud-id-service-ecid}

El requisito previo principal para pasar a [!UICONTROL Server-Side Forwarding] es la implementación del servicio de ID de Experience Cloud. Esto se hace más fácilmente si utiliza Experience Platform Launch, en cuyo caso simplemente instala la extensión ECID y hará el resto.

Si está utilizando un sistema de administración de etiquetas que no es de Adobe, o ningún sistema de administración de etiquetas, implemente ECID para ejecutarse **antes** de cualquier otra solución de Adobe. Consulte la documentación [del](https://marketing.adobe.com/resources/help/es_ES/mcvid/) ECID para obtener más información. El único otro requisito previo es con respecto a las versiones de código, por lo que, como simplemente aplica las versiones más recientes del código en los siguientes pasos, estará bien.

>[!NOTE]
>
>Lea todo este documento antes de la implementación. La sección &quot;Temporización&quot; de abajo tiene información importante sobre *cuándo* debe implementar cada pieza, incluyendo ECID (si aún no se ha implementado).

### Paso 1: Registrar opciones usadas actualmente desde el código DIL {#step-record-currently-used-options-from-dil-code}

A medida que esté listo para pasar del código de [!DNL Client-Side] DIL a [!UICONTROL Server-Side Forwarding], el primer paso es identificar todo lo que está haciendo con el código de DIL, incluyendo la configuración personalizada y los datos enviados a AAM. Entre las cosas que hay que notar y considerar se incluyen:

* Variables normales [!DNL Analytics] , usando el módulo [!DNL siteCatalyst.init] DIL: no tendrá que preocuparse por esta, ya que su trabajo es simplemente enviar las [!DNL Analytics] variables normales, y eso sucederá simplemente por tener SSF habilitado.
* Subdominio de socio: en la función DIL.create, anote el `partner` parámetro. Esto se conoce como &quot;subdominio de socio&quot; o a veces &quot;ID de socio&quot; y será necesario cuando coloque el nuevo código SSF.
* [!DNL Visitor Service Namespace] - También se le conoce como &quot;[!DNL Org ID]&quot; o &quot;[!DNL IMS Org ID]&quot;, lo necesitará también cuando configure el nuevo código SSF. Anote esto.
* containerNSID, uuidCookie y otras opciones avanzadas: tome nota de las opciones avanzadas adicionales que esté utilizando para poder configurarlas también en el código SSF.
* Variables de página adicionales: si se envían otras variables a AAM desde la página (además de las variables normales que [!DNL Analytics] administra SiteCatalyst.init), deberá tener en cuenta dichas variables para que se puedan enviar a través de SSF (alerta de spoiler: mediante [!DNL contextData] variables).

### Paso 2: Actualización del código {#step-updating-the-code}

En la sección anterior titulada &quot;Opciones de implementación&quot;, se proporcionan varias opciones con respecto a cómo y dónde se implementa [!UICONTROL Server-Side Forwarding]. Para que esta sección sea efectiva, necesitamos dividirla en estas secciones (con dos de ellas combinadas). Vaya al método de esta sección que describe mejor sus necesidades.

#### Adobe Experience Platform Launch {#launch-by-adobe}

Vea el siguiente vídeo para obtener información sobre cómo mover las opciones de implementación del código de [!DNL Client-Side] DIL a [!UICONTROL Server-Side Forwarding] en Experience Platform Launch.

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### &quot;En la página&quot; o no Adobe Tag Manager {#on-the-page-or-non-adobe-tag-manager}

Vea el siguiente vídeo para obtener información sobre cómo mover las opciones de implementación del código de [!DNL Client-Side] DIL al código [!UICONTROL Server-Side Forwarding] en [!DNL AppMeasurement] código, ya sea en un archivo o en un sistema de administración de etiquetas que no sea de Adobe.

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### Paso 3: Habilitación del reenvío (por [!UICONTROL Report Suite]) {#step-enabling-the-forwarding-per-report-suite}

Hasta ahora en este tutorial hemos dedicado todo nuestro tiempo a cambiar el código de [!DNL Client-Side DIL] código a [!UICONTROL Server-Side Forwarding]. Está bien, porque es la parte más difícil. Esta sección, aunque verá que es muy fácil, es tan importante como actualizar el código. En este vídeo, verá cómo voltear el conmutador que habilita el reenvío real de datos de Analytics a Audience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**NOTA:** Como se indica en el vídeo, recuerde que la habilitación del reenvío tardará hasta 4 horas en implementarse completamente en el servidor del Experience Cloud.

## Temporización {#timing}

Como recordatorio, existen dos tareas principales para pasar de [!DNL Client-Side DIL] a [!UICONTROL Server-Side Forwarding]:

1. Actualización del código
1. Volteando el conmutador en el [!DNL Analytics] [!DNL Admin Console]

Pero la pregunta es, ¿cuál haces primero? ¿Importa? Bien, lo siento, fueron dos preguntas. Pero las respuestas son... depende, y sí, *puede* importar. ¿Cómo es esto para vago? Vamos a desglosarlo. Pero primero una pregunta adicional que puede surgir si usted es una gran organización con muchos sitios: ¿Tengo que hacer todo a la vez? Ese es un poco más fácil. No. Puedes hacerlo pieza a pieza... como que... :)

### Un poco más profundo {#a-little-deeper-dive}

La razón por la que el tiempo y el orden son importantes es por la manera en que el reenvío *realmente funciona, que se puede resumir en los siguientes pocos hechos técnicos:

* Si ha implementado el servicio de ID de Experience Cloud (ECID) y el conmutador del [!DNL Analytics] (&quot;el conmutador&quot;) está activado, los datos se reenviarán de [!DNL Admin Console] [!DNL Analytics] a AAM, aunque aún no haya actualizado el código.
* Si no se ha implementado ECID, los datos no se reenviarán, incluso si tiene el conmutador activado, y tiene el código SSF.
* El código SSF (ya sea en la página [!DNL Launch] o en ella) realmente gestiona la respuesta y, por supuesto, es necesario para completar la migración.
* Recuerde que el conmutador SSF está habilitado por [!UICONTROL Report Suite], pero que el código se gestiona mediante la propiedad en [!DNL Launch]o por [!DNL AppMeasurement] archivo si no utiliza [!DNL Launch]

### Prácticas recomendadas {#best-practices}

Basándose en estos detalles técnicos, aquí están las recomendaciones para el momento de &quot;qué hacer cuando&quot;:

#### Si NO tiene ECID implementado aún {#if-you-do-not-have-ecid-yet-implemented}

1. Encienda el interruptor [!DNL Analytics] para cada uno de los [!UICONTROL report suite] que habilite para SSF

   1. El reenvío aún no inicio porque no tiene ECID

1. Por sitio, actualice el código de [!DNL Client-Side DIL] a SSF (esto podría estar en [!DNL Launch] o en la página, como se explica en otra sección anterior)

   1. El reenvío fluirá ahora (como ha agregado ECID) y también debe recibir una respuesta JSON adecuada a su [!DNL Analytics] señalización (consulte la sección Validación y resolución de problemas para obtener más detalles)

#### Si ha implementado ECID {#if-you-do-have-ecid-implemented}

1. Prepárese y planifique para que esté preparado para actualizar el código de DIL a SSF PER [!UICONTROL report suite] , que habilitará para SSF:

   1. Encienda el interruptor [!DNL Analytics] para activar el SSF

      1. Reenviar inicio WILL porque tiene habilitado ECID
   1. Tan pronto como sea posible, actualice el código de [!DNL Client-Side DIL] a SSF (esto podría estar en [!DNL Launch] o en la página, como se explica en otra sección anterior)

      1. Debe recibir una respuesta JSON adecuada a su [!DNL Analytics] señalización (consulte la sección Validación y resolución de problemas más abajo para obtener más detalles)


**NOTA 1:** Es importante realizar estos dos pasos lo más cerca posible entre sí, ya que entre los pasos 1 y 2 anteriores, se producirá una duplicación de los datos que se irán a AAM. En otras palabras, el SSF habrá comenzado a enviar datos de [!DNL Analytics] a AAM, y dado que el código de DIL aún está en la página, también habrá una visita que irá directamente de la página a AAM, duplicando así los datos. Tan pronto como actualice el código de DIL a SSF, esto se aliviará.

**NOTA 2:** Si prefiere tener una pequeña discrepancia en los datos en lugar de una pequeña duplicación de datos, puede cambiar el orden de los pasos 1 y 2 anteriores. Si se mueve el código de DIL a SSF, se detendrá el flujo de datos a AAM hasta que se pueda voltear el conmutador para activar el SSF para el [!UICONTROL report suite]. Generalmente, los clientes prefieren duplicar los datos en lugar de dejar de obtener visitantes en [!UICONTROL traits] y [!UICONTROL segments].

#### Temporización de migración cuando tiene muchos sitios y [!UICONTROL Report Suites] {#migration-timing-when-you-have-many-sites-and-report-suites}

Este tema se aborda brevemente en secciones anteriores, ya que la estrategia principal puede resumirse de la siguiente manera:

Migrar un sitio/[!UICONTROL report suite] (o grupo de sitios/[!UICONTROL report suites]) a la vez.

Sin embargo, esto puede resultar un poco complicado en base a algunos escenarios posibles:

* Tiene un sitio que contiene varias [!UICONTROL report suites]
* Tiene un [!UICONTROL report suite] que incluye varios sitios (como un global [!UICONTROL report suite])
* Utilice una [!DNL Launch] propiedad para cubrir varios sitios
* Tiene diferentes equipos de desarrollo para diferentes sitios

Debido a estos elementos, se puede complicar un poco. Lo mejor que puedo sugerir son:

* Tómese algún tiempo para elaborar una estrategia de migración a la SSF, en base a lo que se ha explicado anteriormente
* En base al hecho de que una sola propiedad en [!DNL Launch] (o un solo [!DNL AppMeasurement] archivo) se asigna normalmente a 1 o 2 distintos [!UICONTROL report suites], es probable que pueda realizar un plan que funcione en estos grupos distintos uno por uno, actualizando su empresa a SSF
* Si está trabajando con la asesoría de Adobe, comuníquese con ellos en relación con su plan de migración para que puedan ayudarle según sea necesario

## Validación y resolución de problemas {#validation-and-troubleshooting}

La forma principal de validar que la aplicación [!UICONTROL Server-Side Forwarding] está activa es mirando la respuesta a cualquiera de las visitas de Adobe Analytics procedentes de la aplicación.

Si no se están haciendo datos [!UICONTROL server-side forwarding] de [!DNL Analytics] a Audience Manager, entonces no hay realmente ninguna respuesta a la [!DNL Analytics] señalización (además de un píxel 2x2). Sin embargo, si está haciendo SSF, hay elementos que puede comprobar en la solicitud y la respuesta que le harán saber que [!DNL Analytics] [!DNL Analytics] se está comunicando correctamente con el Audience Manager, reenviando la visita y obteniendo una respuesta.

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

**ADVERTENCIA:** Tenga cuidado con el falso &quot;éxito&quot; - Si hay una respuesta, y todo parece estar funcionando, asegúrese de tener el objeto &quot;material&quot; en la respuesta. Si no lo hace, puede que vea un mensaje que diga [!DNL "status":"SUCCESS"]. Por más loco que esto suene, esto es prueba de que NO está funcionando correctamente. Si lo ve, significa que ha completado la actualización de código en [!DNL Launch] o [!DNL AppMeasurement], pero que el reenvío en la [!DNL Analytics] [!DNL Admin Console] página aún no se ha completado. En este caso, debe comprobar que ha habilitado SSF en el [!DNL Analytics] formulario [!DNL Admin Console] para su [!UICONTROL report suite]. Si lo han hecho, y no han pasado aún 4 horas, tengan paciencia, ya que puede tomar tanto tiempo hacer todos los cambios necesarios en el servidor.

![falso éxito](assets/falsesuccess.png)

Para obtener más información sobre [!UICONTROL Server-Side Forwarding], consulte la [documentación](https://marketing.adobe.com/resources/help/en_US/reference/ssf.html).
