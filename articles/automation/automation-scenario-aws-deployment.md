---
title: aaaAutomating distribution av en virtuell dator i Amazon Web Services | Microsoft Docs
description: "Den här artikeln visar hur toouse Azure Automation tooautomate skapandet av en Amazon Web Service VM"
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 1d85c01a-d795-4523-8194-84fc15b53838
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/14/2017
ms.author: tiandert; bwren
ms.openlocfilehash: dd1f3af47d8700ced85a3ee5a406dde1c1311990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario---provision-an-aws-virtual-machine"></a>Azure Automation-scenario – etablera en virtuell dator i AWS
I den här artikeln visar vi hur du kan utnyttja Azure Automation tooprovision en virtuell dator i din prenumeration Amazon Web Service (AWS) och ge den virtuella datorn ett specifikt namn – vilka AWS refererar tooas ”märkning” hello VM.

## <a name="prerequisites"></a>Krav
Hello enligt den här artikeln behöver du toohave ett Azure Automation-konto och en AWS-prenumeration. Mer information om att konfigurera en Azure Automation-konto och konfigurera den med dina autentiseringsuppgifter för AWS-prenumeration, granska [konfigurera autentisering med Amazon Web Services](automation-configure-aws-account.md).  Det här kontot ska skapas eller uppdateras med autentiseringsuppgifterna AWS prenumeration innan du fortsätter, som vi kommer att referera till det här kontot i hello stegen nedan.

## <a name="deploy-amazon-web-services-powershell-module"></a>Distribuera Amazon Web Services PowerShell-modul
Vår VM etablering runbook utnyttja hello AWS PowerShell-modulen toodo sitt arbete. Utföra hello följande steg tooadd hello modulen tooyour Automation-konto som har konfigurerats med dina autentiseringsuppgifter för AWS-prenumeration.  

1. Öppna webbläsaren och gå toohello [PowerShell-galleriet](http://www.powershellgallery.com/packages/AWSPowerShell/) och klicka på hello **distribuera tooAzure Automation knappen**.<br><br> ![AWS PS Modulimporten](./media/automation-scenario-aws-deployment/powershell-gallery-download-awsmodule.png)
2. Du tas toohello Azure inloggningssidan och efter autentisering, du kommer att dirigeras toohello Azure-portalen och presenteras med hello följande bladet.<br><br> ![Importera modulen bladet](./media/automation-scenario-aws-deployment/deploy-aws-powershell-module-parameters.png)
3. Välj hello resursgruppen från hello **resursgruppen** nedrullningsbara listan och på bladet parametrar hello anger hello följande information:
   
   * Från hello **nytt eller befintligt Automation-konto (sträng)** listrutan Välj **befintliga**.  
   * I hello **Automation kontonamn (sträng)** rutan Ange hello exakta namnet för hello Automation-konto som innehåller hello autentiseringsuppgifterna för prenumerationen AWS.  Till exempel om du har skapat ett särskilt konto med namnet **AWSAutomation**, då du skriver i hello.
   * Välj hello aktuell region från hello **Automation Kontoplats** listrutan.
4. När du har slutfört anger hello krävs information klickar du på **skapa**.
   
   > [!NOTE]
   > När du importerar en PowerShell-modul till Azure Automation kan den också extraherar hello-cmdlets och dessa aktiviteter visas inte förrän hello modulen har slutat import och extraherar hello-cmdlets. Den här processen kan ta några minuter.  
   > <br>
   > 
   > 
5. Öppna ditt Automation-konto i steg 3 i hello Azure-portalen.
6. Klicka på hello **tillgångar** panelen och på hello **tillgångar** bladet, Välj hello **moduler** panelen.
7. På hello **moduler** bladet visas hello **AWSPowerShell** modul i hello-listan.

## <a name="create-aws-deploy-vm-runbook"></a>Skapa AWS distribuera VM-runbook
En gång hello AWS PowerShell-modulen har distribuerats, vi nu skapa en runbook tooautomate etablera en virtuell dator i AWS använder ett PowerShell-skript. hello stegen nedan visar hur tooleverage ursprunglig PowerShell-skript i Azure Automation.  

> [!NOTE]
> För ytterligare alternativ och information om det här skriptet besöker hello [PowerShell-galleriet](https://www.powershellgallery.com/packages/New-AwsVM/DisplayScript).
> 

1. Hämta hello PowerShell-skript ny AwsVM från hello PowerShell-galleriet genom att öppna en PowerShell-session och skriva hello följande:<br>
   ```
   Save-Script -Name New-AwsVM -Path <path>
   ```
   <br>
2. Från hello Azure-portalen, öppna ditt Automation-konto och klicka på hello **Runbooks** panelen.  
3. Från hello **Runbooks** bladet väljer **lägga till en runbook**.
4. På hello **lägga till en runbook** bladet väljer **Snabbregistrering** (skapa en ny runbook).
5. På hello **Runbook** egenskapsbladet, ange ett namn i hello namn för din runbook och från hello **runbooktyp** listrutan Välj **PowerShell**, och klicka sedan på  **Skapa**.<br><br> ![Importera modulen bladet](./media/automation-scenario-aws-deployment/runbook-quickcreate-properties.png)
6. När hello redigera PowerShell-Runbook-bladet visas, kopiera och klistra in hello PowerShell-skript i hello runbook authoring arbetsytan.<br><br> ![Runbook PowerShell-skript](./media/automation-scenario-aws-deployment/runbook-powershell-script.png)<br>
   
    > [!NOTE]
    > Observera följande hello när du arbetar med hello exempel PowerShell-skript:
    > 
    > * Hej runbook innehåller ett antal standardparametervärden. Utvärdera alla standardvärden och uppdatera vid behov.
    > * Om du har lagrat AWS-autentiseringsuppgifter som en autentiseringstillgång med namnet på ett annat sätt än **AWScred**, behöver du tooupdate hello skriptet på raden 57 toomatch därefter.  
    > * När du arbetar med hello AWS CLI-kommandon i PowerShell, särskilt med exempel-runbook måste du ange hello AWS region. Annars misslyckas hello-cmdlets.  Visa AWS avsnittet [ange AWS Region](http://docs.aws.amazon.com/powershell/latest/userguide/pstools-installing-specifying-region.html) i hello AWS-verktyg för PowerShell-dokumentet för mer information.  
    >

7. tooretrieve en lista över namn på bilder från prenumerationen AWS starta PowerShell ISE och importera hello AWS PowerShell-modulen.  Autentisera mot AWS genom att ersätta **Get-AutomationPSCredential** i ISE-miljön med **AWScred = Get-Credential**.  Detta kommer att fråga efter dina autentiseringsuppgifter och du kan ge din **åtkomst nyckel-ID** för hello användarnamn och **hemlig nyckel för åtkomst** hello lösenord.  Se hello exemplet nedan:  

        #Sample tooget hello AWS VM available images
        #Please provide hello path where you have downloaded hello AWS PowerShell module
        Import-Module AWSPowerShell
        $AwsRegion = "us-west-2"
        $AwsCred = Get-Credential
        $AwsAccessKeyId = $AwsCred.UserName
        $AwsSecretKey = $AwsCred.GetNetworkCredential().Password
   
        # Set up hello environment tooaccess AWS
        Set-AwsCredentials -AccessKey $AwsAccessKeyId -SecretKey $AwsSecretKey -StoreAs AWSProfile
        Set-DefaultAWSRegion -Region $AwsRegion
   
        Get-EC2ImageByName -ProfileName AWSProfile

    hello returneras följande utdata:<br><br>
   ![Hämta AWS bilder](./media/automation-scenario-aws-deployment/powershell-ise-output.png)<br>  
8. Kopiera och klistra in hello en hello avbildningsnamn i ett Automation-variabel som refereras till i hello runbook **$InstanceType**. Eftersom ledigt AWS nivåer i det här exemplet använder vi hello prenumerationen, använder vi **t2.micro** i vårt exempel runbook.  
9. Spara hello runbook och klicka sedan på **publicera** toopublish hello runbook och sedan **Ja** när du tillfrågas.

### <a name="testing-hello-aws-vm-runbook"></a>Testa hello AWS VM-runbook
Innan vi fortsätter tester hello runbook, måste tooverify några saker. Närmare bestämt:  

* En tillgång för att autentisera mot AWS har skapats kallas **AWScred** eller hello skript har uppdaterats tooreference hello namnet på din autentiseringsuppgiftstillgång.    
* hello AWS PowerShell-modulen har importerats i Azure Automation  
* En ny runbook har skapats och parametervärden har verifierats och uppdateras vid behov  
* **Logga utförliga poster** och eventuellt **Förloppsposterna** under hello runbook inställning **loggning och spårning** har ställts in för**på**.<br><br> ![Runbook-loggning och spårning](./media/automation-scenario-aws-deployment/runbook-settings-logging-and-tracing.png)  

1. Vi vill toostart hello runbook, så klicka på **starta** och klicka sedan på **OK** när hello starta Runbook-bladet öppnas.
2. På hello starta Runbook-bladet, anger du en **VMname**.  Acceptera hello standardvärden för hello andra parametrar som du tidigare förkonfigurerade i hello skript.  Klicka på **OK** toostart hello runbook-jobbet.<br><br> ![Starta ny AwsVM runbook](./media/automation-scenario-aws-deployment/runbook-start-job-parameters.png)
3. Ett jobb-fönstret har öppnats med hello runbook-jobb som vi just har skapat. Stäng det här fönstret.
4. Vi kan visa förloppet för hello jobbet och visa utdata **dataströmmar** genom att välja hello **alla loggar** panelen från hello runbook jobb-bladet.<br><br> ![Strömmad utdata](./media/automation-scenario-aws-deployment/runbook-job-streams-output.png)
5. tooconfirm hello VM etableras, logga in på hello AWS-hanteringskonsolen om du för närvarande inte har loggat in.<br><br> ![AWS-konsolen distribueras VM](./media/automation-scenario-aws-deployment/aws-instances-status.png)

## <a name="next-steps"></a>Nästa steg
* tooget igång med grafiska runbooks finns [min första grafisk runbook](automation-first-runbook-graphical.md)
* tooget igång med PowerShell arbetsflöde runbooks finns [min första PowerShell-arbetsflödesrunbook](automation-first-runbook-textual.md)
* tooknow mer om runbook-typer, fördelar och begränsningar finns [typer för Azure Automation-runbook](automation-runbook-types.md)
* Mer information om PowerShell-skriptstöd finns i [Inbyggt PowerShell-skriptstöd i Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)

