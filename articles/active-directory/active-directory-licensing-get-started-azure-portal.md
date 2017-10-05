---
title: "Kom igång med licensiering i Azure Active Directory | Microsoft Docs"
description: "Hur Azure Active Directory-licensiering fungerar, hur du kommer igång med bästa praxis"
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
ms.openlocfilehash: 6fa7cbbc452861870136482aa320d268e78fe3d9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
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

Precis som många Microsoft online services som mest Azure AD betald versioner levereras via per användare rättigheter som i Office 365, Microsoft Intune och Azure AD. I dessa fall kan tjänsten köpet representeras av en eller flera prenumerationer och varje prenumeration omfattar vissa prepurchased licenser i din klient. Per användare rättigheter uppnås genom att:

* Tilldela en licens. 
* Skapa en länk mellan användaren och produkten.
* Aktiverar tjänstkomponenter för användaren.
* Använda en av de förbetalda licenserna.

[Försök Azure AD Premium nu.](https://portal.office.com/Signup/Signup.aspx?OfferId=01824d11-5ad8-447f-8523-666b0848b381&ali=1#0)

En översikt av funktionerna i Azure AD-tjänsten finns [vad är Azure AD?](active-directory-whatis.md). Mer information finns i vår [serviceavtal sidan](https://azure.microsoft.com/support/legal/sla/active-directory/v1_0/).

> [!NOTE]
> Azure-prenumeration med användningsbaserad prenumerationer aktivera skapandet av Azure-resurser och mappa dem till din betalningsmetod. Det finns inga licensantal som är associerade med prenumerationen. När du tilldelar en användarbehörighet att använda Azure-resurser mappas till prenumerationen är associerade med prenumerationen och kan hantera prenumerationsresurser.

## <a name="how-does-azure-active-directory-licensing-work"></a>Hur fungerar licensiering i Azure Active Directory?

Licens-baserad Azure AD-tjänster fungerar genom att aktivera en prenumeration i Azure AD-tjänsten-klienten. Efter att prenumerationen är aktiv, service-funktionerna hanteras av Azure AD-administratörer och används av licensierade användare.

### <a name="manage-subscription-information"></a>Hantera prenumerationsinformation

När du köper Enterprise Mobility + Security, Azure AD Premium eller Azure AD Basic har din klient uppdaterats med prenumeration, inklusive dess giltighetstid och förbetalda licenser. Din prenumerationsinformation antalet tilldelade eller tillgängliga licenser är tillgängliga via Azure portal: Under **Azure Active Directory**öppnar den **licenser** panelen för specifik katalog. Den **licenser** bladet är också den bästa platsen för att hantera din licens.

Varje prenumeration består av en eller flera serviceplaner, t.ex Azure AD, Multifaktorautentisering, Intune, Exchange Online eller SharePoint Online.  Azure AD-licenshantering har *inte* kräver management service-plan-nivå. Office 365 skiljer sig eftersom det är beroende av den här avancerad konfiguration-läge för att hantera åtkomst till tjänster som ingår. Azure AD är beroende av bruk konfiguration för att aktivera funktioner och hantera enskilda behörigheter.

> [!IMPORTANT]
> Azure AD Premium, Azure AD Basic och Enterprise Mobility + Security prenumerationer är begränsade till deras etablerade directory /-klient. Prenumerationer kan inte delas upp mellan kataloger eller används för att berättiga användare i andra kataloger. Flytta en prenumeration mellan kataloger är möjligt, men måste skicka ett supportärende eller avbryta och återköp för direkt inköp.
>
> När Azure AD eller Enterprise Mobility + Security har köpts via en Volume Licensing-prenumeration och när avtalet innehåller andra Microsoft Online-tjänster (till exempel Office 365), aktivering sker automatiskt. 

### <a name="assign-licenses"></a>Tilldela licenser

Även om hur du skaffar en prenumeration är allt du behöver konfigurera betalda funktioner, måste du fortfarande distribuera licenser för Azure AD betald funktioner för användare. Alla användare som ska ha åtkomst till eller som hanteras via en Azure AD betald funktionen måste tilldelas en licens. Licenstilldelning finns en mappning mellan en användare och en köpta tjänst, till exempel Azure AD Premium, Basic eller Enterprise Mobility + Security.


Hantera vilka användare i din katalog ska ha en licens kan åstadkommas genom att: 

* Tilldela licenser till grupper i den [Azure-portalen](active-directory-licensing-whatis-azure-portal.md).
* Tilldela licenser direkt till användare via portalen, PowerShell eller API: er. 

När du tilldelar licenser till en grupp, kan alla medlemmar har tilldelats en licens. Om användare läggs till eller tas bort från gruppen, tilldelas rätt licens eller tas bort. Grupptilldelning kan utnyttja alla tillgängliga grupphantering och konsekvent med gruppbaserad tilldelning till program.

Du kan använda [gruppbaserade licenstilldelning](active-directory-licensing-whatis-azure-portal.md) att ställa in regler som t.ex följande:
* Alla användare i din katalog få automatiskt en licens
* Alla med lämpliga befattning hämtar en licens
* Du kan delegera beslutet till andra chefer i organisationen (med hjälp av [självbetjäning grupper](active-directory-accessmanagement-self-service-group-management.md))

En detaljerad beskrivning av licenstilldelning till grupper, inklusive avancerade scenarier och Office 365-licensiering scenarier, finns i [tilldela licenser till användare av medlemskap i Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md).

## <a name="get-started-with-azure-ad-licensing"></a>Kom igång med Azure AD-licensiering

Det är lätt att komma igång med Azure AD. Du kan alltid skapa katalogen som en del av att registrera dig för en [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/offers/ms-azr-0044p/).

Följande säkerhetsmetoder kan hjälpa till att säkerställa att din klient är i linje med andra Microsoft-tjänster som du kan använda och målen för tjänsten:

- Om du redan använder någon av de organisatoriska tjänsterna från Microsoft kan har du redan en Azure AD-klient. Det är praktiskt att använda samma klientorganisation för andra tjänster så att core Identitetshantering, inklusive etablering och hybrid enkel inloggning (SSO), kan du använda tjänster. Med enkel inloggning dra användarna nytta av de avancerade funktionerna tjänster. Om du vill köpa en Azure AD betald service för din arbetsstyrka, rekommenderar vi därför att du använder samma klientorganisation igen.

- Vi rekommenderar att du använder en ny klient i Azure portal om du planerar att:
  - Använda Azure AD för en annan uppsättning användare (till exempel partners eller kunder).
  - Utvärdera Azure AD-tjänster avskilt från produktionstjänsten.
  - Ställ in begränsat läge för dina tjänster.

  Den nya katalogen skapas med ditt konto som en extern användare med globala administratörsbehörigheter. Du kan se den här innehavaren och åtkomst till alla administrationsuppgifter när du loggar in på Azure-portalen med det här kontot.

> [!NOTE]
> Azure AD stöder ”gästanvändare”, som är användarkonton i Azure AD-klient som har skapats via ett Microsoft-konto eller en identitet för Azure AD från en annan klient. Office 365-administrationsportalen stöder för närvarande inte dessa användare. Gästanvändare med Microsoft-konton kan inte komma åt Office 365-administrationsportalen, när gästanvändare från andra Azure AD-klienter kommer att ignoreras. I det senare fallet endast användarens lokala kontot, i Azure AD eller Office 365-klient där användaren ursprungligen skapades, är tillgänglig.

### <a name="select-one-or-more-license-trials"></a>Välj en eller flera licens försök

Du kan aktivera en Azure AD Premium eller Enterprise Mobility + Security utvärderingsprenumeration under **Azure Active Directory** &gt; **Snabbstart**.

![Välj en licens-utvärderingsversion](media/active-directory-licensing-get-started-azure-portal/select-a-license-trial.png)

Utvärderingsversion licenser är tillgängliga på den **licenser** bladet.

### <a name="assign-licenses-to-users-and-groups"></a>Tilldela licenser till användare och grupper

Efter att prenumerationen är aktiv, bör du tilldela en licens till dig själv. Uppdatera sedan webbläsaren för att se till att du ser alla funktioner. Nästa steg är att tilldela licenser till användare som behöver åtkomst till betald Azure AD-funktioner. Enligt beskrivningen i [tilldela licenser](#assign-licenses), ett enkelt sätt att tilldela licenser är att identifiera datorgruppen som representerar målgruppen och tilldela den licensen. På så sätt kan användare som läggs till eller tas bort från gruppen över dess livscykel tilldelad eller tas bort från licensen, respektive.

> [!NOTE]
> Vissa Microsoft-tjänster är inte tillgängliga på alla platser. Innan en användare kan tilldelas en licens, administratören måste ange den **användningsplats** -egenskapen för användaren. Du kan ange egenskapen under **användare** &gt; **profil** &gt; **inställningar** i Azure-portalen. När du använder gruppen licenstilldelning ärver alla användare vars användningsplats inte anges platsen för katalogen.

Tilldela en licens under **Azure Active Directory** &gt; **licenser** &gt; **alla produkter**, Välj en eller flera produkter och välj sedan **tilldela** i kommandofältet.

![Välj en licens för att tilldela](media/active-directory-licensing-get-started-azure-portal/select-license-to-assign.png)

Du kan använda den **användare och grupper** bladet att välja flera användare eller grupper eller inaktivera serviceplaner i produkten. Använda sökrutan överst för att söka efter användarnamn, gruppnamn.

![Välj en användare eller grupp för licenstilldelning](media/active-directory-licensing-get-started-azure-portal/select-user-for-license-assignment.png)

När du tilldelar en licens till en grupp, kan det ta lite tid innan alla användare ärver licens, beroende på antalet användare i gruppen. Du kan kontrollera Bearbetningsstatus av **grupp** bladet under den **licenser** panelen.

![Licensstatus för tilldelning](media/active-directory-licensing-get-started-azure-portal/license-assignment-status.png)

Tilldelningsfel kan uppstå under Azure AD-licenstilldelning men är relativt ovanliga vid hantering av Azure AD och Enterprise Mobility + Security-produkter. Potentiella Tilldelningsfel är begränsade till:
- Tilldelningen konflikt: när en användare tidigare tilldelats en licens som inte är kompatibel med den aktuella licensen. I det här fallet kräver tilldela nya licensen tar bort det aktuella.
- Överskred tillgängliga licenser: när antalet användare i tilldelade grupper överskrider de tillgängliga licenserna, status för en användares klienttilldelning återspeglar inte går att koppla på grund av saknade licenser.

#### <a name="azure-ad-b2b-collaboration-licensing"></a>Azure AD B2B-samarbete och licensiering

B2B-samarbete kan du bjuda in gästanvändare i Azure AD-klient för att ge åtkomst till Azure AD-tjänster och alla Azure-resurser som du gör tillgängliga.  

Det är gratis för bjuda in B2B-användare och tilldela dem till ett program i Azure AD. Upp till 10 appar per gästanvändare och 3 grundläggande rapporter också är gratis för B2B-samarbete användare. Om din gästanvändare har lämpliga licenser tilldelade i partnerns Azure AD-klient, ska de licensieras med din egen samt.

Det krävs inte, men om du vill ge åtkomst till betald Azure AD-funktioner, dessa B2B gästanvändare måste ha licens med lämpliga licenser för Azure AD. En bjuda in klient med en Azure AD betald licens kan tilldela B2B-samarbete användarrättigheter till en ytterligare fem gästanvändare bjudits in till klienten. Scenarier och information finns i [B2B-samarbete licensiering vägledning](active-directory-b2b-licensing.md).

### <a name="view-assigned-licenses"></a>Visa tilldelade licenser

En sammanfattning av tilldelade och tillgängliga licenser visas under **Azure Active Directory** &gt; **licenser** &gt; **alla produkter**.

![Visa licens sammanfattning](media/active-directory-licensing-get-started-azure-portal/view-license-summary.png)

En detaljerad lista över tilldelade användare och grupper är tillgänglig när du väljer en viss produkt. Den **licensierade användare** listan visas alla användare som för närvarande använder en licens och om licensen har tilldelats direkt till användaren eller om den har ärvts från en grupp.

![Visa information om licens](media/active-directory-licensing-get-started-azure-portal/view-license-detail.png)

På liknande sätt den **licensierad grupper** i listan visas alla grupper som har tilldelats licenser. Välj en användare eller grupp att öppna den **licenser** som visar alla licenser som är kopplade till det aktuella objektet.

### <a name="remove-a-license"></a>Ta bort en licens

Om du vill ta bort en licens, gå till användaren eller gruppen och öppna den **licenser** panelen. Välj licensvillkoren och klicka på **ta bort**.

![Ta bort en licens](media/active-directory-licensing-get-started-azure-portal/remove-license.png)

Licenser som ärvs av användaren från en grupp kan inte tas bort direkt. Ta bort användaren från gruppen från vilken de ärver licensen i stället.

### <a name="extend-trials"></a>Utöka försök

Utvärderingsversion tillägg för kunder som är tillgängliga som självbetjäning registrering via Office 365-portalen. En kund-administratör kan gå till Office-portalen (åtkomst beror på behörigheter för Office-portalen) och välj Azure AD Premium-utvärderingsversion. Klicka på den **förlänga utvärderingsperioden** länk startar processen tillägg. Ett kreditkort krävs, men debiteras inte.

![Utöka en utvärderingsversion i Azure-portalen](media/active-directory-licensing-get-started-azure-portal/extend-trial-beginning.png)

## <a name="next-steps"></a>Nästa steg

Mer information om avancerade scenarier för hantering av programvarulicenser genom grupper artikeln [tilldela licenser till en grupp](active-directory-licensing-group-assignment-azure-portal.md).

Här finns information om hur du konfigurerar och använder andra Azure AD betald funktioner:

* [Lösenordsåterställning via självbetjäning](active-directory-manage-passwords.md)
* [Grupphantering via självbetjäning](active-directory-accessmanagement-self-service-group-management.md)
* [Azure AD Connect health](active-directory-aadconnect-health.md)
* [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)
* [Inköp av Azure AD Premium-licenser](http://aka.ms/buyaadp)
