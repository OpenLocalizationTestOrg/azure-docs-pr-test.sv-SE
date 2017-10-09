---
title: "aaaConfiguring diagnostik för virtuella datorer och Azure Cloud Services | Microsoft Docs"
description: "Beskriver hur tooconfigure diagnostikinformation för felsökning Azure cloude tjänster och virtuella maskiner (VMs) i Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: e70cd7b4-6298-43aa-adea-6fd618414c26
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 74cdf49413d6d89a84195070dd9d817da2463373
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-diagnostics-for-azure-cloud-services-and-virtual-machines"></a>Konfigurera diagnostik för Azure-molntjänster och virtuella datorer
När du behöver tootroubleshoot en Azure-molntjänst eller en virtuell Azure-dator kan konfigurera du Azure-diagnostik enklare med hjälp av Visual Studio. Azure diagnostics avbildas systemdata och loggningsdata på hello virtuella datorer och instanser för virtuella datorer som kör Molntjänsten och överför data till ett lagringskonto som du väljer. Se [aktivera diagnostikloggning för web apps i Azure App Service](app-service-web/web-sites-enable-diagnostic-log.md) mer information om loggning i Azure diagnostics.

Det här avsnittet beskrivs hur du tooenable och konfigurera Azure-diagnostik i Visual Studio, både före och efter distributionen, såväl som virtuella Azure-datorer. Där visas också hur tooselect hello typer av diagnostik information toocollect och hur tooview hello information när det samlas in.

Du kan konfigurera Azure-diagnostik i hello följande sätt:

* Du kan ändra konfigurationsinställningarna för diagnostik via hello **diagnostik Configuration** dialogrutan i Visual Studio. hello inställningarna sparas i en fil med namnet diagnostics.wadcfgx (diagnostics.wadcfg i Azure SDK 2.4 eller tidigare). Alternativt kan du direkt ändra hello konfigurationsfilen. Om du manuellt uppdatera hello filen börjar hello configuration ändringarna gälla hello nästa gång du distribuerar hello cloud service tooAzure eller kör hello tjänst i hello-emulatorn.
* Använd **Cloud Explorer** eller **Server Explorer** i Visual Studio toochange hello diagnostikinställningarna för en tjänst körs i molnet eller virtuell dator.

## <a name="azure-26-diagnostics-changes"></a>Azure 2.6 diagnostik ändringar
Hello följande ändringar har gjorts för Azure SDK 2.6-projekt i Visual Studio. (Ändringarna gäller även toolater versioner av Azure SDK.)

* hello lokala emulator stöder nu diagnostik. Det innebär att du kan samla in diagnostikdata och se till att programmet skapar hello rätt spår när du utvecklar och testar i Visual Studio. Hej anslutningssträngen `UseDevelopmentStorage=true` aktiverar diagnostik datainsamling när du kör cloud service-projekt i Visual Studio med hjälp av hello Azure storage-emulatorn. Alla diagnostikdata samlas in i hello (utveckling lagring) storage-konto.
* hello diagnostik konto lagringsanslutningssträng (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) lagras igen i hello tjänstekonfigurationsfilen (.cscfg). I Azure SDK 2.5 angavs hello diagnostiklagringskonto i hello diagnostics.wadcfgx-filen.

Det finns några viktiga skillnader mellan hur hello anslutningssträngen fungerade i Azure SDK 2.4 och tidigare och hur det fungerar i Azure SDK 2.6 eller senare.

* I Azure SDK 2.4 och tidigare användes hello anslutningssträngen som en körning av hello diagnostik plugin-programmet tooget hello information om lagringskonto för överföring av diagnostik loggar.
* Azure SDK 2.6 och senare, används hello diagnostik anslutningssträngen av Visual Studio tooconfigure hello diagnostik tillägg med information om hello lämpliga lagringskonto vid publicering. hello anslutningssträngen kan du definiera olika lagringskonton för olika konfigurationer som Visual Studio ska använda när du publicerar. Eftersom hello diagnostik plugin-programmet inte längre är tillgängliga (efter Azure SDK 2.5) kan inte hello .cscfg-filen ensamt aktivera hello diagnostik tillägg. Du har tooenable hello tillägget separat via verktyg som Visual Studio eller PowerShell.
* toosimplify hello konfigureringen av hello diagnostik tillägget med PowerShell, hello paketet utdata från Visual Studio innehåller också hello offentliga konfigurations-XML för hello diagnostik filnamnstillägget för varje roll. Visual Studio använder hello diagnostik toopopulate hello storage-kontoinformation om anslutningssträngar finns i hello offentliga konfiguration. hello offentliga config-filer skapas i hello tillägg mapp och följer hello mönster PaaSDiagnostics. &lt;RoleName >. PubConfig.xml. PowerShell-baserade distributioner kan använda det här mönstret toomap varje configuration tooa roll.
* hello anslutningssträngen i hello .cscfg-filen används också av hello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040) tooaccess hello diagnostikdata så visas det i hello **övervakning** fliken hello anslutningssträng krävs tooconfigure hello service tooshow utförlig övervakningsdata i hello-portalen.

## <a name="migrating-projects-tooazure-sdk-26-and-later"></a>Migrera projekt tooAzure SDK 2.6 och senare
När du migrerar från Azure SDK 2,5 tooAzure SDK 2.6 eller senare, om du har en diagnostiklagringskonto som anges i hello .wadcfgx filen sedan förblir den det. tootake nytta av hello flexibilitet för att använda olika lagringsplatser konton för olika lagringskonfigurationer, har du toomanually lägga till hello anslutning sträng tooyour projekt. Om du migrerar ett projekt från Azure SDK 2.4 eller tidigare tooAzure SDK 2.6 hello du diagnostik anslutningssträngar bevaras. Observera dock hello ändringar i hur anslutningssträngar behandlas i Azure SDK 2.6 som anges i hello föregående avsnitt.

### <a name="how-visual-studio-determines-hello-diagnostics-storage-account"></a>Hur hello diagnostiklagringskonto avgör i Visual Studio
* Om en anslutningssträng för diagnostik anges i .cscfg-filen för hello använder Visual Studio den tooconfigure hello diagnostik tillägget när du publicerar och vid generering av xml-filer för hello offentliga konfiguration under paketering.
* Om inga diagnostik anslutningssträngen har angetts i hello .cscfg-filen, sedan faller Visual Studio tillbaka toousing hello storage-konto som anges i hello .wadcfgx tooconfigure hello diagnostik filnamnstillägg när filen publicering och generera hello offentliga XML-konfigurationsfiler när paketera.
* hello diagnostik anslutningssträngen i hello .cscfg-filen har företräde framför hello storage-konto i hello .wadcfgx-filen. Om en anslutningssträng för diagnostik är anges i hello .cscfg-filen har Visual Studio använder som och ignorerar hello storage-konto i .wadcfgx.

### <a name="what-does-hello-update-development-storage-connection-strings-checkbox-do"></a>Vad hello ”uppdatera development storage-anslutningssträngar...” kryssrutan för?
Hej kryssrutan för **uppdatera development storage-anslutningssträngar för diagnostik- och cachelagring med Microsoft Azure storage-kontoautentiseringsuppgifter när du publicerar tooMicrosoft Azure** ger dig ett enkelt sätt tooupdate alla utveckling lagringskontots anslutningssträngar med hello Azure storage-konto anges vid publicering.

Anta att du väljer den här kryssrutan och hello diagnostik anslutningssträngen anger `UseDevelopmentStorage=true`. När du publicerar hello projektet tooAzure uppdateras automatiskt hello diagnostik anslutningssträngen med hello storage-konto som du angav i guiden för hello publicera i Visual Studio. Men om en verklig lagringskonto har angetts som hello diagnostik anslutningssträng, används detta konto i stället.

## <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Diagnostik funktioner skillnaderna mellan Azure SDK 2.4 och tidigare och Azure SDK 2,5 och senare
Om du uppgraderar ditt projekt från Azure SDK 2.4 tooAzure SDK 2.5 eller senare, du bör ha i åtanke hello följande diagnostik funktioner skillnader.

* **Konfiguration av API: er är inaktuella** – programkonfiguration av diagnostik är tillgänglig i Azure SDK 2.4 eller tidigare versioner, men är föråldrad i Azure SDK 2.5 och senare. Om konfigurationen av diagnostik har definierats i koden, behöver du tooreconfigure dessa inställningar från grunden i hello migrerade projekt för diagnostik tookeep fungerar. konfigurationsfilen för hello diagnostik för Azure SDK 2.4 är diagnostics.wadcfg och diagnostics.wadcfgx för Azure SDK 2.5 och senare.
* **Diagnostik för molnprogram för tjänsten kan bara konfigureras på hello rollnivå, inte på instansnivå hello.**
* **Varje gång du distribuerar appen hello diagnostik konfiguration uppdateras** – om du ändrar konfigurationen diagnostik från Server Explorer och distribuera din app kan det medföra problem för paritet.
* **Azure SDK 2.5 och senare, krascher Dumpar konfigureras i hello konfigurationsfil diagnostik, inte i kod** – om du har krascher Dumpar som konfigurerats i koden har du toomanually överföring hello konfiguration från kod toohello konfigurationsfilen eftersom hello kraschdumpar överförs inte under hello migrering tooAzure SDK 2.6.

## <a name="enable-diagnostics-in-cloud-service-projects-before-deploying-them"></a>Aktivera diagnostik i molntjänstprojekt innan du distribuerar dem.
Du kan välja toocollect diagnostikdata för roller som körs i Azure, när du kör hello service i hello-emulator innan du distribuerar den i Visual Studio. Alla ändringar toodiagnostics inställningar i Visual Studio sparas i hello diagnostics.wadcfgx konfigurationsfil. Dessa konfigurationsinställningar ange hello lagringskonto där diagnostikdata sparas när du distribuerar din tjänst i molnet.

[!INCLUDE [cloud-services-wad-warning](../includes/cloud-services-wad-warning.md)]

### <a name="tooenable-diagnostics-in-visual-studio-before-deployment"></a>tooenable diagnostik i Visual Studio före distributionen
1. Hello snabbmenyn för hello roll som intresserar dig välja **egenskaper**, och välj sedan hello **Configuration** fliken hello roll **egenskaper** fönster.
2. I hello **diagnostik** Kontrollera att hello **aktivera diagnostik** är markerad.
   
    ![Åtkomst till hello aktivera diagnostik alternativet](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796660.png)
3. Välj hello ellips (...)-knappen toospecify hello storage-konto där du vill att hello diagnostik data toobe lagras. hello storage-konto som du väljer kommer att hello plats där diagnostikdata lagras.
   
    ![Ange hello storage-konto toouse](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796661.png)
4. I hello **skapa Lagringsanslutningssträng** dialogrutan Ange om du vill tooconnect med hello Azure Storage-emulatorn, en Azure-prenumeration eller autentiseringsuppgifterna anges manuellt.
   
    ![Dialogrutan för Storage-konto](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796662.png)
   
   * Om du väljer alternativet för hello Microsoft Azure Storage-emulatorn hello anslutningssträngen anges tooUseDevelopmentStorage = true.
   * Om du väljer hello alternativ för en prenumeration kan välja du hello Azure-prenumeration du vill toouse och hello kontonamn. Du kan välja hello hantera konton knappen toomanage dina Azure-prenumerationer.
   * Om du väljer alternativet för hello anges manuellt autentiseringsuppgifter, är du tillfrågas tooenter hello namn och nyckel för hello Azure-konto du vill toouse.
5. Välj hello **konfigurera** knappen tooview hello **diagnostik configuration** dialogrutan. Varje flik (förutom för **allmänna** och **loggen kataloger**) representerar en diagnostiska datakälla som du kan samla in. hello standardflik **allmänna**, ger du hello följande diagnostikalternativ data samling: **endast fel**, **All information**, och **anpassade plan** . Hej standardalternativet **endast fel**, tar hello minsta mängd lagringsutrymme eftersom den inte överföra varningar eller spårning av meddelanden. hello alla information alternativet överföringar hello information och är därför hello dyraste alternativet vad gäller lagring.
   
    ![Aktivera Azure-diagnostik och konfiguration](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)
6. Välj det här exemplet hello **anpassade plan** alternativet så att du kan anpassa hello data som samlas in.
7. Hej **diskkvot i MB** ruta anger hur mycket utrymme som du vill använda tooallocate i lagringen redovisa diagnostikdata. Du kan ändra hello standardvärdet om du vill.
8. På varje flik i diagnostikdata som du vill toocollect, Välj dess **Aktivera överföring av <log type>**  kryssrutan. Om du vill toocollect programloggarna, väljer du exempelvis hello **Aktivera överföring av programloggar** kryssrutan på hello **programloggarna** fliken. Dessutom ange annan information som krävs för varje datatyp för diagnostik. Avsnittet hello **konfigurera diagnostik-datakällor** senare i det här avsnittet för konfigurationsinformation på varje flik.
9. När du har aktiverat insamling av all hello diagnostikdata som du vill välja hello **OK** knappen.
10. Kör ditt Azure cloud service-projekt i Visual Studio som vanligt. När du använder programmet sparas hello logginformation som du har aktiverat toohello Azure storage-konto som du har angett.

## <a name="enable-diagnostics-in-azure-virtual-machines"></a>Aktivera diagnostik i virtuella Azure-datorer
Du kan välja toocollect diagnostikdata för virtuella Azure-datorer i Visual Studio.

### <a name="tooenable-diagnostics-in-azure-virtual-machines"></a>tooenable diagnostik i virtuella Azure-datorer
1. I **Server Explorer**, Välj hello Azure noden och sedan ansluta tooyour Azure-prenumeration om du inte redan är ansluten.
2. Expandera hello **virtuella datorer** nod. Du kan skapa en ny virtuell dator eller välj en befintliga.
3. Välj på snabbmenyn för hello virtuell dator som du är intresserad av hello **konfigurera**. Detta visar dialogrutan konfiguration av hello virtuell dator.
   
    ![Konfigurera en virtuell Azure-dator](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796663.png)
4. Lägg till hello-tillägget för Microsoft Monitoring Agent-diagnostik om den inte redan är installerat. Det här tillägget kan du samla in diagnostikdata för hello virtuella Azure-datorn. Välj hello Välj en tillgänglig tillägget nedrullningsbara menyn och välj sedan Microsoft Monitoring Agent-diagnostik i hello installerat tillägg lista.
   
    ![Installera ett tillägg för virtuell Azure-dator](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766024.png)
   
   > [!NOTE]
   > Andra diagnostik tillägg är tillgängligt för virtuella datorer. Mer information finns i Azure VM-tillägg och funktioner.
   > 
   > 
5. Välj hello **Lägg till** knappen tooadd hello-tillägget och visa dess **diagnostik configuration** dialogrutan.
6. Välj hello **konfigurera** knappen toospecify ett lagringskonto och välj sedan hello **OK** knappen.
   
    Varje flik (förutom för **allmänna** och **loggen kataloger**) representerar en diagnostiska datakälla som du kan samla in.
   
    ![Aktivera Azure-diagnostik och konfiguration](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)
   
    hello standardflik **allmänna**, ger du hello följande diagnostikalternativ data samling: **endast fel**, **All information**, och **anpassade plan** . Hej standardalternativet **endast fel**, tar hello minsta mängd lagringsutrymme eftersom den inte överföra varningar eller spårning av meddelanden. Hej **All information** alternativet överföringar hello information och är därför hello dyraste alternativet vad gäller lagring.
7. Välj det här exemplet hello **anpassade plan** alternativet så att du kan anpassa hello data som samlas in.
8. Hej **diskkvot i MB** ruta anger hur mycket utrymme som du vill använda tooallocate i lagringen redovisa diagnostikdata. Du kan ändra hello standardvärdet om du vill.
9. På varje flik i diagnostikdata som du vill toocollect, Välj dess **Aktivera överföring av <log type>**  kryssrutan.
   
    Om du vill toocollect programloggarna, väljer du exempelvis hello **Aktivera överföring av programloggar** kryssrutan på hello **programloggarna** fliken. Dessutom ange annan information som krävs för varje datatyp för diagnostik. Avsnittet hello **konfigurera diagnostik-datakällor** senare i det här avsnittet för konfigurationsinformation på varje flik.
10. När du har aktiverat insamling av all hello diagnostikdata som du vill välja hello **OK** knappen.
11. Spara hello uppdateras projekt.
    
     Du ser ett meddelande i hello **Microsoft Azure-aktivitetsloggen** fönster som hello virtuella datorn har uppdaterats.

## <a name="configure-diagnostics-data-sources"></a>Konfigurera diagnostik-datakällor
När du aktiverar datainsamling för diagnostik, kan du välja vilka datakällor som du vill att toocollect och vilken information som samlas in. hello följer en lista över flikar i hello **diagnostik configuration** dialogrutan och innebär att varje konfigurationsalternativet.

### <a name="application-logs"></a>Programloggar
**Programloggarna** innehåller diagnostikinformation som produceras av ett webbprogram. Om du vill toocapture programloggarna, Välj hello **Aktivera överföring av programloggar** kryssrutan. Du kan öka eller minska hello antalet minuter när hello programloggarna överförs tooyour storage-konto genom att ändra hello **överför Period (min)** värde. Du kan också ändra hello mängden information som hämtas hello loggen genom att ange hello loggen värde. Du kan till exempel välja **utförlig** tooget mer information eller välj **kritisk** toocapture endast kritiska fel. Om du har en särskild diagnostik-provider som genererar programloggarna kan du fånga in dem genom att lägga till hello providern GUID toohello **Provider-GUID** rutan.

  ![Programloggar](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758145.png)

  Se [aktivera diagnostikloggning för web apps i Azure App Service](app-service-web/web-sites-enable-diagnostic-log.md) mer information om programloggar.

### <a name="windows-event-logs"></a>Windows-händelseloggar
Om du vill toocapture Windows-händelseloggar, Välj hello **Aktivera överföring av Windows-händelseloggar** kryssrutan. Du kan öka eller minska hello antalet minuter när hello händelseloggar överförs tooyour storage-konto genom att ändra hello **överför Period (min)** värde. Välj hello kryssrutorna för hello typer av händelser som du vill tootrack.

  ![Händelseloggar](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796664.png)

Om du använder Azure SDK 2.6 eller senare och vill toospecify en anpassad datakälla, anger du i hello  **<Data source name>**  texten och väljer hello **Lägg till** knappen Nästa tooit. hello datakällan har lagts till toohello diagnostics.cfcfg fil.

Om du använder Azure SDK 2.5 och vill toospecify en anpassad datakälla, du kan lägga till den toohello `WindowsEventLog` avsnitt i hello diagnostics.wadcfgx-fil, exempelvis hello följande exempel.

```
<WindowsEventLog scheduledTransferPeriod="PT1M">
   <DataSource name="Application!*" />
   <DataSource name="CustomDataSource!*" />
</WindowsEventLog>
```
### <a name="performance-counters"></a>Prestandaräknare
Prestandaräknaren som kan hjälpa dig hitta system flaskhalsar och finjustera system- och programprestanda. Se [skapa och Använd prestandaräknare i ett Azure-program](https://msdn.microsoft.com/library/azure/hh411542.aspx) för mer information. Om du vill toocapture prestandaräknare, Välj hello **Aktivera överföring av prestandaräknare** kryssrutan. Du kan öka eller minska hello antalet minuter när hello händelseloggar överförs tooyour storage-konto genom att ändra hello **överför Period (min)** värde. Markera kryssrutorna för hello för hello prestandaräknare som du vill tootrack.

  ![Prestandaräknare](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758147.png)

tootrack en prestandaräknare som inte finns i listan, ange den med hjälp av hello föreslagna syntax och välj sedan hello **Lägg till** knappen. hello operativsystemet på den virtuella datorn hello bestämmer vilka prestandaräknare som du kan följa. Mer information om syntaxen finns [att ange en räknarsökvägen](https://msdn.microsoft.com/library/windows/desktop/aa373193.aspx).

### <a name="infrastructure-logs"></a>Infrastruktur-loggar
Om du vill toocapture infrastruktur loggar som innehåller information om hello Azure-diagnostik infrastrukturen, hello RemoteAccess modulen och hello RemoteForwarder-modulen väljer hello **Aktivera överföring av infrastrukturen loggar**kryssrutan. Du kan öka eller minska hello antalet minuter när hello loggar överförs tooyour storage-konto genom att ändra värdet för hello överföra Period (min).

  ![Diagnostik infrastruktur loggar](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758148.png)

  Se [samla in Data för loggning med hjälp av Azure-diagnostik](https://msdn.microsoft.com/library/azure/gg433048.aspx) för mer information.

### <a name="log-directories"></a>Loggen kataloger
Om du vill toocapture loggen kataloger som innehåller data som samlas in från loggen kataloger för Internet Information Services (IIS)-begäranden, misslyckade begäranden eller mappar som du väljer, väljer hello **Aktivera överföring av loggen kataloger**kryssrutan. Du kan öka eller minska hello antalet minuter när hello loggar överförs tooyour storage-konto genom att ändra hello **överför Period (min)** värde.

Du kan välja hello rutor hello loggar du vill toocollect, såsom **IIS-loggar** och **kunde inte begära** loggar. Standard lagring behållarnamn finns, men kan du ändra hello namn om du vill.

Dessutom kan du fånga in loggar från en mapp. Ange bara hello sökväg i hello **loggen från absoluta Directory** avsnittet och väljer sedan hello **Lägg till katalog** knappen. hello loggar kommer att sparas toohello angetts behållare.

  ![Loggen kataloger](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796665.png)

### <a name="etw-logs"></a>ETW-loggar
Om du använder [Event Tracing for Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803\(v=vs.85\).aspx) (ETW) och vill toocapture ETW loggar, Välj hello **Aktivera överföring av ETW-loggar** kryssrutan. Du kan öka eller minska hello antalet minuter när hello loggar överförs tooyour storage-konto genom att ändra hello **överför Period (min)** värde.

hello händelser har hämtats från händelsekällor och händelsemanifest som du anger. toospecify en händelsekälla, ange ett namn i hello **händelsekällor** avsnittet och väljer sedan hello **lägga till händelsekällan** knappen. På liknande sätt kan du ange ett manifest för händelsen i hello **händelse visar** avsnittet och väljer sedan hello **Lägg till händelse Manifest** knappen.

  ![ETW-loggar](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766025.png)

  hello ETW framework stöds i ASP.NET via klasser i hello [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110) namnområde. Hej Microsoft.WindowsAzure.Diagnostics namnområde som ärver från och utökar standard [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110) klasser, aktiverar hello användning av () [System.Diagnostics.aspx] https://msdn.microsoft.com/library/system.Diagnostics (v=vs.110) som ett loggningsramverk i hello Azure-miljön. Mer information finns i [ta kontroll av loggning och spårning i Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) och [aktiverar diagnostik i Azure-molntjänster och virtuella datorer](cloud-services/cloud-services-dotnet-diagnostics.md).

### <a name="crash-dumps"></a>Krasch minnesdumpar
Om du vill toocapture information om när en rollinstans kraschar, Välj hello **Aktivera överföring av krascher Dumpar** kryssrutan. (Eftersom ASP.NET hanterar de flesta undantag, detta är vanligtvis bara användbar för arbetsroller.) Du kan öka eller minska hello procentandelen lagring utrymme dedikerade toohello krashdumpar genom att ändra hello **Directory kvoten (%)** värde. Du kan ändra hello lagringsbehållaren där hello krascher minnesdumpar lagras och du kan välja om du vill toocapture en **fullständig** eller **Mini** dumpen.

hello processer som spåras visas. Markera kryssrutorna för hello för hello processer som du vill toocapture. tooadd en annan process toohello lista, ange hello processnamn och välj hello **lägga till processen** knappen.

  ![Krasch minnesdumpar](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766026.png)

  Se [ta kontroll av loggning och spårning i Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) och [Microsoft Azure Diagnostics del 4: Anpassad loggning komponenter och Azure Diagnostics 1.3 ändringar](http://justazure.com/microsoft-azure-diagnostics-part-4-custom-logging-components-azure-diagnostics-1-3-changes/) för mer information.

## <a name="view-hello-diagnostics-data"></a>Visa hello diagnostikdata
När du har lagrat hello diagnostikdata för en molnbaserad tjänst eller en virtuell dator, kan du visa den.

### <a name="tooview-cloud-service-diagnostics-data"></a>diagnostikdata för tooview cloud service
1. Distribuera din molntjänst som vanligt och sedan köra den.
2. Du kan visa hello diagnostikdata i en rapport som Visual Studio genererar eller tabeller i ditt lagringskonto. Öppna tooview hello data i en rapport **Cloud Explorer** eller **Server Explorer**, öppna hello snabbmenyn för hello nod för hello roll som du är intresserad av och välj sedan **visa diagnostikdata** .
   
    ![Visa diagnostik-Data](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748912.png)
   
    En rapport som visar hello tillgängliga data visas.
   
    ![Microsoft Azure Diagnostics rapporten i Visual Studio](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796666.png)
   
    Om hello senaste data inte visas kan kanske du toowait för hello överföring period tooelapse.
   
    Välj hello **uppdatera** länka tooimmediately uppdatering hello data eller välj ett intervall i hello **uppdatera** nedrullningsbara listan rutan toohave hello data uppdateras automatiskt. tooexport hello Feldata, Välj hello **exportera tooCSV** knappen toocreate en CSV-fil som kan öppnas i ett kalkylblad.
   
    I **Cloud Explorer** eller **Server Explorer**, öppna hello storage-konto som är kopplad till hello-distribution.
3. Öppna hello diagnostik tabeller i hello tabell viewer och granska hello data som du samlade in. Du kan öppna en blob-behållare för IIS-loggar och anpassade loggar. Du kan hitta hello tabell eller blob-behållare som innehåller hello data som intresserar dig genom att granska hello i den följande tabellen. Dessutom toohello data för som loggfilen, hello tabellposter innehålla EventTickCount, DeploymentId, roll och RoleInstance toohelp du identifiera vilka virtuella datorer och rollen som har genererats hello och när. 
   
   | Diagnostikdata | Beskrivning | Plats |
   | --- | --- | --- |
   | Programloggar |Loggar som genereras av din kod genom att anropa metoder i hello System.Diagnostics.Trace klass. |WADLogsTable |
   | Händelseloggar |Dessa data är från hello Windows-händelseloggar på hello virtuella datorer. Windows lagrar information i dessa loggar, men program och tjänster som också använder dem tooreport fel eller logga information. |WADWindowsEventLogsTable |
   | Prestandaräknare |Du kan samla in data på alla prestandaräknare som är tillgänglig på hello virtuella datorn. hello operativsystemet tillhandahåller prestandaräknare som innehåller många statistik, till exempel minne användnings- och tid. |WADPerformanceCountersTable |
   | Infrastruktur-loggar |Dessa loggar genereras från hello diagnostik infrastruktur sig själv. |WADDiagnosticInfrastructureLogsTable |
   | IIS-loggar |De här loggarna registrera webbegäranden. Om din molntjänst hämtar en stor mängd trafik, kan dessa loggar vara ganska långa, så du bör samla in och lagra dessa data endast när du behöver den. |Du kan hitta misslyckades begäran loggas i hello blob-behållare under bomullstuss iis failedreqlogs under en sökväg för den distribution, roll och instans. Du kan hitta klar loggar under bomullstuss-iis-loggfiler. För varje fil skapas transaktioner i hello WADDirectories tabell. |
   | Krasch minnesdumpar |Den här informationen innehåller binära avbildningar av din molntjänst processen (vanligtvis en arbetsroll). |bomullstuss-crush-Dumpar blob-behållare |
   | Anpassade loggfiler |Loggar av data som du fördefinierade. |Du kan ange i koden hello platsen för anpassade loggfiler i ditt lagringskonto. Du kan till exempel ange en anpassad blob-behållare. |
4. Om data av valfri typ trunkeras kan du öka hello buffert för datatypen eller förkorta hello intervallet mellan dataöverföringar från hello virtuella tooyour storage-konto.
5. (valfritt) Rensa data från hello lagring kontot ibland tooreduce totala lagringskostnader.
6. När du gör en fullständig distribution hello diagnostics.cscfg filen (.wadcfgx för Azure SDK 2.5) uppdateras i Azure och din molntjänst hämtar ändringar tooyour diagnostik konfiguration. Om du, i stället uppdaterar en befintlig distribution, uppdateras inte hello .cscfg-filen i Azure. Du kan fortfarande ändra diagnostikinställningarna, men genom att följa hello stegen i nästa avsnitt om hello. Mer information om hur du utför en fullständig distribution uppdaterar en befintlig distribution finns [Publiceringsguiden för Azure-program](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="tooview-virtual-machine-diagnostics-data"></a>tooview diagnostikdata för virtuell dator
1. Välj på snabbmenyn för virtuell dator är hello hello **visa diagnostikdata**.
   
    ![Visa diagnostik-data i Azure-dator](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766027.png)
   
    Då öppnas hello **diagnostik sammanfattning** fönster.
   
    ![Virtuell dator i Azure diagnostics sammanfattning](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796667.png)  
   
    Om hello senaste data inte visas kan kanske du toowait för hello överföring period tooelapse.
   
    Välj hello **uppdatera** länka tooimmediately uppdatering hello data eller välj ett intervall i hello **uppdatera** nedrullningsbara listan rutan toohave hello data uppdateras automatiskt. tooexport hello Feldata, Välj hello **exportera tooCSV** knappen toocreate en CSV-fil som kan öppnas i ett kalkylblad.

## <a name="configure-cloud-service-diagnostics-after-deployment"></a>Konfigurera cloud service diagnostik efter distribution
Om du vill veta problem med en molnbaserad tjänst som redan körs, kanske du vill toocollect data som du inte har angett innan du ursprungligen distribuerade hello-rollen. Du kan då starta toocollect dessa data med hjälp av hello inställningar i Server Explorer. Du kan konfigurera diagnostik för en enda instans eller alla hello-instanser i en roll, beroende på om du öppnar dialogrutan diagnostik konfiguration av hello hello snabbmenyn hello-instans eller en hello roll. Om du konfigurerar hello rollen nod, tillämpas ändringar tooall instanser. Om du konfigurerar hello instans nod, tillämpas ändringar toothat-instans.

### <a name="tooconfigure-diagnostics-for-a-running-cloud-service"></a>tooconfigure diagnostik för en molnbaserad tjänst som körs
1. I Server Explorer expanderar du hello **molntjänster** nod, och sedan Expandera noderna toolocate hello roll eller instans som du vill tooinvestigate eller båda.
   
    ![Konfigurera diagnostik](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748913.png)
2. Välj på hello snabbmenyn för en instans noden eller en roll **uppdatera diagnostikinställningar**, och välj sedan hello diagnostikinställningarna som du vill toocollect.
   
    Information om konfigurationsinställningar för hello finns **konfigurera diagnostik-datakällor** i det här avsnittet. Information om hur tooview hello diagnostikdata finns **visa hello diagnostikdata** i det här avsnittet.
   
    Om du ändrar datainsamling i **Server Explorer**, ändringarna gäller tills du distribuerar fullständigt Molntjänsten. Om du använder hello standard Publiceringsinställningar hello ändringar är inte över, eftersom hello standard publicera inställningen är tooupdate hello befintlig distribution, i stället för att göra en fullständig omdistributionen. toomake att hello inställningar Rensa vid tidpunkten för distribution går toohello **avancerade inställningar** fliken i hello publicera guiden och avmarkera hello **uppdatering av programdistribution** kryssrutan. När du distribuerar med kryssrutan avmarkerad återställas hello inställningar toothose i hello .wadcfgx (eller .wadcfg) som angetts via hello Egenskapsredigeraren för hello roll. Om du uppdaterar din distribution, behåller Azure hello gamla inställningar.

## <a name="troubleshoot-azure-cloud-service-issues"></a>Felsökning av problem med Azure cloud service
Om du får problem med ditt molntjänstprojekt, till exempel en roll som hämtar fastnat i statusen ”upptagen” återanvänds, eller flera utlöser ett internt serverfel finns det verktyg och tekniker som du kan använda toodiagnose och åtgärda problemen. Exempel på vanliga problem och lösningar specifika, samt en översikt över hello begrepp och verktyg användas toodiagnose och åtgärda dessa fel finns [Compute diagnostikdata i Azure PaaS](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

## <a name="q--a"></a>Frågor och svar
**Vad är hello buffertstorlek och hur stor ska den vara?**

På varje virtuell dator-instans begränsa kvoter hur mycket diagnostiska data kan lagras på hello lokala filsystem. Dessutom kan ange du en buffertstorlek för varje typ av diagnostiska data, som är tillgänglig. Den här buffertstorlek fungerar som en individuell kvot för den typ av data. Du kan fastställa genom att kontrollera hello längst ned i dialogrutan för hello hello övergripande kvoten och hello mängden minne som finns kvar. Om du anger större buffertar eller fler typer av data, hanterar du hello övergripande kvoten. Du kan ändra hello övergripande kvot genom att ändra hello diagnostics.wadcfg/.wadcfgx konfigurationsfilen. hello diagnostik data lagras på hello samma filsystem som programdata, så om programmet använder mycket diskutrymme, bör du öka hello övergripande diagnostik kvoten.

**Vad är hello överföring period och hur lång tid ska den vara?**

hello överföringsperioden är hello mängden tid som förflutit mellan data samlar in. Efter varje överföring period flyttas data från hello lokala filsystem på en virtuell dator tootables i ditt lagringskonto. Om hello mängden data som samlas in överskrider kvoten hello innan hello slutet av överföringsperioden, ignoreras äldre data. Om du är att förlora data eftersom dina data överskrider buffertstorleken hello eller hello övergripande kvoten kanske du vill toodecrease hello överföring period.

**Vilken tidszon är hello tidsstämplar i?**

hello tidsstämplar finns i hello lokala tidszon hello datacenter som är värd för Molntjänsten. hello används följande tre timestamp-kolumner i tabellerna hello.

* **PreciseTimeStamp** hello ETW tidsstämpeln för hello-händelse. Det vill säga loggas hello tid hello händelsen hello-klient.
* **TIDSSTÄMPEL** PreciseTimeStamp avrundas nedåt toohello överför frekvens gräns. Så om din överföringsfrekvensen är 5 minuter och hello händelse tid 00:17:12, blir TIDSSTÄMPEL 00:15:00.
* **Tidsstämpel** hello tidsstämpeln på vilka hello entiteten skapades i hello Azure-tabellen.

**Hur hanterar jag kostnader vid insamling av diagnostikinformation?**

hello standardinställningar (**Certifikatutfärdarnivå** ställa in också**fel** och **överföring period** ställa in också**1 minut**) är utformad toominimize kostnad. Compute-kostnaderna ökar om du samlar in mer diagnostikdata och minska hello överföring period. Samla inte in mer data än vad du behöver och Glöm inte toodisable datainsamling när du inte längre behöver. Du kan alltid aktivera det igen, även vid körning, som visas i hello föregående avsnitt.

**Hur samlar misslyckades begäran loggar från IIS?**

Som standard IIS inte samla in loggar misslyckades-begäran. Du kan konfigurera IIS toocollect dem om du redigerar hello web.config-filen för webbrollen.

**Jag får inte spårningsinformation från RoleEntryPoint metoder som OnStart. Vad är fel?**

hello-metoder för RoleEntryPoint kallas hello gäller WAIISHost.exe inte IIS. Därför hello konfigurationsinformation i web.config som normalt inte gäller aktiverar spårning. tooresolve problemet, Lägg till en .config-fil tooyour webbrollsprojektet och namnge hello filen toomatch hello utdata sammansättningen som innehåller hello RoleEntryPoint kod. I hello standard webbrollsprojektet, skulle hello hello .config-fil vara WAIISHost.exe.config. Lägg sedan till följande rader toothis hello:

```
<system.diagnostics>
  <trace>
      <listeners>
          <add name “AzureDiagnostics” type=”Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener”>
              <filter type=”” />
          </add>
      </listeners>
  </trace>
</system.diagnostics>
```

Nu i hello **egenskaper** fönster, ange hello **kopiera tooOutput Directory** egenskapen för**Kopiera alltid**.

## <a name="next-steps"></a>Nästa steg
toolearn mer om diagnostik loggning i Azure, se [aktiverar diagnostik i Azure-molntjänster och virtuella datorer](cloud-services/cloud-services-dotnet-diagnostics.md) och [aktivera diagnostikloggning för web apps i Azure App Service](app-service-web/web-sites-enable-diagnostic-log.md).

