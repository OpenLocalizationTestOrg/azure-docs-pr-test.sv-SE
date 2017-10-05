---
title: "Konfigurera och använda Machine Learning rekommendationer API | Microsoft Docs"
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
redirect_document_id: TRUE
ms.openlocfilehash: 3851589818bb8f4309bf3c65f17b115e0dcd27fa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="setting-up-and-using-machine-learning-recommendations-api-faq"></a>Konfigurera och använda Machine Learning, rekommendationer API, vanliga frågor och svar
**Vad är rekommendationer?**

> [!NOTE]
> Du bör börja använda kognitiva Rekommendationstjänsten API i stället för den här versionen. Kognitiva Rekommendationstjänsten ersätter den här tjänsten och de nya funktionerna det kommer fram. Den har nya funktioner som batchbearbetning support, bättre API Explorer, en tydligare API underlag, mer konsekvent signup/fakturerings experience och så vidare.
> Lär dig mer om [migrera till den nya kognitiva tjänsten](http://aka.ms/recomigrate)
> 
> 

För organisationer och företag som förlitar sig på rekommendationer mellan säljer och upp säljer produkter och tjänster till sina kunder rekommendationer i Azure Machine Learning ger en motor för självbetjäning rekommendationer. Det är en implementering av samarbetsfunktioner filtrering som använder matrisen factorization som core algoritm. Programutvecklare kan komma åt rekommendationer med hjälp av REST API: er. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Vad kan jag göra med rekommendationer?**

REKOMMENDATIONER som indata för ett objekt eller en uppsättning objekt och returnerar en lista över relevanta rekommendationer. Till exempel: kund hos en återförsäljare som online klickar du på en produkt. Online återförsäljaren skickar produkten som indata till rekommendationer, hämtar en lista över produkter i RETUR och bestämmer vilken av dessa produkter som ska visas för kunden. Du kanske vill använda rekommendationer att optimera din onlinebutiken eller även att meddela din inom försäljning avdelning eller anrop center.

**Finns det några begränsningar?**

Rekommendationerna har följande begränsningar:

* Maximala antalet modeller per prenumeration: 10
* Maximalt antal objekt som kan innehålla en katalog: 100 000
* Det maximala antalet punkter för användning som hålls är ~ 5,000,000. Den äldsta tas bort om nya överföras eller rapporteras.
* Maximal storlek för data som kan skickas via e-post (till exempel importera katalogdata import användningsdata) är 200 MB
* Antal transaktioner per sekund (Transaktionsprogram) för en modell rekommendationer-version som inte är aktiv är ~ 2 Transaktionsprogram. En rekommendationer modell-version som är aktiv kan innehålla upp till 20 Transaktionsprogram.

## <a name="purchase-and-billing"></a>Köp och fakturering
**Hur mycket kostar rekommendationer under starta?**

Rekommendationer är en prenumeration-baserad tjänst. Debitering baserat på mängden transaktioner per månad. Du kan kontrollera den [erbjuder sidan](https://datamarket.azure.com/dataset/amla/recommendations) i Microsoft Azure Marketplace för information om priser.

**Finns det några kostnader som är associerade med med rekommendationer spåra och lagra användaraktivitet för mig?**

Inte för tillfället.

**Har rekommendationer för en kostnadsfri utvärderingsversion?**

Det finns en kostnadsfri testversion som är begränsad till 10 000 transaktioner per månad.

**När jag debiteras för rekommendationer?**

En betald prenumeration är någon prenumeration för vilken det finns en månadsavgift. Du debiteras omedelbart för den första månaden när du köper en betald prenumeration. Du debiteras belopp som hör till erbjudandet på prenumerationssidan (plus eventuella skatter). Den här månadskostnaden görs varje månad kalender samma dag som köpet ursprungliga tills du avbryter prenumerationen. 

**Hur uppgraderar till en högre nivå-tjänst?**

Du kan köpa eller uppdatera din prenumeration från den [erbjuder sidan](https://datamarket.azure.com/dataset/amla/recommendations) sida på Microsoft Azure Marketplace.

När du uppgraderar en prenumeration:

* Transaktioner som kvar i din gamla prenumeration har inte lagts till din prenumeration. 
* Du betalar fullständig pris för den nya prenumerationen, även om du har oanvända transaktioner på din gamla prenumeration.

Processen att uppgradera en prenumeration:

* Nevigate till den [erbjuder sidan](https://datamarket.azure.com/dataset/amla/recommendations).
* Logga in om du inte redan är inloggad på Marketplace.
* I den högra rutan visas alla tillgängliga planer. Klicka på knappen för den plan som du vill uppgradera till.
* Om du vill uppgradera klickar du på **OK**. Om du inte vill uppgradera klickar du på **Avbryt**.

**Viktiga** dialogrutan Läs noggrant innan du uppgraderar eftersom det finns fakturerings- och konsekvenser.

**När avslutas min prenumeration till rekommendationer?**

Din prenumeration avslutas när du avbryter den. Om du vill avbryta dina prenumerationer finns i följande anvisningar.

**Hur avbryter min rekommendationer prenumeration?**

Använd följande steg om du vill avbryta din prenumeration. Om din aktuella prenumeration är en betald prenumeration, fortsätter prenumerationen gäller fram till slutet av den aktuella faktureringsperioden. Om du behöver avbryta gälla omedelbart kan du kontakta oss på [Microsoft-supporten](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

**Obs** bidrag ges om du avbryter före utgången av en faktureringsperiod eller för transaktioner som inte används i en faktureringsperioden.

* Navigera till den [erbjuder sidan](https://datamarket.azure.com/dataset/amla/recommendations).
* Logga in om du inte redan är inloggad på Marketplace.
* Klicka på **Avbryt** till höger om dataset-namnet och status. Du kan använda den här prenumerationen till slutet av den aktuella faktureringsperioden eller transaktionen gränsen har nåtts (beroende på vilket som inträffar först).

Om du vill avbryta prenumerationen omedelbart så du kan köpa en prenumeration filen en biljett på [Microsoft-supporten](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

## <a name="getting-started-with-recommendations"></a>Komma igång med rekommendationer
**Är rekommendationer för mig?** 

Rekommendationerna i Machine Learning är för organisationer och företag som förlitar sig på rekommendationer mellan säljer och upp säljer produkter eller tjänster till sina kunder. Om du har en kund-riktade webbplats, en säljstyrkan, en inre säljstyrkan eller Callcenter, och om du tillhandahåller en katalog över flera dussin produkter eller tjänster slutresultatet kan dra nytta av rekommendationer. 

Experimentera med rekommendationer är avsedda att vara ganska enkel. Den aktuella versionen av API-baserade kräver grundläggande kunskaper i programmering. Kontakta leverantören som utvecklat din webbplats om du behöver hjälp. Om du har en intern IT-avdelningen eller intern systemutvecklare ska de kunna få rekommendationer som passar dig. 

**Vilka är kraven för att ställa in rekommendationer?**

Rekommendationer kräver att du har en logg över användarens val i relation till katalogen. Om du inte har sådan loggen och du har en kund-webbplatser, rekommendationer kan samla in användaraktivitet för dig. 

Rekommendationer kräver också en katalog med dina produkter eller tjänster. Om du inte har katalogen kan rekommendationer använda faktiska användning kunddata och destillera en katalog. En underförstådda katalog kommer inte att innehålla objekt som inte rapporterades som en del av användartransaktioner.

**Hur ställer jag in rekommendationer för första gången?**

Efter [prenumerationsapp](https://datamarket.azure.com/dataset/amla/recommendations) rekommendationer, bör du använda API-dokumentationen i den [Azure Machine Learning rekommendationer - snabbstartsguiden](machine-learning-recommendation-api-quick-start-guide.md) att konfigurera tjänsten.

**Var hittar jag API-dokumentationen?** 

API-dokumentationen är [rekommendationer för Azure Machine Learning-snabbstartsguiden](machine-learning-recommendation-api-quick-start-guide.md).

**Vilka alternativ finns att överföra katalog-och användningsdata till rekommendationer?**

Har du två alternativ för uppladdning av data catalog- och användningsdata: du kan exportera data från ditt CRM-system eller andra loggar och överföra den till rekommendationer eller du kan lägga till taggar till din webbplats som ska övervaka användaraktiviteter. Om du använder den andra metoden lagras data i Azure.

## <a name="maintenance-and-support"></a>Underhåll och support
**Hur stor kan min datauppsättning?**

Varje datamängd kan innehålla upp till 100 000 katalogobjekt och upp till 2 048 MB användningsdata.
En prenumeration kan dessutom innehålla upp till 10 datauppsättningar (modeller).

**Var hittar jag teknisk support för rekommendationer**

Teknisk support är tillgänglig på den [stöd för Microsoft Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) plats.

**Var hittar jag användningsvillkoren**

[Microsoft Azure Machine Learning rekommendationer API användarvillkoren](https://datamarket.azure.com/dataset/amla/recommendations#terms).

