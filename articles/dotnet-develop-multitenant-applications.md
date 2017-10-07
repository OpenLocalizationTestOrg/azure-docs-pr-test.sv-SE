---
title: "aaaMulti klient Web programmönster | Microsoft Docs"
description: "Hitta arkitektur översikter och designmönster som beskriver hur tooimplement flera innehavare webbprogrammet på Azure."
services: 
documentationcenter: .net
author: wadepickett
manager: wpickett
editor: 
ms.assetid: 4f0281d2-1555-42b0-a99d-1222fade0b0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/05/2015
ms.author: wpickett
ms.openlocfilehash: 3b7822c8ca4aa50d295a174973ae4746c230ba66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="multitenant-applications-in-azure"></a>Flera program i Azure
Ett multitenant program är en delad resurs som gör att olika användare eller ”innehavare”, tooview hello program som om den har sina egna. Ett typiskt scenario som lämpar sig tooa flera program är en i vilken alla användare av hello kan programmet vill toocustomize hello användarupplevelsen men annars har hello samma grundläggande affärsbehov. Exempel på stora multitenant program är Office 365, Outlook.com och visualstudio.com.

En programprovider ur hello fördelarna med multitenancy främst relatera toooperational och kostnaden effektivitet. En version av programmet kan uppfylla hello behov av många hyresgäster/kunder, så att slå samman system administrationsuppgifter, till exempel övervakning, Prestandajustering, programvaruunderhåll och säkerhetskopiering av data.

hello följande innehåller en lista över hello viktigaste mål och krav ur en provider.

* **Etablering**: du måste vara kan tooprovision nya klienter för hello program.  För flera program med ett stort antal klienter är vanligtvis behöver tooautomate processen genom att aktivera självbetjäning etablering.
* **Underhålla**: du måste kunna tooupgrade hello program och utföra andra underhållsaktiviteter när flera klienter använder den.
* **Övervaka**: du måste vara kan toomonitor hello program vid alla tidpunkter tooidentify eventuella problem och tootroubleshoot dem. Detta omfattar övervakning av hur varje klient använder hello program.

En välimplementerad multitenant programmet tillhandahåller hello följande fördelar toousers.

* **Isolering**: hello aktiviteter för enskilda klienter inte påverkar hello användning av programmet hello av andra klienter. Klienter kommer inte åt varandras data. Toohello klient verkar det som om de har exklusiv användning av programmet hello.
* **Tillgänglighet**: enskilda klienter vill hello programmet toobe ständigt tillgänglig kanske garantier som definierats i ett serviceavtal. Igen och påverkar inte hello aktiviteter för andra hyresgäster hello tillgängligheten för hello program.
* **Skalbarhet**: programmet hello skalas toomeet hello begäran av enskilda klienter. hello bör förekomst och åtgärder för andra hyresgäster inte påverka hello programmet hello prestanda.
* **Kostnader**: kostnader som är lägre än en dedikerad, stöd för en innehavare programmet kördes eftersom multitenans gör hello delning av resurser.
* **Anpassningsbarheten**. hello möjlighet toocustomize hello program för en enskild klientorganisation på olika sätt, till exempel att lägga till eller ta bort funktioner, ändra färger och logotyper eller även att lägga till egen kod eller skript.

Kort sagt, medan det finns många saker du måste göra till kontot tooprovide en mycket skalbar tjänst finns också ett antal hello mål och krav som är vanliga toomany flera program. Vissa kanske inte är relevant i specifika scenarier och hello vikten av enskilda mål och krav varierar i varje scenario. Som en provider för hello multitenant program har du också mål och krav som, uppfyller hello klienternas mål och krav, lönsamhet, fakturering, flera servicenivåer, etablering, underhålla övervakning och automatisering.

Mer information om ytterligare överväganden för flera program, se [värd för flera innehavare-program på Azure][Hosting a Multi-Tenant Application on Azure]. Information om vanliga mönster för dataarkitekturen i SaaS-databasprogram (Software-as-a-Service) med flera klienter finns i [Designmönster för SaaS-program med flera klienter i Azure SQL Database](sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md). 

Azure tillhandahåller många funktioner som gör att du tooaddress hello viktiga problem som uppstår när du utformar en multitenant system.

**Isolering**

* Segment webbplats innehavare av värdhuvuden med eller utan SSL-kommunikation
* Segmentera webbplats innehavare av frågeparametrar
* Web Services arbetsroller
  * Arbetsroller. som vanligtvis bearbeta data på hello serverdel för ett program.
  * Web-roller som vanligtvis fungerar som klientdel hello för program.

**Storage**

Datahantering, till exempel Azure SQL Database eller Azure Storage-tjänster, till exempel hello tabelltjänst som tillhandahåller tjänster för lagring av stora mängder Ostrukturerade data och hello Blob-tjänst som tillhandahåller tjänster toostore stora mängder Ostrukturerade text eller binära data, till exempel video, ljud och bilder.

* För att skydda Multitenant Data i SQL-databas som är lämpliga per-innehavaren SQL Server-inloggningar.
* Med hjälp av Azure-tabeller för programmet resurser genom att ange en princip för åtkomst av behållare hello du möjlighet tooadjust behörigheter utan tooissue nya URL: er för hello resurser som skyddas med signaturer för delad åtkomst.
* Azure köer för programmet resurser Azure köer är vanliga toodrive bearbetning för klienter, men kan även använda toodistribute arbete krävas för etablering och hantering.
* Service Bus-köer för resurser som push-meddelanden fungerar tooa delade en tjänst kan du använda en enskild kö där varje innehavare avsändare bara har behörigheter (som härletts från anspråk som utfärdats av ACS) toopush toothat kön när endast hello mottagare från hello-tjänsten ha behörighet toopull från hello kön hello data från flera klienter.

**Anslutning och säkerhetstjänster**

* Azure Service Bus i en infrastruktur för meddelanden som placeras mellan program så att de tooexchange meddelanden på ett sätt som löst kopplade för bättre skalning och återhämtning.

**Nätverkstjänster**

Azure tillhandahåller flera nätverkstjänster som stöder autentisering och förbättra hanteringen av dina program med värdar. Tjänsterna omfattar hello följande:

* Azure Virtual Network, kan du etablera och hantera virtuella privata nätverk (VPN) i Azure som på ett säkert sätt länka dessa med lokal IT-infrastruktur.
* Virtuella nätverk Traffic Manager kan du tooload saldo inkommande trafik över flera Azure för värdbaserade tjänster, oavsett om de kör i hello samma datacenter eller i olika Datacenter hello världen.
* Azure Active Directory (AD Azure) är en modern REST-baserad tjänst som tillhandahåller identity management och åtkomst kontroll funktioner för dina molnprogram. Använda Azure AD för programresurser Azure AD tooprovides ett enkelt sätt att autentisera användare toogain åtkomst tooyour webbprogram och tjänster medan hello funktioner för autentisering och auktorisering toobe inberäknade av din kod.
* Azure Service Bus är en säker och kapaciteten för flödet av data för distribuerade och hybridprogram, till exempel kommunikation mellan Azure finns program och lokala program och tjänster, utan komplexa brandvägg och säkerhet infrastruktur. Använda Service Bus Relay för programresurser toohello tjänster som visas som slutpunkter kan höra toohello klient (till exempel finns utanför hello-system såsom lokalt) eller de kanske tjänster som skapats specifikt för hello-klient ( eftersom känsliga, klient-specifika data överförs mellan dem).

**Etablering av resurser**

Azure tillhandahåller ett antal sätt etablera nya klienter för hello program. För flera program med ett stort antal klienter är vanligtvis behöver tooautomate processen genom att aktivera självbetjäning etablering.

* Arbetsroller kan du tooprovision och avinstallation etablera per klientresurser (t.ex när en ny klient loggar in eller avbryter), samla in statistik för Avläsning av använda och hantera skala enligt ett visst schema eller i svaret toohello korsande av tröskelvärden för nyckeln nyckeltal. Samma rollen kan även vara används toopush ut uppdateringar och uppgraderingar toohello lösning.
* Azure BLOB kan vara används tooprovision beräkning eller före initierad lagringsresurser för nya klienter samtidigt som man behållaren administratörsnivå principer tooprotect hello beräkning servicepaket VHD-avbildningar och andra resurser.
* Alternativ för att etablera SQL Database-resurser för en klient inkluderar:
  
  * DDL i skript eller inbäddade som resurser i sammansättningar
  * SQL Server 2008 R2 DAC-paket distribueras via programmering.
  * Kopiera från en för master-referensdatabasen
  * Använda databasen Import och Export tooprovision nya databaser från en fil.

<!--links-->

[Hosting a Multi-Tenant Application on Azure]: http://msdn.microsoft.com/library/hh534480.aspx
[Designing Multitenant Applications on Azure]: http://msdn.microsoft.com/library/windowsazure/hh689716
