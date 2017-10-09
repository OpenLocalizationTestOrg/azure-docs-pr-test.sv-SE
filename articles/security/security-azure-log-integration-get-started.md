---
title: "aaaGet igång med Azure logganalys integration | Microsoft Docs"
description: "Lär dig hur tooinstall hello Azure logga integrationstjänsten och integrera loggar från Azure storage, Azure-granskningsloggarna och Azure Security Center-aviseringar."
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 53f67a7c-7e17-4c19-ac5c-a43fabff70e1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 07/26/2017
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 26c19070d76ff73b1bdbd32ba77fb04978af387e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-log-integration-with-azure-diagnostics-logging-and-windows-event-forwarding"></a>Azure logganalys-integrering med Azure Diagnostics loggning och vidarebefordran av Windows-händelser
Azure Log-integrering (AzLog) kan du toointegrate loggarna från Azure-resurser i ditt lokala Security Information and Event Management SIEM ()-system. Den här integreringen gör det möjligt toohave en enhetlig säkerhet instrumentpanel för alla tillgångar, lokalt eller i hello molnet så att du kan aggregera korrelera, analysera och varna för säkerhetshändelser som är kopplade till dina program.
>[!NOTE]
Mer information om Azure Log-integrering kan du granska hello [översikt över Azure Log-integrering](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview).

Den här artikeln hjälper dig att komma igång med Azure Log Integration genom att fokusera på hello installation av hello Azlog service och integrera hello service med Azure-diagnostik. hello Azure Log integrationstjänsten kommer sedan att kunna toocollect händelseloggen information från hello Windows händelse säkerhetskanal från virtuella datorer i Azure IaaS. Detta är mycket lik för ”vidarebefordran av händelser” som du har använt lokalt.

>[!NOTE]
>hello möjlighet toobring hello utdata från Azure logganalys-integrering i toohello SIEM tillhandahålls av hello SIEM sig själv. Hello artikel [integrera Azure Log-integrering med lokala SIEM](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) för mer information.

toobe tydliga - hello Azure Log-integrering-tjänsten körs på en fysisk eller virtuell dator som använder hello Windows Server 2008 R2 eller senare operativsystem (Windows Server 2012 R2 eller Windows Server 2016 rekommenderas).

hello fysiska datorn kan köra lokalt (eller på en plats för värden). Om du väljer toorun hello Azure Log Integration service på en virtuell dator som den virtuella datorn kan finnas lokalt eller i ett offentligt moln, till exempel Microsoft Azure.

hello fysisk eller virtuell dator som kör hello Azure Log integrationstjänsten kräver network connectivity toohello offentliga Azure-molnet. Stegen i den här artikeln innehåller detaljerad information om hello konfiguration.

## <a name="prerequisites"></a>Krav
Som minimum måste hello installera AzLog hello följande objekt:
* En **Azure-prenumeration**. Om du inte har en prenumeration kan du registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/free/).
* En **lagringskonto** som kan användas för Windows Azure diagnostikloggning (du kan använda en förkonfigurerad storage-konto eller skapa en ny – vi visar hur tooconfigure hello storage-konto senare i den här artikeln)
  >[!NOTE]
  Beroende på scenario kanske inte ett lagringskonto krävs. För hello Azure diagnostics scenariot beskrivs i den här artikeln som en sådan krävs.
* **Två system**: en dator som ska köra hello Azure Log-integrering tjänsten och en dator som ska övervakas och har dess loggningsinformation skickas toohello Azlog maskin.
   * En dator som du vill toomonitor – detta är en virtuell dator som körs som en [Azure-dator](../virtual-machines/virtual-machines-windows-overview.md)
   * En dator som ska köra hello Azure integration Loggtjänsten; den här datorn samlar in hello logginformation som senare ska importeras till din SIEM.
    * Systemet kan vara lokalt eller i Microsoft Azure.  
    * Kör en x64 toobe måste version av Windows server 2008 R2 SP1 eller senare och har .NET 4.5.1 har installerats. Du kan fastställa hello .NET-version som installeras med följande hello artikeln [så här: fastställa som .NET Framework-versioner är installerade](https://msdn.microsoft.com/library/hh925568)  
    Den måste ha anslutning toohello Azure storage-konto som används för Azure diagnostikloggning. Vi tillhandahåller anvisningarna nedan om hur du kan bekräfta den här anslutningen

Ta en titt på hello video nedan för en snabb demonstration av hello processen att skapa en virtuell dator med hello Azure-portalen.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Create-a-Virtual-Machine/player]



## <a name="deployment-considerations"></a>Distributionsöverväganden
Du kan använda alla system som uppfyller krav på operativsystem hello när du testar Azure Log-integrering. För en produktion miljö hello dock kräver belastningen tooplan för att skala upp eller ned.

Du kan köra flera instanser av hello Azure Log integrationstjänsten (en instans per fysisk eller virtuell dator) om händelsen volymen är hög. Dessutom kan kan du belastningsutjämna diagnostik för Azure storage-konton för Windows (BOMULLSTUSS) och hello antalet prenumerationer tooprovide toohello instanser ska baseras på din kapacitet.
>[!NOTE]
Just nu har vi inte specifika rekommendationer för när tooscale ut instanser av Azure loggar integration datorer (t.ex. datorer som kör hello Azure logganalys integration service) eller för storage-konton eller prenumerationer. Skalning beslut ska baseras på dina synpunkter prestanda i dessa olika områden.

Du kan också ha hello alternativet tooscale in hello Azure Log Integration service toohelp förbättra prestanda. hello följande prestandamått kan hjälpa dig i sizing hello-datorer som du väljer toorun hello Azure integration Loggtjänsten:
* På en dator med 8-processor (core) kan en enda instans av Azlog Integrator bearbeta cirka 24 miljoner händelser per dag (~1M/hour).

* En instans av Azlog Integrator kan bearbeta ungefär 1,5 miljoner händelser per dag (~62.5K/hour) på en dator i 4-processor (kärna).

## <a name="install-azure-log-integration"></a>Installera Azure logganalys-integrering
tooinstall Azure Log-integrering, behöver du toodownload hello [Azure logganalys integration](https://www.microsoft.com/download/details.aspx?id=53324) installationsfilen. Kör via hello installationsprogrammet rutinen och Bestäm om du vill tooprovide telemetri information tooMicrosoft.  

![Installationsskärmen med telemetri ikryssad](./media/security-azure-log-integration-get-started/telemetry.png)

*
> [!NOTE]
> Vi rekommenderar att du har Microsoft toocollect telemetridata. Du kan stänga av insamling av telemetridata genom att avmarkera det här alternativet.
>


hello Azure Log-integrering tjänsten samlar in telemetridata från hello-dator som är installerad.  

I insamlade telemetridata är:

* Undantag som uppstår vid körning av Azure logganalys-integrering
* Mått om hello antalet frågor och händelser som har bearbetats
* Statistik om vilka Azlog.exe kommandoradsalternativ som används

hello installationsprocessen ingår i hello video nedan.
> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Install-Azure-Log-Integration/player]



## <a name="post-installation-and-validation-steps"></a>Efter installation och verifiering av steg
När du har slutfört hello grundinställning rutinen du är klar steg tooperform efter installation och verifiering steg:
1. Öppna ett upphöjt PowerShell-fönster och navigera för**c:\Program Files\Microsoft Azure Log-integrering**
2. hello första steget behöver du tootake är tooget hello AzLog Cmdlets importeras. Du kan göra det genom att köra skript hello **LoadAzlogModule.ps1** (meddelande hello ”. \” i hello följande kommando). Typen **.\LoadAzlogModule.ps1** och tryck på **RETUR**.  
Du bör se något som liknar det som visas i hello bilden nedan. </br></br>
![Installationsskärmen med telemetri ikryssad](./media/security-azure-log-integration-get-started/loaded-modules.png) </br></br>
3. Du måste nu tooconfigure AzLog toouse specifika Azure-miljön. Ett ”Azure-miljön” är hello ”typ” för Azure-molndatacenter som du vill toowork med. Det finns flera Azure miljöer just nu, hello för närvarande relevanta alternativen är antingen **AzureCloud** eller **AzureUSGovernment**.   I upphöjd PowerShell-miljön måste du kontrollera att du är i **c:\program files\Microsoft Azure Log-integrering\** </br></br>
    När det, kör hello-kommando: </br>
    ``Set-AzlogAzureEnvironment -Name AzureCloud``(för Azure kommersiella)

      >[!NOTE]
      När hello-kommandot lyckas får inte feedback.  Om du vill toouse hello US Government Azure-molnet, använder du **AzureUSGovernment** (för hello - variabel) för hello amerikanska myndigheter molnet. Andra Azure-moln stöds inte just nu.  
4. Innan du kan övervaka ett system behöver hello namnet på hello storage-konto används för Azure-diagnostik.  Hello Azure-portalen navigera i för**virtuella datorer** och leta efter hello virtuell dator som ska övervakas. I hello **egenskaper** väljer **diagnostikinställningar**.  Klicka på **Agent** och anteckna hello lagringskontonamnet har angetts. Du behöver det här namnet på tjänstkontot för ett senare steg.
![Diagnostikinställningar för Azure](./media/security-azure-log-integration-get-started/storage-account-large.png) </br></br>

      ![Diagnostikinställningar för Azure](./media/security-azure-log-integration-get-started/azure-monitoring-not-enabled-large.png)
      >[!NOTE]
      Om övervakning inte aktiverades under generering av virtuell dator får du hello alternativet tooenable den som ovan.
5. Nu ska vi växla våra uppmärksamhet tillbaka toohello Azure logganalys integration datorn. Vi behöver tooverify att du har anslutning toohello Storage-konto från hello system där du installerade Azure Log-integrering. hello fysisk eller virtuell dator som kör hello Azure Log Integration service behöver komma åt toohello tooretrieve information om lagringskontot loggats av Azure-diagnostik som konfigurerats på varje hello övervakade system.  
  1. Du kan hämta Azure Lagringsutforskaren [här](http://storageexplorer.com/).
  2. Gå igenom hello installationsprogrammet rutinen
  3. När hello installationen är klar klickar du på **nästa** och lämna hello kryssrutan **starta Microsoft Azure-Lagringsutforskaren** markerad.  
  4. Logga in tooAzure.
  5. Kontrollera att du kan se hello storage-konto som du har konfigurerat för Azure-diagnostik.  
![Lagringskonton](./media/security-azure-log-integration-get-started/storage-account.jpg) </br></br>
   6. Observera att det inte finns några alternativ under storage-konton. En av dem är **tabeller**. Under **tabeller** bör du se en kallas **WADWindowsEventLogsTable**. </br></br>
   ![Storage-konton](./media/security-azure-log-integration-get-started/storage-explorer.png) </br>

## <a name="integrate-azure-diagnostic-logging"></a>Integrera Azure diagnostikloggning
I det här steget konfigurerar du hello-dator som kör hello Azure Log Integration service tooconnect toohello storage-konto som innehåller hello loggfiler.
toocomplete det här steget måste vi några saker direkt.  
* **FriendlyNameForSource:** detta är ett eget namn som du kan använda toohello storage-konto som du har konfigurerat hello virtuella toostore information från Azure-diagnostik
* **StorageAccountName:** är hello namnet på hello storage-konto som du angav när du konfigurerade Azure diagnotics.  
* **StorageKey:** här sparas hello lagringsnyckel för hello storage-konto där hello Azure diagnostisk information för den virtuella datorn.  

Utför följande steg tooobtain hello lagringsnyckel hello:
 1. Bläddra toohello [Azure-portalen](http://portal.azure.com).
 2. I hello navigeringsfönstret i hello Azure-konsolen, bläddra toohello längst ned och klicka på **fler tjänster.**

 ![Fler tjänster](./media/security-azure-log-integration-get-started/more-services.png)
 3. Ange **lagring** i hello **Filter** textruta. Klicka på **lagringskonton** (den visas när du har angett **lagring**)

   ![filterfältet](./media/security-azure-log-integration-get-started/filter.png)
 4. En lista över storage-konton visas, dubbelklicka på hello-konto som du tilldelade tooLog lagring.

   ![lista över storage-konton](./media/security-azure-log-integration-get-started/storage-accounts.png)
 5. Klicka på **åtkomstnycklar** i hello **inställningar** avsnitt.

  ![Snabbtangenter](./media/security-azure-log-integration-get-started/storage-account-access-keys.png)
 6. Kopiera **key1** och placera den i en säker plats som du kan komma åt för hello nästa steg.

   ![två åtkomstnycklar](./media/security-azure-log-integration-get-started/storage-account-access-keys.png)
 7. Öppna en upphöjd kommandotolk (Observera att vi använder ett upphöjt kommandotolksfönster här, inte en upphöjd PowerShell-konsol) på hello servern att du har installerat Azure Log-integrering.
 8. Navigera för**c:\Program Files\Microsoft Azure Log-integrering**
 9. Kör ``Azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey> `` </br> Till exempel ``Azlog source add Azlogtest WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==`` om du vill att hello prenumerations-ID tooshow upp i hello händelse XML, bifoga hello prenumerations-ID toohello eget namn: ``Azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>`` eller t.ex.``Azlog source add Azlogtest.YourSubscriptionID WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==``

>[!NOTE]  
Vänta upp too60 minuter och sedan visa hello händelser som hämtas från hello storage-konto. tooview, öppna **Loggboken > Windows-loggar > vidarebefordrade händelser** på hello Azlog Integrator.

Här kan du se en video som skickas via hello stegen som beskrivs ovan.
> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Enable-Diagnostics-and-Storage/player]


## <a name="what-if-data-is-not-showing-up-in-hello-forwarded-events-folder"></a>Vad händer om data inte visas i hello vidarebefordrade händelser mappen?
Om en timme efter att data inte visas i hello **vidarebefordrade händelser** mappen sedan:

1. Hello datorn hello Azure Log-integrering tjänsten som körs och att den har åtkomst till Azure. Försök tooopen hello-tootest anslutning [Azure-portalen](http://portal.azure.com) från hello webbläsare.
2. Kontrollera att användarkontot i hello **Azlog** har skrivbehörighet på hello mappen **users\Azlog**.
  <ol type="a">
   <li>Öppna **Utforskaren** </li>
  <li> Navigera för**c:\users** </li>
  <li> Högerklicka på **c:\users\Azlog** </li>
  <li> Klicka på **säkerhet**  </li>
  <li> Klicka på **NT Service\Azlog** och kontrollera hello hello-kontot behörighet för. Om hello kontot saknas i den här fliken eller om hello behörighet för inte närvarande beviljar visar du åtkomsträttigheter Hej på den här fliken.</li>
  </ol>
3.Kontrollera att hello storage-konto läggas till i hello kommandot **Azlog källa lägga till** visas när du kör kommandot hello **Azlog källistan**.
4. Gå för**Loggboken > Windows-loggar > programmet** toosee om det finns några fel rapporterats från hello Azure logganalys-integration.


Om du stöter på problem under hello installation och konfiguration, öppnar du en [supportbegäran](../azure-supportability/how-to-create-azure-support-request.md)väljer **loggen Integration** som hello-tjänst som du begär support.

Ett annat alternativ är hello [Azure Log Integration MSDN-Forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureLogIntegration). Här kan hello gemenskapen stödja varandra med frågor, svar, tips och råd om hur tooget hello mest utanför Azure Log-integrering. Dessutom övervakar det här forumet hello Azure Log-integrering team och hjälper när vi kan.

## <a name="next-steps"></a>Nästa steg
toolearn mer om Azure Log-integrering finns hello följande dokument:

* [Microsoft Azure Log-integrering för Azure loggar](https://www.microsoft.com/download/details.aspx?id=53324) – Download Center för ytterligare information, systemkrav, och installera instruktioner på Azure logganalys-integration.
* [Introduktion tooAzure loggen integration](security-azure-log-integration-overview.md) – det här dokumentet introducerar tooAzure loggen integration, de viktigaste funktionerna och hur det fungerar.
* [Samarbeta konfigurationssteg](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – det här blogginlägget visar hur tooconfigure Azure logga toowork integrering med partnerlösningar Splunk, HP ArcSight och IBM QRadar. Det är vår aktuella vägledning om hur tooconfigure hello SIEM-komponenter. Kontrollera med leverantören SIEM först för ytterligare information.
* [Azure logganalys Integration vanliga frågor (FAQ)](security-azure-log-integration-faq.md) – det här avsnittet får du svar frågor om Azure logganalys-integration.
* [Integrera Security Center aviseringar med Azure logga Integration](../security-center/security-center-integrating-alerts-with-log-integration.md) – det här dokumentet beskrivs hur toosync Security Center aviseringar, tillsammans med virtuella säkerhetshändelser samlas in av Azure-diagnostik och Azure aktivitetsloggar med din logganalys eller SIEM-lösning.
* [Nya funktioner för Azure-diagnostik och Azure-granskningsloggarna](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – det här blogginlägget introducerar tooAzure granskningsloggar och andra funktioner som hjälper dig få insikter om hello operations resurserna i Azure.
