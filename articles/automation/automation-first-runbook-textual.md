---
title: "aaaMy första PowerShell-arbetsflödesrunbook i Azure Automation | Microsoft Docs"
description: "Självstudiekurs med stegvisa hello skapa, testa och publicering av en enkel text-runbook med hjälp av PowerShell-arbetsflöde."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
keywords: "powershell-arbetsflöde, powershell-arbetsflödesexempel, arbetsflöde powershell"
ms.assetid: 0002d7f7-e2b5-46e3-b5eb-4596b84fd526
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/26/2017
ms.author: magoedte;bwren
ms.openlocfilehash: b5a34d363ef4865878f3f68119833367b5280f83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="my-first-powershell-workflow-runbook"></a>Min första PowerShell Workflow-runbook

> [!div class="op_single_selector"]
> * [Grafisk](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell-arbetsflöde](automation-first-runbook-textual.md)
> 
> 

Den här självstudiekursen vägleder dig genom hello skapandet av en [PowerShell-arbetsflödesrunbook](automation-runbook-types.md#powershell-workflow-runbooks) i Azure Automation. Vi börjar med en enkel runbook som vi testa och publicera samtidigt som förklarar hur tootrack hello hello runbook jobbets status. Vi ändra hello runbook tooactually hantera Azure-resurser, i det här fallet startar en virtuell Azure-dator. Slutligen gör vi hello runbook stabilare genom att lägga till runbook-parametrar.

## <a name="prerequisites"></a>Krav
toocomplete den här kursen behöver du hello följande:

* En Azure-prenumeration. Om du inte redan har ett konto kan du [aktivera dina MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller <a href="/pricing/free-account/" target="_blank">[registrera dig för ett kostnadsfritt konto](https://azure.microsoft.com/free/).
* [Automation-konto](automation-sec-configure-azure-runas-account.md) toohold hello runbook och autentisera tooAzure resurser.  Det här kontot måste ha behörighet toostart och stoppa hello virtuella datorn.
* En virtuell dator i Azure. Eftersom vi ska stoppa och starta den här datorn bör det inte vara en virtuell dator som finns i produktionsmiljön.

## <a name="step-1---create-new-runbook"></a>Steg 1 – Skapa en ny runbook
Vi börjar med att skapa en enkel runbook som matar ut hello text *Hello World*.

1. Hello Azure-portalen, öppna ditt Automation-konto.  
   sidan för hello Automation-konto får du en snabb överblick över hello resurser i det här kontot. Du bör redan ha vissa tillgångar. De flesta av dessa är hello-moduler som ingår automatiskt i ett nytt Automation-konto. Du bör också ha hello autentiseringsuppgiftstillgång som nämns i hello [krav](#prerequisites).
2. Klicka på hello **Runbooks** panelen tooopen hello lista över runbooks.<br><br> ![Runbook-kontroll](media/automation-first-runbook-textual/runbooks-control-tiles.png)
3. Skapa en ny runbook genom att klicka på hello **lägga till en runbook** knappen och sedan **skapa en ny runbook**.
4. Namnge hello runbook hello *MyFirstRunbook arbetsflöde*.
5. I det här fallet ska vi toocreate en [PowerShell-arbetsflödesrunbook](automation-runbook-types.md#powershell-workflow-runbooks) så väljer **Powershell-arbetsflöde** för **runbooktyp**.<br><br> ![Ny runbook](media/automation-first-runbook-textual/new-runbook-properties.png)
6. Klicka på **skapa** toocreate hello runbook och öppna hello textrepresentation editor.

## <a name="step-2---add-code-toohello-runbook"></a>Steg 2 – Lägg till kod toohello runbook
Du kan antingen Typkod direkt i hello runbook eller kan du välja cmdlets, runbooks och tillgångar från hello biblioteket kontroll och har dem lagt till toohello runbook med eventuella relaterade parametrar. Den här genomgången ska vi skriva direkt i hello runbook.

1. Vår runbook är tom med endast hello krävs *arbetsflöde* nyckelord, hello namnet på våra runbook och hello klammerparenteser som kommer encase hello hela arbetsflödet.

   ```
   Workflow MyFirstRunbook-Workflow
   {
   }
   ```
2. Skriv *Write-Output "Hello World."* mellan hello klammerparenteser.

   ```
   Workflow MyFirstRunbook-Workflow
   {
   Write-Output "Hello World"
   }
   ```
3. Spara hello runbook genom att klicka på **spara**.<br><br> ![Spara runbook](media/automation-first-runbook-textual/automation-runbook-edit-controls-save.png)

## <a name="step-3---test-hello-runbook"></a>Steg 3 – runbook-testet hello
Innan vi publicerar hello runbook toomake den tillgänglig i produktion, vi vill tootest den toomake till att den fungerar korrekt. När du testar en runbook kör du dess **utkastversion** och visar dess utdata interaktivt.

1. Klicka på **Test fönstret** tooopen hello Test fönstret.<br><br> ![Testfönstret](media/automation-first-runbook-textual/automation-runbook-edit-controls-test.png)
2. Klicka på **starta** toostart hello test. Detta bör vara hello endast aktiverade alternativet.
3. Ett [runbook-jobb](automation-runbook-execution.md) skapas och dess status visas.  
   hello jobbstatus startar som *i kö* som anger att den väntar på en runbook worker i hello molnet toocome tillgängliga. Sedan flyttas för*Start* när en arbetsprocess anspråk hello jobb, och sedan *kör* när hello runbook faktiskt börjar köras.  
4. När hello runbook-jobbet är klart visas dess utdata. I detta fall bör du se *Hello World*.<br><br> ![Hello World](media/automation-first-runbook-textual/test-output-hello-world.png)
5. Stäng hello Test fönstret tooreturn toohello arbetsytan.

## <a name="step-4---publish-and-start-hello-runbook"></a>Steg 4 – publicera och starta hello runbook
hello-runbook som vi just skapade är fortfarande i utkastläge. Vi behöver toopublish den innan vi kan köra den i produktion. När du publicerar en runbook kan du skriva över hello befintliga publicerade versionen med hello utkastet. I vårt fall har vi inte en publicerad version ännu eftersom vi just skapade hello runbook.

1. Klicka på **publicera** toopublish hello runbook och sedan **Ja** när du tillfrågas.<br><br> ![Publicera](media/automation-first-runbook-textual/automation-runbook-edit-controls-publish.png)
2. Om du bläddrar vänstra tooview hello runbook i hello **Runbooks** fönstret nu visas en **redigering Status** av **publicerade**.
3. Rulla tillbaka toohello rätt tooview hello fönstret för **MyFirstRunbook arbetsflöde**.  
   hello alternativ överst hello kan vi toostart hello runbook, schemalägga toostart någon gång hello framtida eller skapa en [webhook](automation-webhooks.md) så att den kan startas via en HTTP-anrop.
4. Vi vill bara toostart hello runbook så klicka på **starta** och sedan **Ja** när du tillfrågas.<br><br> ![Starta runbook](media/automation-first-runbook-textual/automation-runbook-controls-start.png)
5. Ett jobb-fönstret har öppnats med hello runbook-jobb som vi just har skapat. Du kan stänga det här fönstret, men i det här fallet vi lämnar den öppen så att vi kan titta på hello jobb pågår.
6. hello jobbstatus visas i **jobbsammanfattning** och matchar hello status som vi såg när vi testade hello runbook.<br><br> ![Jobbsammanfattning](media/automation-first-runbook-textual/job-pane-status-blade-jobsummary.png)
7. En gång hello runbook status visas *slutförd*, klickar du på **utdata**. hello utdatafönstret har öppnats och kan vi se vår *Hello World*.<br><br> ![Jobbsammanfattning](media/automation-first-runbook-textual/job-pane-status-blade-outputtile.png)  
8. Stäng hello utdatafönstret.
9. Klicka på **alla loggar** tooopen hello dataströmmar fönstret för hello runbook-jobbet. Vi ska bara se *Hello World* i hello utdata stream, men detta kan du visa andra dataströmmar för en runbook-jobb, till exempel utförlig och fel om hello runbook skriver toothem.<br><br> ![Jobbsammanfattning](media/automation-first-runbook-textual/job-pane-status-blade-alllogstile.png)
10. Stäng hello dataströmmar och hello jobbet fönstret tooreturn toohello MyFirstRunbook.
11. Klicka på **jobb** tooopen hello jobb-fönstret för runbook. Detta visar alla hello jobben som skapats av denna runbook. Vi ska bara se ett jobb visas eftersom vi bara körde hello jobbet en gång.<br><br> ![Jobb](media/automation-first-runbook-textual/runbook-control-job-tile.png)
12. Du kan klicka på det här jobbet tooopen hello samma jobb-fönstret som vi visas när vi startade hello runbook. Detta ger dig toogo bakåt i tiden och visa hello-information för alla jobb som har skapats för en viss runbook.

## <a name="step-5---add-authentication-toomanage-azure-resources"></a>Steg 5 – Lägg till autentisering toomanage Azure-resurser
Vi har testat och publicerat vår runbook, men hittills gör den egentligen inget användbart. Vi vill toohave den hantera Azure-resurser. Du kommer inte att kunna toodo med hello-autentiseringsuppgifter som är att om vi inte autentisera kallas tooin hello [krav](#prerequisites). Vi kan göra det med hello **Add-AzureRMAccount** cmdlet.

1. Öppna hello textrepresentation editor genom att klicka på **redigera** i fönstret hello MyFirstRunbook-arbetsflöde.<br><br> ![Redigera runbook](media/automation-first-runbook-textual/automation-runbook-controls-edit.png)
2. Vi behöver inte hello **Write-Output** rad längre, och nu måste du ta bort den.
3. Placera markören hello på en tom rad mellan hello klammerparenteser.
4. Skriv eller kopiera och klistra in följande kod som hanterar hello autentisering med ditt Automation kör som-konto hello:

   ```
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   ```
5. Klicka på **Test fönstret** så att vi kan testa hello runbook.
6. Klicka på **starta** toostart hello test. När den är klar bör du få utdata liknande toohello efter, visa grundläggande information från ditt konto. Det här bekräftar att hello-autentiseringsuppgifter är giltiga.<br><br> ![Autentisera](media/automation-first-runbook-textual/runbook-auth-output.png)

## <a name="step-6---add-code-toostart-a-virtual-machine"></a>Steg 6 – Lägg till kod toostart en virtuell dator
Nu när våra runbook autentiseras tooour Azure-prenumeration, hanterar vi resurser. Vi lägga till ett kommando toostart en virtuell dator. Du kan välja en virtuell dator i din Azure-prenumeration och nu ska vi hardcoding namn i hello runbook.

1. Efter *Add-AzureRmAccount*, typen *Start AzureRmVM-namnet 'VMName' - ResourceGroupName 'NameofResourceGroup'* tillhandahåller hello namn och resursgruppen på hello virtuella toostart.  

   ```
   workflow MyFirstRunbook-Workflow
   {
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName'
   }
   ```
2. Spara hello runbook och klicka sedan på **Test fönstret** så att vi kan testa den.
3. Klicka på **starta** toostart hello test. När den är klar kan du kontrollera att hello den virtuella datorn har startats.

## <a name="step-7---add-an-input-parameter-toohello-runbook"></a>Steg 7 – Lägg till en indataparameter toohello runbook
Vår runbook startar hello virtuella datorn som vi hårdkodad i hello runbook, men det är mer användbar om vi kan ange hello virtuell dator när hello runbook startas. Vi ska nu lägga till indataparametrar toohello runbook tooprovide funktionen.

1. Lägga till parametrar för *VMName* och *ResourceGroupName* toohello runbook och använda dessa variabler med hello **Start AzureRmVM** cmdlet som hello exemplet nedan.

   ```
   workflow MyFirstRunbook-Workflow
   {
    Param(
     [string]$VMName,
     [string]$ResourceGroupName
    )  
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
   }
   ```
2. Spara hello runbook och öppna hello Test. Observera att du kan nu ange värden för hello två inkommande variabler som ska användas i hello test.
3. Stäng hello Test-fönstret.
4. Klicka på **publicera** toopublish hello ny version av hello runbook.
5. Stoppa hello virtuell dator som du startade i hello föregående steg.
6. Klicka på **starta** toostart hello runbook. Typen i hello **VMName** och **ResourceGroupName** för hello virtuell dator som du ska toostart.<br><br> ![Starta runbook](media/automation-first-runbook-textual/automation-pass-params.png)<br>  
7. När hello runbook är klar kontrollerar du att hello den virtuella datorn har startats.  

## <a name="next-steps"></a>Nästa steg
* tooget igång med grafiska runbooks finns [min första grafisk runbook](automation-first-runbook-graphical.md)
* tooget igång med PowerShell runbooks finns [min första PowerShell-runbook](automation-first-runbook-textual-powershell.md)
* toolearn mer om runbook-typer, fördelar och begränsningar finns [typer för Azure Automation-runbook](automation-runbook-types.md)
* Mer information om PowerShell-skriptstöd finns i [Inbyggt PowerShell-skriptstöd i Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)
