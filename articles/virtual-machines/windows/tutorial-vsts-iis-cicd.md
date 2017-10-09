---
title: aaaCreate en CI/CD-pipeline i Azure med Team Services | Microsoft Docs
description: "Lär dig hur toocreate en Visual Studio Team Services pipeline för kontinuerlig integration och som distribuerar ett web app tooIIS på en Windows VM"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: b758a124c4742854dd3b543f747fd8700f954414
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-continuous-integration-pipeline-with-visual-studio-team-services-and-iis"></a>Skapa en pipeline för kontinuerlig integrering med Visual Studio Team Services och IIS
Du kan använda en kontinuerlig integrering och distribution (CI/CD) pipeline tooautomate hello build-, test- och faser för distribution av programutveckling. I den här självstudiekursen skapar du en CI/CD-pipeline med hjälp av Visual Studio Team Services och en Windows-dator (VM) i Azure som kör IIS. Lär dig att:

> [!div class="checklist"]
> * Publicera ett ASP.NET-program tooa Team Services webbprojekt
> * Skapa en definition av build som utlöses av koden incheckningar
> * Installera och konfigurera IIS på en virtuell dator i Azure
> * Lägg till hello IIS instans tooa distributionsgruppen i Team Services
> * Skapa en ny webbplats för versionen definition toopublish distribuera paket tooIIS
> * Testa hello CI/CD-pipeline

Den här kursen kräver hello Azure PowerShell module 3,6 eller senare. Kör `Get-Module -ListAvailable AzureRM` toofind hello version. Om du behöver tooupgrade finns [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).


## <a name="create-project-in-team-services"></a>Skapa projekt i Team Services
Visual Studio Team Services möjliggör enkel samarbete och utveckling utan att behålla en hanteringslösning för lokal kod. Team Services innehåller molnet kod testning, bygga och application insights. Du kan välja en version kontrollen lagringsplatsen och IDE som bäst passar din kod utveckling. Du kan använda ett kostnadsfritt konto toocreate en grundläggande ASP.NET webbapp och CI/CD-pipeline för den här självstudiekursen. Om du inte redan har ett Team Services-konto [skapar du en](http://go.microsoft.com/fwlink/?LinkId=307137).

toomanage hello kod commit processen skapa definitioner, och viktiga definitioner, skapa ett projekt i Team Services på följande sätt:

1. Öppna instrumentpanelen Team Services i en webbläsare och välj **nytt projekt**.
2. Ange *myWebApp* för hello **projektnamn**. Lämna alla andra standard värden toouse *Git* versionskontroll och *Agile* process för arbetsobjekt.
3. Välj hello alternativet för**dela med** *gruppmedlemmar*och välj **skapa**.
5. När projektet har skapats, Välj hello alternativet för**initieras med en viktigt- eller gitignore**, sedan **initiera**.
6. I det nya projektet, Välj **instrumentpaneler** hello överst, Välj **öppna i Visual Studio**.


## <a name="create-aspnet-web-application"></a>Skapa ASP.NET-webbprogram
I föregående steg hello skapat du ett projekt i Team Services. hello sista steget öppnar det nya projektet i Visual Studio. Du hanterar din kod incheckningar i hello **Team Explorer** fönster. Skapa en lokal kopia av det nya projektet och sedan skapa en ASP.NET-webbprogram från en mall på följande sätt:

1. Välj **klona** toocreate en lokal git-lagringsplatsen i ditt Team Services-projekt.
    
    ![Klona lagringsplatsen från Team Services-projekt](media/tutorial-vsts-iis-cicd/clone_repo.png)

2. Under **lösningar**väljer **ny**.

    ![Skapa-webbprogramslösning](media/tutorial-vsts-iis-cicd/new_solution.png)

3. Välj **Web** mallar, och välj sedan hello **ASP.NET-webbprogram** mall.
    1. Ange ett namn för programmet, till exempel *myWebApp*, och avmarkerar kryssrutan hello för **Skapa katalog för lösning**.
    2. Om hello alternativet är tillgängligt, avmarkerar du kryssrutan för hello för**Lägg till Application Insights tooproject**. Application Insights kräver du tooauthorize ditt webbprogram med Azure Application Insights. tookeep den enkla i den här självstudiekursen, hoppa över den här processen.
    3. Välj **OK**.
4. Välj **MVC** hello mall-listan.
    1. Välj **ändra autentisering**, Välj **ingen autentisering**och välj **OK**.
    2. Välj **OK** toocreate din lösning.
5. I hello **Team Explorer** fönstret Välj **ändringar**.

    ![Genomför lokala ändringar tooTeam Services git repo](media/tutorial-vsts-iis-cicd/commit_changes.png)

6. Hello genomförande i textrutan anger du ett meddelande som *inledande commit*. Välj **genomför alla och Sync** hello nedrullningsbara menyn.


## <a name="create-build-definition"></a>Skapa build-definition
I Team Services använder du en build definition toooutline hur programmet ska byggas. I den här självstudiekursen skapar vi en grundläggande definition att tar våra källkoden bygger hello lösning och sedan skapar web distribuera paket som vi kan använda toorun hello webbprogram på en IIS-server.

1. I projektet Team Services väljer **Skapa & släpper** hello överst, Välj **bygger**.
3. Välj **+ ny definition**.
4. Välj hello **ASP.NET (FÖRHANDSGRANSKNING)** mall och välj **tillämpa**.
5. Lämna alla hello standard uppgiften värden. Under **hämta källor**, se till att hello *myWebApp* databasen och *master* grenen väljs.

    ![Skapa build-definition i Team Services-projekt](media/tutorial-vsts-iis-cicd/create_build_definition.png)

6. På hello **utlösare** fliken, flytta Hej reglage för **aktivera den här utlösaren** för*aktiverad*.
7. Spara hello build definitions- och kön en ny version genom att välja **Spara & kö** , sedan **Spara & kö** igen. Lämna hello standardvärden och välj **kön**.

Titta på startar som hello build schemaläggs på en värdbaserad agent sedan toobuild. hello utdata är liknande toohello följande exempel:

![Lyckad version av Team Services-projekt](media/tutorial-vsts-iis-cicd/successful_build.png)


## <a name="create-virtual-machine"></a>Skapa en virtuell dator
tooprovide en plattform toorun ditt ASP.NET-webbprogram måste en virtuell Windows-dator som kör IIS. Team Services använder en agent toointeract med hello IIS-instans som du kopplar kod och versioner har utlösts.

Skapa en virtuell Windows Server 2016-dator med hjälp av [detta skriptexempel](../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json). Det tar några minuter för hello skriptet toorun och skapa hello VM. När hello VM har skapats, öppna port 80 för webbtrafik med [Lägg till AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.resources/new-azurermresourcegroup) på följande sätt:

```powershell
Get-AzureRmNetworkSecurityGroup `
  -ResourceGroupName $resourceGroup `
  -Name "myNetworkSecurityGroup" | `
Add-AzureRmNetworkSecurityRuleConfig `
  -Name "myNetworkSecurityGroupRuleWeb" `
  -Protocol "Tcp" `
  -Direction "Inbound" `
  -Priority "1001" `
  -SourceAddressPrefix "*" `
  -SourcePortRange "*" `
  -DestinationAddressPrefix "*" `
  -DestinationPortRange "80" `
  -Access "Allow" | `
Set-AzureRmNetworkSecurityGroup
```

tooconnect tooyour VM, hämta hello offentliga IP-adress med [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) på följande sätt:

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup | Select IpAddress
```

Skapa en fjärrskrivbordssession tooyour VM:

```cmd
mstsc /v:<publicIpAddress>
```

Öppna på hello VM, en **administratör PowerShell** kommandotolk. Installera IIS och nödvändiga .NET-funktioner på följande sätt:

```powershell
Install-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features
```


## <a name="create-deployment-group"></a>Skapa distributionsgrupp
toopush ut hello web driftsätta paketet toohello IIS-server kan du definiera en distributionsgrupp i Team Services. Den här gruppen kan du toospecify vilka servrar som hello mål för nya versioner som du har checkat kod tooTeam tjänster och versioner är slutförda.

1. I Team Services väljer **Skapa & släpper** och välj sedan **distributionsgrupper**.
2. Välj **lägga till distributionsgruppen**.
3. Ange ett namn för hello gruppen som *myIIS*och välj **skapa**.
4. I hello **registrera datorer** Kontrollera *Windows* är markerat och klicka sedan på kryssrutan hello för**använder en personlig åtkomsttoken i hello skript för autentisering**.
5. Välj **kopiera skriptet tooclipboard**.


### <a name="add-iis-vm-toohello-deployment-group"></a>Lägg till IIS VM toohello distributionsgruppen
Lägg till varje IIS-instans toohello grupp med hello distributionsgruppen har skapats. Team Services genererar ett skript som hämtar och konfigurerar en agent på hello VM som tar emot nya distribuera paket och använder sedan den tooIIS.

1. Tillbaka i hello **administratör PowerShell** session på den virtuella datorn, klistra in och kör hello-skript som kopieras från Team Services.
2. När begärd tooconfigure taggar för hello agent väljer *Y* och ange *web*.
3. När du uppmanas att ange hello användarkonto trycker du på *returnera* tooaccept hello standardvärden.
4. Vänta tills hello skriptet toofinish med ett meddelande *vstsagent.account.computername för tjänsten har startats*.
5. I hello **distributionsgrupper** sidan hello **Skapa & släpper** -menyn, öppna hello *myIIS* distributionsgruppen. På hello **datorer** kontrollerar du att den virtuella datorn visas.

    ![TooTeam Services distributionsgruppen har lagts till VM](media/tutorial-vsts-iis-cicd/deployment_group.png)


## <a name="create-release-definition"></a>Skapa en definition för versionen
toopublish din versioner du skapa en definition av versionen i Team Services. Den här definitionen utlöses automatiskt av en lyckad version av programmet. Du väljer hello distribution grupp toopush webbplatsen distribuera paketet till och definiera hello IIS-inställningarna.

1. Välj **Skapa & släpper**och välj **bygger**. Välj hello build definition skapade i föregående steg.
2. Under **nyligen slutförts**, Välj hello senaste build och markerar sedan **versionen**.
3. Välj **Ja** toocreate en definition av versionen.
4. Välj hello **tom** mall och välj sedan **nästa**.
5. Kontrollera hello projektet och källa build-definition fylls med projektet.
6. Välj hello **kontinuerlig distribution** kryssrutan och välj sedan **skapa**.
7. Välj hello listrutan bredvid för**+ Lägg till aktiviteter** och välj *lägga till en distribution grupp fas*.
    
    ![Lägg till toorelease aktivitetsdefinitionen i Team Services](media/tutorial-vsts-iis-cicd/add_release_task.png)

8. Välj **Lägg till** nästa för**IIS Web App Deploy(Preview)**och välj **Stäng**.
9. Välj hello **körs på distributionsgruppen** överordnad aktivitet.
    1. För **distributionsgruppen**väljer hello distribution gruppen du skapade tidigare, såsom *myIIS*.
    2. I hello **datorn taggar** väljer **Lägg till** och välj hello *web* tagg.
    
    ![Släpp definition distribution grupp uppgiften för IIS](media/tutorial-vsts-iis-cicd/release_definition_iis.png)
 
11. Välj hello **distribuera: IIS Web App Deploy** aktivitet tooconfigure din IIS-instansen följande inställningar:
    1. För **webbplatsnamn**, ange *standardwebbplats*.
    2. Lämna alla hello andra standardinställningarna.
12. Välj **spara**och välj **OK** två gånger.


## <a name="create-a-release-and-publish"></a>Skapa en version och publicera
Du kan nu push webbplatsen distribuera paket som en ny version. Det här steget kommunicerar med hello-agenten på varje-instans som är en del av hello distributionsgruppen, skickar hello web distribuera paket och konfigurerar IIS toorun hello uppdateras webbprogram.

1. Markera i definitionen för versionen **+ släpper**, Välj **skapa släpper**.
2. Kontrollera att hello senaste versionen är markerad i hello listrutan, tillsammans med **automatisk distribution: efter att versionen har skapats**. Välj **Skapa**.
3. En liten banderoll visas överst hello av definitionen för versionen som *version 'Version 1' har skapats*. Välj hello versionen länk.
4. Öppna hello **loggar** fliken toowatch hello versionen pågår.
    
    ![Lyckad Team Services versionen och web distribuera paketet push](media/tutorial-vsts-iis-cicd/successful_release.png)

5. När hello-versionen har slutförts, öppna en webbläsare och ange hello offentliga PIA-adressen för den virtuella datorn. ASP.NET-webbprogram körs.

    ![ASP.NET-webbprogram som körs på IIS VM](media/tutorial-vsts-iis-cicd/running_web_app.png)


## <a name="test-hello-whole-cicd-pipeline"></a>Testa hello hela CI/CD-pipeline
Med ditt webbprogram som körs på IIS, försök hello hela CI/CD-pipeline. När du gör en ändring i Visual Studio och spara utlöses koden, en version som utlöser en version av webbplatsen uppdaterade distribuera paketet tooIIS:

1. Öppna i Visual Studio hello **Solution Explorer** fönster.
2. Navigera tooand öppna *myWebApp | Vyer | Start | Index.cshtml*
3. Redigera rad 6 tooread:

    `<h1>ASP.NET with VSTS and CI/CD!</h1>`

4. Spara hello-filen.
5. Öppna hello **Team Explorer** fönster, Välj hello *myWebApp* projektet och välj sedan **ändringar**.
6. Ange ett commit-meddelande som *testning CI/CD-pipeline*, Välj **alla genomförande och Sync** hello nedrullningsbara menyn.
7. I arbetsytan Team Services utlöses en ny version från hello kod genomförande. 
    - Välj **Skapa & släpper**och välj **bygger**. 
    - Välj build-definition, och välj sedan hello **i kö & körs** build toowatch som hello skapa fortskrider.
9. När hello build lyckas, utlöses en ny version.
    - Välj **Skapa & släpper**, sedan **versioner** toosee hello webbdistributionspaket pushas tooyour IIS VM. 
    - Välj hello **uppdatera** ikonen tooupdate hello status. När hello *miljöer* kolumnen visas en grön bock, hello-versionen har distribuerats tooIIS.
11. toosee ändringarna tillämpas, uppdatera IIS-webbplatsen i en webbläsare.

    ![ASP.NET-webbprogram som körs på IIS VM från CI/CD-pipeline](media/tutorial-vsts-iis-cicd/running_web_app_cicd.png)


## <a name="next-steps"></a>Nästa steg

I kursen får du skapade ASP.NET-webbprogram i Team Services och konfigurerade version och utgåva definitioner toodeploy ny webbplats distribuera paket tooIIS på varje kod genomförande. Du har lärt dig att:

> [!div class="checklist"]
> * Publicera ett ASP.NET-program tooa Team Services webbprojekt
> * Skapa en definition av build som utlöses av koden incheckningar
> * Installera och konfigurera IIS på en virtuell dator i Azure
> * Lägg till hello IIS instans tooa distributionsgruppen i Team Services
> * Skapa en ny webbplats för versionen definition toopublish distribuera paket tooIIS
> * Testa hello CI/CD-pipeline

I förväg toohello nästa självstudiekurs toolearn hur toosecure en webbserver med SSL-certifikat.

> [!div class="nextstepaction"]
> [Säker webbserver med SSL](tutorial-secure-web-server.md)