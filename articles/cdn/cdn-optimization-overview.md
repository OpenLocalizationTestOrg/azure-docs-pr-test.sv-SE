---
title: "aaaOptimize Azure content delivery för ditt scenario"
description: "Hur toooptimize leverans av ditt innehåll för specifika scenarier"
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: v-semcev
ms.openlocfilehash: 922a92fdbf7e6e21f2b5ae9a2fb4ac32735fc15a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-azure-content-delivery-for-your-scenario"></a>Optimera Azure content delivery för ditt scenario

När du levererar innehåll tooa stor global målgrupp är kritiska tooensure hello optimerade överföringen av innehållet. hello Azure innehåll Delivery Network kan optimera hello leverans miljö baserat på hello typ av innehåll som du har. Innehållet kan vara en webbplats, en direktsänd dataström, en video eller en stor fil för hämtning. När du skapar en slutpunkt för content delivery network (CDN) måste du ange ett scenario i hello **optimerade för** alternativet. Ditt val avgör vilka optimering är tillämpade toohello innehåll från hello CDN-slutpunkten.

Optimering alternativ är utformad toouse metodtips beteenden tooimprove innehållsleverans prestanda och bättre ursprung-avlastning. Dina val för scenariot påverkar prestanda genom att ändra konfigurationer för partiellt cachelagring, objektet högoptimerat och hello ursprung fel i principen. 

Den här artikeln innehåller en översikt över olika optimering funktioner och när du ska använda dem. Mer information om funktioner och begränsningar finns i hello respektive artiklar på varje typ av enskilda optimering.

> [!NOTE]
> Din **optimerade för** alternativen kan variera beroende på hello-provider som du väljer. CDN-providers gäller förbättring på olika sätt beroende på hello scenario. 

## <a name="provider-options"></a>Providern alternativ

hello Azure Content Delivery Network från Akamai stöder:

* Allmän web leverans 

* Allmän direktuppspelning

* Video-on-demand-direktuppspelning

* Hämtning av stora filer

* Dynamiska acceleration 

hello Azure Content Delivery Network från Verizon stöder endast Internet-leverans. Det kan användas för video på begäran och hämtning av stora filer. Du har inte tooselect en typ av optimering.

Vi rekommenderar starkt att du testar variationer mellan olika providrar tooselect hello optimala provider för din leverans.

## <a name="select-and-configure-optimization-types"></a>Välj och konfigurera optimering typer

toocreate en ny slutpunkt, Välj en optimering-typ som bäst passar hello scenario och typ av innehåll som du vill hello endpoint toodeliver. **Allmän webben** är hello standardvalet. Du kan uppdatera hello optimeringsalternativ för alla befintliga Akamai slutpunkten när som helst. Den här ändringen avbryta inte leverans från hello CDN. 

1. Välj en slutpunkt i en profil för Standard Akamai.

    ![Val av slutpunkten ](./media/cdn-optimization-overview/01_Akamai.png)

2. Under **inställningar**väljer **optimering**. Välj en typ från hello **optimerade för** listrutan.

    ![Optimering och typ](./media/cdn-optimization-overview/02_Select.png)

## <a name="optimization-for-specific-scenarios"></a>Optimering för specifika scenarier

Du kan optimera hello CDN-slutpunkten för en av följande scenarier hello. 

### <a name="general-web-delivery"></a>Allmän web leverans

Allmän webben är hello vanligaste optimeringsalternativ. Den är avsedd för allmän web content optimering, till exempel webbplatser och webbprogram. Denna optimering kan också användas för filen och hämtar video.

En typisk webbplats innehåller statiska och dynamiska innehåll. Statiskt innehåll innehåller bilder, JavaScript-bibliotek och formatmallar som kan cachelagras och leverera toodifferent användare. Dynamiskt innehåll anpassas för en enskild användare, t.ex. artiklar som är skräddarsydda tooa användarprofil. Dynamiskt innehåll cachelagras inte eftersom den är unik tooeach användare, till exempel handla kundvagn innehållet. Allmän web leverans kan optimera hela webbplatsen. 

> [!NOTE]
> Om du använder hello Azure Content Delivery Network från Akamai kanske du vill toouse denna optimering om genomsnittliga filstorleken är mindre än 10 MB. Om din Genomsnittlig filstorlek är större än 10 MB väljer **stora Filhämtning** från hello **optimerade för** listrutan.

### <a name="general-media-streaming"></a>Allmän direktuppspelning

Om du behöver toouse hello slutpunkten för direktsänd strömning och video-on-demand strömning rekommenderar vi Allmänt direktuppspelning optimering.

Direktuppspelning är skiftlägeskänslig, tid eftersom paket som anländer sent på hello klient kan orsaka en försämrad visa upplevelse, till exempel ofta buffring av videoinnehåll. Direktuppspelning optimering minskar hello svarstiden för leverans av innehåll för medier och ger en smooth streaming-upplevelse för användare. 

Det här scenariot är gemensamt för Azure media-kunder. När du använder Azure media services kan få du en strömmande slutpunkt som kan användas för strömning live och på begäran. Med det här scenariot behöver kunder inte tooswitch tooanother slutpunkt när de ändrar från strömning live tooon-begäran. Allmän media strömmande optimering har stöd för den här typen av scenario.

hello Azure innehåll Delivery Network från Verizon använder hello allmänna leverans optimering typen toodeliver strömmande media webbinnehåll.

toolearn mer om direktuppspelning optimering, se [direktuppspelning optimering](cdn-media-streaming-optimization.md).

### <a name="video-on-demand-media-streaming"></a>Video-on-demand-direktuppspelning

Video-on-demand media strömmande optimering förbättrar video-on-demand liveströmmat innehåll. Om du använder en slutpunkt för video-on-demand strömning kanske du vill toouse det här alternativet.

hello Azure innehåll Delivery Network från Verizon använder hello allmänna leverans optimering typen toodeliver strömmande media webbinnehåll.

toolearn mer om direktuppspelning optimering, se [direktuppspelning optimering](cdn-media-streaming-optimization.md).

> [!NOTE]
> Om hello slutpunkt har främst video-on-demand-innehåll, Använd den här typen av optimering. hello största skillnaden mellan denna optimering och hello allmänna media strömmande optimering är hello gör timeout-värde. hello timeout är mycket kortare toowork med direktsänd strömning scenarier.

### <a name="large-file-download"></a>Hämtning av stora filer

Om du använder hello Azure Content Delivery Network från Akamai, måste du använda en stor fil download toodeliver filer som är större än 1,8 GB. hello Azure Content Delivery Network från Verizon har inte en begränsning på filen filstorlek i dess allmänna web leveransoptimering.

Om du använder hello Azure innehåll Delivery Network från Akamai är hämtning av stora filer optimerade för innehåll som är större än 10 MB. Om din genomsnittlig storlek är mindre än 10 MB, kanske du vill toouse allmänna web leverans. Om din Genomsnittlig filstorlek är konsekvent större än 10 MB, kan det vara effektivare toocreate en separat slutpunkt för stora filer. Inbyggd programvara eller programuppdateringar normalt exempelvis stora filer.

hello Azure innehåll Delivery Network från Verizon använder hello allmänna leverans optimering typen toodeliver strömmande media webbinnehåll.

toolearn mer om optimering av stora filer, se [optimering för stora filer](cdn-large-file-optimization.md).

### <a name="dynamic-site-acceleration"></a>Dynamiska acceleration

 Dynamiska acceleration är tillgänglig från både Akamai och Verizon innehållsleveransnätverk profiler. Denna optimering innebär en avgift toouse. Mer information finns i hello sida med priser.

Dynamiska acceleration innehåller olika tekniker som omfattas hello svarstid och prestanda för dynamiskt innehåll. Tekniken omfattar flödes- och optimering, optimering för TCP med mera. 

Du kan använda den här optimering tooaccelerate en webbapp som innehåller flera svar som inte är Cacheable ställs. Exempel är sökresultat, utcheckningen transaktioner eller realtidsdata. Du kan fortsätta toouse CDN cache grundfunktionerna för statiska data. 



