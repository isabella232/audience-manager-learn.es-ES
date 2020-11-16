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


# Aumentar ROAS usando un algoritmo (similar) [!UICONTROL Models] en el Audience Manager {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

El verdadero poder de la apariencia de los Audience Manager [!UICONTROL Modeling] viene cuando se busca expandir su audiencia de base contra un conjunto de usuarios de calidad, totalmente nuevo, de [!UICONTROL second party] y [!UICONTROL third party][!UICONTROL data sources]. En este tutorial, aprenda los pasos necesarios para crear un archivo [!UICONTROL model] a partir de estos datos.

## Habilitar [!UICONTROL Second Party] o [!UICONTROL Third Party] flujos de datos desde el Audience Marketplace {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

Para poder utilizar [!UICONTROL second party] y [!UICONTROL third party] datos en una apariencia similar [!UICONTROL model], primero debemos habilitar estos datos en la interfaz de Audience Manager. Adobe cuenta con un gran número de proveedores [!UICONTROL second party] de datos y [!UICONTROL third party] de los que puede elegir. Están disponibles para usted en una interfaz de autoservicio en AAM, a través del Audience Marketplace. Navegue hasta el Audience Marketplace y explore las posibilidades. En el siguiente vídeo se muestra cómo hacerlo, incluida la forma de habilitar los flujos gratuitos &quot;Inténtelo antes de comprar&quot;, para que pueda bloquear los datos que resulten más útiles para su organización antes de comprometerse con los precios del proveedor de datos.

Además, para ayudarle a investigar y decidir qué proveedor de datos utilizar, un recurso bueno es el [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/).

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## Identifique o cree un usuario ideal (conversión) [!UICONTROL trait] o [!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

¿Qué intenta hacer que la gente haga en su sitio? ¿Cuál es su evento de conversión? Por supuesto, hay muchas respuestas diferentes a esta pregunta, según el tipo de sitio o el vertical y los objetivos de la organización. En cualquier caso, es común en AAM crear un [!UICONTROL trait] para visitantes que hayan cumplido esos criterios.

En el siguiente vídeo, le mostraré cómo crear una conversión [!UICONTROL trait], que querrá tener en su lugar a medida que continúe con este tutorial y cree una parecida [!UICONTROL model].

Además, al usar eventos de Adobe Analytics para crear [!UICONTROL traits], hay una importante gotcha que debe tener en cuenta, de modo que no recopile más usuarios de los que debería en el [!UICONTROL trait]. Vea el siguiente video para ver la gran revelación. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**NOTA:** En el vídeo de arriba, el ejemplo que muestro supone que tienes Adobe Analytics. Evidentemente, tal vez no sea así. Si tiene Google Analytics (GA), tenemos un módulo que puede utilizar para enviar datos a AAM (consulte la [documentación](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)), y si la actividad de conversión de su sitio es enviada a AAM por GA, puede crear la conversión [!UICONTROL trait] a partir de eso. Si tiene una solución de análisis diferente (o no tiene una solución de análisis), puede enviar datos a AAM mediante nuestro código de DIL y la `submit` función, etc. (consulte la [documentación](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)). A continuación, cree la conversión [!UICONTROL trait] en función de los datos enviados cuando se realice la actividad de conversión en el sitio.

## Crear un aspecto similar [!UICONTROL Model] a partir de [!UICONTROL Second Party] o de [!UICONTROL Third Party] datos {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

Después de completar los pasos anteriores, ya estamos listos para crear un algoritmo (similar) [!UICONTROL Model]. A medida que configuremos el [!UICONTROL model], usaremos la conversión [!UICONTROL trait] como base [!UICONTROL trait] (visitantes clave a los que queremos dar duplicados) y usaremos el flujo de [!UICONTROL third party] datos habilitado como grupo de personas del que extraer.

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## Una buena práctica importante {#an-important-best-practice}

Al crear el algoritmo [!UICONTROL model] en Audience Manager, obviamente queremos que el [!UICONTROL model] sea lo más efectivo posible. Como el [!UICONTROL model] está considerando todo lo [!UICONTROL traits] que los miembros de su base [!UICONTROL trait]/[!UICONTROL segment] son parte, no ayuda el [!UICONTROL model] si TODAS las personas están en una [!UICONTROL trait]/[!UICONTROL segment]. Por lo tanto, si tiene algún supergenérico [!UICONTROL traits] (como todos los que fueron a su sitio, o todos los que recibieron publicidad de usted, etc.), asegúrese de que el [!UICONTROL data source] que pertenecen NO esté incluido en el [!UICONTROL data sources] en su [!UICONTROL model]. En el caso de uso de este artículo, es poco probable que lo haga, ya que nos estamos centrando en mirar [!UICONTROL third party] los datos de nuestros nuevos &quot;look-alikes&quot;, pero vale la pena mencionarlos de todos modos y se aplica a TODOS los algoritmos [!UICONTROL models].

## Creación de un algoritmo [!UICONTROL Trait] {#creating-an-algorithmic-trait}

A continuación, tendremos que crear un algoritmo [!UICONTROL Trait]para poder utilizar los resultados del [!UICONTROL model] . Sin crear un [!UICONTROL trait]modelo, éste es inútil. Por lo tanto, después de la ejecución [!UICONTROL model] , asegúrese de entrar en el [!UICONTROL trait] cuadro de diálogo y crear un algoritmo [!UICONTROL Trait]. El siguiente vídeo lo recorre y muestra un par de consejos.

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## Creación [!UICONTROL Segment] de un archivo a partir de los [!UICONTROL Model] datos y envío a DSP {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

Una vez creado un algoritmo [!UICONTROL Trait], puede crear un nuevo [!UICONTROL segment] para incluirlo, de modo que pueda activar los datos (no puede activar un algoritmo [!UICONTROL trait], sino crear uno nuevo[!UICONTROL trait] [!UICONTROL segment] con el algoritmo [!UICONTROL Trait] para activar (utilizar) el [!UICONTROL segment]).

Una vez que haya creado un [!UICONTROL segment] a partir de este algoritmo [!UICONTROL trait], tendrá una audiencia de clientes potenciales que parezcan personas que ya se han convertido en su sitio. Ahora puede asignar esto [!UICONTROL segment] a cualquiera de sus DSP [!UICONTROL destinations] en Audience Manager. Podrá destinatario su mercadotecnia a esos &quot;look-alikes&quot;, que tienen más probabilidades de convertirse en su sitio que sólo en el público normal, lo que aumenta el retorno de inversión en publicidad. ¡Buena suerte!
