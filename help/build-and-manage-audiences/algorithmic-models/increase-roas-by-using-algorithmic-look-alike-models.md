---
title: Aumentar ROAS usando modelos algorítmicos (similares) en Audience Manager
description: El verdadero poder del modelado similar al de los Audience Manager viene cuando se busca expandir la audiencia de línea de base en comparación con un conjunto de usuarios nuevos y de calidad de fuentes de datos de terceros. En este tutorial, aprenda los pasos para crear un modelo a partir de estos datos.
feature: algorithmic models
topics: null
audience: all
activity: use
doc-type: feature video
team: Technical Marketing
kt: 1849
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '860'
ht-degree: 0%

---


# Aumentar ROAS mediante el uso de algoritmos (Look Alike) [!UICONTROL Models] en el Audience Manager {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

El poder real de la apariencia de los Audience Manager [!UICONTROL Modeling] se produce cuando se busca expandir la audiencia de línea de base con un nuevo conjunto de usuarios de [!UICONTROL second party] y [!UICONTROL third party] [!UICONTROL data sources] de calidad. En este tutorial, aprenda los pasos necesarios para crear un [!UICONTROL model] a partir de estos datos.

## Habilitar flujos de datos [!UICONTROL Second Party] o [!UICONTROL Third Party] desde el Audience Marketplace {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

Para utilizar datos [!UICONTROL second party] y [!UICONTROL third party] en un [!UICONTROL model] similar, primero debemos habilitar estos datos en la interfaz de Audience Manager. Adobe tiene un gran número de [!UICONTROL second party] y [!UICONTROL third party] proveedores de datos de los que puede elegir. Están disponibles para usted en una interfaz de autoservicio en AAM, a través del Audience Marketplace. Navegue hasta el Audience Marketplace y explore las posibilidades. En el siguiente vídeo se muestra cómo hacerlo, incluida la forma de habilitar los flujos gratuitos &quot;Inténtelo antes de comprar&quot;, para que pueda bloquear los datos que resulten más útiles para su organización antes de comprometerse con los precios del proveedor de datos.

Además, para ayudarle a investigar y decidir qué proveedor de datos utilizar, un recurso bueno es [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/).

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## Identifique o cree un usuario ideal (conversión) [!UICONTROL trait] o [!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

¿Qué intenta hacer que la gente haga en su sitio? ¿Cuál es su evento de conversión? Por supuesto, hay muchas respuestas diferentes a esta pregunta, según el tipo de sitio o el vertical y los objetivos de la organización. En cualquier caso, es común en AAM crear un [!UICONTROL trait] para visitantes que hayan cumplido esos criterios.

En el siguiente video, le mostraré cómo crear una conversión [!UICONTROL trait], que querrá tener en su lugar a medida que continúe con este tutorial y cree un [!UICONTROL model] similar.

Además, al usar eventos de Adobe Analytics para crear [!UICONTROL traits], hay una importante gotcha que debe tener en cuenta, de modo que no recopile más usuarios de los que debiera en el [!UICONTROL trait]. Vea el siguiente video para ver la gran revelación. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**NOTA:** En el vídeo anterior, el ejemplo que muestro supone que tienes Adobe Analytics. Evidentemente, tal vez no sea así. Si tiene Google Analytics (GA), tenemos un módulo que puede utilizar para enviar datos a AAM (consulte la [documentación](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)), y si la actividad de conversión en su sitio es enviada a AAM por GA, puede crear la conversión [!UICONTROL trait] a partir de eso. Si tiene una solución de análisis diferente (o no tiene una solución de análisis), puede enviar datos a AAM mediante nuestro código de DIL y la función `submit`, etc. (consulte la [documentación](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)). A continuación, cree la conversión [!UICONTROL trait] en función de los datos enviados cuando se realice la actividad de conversión en el sitio.

## Crear un [!UICONTROL Model] similar a partir de [!UICONTROL Second Party] o [!UICONTROL Third Party] datos {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

Después de completar los pasos anteriores, ya estamos listos para crear un algoritmo (similar) [!UICONTROL Model]. Mientras configuramos el [!UICONTROL model], usaremos la conversión [!UICONTROL trait] como base [!UICONTROL trait] (visitantes clave a los que queremos hacer duplicados), y usaremos el flujo de datos [!UICONTROL third party] habilitado como grupo de personas del cual extraer.

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## Una Práctica Recomendada Importante {#an-important-best-practice}

Al crear el [!UICONTROL model] algorítmico en Audience Manager, obviamente queremos que el [!UICONTROL model] sea lo más efectivo posible. Como [!UICONTROL model] está considerando todo el [!UICONTROL traits] del que forman parte los miembros de su base [!UICONTROL trait]/[!UICONTROL segment], no ayuda a [!UICONTROL model] si TODAS las personas están en un [!UICONTROL trait]/[!UICONTROL segment]. Por lo tanto, si tiene algún [!UICONTROL traits] supergenérico (como todos los que fueron a su sitio, o todos los que recibieron publicidad de usted, etc.), asegúrese de que el [!UICONTROL data source] al que pertenecen NO esté incluido en el [!UICONTROL data sources] de su [!UICONTROL model]. En el caso de uso de este artículo, es poco probable que lo haga, ya que nos centramos en mirar [!UICONTROL third party] datos para nuestros nuevos &quot;look-alikes&quot;, pero vale la pena mencionarlos de todos modos y se aplica a TODOS los [!UICONTROL models] algorítmicos.

## Creación de un algoritmo [!UICONTROL Trait] {#creating-an-algorithmic-trait}

Luego, tendremos que crear un algoritmo [!UICONTROL Trait] para poder utilizar los resultados de [!UICONTROL model]. Sin crear un [!UICONTROL trait], el modelo es inútil. Por lo tanto, después de ejecutar [!UICONTROL model], asegúrese de entrar en el cuadro de diálogo [!UICONTROL trait] y crear un algoritmo [!UICONTROL Trait]. El siguiente vídeo lo recorre y muestra un par de consejos.

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## Crear un [!UICONTROL Segment] a partir de los [!UICONTROL Model] datos y enviarlo a DSP {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

Una vez que haya creado un [!UICONTROL Trait] algorítmico, puede crear un nuevo [!UICONTROL segment] para colocarlo, de modo que pueda activar los datos (no puede activar un [!UICONTROL trait], sino crear un nuevo [!UICONTROL trait] [!UICONTROL segment] con el Algoritmo [!UICONTROL Trait], para que pueda activar (utilizar) el [!UICONTROL segment]).

Una vez que haya creado un [!UICONTROL segment] a partir de este [!UICONTROL trait] algoritmo, tendrá una audiencia de clientes potenciales que parezcan personas que ya se han convertido en su sitio. Ahora puede asignar esto [!UICONTROL segment] a cualquiera de sus DSP [!UICONTROL destinations] en Audience Manager. Podrá destinatario su mercadotecnia a esos &quot;look-alikes&quot;, que tienen más probabilidades de convertirse en su sitio que sólo en el público normal, lo que aumenta el retorno de inversión en publicidad. ¡Buena suerte!
