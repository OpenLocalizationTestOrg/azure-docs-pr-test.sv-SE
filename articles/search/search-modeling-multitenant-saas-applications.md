---
title: aaaModeling Multitenancy i Azure Search | Microsoft Docs
description: "Mer information om vanliga designmönster för multitenant SaaS-program när du använder Azure Search."
services: search
manager: jhubbard
author: ashmaka
documentationcenter: 
ms.assetid: 72e9696a-553b-47dc-9e05-a82db0ebf094
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/26/2016
ms.author: ashmaka
ms.openlocfilehash: dd46cda772d32566b9aaa18d407f12fdf178bd43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="design-patterns-for-multitenant-saas-applications-and-azure-search"></a>Designmönster för multitenant SaaS-program och Azure Search
Flera program är en som innehåller hello samma funktioner och tjänster tooany antalet klienter som inte kan se eller dela hello data med andra innehavare. Det här dokumentet beskrivs klient isolering strategier för flera program som skapats med Azure Search.

## <a name="azure-search-concepts"></a>Azure Search-begrepp
Som en sökning som en tjänst-lösning kan Azure Search utvecklare tooadd omfattande sökning får tooapplications utan hanterar all infrastruktur eller bli expert i sökningen. Data överförs toohello service och lagras sedan i hello molnet. Använda enkla begäranden toohello Azure Search API kan hello data sedan ändras och söka i. En översikt över hello-tjänsten finns i [i den här artikeln](http://aka.ms/whatisazsearch). Innan du diskutera designmönster, är det viktigt toounderstand några begrepp i Azure Search.

### <a name="search-services-indexes-fields-and-documents"></a>Search-tjänster, index, fält och dokument
När du använder Azure Search kan en prenumererar tooa *söktjänsten*. Eftersom data är överförda tooAzure sökning, den är lagrad i ett *index* inom hello search-tjänsten. Det kan finnas ett antal index i en enskild tjänst. toouse hello bekant begreppet databaser, hello search-tjänsten kan vara liknas tooa databas hello index i en tjänst kan vara liknas tootables inom en databas.

Varje index i en söktjänst har ett eget schema som definieras av ett antal anpassningsbara *fält*. Data läggs tooan Azure Search index i hello form av enskilda *dokument*. Varje dokument måste vara överförda tooa viss index och måste passa den indexeringsschema. När du söker efter data med Azure Search utfärdas hello fulltextsökning frågor mot ett visst index.  toocompare dessa begrepp toothose av en databas, fält kan liknas toocolumns i en tabell och dokument kan vara liknas toorows.

### <a name="scalability"></a>Skalbarhet
Alla Azure Search-tjänster i hello Standard [prisnivån](https://azure.microsoft.com/pricing/details/search/) kan skala i två dimensioner: lagring och tillgänglighet.

* *Partitioner* kan läggas till tooincrease hello lagring av en söktjänst.
* *Repliker* kan läggas till tooa service tooincrease hello genomflödet av förfrågningar som en söktjänst kan hantera.

Lägga till och ta bort partitioner och repliker vid tillåter hello kapacitet hello search-tjänsten toogrow med hello mängden data och trafik hello programbegäran. I ordning för en sökning service tooachieve Läs [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/), den kräver två repliker. I ordning för en tjänst tooachieve en skrivskyddad [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/), kräver tre repliker.

### <a name="service-and-index-limits-in-azure-search"></a>Tjänsten och index gränser i Azure Search
Det finns några olika [prisnivåer](https://azure.microsoft.com/pricing/details/search/) i Azure Search hello nivåer har olika [gränser och kvoter](search-limits-quotas-capacity.md). Vissa av dessa gränser finns på hello servicenivåer, vissa är på hello index-nivå och vissa är på hello partition-nivå.

|  | Basic | Standard1 | Standard2 | Standard3 | Standard3 HD |
| --- | --- | --- | --- | --- | --- |
| Maximal repliker per tjänst |3 |12 |12 |12 |12 |
| Maximal partitioner per tjänst |1 |12 |12 |12 |1 |
| Maximal Search-enheter (repliker * partitioner) per tjänst |3 |36 |36 |36 |36 (max 3 partitioner) |
| Maximal dokument per tjänst |1 miljon |180 miljoner |720 miljoner |1.4 miljarder |600 miljoner |
| Maximalt lagringsutrymme per tjänst |2 GB |300 GB |1,2 TB |2,4 TB |600 GB |
| Maximal dokument per Partition |1 miljon |15 miljoner |60 miljoner |120 miljoner |200 miljoner |
| Maximalt lagringsutrymme per Partition |2 GB |25 GB |100 GB |200 GB |200 GB |
| Maximal index per tjänst |5 |50 |200 |200 |3000 (max 1000 index/partition) |

#### <a name="s3-high-density"></a>S3 Hög densitet '
Det finns ett alternativ för hello hög densitet (HD) läge utformats speciellt för multitenant-scenarier i Azure Search S3 prisnivån. I många fall är det nödvändigt toosupport ett stort antal mindre klienter under en enda tooachieve hello fördelarna med enkelhet och kostnadsbesparingar.

S3 HD tillåter hello många små index toobe packade hello hanteras av en enda söktjänsten av handel hello möjlighet tooscale ut index med partitioner för hello möjlighet toohost fler index i en enskild tjänst.

S3-tjänsten kan concretely, ha mellan 1 och 200 index som tillsammans kan vara värd för too1.4 miljarder dokument. En S3 HD hello gör andra hand att enskilda index tooonly gå too1 miljoner dokument, men den kan hantera upp too1000 index per partition (upp too3000 per service) och det totala dokumentantal 200 miljoner per partition (in too600 miljoner per tjänst).

## <a name="considerations-for-multitenant-applications"></a>Överväganden för flera program
Flera program måste effektivt distribuera resurser mellan hello klienter samtidigt som någon form av sekretess mellan hello olika klienter. Det finns några överväganden vid utformning av hello arkitektur för ett sådant program:

* *Klientisolering:* programutvecklare behöver tootake lämpliga åtgärder tooensure att inga klienter har Ej behörig eller oönskad åtkomst toohello data för andra klienter. Utöver hello perspektiv datasekretess kräver klient isolering strategier effektiv hantering av delade resurser och skydd mot störningar grannar.
* *Molnet resurskostnader:* med alla andra program måste programlösningar förblir kostnaden konkurrenskraftiga som en del av ett flera program.
* *Enkel Operations:* när du utvecklar en arkitektur med flera hello inverkan på hello programmet operations och komplexitet är viktigt. Azure Search har en [SLA för 99,9%](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
* *Globala storleken:* flera program kan behöva tooeffectively fungera klienter som distribueras över hello jordglob.
* *Skalbarhet:* programutvecklare behöver tooconsider hur de stämmer mellan upprätthålla en tillräckligt låg nivå av programmet komplexitet och utformning av hello programmet tooscale med många klienter och hello storleken på klienternas data och arbetsbelastning.

Azure Search erbjuder några gränser som kan vara används tooisolate klienternas data och arbetsbelastning.

## <a name="modeling-multitenancy-with-azure-search"></a>Modeling multitenancy med Azure Search
I hello fallet på ett scenario med flera hello programutvecklaren förbrukar en eller flera search-tjänster och dela sina klienter bland tjänster, index eller båda. Azure Search har några vanliga mönster när modeling ett scenario med flera:

1. *Index per klient:* varje innehavare har sin egen index i en söktjänst som delas med andra klienter.
2. *Tjänsten per klient:* varje innehavare har en egen dedikerade Azure Search-tjänst ger högsta nivå uppdelning av data och arbetsbelastning.
3. *Blandning av båda:* större och mer aktiva klienter tilldelas dedikerade tjänster medan mindre klienter tilldelas enskilda index i delade tjänster.

## <a name="1-index-per-tenant"></a>1. Index per klient
![En bild av hello index per klient modell](./media/search-modeling-multitenant-saas-applications/azure-search-index-per-tenant.png)

I ett index per klient modellen anges flera innehavare på en enda Azure Search-tjänst där varje innehavare har sina egna index.

Klienter uppnå isolering av data eftersom alla söka begäranden och dokumentet operations utfärdas på indexnivå i Azure Search. I hello programnivå finns hello måste medvetenhet toodirect hello olika klienter trafik toohello rätt index och hanterar resurser på hello servicenivå över alla klienter.

Nyckelattribut hello index per klient modellen är hello möjligheten för hello programmet developer toooversubscribe hello kapacitet på en söktjänst bland hello programmets klienter. Om hello klienter har en ojämn fördelning av arbetsbelastning, hello optimal kombination av klienter kan fördelas på indexerar en söktjänst tooaccommodate ett antal mycket aktiv, resurskrävande klienter samtidigt samtidigt som man betjänar en lång slutet av mindre aktiva klienter. hello kompromiss är hello oförmåga hello modellen toohandle situationer där varje innehavare samtidigt är mycket aktiv.

hello index per klient modellen ger hello grund för en modell med rörlig kostnad, där en hela Azure Search-tjänsten köps direkta och därefter fylls med klienter. Detta ger outnyttjad kapacitet toobe avsedda för försök och kostnadsfria konton.

För program med en global storleken kanske inte hello index per klient modellen hello mest effektiva. Om klienter för ett program distribueras över hela världen hello, kan det vara nödvändigt för varje region som duplicera kostnader över dem en separat tjänst.

Azure Search kan hello skalan för både hello enskilda index och hello Totalt antal index toogrow. Om en lämplig priser valt nivån partitioner och repliker kan läggas till toohello hela söktjänsten när ett enskilt index i hello tjänsten blir för stor vad gäller lagring eller trafik.

Om hello Totalt antal index blir för stor för en enskild tjänst har en annan tjänst toobe etableras tooaccommodate hello nya klienter. Om index har toobe flyttas mellan search-tjänster som nya tjänster läggs, hello data från hello indexet har toobe som manuellt kopieras från ett index toohello andra som Azure Search inte tillåter ett index toobe flyttas.

## <a name="2-service-per-tenant"></a>2. Tjänsten per klient
![En bild av hello service per klient modell](./media/search-modeling-multitenant-saas-applications/azure-search-service-per-tenant.png)

Varje innehavare har sin egen söktjänsten i en arkitektur med tjänsten per klient.

I den här modellen uppnår hello programmet hello maximala nivån för isolering för sina klienter. Varje tjänst har särskilda lagrings- och genomströmning för hantering av sökbegäran samt separata API-nycklar.

För program där varje innehavare har ett stort utrymme eller hello arbetsbelastningen har lite variationen från innehavaren tootenant är hello service per klient modell ett effektivt val som resurser inte delas mellan olika klienternas arbetsbelastningar.

En tjänst per klient modell erbjuder också hello fördelen med en förutsägbar, fast kostnad-modell. Det finns inga direkta investeringar i ett hela söktjänsten tills det finns en klient toofill, men hello kostnad per klient är högre än en modell för index per klient.

hello-tjänsten per klient modellen är ett effektivt val för program med en global storleken. Med geografiskt spridd klienter, är det enkelt toohave tjänst för varje klient i hello aktuell region.

hello utmaningar skalningen av det här mönstret uppstå när enskilda klienter växa ifrån deras tjänst. Azure Search stöder för närvarande inte uppgradera hello prisnivån search-tjänsten så att alla data skulle ha toobe som manuellt kopieras tooa ny tjänst.

## <a name="3-mixing-both-models"></a>3. Blandning av båda modellerna
En annan mönster för modellering multitenancy blanda strategier för både index per klient och tjänstens per klient.

Blandas hello två mönster, upptar största klienter för ett program dedikerade tjänster medan hello länge arkivdel för mindre aktiva, mindre klienter får ha index i en delad tjänst. Den här modellen säkerställer att hello största hyresgäster konsekvent höga prestanda från hello-tjänsten medan tooprotect hello mindre klienter från alla störningar grannar.

Implementera den här strategin bygger dock förutseende förutsäga vilken klienter kräver en särskild service jämfört med ett index i en delad tjänst. Programmet komplexitet ökar med hello måste toomanage båda dessa multitenancy modeller.

## <a name="achieving-even-finer-granularity"></a>Uppnå även ökad detaljnivå
Hej ovanför design mönster toomodel flera scenarier i Azure Search förutsätter en enhetlig scopet där varje innehavare en hel instans av ett program. Program kan ibland hantera många mindre scope.

Om tjänsten per klient och index per klient modeller inte är tillräckligt små för scope är möjliga toomodel ett index tooachieve även finmaskig granularitet.

toohave ett index beter sig på olika sätt för olika klientslutpunkter, ett fält kan vara tillagda tooan index som anger ett visst värde för varje klient som möjligt. Varje gång en klient anropar Azure Search tooquery eller ändra ett index, hello kod från hello klientprogrammet anger hello lämpligt värde för fältet med Azure Search [filter](https://msdn.microsoft.com/library/azure/dn798921.aspx) kapaciteten när databasfrågan.

Den här metoden kan vara används tooachieve funktionerna i separata användarkonton, separat behörighetsnivåer och även helt separata program.

> [!NOTE]
> Med hello metoden ovan tooconfigure en enda index tooserve flera innehavare påverkar hello relevans sökresultat. Sök relevans poäng beräknas på ett index på objektnivå scope, inte ett klient-nivå scope, så alla klienter data ingår i hello relevans poäng underliggande statistik, till exempel termen frekvens.
> 
> 

## <a name="next-steps"></a>Nästa steg
Azure Search är en tvingande val för många program [Läs mer om hello service robusta funktionerna](http://aka.ms/whatisazsearch). När utvärderar hello olika designmönster för flera program bör du överväga att hello [olika prisnivåer](https://azure.microsoft.com/pricing/details/search/) och hello respektive [gränser](search-limits-quotas-capacity.md) toobest skräddarsy Azure Search toofit programbelastningar och arkitekturer i alla storlekar.

Frågor om Azure Search och flera scenarier kan dirigeras tooazuresearch_contact@microsoft.com.

