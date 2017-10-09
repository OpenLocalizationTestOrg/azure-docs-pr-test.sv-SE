---
title: aaaUsing hello Visual Studio Azure guiden Publicera program | Microsoft Docs
description: "Lär dig hur tooconfigure hello olika inställningar i hello Visual Studio Azure guiden Publicera program"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 7d8f1ac9-e439-47e0-a183-0642c4ea1920
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 3f0b580d0054cc246372597660d8ae317d9b8686
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-visual-studio-publish-azure-application-wizard"></a>Med hjälp av hello Visual Studio Azure guiden Publicera program
När du utvecklar ett program i Visual Studio, kan du publicera som programmet tooan Azure-molntjänst genom att använda hello **publicera Azure-programmet** guiden. 

> [!NOTE]
> Den här artikeln handlar om hur du distribuerar toocloud tjänster, inte tooweb platser. Information om hur du distribuerar tooweb platser finns i [hur tooDeploy en Azure-webbplats](https://social.msdn.microsoft.com/Search/windowsazure?query=How%20to%20Deploy%20an%20Azure%20Web%20Site&Refinement=138&ac=4#refinementChanges=117&pageNumber=1&showMore=false).
> 
> 

## <a name="accessing-hello-publish-azure-application-wizard"></a>Åtkomst till guiden för hello publicera Azure-program

Du kan använda guiden för hello publicera Azure-program på två sätt beroende på hello typ av Visual Studio-projekt som du har.

**Om du har en Azure cloud service-projekt:**

1. Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.

1. I **Solution Explorer**, högerklicka på hello-projektet och, hello snabbmenyn väljer **publicera**.

**Om du har ett webbprojekt för program som inte är aktiverad för Azure:**

1. Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.

1. I **Solution Explorer**, högerklicka på hello-projektet och, hello snabbmenyn väljer **konvertera** > **konvertera tooAzure Molntjänstprojekt**. 

1. I **Solution Explorer**, högerklicka hello nyskapad Azure-projekt och, hello snabbmenyn, Välj **publicera**.

## <a name="sign-in-page"></a>Inloggningssidan

![Inloggningssidan](./media/vs-azure-tools-publish-azure-application-wizard/sign-in.png)

**Kontot** – Välj ett konto eller välj **Lägg till ett konto** i listrutan för hello-konto.

**Välj din prenumeration** -Välj hello prenumeration toouse för din distribution.
   
## <a name="settings-page---common-settings-tab"></a>Inställningssidan - gemensamma inställningar   

![Vanliga inställningar](./media/vs-azure-tools-publish-azure-application-wizard/settings-common-settings.png)

**Molntjänsten** -använder hello listrutan, Välj antingen en befintlig molnet tjänsten eller välj  **&lt;Skapa nytt >**, och skapa en tjänst i molnet. hello Datacenter visas inom parentes för varje tjänst i molnet. Du rekommenderas att hello data center plats för hello Molntjänsten vara hello samma som hello data center plats för hello storage-konto (avancerade inställningar).  

**Miljö** -väljer du antingen **produktion** eller **mellanlagring**. Välj hello mellanlagring miljön om du vill toodeploy ditt program i en testmiljö. 

**Skapa configuration** -väljer du antingen **felsöka** eller **versionen**.

**Tjänstkonfigurationen** -väljer du antingen **moln** eller **lokala**.
   
**Aktivera Fjärrskrivbord för alla roller** -kontrollera det här alternativet om du vill toobe kan tooremotely ansluta toohello service. Det här alternativet används främst för felsökning. När du väljer den här kryssrutan hello **konfiguration av fjärrskrivbord** dialogrutan visas. Välj hello **inställningar** länk toochange hello konfiguration.
   
**Aktivera webbdistribution för alla webbprogram roller** -Markera det här alternativet, tooenable web-distribution för hello-tjänsten. Du måste välja hello **aktivera Fjärrskrivbord för alla roller** alternativet toouse den här funktionen. Mer information finns i [ [publicera ett Azure cloud service med Visual Studio](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx). 

## <a name="settings-page---advanced-settings-tab"></a>Inställningssidan - inställningar fliken Avancerat

![Avancerade inställningar](./media/vs-azure-tools-publish-azure-application-wizard/settings-advanced-settings.png)

**Distributionsetiketten** -acceptera standardnamnet hello, eller ange ett namn du väljer. tooappend hello datum toohello distributionsetiketten, lämna hello kryssrutan är markerad. 
   
**Lagringskontot** – Välj hello storage-konto toouse för den här distributionen **&lt;Skapa nytt > toocreate ett lagringskonto. hello Datacenter visas inom parentes för varje storage-konto. Du rekommenderas att hello data center plats för hello lagringskonto vara hello samma som hello data center plats för hello molntjänst (vanliga inställningar).  
   
hello Azure storage-konto lagras hello-paket för distribution av hello. När hello programmet distribueras tas hello paketet bort från hello storage-konto.

**Ta bort distributionen på fel** -väljer det här alternativet toohave hello distribution bort om fel uppstår vid publicering. Ska den vara avmarkerad om du vill toomaintain en konstant virtuella IP-adressen för din tjänst i molnet.

**Uppdatering av programdistribution** – Välj det här alternativet om du vill toodeploy endast uppdaterade komponenter. Den här typen av distribution kan vara snabbare än en fullständig distribution. Detta ska kontrolleras om du vill toomaintain en konstant virtuella IP-adressen för din tjänst i molnet. 

**Uppdatering av programdistribution - inställningar** -den här dialogrutan används toofurther anger du hur hello roller toobe uppdateras. Om du väljer **stegvis uppdatering**, varje instans av ditt program är uppdaterade efter en annan, så att programmet hello alltid är tillgängligt. Om du väljer **samtidig uppdatering**, alla instanser av programmet uppdateras vid hello samtidigt. Samtidig uppdatering är snabbare, men tjänsten kanske inte är tillgänglig under hello uppdateringsprocessen. 

![Inställningar för distribution](./media/vs-azure-tools-publish-azure-application-wizard/deployment-settings.png)

**Aktivera IntelliTrace** -ange om du vill tooenable IntelliTrace. Med IntelliTrace, kan du logga omfattande felsökningsinformation för en rollinstans när den körs i Azure. Om du behöver toofind hello orsaken till ett problem kan använda du hello IntelliTrace-loggar toostep genom från Visual Studio-koden som om den kördes i Azure. Läs mer om hur du använder IntelliTrace [felsökning en publicerade Azure cloud service med Visual Studio och IntelliTrace](./vs-azure-tools-intellitrace-debug-published-cloud-services.md). 

**Aktivera profilering** -ange om du vill tooenable prestanda profilering. hello Visual Studio profiler kan tooget en detaljerad analys av hello beräkningar aspekter av hur Molntjänsten körs. Mer information om hur du använder hello Visual Studio profiler finns [testa hello prestanda för en Azure-molntjänst](./vs-azure-tools-performance-profiling-cloud-services.md).

**Aktivera Remote felsökare för alla roller** -ange om du vill tooenable fjärrfelsökning. Mer information om hur du felsöker molntjänster med hjälp av Visual Studio finns [felsökning en Azure-molntjänst eller en virtuell dator i Visual Studio](./vs-azure-tools-debug-cloud-services-virtual-machines.md).

## <a name="diagnostics-settings-page"></a>Sidan Inställningar för diagnostik

![Diagnostikinställningar](./media/vs-azure-tools-publish-azure-application-wizard/diagnostic-settings.png)

Diagnostiken kan du tootroubleshoot en Azure-molntjänst (eller virtuella Azure-datorn). Information om diagnostik finns [konfigurera diagnostik för virtuella datorer och Azure Cloud Services](./vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md). Information om Application Insights finns [vad är Application Insights?](./application-insights/app-insights-overview.md).

## <a name="summary-page"></a>Sammanfattningssida

![Sammanfattning](./media/vs-azure-tools-publish-azure-application-wizard/summary.png)

**Rikta profil** -du kan välja toocreate en publiceringsprofil hello-inställningar som du har valt. Du kan till exempel skapa en profil för en testmiljö och en annan för produktion. toosave detta profilen väljer hello **spara** ikon. hello guiden skapar hello profil och sparar den i hello Visual Studio-projekt. toomodify hello profilnamn öppna hello **mål profil** listan och väljer sedan **< hantera... >**.
   
   > [!NOTE]
   > Hej publiceringsprofilen visas i Solution Explorer i Visual Studio och hello profilinställningar skrivs tooa fil med filnamnstillägget .azurePubxml. Inställningarna sparas som attribut till XML-taggar.
   > 
   > 

## <a name="publishing-your-application"></a>Publicering av programmet

När du konfigurerar alla hello-inställningarna för ditt projekt distributionen, välja **publicera** längst hello hello dialogrutan. Du kan övervaka hello processtatus i hello **utdata** fönstret i Visual Studio.

## <a name="next-steps"></a>Nästa steg
- [Migrera och publicera en webbprogrammet tooan Azure-molntjänst från Visual Studio](./vs-azure-tools-migrate-publish-web-app-to-cloud-service.md)
- [Lär dig hur toouse Visual Studio toopublish en Azure-molntjänst](./vs-azure-tools-publishing-a-cloud-service.md)
- [Felsökning av en publicerad Azure cloud service med Visual Studio och IntelliTrace](./vs-azure-tools-intellitrace-debug-published-cloud-services.md)
- [Testa hello prestanda för en Azure-molntjänst](./vs-azure-tools-performance-profiling-cloud-services.md)
- [Konfigurera diagnostik för Azure-molntjänster och virtuella datorer](./vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md). 
- [Vad är Application Insights?](./application-insights/app-insights-overview.md)