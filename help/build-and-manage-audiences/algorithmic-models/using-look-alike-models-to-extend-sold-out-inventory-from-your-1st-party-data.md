---
title: Uso de modelos similares para ampliar el inventario vendido a partir de los datos de origen
description: En este tutorial, explicaremos los pasos que debe seguir para configurar y utilizar modelos parecidos a los de búsqueda, de modo que pueda crear nuevas audiencias similares y venderlas como una extensión a su segmento de conversión.
feature: modelos algorítmicos
topics: null
audience: all
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 23523.jpg
kt: 1688
translation-type: tm+mt
source-git-commit: ba76f9437e5d8f0495e4f2dfafb90cbf2da6454f
workflow-type: tm+mt
source-wordcount: '827'
ht-degree: 0%

---


# Uso de Look-Alike [!UICONTROL Models] para extender el inventario vendido de sus [!UICONTROL First Party] datos {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

En este tutorial, explicaremos los pasos que debe seguir para configurar y utilizar Look-Alike [!UICONTROL Models], de modo que pueda crear nuevas audiencias similares y venderlas como una extensión de su conversión [!UICONTROL segment].

## Detalles del caso de uso {#use-case-details}

Usted es un editor de contenido. Si ya ha agotado el inventario de los convertidores del sitio, puede que piense que la oportunidad termina ahí. Introduzca la apariencia de AAM [!UICONTROL Models]. Al utilizar esta función, puede ampliar aún más el inventario vendido y también vender audiencias de personas que quizá no se hayan convertido aún, pero que se vean o actúen como personas que se han convertido. Esta audiencia [!UICONTROL segment] generalmente se vendería por menos que los convertidores reales, pero sin embargo le permite agregar a su resultado final al proporcionar una opción de audiencia adicional para los anunciantes que deseen colocar publicidades en su sitio. La ventaja adicional de este caso de uso es que no le cuesta nada ejecutar este modelo en los datos de origen.

Los pasos de este tutorial serán los siguientes:

1. Identifique o cree un usuario ideal (conversión) [!UICONTROL trait] o [!UICONTROL segment]
1. Cree un [!UICONTROL model] usando esta conversión [!UICONTROL trait]/[!UICONTROL segment] como elemento base
1. Elija [!UICONTROL First party] fuentes de datos en el [!UICONTROL model] y ejecute el [!UICONTROL model]
1. Cree un algoritmo [!UICONTROL Trait] a partir de los resultados [!UICONTROL model] y agregue el [!UICONTROL trait] a un [!UICONTROL segment]
1. Oferta de [!UICONTROL segment] a los anunciantes interesados para ampliar las ventas de conversión [!UICONTROL segment]

## Identifique o cree un usuario ideal (conversión) [!UICONTROL trait] o [!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

¿Qué intenta hacer que la gente haga en su sitio? ¿Cuál es su evento de conversión? Por supuesto, hay muchas respuestas diferentes a esta pregunta, según el tipo de sitio o el vertical y los objetivos de la organización. En cualquier caso, es común en AAM crear un [!UICONTROL trait] para visitantes que hayan cumplido esos criterios.

En este caso de uso, esto ya se supone, porque se ha agotado el inventario para las personas que son convertidores. Sin embargo, a los efectos de este tutorial, es bueno discutirlo como referencia para el resto del caso de uso.

Además, al usar eventos para crear [!UICONTROL traits], hay una importante gotcha que debe tener en cuenta, de modo que no recopile más usuarios de los que debiera en el [!UICONTROL trait]. Vea el siguiente video para ver la gran revelación. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**NOTA:** En el vídeo anterior, el ejemplo que muestro supone que tienes Adobe Analytics. Evidentemente, tal vez no sea así. Si tiene Google Analytics (GA), tenemos un módulo que puede utilizar para enviar datos a AAM (consulte la [documentación](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)), y si la actividad de conversión del sitio se envía a AAM por GA, puede crear su característica de conversión a partir de eso. Si tiene una solución de análisis diferente (o no tiene una solución de análisis), puede enviar datos a AAM mediante nuestro código de DIL y la función `submit`, etc. (consulte la [documentación](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)). A continuación, vuelva a crear la característica de conversión en función de los datos enviados cuando se realice la actividad de conversión en el sitio.

## Creación de una apariencia similar [!UICONTROL Model] a partir de [!UICONTROL First Party] datos {#creating-a-look-alike-model-from-first-party-data}

En este paso, vamos a crear un [!UICONTROL First Party] Look-Alike [!UICONTROL Model]. Esto significa que no sólo vamos a utilizar una [!UICONTROL first party] conversión [!UICONTROL trait]/[!UICONTROL segment] para nuestra base [!UICONTROL trait]/[!UICONTROL segment] (esto sería normal para la mayoría de [!UICONTROL models] de todos modos), sino que también sólo estaremos mirando el grupo de datos [!UICONTROL first party] para más personas que se vean como los convertidores. No analizaremos los datos [!UICONTROL second party] o [!UICONTROL third party].

En este caso de uso, esto es importante, porque estamos tratando de crear un [!UICONTROL segment] de usuarios en nuestro sitio que parecen convertidores pero que aún no se han convertido, para que podamos vender este [!UICONTROL segment] similar a los anunciantes interesados.

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## Creación de un algoritmo [!UICONTROL Trait] {#creating-an-algorithmic-trait}

Luego tendremos que crear un algoritmo [!UICONTROL Trait] para poder utilizar los resultados del [!UICONTROL model]. Sin crear un [!UICONTROL trait], el [!UICONTROL model] es inútil. Después de ejecutar [!UICONTROL model], asegúrese de entrar en el cuadro de diálogo [!UICONTROL trait] y crear un algoritmo [!UICONTROL Trait]. El siguiente vídeo lo atraviesa y muestra un par de consejos.

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## Ofreciendo el algoritmo [!UICONTROL Segment] a los anunciantes {#offering-the-algorithmic-segment-to-advertisers}

Una vez que haya creado un [!UICONTROL Trait] algorítmico, puede crear un nuevo [!UICONTROL segment] para colocarlo, de modo que pueda activar los datos (no puede activar un [!UICONTROL trait], sino crear un nuevo [!UICONTROL trait] [!UICONTROL segment] con el Algoritmo [!UICONTROL Trait], para que pueda activar (utilizar) el [!UICONTROL segment].

Una vez que haya creado un [!UICONTROL segment] de [!UICONTROL first party] visitantes con una puntuación alta en la categoría similar [!UICONTROL model] (es decir, que parecen convertidores pero aún no han tenido una conversión), puede oferta este [!UICONTROL segment] a los anunciantes del sitio, incluso después de haber agotado todo el inventario de convertidores reales del sitio. Esta es una buena manera de extender esta audiencia y seguir viendo ingresos adicionales mediante el uso de Look-Alike [!UICONTROL Models] en Audience Manager.
