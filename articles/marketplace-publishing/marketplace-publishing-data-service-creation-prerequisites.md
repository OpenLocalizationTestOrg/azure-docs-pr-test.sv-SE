---
title: "aaaTechnical förutsättningar för att skapa en datatjänst för hello Marketplace | Microsoft Docs"
description: "Förstå hello kraven för att skapa en datatjänst toodeploy och sälja på hello Azure Marketplace"
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: aaff609a-1cd1-4146-98f4-d04166b0fce0
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 2bba4282473fed63c3fcab43043a97e179705844
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="technical-pre-requisites-for-creating-a-data-service-offer-for-hello-azure-marketplace"></a>Tekniska krav för att skapa en datatjänst erbjudande för hello Azure Marketplace
> [!IMPORTANT]
> **Just nu vi inte längre onboarding några nya Data Service-utgivare. Nya dataservices kommer inte godkännas för lista.** Om du har ett SaaS-affärsprogram som toopublish på AppSource hittar du mer information [här](https://appsource.microsoft.com/partners). Om du har en IaaS-program eller utvecklare tjänst du skulle t.ex toopublish på Azure Marketplace hittar du mer information [här](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

Läs hello processen noggrant innan du börjar och förstå varför och där varje steg utförs. Så mycket som möjligt du ska förbereda företagets information och andra data, hämta nödvändiga verktyg och skapa tekniska komponenter innan du börjar hello erbjudande skapas.

Du bör ha följande till hands innan du börjar hello hello:

## <a name="make-a-decision-on-what-technology-will-be-used-toopublish-your-data-service-offer"></a>Gör ett beslut om vilken teknik kommer att använda toopublish erbjudandet Data Service
En utgivare kan du välja mellan flera tekniker när du publicerar Data Service i Azure Marketplace. hello huvudsakliga tekniker som stöds beskrivs nedan. Oavsett vilken teknik används toopublish hello datatjänst, hello slutanvändarens förbrukar hello data via hello **OData-feeden** visas av Azure Marketplace-tjänsten. Fullständig information om OData-tjänst som du hittar på [http://www.odata.org/](http://www.odata.org/)

## <a name="sql-azure-database"></a>SQL Azure-databas
Med dataset redo i SQL Azure är Publisher ansvar. Du behöver toosubscribe tooAzure etablera lämpliga storleken på databasen och ladda upp Data till SQL Azure DB. Utgivaren är också ansvarig tookeep hans/hennes data alltid uppdaterad. Mer information om prenumerationer tooAzure tjänster hittar du på [https://azure.microsoft.com/services/sql-database/](https://azure.microsoft.com/services/sql-database/)

När du flyttar hello data till SQL Azure kan hello Azure Marketplace exponera tabeller och vyer. hello Publisher kan ange vilka tabeller/vyer och kolumner som är exponerade toohello slutanvändaren. Ytterligare hello innehållsleverantören kan också ange vilka kolumner som kan efterfrågas av hello slutanvändare och vilka som returneras endast i hello nyttolast. Detta ger en hög nivå av flexibilitet om vilka data i hello databas bör exponeras. Kolumner som kan efterfrågas måste toobe backas upp av en eller flera index i databasen.

## <a name="rest-based-web-service"></a>REST-baserad webbtjänst
Protokoll som stöds: **endast HTTPS**

Befintliga REST-baserade tjänster kan exponeras via hello Azure Marketplace. Eftersom hello dataset alltid är synliga toohello slutanvändaren som en OData-feed, hello Azure Marketplace-tjänsten måste toobe kan toomap hello service tooa baserat OData-tjänst. toodo så hello REST-baserad slutpunkter måste tooexpose alla parametrar som HTTP-parametrar.

hello nyttolasten måste toobe i ett formulär kan mappas till ett ATOM-svar. Därför hello svar från hello services måste toobe i XML-format och kan bara innehålla ett upprepande element som innehåller värden som hello nyttolasten (till exempel postuppsättning). hello Azure Marketplace-tjänsten ska mappa hello upprepande nod toohello post nod i ATOM och hello nyttolast-värden i egenskapen noder i hello post nod.

Auktoriseringsinformation (till exempel API nyckeln, autentisering token, osv.) måste toobe anges som en HTTP-parameter eller i hello HTTP-huvudet (nyckel/värde-par) – grundläggande autentisering stöds också. En giltig nyckel måste toobe tillhandahålls och alla förfrågningar via Azure Marketplace görs via nyckeln. Övervakning och fakturering sker på hello Azure Marketplace-nivån.

Fel som returneras av hello service måste toobe mappas till HTTP-statuskoder. Om hello-tjänsten returnerar en XML som innehåller hello fel kommer dessa toobe mappas av hello Azure Marketplace-tjänsten tooHTTP statuskoder.

## <a name="soap-based-web-services"></a>SOAP-baserat webbtjänsterna
Protokoll: **endast HTTPS**

hello kraven är hello samma som i hello REST-baserad tjänsteavsnitt. hello enda skillnaden är parametrar kan också anges i en XML-innehållet är att anslås toohello Publisher service med alla begäranden som görs via Azure Marketplace. Detta innebär att HTTP-parametrar hello användare ger dig tillgång till hello frontend översätts till XML-element i ett XML-dokument som skickas med hello-begäran-toohello innehåll provider-webbtjänsten.

## <a name="odata-based-web-services"></a>OData-baserat webbtjänsterna
Protokoll: **endast HTTPS**

Data kan visas som en OData-tjänsten tooAzure Marketplace. hello system är pågående toopass hello tjänsten via och ersätter hello roten för hello tjänsten med hello Azure Marketplace service rot – tooensure alla efterföljande anrop gå igenom Azure Marketplace.

OData-tjänster behöver inte bara toogo mot en databas i hello backend. OData stöder alla typer av lagring eller business logic toodrive hello-tjänsten.

## <a name="next-steps"></a>Nästa steg
Nu när du har granskat hello förutsättningar och slutfört hello nödvändiga uppgifter du kan gå vidare med hello skapar ditt Data tjänsterbjudande som detaljerad i hello [Data Service publiceringsguide](marketplace-publishing-data-service-creation.md).

Eller, om du vill att tooreview hello övergripande processer och hello respektive artiklar för varje hello publicering faser, går du till hello artikel [komma igång: hur toopublish ett erbjudande toohello Azure Marketplace](marketplace-publishing-getting-started.md).

[link-acct]:marketplace-publishing-accounts-creation-registration.md
