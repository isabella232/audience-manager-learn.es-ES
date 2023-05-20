---
title: Utilice modelos de similitud para ampliar el inventario agotado de datos de origen
description: En este tutorial, explicamos los pasos que debe seguir para configurar y utilizar modelos de similitud, de modo que pueda crear nuevas audiencias de similitud y venderlas como una extensión para su segmento de conversión.
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 23523.jpg
kt: 1688
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6820528e-3211-4a1d-be05-50f1292179d2
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '861'
ht-degree: 0%

---

# Utilice modelos de similitud para ampliar el inventario agotado de datos de origen {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

En este tutorial, explicaremos los pasos que debe seguir para configurar y utilizar la similitud [!UICONTROL Models], para que pueda crear nuevas audiencias de similitud y venderlas como una extensión de su segmento de conversión.

## Detalles del caso de uso {#use-case-details}

Es editor de contenido. Si ya ha agotado el inventario de convertidores en su sitio, es posible que piense que su oportunidad termina allí. AAM Introducir la similitud de los usuarios de la [!UICONTROL Models]. Al utilizar esta función, puede ampliar aún más el inventario de productos agotados y también vender audiencias de personas que quizá aún no se hayan convertido, pero que se ven o actúan como personas que se han convertido. Este segmento de audiencia generalmente se vendería por menos que los convertidores reales, pero sin embargo le permite agregar a sus resultados finales al proporcionar una opción de audiencia adicional para anunciantes que deseen colocar anuncios en su sitio. La ventaja adicional de este caso de uso es que no le cuesta nada ejecutar este modelo en los datos de origen.

Los pasos de este tutorial son los siguientes:

1. Identificar/crear un rasgo o segmento de usuario (conversión) ideal
1. Crear un modelo utilizando este rasgo o segmento de conversión como elemento base
1. Elegir [!UICONTROL First party] fuentes de datos en el modelo y ejecutar el modelo
1. Crear un [!UICONTROL Algorithmic Trait] a partir de los resultados del modelo y añada la característica a un segmento
1. Ofrezca el segmento a los anunciantes interesados para ampliar las ventas del segmento de conversión

## Identificar o crear un rasgo o segmento de usuario (conversión) ideal {#identify-create-an-ideal-user-conversion-trait-or-segment}

¿Qué está intentando que la gente haga en su sitio? ¿Cuál es su evento de conversión? Por supuesto, hay muchas respuestas diferentes a esta pregunta, según el tipo/vertical del sitio y los objetivos de la organización. AAM En cualquier caso, es habitual en los visitantes crear un rasgo para los visitantes que cumplen con esos criterios.

En este caso de uso, esto ya se supone, porque ha vendido el inventario para las personas que son convertidores. Sin embargo, a los efectos de este tutorial, es bueno analizarlo como referencia para el resto del caso de uso.

Además, cuando se utilizan eventos para crear características, hay una clave importante que debe tenerse en cuenta para que no se recopilen más usuarios de los que deberían en la característica. Vea el siguiente vídeo para la gran revelación. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**NOTA:** En el vídeo anterior, el ejemplo que muestro supone que tiene Adobe Analytics. Obviamente, este puede no ser el caso. Si tiene Google Analytics AAM de (GA), tenemos un módulo que puede utilizar para enviar datos a los usuarios de (consulte la sección sobre el envío de datos de ) a los que se puede acceder a través de la interfaz de usuario de (véase la página de ayuda de ). [documentación](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html)AAM ), y si GA envía a la actividad de conversión de su sitio, puede crear su rasgo de conversión a partir de ese elemento. AAM Si tiene una solución de análisis diferente (o no tiene ninguna solución de análisis), aún puede enviar datos a las soluciones a través de nuestro código de DIL y la dirección de correo electrónico de la aplicación de datos de a través de la interfaz de usuario de la aplicación de datos de. `submit` función, etc. (consulte la [documentación](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html)). A continuación, vuelva a crear la característica de conversión en función de los datos enviados cuando la actividad de conversión se realiza en el sitio.

## Crear un modelo de similitud a partir de datos de origen {#creating-a-look-alike-model-from-first-party-data}

En este paso, vamos a crear una [!UICONTROL First Party] Modelo de similitud. Esto significa que no solo vamos a utilizar un rasgo o segmento de conversión de origen para nuestro rasgo o segmento base (esto sería normal para la mayoría de los modelos de todos modos), sino que también vamos a buscar en el conjunto de datos de origen de más personas que se parecen a los convertidores. No se buscan datos de segundo nivel o de terceros.

En este caso de uso, esto es importante, ya que estamos intentando crear un segmento de usuarios en nuestro sitio que se parecen a los convertidores, pero que aún no se han convertido, de modo que podamos vender este segmento de similitud a anunciantes interesados.

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## Crear un rasgo algorítmico {#creating-an-algorithmic-trait}

A continuación, tendremos que crear una [!UICONTROL Algorithmic Trait], para que se puedan utilizar los resultados del modelo. Sin crear un rasgo, el modelo es inútil. Por lo tanto, después de ejecutar el modelo, asegúrese de entrar en el cuadro de diálogo de características y crear una [!UICONTROL Algorithmic Trait]. El siguiente vídeo lo analiza y muestra un par de sugerencias.

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## Ofrezca la [!UICONTROL Algorithmic Segment] a anunciantes {#offering-the-algorithmic-segment-to-advertisers}

Una vez que haya creado un [!UICONTROL Algorithmic Trait], puede crear un nuevo segmento para colocarlo en, de modo que pueda activar los datos (no puede activar una característica, sino crear un nuevo segmento de una característica con la variable [!UICONTROL Algorithmic Trait] en él, para que pueda activar (utilizar) el segmento.

Una vez que haya creado un segmento de visitantes de origen con una puntuación alta en el modelo de similitud (es decir, que parecen convertidores pero aún no se han convertido), puede ofrecer este segmento a los anunciantes del sitio, incluso después de haber vendido todo el inventario de convertidores reales del sitio. Esta es una buena manera de ampliar esta audiencia y seguir viendo ingresos adicionales mediante la similitud [!UICONTROL Models] en Audience Manager.
