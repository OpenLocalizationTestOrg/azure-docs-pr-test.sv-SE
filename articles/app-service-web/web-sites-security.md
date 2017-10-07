---
title: aaaSecure en app i Azure App Service
description: "Lär dig hur toosecure webbprogram, mobilappsserverdel och API-app i Azure App Service."
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
ms.openlocfilehash: fceef7963b29516f33602dcd50591d14309bf188
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="secure-an-app-in-azure-app-service"></a>Skydda en app i Azure App Service
Den här artikeln hjälper dig att komma igång med att skydda ditt webbprogram, mobilappsserverdel eller API-app i Azure App Service. 

Säkerhet i Azure App Service har två nivåer: 

* **Säkerhet för infrastruktur och plattform** -förtroende Azure toohave hello tjänster du behöver tooactually kör saker på ett säkert sätt i hello molnet.
* **Programsäkerhet** -du behöver toodesign hello själva appen på ett säkert sätt. Detta innefattar hur du integrerar med Azure Active Directory, hur du hanterar certifikat och hur du ser till att toodifferent tjänster kan kommunicera på ett säkert sätt. 

#### <a name="infrastructure-and-platform-security"></a>Säkerhet för infrastruktur och plattform
Eftersom Apptjänst underhåller hello virtuella Azure-datorer, lagring, nätverksanslutningar, web ramverk, hantering och integrering och mycket mer är aktivt skyddade och härdat och går igenom kraftig efterlevnad och kontrollerar att på ett kontinuerligt toomake som:

* Apptjänst-appar isoleras från båda hello Internet och från hello Azure-resurser för andra kunder.
* Kommunikation av hemligheter (t.ex. anslutningssträngar) mellan din App Service-appen och andra Azure-resurser (t.ex. SQL Database) i en resursgrupp håller sig inom Azure och mellan inte några nätverksgränser. Hemligheter krypteras alltid.
* All kommunikation mellan dina Apptjänst-app och externa resurser, till exempel PowerShell-hantering, kommandoradsgränssnitt, Azure SDK: er, REST API: er och hybridanslutningar, krypteras korrekt.
* 24-timmarsformat threat management skyddar Apptjänst resurser från skadlig kod, distribuerade denial of service (DDoS), man-in-the-middle (MITM) och andra hot. 

Mer information om infrastruktur och plattform säkerhet i Azure finns [Azure Säkerhetscenter](https://azure.microsoft.com/support/trust-center/security/).

#### <a name="application-security"></a>Programsäkerhet
Azure är ansvarig för att skydda hello infrastruktur och plattform som tillämpningsprogrammet körs på, men det är ditt ansvar toosecure tillämpningsprogrammet sig själv. Med andra ord måste toodevelop, distribuera och hantera din programkod och innehåll på ett säkert sätt. Utan detta, kan din programkod eller innehåll fortfarande vara utsatt toothreats som:

* SQL Injection
* Sessionskapning
* Cross-skriptkörning
* Programnivå MITM
* DDoS på programnivå

En fullständig beskrivning av säkerhetsaspekter för webbaserade program ligger utanför hello i det här dokumentet. Som en startpunkt för ytterligare vägledning om hur du skyddar ditt program finns hello [öppna Web Application säkerhet projekt (OWASP)](https://www.owasp.org/index.php/Main_Page), särskilt hello [topp 10 projektet.](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project), som visar hello aktuella topp 10 kritiska web application säkerhetsbrister, utifrån OWASP medlemmar.

## <a name="perform-penetration-testing-on-your-app"></a>Utföra intrång testa på din app
En av hello enklaste sätt tooget startas med testning för säkerhetsriskerna App Service-appen är toouse hello [integrering med Tinfoil Security](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) tooperform enkelklickning säkerhetsproblem genomsökning av din app. Du kan visa hello testresultaten i ett enkelt att förstå rapport och lära dig hur toofix varje säkerhetsproblem med stegvisa instruktioner.

Om du föredrar tooperform egna intrång testar eller vill toouse en annan skanner suite eller providern, måste du följa hello [Azure intrång testa godkännandeprocessen](https://security-forms.azure.com/penetration-testing/terms) och få godkännande i förväg tooperform hello önskad intrång tester.

## <a name="https"></a>Säker kommunikation med kunder
Om du använder hello  **\*. azurewebsites.net** domännamn som skapats för din App Service-appen du omedelbart använda HTTPS, som ett SSL-certifikat har angetts för alla  **\*. azurewebsites.net** domännamn. Om din plats använder en [domännamn](app-service-web-tutorial-custom-domain.md), du kan ladda upp ett SSL-certifikat för[aktivera HTTPS](app-service-web-tutorial-custom-ssl.md) för hello anpassade domäner.

Aktivera [HTTPS](https://en.wikipedia.org/wiki/HTTPS) skyddar mot MITM-attacker på hello kommunikationen mellan appen och dess användare.

## <a name="secure-data-tier"></a>Säker datanivå
Apptjänst hög integreras med SQL-databas, så att alla hello anslutningssträngar krypteras över hello-kort och endast dekrypteras på hello VM hello appen körs på *och* hello endast när appen körs. Dessutom Azure SQL Database innehåller många säkerhet funktioner toohelp du skydda dina programdata från cyber hot, inklusive [kryptering i vila](https://msdn.microsoft.com/library/dn948096.aspx), [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx), [ Dynamisk Datamaskning](../sql-database/sql-database-dynamic-data-masking-get-started.md), och [Hotidentifiering](../sql-database/sql-database-threat-detection.md). Om du har känsliga data- och kompatibilitetskrav Se [skydda SQL-databasen](../sql-database/sql-database-security-overview.md) för mer information om hur toosecure dina data.

Om du använder en tredje parts databasprovider, till exempel ClearDB, bör du kontakta hello leverantörens dokumentation direkt på säkerhetsmetoder.  

## <a name="develop"></a>Säker utveckling och distribution
### <a name="publishing-profiles-and-publish-settings"></a>Publicering profiler och inställningar för publicering
När du utvecklar program hanteringsuppgifter eller automatisera uppgifter med hjälp av verktyg som **Visual Studio**, **Web Matrix**, **Azure PowerShell** eller hello **Azure-kommandoradsgränssnittet (Azure CLI)**, du kan använda antingen en *Publiceringsinställningar* fil eller en *Publicera profil*. Båda typerna av programinstallationsfiler autentisera dig med Azure och bör skyddas tooprevent obehörig åtkomst.

* En **Publiceringsinställningar** filen innehåller
  
  * Azure prenumerations-ID
  * Ett certifikat som du kan använda tooperform hanteringsuppgifter för din prenumeration *utan tooprovide kontonamn eller lösenord*.
* En **Publicera profil** filen innehåller
  
  * Information för att publicera tooyour app

Om du använder ett verktyg som använder en publicera inställningar eller publicera profilfil importera hello-fil som innehåller hello publiceringsinställningarna eller profil i hello verktyget och sedan **ta bort** hello-filen. Om du vill hålla hello filen tooshare med andra som arbetar på hello-projektet till exempel lagra den på en säker plats som en *krypterade* katalogen med begränsad behörighet.

Dessutom bör du kontrollera hello importeras autentiseringsuppgifter skyddas. Till exempel **Azure PowerShell** och hello **Azure-kommandoradsgränssnittet (Azure CLI)** både lagra importerade informationen i dina **hemkatalog** ( *~*  för Linux eller OS X-datorer och */users/användarnamn* på Windows-datorer.) För extra säkerhet kan du för**kryptera** dessa platser med hjälp av kryptering verktygen för ditt operativsystem.

### <a name="configuration-settings-and-connection-strings"></a>Konfigurationsinställningar och anslutningssträngar
Det är vanliga praxis toostore anslutningssträngar, autentiseringsuppgifter och annan känslig information i konfigurationsfilerna. Tyvärr kanske filerna är exponerad på din webbplats eller kontrolleras i en gemensam databas exponera informationen. En enkel sökning i [GitHub](https://github.com), till exempel kan avslöja oräkneliga konfigurationsfiler med exponerade hemligheter i hello offentliga databaser.

Hej bästa praxis är tookeep denna information utanför konfigurationsfiler för din app. App Service kan du lagra konfigurationsinformation som en del av hello körningsmiljö som **appinställningar** och **anslutningssträngar**. hello-värden är synliga tooyour program vid körning via *miljövariabler* för de flesta programmeringsspråk. För .NET-program injekteras värdena i .NET-konfiguration vid körning. Förutom dessa situationer dessa konfigurationsinställningar förblir krypterade såvida du inte visa eller konfigurera dem med hjälp av hello [Azure Portal](https://portal.azure.com) eller verktyg, till exempel PowerShell eller hello Azure CLI. 

Lagring av konfigurationsinformation i App Service gör det möjligt för hello app administratören toolock ned känslig information för hello produktion appar. Utvecklare kan använda en separat uppsättning inställningar för utveckling av appar och hello inställningar automatiskt kan åsidosättas av hello inställningarna i Apptjänst. Inte ens hello utvecklare behöver tooknow hello hemligheter som konfigurerats för hello produktionsprogrammet. Mer information om hur du konfigurerar inställningar för appen och anslutningssträngar i App Service finns [konfigurera webbprogram](web-sites-configure.md).

### <a name="ftps"></a>FTPS
Azure App Service tillhandahåller säker FTP-åtkomst toohello filsystem för din app genom **FTPS**. Detta gör åtkomst toosecurely hello programkod på hello webbprogrammet samt diagnostik loggar. Det rekommenderas att du alltid använder FTPS i stället för FTP. 

hello FTPS länken för din app kan hittas med hello följande steg:

1. Öppna hello [Azure Portal](https://portal.azure.com).
2. Välj **Bläddra igenom alla**.
3. Från hello **Bläddra** bladet väljer **Apptjänster**.
4. Från hello **Apptjänster** bladet, Välj hello önskade app.
5. Hello app bladet välj **alla inställningar**.
6. Från hello **inställningar** bladet väljer **egenskaper**.
7. hello FTP och FTPS länkar finns på hello **inställningar** bladet. 

Mer information om FTPS finns [File Transfer Protocol](http://en.wikipedia.org/wiki/File_Transfer_Protocol).

## <a name="next-steps"></a>Nästa steg
Mer information om hello säkerhet för hello Azure-plattformen, information om rapportering en **säkerhetsincident eller missbruk**, eller tooinform Microsoft som du kommer att utföra **intrång testning** av din läser du avsnittet om hello säkerhet i hello [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/security/).

Mer information om **web.config** eller **applicationhost.config** filer i Apptjänst-appar finns [konfigurationsalternativ låsas upp i Azure App Service web apps](https://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/).

Mer information om loggningsinformation för Apptjänst-appar som kan vara användbart för identifiering av attacker, finns [aktivera diagnostikloggning](web-sites-enable-diagnostic-log.md).

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="whats-changed"></a>Nyheter
* En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

