---
title: aaaAzure Fallstudie SQL Database Azure - Snelstart | Microsoft Docs
description: "Lär dig mer om hur SnelStart använder SQL-databas toorapidly expanderat dess företagstjänster med 1 000 nya Azure SQL-databaser per månad"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: fab506b2-439d-4f1a-bdc5-d1d25c80d267
ms.service: sql-database
ms.custom: reference
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 69572393f41a9baf44f41b4f5f4ebbef347de851
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="with-azure-snelstart-has-rapidly-expanded-its-business-services-at-a-rate-of-1000-new-azure-sql-databases-per-month"></a>Med Azure, har SnelStart snabbt utökats dess företagstjänster med 1 000 nya Azure SQL-databaser per månad
![SnelStartLogo](./media/sql-database-implementation-snelstart/snelstartlogo.png)

SnelStart gör populära finansiella - och business-hanteringsprogramvara för små och medelstora företag (SMB) i hello Nederländerna. Kunderna 55,000 underhålls av en avdelning för 110 anställda, inklusive en IT-personal 35. Genom att flytta från programvara för skrivbordet tooa programvara som en-tjänst (SaaS) erbjudande som bygger på Azure SnelStart gjorts hello mest av inbyggda tjänster, automatisera hanteringen med välbekant miljö i C# och optimera prestanda och skalbarhet genom att varken över - eller under etablering företag med elastiska pooler. Med Azure ger SnelStart hello flytbarhet för att flytta kunder mellan lokala och hello molnet.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-Case-Study-SnelStart/player]
> 
> 

## <a name="why-snelstart-extended-services-from-hello-desktop-toohello-cloud"></a>Varför SnelStart utökade tjänster från hello skrivbord toohello moln
> ”Arbeta med Azure innebär att vi kan leverera programvara snabbare, snabbt reagera toocustomer krav och skala lösningar när kraven ökar”.
> 
> – Henrik har programvara systemarkitekt
> 
> 

SnelStart körde en lyckad programvara företag i år, med hjälp av en utvecklingsmodell för traditionella: code, paket, levereras och upprepa. Över tiden, hello takt ändring vuxen snabbare och snabbare. Förordningar som ändras ofta och kunder behövs enklare sätt tooprocess ekonomiska poster och samarbeta med deras revisorer och myndigheter tookeep dig med sådana ändringar.

”Leverera programvara toocustomers har dyrt och begränsande”, enligt tooHenry har programvara systemarkitekter. ”Produktion, förpackning och leveranskostnader begränsade hur ofta vi gick ut programvara. Vi hade toopackage uppdateringar för periodiska leveranser, men som gjorde det svårt toomeet ändra kundernas behov. Vi kan aldrig vara säker på att våra kunder uppgraderad toohello senaste versionen av hello produkten. Därför kan hade vi toosupport flera versioner gör hello också stöd för jobbet väldigt svår toodo ”.

Har läggs till ”, vi vill sätt tooprogram och version uppdateringar i en snabbare pace, så att vi kan förnya snabbare och skapa nya tjänster för våra kunder. Vi ville också ett sätt tooautomate mer processer i ordning toosimplify våra kunder business-administrationsbehov ”.

Hej lösning för SnelStart, var tooextend dess tjänster genom att bli en molnbaserad SaaS-provider. hello Azure SQL Database-plattformen hjälpt SnelStart dit utan att det medför hello stora IT-omkostnader som skulle ha krävt en lösning för infrastruktur-som-en-tjänst (IaaS).

hello modell också aktiverar SnelStart toofix buggar och innehåller nya funktioner för snabbt, utan kunder toodownload och uppgradera programvara. Bl.a tooBeen, ”Using Azure cloud services gör att vi tooquickly act efter behoven från tredje part. I stället för att behöva tooship toothousands för en ny version av kunder kan anpassa vi informationen som skickas från våra skrivbordsprogram toonew format som krävs av tredje part ”.

## <a name="a-modern-company-with-traditional-roots"></a>En modern företag med traditionella rötter
SnelStart är en modern, flexibel, högteknologiska företag med blygsam rötter räknat too1964, när hello grundare igång hello företag som tillverkar musik betalningsinstrument delar. hello hjärtat i hello SnelStart programvara verksamheten igång verkligen beating i hello 1980 talet, under hello ökande mängden av hello dator. hello företagets behövs ett bättre alternativ toohello redovisningsprogram som tillgänglig för närvarande hello, så det tog frågor i sin egen händer. 1982 skapa hello företagets hello grunden för vad slutligen skulle bli SnelStart redovisningsprogram. Från hello start hello programvara har admired för enkelhetens skull och hastighet, så mycket så att hello företagets slutligen ändras fokus och reinvented sig i ett programvaruföretag.

1995 släppt SnelStart dess första bokföring program för Windows. hello-program som bygger på Microsoft Visual Basic 1.0 och Microsoft Access-databaser, hjälpt växa hello kunden grundläggande toomore än 5 000 användare.

Idag är SnelStart större tillhandahåller en branschspecifika (LOB) och företag administration programmet riktad mot nederländska små och medelstora företag och enskilda företagare. Carlo Kuip, IT systemarkitekt säger är ”vårt mål tooprovide 100 procent automation för företag-administrativa tjänster för våra kunder”.

## <a name="optimizing-performance-and-cost-with-elastic-pools"></a>Optimera prestanda och kostnad med elastiska pooler
SnelStart var en storskalig tidig stöd för elastiska pooler. Elastiska pooler hello företagets gränsen kostnader och hantera prestandakrav mer effektivt. Enligt tooBeen, ”med elastiska pooler kan vi kan optimera prestanda baserat på hello behov av våra kunder, utan överbelasta. Om vi hade tooprovision utifrån belastning, skulle det vara kostsamt. I stället hello alternativet tooshare resurser mellan flera låg belastning databaser kan vi toocreate en lösning som utför väl och kostnadseffektivt ”.

## <a name="azure-sql-databases-help-containerize-data-for-isolation-and-security"></a>Azure SQL-databaser att containerize data för isolering och säkerhet
Azure SQL Database kan SnelStart tooeasily och flytta transparent kunders lokalt företag administrationsdata tooAzure. hello Azure SQL Database är en lämplig behållare som ger isolering, en gräns för autentisering, auktorisering och enkelt funktioner för säkerhetskopiering och återställning. Databaserna utgör en väl lämpade konceptuell modell för företag-administration. Bl.a tooCarlo Kuip IT systemarkitekt ”objekt inom den här behållaren innehåller känsliga data som är avgörande tooa företag och lagra objekten i en isolerad databasen håller dem väl skyddad. Vi kan hantera auktorisering på hello databasnivå och även automatisera hello hantering och skalbar för dessa databaser utan databasadministratörer (DBAs) på personal ”.

Azure SQL Data Warehouse spelar också en roll i hello SnelStart säkerhets- och artikeln genom att hjälpa hello företaget samla in telemetridata som intrångsidentifiering, användaren aktivitetsloggning och anslutningen.

## <a name="azure-removes-overhead-so-that-developers-can-spend-more-time-delivering-value"></a>Azure tar bort omkostnader så att utvecklare kan flera ägna tid åt att leverera värde
hello Azure-plattformen modellen bort infrastruktur kostnader och aktiverat SnelStart tooautomate distributioner med hjälp av C# hantering av bibliotek. Eftersom Kuip tillstånd kunde ”vi kan toogrow våra aktuella åtgärder med en mycket liten avdelning när samtidigt ökad skalbarhet, hastighet och disaster recovery-alternativen för våra kunder. hello SKIFT tooservices development frigjort resurser toofocus på nya tjänster och funktioner, i stället för bara uppdaterar befintlig kod tookeep dig med nya förordningar eller skatt koder ”. Han lägger till ”genom att automatisera hanteringen av och använda hello SaaS erbjudande, är vi kan toodeliver mer värde för våra kunder utan toomake stora investeringar i operativa personal”. Till exempel med hjälp av Azure och elastiska pooler var SnelStart kan tooadd en mängd nya funktioner, inklusive stabilare kundinformation integrering med banker, fakturering nya tjänster, småföretag bakgrundskontroller och e-tjänster.

> ”Vi utökat vårt Azure SQL Database-distributioner från cirka 5 500 toomore än 12 000 i bara hello första månaderna 2016, och vi för närvarande lägger till ca 1 000 databaser per månad”.
> 
> – Henrik har programvara systemarkitekt
> 
> 

Databashantering ytterligare automatiskt med hjälp av funktionen för hello elastiska jobb. Eftersom Kuip tillstånd tack ”hög hello automatisk identifiering av databaser på en [server] instans av SQL-databas”. Detta ger SnelStart tooexecute hanteringsåtgärder över dynamiskt växande kunden grundläggande utan ytterligare kostnader.

SnelStart också utvecklar en API som fungerar som en koordinator mellan kundens data och appar som skapats av programvara från tredje part partners. Som Kuip tillstånd kan ”detta API andra leverantörer tooadd funktioner tooour programvara, till exempel att dataposten för fakturor och andra dokument”. Kunder som har inte längre toomanually typen fakturor till sina småföretag redovisningsprogram; Hej SnelStart programvara kommer att utbyta hello nödvändig information direkt. Detta gör att kunder toojoin sina företag-administrativa uppgifter med hello ekosystem med information som härrör från digitala transformation i hello bransch.  

## <a name="how-azure-services-enable-saas-for-snelstart"></a>Hur Azure-tjänster aktivera SaaS för SnelStart
Med hjälp av Azure kan SnelStart hantera sina kunder och deras revisorer mer sömlöst i hello molnet. Till exempel dela kunder och revisorer information med direkt åtkomst till Snelstarts klient-API som finns på Azure. Kuip tillstånd ”tjänsterna återanvändbara är tillgängliga tooour kund-riktade webbprogram och ger gemensam infrastruktur och funktioner tooallow åtkomst toobusiness administration för kunder och toothird parts programvara som används av våra kunder. Exempel: tillhandahåller produktkonfigurationen funktioner, hantera brandväggsregler och hantera tidskrävande processer som säkerhetskopieringar ”.

> Vårt mål är tooprovide 100 procent automation för företag-administrativa tjänster för våra kunder ”. 
> 
> – Carlo Kuip, IT systemarkitekt
> 
> 

Dessutom kan SnelStart webbtjänster både kunder och revisorer tooeasily åtkomst till data i Azure SQL Database elastiska pooler. Den här SaaS-modellen, tillsammans med databasen elasticitet och Azure Resource Manager tillhandahåller SnelStart skalbarhet funktioner som passar alla Azure-distribution. hello implementering automatiserad helt hantering av bibliotek C#.

![SnelStart arkitektur](./media/sql-database-implementation-snelstart/figure1.png)

Bild 1. Från och med juni 2016 har SnelStart mer än 11 000 databaser och mer än 50 elastiska pooler

## <a name="simplicity-from-hello-cloud"></a>Enkelhet från hello moln
Sedan flytta tooan Azure molnbaserad lösning har SnelStart kan toosupport snabb kunden tillväxt samtidigt som den erbjuder innovativa funktioner och tjänster. Enligt tooBeen, ”med Azure, vi kan leverera nästan kontinuerlig uppdateringar för våra kunder, utan att expandera våra driftspersonal. Och vi får alla hello andra fantastiska Azure-funktioner – som skalbarhet och katastrofåterställning – finns i ”.

> ”När vi har faktiskt över i Redmond... Jag har fått ett anrop från utvecklaren i hello Nederländerna ringa mig om ett specifikt problem. Vi har kan tooget Microsoft toodeliver en ändring i sin produktionsmiljö inom 48 timmar toosolve våra problemet ”.
> 
> – Henrik har programvara systemarkitekt
> 
> 

SnelStart appreciates också hello starkt partnerskap som de har utvecklats med hello Microsoft Azure SQL DB-teamet. Som Kuip tillstånd ”har vi diskussioner på funktioner och hur toouse teknik, vilket är något uppskattar på båda sidor”.
hello omedelbar målet för SnelStart är tookeep växande nöjd kunden bas. Varit tillstånd ”utan hello tekniska och resursbegränsningar som vi hade som en ISV, det finns ingen gräns toohow långt vi kan växa”.

## <a name="more-information"></a>Mer information
* toolearn mer om Azure elastiska pooler finns [elastiska pooler](sql-database-elastic-pool.md).
* toolearn mer om Web och arbetsroller, se [arbetsroller](../fundamentals-introduction-to-azure.md#compute).    
* toolearn mer om Azure SQL Data Warehouse finns [SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/)
* toolearn mer om SnelStart, se [SnelStart](http://www.snelstart.nl).

