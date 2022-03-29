---
title: Aumentar ROAS mediante modelos algorítmicos (de similitud)
description: El verdadero poder del modelado de similitud de los Audience Manager se obtiene cuando se busca expandir la audiencia de línea de base con un conjunto de usuarios de segunda y tercera fuente de datos de calidad y totalmente nuevo. En este tutorial, aprenda los pasos para crear un modelo a partir de estos datos.
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 25188.jpg
kt: 1849
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6626ae11-8709-4302-9e03-0d55878d2409
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '909'
ht-degree: 0%

---

# Aumente ROAS utilizando modelos algorítmicos (de similitud) en el Audience Manager {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

El verdadero poder de la similitud del Audience Manager [!UICONTROL Modeling] se produce cuando se busca expandir la audiencia de línea de base con un conjunto de usuarios nuevo y de calidad procedentes de fuentes de datos de segundo nivel y de terceros. En este tutorial, aprenda los pasos necesarios para crear un modelo a partir de estos datos.

## Habilitar flujos de datos de segundo nivel o de terceros desde el Audience Marketplace {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

Para utilizar datos de segundo nivel y de terceros en un modelo de similitud, primero debemos habilitar estos datos en la interfaz de Audience Manager. Adobe tiene un gran número de proveedores de datos de segundo nivel y de terceros, de los que puede elegir. Están disponibles para usted en una interfaz de autoservicio en AAM, a través del Audience Marketplace . Vaya al Audience Marketplace y explore las posibilidades. En el siguiente vídeo se muestra cómo hacerlo, incluido cómo habilitar flujos gratuitos &quot;inténtelo antes de comprar&quot;, de modo que pueda bloquear los datos que resulten más útiles para su organización antes de comprometerse con los precios del proveedor de datos.

Además, para ayudarle a investigar y decidir qué proveedor de datos utilizar, un recurso bueno es el [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/).

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## Identificar o crear un rasgo o segmento (conversión) de usuario ideal {#identify-create-an-ideal-user-conversion-trait-or-segment}

¿Qué está intentando hacer que la gente haga en su sitio? ¿Cuál es su evento de conversión? Por supuesto, hay muchas respuestas diferentes a esta pregunta, según el tipo de sitio o vertical y los objetivos de la organización. En cualquier caso, es común en AAM crear un rasgo para los visitantes que han cumplido esos criterios.

En el siguiente vídeo, le mostraré cómo crear un rasgo de conversión, que querrá tener en su sitio mientras continúa con este tutorial y crea un modelo de similitud.

Además, al usar eventos de Adobe Analytics para crear características, hay una gotcha importante que debe tener en cuenta para que no recopile más usuarios de los que debería en la característica. Vea el siguiente vídeo para ver la gran revelación. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**NOTA:** En el vídeo anterior, el ejemplo que mostré supone que tiene Adobe Analytics. Obviamente, tal vez no sea así. Si tiene Google Analytics (GA), tenemos un módulo que puede utilizar para enviar datos a AAM (consulte la [documentación](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html)), y si la actividad de conversión de su sitio se envía a AAM por medio de GA, puede crear la característica de conversión a partir de eso. Si tiene una solución de análisis diferente (o no tiene ninguna solución de análisis), aún puede enviar datos a AAM mediante nuestro código de DIL y el `submit` función, etc. (consulte la [documentación](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html)). A continuación, cree el rasgo de conversión en función de los datos enviados cuando se realiza la actividad de conversión en el sitio.

## Creación de un modelo de similitud a partir de datos de segundo nivel o de terceros {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

Después de completar los pasos anteriores, ya estamos listos para crear un modelo algorítmico (de similitud). Al configurar el modelo, utilizaremos el rasgo de conversión como rasgo base (visitantes clave que queremos duplicar) y el flujo de datos de terceros habilitado como grupo de personas desde el que extraemos datos.

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## Una práctica recomendada importante {#an-important-best-practice}

Al crear el modelo algorítmico en Audience Manager, obviamente queremos que el modelo sea lo más efectivo posible. Como el modelo está considerando todos los rasgos de los que forman parte los miembros del rasgo o segmento base, no ayuda si TODAS las personas están en un rasgo o segmento. Por lo tanto, si tiene cualquier característica supergenérica (como todas las personas que visitaron su sitio, o todas las personas que han recibido publicidad suya, etc.), asegúrese de que la fuente de datos a la que pertenecen NO esté incluida en las fuentes de datos de su modelo. En el caso de uso de este artículo, es poco probable que lo haga, ya que nos centramos en mirar los datos de terceros para nuestros nuevos alias, pero vale la pena mencionarlos de todos modos y se aplica a TODOS sus modelos algorítmicos.

## Cree un [!UICONTROL Algorithmic Trait] {#creating-an-algorithmic-trait}

A continuación, se debe crear un  [!UICONTROL Algorithmic Trait], para que se puedan utilizar los resultados del modelo. Sin crear un rasgo, el modelo es inútil. Por lo tanto, después de ejecutar el modelo, asegúrese de entrar en el cuadro de diálogo de características y crear un [!UICONTROL Algorithmic Trait]. El siguiente vídeo lo recorre y muestra un par de consejos.

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## Cree un segmento a partir de los datos del modelo y envíelo a DSP {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

Una vez que haya creado un [!UICONTROL Algorithmic Trait], puede crear un segmento nuevo para incluirlo, de modo que pueda activar los datos (no puede activar un rasgo, sino crear un nuevo segmento de un rasgo con la variable [!UICONTROL Algorithmic Trait] , para poder activar (utilizar) el segmento).

Una vez que haya creado un segmento a partir de este rasgo algorítmico, tendrá una audiencia de clientes potenciales que se verán como personas que ya se han convertido en su sitio. Ahora puede asignar este segmento a cualquiera de sus destinos de DSP en Audience Manager. Podrá dirigir el marketing a esos alias, que tienen más probabilidades de generar conversiones en su sitio que solo el público normal, lo que aumenta el retorno del gasto en publicidad.
