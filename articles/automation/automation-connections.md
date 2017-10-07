---
title: "aaaConnection tillgångar i Azure Automation | Microsoft Docs"
description: "Anslutningstillgångar i Azure Automation innehålla hello information som krävs för tooconnect tooan externa tjänster eller program från en runbook eller DSC-konfigurationen. Den här artikeln förklarar hello information om anslutningar och hur toowork med dem i både text och grafiska redigering."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: f0239017-5c66-4165-8cca-5dcb249b8091
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/13/2017
ms.author: magoedte; bwren
ms.openlocfilehash: f0f6b9fb960789b34af7b60eb1069313fdcf071c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connection-assets-in-azure-automation"></a>Anslutningstillgångar i Azure Automation

Ett Automation-anslutningstillgång innehåller hello information som krävs för tooconnect tooan externa tjänster eller program från en runbook eller DSC-konfigurationen. Detta kan inkludera information som krävs för autentisering, till exempel användarnamn och lösenord i tooconnection mer information, till exempel en URL eller en port. hello-värdet för en anslutning hindrar alla hello egenskaper för att ansluta tooa visst program i en tillgång som skillnad från toocreating flera variabler. hello-användaren kan redigera hello värden för en anslutning i en enda plats och du kan skicka hello namnet på en anslutning tooa runbook eller DSC-konfigurationen i en parameter. Hej egenskaper för en anslutning kan nås i hello runbook eller DSC-konfigurationen med hello **Get-AutomationConnection** aktivitet.

När du skapar en anslutning måste du ange en *anslutningstypen*. hello anslutningstypen är en mall som definierar en uppsättning egenskaper. hello anslutning definierar värden för varje egenskap som definierats i dess anslutningstypen. Anslutningar som har lagts till tooAzure Automation i integreringsmoduler eller skapats med hello [Azure Automation API](http://msdn.microsoft.com/library/azure/mt163818.aspx) om hello integrationsmodul innehåller en anslutningstyp och importeras till ditt Automation-konto. I annat fall måste toocreate en metadata filen toospecify ett Automation-anslutningstypen.  Mer information om detta finns [integreringsmoduler](automation-integration-modules.md).  

>[!NOTE] 
>Säkra tillgångar i Azure Automation inkluderar autentiseringsuppgifter, certifikat, anslutningar och krypterade variabler. Dessa tillgångar krypteras och lagras i hello Azure Automation med en unik nyckel som skapas för varje automation-konto. Den här nyckeln är krypterad med ett certifikat för master och lagras i Azure Automation. Innan de lagras en säker tillgång hello nyckel för hello automation-konto dekrypteras med hello master certifikatet och sedan används tooencrypt hello tillgången.

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell-Cmdlets

hello-cmdlets i följande tabell hello är används toocreate och hantera anslutningar för Automation med Windows PowerShell. De levereras som en del av hello [Azure PowerShell-modulen](/powershell/azure/overview) som är tillgänglig för användning i Automation-runbooks och DSC-konfigurationer.

|Cmdlet|Beskrivning|
|:---|:---|
|[Get-AzureRmAutomationConnection](/powershell/module/azurerm.automation/get-azurermautomationconnection)|Hämtar en anslutning. Innehåller en hashtabell med hello värden för hello anslutning fält.|
|[Ny AzureRmAutomationConnection](/powershell/module/azurerm.automation/new-azurermautomationconnection)|Skapar en ny anslutning.|
|[Ta bort AzureRmAutomationConnection](/powershell/module/azurerm.automation/remove-azurermautomationconnection)|Ta bort en befintlig anslutning.|
|[Ange AzureRmAutomationConnectionFieldValue](/powershell/module/azurerm.automation/set-azurermautomationconnectionfieldvalue)|Anger hello-värdet för ett visst fält för en befintlig anslutning.|

## <a name="activities"></a>Aktiviteter

hello aktiviteter i hello i den följande tabellen finns används tooaccess anslutningar i en runbook eller DSC-konfigurationen.

|Aktiviteter|Beskrivning|
|---|---|
|[Get-AutomationConnection](/powershell/module/azure/get-azureautomationconnection?view=azuresmps-3.7.0)|Hämtar en anslutning toouse. Returnerar en hash-tabell med hello egenskaperna för hello-anslutning.|

>[!NOTE] 
>Du bör undvika att använda variabler med hello – Name-parametern i **Get - AutomationConnection** eftersom detta kan göra det svårare att hitta beroenden mellan runbooks eller DSC-konfigurationer och anslutningstillgångar i designläge.

## <a name="creating-a-new-connection"></a>Skapa en ny anslutning

### <a name="toocreate-a-new-connection-with-hello-azure-portal"></a>toocreate en ny anslutning med hello Azure-portalen

1. Från ditt automation-konto klickar du på hello **tillgångar** del tooopen hello **tillgångar** bladet.
2. Klicka på hello **anslutningar** del tooopen hello **anslutningar** bladet.
3. Klicka på **lägga till en anslutning** hello överst i hello-bladet.
4. I hello **typen** listrutan, Välj hello typ av anslutning du vill toocreate. hello formuläret visas hello egenskaper för den här typen.
5. Slutför hello formuläret och klickar på **skapa** toosave hello ny anslutning.

### <a name="toocreate-a-new-connection-with-hello-azure-classic-portal"></a>toocreate en ny anslutning med hello klassiska Azure-portalen

1. Från ditt automation-konto klickar du på **tillgångar** hello överst i fönstret hello.
2. Hello längst ned i hello-fönstret klickar du på **Lägg till inställning**.
3. Klicka på **Lägg till anslutning**.
4. I hello **anslutningstypen** listrutan, Välj hello typ av anslutning du vill toocreate.  hello guiden presenterar hello egenskaper för den här typen.
5. Slutför guiden för hello och klicka på hello kryssrutan toosave hello ny anslutning.

### <a name="toocreate-a-new-connection-with-windows-powershell"></a>toocreate en ny anslutning med Windows PowerShell

Skapa en ny anslutning med Windows PowerShell med hjälp av hello [ny AzureRmAutomationConnection](/powershell/module/azurerm.automation/new-azurermautomationconnection) cmdlet. Denna cmdlet har en parameter med namnet **ConnectionFieldValues** som förväntar sig en [hash-tabell](http://technet.microsoft.com/library/hh847780.aspx) Definiera värden för varje hello egenskaper som definierats av hello-anslutningstypen.

Om du är bekant med hello Automation [kör som-konto](automation-sec-configure-azure-runas-account.md) tooauthenticate runbooks med hello tjänstens huvudnamn hello PowerShell-skriptet som tillhandahålls som ett alternativt toocreating hello hello kör som-konto från hello-portalen skapar en ny anslutningstillgång med hello följande exempelkommandon.  

    $ConnectionAssetName = "AzureRunAsConnection"
    $ConnectionFieldValues = @{"ApplicationId" = $Application.ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Cert.Thumbprint; "SubscriptionId" = $SubscriptionId}
    New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureServicePrincipal -ConnectionFieldValues $ConnectionFieldValues 

Du är kan toouse hello skriptet toocreate hello anslutningstillgång eftersom när du skapar ditt Automation-konto kan den automatiskt inkluderar flera globala moduler som standard tillsammans med hello anslutningstypen **AzurServicePrincipal**toocreate hello **AzureRunAsConnection** anslutningstillgång.  Detta är viktigt tookeep i åtanke, eftersom om du försöker toocreate en ny anslutning tillgången tooconnect tooa tjänst eller ett program med en annan autentiseringsmetod det misslyckas eftersom hello-anslutningstypen inte redan har definierats i Automation-kontot.  Mer information om hur toocreate egna anslutningen skriver för anpassad eller modul från hello [PowerShell-galleriet](https://www.powershellgallery.com), se [integreringsmoduler](automation-integration-modules.md)
  
## <a name="using-a-connection-in-a-runbook-or-dsc-configuration"></a>Med hjälp av en anslutning i en runbook eller DSC-konfiguration

Du kan hämta en anslutning i en runbook eller DSC-konfigurationen med hello **Get-AutomationConnection** cmdlet.  Du kan inte använda hello [Get-AzureRmAutomationConnection](https://docs.microsoft.com/powershell/resourcemanager/azurerm.automation/v1.0.12/Get-AzureRmAutomationConnection?redirectedfrom=msdn) aktivitet.  Den här aktiviteten hämtar hello värden för hello olika fält i hello-anslutningen och returnerar dem som en [hash-tabell](http://go.microsoft.com/fwlink/?LinkID=324844) som sedan kan användas med hello lämpliga kommandon i hello runbook eller DSC-konfigurationen.

### <a name="textual-runbook-sample"></a>Text-runbook-exempel

hello följande exempelkommandon visar hur toouse hello kör som-kontot tidigare nämnts tooauthenticate med Azure Resource Manager-resurser i din runbook.  Den använder hello anslutning tillgång som representerar hello kör som-konto som refererar till hello certifikatbaserad tjänstens huvudnamn inte autentiseringsuppgifter.  

    $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint 

### <a name="graphical-runbook-samples"></a>Grafiska runbook-exempel

Du lägger till en **Get-AutomationConnection** aktiviteten tooa grafisk runbook genom att högerklicka på hello anslutning hello biblioteket i fönstret hello grafiska redigerare och välja **lägga till toocanvas**.

![](media/automation-connections/connection-add-canvas.png)

hello visar följande bild ett exempel på hur du använder en anslutning i en grafisk runbook.  Detta är hello samma exemplet ovan för att autentisera med hjälp av hello-kör som-konto med text-runbook.  Det här exemplet används hello **konstantvärde** datauppsättning för hello **hämta RunAs-anslutning** aktivitet som använder ett anslutningsobjekt för autentisering.  En [pipelinelänk](automation-graphical-authoring-intro.md#links-and-workflow) används här eftersom hello ServicePrincipalCertificate parameteruppsättning förväntar sig ett enskilt objekt.

![](media/automation-connections/automation-get-connection-object.png)

## <a name="next-steps"></a>Nästa steg

- Granska [länkar i grafiska redigering](automation-graphical-authoring-intro.md#links-and-workflow) toounderstand hur toodirect och kontroll hello flöda för logiken i dina runbooks.  

- toolearn mer om Azure Automation användning av PowerShell-moduler och bästa praxis för att skapa egna toowork för PowerShell-moduler som integreringsmoduler i Azure Automation finns [integreringsmoduler](automation-integration-modules.md).  
