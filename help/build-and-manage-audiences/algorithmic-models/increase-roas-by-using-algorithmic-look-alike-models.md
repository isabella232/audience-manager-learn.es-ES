---
title: Aumente ROAS utilizando modelos algorítmicos (de similitud) en Audience Manager
description: La potencia real del Modelado de similitudes de Audience Manager se obtiene cuando se busca expandir la audiencia de línea de base con un conjunto de usuarios nuevo y de calidad de fuentes de datos de segundo nivel y de terceros. En este tutorial, aprenda los pasos para crear un modelo a partir de estos datos.
feature: Modelos algorítmicos
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 25188.jpg
kt: 1849
role: '"Profesional del negocio, Desarrollador, Ingeniero de datos, Arquitecto, Arquitecto de datos, Administrador, Líder"'
level: Intermedio
translation-type: tm+mt
source-git-commit: a7dc335e75697a7b1720eccdadbb9605fdeda798
workflow-type: tm+mt
source-wordcount: '873'
ht-degree: 0%

---


# Aumente el ROAS utilizando Algoritmo (similar) [!UICONTROL Models] en Audience Manager {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

El poder real de la similitud [!UICONTROL Modeling] de Audience Manager se obtiene cuando se busca expandir la audiencia de línea de base con un conjunto de usuarios de [!UICONTROL second party] y [!UICONTROL third party] [!UICONTROL data sources] de calidad y nueva marca. En este tutorial, aprenda los pasos necesarios para crear un [!UICONTROL model] a partir de estos datos.

## Habilitar [!UICONTROL Second Party] o [!UICONTROL Third Party] flujos de datos desde Audience Marketplace {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

Para utilizar datos [!UICONTROL second party] y [!UICONTROL third party] en un aspecto similar [!UICONTROL model], primero debemos habilitar estos datos en la interfaz de Audience Manager. Adobe tiene un gran número de proveedores de datos [!UICONTROL second party] y [!UICONTROL third party] de los que puede elegir. Están disponibles para usted en una interfaz de autoservicio en AAM, a través de Audience Marketplace. Vaya a Audience Marketplace y explore las posibilidades. En el siguiente vídeo se muestra cómo hacerlo, incluido cómo habilitar flujos gratuitos &quot;inténtelo antes de comprar&quot;, de modo que pueda bloquear los datos que resulten más útiles para su organización antes de comprometerse con los precios del proveedor de datos.

Además, para ayudarle a investigar y decidir qué proveedor de datos utilizar, un recurso excelente es [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/).

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## Identificar/Crear un usuario ideal (conversión) [!UICONTROL trait] o [!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

¿Qué está intentando hacer que la gente haga en su sitio? ¿Cuál es su evento de conversión? Por supuesto, hay muchas respuestas diferentes a esta pregunta, según el tipo de sitio o vertical y los objetivos de la organización. En cualquier caso, es común en AAM crear un [!UICONTROL trait] para los visitantes que han cumplido esos criterios.

En el siguiente vídeo, le mostraré cómo crear una conversión [!UICONTROL trait], que querrá tener en su lugar mientras continúa con este tutorial y crea un aspecto similar [!UICONTROL model].

Además, al usar eventos de Adobe Analytics para crear [!UICONTROL traits], debe tener en cuenta una importante gotcha, de modo que no recopile más usuarios de los que debiera en la [!UICONTROL trait]. Vea el siguiente vídeo para ver la gran revelación. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**NOTA:** En el vídeo anterior, el ejemplo que mostré supone que tiene Adobe Analytics. Obviamente, tal vez no sea así. Si tiene Google Analytics (GA), tenemos un módulo que puede utilizar para enviar datos a AAM (consulte la [documentación](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)) y, si la actividad de conversión de su sitio se envía a AAM mediante GA, puede crear la conversión [!UICONTROL trait] a partir de eso. Si tiene una solución de análisis diferente (o ninguna solución de análisis), aún puede enviar datos a AAM a través de nuestro código DIL y la función `submit` , etc. (consulte la [documentación](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)). A continuación, cree la conversión [!UICONTROL trait] en función de los datos enviados cuando se realiza la actividad de conversión en el sitio.

## Crear un [!UICONTROL Model] similar a partir de [!UICONTROL Second Party] o [!UICONTROL Third Party] datos {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

Después de completar los pasos anteriores, ya estamos listos para crear un algoritmo (parecido) [!UICONTROL Model]. Al configurar el [!UICONTROL model], utilizaremos la conversión [!UICONTROL trait] como nuestra base [!UICONTROL trait] (visitantes clave que queremos duplicar) y usaremos el flujo de datos habilitado [!UICONTROL third party] como nuestro grupo de personas del que extraer.

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## Una práctica recomendada importante {#an-important-best-practice}

Al crear el [!UICONTROL model] algorítmico en Audience Manager, obviamente queremos que el [!UICONTROL model] sea lo más efectivo posible. Como el [!UICONTROL model] está considerando todos los [!UICONTROL traits] de los que forman parte los miembros de su base [!UICONTROL trait]/[!UICONTROL segment], no ayuda al [!UICONTROL model] si TODAS las personas están en un [!UICONTROL trait]/[!UICONTROL segment]. Por lo tanto, si tiene algún [!UICONTROL traits] supergenérico (como todos los que fueron a su sitio, o todos los que recibieron publicidad de usted, etc.), asegúrese de que el [!UICONTROL data source] al que pertenecen NO esté incluido en el [!UICONTROL data sources] de su [!UICONTROL model]. En el caso de uso de este artículo, es poco probable que lo haga, ya que nos centramos en mirar los [!UICONTROL third party] datos de nuestros nuevos alias, pero vale la pena mencionarlos de todos modos, y se aplica a TODOS sus [!UICONTROL models] algorítmicos.

## Creación de un [!UICONTROL Trait] {#creating-an-algorithmic-trait} algorítmico

A continuación, tendremos que crear un [!UICONTROL Trait] algorítmico para poder utilizar los resultados del [!UICONTROL model]. Sin crear un [!UICONTROL trait], el modelo es inútil. Por lo tanto, después de ejecutar [!UICONTROL model], asegúrese de entrar en el cuadro de diálogo [!UICONTROL trait] y crear un [!UICONTROL Trait] algorítmico. El siguiente vídeo lo recorre y muestra un par de consejos.

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## Creación de un [!UICONTROL Segment] a partir de los [!UICONTROL Model] datos y envío a los DSP {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

Una vez creado un [!UICONTROL Trait] algorítmico, puede crear un [!UICONTROL segment] nuevo para introducirlo, de modo que pueda activar los datos (no puede activar un [!UICONTROL trait], sino crear un [!UICONTROL trait] [!UICONTROL segment] nuevo con el [!UICONTROL Trait] algorítmico para que pueda activar (utilizar) el [!UICONTROL segment]).

Una vez que haya creado un [!UICONTROL segment] a partir de este [!UICONTROL trait] algorítmico, tendrá una audiencia de clientes potenciales que parezcan personas que ya se han convertido en su sitio. Ahora puede asignar esto [!UICONTROL segment] a cualquiera de sus DSP [!UICONTROL destinations] en Audience Manager. Podrá dirigir el marketing a esos alias, que tienen más probabilidades de generar conversiones en su sitio que solo el público normal, lo que aumenta el retorno del gasto en publicidad. ¡Buena suerte!
