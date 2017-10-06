---
title: "aaaResolve licens problem för en grupp i Azure Active Directory | Microsoft Docs"
description: "Hur tooidentify och Lös licens tilldelning problem när du använder Azure Active Directory gruppbaserade licensiering"
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
ms.openlocfilehash: ec9c74214a9e246f21fcf767b0bbd586ae702445
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="identify-and-resolve-license-assignment-problems-for-a-group-in-azure-active-directory"></a>Identifiera och lösa problem med licensen tilldelning för en grupp i Azure Active Directory

Gruppbaserade licensiering i Azure Active Directory (AD Azure) introducerar hello begreppet användare i en licensiering feltillstånd. I den här artikeln förklarar vi hello skäl varför användare kan hamna i det här tillståndet.

När du tilldelar licenser direkt tooindividual användare, utan att använda gruppbaserade licensiering misslyckas hello tilldelningen. Till exempel när du kör PowerShell-cmdlet för hello `Set-MsolUserLicense` för en användare hello cmdlet kan misslyckas av flera skäl som är relaterade toobusiness logik. Till exempel det kanske inte tillräckligt många licenser eller en konflikt mellan två serviceplaner som inte kan tilldelas på hello samma tid. hello problemet omedelbart rapporteras tillbaka tooyou.

När du använder gruppbaserade licensiering, hello samma fel inträffar, men de inträffa i bakgrunden hello när hello Azure AD-tjänsten tilldela licenser. Därför hello fel får inte vara meddelas tooyou omedelbart. I stället de noterad hello användarobjektet och sedan rapporteras via hello administrativa portal. Observera att hello ursprungliga avsiktshantering toolicense hello användaren aldrig går förlorade, men den registreras i ett feltillstånd för framtida undersökningar och upplösning.

## <a name="how-toofind-license-assignment-errors"></a>Hur toofind licens Tilldelningsfel

1. toofind användare i ett feltillstånd i en specifik grupp, öppna hello bladet för hello grupp. Under **licenser**, kommer det att ett meddelande visas om det finns några användare i ett feltillstånd.

![Gruppen felmeddelanden](media/active-directory-licensing-group-problem-resolution-azure-portal/group-error-notification.png)

2. Klicka på hello meddelande tooopen en lista över alla användare som påverkas. Du kan klicka på varje användare individuellt toosee mer information.

![Gruppen, lista över användare i feltillstånd](media/active-directory-licensing-group-problem-resolution-azure-portal/list-of-users-with-errors.png)

3. toofind alla grupper som innehåller minst ett fel på hello **Azure Active Directory** bladet välj **licenser** och sedan **översikt**. En info visas när vissa grupper kräver din uppmärksamhet.

![Översikt, information om grupper i feltillstånd](media/active-directory-licensing-group-problem-resolution-azure-portal/group-errors-widget.png)

4. Klicka på hello rutan toosee en lista över alla grupper med fel. Du kan klicka på varje grupp för mer information.

![Översikt över grupper med fel](media/active-directory-licensing-group-problem-resolution-azure-portal/list-of-groups-with-errors.png)


Nedan följer en beskrivning av varje potentiella problem och hello sätt tooresolve den.

## <a name="not-enough-licenses"></a>Det finns inte tillräckligt med licenser

**Problem:** det inte finns tillräckligt många tillgängliga licenser för en av hello produkter som anges i hello grupp. Du behöver tooeither köpa flera licenser för hello produkt eller frigör oanvända licenser från andra användare eller grupper.

toosee hur många licenser är tillgängliga finns för**Azure Active Directory** > **licenser** > **alla produkter**.

toosee vilka användare och grupper som förbrukar licenser, klicka på en produkt. Under **licensierade användare**, visas alla användare som har haft användarlicenser direkt eller via en eller flera grupper. Under **licensierad grupper**, visas alla grupper som har tilldelats produkten.

**PowerShell:** PowerShell-cmdlets rapportera felet som _CountViolation_.

## <a name="conflicting-service-plans"></a>Motstridiga service-planer

**Problem:** en hello produkter som anges i hello-grupp som innehåller en serviceplan som står i konflikt med en annan service-plan som redan är tilldelade toohello användaren via en annan produkt. Vissa serviceplaner konfigureras så att de inte kan tilldelas toohello samma användare som en annan relaterade service-plan.

Överväg att hello följande exempel. En användare har en licens för Office 365 Enterprise **E1** tilldelats direkt, med alla hello planer aktiveras. hello användaren har lagts tooa grupp som har hello Office 365 Enterprise **E3** produkten som tilldelats tooit. Den här produkten innehåller serviceplaner som inte överlappar med hello planer som ingår i E1, så hello grupp licenstilldelning misslyckas med hello ”i konflikt serviceplaner” fel. I det här exemplet är hello motstridiga service-planer

-   SharePoint Online (planera 2) i konflikt med SharePoint Online (planera 1).
-   Exchange Online (planera 2) i konflikt med Exchange Online (planera 1).

toosolve konflikten, behöver du toodisable dessa två planer antingen på hello E1 licens som direkt tilldelade toohello användare. Eller så du behöver toomodify hello hela gruppen licenstilldelning och inaktivera hello planer i hello E3-licens. Du kan också bestämma tooremove hello E1 licens från hello användaren om det är redundant hello gäller hello E3 licens.

hello beslut om hur tooresolve motstridiga produktlicenser alltid tillhör toohello administratör. Azure AD lösa inte automatiskt konflikter licens.

**PowerShell:** PowerShell-cmdlets rapportera felet som _MutuallyExclusiveViolation_.

## <a name="other-products-depend-on-this-license"></a>Andra produkter är beroende av den här licensen

**Problem:** en hello produkter som anges i hello-grupp som innehåller en serviceplan som måste aktiveras för en annan service-plan i en annan produkt ska fungera. Det här felet uppstår när Azure AD försöker tooremove hello underliggande service-plan. Detta kan inträffa på grund av hello användaren tas bort från hello grupp.

toosolve det här problemet behöver du toomake att planen hello krävs fortfarande är tilldelad toousers via någon annan metod, eller att hello beroende tjänster är inaktiverat för dessa användare. När du har gjort som du ska ta bort hello gruppen licens från dessa användare.

**PowerShell:** PowerShell-cmdlets rapportera felet som _DependencyViolation_.

## <a name="usage-location-isnt-allowed"></a>Användningsplats är inte tillåten

**Problem:** vissa Microsoft-tjänster är inte tillgängliga på alla platser på grund av lokala lagar och förordningar. Innan du kan tilldela en licens tooa användare ha toospecify hello ”användning” Platsegenskapen för hello användare. Du kan göra detta under hello **användare** > **profil** > **inställningar** avsnitt i hello Azure-portalen.

Azure AD försöker tooassign grupp licens tooa användare vars användningsplats inte stöds, misslyckas och registrera det här felet på hello användare.

toosolve problemet, ta bort användare från platser som inte stöds från hello licensierad grupp. Du kan också om hello aktuella plats värden inte representerar hello faktiska Användarplats, kan du ändra dem så att hello licenser tilldelas korrekt nästa gång (om hello ny plats stöds).

**PowerShell:** PowerShell-cmdlets rapportera felet som _ProhibitedInUsageLocationViolation_.

> [!NOTE]
> När Azure AD tilldelar grupp licenser, ärver alla användare utan en användningsplats anges hello platsen för hello katalog. Vi rekommenderar att administratörer hello korrekt användning plats värden på användare innan du använder gruppbaserade licensiering toocomply med lokala lagar och förordningar.

## <a name="what-happens-when-theres-more-than-one-product-license-on-a-group"></a>Vad händer när det finns fler än en produktlicens på en grupp?

Du kan tilldela fler än en produkt tooa licensgrupp. Exempelvis kan du tilldela Office 365 Enterprise E3 och Enterprise Mobility + Security tooa grupp tooeasily aktivera alla inkluderade tjänster för användare.

Azure AD försöker tooassign alla licenser som anges i hello grupp tooeach användare. Om Azure AD kan inte tilldela någon av hello produkter på grund av logiken affärsproblem (till exempel om det inte finns tillräckligt många licenser för alla eller om det finns konflikter med andra tjänster som är aktiverade på hello användare), den kommer inte att tilldela hello andra licenser i hello grupp antingen.

Du kommer att kunna toosee hello användare misslyckades tooget tilldelade misslyckades och kontrollera vilka produkter som har påverkats av detta.

## <a name="license-assignment-fails-silently-for-a-user-due-tooduplicate-proxy-addresses-in-exchange-online"></a>Licenstilldelning misslyckas tyst för en användare på grund av tooduplicate proxyadresser i Exchange Online

Om du använder Exchange Online, vissa användare i din klient kanske är felaktigt konfigurerad med hello samma värde för proxy-adress. När gruppbaserade licensiering försöker tooassign licens-toosuch en användare, den misslyckas och kommer inte att registrera ett fel (till skillnad från i hello andra fel fall som beskrivs ovan) – detta är en begränsning i hello förhandsversionen av den här funktionen och vi tooaddress det innan  *Allmän tillgänglighet*.

> [!TIP]
> Om du märker att vissa användare fick inte en licens och det finns inget fel registreras på de användarna du först kontrollera om de har dubbla proxyadress.
> Detta kan göras genom att köra följande PowerShell-cmdleten mot Exchange Online hello:
```
Run Get-Recipient | where {$_.EmailAddresses -match "user@contoso.onmicrosoft.com"} | fL Name, RecipientType,emailaddresses
```
> [Den här artikeln](https://support.microsoft.com/help/3042584/-proxy-address-address-is-already-being-used-error-message-in-exchange-online) innehåller mer information om det här problemet, inklusive information om [hur tooconnect tooExchange Online med fjärr-PowerShell](https://technet.microsoft.com/library/jj984289.aspx).

När proxy lösa eventuella problem har lösts för hello påverkade användare, kontrollerar du att tooforce licens bearbetning på hello grupp toomake att licenser kan nu användas igen.

## <a name="how-do-you-force-license-processing-in-a-group-tooresolve-errors"></a>Hur du för att tvinga licens som bearbetas i en grupp tooresolve fel?

Beroende på hur du har gjort tooresolve fel, kan det vara nödvändigt toomanually utlösaren bearbetning av en grupp tooupdate hello användarens tillstånd.

Till exempel om du har frigjort vissa licenser genom att ta bort direkt licenstilldelning från användare behöver tootrigger bearbetningen av grupper som tidigare inte toofully licens alla användare medlemmar. toodo som hitta hello blad, öppna **licenser**, och välj hello **Ombearbeta** knapp i toolbar hello.

## <a name="next-steps"></a>Nästa steg

toolearn mer information om andra scenarier för hantering av programvarulicenser genom grupper, finns följande hello:

* [Tilldela licenser tooa grupp i Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md)
* [Vad är gruppbaserade licensiering i Azure Active Directory?](active-directory-licensing-whatis-azure-portal.md)
* [Hur toomigrate individuella licensierade användare toogroup-baserade licensiering i Azure Active Directory](active-directory-licensing-group-migration-azure-portal.md)
* [Azure Active Directory gruppbaserade licensiering fler scenarier](active-directory-licensing-group-advanced.md)
