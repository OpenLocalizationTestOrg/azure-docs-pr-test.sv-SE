---
title: "aaaGet igång med licensiering i Azure Active Directory | Microsoft Docs"
description: "Hur Azure Active Directory-licensiering fungerar, hur tooget igång med bästa praxis"
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
ms.date: 07/26/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: H1Hack27Feb2017;it-pro
ms.openlocfilehash: 268dab806b8b959790341d630a0355c6a43871d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="license-yourself-and-your-users-in-azure-active-directory"></a>Licens dig och dina användare i Azure Active Directory

> [!div class="op_single_selector"]
> * [Azure portal instruktioner](active-directory-licensing-get-started-azure-portal.md)
> * [Hämta Azure klassiska portal info](active-directory-licensing-what-is.md)
>
>

Azure Active Directory (AD Azure) är en identitet som en tjänst (IDaaS) och plattform från Microsoft. Azure AD finns i olika versioner:

* Azure Active Directory ledigt, som är tillgängliga med alla Microsoft-tjänster, till exempel Office 365, Dynamics, Microsoft Intune och Azure. Azure AD genererar inte eventuella kostnader för användning i det här läget.

* Azure AD betald utgåvor som:
  - Enterprise Mobility + Security 
  - Azure AD Premium (P1 och P2)
  - Azure AD Basic
  - Azure Multi-Factor Authentication

Precis som många Microsoft online services som mest Azure AD betald versioner levereras via per användare rättigheter som i Office 365, Microsoft Intune och Azure AD. I dessa fall hello service köp representeras av en eller flera prenumerationer och varje prenumeration omfattar vissa prepurchased licenser i din klient. Per användare rättigheter uppnås genom att:

* Tilldela en licens. 
* Skapa en länk mellan hello användar- och hello produkten.
* Aktiverar hello tjänstkomponenter för hello användare.
* Använda en av hello förskottsbetald licenser.

[Försök Azure AD Premium nu.](https://portal.office.com/Signup/Signup.aspx?OfferId=01824d11-5ad8-447f-8523-666b0848b381&ali=1#0)

En översikt av funktionerna i Azure AD-tjänsten finns [vad är Azure AD?](active-directory-whatis.md). Mer information finns i vår [serviceavtal sidan](https://azure.microsoft.com/support/legal/sla/active-directory/v1_0/).

> [!NOTE]
> Azure-prenumeration med användningsbaserad prenumerationer skapa ett Azure-resurser och sedan avbilda dem tooyour betalningsmetod. Det finns inga licensantal som är associerade med hello prenumeration. När du tilldelar en användare behörighet toooperate på Azure-resurser mappas toohello prenumeration associeras med hello prenumerationen och kan hantera prenumerationsresurser.

## <a name="how-does-azure-active-directory-licensing-work"></a>Hur fungerar licensiering i Azure Active Directory?

Licens-baserad Azure AD-tjänster fungerar genom att aktivera en prenumeration i Azure AD-tjänsten-klienten. När hello prenumeration är aktiv, hanteras av Azure AD-administratörer hello funktioner och används av licensierade användare.

### <a name="manage-subscription-information"></a>Hantera prenumerationsinformation

När du köper Enterprise Mobility + Security, Azure AD Premium eller Azure AD Basic, din klient uppdateras med hello-prenumeration, inklusive giltighetsperioden och förskottsbetald licenser. Din prenumerationsinformation hello antalet tilldelade eller tillgängliga licenser är tillgängliga via hello Azure-portalen: Under **Azure Active Directory**öppnar hello **licenser** panelen för hello specifik katalog. Hej **licenser** bladet är också hello bästa plats toomanage licenstilldelningar.

Varje prenumeration består av en eller flera serviceplaner, t.ex Azure AD, Multifaktorautentisering, Intune, Exchange Online eller SharePoint Online.  Azure AD-licenshantering har *inte* kräver management service-plan-nivå. Office 365 skiljer sig eftersom det är beroende av den här avancerad konfiguration läge toomanage tooincluded åtkomsttjänster. Azure AD beroende bruk configuration tooenable funktioner och hantera enskilda behörigheter.

> [!IMPORTANT]
> Azure AD Premium, Azure AD Basic och Enterprise Mobility + Security prenumerationer är begränsat tootheir etablerats directory /-klient. Prenumerationer kan inte delas mellan kataloger eller använda tooentitle användare i andra kataloger. Flytta en prenumeration mellan kataloger är möjligt, men måste skicka ett supportärende eller avbryta och återköp för direkt inköp.
>
> När Azure AD eller Enterprise Mobility + Security har köpts via en Volume Licensing-prenumeration och när hello avtalet innehåller andra Microsoft Online-tjänster (till exempel Office 365), aktivering sker automatiskt. 

### <a name="assign-licenses"></a>Tilldela licenser

Även om hur du skaffar en prenumeration är alla måste tooconfigure betald funktioner, måste du fortfarande distribuera licenser för Azure AD betald funktioner toousers. Alla användare som ska ha åtkomst tooor som hanteras via en Azure AD betald funktionen måste tilldelas en licens. Licenstilldelning finns en mappning mellan en användare och en köpta tjänst, till exempel Azure AD Premium, Basic eller Enterprise Mobility + Security.


Hantera vilka användare i din katalog ska ha en licens kan åstadkommas genom att: 

* Tilldela licenser toogroups i hello [Azure-portalen](active-directory-licensing-whatis-azure-portal.md).
* Tilldela licenser direkt toousers som hello-portalen, PowerShell eller API: er. 

När du tilldelar licenser tooa grupp har alla medlemmar tilldelats en licens. Om användare läggs till eller tas bort från hello-gruppen, tilldelas hello rätt licens eller tas bort. Grupptilldelning kan utnyttja alla tillgängliga tooyou för grupp-hantering och konsekvent med gruppbaserad tilldelning tooapplications.

Du kan använda [gruppbaserade licenstilldelning](active-directory-licensing-whatis-azure-portal.md) tooset regler, till exempel hello följande:
* Alla användare i din katalog få automatiskt en licens
* Alla med hello lämpliga befattning hämtar en licens
* Du kan delegera hello beslut tooother chefer i hello organisation (med hjälp av [självbetjäning grupper](active-directory-accessmanagement-self-service-group-management.md))

En detaljerad beskrivning av licens tilldelning toogroups, inklusive avancerade scenarier och Office 365-licensiering scenarier, finns i [tilldela licenser toousers genom medlemskap i Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md).

## <a name="get-started-with-azure-ad-licensing"></a>Kom igång med Azure AD-licensiering

Det är lätt att komma igång med Azure AD. Du kan alltid skapa katalogen som en del av att registrera dig för en [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/offers/ms-azr-0044p/).

hello kan följande säkerhetsmetoder se till att din klient är i linje med andra Microsoft-tjänster som du kan använda och målen för hello-tjänsten:

- Om du redan använder någon av hello organisationens tjänster från Microsoft kan har du redan en Azure AD-klient. Det är användbart toouse hello samma klient för andra tjänster så att core Identitetshantering, inklusive etablering och hybrid enkel inloggning (SSO) kan användas för hello-tjänster. Med enkel inloggning dra användarna nytta av hello omfattande funktioner för hello-tjänster. Om du väljer toobuy en Azure AD betald-tjänsten för din arbetsstyrka, rekommenderar vi därför att du använder hello samma klient igen.

- Vi rekommenderar att du använder en ny klient i hello Azure-portalen om du planerar att:
  - Använda Azure AD för en annan uppsättning användare (till exempel partners eller kunder).
  - Utvärdera Azure AD-tjänster avskilt från produktionstjänsten.
  - Ställ in begränsat läge för dina tjänster.

  hello ny katalog skapas med ditt konto som en extern användare med globala administratörsbehörigheter. Du kan se den här innehavaren och åtkomst till alla administrationsuppgifter när du loggar in toohello Azure-portalen med det här kontot.

> [!NOTE]
> Azure AD stöder ”gästanvändare”, som är användarkonton i Azure AD-klient som har skapats via ett Microsoft-konto eller en identitet för Azure AD från en annan klient. hello Office 365-administrationsportalen stöder för närvarande inte dessa användare. Gästanvändare med Microsoft-konton är inte kan tooaccess hello Office 365-administrationsportalen, medan gästanvändare från andra Azure AD-klienter kommer att ignoreras. I hello senare fallet endast hello användarens lokala konto i hello Azure AD eller Office 365-klient där hello användaren ursprungligen skapades, är tillgänglig.

### <a name="select-one-or-more-license-trials"></a>Välj en eller flera licens försök

Du kan aktivera en Azure AD Premium eller Enterprise Mobility + Security utvärderingsprenumeration under **Azure Active Directory** &gt; **Snabbstart**.

![Välj en licens-utvärderingsversion](media/active-directory-licensing-get-started-azure-portal/select-a-license-trial.png)

Utvärderingsversion licenser är tillgängliga på hello **licenser** bladet.

### <a name="assign-licenses-toousers-and-groups"></a>Tilldela licenser toousers och grupper

När hello prenumeration är aktiv, bör du tilldela en licens tooyourself. Uppdatera sedan hello webbläsare tooensure att du ser alla hello-funktioner. hello nästa steg är tooassign licenser toohello användare som behöver åtkomst toopaid Azure AD-funktioner. Enligt beskrivningen i [tilldela licenser](#assign-licenses), ett enkelt sätt tooassign licenser är tooidentify hello grupp som representerar hello önskad målgrupp och tilldela hello licens tooit. På så sätt kan användare som har lagts till eller tagits bort från hello över dess livscykel tilldelad eller tas bort från hello licens respektive.

> [!NOTE]
> Vissa Microsoft-tjänster är inte tillgängliga på alla platser. Innan en licens kan tilldelas tooa användare, hello administratören måste ange hello **användningsplats** egenskapen för hello användare. Du kan ange egenskapen under **användare** &gt; **profil** &gt; **inställningar** i hello Azure-portalen. När du använder gruppen licenstilldelning ärver alla användare vars användningsplats anges hello platsen för hello katalog.

tooassign en licens under **Azure Active Directory** &gt; **licenser** &gt; **alla produkter**, Välj en eller flera produkter och väljer sedan **Tilldela** i hello kommandofält.

![Välj en licens tooassign](media/active-directory-licensing-get-started-azure-portal/select-license-to-assign.png)

Du kan använda hello **användare och grupper** bladet toochoose flera användare, grupper eller toodisable service-planer i hello produkten. Använd hello sökrutan på översta toosearch för användar-och gruppnamn.

![Välj en användare eller grupp för licenstilldelning](media/active-directory-licensing-get-started-azure-portal/select-user-for-license-assignment.png)

När du tilldelar en tooa licensgrupp, kan det ta lite tid innan alla användare ärver hello licens, beroende på hello antalet användare i gruppen hello. Du kan kontrollera status för bearbetning av hello på hello **grupp** bladet under hello **licenser** panelen.

![Licensstatus för tilldelning](media/active-directory-licensing-get-started-azure-portal/license-assignment-status.png)

Tilldelningsfel kan uppstå under Azure AD-licenstilldelning men är relativt ovanliga vid hantering av Azure AD och Enterprise Mobility + Security-produkter. Potentiella Tilldelningsfel är begränsade till:
- Tilldelningen konflikt: när en användare tidigare tilldelats en licens som inte är kompatibel med aktuellt hello-licens. I det här fallet kräver tilldela hello ny licens att ta bort hello aktuella.
- Överskred tillgängliga licenser: när hello antalet användare i tilldelade grupper överskrider hello tillgängliga licenser, status för en användares klienttilldelning visar en tooassign misslyckades på grund av toomissing licenser.

#### <a name="azure-ad-b2b-collaboration-licensing"></a>Azure AD B2B-samarbete och licensiering

B2B-samarbete kan du tooinvite gästanvändare i din Azure AD-klient tooprovide åtkomst tooAzure AD-tjänster och alla Azure-resurser som du gör tillgängliga.  

Det är gratis för bjuda in B2B-användare och tilldela dem tooan program i Azure AD. In too10 appar per gäst kan användar- och 3 grundläggande rapporter även för B2B-samarbete användare. Om din gästanvändare har lämpliga licenser tilldelade i hello partner Azure AD-klient, ska de licensieras med din egen samt.

Det krävs inte, men om du vill tooprovide åtkomstfunktioner toopaid Azure AD B2B gäst användarna måste ha licens med lämpliga licenser för Azure AD. En bjuda in klient med en Azure AD betald licens kan tilldela användarrättigheter för B2B-samarbete tooan ytterligare fem gästanvändare inbjuden toohello klient. Scenarier och information finns i [B2B-samarbete licensiering vägledning](active-directory-b2b-licensing.md).

### <a name="view-assigned-licenses"></a>Visa tilldelade licenser

En sammanfattning av tilldelade och tillgängliga licenser visas under **Azure Active Directory** &gt; **licenser** &gt; **alla produkter**.

![Visa licens sammanfattning](media/active-directory-licensing-get-started-azure-portal/view-license-summary.png)

En detaljerad lista över tilldelade användare och grupper är tillgänglig när du väljer en viss produkt. Hej **licensierade användare** listan visas alla användare som för närvarande använder en licens och om hello licens tilldelades direkt toohello användare eller om den har ärvts från en grupp.

![Visa information om licens](media/active-directory-licensing-get-started-azure-portal/view-license-detail.png)

På liknande sätt hello **licensierad grupper** listan visas alla grupper toowhich licenser har tilldelats. Välj en användare eller grupp tooopen hello **licenser** som visar alla licenser som tilldelats toothat objekt.

### <a name="remove-a-license"></a>Ta bort en licens

tooremove en licens gå toohello användaren eller gruppen och öppna hello **licenser** panelen. Välj hello licens och klicka på **ta bort**.

![Ta bort en licens](media/active-directory-licensing-get-started-azure-portal/remove-license.png)

Licenser som ärvs av hello användaren från en grupp kan inte tas bort direkt. I stället tar du bort hello användare från hello-grupp som de ska ärva hello-licens.

### <a name="extend-trials"></a>Utöka försök

Utvärderingsversion tillägg för kunder som är tillgängliga som självbetjäning registrering via hello Office 365-portalen. En kund-administratör kan gå toohello Office-portalen (åtkomst beror på behörigheter för hello Office-portalen) och välj hello Azure AD Premium-utvärderingsversion. Klicka på hello **förlänga utvärderingsperioden** länk startar hello-tilläggsprocess. Ett kreditkort krävs, men debiteras inte.

![Utöka en utvärderingsversion i hello Azure-portalen](media/active-directory-licensing-get-started-azure-portal/extend-trial-beginning.png)

## <a name="next-steps"></a>Nästa steg

Mer om toolearn avancerade scenarier för hantering av programvarulicenser genom grupper, läsa hello artikel [tilldela licenser tooa grupp](active-directory-licensing-group-assignment-azure-portal.md).

Här finns information om hur tooconfigure och använda andra Azure AD betald funktioner:

* [Lösenordsåterställning via självbetjäning](active-directory-manage-passwords.md)
* [Grupphantering via självbetjäning](active-directory-accessmanagement-self-service-group-management.md)
* [Azure AD Connect health](active-directory-aadconnect-health.md)
* [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)
* [Inköp av Azure AD Premium-licenser](http://aka.ms/buyaadp)
