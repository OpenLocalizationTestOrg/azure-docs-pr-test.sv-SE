---
title: "begränsa aaaUnderstand Azure utgifter | Microsoft Docs"
description: "Beskriver hur Azure utgiftsgränsen fungerar och hur tooremove den"
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: genli
ms.openlocfilehash: ed01401a07c3d0e7edebe42fb1482b7b60b1df51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-azure-spending-limit-and-how-tooremove-it"></a>Förstå Azure utgiftsgräns och hur tooremove den

Azure utgiftsgräns är en gräns för hur mycket din Azure-prenumeration kan spendera. Alla nya kunder som registrerar dig för hello utvärderingsversion eller erbjudanden som innehåller krediter över flera månader har hello utgiftsgräns aktiverad som standard. hello utgiftsgräns är 0. Det går inte att ändra. hello utgiftsgräns är inte tillgängligt för prenumerationstyper, till exempel betala per användning prenumerationer och åtagande planer. Se hello [fullständig lista över Azure erbjudanden och hello tillgängligheten för hello utgiftsgräns](https://azure.microsoft.com/support/legal/offer-details/).

## <a name="what-happens-when-i-reach-hello-spending-limit"></a>Vad händer när jag nå hello utgiftsgräns?

När din användning resultat i avgifter som få slut på hello månatliga mängder som ingår i erbjudandet, inaktiveras hello-tjänster som du har distribuerat hello resten av den faktureringsmånaden. Molntjänster som du har distribuerat tas till exempel bort från produktionen och dina virtuella Azure-datorer stoppas och frigörs. tooprevent tjänsterna inaktiveras, kan du välja tooremove din utgiftsgräns. När dina tjänster är inaktiverade finns hello data i dina lagringskonton och databaser i en skrivskyddad sätt för administratörer. Hello början av hello nästa Faktureringsmånad, om erbjudandet omfattar krediter över flera månader ska prenumerationen återaktiveras. Du kan distribuera dina molntjänster och ha fullständig åtkomst tooyour storage-konton och databaser.

När hello kostnadsfri utvärderingsprenumeration når hello utgiftsgräns, kan du återaktivera hello prenumeration och har automatiskt [uppgradera tooour standard betala](billing-upgrade-azure-subscription.md) inom 90 dagar.

Du kan få meddelanden när du träffar hello utgiftsgränsen för ditt erbjudande. Logga in toohello [Azure Kontocenter](https://account.windowsazure.com)väljer **konto**, och välj sedan **prenumerationer**. Du kan se aviseringar om prenumerationer som har nått utgiftsgränsen hello.

## <a name="things-you-are-charged-for-even-if-you-have-a-spending-limit-enabled"></a>Saker du debiteras för även om du har en utgiftsgräns aktiveras

Vissa Azure-tjänster och [Marketplace-inköp](https://azure.microsoft.com/marketplace/) kan avgifter under hello betalningsmetod (CC) även om utgiftsgränsen anges. Exempel är Visual studio licenser, Azure Active Directory premium, supportplaner och de flesta från tredje part märkta tjänster som säljs via hello Marketplace.


## <a name="when-not-toouse-hello-spending-limit"></a>När inte toouse hello utgiftsgräns

hello utgiftsgräns kan hindra dig från att distribuera eller använda vissa marketplace och Microsoft-tjänster. Här följer hello scenarier där du bör ta bort hello utgiftsgränsen för din prenumeration.

- Du planerar toodeploy första part bilder som Oracle och tjänster som Visual Studio Team Services. Det här scenariot orsakar tooexceed din utgiftsgränsen nästan omedelbart och gör att din prenumeration toobe inaktiverad.

- Du har tjänster som inte får avbrytas.

- Du har tjänster och resurser med inställningar som virtuella IP-adresser som du vill inte toolose. Dessa inställningar går förlorade när hello tjänster och resurser har frigjorts.


## <a name="remove-hello-spending-limit"></a>Ta bort hello utgiftsgräns

Du kan ta bort hello utgiftsgräns när som helst så länge det finns en giltig betalningsmetod som är associerad med din prenumeration. För erbjudanden som har kredit över flera månader, kan du också återaktivera hello utgiftsgräns hello början av nästa faktureringscykel.

tooremove din utgiftsgränsen, gör du följande:

1. Logga in toohello [Azure Kontocenter](https://account.windowsazure.com).

2. Välj en prenumeration.

3. Om hello prenumerationen har inaktiverats på grund av toohello utgiftsgränsen nås, klickar du på det här meddelandet: ”prenumerationen uppnåtts hello utgiftsgräns och har varit inaktiverad tooprevent avgifter”. Annars klickar du på **ta bort utgiftsgränsen** i hello **PRENUMERATIONSSTATUS** område.

4. Välj ett alternativ som passar dig.

|Alternativ|Verkan|
|-------|-----|
|Ta bort utgiftsgränsen på obestämd tid|Tar bort hello utgiftsgräns utan att starta den automatiskt hello början av hello nästa faktureringsperiod.|
|Ta bort utgiftsgränsen för hello aktuella fakturering period|Tar bort hello utgiftsgräns så att den aktiverar tillbaka automatiskt hello början av hello nästa faktureringsperiod.|

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten.
Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget problemet lösas snabbt.
