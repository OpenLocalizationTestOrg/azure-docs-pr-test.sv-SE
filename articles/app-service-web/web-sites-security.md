---
title: Skydda en app i Azure App Service
description: "Lär dig mer om att säkra en webbapp, mobilappsserverdel eller API-app i Azure App Service."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 5ce560b4-42d7-4b20-935c-0445fd539e39
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/12/2016
ms.author: cephalin
ms.openlocfilehash: b70d74441f3d6d9793ae516b3f04e36e786a9a8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="secure-an-app-in-azure-app-service"></a>Skydda en app i Azure App Service
Den här artikeln hjälper dig att komma igång med att skydda ditt webbprogram, mobilappsserverdel eller API-app i Azure App Service. 

Säkerhet i Azure App Service har två nivåer: 

* **Säkerhet för infrastruktur och plattform** -du litar på Azure om du vill att de tjänster som du behöver utföra saker på ett säkert sätt i molnet.
* **Programsäkerhet** -måste du utforma själva appen på ett säkert sätt. Detta innefattar hur du integrerar med Azure Active Directory, hur du hanterar certifikat och hur du ser till att du kan på ett säkert sätt tala med olika tjänster. 

#### <a name="infrastructure-and-platform-security"></a>Säkerhet för infrastruktur och plattform
Eftersom Apptjänst har Azure virtuella datorer, lagring, nätverksanslutningar, web ramverk, hantering och integrering och mycket mer är aktivt skyddade och härdat och går igenom kraftig efterlevnad och kontrollerar regelbundet kontrollera som:

* Apptjänst-appar isoleras från både Internet och från andra kunders Azure-resurser.
* Kommunikation av hemligheter (t.ex. anslutningssträngar) mellan din App Service-appen och andra Azure-resurser (t.ex. SQL Database) i en resursgrupp håller sig inom Azure och mellan inte några nätverksgränser. Hemligheter krypteras alltid.
* All kommunikation mellan dina Apptjänst-app och externa resurser, till exempel PowerShell-hantering, kommandoradsgränssnitt, Azure SDK: er, REST API: er och hybridanslutningar, krypteras korrekt.
* 24-timmarsformat threat management skyddar Apptjänst resurser från skadlig kod, distribuerade denial of service (DDoS), man-in-the-middle (MITM) och andra hot. 

Mer information om infrastruktur och plattform säkerhet i Azure finns [Azure Säkerhetscenter](https://azure.microsoft.com/support/trust-center/security/).

#### <a name="application-security"></a>Programsäkerhet
Medan Azure ansvarar för att skydda infrastruktur och plattform som tillämpningsprogrammet körs på, är ditt ansvar att skydda ditt program sig själv. Du behöver med andra ord att utveckla, distribuera och hantera din programkod och innehåll på ett säkert sätt. Utan detta, kan din programkod eller innehåll fortfarande vara sårbar för attacker som:

* SQL Injection
* Sessionskapning
* Cross-skriptkörning
* Programnivå MITM
* DDoS på programnivå

En fullständig beskrivning av säkerhetsaspekter för webbaserade program ligger utanför omfånget för det här dokumentet. Som en startpunkt för mer information om hur du skyddar ditt program finns i [öppna Web Application säkerhet projekt (OWASP)](https://www.owasp.org/index.php/Main_Page), särskilt de [topp 10 projektet.](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project), som listar de aktuella översta 10 värdena kritiska Web application säkerhetsbrister, enligt systemets OWASP medlemmar.

## <a name="perform-penetration-testing-on-your-app"></a>Utföra intrång testa på din app
Ett av de enklaste sätten att komma igång med att testa säkerhetsrisker på din Apptjänst-app är att använda den [integrering med Tinfoil Security](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) att utföra en enda klickning säkerhetsproblem genomsökning av din app. Du kan visa testresultaten i en rapport med enkelt att förstå och lär dig att åtgärda varje säkerhetsproblem med stegvisa instruktioner.

Om du vill utföra egna test för intrång eller vill använda en annan skanner suite eller providern, måste du följa den [Azure intrång testa godkännandeprocessen](https://security-forms.azure.com/penetration-testing/terms) och få godkännande i förväg för att utföra testerna önskade intrång.

## <a name="https"></a>Säker kommunikation med kunder
Om du använder den  **\*. azurewebsites.net** domännamn som skapats för din App Service-appen du omedelbart använda HTTPS, som ett SSL-certifikat har angetts för alla  **\*. azurewebsites.net** domännamn. Om din plats använder en [domännamn](app-service-web-tutorial-custom-domain.md), du kan ladda upp en SSL-certifikatet till [aktivera HTTPS](app-service-web-tutorial-custom-ssl.md) för den anpassade domänen.

Aktivera [HTTPS](https://en.wikipedia.org/wiki/HTTPS) skyddar mot MITM-attacker på kommunikationen mellan appen och dess användare.

## <a name="secure-data-tier"></a>Säker datanivå
Apptjänst hög kan integreras med SQL-databas, så att alla anslutningssträngar krypteras i alla och endast dekrypteras på den virtuella datorn som appen körs på *och* endast när appen körs. Dessutom Azure SQL Database innehåller många säkerhetsfunktioner för att hjälpa dig att skydda dina programdata från cyber hot, inklusive [kryptering i vila](https://msdn.microsoft.com/library/dn948096.aspx), [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx), [ Dynamisk Datamaskning](../sql-database/sql-database-dynamic-data-masking-get-started.md), och [Hotidentifiering](../sql-database/sql-database-threat-detection.md). Om du har känsliga data- och kompatibilitetskrav Se [skydda SQL-databasen](../sql-database/sql-database-security-overview.md) för mer information om hur du skyddar dina data.

Om du använder en tredje parts databasprovider, till exempel ClearDB, bör du kontakta med leverantörens dokumentation direkt på säkerhetsmetoder.  

## <a name="develop"></a>Säker utveckling och distribution
### <a name="publishing-profiles-and-publish-settings"></a>Publicering profiler och inställningar för publicering
När du utvecklar program hanteringsuppgifter eller automatisera uppgifter med hjälp av verktyg som **Visual Studio**, **Web Matrix**, **Azure PowerShell** eller  **Azure-kommandoradsgränssnittet (Azure CLI)**, du kan använda antingen en *Publiceringsinställningar* fil eller en *Publicera profil*. Båda typerna av programinstallationsfiler autentisera dig med Azure och bör vara skyddad och förhindra obehörig åtkomst.

* En **Publiceringsinställningar** filen innehåller
  
  * Azure prenumerations-ID
  * Ett hanteringscertifikat som du kan utföra hanteringsuppgifter för din prenumeration *utan att ange ett kontonamn eller lösenord*.
* En **Publicera profil** filen innehåller
  
  * Information för publicering till din app

Om du använder ett verktyg som använder en publicera inställningar eller publicera profilfil importera filen som innehåller publiceringsinställningarna eller profil i verktyget och sedan **ta bort** filen. Om du vill hålla filen att dela med andra som arbetar på projektet till exempel lagra den på en säker plats som en *krypterade* katalogen med begränsad behörighet.

Dessutom bör du kontrollera de importerade autentiseringsuppgifterna skyddas. Exempelvis **Azure PowerShell** och **Azure-kommandoradsgränssnittet (Azure CLI)** både lagra importerade informationen i din **-hemkataloger** ( *~*  för Linux eller OS X-datorer och */users/användarnamn* på Windows-datorer.) För extra säkerhet kan du **kryptera** dessa platser med hjälp av kryptering verktygen för ditt operativsystem.

### <a name="configuration-settings-and-connection-strings"></a>Konfigurationsinställningar och anslutningssträngar
Det är vanligt att man lagrar anslutningssträngar, autentiseringsuppgifter och annan känslig information i konfigurationsfilerna. Tyvärr kanske filerna är exponerad på din webbplats eller kontrolleras i en gemensam databas exponera informationen. En enkel sökning i [GitHub](https://github.com), till exempel kan avslöja oräkneliga konfigurationsfiler med exponerade hemligheter i offentliga databaser.

Det bästa sättet är att behålla den här informationen utanför konfigurationsfiler för din app. App Service kan du lagra konfigurationsinformation som en del av körningsmiljön som **appinställningar** och **anslutningssträngar**. Värdena som exponeras av ditt program vid körning via *miljövariabler* för de flesta programmeringsspråk. För .NET-program injekteras värdena i .NET-konfiguration vid körning. Förutom dessa situationer dessa konfigurationsinställningar förblir krypterade såvida du inte visa eller konfigurera dem med hjälp av den [Azure Portal](https://portal.azure.com) eller verktyg, till exempel PowerShell eller Azure CLI. 

Lagring av konfigurationsinformation i App Service gör det möjligt för appens administratör att låsa känslig information för produktion appar. Utvecklare kan använda en separat uppsättning inställningar för utveckling av appar och inställningarna kan åsidosättas automatiskt av inställningarna i Apptjänst. Inte ens utvecklare behöver veta hemligheter som konfigurerats för produktionsprogrammet. Mer information om hur du konfigurerar inställningar för appen och anslutningssträngar i App Service finns [konfigurera webbprogram](web-sites-configure.md).

### <a name="ftps"></a>FTPS
Azure App Service tillhandahåller säker FTP-åtkomst till filsystemet för din app genom **FTPS**. Detta gör att du kan på ett säkert sätt komma åt programkoden på webbprogrammet samt diagnostik loggar. Det rekommenderas att du alltid använder FTPS i stället för FTP. 

Länken FTPS för din app kan hittas med följande steg:

1. Öppna den [Azure-portalen](https://portal.azure.com).
2. Välj **Bläddra igenom alla**.
3. Från den **Bläddra** bladet väljer **Apptjänster**.
4. Från den **Apptjänster** bladet Välj önskad appen.
5. Appens bladet välj **alla inställningar**.
6. Från den **inställningar** bladet väljer **egenskaper**.
7. FTP och FTPS länkar finns på den **inställningar** bladet. 

Mer information om FTPS finns [File Transfer Protocol](http://en.wikipedia.org/wiki/File_Transfer_Protocol).

## <a name="next-steps"></a>Nästa steg
Mer information om säkerheten i Azure-plattformen, information om rapportering en **säkerhetsincident eller missbruk**, eller för att informera Microsoft som du kommer att utföra **intrång testning** av din webbplats Se Säkerhetsavsnittet i den [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/security/).

Mer information om **web.config** eller **applicationhost.config** filer i Apptjänst-appar finns [konfigurationsalternativ låsas upp i Azure App Service web apps](https://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/).

Mer information om loggningsinformation för Apptjänst-appar som kan vara användbart för identifiering av attacker, finns [aktivera diagnostikloggning](web-sites-enable-diagnostic-log.md).

> [!NOTE]
> Om du vill komma igång med Azure App Service innan du registrerar dig för ett Azure-konto går du till [prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="whats-changed"></a>Nyheter
* En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

