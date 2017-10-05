---
title: "Lösa problem med en licens för en grupp i Azure Active Directory | Microsoft Docs"
description: "Hur du identifiera och lösa problem med licens tilldelning när du använder Azure Active Directory gruppbaserade licensiering"
services: active-directory
keywords: Azure AD-licensiering
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/05/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bfa951a897c9b383072c0d29c9a4266c163fe753
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="identify-and-resolve-license-assignment-problems-for-a-group-in-azure-active-directory"></a>Identifiera och lösa problem med licensen tilldelning för en grupp i Azure Active Directory

Gruppbaserade licensiering i Azure Active Directory (AD Azure) introducerar konceptet för användare i en licensiering feltillstånd. I den här artikeln förklarar vi skälen varför användare kan hamna i det här tillståndet.

När du tilldelar licenser direkt till enskilda användare, utan att använda gruppbaserade licensiering kan tilldelningens misslyckas. Till exempel när du kör PowerShell-cmdleten `Set-MsolUserLicense` för en användare cmdlet kan misslyckas av flera skäl som är relaterade till affärslogik. Det kan till exempel finnas ett otillräckligt antal licenser eller en konflikt mellan två serviceplaner som inte kan tilldelas på samma gång. Problemet rapporteras omedelbart tillbaka till dig.

När du använder gruppbaserade licensiering, samma fel inträffar, men de sker i bakgrunden när Azure AD-tjänsten är tilldela licenser. Därför går inte att felen meddelas du omedelbart. I stället de registreras i användarobjektet och sedan rapporteras via administrativa portal. Observera att den ursprungliga avsikten att licensiera användaren aldrig går förlorade, men den registreras i ett feltillstånd för framtida undersökningar och upplösning.

## <a name="how-to-find-license-assignment-errors"></a>Så här hittar du licens Tilldelningsfel

1. Öppna bladet för gruppen för att söka efter användare i ett feltillstånd i en specifik grupp. Under **licenser**, kommer det att ett meddelande visas om det finns några användare i ett feltillstånd.

![Gruppen felmeddelanden](media/active-directory-licensing-group-problem-resolution-azure-portal/group-error-notification.png)

2. Klicka på meddelandet för att öppna en lista över alla användare som påverkas. Du kan klicka på varje användare individuellt för att se mer information.

![Gruppen, lista över användare i feltillstånd](media/active-directory-licensing-group-problem-resolution-azure-portal/list-of-users-with-errors.png)

3. Söker efter alla grupper som innehåller minst ett fel på den **Azure Active Directory** bladet välj **licenser** och sedan **översikt**. En info visas när vissa grupper kräver din uppmärksamhet.

![Översikt, information om grupper i feltillstånd](media/active-directory-licensing-group-problem-resolution-azure-portal/group-errors-widget.png)

4. Klicka på kryssrutan för att visa en lista över alla grupper med fel. Du kan klicka på varje grupp för mer information.

![Översikt över grupper med fel](media/active-directory-licensing-group-problem-resolution-azure-portal/list-of-groups-with-errors.png)


Nedan följer en beskrivning av varje potentiella problem och hur man löser problemet.

## <a name="not-enough-licenses"></a>Det finns inte tillräckligt med licenser

**Problem:** det inte finns tillräckligt många tillgängliga licenser för en av de produkter som anges i gruppen. Du måste köpa fler licenser för produkten eller frigör oanvända licenser från andra användare eller grupper.

Hur många licenser är tillgängliga, gå till **Azure Active Directory** > **licenser** > **alla produkter**.

Om du vill se vilka användare och grupper som förbrukar licenser, klickar du på en produkt. Under **licensierade användare**, visas alla användare som har haft användarlicenser direkt eller via en eller flera grupper. Under **licensierad grupper**, visas alla grupper som har tilldelats produkten.

**PowerShell:** PowerShell-cmdlets rapportera felet som _CountViolation_.

## <a name="conflicting-service-plans"></a>Motstridiga service-planer

**Problem:** en av de produkter som har angetts i gruppen innehåller en serviceplan som står i konflikt med en annan service-plan som är redan tilldelad till användare via en annan produkt. Vissa serviceplaner konfigureras så att de inte kan tilldelas till samma användare som en annan relaterade service-plan.

Överväg följande exempel. En användare har en licens för Office 365 Enterprise **E1** tilldelats direkt, med alla planer aktiveras. Användaren har lagts till en grupp som har Office 365 Enterprise **E3** produkten som är kopplade till den. Den här produkten innehåller serviceplaner som inte får överlappa för energischeman som ingår i E1, så licenstilldelning gruppen misslyckas med fel ”i konflikt serviceplaner”. I det här exemplet är i konflikt service-planer

-   SharePoint Online (planera 2) i konflikt med SharePoint Online (planera 1).
-   Exchange Online (planera 2) i konflikt med Exchange Online (planera 1).

Du kan lösa konflikten måste du inaktivera de två planer antingen på E1 licensen som är direkt kopplade till användaren. Eller så måste du ändra licenstilldelningen för hela gruppen och inaktivera planer i E3-licens. Alternativt kanske du vill ta bort E1-licens från användaren om det är redundant i samband med E3-licens.

Beslut om hur du löser motstridiga produktlicenser alltid tillhör administratören. Azure AD lösa inte automatiskt konflikter licens.

**PowerShell:** PowerShell-cmdlets rapportera felet som _MutuallyExclusiveViolation_.

## <a name="other-products-depend-on-this-license"></a>Andra produkter är beroende av den här licensen

**Problem:** en av de produkter som anges i gruppen innehåller en serviceplan som måste aktiveras för en annan service-plan i en annan produkt ska fungera. Det här felet uppstår när Azure AD som försöker ta bort den underliggande service-planen. Detta kan inträffa på grund av användaren tas bort från gruppen.

För att lösa problemet måste du se till att planen krävs fortfarande är tilldelad till användare via någon annan metod eller att de beroende tjänsterna är inaktiverat för dessa användare. När du har gjort som du ska ta bort gruppen licens från dessa användare.

**PowerShell:** PowerShell-cmdlets rapportera felet som _DependencyViolation_.

## <a name="usage-location-isnt-allowed"></a>Användningsplats är inte tillåten

**Problem:** vissa Microsoft-tjänster är inte tillgängliga på alla platser på grund av lokala lagar och förordningar. Du måste ange egenskapen ”användningsplats” för användaren innan du kan tilldela en användare en licens. Du kan göra detta under den **användare** > **profil** > **inställningar** avsnitt i Azure-portalen.

När Azure AD försöker tilldela en licens för grupp till en användare vars användningsplats inte stöds, misslyckas och registrera det här felet på användaren.

Om du vill lösa problemet genom att ta bort användare från platser som inte stöds från gruppen licensierade. Alternativt, om de aktuella värdena för användning plats inte representerar den faktiska platsen, du kan ändra dem så att licenserna som tilldelas korrekt nästa gång (om den nya platsen stöds).

**PowerShell:** PowerShell-cmdlets rapportera felet som _ProhibitedInUsageLocationViolation_.

> [!NOTE]
> När Azure AD tilldelar grupp licenser, ärver alla användare utan en användningsplats anges platsen för katalogen. Vi rekommenderar att administratörer korrekt användning plats värden på användare innan du använder gruppbaserade licensiering för att följa lokala lagar och förordningar.

## <a name="what-happens-when-theres-more-than-one-product-license-on-a-group"></a>Vad händer när det finns fler än en produktlicens på en grupp?

Du kan tilldela fler än en produktlicensen till en grupp. Exempelvis kan du tilldela en grupp för att enkelt aktivera alla inkluderade tjänster för användare med Office 365 Enterprise E3 och Enterprise Mobility + Security.

Azure AD försöker tilldela alla licenser som anges i gruppen för varje användare. Om Azure AD kan inte tilldelas en av produkterna på grund av logiken affärsproblem (till exempel om det inte finns tillräckligt många licenser för alla eller om det är i konflikt med andra tjänster som är aktiverade på användaren), den antingen inte tilldela licenser i gruppen.

Du kommer att kunna se vilka användare som inte kunnat tilldelas misslyckades och kontrollera vilka produkter som har påverkats av detta.

## <a name="license-assignment-fails-silently-for-a-user-due-to-duplicate-proxy-addresses-in-exchange-online"></a>Licenstilldelning misslyckas utan meddelanden för en användare på grund av dubbla proxyadresser i Exchange Online

Om du använder Exchange Online kanske vissa användare i din klientorganisation är felaktigt konfigurerade med samma värde för proxyadress. När gruppbaserade licensiering försöker tilldela en licens till användaren, den misslyckas och kommer inte att registrera ett fel (till skillnad från i andra fall i fel ovan) – detta är en begränsning i av förhandsversionen av den här funktionen och vi kommer att åtgärda detta innan *allmän tillgänglighet*.

> [!TIP]
> Om du märker att vissa användare fick inte en licens och det finns inget fel registreras på de användarna du först kontrollera om de har dubbla proxyadress.
> Detta kan göras genom att köra följande PowerShell-cmdlet mot Exchange Online:
```
Run Get-Recipient | where {$_.EmailAddresses -match "user@contoso.onmicrosoft.com"} | fL Name, RecipientType,emailaddresses
```
> [Den här artikeln](https://support.microsoft.com/help/3042584/-proxy-address-address-is-already-being-used-error-message-in-exchange-online) innehåller mer information om det här problemet, inklusive information om [hur du ansluter till Exchange Online med fjärr-PowerShell](https://technet.microsoft.com/library/jj984289.aspx).

När proxy lösa eventuella problem har lösts för de berörda användarna, se till att tvinga licens bearbetning på gruppen för att kontrollera licenser kan nu användas igen.

## <a name="how-do-you-force-license-processing-in-a-group-to-resolve-errors"></a>Hur du för att tvinga licens bearbetning i en grupp för att åtgärda fel?

Beroende på vilka steg som du har gjort för att lösa problem kan det vara nödvändigt att utlösa bearbetning av en grupp för att uppdatera användartillstånd manuellt.

Till exempel om du har frigjort vissa licenser genom att ta bort direkt licenstilldelning från användare måste starta bearbetning av grupper som tidigare inte har fullständigt licens alla användare medlemmar. För att göra det, hitta bladet, öppna **licenser**, och välj den **Ombearbeta** i verktygsfältet.

## <a name="next-steps"></a>Nästa steg

Mer information om andra scenarier för hantering av programvarulicenser genom grupper finns i följande avsnitt:

* [Tilldela licenser till en grupp i Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md)
* [Vad är gruppbaserade licensiering i Azure Active Directory?](active-directory-licensing-whatis-azure-portal.md)
* [Hur du migrerar enskilda licensierade användare till gruppbaserade licensiering i Azure Active Directory](active-directory-licensing-group-migration-azure-portal.md)
* [Azure Active Directory gruppbaserade licensiering fler scenarier](active-directory-licensing-group-advanced.md)
