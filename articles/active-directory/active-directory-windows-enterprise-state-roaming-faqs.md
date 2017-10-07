---
title: "aaaSettings och dataroaming vanliga frågor och svar | Microsoft Docs"
description: "Ger svar toosome frågor IT-administratörer kan ha om inställningar och data appsynkronisering."
services: active-directory
keywords: "Enterprise tillstånd centrala inställningar för windows-moln, vanliga frågor och svar för enterprise tillstånd centrala"
documentationcenter: 
author: tanning
manager: swadhwa
editor: curtand
ms.assetid: c0824f5c-129b-4240-969f-921f6a64eae7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
ms.openlocfilehash: 4d6d6619b3a5fbd1d274603808d89b73ed942cd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="settings-and-data-roaming-faq"></a>Vanliga frågor och svar om inställningar och dataväxling
Det här avsnittet besvarar några frågor som IT-administratörer kan ha om inställningar och data appsynkronisering.

## <a name="what-data-roams"></a>Vilka data flyttas?
**Windows-inställningar**: hello datorinställningar som är inbyggda i operativsystemet för Windows hello. I allmänhet dessa finns inställningar som anpassar datorn och de inkluderar hello följande kategorier:

* *Temat*, som innehåller funktioner, till exempel inställningar för skrivbord teman och Aktivitetsfältet.
* *Internet Explorer-inställningar*, inklusive nyligen öppnade flikar och Favoriter.
* *Kant webbläsarinställningar*, till exempel läsa listan och Favoriter.
* *Lösenord*, inklusive lösenord för Internet och Wi-Fi-profiler.
* *Språkinställningar*, som innehåller inställningar för tangentbordslayouter, systemspråk, datum och tid.
* *Enkel åtkomstfunktioner*, till exempel hög kontrast tema, Skärmförstoraren och Skärmläsaren.
* *Andra Windows-inställningar*, till exempel inställningar för Kommandotolken och lista.

**Programdata**: universella Windows-appar kan skriva inställningar data tooa centrala mappen och alla data som skrivs toothis mappen synkroniseras automatiskt. Nu är det upp toohello enskilda app developer toodesign en app tootake fördel med den här funktionen. Mer information om hur toodevelop en universell Windows-appar som använder centrala, se hello [appdata lagring API](https://msdn.microsoft.com/library/windows/apps/mt299098.aspx) och hello [Windows 8 appdata centrala bloggen för utvecklare](http://blogs.msdn.com/b/windowsappdev/archive/2012/07/17/roaming-your-app-data.aspx).

## <a name="what-account-is-used-for-settings-sync"></a>Vilket konto används för synkronisering av inställningar?
I Windows 8 och Windows 8.1 används synkronisera inställningar alltid konsumenten Microsoft-konton. Företagsanvändare hade hello möjlighet tooconnect ett Microsoft-konto tootheir toosettings synkronisering av Active Directory-domän konto toogain åtkomst. I Windows 10 anslutna detta Microsoft-konto funktioner ersätts med ett konto för primära och sekundära ramverk.

hello primära konto har definierats som hello konto toosign i tooWindows. Detta kan vara ett Microsoft-konto, ett konto i Azure Active Directory (Azure AD), en lokal Active Directory-konto eller ett lokalt konto. I tillägg toohello primära konto, Windows 10-användare kan lägga till en eller flera sekundära moln konton tootheir enhet. Ett sekundärt konto är vanligtvis ett Microsoft-konto, en Azure AD konto eller några andra t.ex Gmail eller Facebook. Dessa sekundära konton ger åtkomst till tooadditional tjänster, till exempel enkel inloggning och hello Windows Store, men de kan inte stänga synkronisera inställningar.

I Windows 10, endast hello primära konto för hello-enhet kan användas för synkronisering av inställningar (se [hur uppgraderar jag från inställningar för synkronisering av Microsoft-konto i Windows 8 tooAzure AD inställningar för synkronisering i Windows 10?](active-directory-windows-enterprise-state-roaming-faqs.md#how-do-i-upgrade-from-microsoft-account-settings-sync-in-windows-8-to-azure-ad-settings-sync-in-windows-10)).

Data blandas aldrig mellan hello olika användarkonton på hello enhet. Det finns två regler för synkronisering av inställningar:

* Windows-inställningar kommer alltid flyttas med hello primära konto.
* AppData märks med hello konto som används för tooacquire hello app. Endast appar som är märkta med hello primära konto synkroniseras. Appen ägarskap taggning bestäms när en app är sidoladdad via hello Windows Store eller hantering av mobila enheter (MDM).

Om en app ägare kan identifieras, kommer den flyttas med hello primära konto. Om en enhet har uppgraderats från Windows 8 eller Windows 8.1 tooWindows 10 taggas alla hello-appar som skapats av hello Microsoft-konto. Detta beror på att de flesta användare skaffa appar via hello Windows Store och det fanns inget Windows Store-stöd för Azure AD-konton tidigare tooWindows 10. Om en app har installerats via en offline-licens kommer hello app taggas med hello primära konto på hello enhet.

> [!NOTE]
> Windows 10-enheter som ägs av företaget och är anslutna tooAzure AD kan inte längre ansluta sitt Microsoft-konton tooa domän-konto. Hej möjlighet tooconnect ett domänkonto för Microsoft-konto tooa och ha alla hello användarens datasynkronisering toohello Microsoft-konto (det vill säga hello Microsoft-konto centrala via hello anslutna Microsoft-konto och Active Directory-funktioner) tas bort från Windows 10-enheter som är kopplade tooa anslutna Active Directory eller Azure AD-miljö.
>
>

## <a name="how-do-i-upgrade-from-microsoft-account-settings-sync-in-windows-8-tooazure-ad-settings-sync-in-windows-10"></a>Hur uppgraderar jag från inställningar för synkronisering av Microsoft-konto i Windows 8 tooAzure AD inställningar för synkronisering i Windows 10?
Om du är ansluten toohello Active Directory-domän som kör Windows 8 eller Windows 8.1 med anslutna Microsoft-konto synkroniseras inställningarna via ditt Microsoft-konto. När du har uppgraderat tooWindows 10 fortsätter toosync användarinställningar via Microsoft-konto så länge som du är ansluten till domänen användare och hello Active Directory-domän kan inte anslutas till Azure AD.

Om hello lokala Active Directory-domän kan anslutas med Azure AD, försöker enheten toosync inställningar med hjälp av hello anslutna Azure AD-kontot. Om hello Azure AD-administratör inte kan Enterprise tillstånd växling dina anslutna Azure AD-kontot kommer sluta synkronisera inställningar. Om du är en Windows 10-användare och du loggar in med en Azure AD-identitet, startar du synkroniserar windows-inställningar som administratören kan synkronisera inställningar via Azure AD.

Om du har lagrat personliga data på enheten företagets bör du vara medveten att Windows-Operativsystemet och program börjar synkronisera tooAzure AD. Detta har hello följande effekter:

* Inställningarna för ditt personliga Microsoft-konto kommer driva förutom hello inställningar på ditt arbete eller skola Azure AD-konton. Detta beror på att hello Microsoft-konto och Azure AD-inställningar synkronisera nu använder separata konton.
* Personliga data, till exempel Wi-Fi-lösenord, web autentiseringsuppgifter och Favoriter i Internet Explorer som redan har synkroniserats via ett anslutna Microsoft-konto synkroniseras via Azure AD.

## <a name="how-do-microsoft-account-and-azure-ad-enterprise-state-roaming-interoperability-work"></a>Hur gör Microsoft-konto och Azure AD Enterprise tillstånd centrala samverkan arbete?
I hello November 2015 eller senare versioner av Windows 10 stöds Enterprise tillstånd centrala bara för ett enskilt konto i taget. Om du loggar in tooWindows genom att använda ett arbets- eller skolkonto Azure AD-kontot, synkroniseras alla data via Azure AD. Om du loggar in tooWindows med ett personligt microsoftkonto synkroniseras alla data via hello Microsoft-konto. Universal appdata ska flyttas med endast hello primära inloggning konto på hello enhet och det kommer flyttas endast om hello applicensen ägs av hello primära konto. Universal appdata för hello-appar som ägs av alla sekundära konton synkroniseras inte.

## <a name="do-settings-sync-for-azure-ad-accounts-from-multiple-tenants"></a>Inställningar synkroniseras för Azure AD-konton från flera klienter?
När flera Azure AD-konton från olika Azure AD-klienter är på hello samma enhet, måste du uppdatera hello enhetens registret toocommunicate med Azure Rights Management (Azure RMS) för varje Azure AD-klient.  

1. Hitta hello GUID för varje Azure AD-klient. Öppna hello klassiska Azure-portalen och välj en Azure AD-klient. hello GUID för hello klient finns i hello URL: en i hello adressfältet i webbläsaren. Exempel: `https://manage.windowsazure.com/YourAccount.onmicrosoft.com#Workspaces/ActiveDirectoryExtension/Directory/Tenant GUID/directoryQuickStart`
2. När du har hello GUID måste tooadd hello registernyckeln **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\SettingSync\WinMSIPC\<klient-ID-GUID >**.
   Från hello **klient-ID-GUID** nyckeln, skapa ett nytt flersträngsvärde (REG-MULTI-SZ) med namnet **AllowedRMSServerUrls**. Ange hello licensiering distribution point URL: er för hello andra Azure-klienter som hello enhetsåtkomster för sina data.
3. Du kan hitta hello licensiering distributionspunkterna genom att köra hello **Get-AadrmConfiguration** cmdlet. Om hello värden för hello **LicensingIntranetDistributionPointUrl** och **LicensingExtranetDistributionPointUrl** är olika, ange båda värdena. Om värdena är hello hello samma, ange hello värde en gång.

## <a name="what-are-hello-roaming-settings-options-for-existing-windows-desktop-applications"></a>Vad är hello centrala inställningsalternativ för befintliga Windows-datorprogram?
Centrala fungerar bara för universella Windows-appar. Det finns två alternativ för att aktivera roaming på ett befintligt stationära Windows-program:

* Hej [Desktop Bridge](http://aka.ms/desktopbridge) hjälper dig att konfigurera din befintliga Windows-program toohello universella Windows-plattformen. Från den här krävs minimal kod ändringar kommer att tootake nytta av Azure AD-appdata. hello Desktop Bridge ger dina appar med en app-identitet som behövs tooenable AppData för befintliga skrivbordsprogram.
* [Användarens upplevelse virtualisering (UE-V)](https://technet.microsoft.com/library/dn458947.aspx) kan du skapa en mall för anpassade inställningar för befintliga Windows-program och aktivera växling för Win32-appar. Det här alternativet kräver inte hello app developer toochange koden för hello app. UE-V är begränsad tooon lokala Active Directory som är centrala för kunder som har köpt hello Microsoft Desktop Optimization Pack.

Administratörer kan konfigurera UE-V tooroam Windows-skrivbordsapp data genom att ändra centrala inställningar för Windows-Operativsystemet och universell AppData via [grupprinciper för UE-V](https://technet.microsoft.com/itpro/mdop/uev-v2/configuring-ue-v-2x-with-group-policy-objects-both-uevv2), inklusive:

* Flytta Grupprincip för Windows-inställningar
* Synkronisera inte Grupprincip för Windows-appar
* Internet Explorer hello program under nätverksväxling

I framtida hello, Microsoft undersöka sätt toomake djupt integrerad UE-V i Windows och utöka UE-V tooroam inställningar via hello Azure AD-molnet.

## <a name="can-i-store-synced-settings-and-data-on-premises"></a>Kan jag lagra synkroniserade inställningar och data lokalt?
Enterprise tillstånd centrala lagrar alla synkroniserade data i hello Azure-molnet. UE-V erbjuder en lokal centrala lösning.

## <a name="who-owns-hello-data-thats-being-roamed"></a>Vem äger hello data som är att flyttade?
Hej företag egna hello data flyttade via Enterprise tillstånd nätverksväxling. Data lagras i ett Azure-datacenter. Alla användardata krypteras både under överföring och i vila i hello molnet med Azure RMS. Det här är en förbättring jämfört med tooMicrosoft konto-baserade inställningar sync, som krypterar vissa känsliga data, till exempel autentiseringsuppgifter innan de lämnar hello enhet.

Microsoft är allokerat toosafeguarding kundens data. En enterprise-användarens inställningsdata krypteras automatiskt av Azure RMS innan de lämnar en Windows 10-enhet, så att andra användare kan läsa informationen. Om din organisation har en betald prenumeration för Azure RMS, du kan använda andra Azure RMS-funktioner, till exempel spåra och återkalla dokument automatiskt skydda e-postmeddelanden som innehåller känslig information och hantera egna nycklar (hello ”bring your own key”-lösning också kallas BYOK). Mer information om de här funktionerna och hur Azure RMS fungerar finns [vad är Azure Rights Management](https://technet.microsoft.com/jj585026.aspx).

## <a name="can-i-manage-sync-for-a-specific-app-or-setting"></a>Kan jag hantera synkronisering för en viss app eller inställningen?
Det finns ingen MDM eller en Grupprincip inställningen toodisable roaming för ett enskilt program i Windows 10. Innehavaradministratörer kan inaktivera appdata synkronisering för alla program på en hanterad enhet, men det finns ingen ökad kontroll på en per app eller i app-nivå.

## <a name="how-can-i-enable-or-disable-roaming"></a>Hur kan aktivera eller inaktivera centrala?
I hello **inställningar** app, gå för**konton** > **synkronisera dina inställningar**. Den här sidan ser du vilket konto som håller på att använda tooroam inställningar och du kan aktivera eller inaktivera enskilda grupper av inställningar toobe flyttade.

## <a name="what-is-microsofts-recommendation-for-enabling-roaming-in-windows-10"></a>Vad är Microsofts rekommendation för att aktivera centrala i Windows 10?
Microsoft har några olika inställningar för roaming lösningar som är tillgängliga, inklusive centrala användarprofiler, UE-V och Enterprise tillstånd centrala.  Microsoft är allokerat toomaking en investering i Företagsroaming i framtida versioner av Windows. Om din organisation inte är klar eller föredrar att flytta data toohello moln, sedan rekommenderar vi att du använder UE-V som din primära centrala teknik. Om din organisation kräver centrala stöd för befintliga program för Windows-skrivbordet, men är gärna toomove toohello moln, rekommenderar vi att du använder Enterprise tillstånd centrala och UE-V. Men UE-V- och Enterprise tillstånd centrala är mycket liknande tekniker, är de inte ömsesidigt uteslutande. De kompletterar varandra toohelp se till att din organisation innehåller hello centrala tjänster som användarna behöver.  

När du använder Enterprise tillstånd centrala och UE-V, tillämpa hello följande regler:

* Enterprise tillstånd växling är hello primära centrala agent på hello enhet. UE-V är att använda toosupplement hello ”Win32 lucka”.
* UE-V roaming för Windows-inställningar och data med moderna UWP-app ska inaktiveras när principerna för hello UE-V-grupp. Dessa omfattas redan av centrala Enterprise-tillstånd.

## <a name="how-does-enterprise-state-roaming-support-virtual-desktop-infrastructure-vdi"></a>Hur Enterprise tillstånd centrala stöder virtuell datorinfrastruktur (VDI)?
Enterprise tillstånd centrala stöds på Windows 10-klient SKU: er, men inte på servern SKU: er. Om en klient VM finns på en hypervisor-dator och du loggar in via fjärranslutning toohello virtuella datorn, kommer dina data flyttas. Om flera användare delar samma OS och användare loggar in via fjärranslutning tooa server för en fullständig Skrivbordsmiljö hello, kanske roaming inte fungerar. hello stöds senare sessionsbaserad officiellt inte.

## <a name="what-happens-when-my-organization-purchases-azure-rms-after-using-roaming"></a>Vad händer när organisationen köper Azure RMS när du använder centrala?
Om din organisation redan använder nätverksväxling i Windows 10 med hello Azure RMS begränsad användning kostnadsfria prenumerationen köpa en betald prenumeration på Azure RMS har inte någon effekt på hello funktionaliteten för hello centrala och inga konfigurationsändringar kommer att krävs av IT-administratören.

## <a name="known-issues"></a>Kända problem
Se hello-dokumentationen i hello [felsökning](active-directory-windows-enterprise-state-roaming-troubleshooting.md) avsnittet för en lista över kända problem. 

## <a name="related-topics"></a>Relaterade ämnen
* [Övergripande översikt över Enterprise tillstånd](active-directory-windows-enterprise-state-roaming-overview.md)
* [Aktivera företagsroaming i Azure Active Directory](active-directory-windows-enterprise-state-roaming-enable.md)
* [Gruppen principer och MDM-inställningar för synkronisering av inställningar](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Centrala referens för Windows 10](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
* [Felsökning](active-directory-windows-enterprise-state-roaming-troubleshooting.md)
