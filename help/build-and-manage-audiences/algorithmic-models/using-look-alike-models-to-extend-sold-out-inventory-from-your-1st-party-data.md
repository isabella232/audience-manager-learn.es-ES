---
title: Uso de modelos de similitud para ampliar el inventario vendido a partir de los datos de origen
description: En este tutorial, explicaremos los pasos que debe seguir para configurar y utilizar modelos de similitud, de modo que pueda crear nuevas audiencias parecidas y venderlas como una extensión a su segmento de conversión.
feature: Modelos algorítmicos
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 23523.jpg
kt: 1688
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6820528e-3211-4a1d-be05-50f1292179d2
source-git-commit: 4b91696f840518312ec041abdbe5217178aee405
workflow-type: tm+mt
source-wordcount: '827'
ht-degree: 0%

---

# Uso de Aspecto [!UICONTROL Models] para extender el inventario vendido a partir de los datos de [!UICONTROL First Party] {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

En este tutorial, explicaremos los pasos que debe seguir para configurar y utilizar Look-Alike [!UICONTROL Models], de modo que pueda crear nuevas audiencias parecidas y venderlas como una extensión de su conversión [!UICONTROL segment].

## Detalles de caso de uso {#use-case-details}

Usted es un editor de contenido. Si ya ha agotado el inventario de los convertidores del sitio, puede que piense que su oportunidad termina allí. Introduzca el Aspecto de AAM [!UICONTROL Models]. Con esta función, puede ampliar aún más el inventario vendido y también vender audiencias de personas que quizá no se hayan convertido aún, pero que parezcan o actúen como personas que se han convertido. Esta audiencia [!UICONTROL segment] generalmente vendería por menos de los convertidores reales, pero sin embargo le permite agregar a su resultado final al proporcionar una opción de audiencia adicional para los anunciantes que desean colocar anuncios en su sitio. La ventaja adicional de este caso de uso es que no le cuesta nada ejecutar este modelo en los datos de origen.

Los pasos de este tutorial son los siguientes:

1. Identificar/Crear un usuario ideal (conversión) [!UICONTROL trait] o [!UICONTROL segment]
1. Cree un [!UICONTROL model] utilizando esta conversión [!UICONTROL trait]/[!UICONTROL segment] como elemento base
1. Elija [!UICONTROL First party] fuentes de datos en [!UICONTROL model] y ejecute el [!UICONTROL model]
1. Cree un [!UICONTROL Trait] algorítmico a partir de los [!UICONTROL model] resultados y añada el [!UICONTROL trait] a un [!UICONTROL segment]
1. Ofrezca el [!UICONTROL segment] a los anunciantes interesados para ampliar las ventas de conversión [!UICONTROL segment]

## Identificar/Crear un usuario ideal (conversión) [!UICONTROL trait] o [!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

¿Qué está intentando hacer que la gente haga en su sitio? ¿Cuál es su evento de conversión? Por supuesto, hay muchas respuestas diferentes a esta pregunta, según el tipo de sitio o vertical y los objetivos de la organización. En cualquier caso, es común en AAM crear un [!UICONTROL trait] para los visitantes que han cumplido esos criterios.

En este caso de uso, esto ya se supone, porque ha vendido el inventario a las personas que son convertidores. Sin embargo, a los efectos de este tutorial, es bueno discutirlo como referencia para el resto del caso de uso.

Además, al usar eventos para crear [!UICONTROL traits], hay una gotcha importante que debe tener en cuenta, de modo que no recopile más usuarios de los que debiera en el [!UICONTROL trait]. Vea el siguiente vídeo para ver la gran revelación. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**NOTA:** En el vídeo anterior, el ejemplo que mostré supone que tiene Adobe Analytics. Obviamente, tal vez no sea así. Si tiene Google Analytics (GA), tenemos un módulo que puede usar para enviar datos a AAM (consulte la [documentación](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)) y, si la actividad de conversión de su sitio se envía a AAM por medio de GA, puede crear el rasgo de conversión a partir de eso. Si tiene una solución de análisis diferente (o ninguna solución de análisis), aún puede enviar datos a AAM mediante nuestro código de DIL y la función `submit` , etc. (consulte la [documentación](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)). A continuación, de nuevo, cree el rasgo de conversión en función de los datos enviados cuando se realiza la actividad de conversión en el sitio.

## Creación de una apariencia [!UICONTROL Model] a partir de datos [!UICONTROL First Party] {#creating-a-look-alike-model-from-first-party-data}

En este paso, vamos a crear un [!UICONTROL First Party] Aspecto similar [!UICONTROL Model]. Esto significa que no solo vamos a usar una [!UICONTROL first party] conversión [!UICONTROL trait]/[!UICONTROL segment] para nuestra base [!UICONTROL trait]/[!UICONTROL segment] (esto sería normal para la mayoría de los [!UICONTROL models] de todas maneras), sino que solo veremos el grupo de datos [!UICONTROL first party] para más personas que se parezcan a los convertidores. No veremos los datos [!UICONTROL second party] o [!UICONTROL third party].

En este caso de uso, esto es importante, ya que estamos intentando crear un [!UICONTROL segment] de usuarios en nuestro sitio que parezcan convertidores pero que aún no se han convertido, de modo que podamos vender este parecido [!UICONTROL segment] a los anunciantes interesados.

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## Creación de un algoritmo [!UICONTROL Trait] {#creating-an-algorithmic-trait}

A continuación, tendremos que crear un [!UICONTROL Trait] algorítmico para poder utilizar los resultados del [!UICONTROL model]. Sin crear un [!UICONTROL trait], el [!UICONTROL model] no sirve. Por lo tanto, después de ejecutar [!UICONTROL model], asegúrese de entrar en el cuadro de diálogo [!UICONTROL trait] y crear un [!UICONTROL Trait] algorítmico. El siguiente vídeo lo recorre y muestra un par de consejos.

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## Oferta algorítmica [!UICONTROL Segment] a los anunciantes {#offering-the-algorithmic-segment-to-advertisers}

Una vez creado un [!UICONTROL Trait] algorítmico, puede crear un nuevo [!UICONTROL segment] para introducirlo, de modo que pueda activar los datos (no puede activar un [!UICONTROL trait], sino crear un nuevo [!UICONTROL trait] [!UICONTROL segment] con el [!UICONTROL Trait] algorítmico, de modo que pueda activar (utilizar) el [!UICONTROL segment].

Una vez que haya creado un [!UICONTROL segment] de [!UICONTROL first party] visitantes que han obtenido una calificación alta en el parecido [!UICONTROL model] (es decir, que parecen convertidores pero aún no lo han hecho), puede ofrecer este [!UICONTROL segment] a los anunciantes del sitio, incluso después de haber agotado todo el inventario de convertidores reales del sitio. Esta es una buena manera de ampliar esta audiencia y seguir viendo ingresos adicionales mediante el uso de Aspecto [!UICONTROL Models] en Audience Manager.
