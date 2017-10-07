---
title: "aaaSet in och använda hello Machine Learning rekommendationer API | Microsoft Docs"
description: "Microsoft rekommendationer API som byggts med Azure Machine Learning vanliga frågor och svar"
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 1ffc3c16-e040-4225-9d72-105129938dfa
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: luisca
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: True
ms.openlocfilehash: 980bf1a36f3291275d9ef0fee9b4446f7e0cbecf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-and-using-machine-learning-recommendations-api-faq"></a>Konfigurera och använda Machine Learning, rekommendationer API, vanliga frågor och svar
**Vad är rekommendationer?**

> [!NOTE]
> Du bör börja använda hello kognitiva rekommendationer API-tjänsten i stället för den här versionen. hello kognitiva Rekommendationstjänsten ersätter den här tjänsten och alla nya funktioner för hello det kommer fram. Den har nya funktioner som batchbearbetning support, bättre API Explorer, en tydligare API underlag, mer konsekvent signup/fakturerings experience och så vidare.
> Lär dig mer om [migrera toohello ny kognitiva tjänst](http://aka.ms/recomigrate)
> 
> 

REKOMMENDATIONERNA i Azure Machine Learning ger en motor för självbetjäning rekommendationer för organisationer och företag som förlitar sig på rekommendationer toocross-säljer och upp säljer produkter och tjänster tootheir kunder. Det är en implementering av samarbetsfunktioner filtrering som använder matrisen factorization som core algoritm. Programutvecklare kan komma åt rekommendationer med hjälp av REST API: er. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Vad kan jag göra med rekommendationer?**

REKOMMENDATIONER som indata för ett objekt eller en uppsättning objekt och returnerar en lista över relevanta rekommendationer. Till exempel: kund hos en återförsäljare som online klickar du på en produkt. hello online återförsäljare skickar produkten som inkommande tooRECOMMENDATIONS hämtar en lista över produkter i RETUR och bestämmer vilken av dessa produkter visas toohello kunden. Du kanske vill toouse rekommendationer toooptimize onlinebutiken eller ens tooinform din inom försäljning avdelning eller anrop center.

**Finns det några begränsningar?**

Rekommendationerna har hello följande begränsningar:

* Maximala antalet modeller per prenumeration: 10
* Maximalt antal objekt som kan innehålla en katalog: 100 000
* hello maxantalet återställningspunkter för användning som hålls är ~ 5,000,000. hello äldsta tas bort om nya överföras eller rapporteras.
* Maximal storlek för data som kan skickas via e-post (till exempel importera katalogdata import användningsdata) är 200 MB
* Antal transaktioner per sekund (Transaktionsprogram) för en modell rekommendationer-version som inte är aktiv är ~ 2 Transaktionsprogram. En rekommendationer modell-version som är aktiv kan innehålla upp too20 Transaktionsprogram.

## <a name="purchase-and-billing"></a>Köp och fakturering
**Hur mycket kostar rekommendationer under hello starta?**

Rekommendationer är en prenumeration-baserad tjänst. Debitering baserat på mängden transaktioner per månad. Du kan kontrollera hello [erbjuder sidan](https://datamarket.azure.com/dataset/amla/recommendations) i Microsoft Azure Marketplace för information om priser.

**Finns det några kostnader som är associerade med med rekommendationer spåra och lagra användaraktivitet för mig?**

Inte tillfället hello.

**Har rekommendationer för en kostnadsfri utvärderingsversion?**

Det finns en kostnadsfri testversion som är begränsad too10, 000 transaktioner per månad.

**När jag debiteras för rekommendationer?**

En betald prenumeration är någon prenumeration för vilken det finns en månadsavgift. När du köper en betald prenumeration omedelbart debiteras du för hello först månadens användning. Du debiteras hello belopp som är associerad med hello erbjudandet på hello prenumerationssidan (plus eventuella skatter). Den här månadskostnaden görs varje månad hello samma kalender datum som köpet ursprungliga tills du avbryter hello prenumeration. 

**Hur uppgraderar jag tooa tjänst på högre nivå**

Du kan köpa eller uppdatera din prenumeration från hello [erbjuder sidan](https://datamarket.azure.com/dataset/amla/recommendations) sida på Microsoft Azure Marketplace.

När du uppgraderar en prenumeration:

* Transaktioner som kvar på prenumerationen gamla läggs inte tooyour ny prenumeration. 
* Du betalar hela priset för hello ny prenumeration, även om du har oanvända transaktioner på din gamla prenumeration.

Processen tooupgrade en prenumeration:

* Nevigate toohello [erbjuder sidan](https://datamarket.azure.com/dataset/amla/recommendations).
* Logga in toohello Marketplace om du inte redan är inloggad.
* I högra fönstret hello visas alla tillgängliga planer för hello. Klicka på hello alternativknapp för hello-plan som du vill tooupgrade till.
* Om du vill tooupgrade **OK**. Om du inte vill att tooupgrade **Avbryt**.

**Viktiga** noggrant skrivskyddade hello dialogrutan innan du uppgraderar eftersom det finns fakturerings- och konsekvenser.

**Då avslutas tooRecommendations min prenumeration?**

Din prenumeration avslutas när du avbryter den. Om du vill att toocancel hello följa anvisningar finns i dina prenumerationer.

**Hur avbryter min rekommendationer prenumeration?**

toocancel din prenumeration, Använd hello följande steg. Om din aktuella prenumeration är en betald prenumeration, fortsätter prenumerationen i kraft förrän hello slutet av hello aktuella fakturering period. Om du behöver hello annullering toobe gälla omedelbart, kontaktar du oss på [Microsoft-supporten](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

**Obs** bidrag ges om du avbryter innan hello slutet av en faktureringsperiod eller för transaktioner som inte används i en faktureringsperioden.

* Navigera toohello [erbjuder sidan](https://datamarket.azure.com/dataset/amla/recommendations).
* Logga in toohello Marketplace om du inte redan är inloggad.
* Klicka på **Avbryt** toohello höger i hello dataset-namnet och status. Du kan använda den här prenumerationen förrän hello hello aktuella fakturering period eller transaktionen gränsen slut (beroende på vilket som inträffar först).

Om du vill att toocancel prenumerationen omedelbart så du kan köpa en prenumeration filen en biljett på [Microsoft-supporten](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

## <a name="getting-started-with-recommendations"></a>Komma igång med rekommendationer
**Är rekommendationer för mig?** 

Rekommendationerna i Machine Learning är för organisationer och företag som förlitar sig på rekommendationer toocross säljer och upp säljer produkter eller tjänster tootheir kunder. Om du har en kund-riktade webbplats, en säljstyrkan, en inre säljstyrkan eller Callcenter, och om du tillhandahåller en katalog över flera dussin produkter eller tjänster slutresultatet kan dra nytta av rekommendationer. 

Experimentera med rekommendationer är utformad toobe ganska enkel. hello aktuella API-baserade versionen kräver grundläggande kunskaper i programmering. Kontakta hello-leverantör som utvecklat din webbplats om du behöver hjälp. Om du har en intern IT-avdelningen eller intern systemutvecklare ska de kunna tooget rekommendationer toowork för dig. 

**Vad är hello förutsättningar för att skapa rekommendationer?**

Rekommendationer kräver att du har en logg över användarens val när det gäller tooyour katalog. Om du inte har sådan loggen och du har en kund-webbplatser, rekommendationer kan samla in användaraktivitet för dig. 

Rekommendationer kräver också en katalog med dina produkter eller tjänster. Om du inte har hello katalog rekommendationer använder hello faktiska användning kundinformation och destillera en katalog. En underförstådda katalog kommer inte att innehålla objekt som inte rapporterades som en del av användartransaktioner.

**Hur ställer jag in rekommendationer för hello första gången?**

Efter [prenumerationsapp](https://datamarket.azure.com/dataset/amla/recommendations) tooRecommendations, bör du använda hello API-dokumentationen i hello [Azure Machine Learning rekommendationer - snabbstartsguiden](machine-learning-recommendation-api-quick-start-guide.md) tooset hello-tjänsten.

**Var hittar jag API-dokumentationen?** 

hello API-dokumentationen är [rekommendationer för Azure Machine Learning-snabbstartsguiden](machine-learning-recommendation-api-quick-start-guide.md).

**Alternativ gör jag har tooRecommendations av tooupload katalog- och användningsdata?**

Har du två alternativ för uppladdning av data catalog- och användningsdata: du kan exportera hello data från ditt CRM-system eller andra loggar och överföra den tooRecommendations eller du kan lägga till taggar tooyour webbplats som ska övervaka användaraktiviteter. Om du använder hello senare metoden lagras hello data i Azure.

## <a name="maintenance-and-support"></a>Underhåll och support
**Hur stor kan min datauppsättning?**

Varje datamängd kan innehålla upp too100 000 katalogobjekt och in användningsdata too2048 MB.
En prenumeration kan dessutom innehålla too10 data uppsättningar (modeller).

**Var hittar jag teknisk support för rekommendationer**

Teknisk support är tillgänglig på hello [stöd för Microsoft Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) plats.

**Var hittar jag hello användningsvillkor**

[Microsoft Azure Machine Learning rekommendationer API användarvillkoren](https://datamarket.azure.com/dataset/amla/recommendations#terms).

