---
title: Utilice modelos de similitud para ampliar el inventario vendido a partir de datos de origen
description: En este tutorial, explicamos los pasos que debe seguir para configurar y utilizar modelos de similitud, de modo que pueda crear nuevas audiencias parecidas y venderlas como una extensión del segmento de conversión.
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

# Utilice modelos de similitud para ampliar el inventario vendido a partir de datos de origen {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

En este tutorial, veremos los pasos que debe seguir para configurar y utilizar Look-Alike [!UICONTROL Models], para poder crear nuevas audiencias parecidas y venderlas como una extensión al segmento de conversión.

## Detalles de caso de uso {#use-case-details}

Usted es un editor de contenido. Si ya ha agotado el inventario de los convertidores del sitio, puede que piense que su oportunidad termina allí. Introduzca el aspecto del AAM [!UICONTROL Models]. Con esta función, puede ampliar aún más el inventario vendido y también vender audiencias de personas que quizá no se hayan convertido aún, pero que parezcan o actúen como personas que se han convertido. Este segmento de audiencia generalmente vendería por menos de los convertidores reales, pero sin embargo le permite agregar a su resultado final al proporcionar una opción de audiencia adicional para los anunciantes que desean colocar anuncios en su sitio. La ventaja adicional de este caso de uso es que no le cuesta nada ejecutar este modelo en los datos de origen.

Los pasos de este tutorial son los siguientes:

1. Identificar/crear un rasgo o segmento ideal de usuario (conversión)
1. Cree un modelo utilizando este rasgo o segmento de conversión como elemento base
1. Choose [!UICONTROL First party] fuentes de datos en el modelo y ejecutar el modelo
1. Cree un [!UICONTROL Algorithmic Trait] de los resultados del modelo y añada el rasgo a un segmento
1. Ofrezca el segmento a los anunciantes interesados para ampliar las ventas del segmento de conversión

## Identificar o crear un rasgo o segmento (conversión) de usuario ideal {#identify-create-an-ideal-user-conversion-trait-or-segment}

¿Qué está intentando hacer que la gente haga en su sitio? ¿Cuál es su evento de conversión? Por supuesto, hay muchas respuestas diferentes a esta pregunta, según el tipo de sitio o vertical y los objetivos de la organización. En cualquier caso, es común en AAM crear un rasgo para los visitantes que han cumplido esos criterios.

En este caso de uso, esto ya se supone, porque ha vendido el inventario a las personas que son convertidores. Sin embargo, a los efectos de este tutorial, es bueno discutirlo como referencia para el resto del caso de uso.

Además, al usar eventos para crear características, hay una gotcha importante que debe tener en cuenta para no recopilar más usuarios de los que debería en el rasgo. Vea el siguiente vídeo para ver la gran revelación. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**NOTA:** En el vídeo anterior, el ejemplo que mostré supone que tiene Adobe Analytics. Obviamente, tal vez no sea así. Si tiene Google Analytics (GA), tenemos un módulo que puede utilizar para enviar datos a AAM (consulte la [documentación](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html)), y si la actividad de conversión de su sitio se envía a AAM por medio de GA, puede crear la característica de conversión a partir de eso. Si tiene una solución de análisis diferente (o no tiene ninguna solución de análisis), aún puede enviar datos a AAM mediante nuestro código de DIL y el `submit` función, etc. (consulte la [documentación](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html)). A continuación, de nuevo, cree el rasgo de conversión en función de los datos enviados cuando se realiza la actividad de conversión en el sitio.

## Crear un modelo de similitud a partir de datos de origen {#creating-a-look-alike-model-from-first-party-data}

En este paso, crearemos un [!UICONTROL First Party] Modelo similar. Esto significa que no solo vamos a usar un rasgo o segmento de conversión de origen para nuestro rasgo o segmento base (esto sería normal para la mayoría de los modelos de todas maneras), sino que también solo vamos a buscar en el grupo de datos de origen para más personas que se parezcan a los convertidores. No analizaremos los datos de segundo nivel o de terceros.

En este caso de uso, esto es importante, ya que estamos intentando crear un segmento de usuarios en nuestro sitio que parezcan convertidores pero que aún no se han convertido, de modo que podamos vender este segmento de similitud a los anunciantes interesados.

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## Crear un rasgo algorítmico {#creating-an-algorithmic-trait}

A continuación, tendremos que crear un [!UICONTROL Algorithmic Trait], para que se puedan utilizar los resultados del modelo. Sin crear un rasgo, el modelo es inútil. Así que después de ejecutar el modelo, asegúrese de entrar en el cuadro de diálogo de características y crear un [!UICONTROL Algorithmic Trait]. El siguiente vídeo lo recorre y muestra un par de consejos.

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## Ofrezca la variable [!UICONTROL Algorithmic Segment] a anunciantes {#offering-the-algorithmic-segment-to-advertisers}

Una vez que haya creado un [!UICONTROL Algorithmic Trait], puede crear un segmento nuevo para incluirlo, de modo que pueda activar los datos (no puede activar un rasgo, sino crear un nuevo segmento de un rasgo con la variable [!UICONTROL Algorithmic Trait] en ella, para que pueda activar (utilizar) el segmento.

Una vez creado un segmento de visitantes de origen que han obtenido una puntuación alta en el modelo de similitud (es decir, que parecen convertidores pero que aún no han convertido), puede ofrecer este segmento a los anunciantes del sitio, incluso después de haber agotado todo el inventario de convertidores reales del sitio. Esta es una buena forma de ampliar esta audiencia y seguir viendo ingresos adicionales mediante el uso de &quot;Look Alike&quot; [!UICONTROL Models] en Audience Manager.
