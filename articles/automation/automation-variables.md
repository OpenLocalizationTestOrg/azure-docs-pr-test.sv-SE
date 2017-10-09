---
title: "aaaVariable tillgångar i Azure Automation | Microsoft Docs"
description: "Variabeln tillgångar är värden som är tillgängliga tooall runbooks och i Azure Automation DSC-konfigurationer.  Den här artikeln förklarar hello information om variabler och hur toowork med dem i både text och grafiska redigering."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: b880c15f-46f5-4881-8e98-e034cc5a66ec
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/09/2017
ms.author: magoedte;bwren
ms.openlocfilehash: f9daa49fc1dc883ffb218a9adf26e36df1d6bb27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="variable-assets-in-azure-automation"></a>Variabeln tillgångar i Azure Automation

Variabeln tillgångar är värden som är tillgängliga tooall runbooks och DSC-konfigurationer i automation-kontot. De kan skapas, ändras och hämtas från hello Azure-portalen Windows PowerShell och inifrån en runbook eller DSC-konfigurationen. Automationsvariabler är användbara för hello följande scenarier:

- Dela ett värde mellan flera runbooks eller DSC-konfigurationer.

- Dela ett värde mellan flera jobb från hello samma runbook eller DSC-konfigurationen.

- Hantera ett värde från hello-portalen eller hello Windows PowerShell-kommandoraden som används av runbooks eller DSC-konfigurationer, till exempel en uppsättning gemensamma konfigurationsobjekt som specifik lista av VM-namn, en specifik resursgrupp, en AD-domännamn och så vidare.  

Automationsvariabler är beständiga så att de fortsätter toobe tillgängliga även om hello runbook eller DSC-konfigurationen misslyckas.  Detta kan även ett värde toobe som angetts av en runbook som sedan används av en annan eller används av hello samma runbook eller DSC-konfiguration hello nästa gång den körs.

När en variabel har skapats kan du ange att den lagras krypterade.  När en variabel krypteras, lagras den säkert i Azure Automation och dess värde kan inte hämtas från hello [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx) cmdlet som levereras som en del av hello Azure PowerShell-modulen.  hello enda sättet att ett krypterat värde kan hämtas är från hello **Get-automationvariable,** aktivitet i en runbook eller DSC-konfigurationen.

> [!NOTE]
> Säkra tillgångar i Azure Automation inkluderar autentiseringsuppgifter, certifikat, anslutningar och krypterade variabler. Dessa tillgångar krypteras och lagras i hello Azure Automation med en unik nyckel som skapas för varje automation-konto. Den här nyckeln är krypterad med ett certifikat för master och lagras i Azure Automation. Innan de lagras en säker tillgång hello nyckel för hello automation-konto dekrypteras med hello master certifikatet och sedan används tooencrypt hello tillgången.

## <a name="variable-types"></a>Datatyper

När du skapar en variabel med hello Azure-portalen, måste du ange en datatyp från listrutan hello så hello portal kan visa hello lämplig kontroll för att ange hello variabelvärdet. hello variabel är inte begränsade toothis data typ, men du måste ange hello variabel med Windows PowerShell om du vill toospecify ett värde av en annan typ. Om du anger **inte har definierats**, och sedan hello hello variabels värdet för**$null**, och du måste ange hello-värde med hello [Set AzureAutomationVariable](http://msdn.microsoft.com/library/dn913767.aspx) cmdlet eller **Set-automationvariable,** aktivitet.  Du kan inte skapa eller ändra hello värdet för en variabel för komplex typ i hello-portalen, men du kan ange ett värde av valfri typ med Windows PowerShell. Komplexa typer returneras som en [PSCustomObject](http://msdn.microsoft.com/library/system.management.automation.pscustomobject.aspx).

Du kan lagra flera värden tooa enda variabel genom att skapa en matris eller hash-tabell och spara den toohello variabeln.

hello nedan följer en lista över variabla typer som är tillgängliga i Automation:

* Sträng
* Integer
* Datum och tid
* Booleskt värde
* Null

## <a name="cmdlets-and-workflow-activities"></a>Aktiviteter-cmdlets och arbetsflöde

hello-cmdlets i följande tabell hello är används toocreate och hantera automatisering variabler med Windows PowerShell. De levereras som en del av hello [Azure PowerShell-modulen](../powershell-install-configure.md) som är tillgänglig för användning i Automation-runbooks och DSC-konfigurationen.

|Cmdlet: ar|Beskrivning|
|:---|:---|
|[Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx)|Hämtar hello-värdet för en befintlig variabel.|
|[Ny AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603613.aspx)|Skapar en ny variabel och anger dess värde.|
|[Ta bort AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt619354.aspx)|Tar bort en befintlig variabel.|
|[Ange AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603601.aspx)|Anger hello-värde för en befintlig variabel.|

hello-arbetsflödesaktiviteter i hello i den följande tabellen finns används tooaccess automationsvariabler i en runbook. De är bara tillgängliga för användning i en runbook eller DSC-konfigurationen och levereras inte som en del av hello Azure PowerShell-modulen.

|Arbetsflödesaktiviteter|Beskrivning|
|:---|:---|
|Get-automationvariable|Hämtar hello-värdet för en befintlig variabel.|
|Set-automationvariable|Anger hello-värde för en befintlig variabel.|

> [!NOTE] 
> Du bör undvika att använda variabler i hello – Name-parametern i **Get-automationvariable,** i en runbook eller DSC-konfigurationen eftersom detta kan göra det svårare att hitta beroenden mellan runbooks eller DSC-konfiguration och automatisering variabler i designläge.

## <a name="creating-a-new-automation-variable"></a>Skapa en ny Automation-variabel

### <a name="toocreate-a-new-variable-with-hello-azure-portal"></a>toocreate en ny variabel med hello Azure-portalen

1. Från ditt Automation-konto klickar du på hello **tillgångar** panelen och klicka sedan på hello **tillgångar** bladet väljer **variabler**.
2. På hello **variabler** panelen, väljer **lägga till en variabel**.
3. Slutföra hello alternativ på hello **ny variabel** bladet och klicka på **skapa** spara hello ny variabel.

### <a name="toocreate-a-new-variable-with-windows-powershell"></a>toocreate en ny variabel med Windows PowerShell

Hej [ny AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603613.aspx) cmdlet skapar en ny variabel och anger sitt ursprungliga värde. Du kan hämta hello värde med hjälp av [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx). Om hello-värdet är en enkel typ, returneras samma typ.. Om det är en komplex typ och sedan en **PSCustomObject** returneras.

hello följande exempel kommandon visar hur toocreate en variabel av typen string och returnerar sedan värdet.

    New-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" 
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' `
    –Encrypted $false –Value 'My String'
    $string = (Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value

hello följande exempel kommandon visar hur toocreate en variabel med en komplex och returnerar sedan dess egenskaper. I det här fallet en virtuell dator objekt från **Get-AzureRmVm** används.

    $vm = Get-AzureRmVm -ResourceGroupName "ResourceGroup01" –Name "VM01"
    New-AzureRmAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm
    
    $vmValue = (Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
    $vmName = $vmValue.Name
    $vmIpAddress = $vmValue.IpAddress



## <a name="using-a-variable-in-a-runbook-or-dsc-configuration"></a>Använda en variabel i en runbook eller DSC-konfiguration

Använd hello **Set-automationvariable,** aktiviteten tooset hello värdet för en Automation-variabel i en runbook eller DSC-konfigurationen och hello **Get-automationvariable,** tooretrieve den.  Du bör inte använda hello **Set AzureAutomationVariable** eller **Get-AzureAutomationVariable** cmdletarna i en runbook eller DSC-konfigurationen eftersom de är mindre effektivt än hello arbetsflödesaktiviteter.  Du kan också hämta hello värdet för säker variabler med **Get-AzureAutomationVariable**.  Hej endast sätt toocreate en ny variabel från inifrån en runbook eller DSC-konfigurationen är toouse hello [ny AzureAutomationVariable](http://msdn.microsoft.com/library/dn913771.aspx) cmdlet.


### <a name="textual-runbook-samples"></a>Textrepresentation runbook-exempel

#### <a name="setting-and-retrieving-a-simple-value-from-a-variable"></a>Ställa in och hämta ett enkelt värde från en variabel

hello följande exempel kommandon visar hur tooset och hämtar en variabel i text-runbook. I det här exemplet förutsätts att variabler av typen heltal med namnet *NumberOfIterations* och *NumberOfRunnings* och en variabel av typen sträng med namnet *SampleMessage* redan har skapats.

    $NumberOfIterations = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfIterations'
    $NumberOfRunnings = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfRunnings'
    $SampleMessage = Get-AutomationVariable -Name 'SampleMessage'
    
    Write-Output "Runbook has been run $NumberOfRunnings times."
    
    for ($i = 1; $i -le $NumberOfIterations; $i++) {
       Write-Output "$i`: $SampleMessage"
    }
    Set-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" –Name NumberOfRunnings –Value ($NumberOfRunnings += 1)

#### <a name="setting-and-retrieving-a-complex-object-in-a-variable"></a>Ställa in och hämta ett komplext objekt i en variabel

hello följande exempel visas hur tooupdate en variabel med ett komplext värde i text-runbook. I det här exemplet hämtas en virtuell Azure-dator med **Get-AzureVM** och sparade tooan befintlig Automation-variabel.  Enligt beskrivningen i [datatyper](#variable-types), detta lagras som en PSCustomObject.

    $vm = Get-AzureVM -ServiceName "MyVM" -Name "MyVM"
    Set-AutomationVariable -Name "MyComplexVariable" -Value $vm


I följande kod hello, hämtas hello värdet från hello variabel och använda toostart hello virtuell dator.

    $vmObject = Get-AutomationVariable -Name "MyComplexVariable"
    if ($vmObject.PowerState -eq 'Stopped') {
       Start-AzureVM -ServiceName $vmObject.ServiceName -Name $vmObject.Name
    }


#### <a name="setting-and-retrieving-a-collection-in-a-variable"></a>Ställa in och hämta en samling i en variabel

hello följande exempel visas hur toouse en variabel med en samling med komplexa värden i text-runbook. I det här exemplet hämtas flera virtuella Azure-datorer med **Get-AzureVM** och sparade tooan befintlig Automation-variabel.  Enligt beskrivningen i [datatyper](#variable-types), lagras detta som en samling PSCustomObjects.

    $vms = Get-AzureVM | Where -FilterScript {$_.Name -match "my"}     
    Set-AutomationVariable -Name 'MyComplexVariable' -Value $vms

I följande kod hello, hello samlingen hämtas från hello variabeln och används toostart för varje virtuell dator.

    $vmValues = Get-AutomationVariable -Name "MyComplexVariable"
    ForEach ($vmValue in $vmValues)
    {
       if ($vmValue.PowerState -eq 'Stopped') {
          Start-AzureVM -ServiceName $vmValue.ServiceName -Name $vmValue.Name
       }
    }


### <a name="graphical-runbook-samples"></a>Grafiska runbook-exempel

Lägg till hello i en grafisk runbook **Get-automationvariable,** eller **Set-automationvariable,** genom att högerklicka på hello variabeln i hello biblioteket rutan hello grafiska redigerare och välja hello aktivitet som du vill använda.

![Lägg till variabel toocanvas](media/automation-variables/runbook-variable-add-canvas.png)

#### <a name="setting-values-in-a-variable"></a>Ange värden i en variabel
hello följande bild visar exempel aktiviteter tooupdate en variabel med ett enkelt värde i en grafisk runbook. I det här exemplet hämtas en enda virtuell Azure-dator med **Get-AzureRmVM** och hello datornamn sparas tooan befintlig Automation-variabel av typen sträng.  Det spelar ingen roll om hello [länken är en pipeline eller sekvens](automation-graphical-authoring-intro.md#links-and-workflow) eftersom vi räknar endast ett objekt i hello utdata.

![Ange enkla variabel](media/automation-variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a>Nästa steg

* toolearn mer information om hur du ansluter aktiviteter tillsammans i grafiska redigering finns [länkar i grafiska redigering](automation-graphical-authoring-intro.md#links-and-workflow)
* tooget igång med grafiska runbooks finns [min första grafisk runbook](automation-first-runbook-graphical.md) 

