---
title: Migre la implementación de Audience Manager de su sitio desde el DIL del lado del cliente al reenvío del lado del servidor
description: Obtenga información sobre cómo migrar la implementación de Audience Manager (AAM) de su sitio de DIL del lado del cliente al reenvío del lado del servidor. Este tutorial se aplica si tiene AAM y Adobe Analytics, y envía visitas desde la página a AAM mediante código de DIL (Data Integration Library), así como visitas desde la página a Adobe Analytics.
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

# Migre la implementación de Audience Manager de su sitio desde el DIL del lado del cliente al reenvío del lado del servidor {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

Este tutorial se aplica si tiene Adobe Audience Manager (AAM) y Adobe Analytics, y actualmente está enviando una visita desde la página a AAM con el DIL ([!DNL Data Integration Library]) y también enviando una visita desde la página a Adobe Analytics. Dado que tiene ambas soluciones y que ambas forman parte de Adobe Experience Cloud, tiene la oportunidad de seguir la práctica recomendada de activar el reenvío del lado del servidor, que habilita la función [!DNL Analytics] servidores de recopilación de datos para reenviar datos de análisis del sitio en tiempo real al Audience Manager, en lugar de hacer que el código del lado del cliente envíe una visita adicional desde la página a AAM. Este tutorial lo acompaña durante los pasos necesarios para realizar el cambio de la implementación de DIL del lado del cliente anterior al método de reenvío del lado del servidor más reciente.

## Lado del cliente (DIL) frente a lado del servidor {#client-side-dil-vs-server-side}

Al comparar y contrastar estos dos métodos de obtención de datos de Adobe Analytics en AAM, podría resultar útil visualizar primero las diferencias en la siguiente imagen:

![del lado del cliente al lado del servidor](assets/client-side_vs_server-side_aam_implementation.png)

### Implementación del DIL del lado del cliente {#client-side-dil-implementation}

Si utiliza este método para obtener datos de Adobe Analytics en AAM, tiene dos visitas provenientes de las páginas web: Una que va a [!DNL Analytics]y uno que va a AAM (después de haber copiado el [!DNL Analytics] datos de la página web. [!UICONTROL Segments] se devuelven de AAM a la página, donde se pueden utilizar para la personalización, etc. Esto se considera una implementación heredada y ya no se recomienda.

Más allá del hecho de que esto no sigue las prácticas recomendadas, las desventajas de utilizar este método incluyen:

* Dos visitas provenientes de la página en lugar de solo una
* el reenvío del lado del servidor es necesario para el uso compartido en tiempo real de AAM audiencias a [!DNL Analytics], de modo que las implementaciones del lado del cliente no permiten esta función (y otras funciones potenciales en el futuro)

Se recomienda pasar a un método de reenvío del lado del servidor de AAM implementación.

### Implementación de reenvío del lado del servidor {#server-side-forwarding-implementation}

Como se muestra en la imagen anterior, una visita viene de la página web a Adobe Analytics. [!DNL Analytics] luego reenvía esos datos a AAM en tiempo real, y los visitantes se evalúan en rasgos AAM y [!UICONTROL segments], igual que si la visita hubiera llegado directamente desde la página.

[!UICONTROL Segments] se devuelven en la misma visita en tiempo real a [!DNL Analytics], que reenvía la respuesta a la página web para su personalización, etc.

No hay un inconveniente en el tiempo para pasar al reenvío del lado del servidor. Adobe recomienda encarecidamente que cualquier persona que tenga un Audience Manager y [!DNL Analytics] utiliza este método de implementación.

## Tiene dos tareas principales {#you-have-two-main-tasks}

Hay bastante información en esta página, y todo es importante, por supuesto. Sin embargo, sí **todo se reduce a dos cosas principales que debe hacer**:

1. Cambie el código de código de DIL del lado del cliente a código de reenvío del lado del servidor
1. Girar el interruptor en la [!DNL Analytics] [!DNL Admin Console] para iniciar el reenvío real de datos (por [!UICONTROL report suite])

Si omite cualquiera de estas tareas, el reenvío del lado del servidor no funcionará correctamente. Se han añadido pasos y datos adicionales a este documento para ayudarle a realizar correctamente estos dos pasos en la configuración.

## Opciones de implementación {#implementation-options}

Al pasar del lado del cliente al reenvío del lado del servidor, una de las tareas que tendrá es cambiar el código por el nuevo código de reenvío del lado del servidor. Esto se realiza mediante una de las siguientes opciones:

* Etiquetas Adobe Experience Platform : la opción de implementación recomendada por Adobe para las propiedades web. Verá que esta es una tarea fácil, ya que las etiquetas de Platform han hecho todo el trabajo duro por usted.
* En la página : también puede colocar el nuevo código SSF directamente en el `doPlugins` dentro de su `appMeasurement.js` , si todavía no está utilizando Adobe Launch
* Otros administradores de etiquetas : se pueden tratar como la opción anterior (En la página), ya que aún colocará el código SSF en `doPlugins`, donde el otro administrador de etiquetas almacene la variable [!DNL AppMeasurement] code

Veremos cada uno de estos elementos en la _Actualización del código_ para obtener más información.

## Pasos de implementación {#implementation-steps}

Los siguientes pasos describen la implementación.

### Paso 0: Requisito previo: Servicio de ID de Experience Cloud (ECID) {#step-prerequisite-experience-cloud-id-service-ecid}

El requisito previo principal para pasar al reenvío del lado del servidor es tener implementado el servicio de ID de Experience Cloud. Esto se hace más fácilmente si utiliza Experience Platform Launch, en cuyo caso simplemente instala la extensión ECID y hará el resto.

Si utiliza un sistema de administración de etiquetas que no es de Adobe o ningún sistema de administración de etiquetas, implemente el ECID para ejecutar **before** cualquier otra solución de Adobe. Consulte la [Documentación de ECID](https://experienceleague.adobe.com/docs/id-service/using/home.html) para obtener más información. El único otro requisito previo es el de las versiones de código, por lo que, como simplemente aplica las versiones más recientes del código en los pasos siguientes, estará bien.

>[!NOTE]
>
>Lea todo el documento antes de implementarlo. La sección &quot;Temporización&quot; a continuación contiene información importante sobre *when* debe implementar cada pieza, incluido el ECID (si aún no se ha implementado).

### Paso 1: Registrar las opciones usadas actualmente desde el código del DIL {#step-record-currently-used-options-from-dil-code}

A medida que se prepara para pasar del código de DIL del lado del cliente al reenvío del lado del servidor, el primer paso es identificar todo lo que está haciendo con el código de DIL, incluida la configuración personalizada y los datos enviados a AAM. Algunas cosas que hay que tener en cuenta y considerar son:

* Normal [!DNL Analytics] usando la variable `siteCatalyst.init` Módulo de DIL: no necesita preocuparse por esto, ya que su trabajo es simplemente enviar la variable [!DNL Analytics] y esto sucede simplemente teniendo habilitado el reenvío del lado del servidor.
* Subdominio de socio : en la variable `DIL.create` , tome nota de la función `partner` parámetro. Esto se conoce como su &quot;subdominio de socio&quot; o, en ocasiones, &quot;ID de socio&quot; y será necesario cuando coloque el nuevo código de reenvío del lado del servidor.
* [!DNL Visitor Service Namespace] - También conocido como su[!DNL Org ID]&quot; o &quot;[!DNL IMS Org ID],&quot; también lo necesitará cuando configure el nuevo código de reenvío del lado del servidor. Anote esto.
* containerNSID, uuidCookie y otras opciones avanzadas : tome nota de las opciones avanzadas adicionales que esté utilizando para que pueda configurarlas también en el código de reenvío del lado del servidor.
* Variables de página adicionales : si se envían otras variables a AAM desde la página (además de la variable [!DNL Analytics] variables gestionadas por siteCatalyst.init), deberá tener en cuenta dichas variables para que se puedan enviar mediante reenvío del lado del servidor (alerta de spoiler: via [!DNL contextData] ).

### Paso 2: Actualizar el código {#step-updating-the-code}

En [Opciones de implementación](#implementation-options) (arriba), se ofrecen múltiples opciones sobre cómo y dónde se implementa el reenvío del lado del servidor. Para que esta sección sea efectiva, necesitamos dividirla en estas secciones (con dos de ellas combinadas). Vaya al método de esta sección que describa mejor sus necesidades.

#### Etiquetas de Adobe Experience Platform {#launch-by-adobe}

Vea el siguiente vídeo para obtener más información sobre cómo mover las opciones de implementación del código de DIL del lado del cliente al reenvío del lado del servidor en Experience Platform Launch.

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### &quot;En la página&quot; o el administrador de etiquetas que no sean de Adobe {#on-the-page-or-non-adobe-tag-manager}

Vea el siguiente vídeo para obtener más información sobre cómo mover las opciones de implementación del código de DIL del lado del cliente al reenvío del lado del servidor en [!DNL AppMeasurement] código, que reside en un archivo o en un sistema de administración de etiquetas que no es de Adobe.

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### Paso 3: Activación del reenvío (por [!UICONTROL Report Suite]) {#step-enabling-the-forwarding-per-report-suite}

Hasta ahora, en este tutorial hemos invertido todo nuestro tiempo en cambiar el código del código del DIL del lado del cliente al reenvío del lado del servidor. Está bien, porque es la parte más difícil. Esta sección, aunque verá que es superfácil, es tan importante como actualizar el código. En este vídeo, verá cómo voltear el conmutador que habilita el reenvío real de datos de Analytics al Audience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**NOTA:** Como se indica en el vídeo, recuerde que la habilitación del reenvío tardará hasta 4 horas en implementarse completamente en el backend del Experience Cloud.

## Temporización {#timing}

Como recordatorio, existen dos tareas principales para pasar del DIL del lado del cliente al reenvío del lado del servidor:

1. Actualización del código
1. Girando el interruptor en el [!DNL Analytics] [!DNL Admin Console]

Pero la pregunta es, ¿cuál haces primero? ¿Importa? Bien, perdón, fueron dos preguntas. Pero las respuestas son... depende, y sí, *can* materia. ¿Cómo es eso para vago? Desglosémoslo. Pero primero una pregunta adicional que puede surgir si es una organización grande con numerosos sitios: ¿Tengo que hacer todo a la vez? Ese es un poco más fácil. No. Puedes hacerlo pieza a pieza.

### Un poco más profundo {#a-little-deeper-dive}

La razón por la que el tiempo y el orden importan es por la forma en que se reenvían _real_ , que puede resumirse en los siguientes aspectos técnicos:

* Si tiene implementado el servicio de ID de Experience Cloud (ECID) y el conmutador en la variable [!DNL Analytics] [!DNL Admin Console] (&quot;el conmutador&quot;) está activado, los datos se reenviarán desde [!DNL Analytics] para AAM, aunque aún no haya actualizado el código.
* Si no tiene implementado ECID, los datos no se reenviarán, aunque tenga el conmutador activado, y el código de reenvío del lado del servidor.
* El código de reenvío del lado del servidor (ya sea en etiquetas de Platform o en la página) realmente gestiona la respuesta y es necesario para completar la migración.
* Recuerde que el conmutador de reenvío del lado del servidor está habilitado por el [!UICONTROL report suite], pero que el código lo gestiona la propiedad de las etiquetas de Platform o la [!DNL AppMeasurement] si no utiliza etiquetas de Platform.

### Prácticas recomendadas {#best-practices}

Basándose en estos detalles técnicos, estas son las recomendaciones para el momento en el que debe hacerse y cuándo:

#### Si NO tiene ECID aún implementado {#if-you-do-not-have-ecid-yet-implemented}

1. Gire el interruptor hacia dentro [!DNL Analytics] para cada [!UICONTROL report suite] que habilitará para el reenvío del lado del servidor.

   1. El reenvío aún no comienza porque no tiene ECID.

1. Por sitio, actualice el código desde el DIL del lado del cliente al reenvío del lado del servidor (puede estar en etiquetas de Platform) o en la página, tal como se explica en otra sección anterior).

   1. El reenvío ahora fluye (como ha añadido ECID) y también debe recibir una respuesta JSON adecuada a su [!DNL Analytics] señalización (consulte la sección Validación y solución de problemas a continuación para obtener más información).

#### Si tiene implementado ECID {#if-you-do-have-ecid-implemented}

1. Prepare y planifique para que esté preparado para actualizar su código de reenvío del lado del DIL al lado del servidor PER [!UICONTROL report suite] que habilitará para el reenvío del lado del servidor:

   1. Gire el interruptor hacia dentro [!DNL Analytics] para habilitar el reenvío del lado del servidor.

      1. El reenvío empezará porque tiene ECID habilitado.
   1. Tan pronto como sea posible, actualice el código desde el DIL del lado del cliente al reenvío de un solo lado (esto podría estar en las etiquetas de Platform o en la página, como se ha analizado en otra sección anterior).

      1. Debe recibir una respuesta JSON adecuada a su [!DNL Analytics] señalización (consulte la [Validación y solución de problemas](#validation-and-troubleshooting) para obtener más información).


>[!NOTE]
>
>Es importante realizar estos dos pasos lo más cerca posible entre sí, ya que entre los pasos 1 y 2 anteriores tendrá duplicación de datos que se AAM. En otras palabras, el reenvío de un solo lado habrá empezado a enviar datos desde [!DNL Analytics] para AAM, y dado que el código de DIL sigue en la página, también habrá una visita que pasará directamente de la página a AAM, duplicando así los datos. Tan pronto como actualice el código del reenvío del lado del DIL al lado del servidor, esto se aliviará.

>[!NOTE]
>
>Si prefiere tener una pequeña discrepancia en los datos en lugar de una pequeña duplicación de datos, puede cambiar el orden de los pasos 1 y 2 anteriores. Si se mueve el código de DIL a reenvío del lado del servidor, se detendrá el flujo de datos en AAM hasta que se pueda voltear el conmutador para activar el reenvío del lado del servidor para la variable [!UICONTROL report suite]. Normalmente, los clientes prefieren duplicar los datos en lugar de dejar de atraer visitantes a rasgos y [!UICONTROL segments].

#### Temporización de migración cuando tiene muchos sitios y [!UICONTROL report suites] {#migration-timing-when-you-have-many-sites-and-report-suites}

Este tema se ha abordado brevemente en secciones anteriores, ya que la estrategia principal puede resumirse de la siguiente manera:

Migración de un sitio/[!UICONTROL report suite] (o grupo de sitios/[!UICONTROL report suites]) a la vez.

Sin embargo, esto puede resultar un poco complicado en función de algunos escenarios posibles:

* Tiene un sitio que contiene varias [!UICONTROL report suites]
* Tiene un [!UICONTROL report suite] que incluye varios sitios (como una [!UICONTROL report suite])
* Utilice una propiedad de etiquetas de Platform para cubrir varios sitios
* Tiene diferentes equipos de desarrollo para diferentes sitios

Debido a estos elementos, puede resultar un poco complicado. Las mejores cosas que puedo sugerir son:

* Tómese algún tiempo para crear una estrategia para migrar al reenvío del lado del servidor, en función de lo que se ha explicado anteriormente
* Basado en el hecho de que una sola propiedad en etiquetas de Platform (o una sola [!DNL AppMeasurement] archivo) normalmente se asigna a 1 o 2 distintos [!UICONTROL report suites], es probable que pueda realizar un plan que funcione en estos grupos diferentes uno a uno, actualizando su empresa al reenvío del lado del servidor
* Si está trabajando con la asesoría de Adobe, hable con ellos acerca de su plan de migración para que puedan ayudarle según sea necesario

## Validación y solución de problemas {#validation-and-troubleshooting}

La forma principal de validar que el reenvío del lado del servidor funciona es consultar la respuesta a cualquiera de sus visitas de Adobe Analytics procedentes de la aplicación.

Si no realiza el reenvío de datos del lado del servidor desde [!DNL Analytics] al Audience Manager, entonces no hay respuesta a la variable [!DNL Analytics] señalización (aparte de un píxel 2x2). Sin embargo, si está reenviando del lado del servidor, hay elementos que puede verificar en la variable [!DNL Analytics] solicitud y respuesta que le harán saber que [!DNL Analytics] se está comunicando correctamente con Audience Manager, reenviando la visita y obteniendo una respuesta.

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

>[!WARNING]
>
>Tenga en cuenta los falsos &quot;Success&quot;. Si hay una respuesta y todo parece estar funcionando, asegúrese de que tiene la variable `stuff` en la respuesta. Si no lo hace, es posible que vea un mensaje que dice `"status":"SUCCESS"`. Aunque parezca absurdo, esto es prueba de que NO funciona correctamente.
>
>Si lo ve, significa que ha completado la actualización del código en las etiquetas de Platform o [!DNL AppMeasurement], pero que el reenvío en la variable [!DNL Analytics] [!DNL Admin Console] aún no se ha completado. En este caso, debe verificar que ha habilitado el reenvío del lado del servidor en la variable [!DNL Analytics] [!DNL Admin Console] para su [!UICONTROL report suite]. Si lo ha hecho, y aún no han pasado 4 horas, tenga paciencia, ya que puede tardar tanto en realizar todos los cambios necesarios en el servidor.


![falso éxito](assets/falsesuccess.png)

Para obtener más información sobre el reenvío del lado del servidor, consulte la [documentación](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html).
