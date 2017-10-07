---
title: aaaCloud Cruiser och Microsoft Azure Billing API-integrering | Microsoft Docs
description: "Ger ett unikt perspektiv från Microsoft Azure Billing partner molnet Cruiser på deras upplevelser integrera hello Azure Billing API: erna i sin produkt.  Detta är särskilt användbart för Azure och molnet Cruiser kunder som vill använda/försök molnet Cruiser för Microsoft Azure-paket."
services: 
documentationcenter: 
author: BryanLa
manager: tonguyen
editor: 
tags: billing
ms.assetid: b65128cf-5d4d-4cbd-b81e-d3dceab44271
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 02/03/2017
ms.author: mobandyo;sirishap;bryanla
ms.openlocfilehash: 74cc19bdeed26c6684210736caa0cb365e8f8821
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-cruiser-and-microsoft-azure-billing-api-integration"></a>Molnet Cruiser och Microsoft Azure Billing API-integrering
Den här artikeln beskriver hur hello information som samlas in från hello nya Microsoft Azure Billing API: er kan användas i molnet Cruiser för arbetsflödet kostnad simulering och analys.

## <a name="azure-ratecard-api"></a>Azure RateCard API
Hej RateCard API ger hastighet information från Azure. När autentisering med rätt autentiseringsuppgifter för hello, kan du fråga hello API toocollect metadata om hello-tjänster som är tillgängliga på Azure, tillsammans med hello priser som är associerade med ditt som erbjuder-ID.

hello följande är ett exempelsvar från hello-API som visar hello priser för hello A0 (Windows) instans:

    {
        "MeterId": "0e59ad56-03e5-4c3d-90d4-6670874d7e29",
        "MeterName": "Compute Hours",
        "MeterCategory": "Virtual Machines",
        "MeterSubCategory": "A0 VM (Windows)",
        "Unit": "Hours",
        "MeterRates":
        {
            "0": 0.029
        },
        "EffectiveDate": "2014-08-01T00:00:00Z",
        "IncludedQuantity": 0.0,
        "MeterStatus": "Active"
    },

### <a name="cloud-cruisers-interface-tooazure-ratecard-api"></a>Molnet Cruisers gränssnittet tooAzure RateCard API
Molnet Cruiser kan utnyttja hello RateCard API information på olika sätt. I den här artikeln visar vi hur det kan vara används toomake IaaS arbetsbelastning kostnaden simulering och analys.

toodemonstrate detta användningsfall, anta en arbetsbelastning med flera instanser som körs på Microsoft Azure-paket. hello målet är toosimulate samma arbetsbelastningen på Azure och uppskattning hello kostnader för att göra sådana migrering. Ordna toocreate simuleringen finns det två huvuduppgifter toobe utföras:

1. **Importera och processen hello tjänstinformation som samlats in från hello RateCard API.** Den här uppgiften utförs också på hello arbetsböcker, där hello utdrag ur hello RateCard API är transformerad och publicerade tooa nya abonnemang. Den här nya abonnemang ska användas på hello simulering tooestimate hello Azure priser.
2. **Normalisera WAP-tjänster och Azure IaaS-tjänster.** Som standard WAP tjänster baseras på enskilda resurser (CPU, minne, diskens storlek osv.) medan Azure services baseras på instansstorleken (A0, A1, A2 osv.). Den här första uppgiften kan utföras av molnet Cruiser ETL-motorn, kallas arbetsböcker, där resurserna kan ligga på instansen storlekar och liknande tooAzure instans tjänster.

### <a name="import-data-from-hello-ratecard-api"></a>Importera data från hello RateCard API
Molnet Cruiser arbetsböcker innehåller en automatiserad metod för toocollect och bearbeta information från hello RateCard API.  ETL (extract-transform-load) arbetsböcker kan du tooconfigure hello insamling, omvandling och publicering av data i hello molnet Cruiser databas.

Varje arbetsbok kan ha en eller flera samlingar, så att du toocorrelate information från olika källor toocomplement eller utöka hello användningsdata. följande två skärmdumpar visar hur hello toocreate en ny *samling* i en befintlig arbetsbok och importera information till hello *samling* från hello RateCard API:

![Bild 1 - skapa en ny samling][1]

![Bild 2 - importera data från hello ny samling][2]

När du importerar hello data i arbetsboken hello, det är möjligt toocreate flera steg och processer för omvandling, toomodify och modellen hello data. I det här exemplet eftersom det bara är intresserad av infrastruktur-som-en-tjänst (IaaS) vi kan använda hello omvandling steg tooremove onödiga rader eller poster som är relaterade tooservices än IaaS.

hello följande skärmbild visar hello omvandling steg används tooprocess hello data som samlas in från RateCard API:

![Bild 3 - Transformation steg tooprocess insamlade data från RateCard API: et][3]

### <a name="defining-new-services-and-rate-plans"></a>Definiera nya tjänster och hastighet planer
Det finns olika sätt toodefine tjänster på molnet Cruiser. En av hello alternativ är tooimport hello tjänster från hello användningsdata. Den här metoden används ofta när du arbetar med offentliga moln, där hello services redan har definierats av hello-providern.

Planera en hastighet är en uppsättning priser eller priser som kan vara tillämpade toodifferent tjänster som bygger på giltighetsdatum eller grupp av kunder, bland andra alternativ. Priser kan även användas på molnet Cruiser toocreate simuleringen eller ”vad händer om”-scenarier, toounderstand hur förändringar i tjänster kan påverka hello totalkostnaden för en arbetsbelastning.

I det här exemplet använder vi hello tjänstinformation från hello RateCard API toodefine nya tjänster i molnet Cruiser. Hello samma sätt, vi kan använda hello priser som är kopplade toohello services toocreate en ny hastighet planera på molnet Cruiser.

Hello slutet av hello omvandling av processen, det är möjligt toocreate ett nytt steg och publicera hello data från hello RateCard API som nya tjänster och priser.

![Bild 4 - publicera hello data från hello RateCard API som nya tjänster och priser][4]

### <a name="verify-azure-services-and-rates"></a>Verifiera Azure-tjänster och priser
När du har publicerat hello tjänster och priser, kan du kontrollera hello listan över importerade tjänster i molnet Cruiser *Services* fliken:

![Bild 5 – verifiera hello nya tjänster][5]

På hello *hastighet planer* fliken kan du hello nya hastighet plan som heter ”AzureSimulation” med hello priser importeras från hello RateCard API.

![Bild 6 - verifierar hello nya priser och associerade priser][6]

### <a name="normalize-wap-and-azure-services"></a>Normalisera WAP och Azure-tjänster
Som standard innehåller WAP användningsinformation baserat på hello användningen av beräkningar, minne och nätverksresurser. Du kan definiera dina tjänster som baseras direkt på hello allokering eller förbrukade användning av dessa resurser i molnet Cruiser. Exempelvis kan du ange en grundläggande hastighet för varje timme av CPU-användning eller kostnad hello GB minne som allokerats tooan instans.

I det här exemplet i kostnader för toocompare mellan WAP och Azure, behöver vi tooaggregate hello resursanvändning på WAP i paket, som sedan kan mappade tooAzure tjänster. Den här omvandlingen kan enkelt implementeras i hello arbetsböcker:

![Figur 7 – omvandla WAP toonormalize datatjänster][7]

hello sista steget i hello arbetsboken är toopublish hello data till hello molnet Cruiser databas. Det här steget hello användningsdata nu finns i tjänster (som mappar toohello Azure-tjänster) och knuten toodefault priser toocreate hello avgifter.

Du kan automatisera hello bearbetning av hello data genom att lägga till en aktivitet på hello Schemaläggaren och ange hello frekvens och tidpunkt för hello arbetsboken toorun efter färdigställa hello arbetsboken.

![Figur 8 - arbetsbok schemaläggning][8]

### <a name="create-reports-for-workload-cost-simulation-analysis"></a>Skapa rapporter för arbetsbelastningen kostnad simuleringen analys
När hello användningsinformation som samlas in och avgifter läses in i hello molnet Cruiser databasen, kan vi utnyttja hello molnet Cruiser insikter modulen toocreate hello arbetsbelastning kostnaden simuleringen som vi vill ha.

I ordning toodemonstrate i det här scenariot vi har skapat hello följande rapport:

![Jämförelse av kostnader][9]

hello översta diagrammet visar en kostnadsjämförelse av tjänster, jämföra hello priset för hello arbetsbelastning för varje specifik tjänst mellan WAP (mörkt blå) och Azure (lätta blå).

hello ned diagrammet visar hello samma data men uppdelade efter avdelning. Detta visar hello kostnader för varje avdelning toorun arbetsbelastningen på WAP och Azure, tillsammans med hello skillnaden mellan dem i hello besparingar-fältet (grön).

## <a name="azure-usage-api"></a>Användning av Azure API
### <a name="introduction"></a>Introduktion
Microsoft introducerade nyligen hello Azure användning API tillåta prenumeranter tooprogrammatically pull i användning data toogain insikter om deras användning. Det här är goda nyheter för molnet Cruiser kunder som kan dra nytta av hello rikare dataset som är tillgängliga via detta API.

Molnet Cruiser kan utnyttja hello integrering med hello API för användning på flera sätt. Hej granularitet (varje timme användningsinformation) och metadata resursinformation tillgängliga via hello API ger hello nödvändiga dataset toosupport flexibla Showback eller återbetalning modeller. 

I den här kursen presenterar vi ett exempel på hur molnet Cruiser kan dra nytta av hello användning API-information. Mer specifikt kommer vi skapa en resursgrupp i Azure, associera taggar för hello-konto och beskriver hello processen att dra och bearbetning av hello tagginformation till molnet Cruiser.

hello slutliga målet är toobe kan toocreate rapporter som hello efter och vara kan tooanalyze kostnad och förbrukning baserat på hello strukturen innehåller hello taggar.

![Bild 10 - rapport med uppdelning med hjälp av taggar][10]

### <a name="microsoft-azure-tags"></a>Microsoft Azure-taggar
hello-data som är tillgängliga via hello Azure användning API innehåller information om förbrukningen utan även resursmetadata, inklusive alla taggar som är kopplade till den. Taggar ger ett enkelt sätt tooorganize dina resurser, men i ordning toobe effektivt, behöver du tooensure som:

* Taggar är korrekt tillämpade toohello resurser etablera som helst
* Taggar används korrekt på hello Showback/förbrukning processen tootie hello användning toohello organisations kontostruktur.

Båda dessa villkor kan vara svårt, särskilt när det är en manuell process på hello etablera eller ladda sida. Felstavat, felaktigt eller också saknas taggar är vanligt klagomål från kunder när med taggar och dessa fel kan underlätta på hello debitering sida är väldigt svårt.

Med hello nya API för användning av Azure, molnet Cruiser kan ta resursen taggning information och via avancerade ETL verktyget arbetsböcker, korrigera felen för vanliga märkning. Genom omvandling med reguljära uttryck och data korrelation, molnet Cruiser identifiera felaktigt formaterad resurser och tillämpa hello rätt taggar, säkerställer hello rätt associering av hello resurser med hello konsumenten.

Hello debitering sida, molnet Cruiser automatiserar hello Showback/förbrukning processen och kan utnyttja hello taggen information tootie hello användning toohello lämplig konsumenten (avdelning, Division, projekt, etc.). Denna automatisering ger en enorm förbättring och säkerställa en konsekvent och granskningsbara laddas process.

### <a name="creating-a-resource-group-with-tags-on-microsoft-azure"></a>Skapa en resursgrupp med taggar i Microsoft Azure
hello första steget i den här kursen är toocreate en resursgrupp i hello Azure-portalen och sedan skapa nya etiketter tooassociate toohello resurser. I det här exemplet ska vi skapa hello följande taggar: avdelning, miljö, ägare, projekt.

hello skärmbilden nedan visar ett exempel resursgrupp med hello associerade taggar.

![Figur 11 - resursgrupp med tillhörande taggar på Azure-portalen][11]

hello nästa steg är toopull hello information från hello API för användning i molnet Cruiser. hello användning API tillhandahåller för närvarande svar i JSON-format. Här är ett exempel på hello data som hämtats:

    {
      "id": "/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXX/providers/Microsoft.Commerce/UsageAggregates/Daily_BRSDT_20150623_0000",
      "name": "Daily_BRSDT_20150623_0000",
      "type": "Microsoft.Commerce/UsageAggregate",
      "properties":
      {
        "subscriptionId": "bb678b04-0e48-4b44-XXXX-XXXXXXXXX",
        "usageStartTime": "2015-06-22T00:00:00+00:00",
        "usageEndTime": "2015-06-23T00:00:00+00:00",
        "meterName": "Compute Hours",
        "meterRegion": "",
        "meterCategory": "Virtual Machines",
        "meterSubCategory": "Standard_D1 VM (Non-Windows)",
        "unit": "Hours",
        "instanceData": "{\"Microsoft.Resources\":{\"resourceUri\":\"/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXXX/resourceGroups/DEMOUSAGEAPI/providers/Microsoft.Compute/virtualMachines/MyDockerVM\",\"location\":\"eastus\",\"tags\":{\"Department\":\"Sales\",\"Project\":\"Demo Usage API\",\"Environment\":\"Test\",\"Owner\":\"RSE\"},\"additionalInfo\":{\"ImageType\":\"Canonical\",\"ServiceType\":\"Standard_D1\"}}}",
        "meterId": "e60caf26-9ba0-413d-a422-6141f58081d6",
        "infoFields": {},
        "quantity": 8

      },
    },


### <a name="import-data-from-hello-usage-api-into-cloud-cruiser"></a>Importera data från hello användning API till molnet Cruiser
Molnet Cruiser arbetsböcker innehåller en automatiserad metod för toocollect och bearbeta information från hello användning API. En arbetsbok för ETL (extract-transform-load) kan du tooconfigure hello insamling, omvandling och publicering av data i hello molnet Cruiser databas.

Varje arbetsbok kan ha en eller flera samlingar. Detta ger dig toocorrelate information från olika källor toocomplement eller utöka hello användningsdata. I det här exemplet skapar vi ett nytt blad i arbetsboken för hello Azure-mall (*UsageAPI)* och ange en ny *samling* tooimport information från hello användning API.

![Bild 3 - användningsdata API som importeras till hello UsageAPI blad][12]

Observera att den här arbetsboken redan har andra ark tooimport tjänster från Azure (*ImportServices*), och bearbeta hello förbrukning information från hello fakturering API (*PublishData*).

Nu kommer vi att använda hello användning API toopopulate hello *UsageAPI* blad och korrelera hello information med hello förbrukningsdata från hello fakturering API på hello *PublishData* blad.

### <a name="processing-hello-tag-information-from-hello-usage-api"></a>Bearbetning av hello tagginformation från hello användning API
När du har importerat hello data i arbetsboken hello vi kommer att skapa omvandling av stegen i hello *UsageAPI* blad i ordning tooprocess hello information från hello API. Första steget är toouse en ”JSON dela” processor tooextract hello taggar från ett enda fält och sedan skapa fält för var och en av dem (avdelning, projekt, ägare och miljö).

![Bild 4 - skapa nya fält för hello tagginformation][13]

Meddelande hello ”nätverk” tjänsten saknar hello tagginformation (gul ruta), men vi kan verifiera att den är en del av hello samma resursgrupp genom att titta på hello *ResourceGroupName* fältet. Eftersom vi har hello taggar för hello andra resurser från den här resursgruppen kan använda vi den här informationen tooapply hello taggar toothis resursen senare i processen för hello saknas.

hello nästa steg är toocreate en sökning tabell associera hello information från hello taggar toohello *ResourceGroupName*. Den här uppslagstabellen ska användas på hello nästa steg tooenrich hello förbrukningsdata med tagginformation.

### <a name="adding-hello-tag-information-toohello-consumption-data"></a>Lägga till hello information toohello förbrukning taggdata
Nu kan vi gå toohello *PublishData* blad, vilka processer hello förbrukning information från hello fakturering API och lägga till hello fält som extraheras från hello taggar. Den här processen utförs genom att titta på hello uppslagstabell skapas på hello föregående steg, med hello *ResourceGroupName* som hello nyckel för hello sökningar.

![Bild 5 - fylla hello kontostruktur med hello information från hello-sökningar][14]

Observera att hello lämpligt konto struktur fält för hello ”nätverk” service har tillämpats, åtgärda hello problem med hello saknas taggar. Vi också fyllts hello struktur fälten för resurser än våra mål resursgrupp med ”andra”, i ordning toodifferentiate dem på hello rapporter.

Nu måste vi bara tooadd ett steg toopublish hello användningsdata. Under det här steget hello lämpliga priser för varje tjänst som definierats på våra priser inte tillämpade toohello användningsinformation med hello resulterande kostnad läses in i hello-databasen.

hello bästa delen är att du bara har toogo genom den här processen en gång. När hello arbetsboken har slutförts, behöver du bara tooadd den toohello scheduler och den kommer att köras varje timme eller varje dag på hello schemalagda tid. Är det bara gäller att skapa nya rapporter och anpassa befintliga, i ordning tooanalyze hello data tooget meningsfulla insikter från dina molnanvändning.

### <a name="next-steps"></a>Nästa steg
* För detaljerade anvisningar om hur du skapar moln Cruiser arbetsböcker och rapporter, se tooCloud Cruiser är online [dokumentationen](http://docs.cloudcruiser.com/) (giltig inloggning krävs).  Mer information om molnet Cruiser Kontakta [ info@cloudcruiser.com ](mailto:info@cloudcruiser.com).
* Se [få insikter om dina Microsoft Azure-resursförbrukning](billing-usage-rate-card-overview.md) en översikt över hello Azure Resursanvändning och RateCard APIs.
* Kolla in hello [Azure Billing REST API-referens](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) mer information om både API: er som är en del av hello uppsättning API: er som tillhandahålls av hello Azure Resource Manager.
* Om du vill toodive i hello exempelkod checka ut våra Microsoft Azure Billing API kodexempel på [Azure kodexempel](https://azure.microsoft.com/documentation/samples/?term=billing).

### <a name="learn-more"></a>Läs mer
* Se hello [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) artikel toolearn mer om hello Azure Resource Manager.

<!--Image references-->

[1]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Create-New-Workbook-Collection.png "Bild 1 - skapa en ny samling"
[2]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Import-Data-From-RateCard.png "Bild 2 - importera data från hello ny samling"
[3]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transformation-Steps-Process-RateCard-Data.png "Bild 3 - Transformation steg tooprocess insamlade data från RateCard API: et"
[4]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Publish-RateCard-Data-New-Services-Rates.png "Bild 4 - publicera hello data från hello RateCard API som nya tjänster och priser"
[5]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing1.png "Bild 5 – verifiera hello nya tjänster"
[6]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing2.png "Bild 6 - verifierar hello nya priser och associerade priser"
[7]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transforming-WAP-Normalize-Services.png "Figur 7 – omvandla WAP toonormalize datatjänster"
[8]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workbook-Scheduling.png "Figur 8 - arbetsbok schemaläggning"
[9]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workload-Cost-Simulation-Report.png "Bild 9 - exempelrapporten för hello arbetsbelastning kostnaden jämförelse scenario"
[10]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/1_ReportWithTags.png "Bild 10 - rapport med uppdelning med hjälp av taggar"
[11]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/2_ResourceGroupsWithTags.png "Figur 11 - resursgrupp med tillhörande taggar på Azure-portalen"
[12]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/3_ImportIntoUsageAPISheet.png "Figur 12 - användningsdata API som importeras till hello UsageAPI blad"
[13]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/4_NewTagField.png "Figur 13 - skapa nya fält för hello tagginformation"
[14]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/5_PopulateAccountStructure.png "Figur 14 - fylla hello kontostruktur med hello information från hello-sökningar"
