---
title: aaaDebugging en Azure cloud service eller en virtuell dator i Visual Studio | Microsoft Docs
description: "Felsökning av en molnbaserad tjänst eller en virtuell dator i Visual Studio"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 945e06e0-2100-41af-b218-72347367ddab
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 32a326430021ba2ea9317a6a71fa005d4b87c273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="debugging-an-azure-cloud-service-or-virtual-machine-in-visual-studio"></a>Felsökning av en Azure-molntjänst eller en virtuell dator i Visual Studio
Visual Studio finns olika alternativ för felsökning av Azure-molntjänster och virtuella datorer.

## <a name="debug-your-cloud-service-on-your-local-computer"></a>Felsöka din molntjänst på den lokala datorn
Du kan spara tid och pengar med hello Azure compute emulator toodebug Molntjänsten på en lokal dator. Du kan felsöka en tjänst lokalt innan du distribuerar det för att förbättra pålitligheten och prestandan utan att betala för beräkning tid. Men vissa kan det uppstå fel bara när du kör en molnbaserad tjänst i Azure sig själv. Du kan felsöka dessa fel om du aktiverar fjärrfelsökning när du publicerar din tjänst och sedan koppla hello felsökare tooa rollinstans.

hello-emulatorn simulerar hello Azure Compute-tjänsten och körs i din lokala miljö så att du kan testa och felsöka din molntjänst innan du distribuerar den. hello-emulatorn hanterar hello livscykeln för dina rollinstanser och ger åtkomst toosimulated resurser, till exempel lokal lagring. När du felsöker eller köra tjänsten från Visual Studio startar automatiskt hello emulatorn som ett program i bakgrunden och distribuerar din tjänst toohello emulatorn. Du kan använda hello emulatorn tooview tjänsten när den körs i hello lokal miljö. Du kan köra hello fullständig version eller hello hello-emulatorns uttrycklig version. (Från och med Azure 2.3 hello hello-emulatorns uttrycklig version är hello standard). Se [med emulatorn Express tooRun och felsöka en Cloud Service lokalt](https://msdn.microsoft.com/library/dn339018.aspx).

### <a name="toodebug-your-cloud-service-on-your-local-computer"></a>toodebug ditt moln-tjänst på den lokala datorn
1. På menyraden hello väljer **felsöka**, **Start Debugging** toorun ditt Azure cloud service-projekt. Alternativt kan du trycka på F5. Du ser ett meddelande som hello Compute Emulator startas. När hello emulatorn startar bekräftar den hello ikonen i systemfältet.

    ![Azure-emulatorn i hello systemfältet](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC783828.png)
2. Visa hello användargränssnitt för hello beräkningsemulatorn genom att öppna hello snabbmenyn för hello Azure ikon i meddelandefältet hello och välj sedan **visa Compute Emulator UI**.

    hello till vänster i hello Användargränssnittet visar hello-tjänster som för närvarande är distribuerat toohello compute emulator och hello rollinstanser som varje tjänst körs. Du kan välja hello service eller roller toodisplay livscykel, loggning och diagnostisk information i hello till höger. Om du placerar hello fokus i hello den övre marginalen i en inkluderad fönstret expanderar toofill hello högra rutan.
3. Stega igenom hello program genom att välja kommandon på hello **felsöka** -menyn och ange brytpunkter i koden. Eftersom du gå igenom hello programmet hello felsökning uppdateras hello fönster med hello aktuell status för programmet hello. När du stoppar felsökning tas hello programdistribution bort. Om ditt program innehåller en webbroll och du har ställt in hello startade åtgärden egenskapen toostart hello webbläsare, startar Visual Studio ditt webbprogram i hello webbläsare. Om du ändrar hello antal instanser av en roll i hello tjänstekonfigurationen måste du stoppa Molntjänsten och sedan starta om felsökningen om så att du kan felsöka de här nya instanser av hello rollen.

    **Obs:** när du stoppas eller felsökning din tjänst hello lokala beräkningsemulatorn och storage-emulatorn inte stoppas. Du måste stoppa dem uttryckligen från hello meddelandefältet.

## <a name="debug-a-cloud-service-in-azure"></a>Felsöka en molnbaserad tjänst i Azure
toodebug en molnbaserad tjänst från en fjärrdator, måste du aktivera funktionen explicit när du distribuerar din molntjänst så att nödvändiga tjänster (till exempel msvsmon.exe) är installerade på hello virtuella datorer som kör rollinstanser av. Om du inte aktiverar fjärrfelsökning när du publicerade hello service kan har du toorepublish hello med fjärrfelsökning aktiverad.

Om du aktiverar fjärrfelsökning för en tjänst i molnet, inte den minskad prestanda eller ytterligare avgifter. Du bör inte använda fjärrfelsökning för en tjänst för produktion, eftersom klienter som använder tjänsten hello kan påverkas negativt.

> [!NOTE]
> När du publicerar en molnbaserad tjänst från Visual Studio kan du aktivera **IntelliTrace** för alla roller som tjänst som mål hello .NET Framework 4 eller hello .NET Framework 4.5. Med hjälp av **IntelliTrace**, kan du granska händelser som inträffade i en rollinstans av i hello senaste och återskapa hello kontext än den tiden. Se [felsökning en publicerade molntjänst med IntelliTrace och Visual Studio](http://go.microsoft.com/fwlink/?LinkID=623016) och [med IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx).
>
>

### <a name="tooenable-remote-debugging-for-a-cloud-service"></a>tooenable felsökning för en tjänst i molnet
1. Öppna hello snabbmenyn för hello Azure-projekt och välj sedan **publicera**.
2. Välj hello **mellanlagring** miljö och hello **felsöka** konfiguration.

    Detta är endast en rekommendation. Du kan välja toorun din testmiljöer i en produktionsmiljö. Men kan du påverkas negativt användare om du aktiverar fjärrfelsökning i hello-produktionsmiljö. Du kan välja hello versionen konfiguration, men hello Debug konfigurationen gör felsökning.

    ![Välj hello Debug-konfiguration](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746717.gif)
3. Gör hello vanligt, men väljer hello **aktivera fjärråtkomst felsökare för alla roller** kryssrutan på hello **avancerade inställningar** fliken.

    ![Felsöka konfiguration](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746718.gif)

### <a name="tooattach-hello-debugger-tooa-cloud-service-in-azure"></a>Molntjänsten för tooattach hello felsökare tooa i Azure
1. I Server Explorer expanderar du hello noden för Molntjänsten.
2. Öppna hello snabbmenyn för hello roll eller rollen instans toowhich du vill tooattach och välj sedan **koppla felsökare**.

    Om du felsöka en roll bifogar hello Visual Studio-felsökaren tooeach instans av rollen. hello felsökare bryts på en brytpunkt för hello första rollinstansen som kör den kodraden och uppfyller eventuella villkor för att brytpunkt. Om du felsöka en instans bifogar hello felsökare tooonly instansen och sidbrytningar på en brytpunkt när den specifika instansen körs den kodraden och uppfyller hello brytpunkts villkor.

    ![Koppla felsökare](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746719.gif)
3. När hello felsökare bifogar tooan instans, felsöka som vanligt. hello felsökare bifogar automatiskt toohello lämplig värdprocess för din roll. Beroende på vilka hello roll är hello felsökare bifogar toow3wp.exe, WaWorkerHost.exe eller WaIISHost.exe. tooverify hello processen toowhich hello felsökare, expandera noden för hello-instans i Server Explorer. Se [Azure rollen arkitektur](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) mer information om Azure processer.

    ![Välj typ i dialogrutan kod](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)
4. tooidentify hello processer toowhich hello felsökare, dialogrutan hello processer, på hello menyraden, välja Debug, Windows, processer. (Tangentbord: Ctrl + Alt + Z) toodetach en specifik process, öppna dess snabbmenyn och välj sedan **koppla från processen**. Eller, hitta hello instans nod i Server Explorer, hitta hello process, öppna dess snabbmenyn och välj sedan **koppla från processen**.

    ![Felsöka processer](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC690787.gif)

> [!WARNING]
> Undvika lång stopp vid brytpunkter när remote felsökning. Azure behandlar en process som har stoppats för längre än några minuter som inte svarar och slutar att skicka trafik toothat instans. Om du stoppar för länge lossa msvsmon.exe från hello-processen.
>
>

toodetach hello felsökare från alla processer i din instans eller roll, öppna hello snabbmenyn för hello roll eller instans som du felsöker och välj sedan **frånkoppling felsökare**.

## <a name="limitations-of-remote-debugging-in-azure"></a>Begränsningar för fjärrfelsökning i Azure
Från Azure SDK 2.3 har fjärrfelsökning hello följande begränsningar.

* Med fjärrfelsökning aktiverad, kan du publicera en molnbaserad tjänst där någon roll har mer än 25 instanser.
* hello felsökare använder portar 30400 too30424, 31400 too31424 och 32400 too32424. Om du försöker toouse någon av dessa portar kan du inte kan toopublish din tjänst och en av följande felmeddelanden hello visas i hello aktivitetsloggen för Azure:

  * Det gick inte att verifiera hello .cscfg-filen mot hello .csdef-filen.
    hello reserverade portintervall 'område' för slutpunkten Microsoft.WindowsAzure.Plugins.RemoteDebugger.Connector av rollen rollen överlappar ett redan definierad port eller ett intervall.
  * Det gick inte att allokera. Försök igen senare, försök att minska hello VM-storlek eller antalet rollinstanser, eller försök att distribuera tooa annan region.

## <a name="debugging-azure-virtual-machines"></a>Felsökning av virtuella Azure-datorer
Du kan felsöka program som körs på virtuella Azure-datorer med hjälp av Server Explorer i Visual Studio. När du aktiverar fjärrfelsökning i en virtuell Azure-dator, installerar Azure hello remote felsökning tillägg på hello virtuell dator. Sedan kan du bifoga tooprocesses på hello virtuell dator och felsöka som vanligt.

> [!NOTE]
> Virtuella datorer som skapats via hello Azure resource manager-stacken kan felsökas via fjärranslutning med hjälp av Cloud Explorer i Visual Studio 2015. Mer information finns i [hantera Azure-resurser med Cloud Explorer](http://go.microsoft.com/fwlink/?LinkId=623031).
>
>

### <a name="toodebug-an-azure-virtual-machine"></a>toodebug en virtuell Azure-dator
1. I Server Explorer expanderar du hello nod för virtuella datorer och välj hello nod för hello virtuell dator som du vill toodebug.
2. Öppna hello snabbmenyn och välj **aktivera felsökning**. När du tillfrågas om du är säker på om du vill tooenable felsökning på hello virtuell dator, väljer **Ja**.

    Azure installerar hello remote felsökning tillägg på hello virtuella tooenable felsökning.

    ![Aktivera felsökning kommando för virtuell dator](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Azure-aktivitetsloggen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)
3. När hello remote felsökning tillägg har installerats, öppna snabbmenyn hello virtuella datorn och väljer **koppla felsökare...**

    Azure hämtar en lista över hello processer på hello virtuella datorn och visar dem hello bifoga tooProcess i dialogrutan.

    ![Koppla felsökare kommando](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)
4. I hello **bifoga tooProcess** dialogrutan **Välj** toolimit hello resultatlistan tooshow endast hello typer kod som du vill toodebug. Du kan felsöka 32 - eller 64-bitars hanterad kod, maskinkod eller båda.

    ![Välj typ i dialogrutan kod](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)
5. Välj hello processer toodebug på hello virtuella datorn och sedan markera **bifoga**. Du kan till exempel välja hello w3wp.exe-processen om du vill toodebug ett webbprogram på hello virtuella datorn. Se [felsöka en eller flera processer i Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) och [Azure rollen arkitektur](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) för mer information.

## <a name="create-a-web-project-and-a-virtual-machine-for-debugging"></a>Skapa ett webbprojekt och en virtuell dator för felsökning
Innan du publicerar ditt Azure-projekt kan det vara användbart tootest den i en innesluten miljö som har stöd för felsökning och testa scenarier och där du kan installera testning och övervaka program. Det här är ett sätt toodo tooremotely felsöka appen på en virtuell dator.

Visual Studio ASP.NET-projekt erbjuder en alternativet toocreate en praktisk virtuell dator som du kan använda för att testa appen. hello virtuell dator innehåller vanligtvis behövs slutpunkter som PowerShell, fjärrskrivbord och WebDeploy.

### <a name="toocreate-a-web-project-and-a-virtual-machine-for-debugging"></a>toocreate ett webbprojekt och en virtuell dator för felsökning
1. Skapa ett nytt ASP.NET-webbprogram i Visual Studio.
2. I hello nytt ASP.NET-projekt dialogruta i hello Azure avsnittet väljer du **virtuella** i listrutan hello.. Lämna hello **skapa fjärresurser** kryssrutan är markerad. Välj **OK** tooproceed.

    Hej **Skapa virtuell dator på Azure** dialogrutan visas.

    ![ASP.NET web project dialogrutan Skapa](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746723.png)

    **Obs:** blir du ombedd toosign i tooyour Azure-konto om du inte redan är inloggad.

1. Välj hello olika inställningar för hello virtuella datorn och välj sedan **OK**. Se [virtuella datorer](http://go.microsoft.com/fwlink/?LinkId=623033) för mer information.

    hello namnet du anger för DNS-namn är hello hello virtuella datorns namn.

    ![Skapa virtuell dator på Azure dialogrutan](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746724.png)

    Azure skapar hello virtuell dator och sedan etablerar och konfigurerar hello slutpunkter, till exempel Remote Desktop- och webbdistribution
2. Efter hello virtuell dator har konfigurerats helt, Välj hello virtuella datorns nod i Server Explorer.
3. Öppna hello snabbmenyn och välj **aktivera felsökning**. När du tillfrågas om du är säker på om du vill tooenable felsökning på hello virtuell dator, väljer **Ja**.

    Azure installerar hello felsökning tillägget toohello virtuella tooenable fjärrfelsökning.

    ![Aktivera felsökning kommando för virtuell dator](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Azure-aktivitetsloggen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)
4. Publicera dina projekt som beskrivs i [så här: distribuera en Web Project med ett klick i Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx). Eftersom du vill toodebug hello virtuella datorn på hello **inställningar** sidan hello **Publicera webbplats** väljer **felsöka** som hello konfiguration. Detta säkerställer att kortnamn är tillgängliga när du felsöker.

    ![Inställningar för publicering](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718349.png)
5. I hello **alternativ för Filpublicering**väljer **ta bort extra filer från destinationen** om hello-projektet har redan distribuerats vid en tidigare tidpunkt.
6. När du publicerar hello projektet på snabbmenyn för hello virtuell dator i Server Explorer markerar **koppla felsökare...**

    Azure hämtar en lista över hello processer på hello virtuella datorn och visar dem hello bifoga tooProcess i dialogrutan.

    ![Koppla felsökare kommando](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)
7. I hello **bifoga tooProcess** dialogrutan **Välj** toolimit hello resultatlistan tooshow endast hello typer kod som du vill toodebug. Du kan felsöka 32 - eller 64-bitars hanterad kod, maskinkod eller båda.

    ![Välj typ i dialogrutan kod](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)
8. Välj hello processer toodebug på hello virtuella datorn och sedan markera **bifoga**. Du kan till exempel välja hello w3wp.exe-processen om du vill toodebug ett webbprogram på hello virtuella datorn. Se [felsöka en eller flera processer i Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) för mer information.

## <a name="next-steps"></a>Nästa steg
* Använd **Intellitrace** toocollect en logg över anrop och händelser från en server för versionen. Se [felsökning en publicerade molntjänst med IntelliTrace och Visual Studio](http://go.microsoft.com/fwlink/?LinkID=623016).
* Använd **Azure Diagnostics** toolog detaljerad information från kod som körs inom roller, oavsett om hello roller körs i hello utvecklingsmiljö eller i Azure. Se [samlar in loggningsdata med hjälp av Azure-diagnostik](http://go.microsoft.com/fwlink/p/?LinkId=400450).
