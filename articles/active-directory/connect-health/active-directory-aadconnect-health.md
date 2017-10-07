---
title: aaaMonitor din lokala identitetsinfrastruktur i hello moln.
description: "Det här är hello Azure AD Connect Health sida som beskriver vad det är och varför du vill använda den."
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 82798ea6-5cd3-4f30-93ae-d56536f8d8e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 84d0b00ec800ba98094343731aa4e7317dfb0c61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-your-on-premises-identity-infrastructure-and-synchronization-services-in-hello-cloud"></a>Övervaka din lokala identitet infrastruktur och synkroniseringstjänster i molnet hello
Azure Active Directory (AD Azure) Connect Health hjälper dig att övervaka och få insikter om din identitet med lokal infrastruktur och hello Sync services. Det gör att du toomaintain en tillförlitlig anslutning tooOffice 365 och Microsoft Online Services genom att tillhandahålla övervakningsfunktioner för viktiga identitetskomponenter, till exempel Active Directory Federation Services (AD FS) servrar, Azure AD Connect-servrar (kallas även som Synkroniseringsmotorn), Active Directory-domänkontrollanter osv. Gör det också hello viktiga datapunkter om komponenterna lättåtkomlig så att du kan få användning och andra viktiga insikter toomake informerat beslut.

hello informationen visas i hello [Azure AD Connect Health-portalen](https://aka.ms/aadconnecthealth). Du kan visa aviseringar, prestandaövervakning, användningsanalys och annan information i hello Azure AD Connect Health-portalen. Azure AD Connect Health kan hello åtkomst till all hälsoinformation om dina viktiga identitetskomponenter på samma plats.

![Vad är Azure AD Connect Health?](./media/active-directory-aadconnect-health/aadconnecthealth2.png)

Eftersom hello funktioner i Azure AD Connect Health ökar hello och ger en enda instrumentpanel via hello lins identitet. Du får en ännu mer robust, felfri och integrerad miljö för dina användare tooincrease sina möjlighet tooget saker gjorda.

## <a name="why-use-azure-ad-connect-health"></a>Varför ska jag använda Azure AD Connect Health?
När du integrerar dina lokala kataloger med Azure AD är användarna mer produktiva eftersom det finns en gemensam identitet tooaccess både molnet och lokala resurser. Men skapar den här integreringen hello utmaning att säkerställa att den här miljön är felfri så att användare på ett tillförlitligt sätt kan komma åt resurser både lokalt och i hello molnet från valfri enhet. Azure AD Connect Health hjälper dig att övervaka och få insikter om den lokala identitetsinfrastrukturen som används tooaccess Office 365 eller andra Azure AD-program. Det enda du behöver göra är att installera en agent på alla lokala identitetsservrar.

## <a name="azure-ad-connect-health-for-ad-fsactive-directory-aadconnect-health-adfsmd"></a>[Azure AD Connect Health för AD FS](active-directory-aadconnect-health-adfs.md)
Azure AD Connect Health för AD FS stöder AD FS 2.0 på Windows Server 2008 R2, Windows Server 2012 och Windows Server 2012 R2. Det stöder också övervakning hello AD FS-proxy eller webbprogramproxyservrarna som tillhandahåller autentisering stöd för åtkomst till extranät. Med en enkel och billig installation av hello Hälsoagenten tillhandahåller Azure AD Connect Health för AD FS hello följande uppsättning viktiga funktioner:

* Övervakning med aviseringar tooknow när AD FS och AD FS-proxyservrar inte är felfria
* E-postmeddelanden för viktiga aviseringar.
* Trender i prestandadata, vilket är användbart för kapacitetsplanering för AD FS.
* Användningsanalys för AD FS inloggningar med vrider (appar, användare, nätverksplats osv), som är användbar toounderstand hur AD FS används
* Rapporter för AD FS, till exempel de 50 användarna med flest misslyckade inloggningsförsök på grund av felaktigt användarnamn/lösenord och deras senaste IP-adresser.

hello ger följande videoklipp en översikt över Azure AD Connect Health för AD FS.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD-Connect-Health--Monitor-you-identity-bridge/player]
>
>

## <a name="azure-ad-connect-health-for-syncactive-directory-aadconnect-health-syncmd"></a>[Azure AD Connect Health för synkronisering](active-directory-aadconnect-health-sync.md)
Azure AD Connect Health för synkronisering övervakar och ger information om hello synkroniseringar som sker mellan din lokala Active Directory och Azure AD. Azure AD Connect Health för synkronisering innehåller hello följande uppsättning viktiga funktioner:

* Övervakning med aviseringar tooknow när en Azure AD Connect-server, kallas även Synkroniseringsmotorn hello, är inte felfri
* E-postmeddelanden för viktiga aviseringar.
* Synkronisera åtgärdsinformation inklusive diagram över svarstider för synkroniseringsåtgärder och trender inom olika åtgärder som tilläggs-, uppdaterings- och borttagningsåtgärder.
* Lätt att se information om synkroniseringsegenskaper och senaste lyckade exportera tooAzure AD
* Rapporter om synkroniseringsfel på objektnivå \(kräver inte Azure AD Premium\).

hello ger följande videoklipp en översikt över Azure AD Connect Health för synkronisering.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-Health-Monitoring-the-sync-engine/player]
>
>

## <a name="azure-ad-connect-health-for-ad-dsactive-directory-aadconnect-health-addsmd"></a>[Azure AD Connect Health för AD DS](active-directory-aadconnect-health-adds.md)
Azure AD Connect Health för Active Directory Domain Services (AD DS) tillhandahåller övervakning för domänkontrollanter som har installerats på Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 och Windows Server 2016. hello Health Agent installation kan du toomonitor dina lokala AD DS-miljön från hello molnet. Azure AD Connect Health för AD DS innehåller hello följande uppsättning viktiga funktioner:

* Övervakning notifieringar toodetect när domänkontrollanter är felfria och e-postmeddelanden för viktiga aviseringar.
* hello domänkontrollanter instrumentpanel, vilket ger en snabb överblick över hello hälsa och driftstatus för domänkontrollanter
* hello replikeringsstatus instrumentpanel som har Hej senaste replikeringsinformation och länkar tootroubleshooting guider om fel hittas
* Snabb tillgång tooperformance data diagram över populära prestandaräknare som är nödvändiga för felsökning och övervakning

hello ger följande videoklipp en översikt över Azure AD Connect Health för AD DS.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD-Connect-Health-monitors-on-premises-AD-Domain-Services/player]
>
>

## <a name="get-started-with-azure-ad-connect-health"></a>Komma igång med Azure AD Connect Health
tooget igång med Azure AD Connect Health, Använd hello följande steg:

1. [Hämta Azure AD Premium](../active-directory-get-started-premium.md) eller [starta en utvärderingsperiod](https://azure.microsoft.com/trial/get-started-active-directory/).
2. [Ladda ned och installera Azure AD Connect Health-agenter](#download-and-install-azure-ad-connect-health-agent) på dina identitetsservrar.
3. Visa hello Azure AD Connect Health-instrumentpanelen på [https://aka.ms/aadconnecthealth](https://aka.ms/aadconnecthealth).

> [!NOTE]
> Kom ihåg att innan du se data i Azure AD Connect Health-instrumentpanelen, behöver du tooinstall hello Azure AD Connect Health-agenter på dina målservrar.
>
>

## <a name="download-and-install-azure-ad-connect-health-agent"></a>Ladda ned och installera Azure AD Connect Health-agenten
* Se till att du [uppfyller kraven för hello](active-directory-aadconnect-health-agent-install.md#requirements) för Azure AD Connect Health.
* Kom igång med Azure AD Connect Health för AD FS
    * [Hämta Azure AD Connect Health Agent för AD FS.](http://go.microsoft.com/fwlink/?LinkID=518973)
    * [Se hello Installationsinstruktioner](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs).
* Kom igång med Azure AD Connect Health för synkronisering
    * [Hämta och installera hello senaste versionen av Azure AD Connect](http://go.microsoft.com/fwlink/?linkid=615771). hello Health Agent för synkronisering installeras som en del av hello Azure AD Connect-installationen (version 1.0.9125.0 eller senare).
* Kom igång med Azure AD Connect Health för AD DS
    * [Ladda ned Azure AD Connect Health Agent för AD DS](http://go.microsoft.com/fwlink/?LinkID=820540).
    * [Se hello Installationsinstruktioner](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-ds).

## <a name="azure-ad-connect-health-portal"></a>Azure AD Connect Health-portalen
hello Azure AD Connect Health-portalen visar vyer för varningar, övervaka prestanda och användningsanalys. Hej https://aka.ms/aadconnecthealth tar URL toohello huvudbladet i Azure AD Connect Health. Tänk dig ett blad som ett fönster. Hej huvudblad, visas **Snabbstart**, tjänster i Azure AD Connect Health och ytterligare konfigurationsalternativ. Se hello följande skärmbild och korta förklaringar som följer hello skärmbild. När du har distribuerat hello agenter anger hello-tjänsten för hälsotillstånd automatiskt hello-tjänster som Azure AD Connect Health övervakar.

> [!NOTE]
> Mer licensieringsinformation finns hello [Azure AD Connect vanliga frågor och svar](active-directory-aadconnect-health-faq.md) eller hello [priser för Azure AD-sidan](https://aka.ms/aadpricing).
    
![Azure AD Connect Health-portalen](./media/active-directory-aadconnect-health/portal4.png)

* **Snabbstart**: när du väljer det här alternativet hello **Snabbstart** blad öppnas. Du kan hämta hello Azure AD Connect Health Agent genom att välja **hämta verktyg**. Du kan också komma åt dokumentationen och ge feedback.
* **Active Directory Federation Services**: det här alternativet visas alla hello AD FS-tjänster som Azure AD Connect Health övervakar för tillfället. När du väljer en instans, visar information om den tjänstinstansen hello bladet som öppnas. Den här informationen innehåller en översikt, egenskaper, aviseringar, övervakning och användningsanalys. Läs mer om hello funktionerna i [med hjälp av Azure AD Connect Health med AD FS](active-directory-aadconnect-health-adfs.md).
* **Azure Active Directory Connect (synkronisering)**: När du väljer det här alternativet visas de Azure AD Connect-servrar som Azure AD Connect Health övervakar för tillfället. När du väljer hello post visar hello bladet som öppnas information om Azure AD Connect-servrar. Läs mer om hello funktionerna i [med hjälp av Azure AD Connect Health för synkronisering](active-directory-aadconnect-health-sync.md).
* **Active Directory Domain Services**: det här alternativet visas alla hello AD DS-skogar att Azure AD Connect Health övervakar för tillfället. När du väljer en skog, visar information om skogen hello bladet som öppnas. Den här informationen innehåller en översikt över viktig information, hello domänkontrollanter instrumentpanelen, hello replikeringsstatus instrumentpanelen, aviseringar och övervakning. Läs mer om hello funktionerna i [med hjälp av Azure AD Connect Health med AD DS](active-directory-aadconnect-health-adds.md).
* **Konfigurera**: det här avsnittet innehåller alternativ tooturn hello följande eller inaktivera:

  - Automatisk uppdatering tooautomatically hello Azure AD Connect Health agent toohello senaste uppdateringsversionen: du kommer att uppdateras automatiskt toohello senaste versionerna av hello Azure AD Connect Health Agent när de blir tillgängliga. Den här funktionen är aktiverad som standard.
  - Tillåt Microsoft access tooyour Azure AD katalogs hälsodata i felsökningssyfte: om den är aktiverad, se Microsoft hello samma data som visas. Den här informationen kan komma till nytta vid felsökning och om du behöver hjälp med olika problem. Alternativet är inaktiverat som standard.

## <a name="related-links"></a>Relaterade länkar
* [Installation av Azure AD Connect Health Agent](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health-åtgärder](active-directory-aadconnect-health-operations.md)
* [Använda Azure AD Connect Health med AD FS](active-directory-aadconnect-health-adfs.md)
* [Använda Azure AD Connect Health för synkronisering](active-directory-aadconnect-health-sync.md)
* [Använda Azure AD Connect Health med AD DS](active-directory-aadconnect-health-adds.md)
* [Vanliga frågor och svar om Azure AD Connect Health](active-directory-aadconnect-health-faq.md)
* [Versionshistorik för Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)
