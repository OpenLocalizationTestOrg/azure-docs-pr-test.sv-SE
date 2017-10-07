---
title: "aaaCertificate tillgångar i Azure Automation | Microsoft Docs"
description: "Certifikat kan lagras på ett säkert sätt i Azure Automation så att de kan nås av runbooks eller DSC-konfigurationer tooauthenticate mot Azure och resurser från tredje part.  Den här artikeln förklarar hello information om certifikat och hur toowork med dem i både text och grafiska redigering."
services: automation
documentationcenter: 
author: mgoedtel
manager: stevenka
editor: tysonn
ms.assetid: ac9c22ae-501f-42b9-9543-ac841cf2cc36
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/19/2016
ms.author: magoedte;bwren
ms.openlocfilehash: 2c25bee937890438ea9022669be2c24c77a110b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="certificate-assets-in-azure-automation"></a>Certifikattillgångar i Azure Automation

Certifikat kan lagras på ett säkert sätt i Azure Automation så att de kan nås av runbooks eller DSC-konfigurationer med hjälp av hello **Get-AzureRmAutomationRmCertificate** aktivitet för Azure Resource Manager-resurser. Detta gör att du toocreate runbooks och DSC-konfigurationer som använder certifikat för autentisering eller lägger till dem tooAzure eller tredje parts resurser.

> [!NOTE] 
> Säkra tillgångar i Azure Automation inkluderar autentiseringsuppgifter, certifikat, anslutningar och krypterade variabler. Dessa tillgångar krypteras och lagras i hello Azure Automation med en unik nyckel som skapas för varje automation-konto. Den här nyckeln är krypterad med ett certifikat för master och lagras i Azure Automation. Innan de lagras en säker tillgång hello nyckel för hello automation-konto dekrypteras med hello master certifikatet och sedan används tooencrypt hello tillgången.
> 

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell-Cmdlets

hello-cmdlets i följande tabell hello är används toocreate och hantera automatisering certifikattillgångar med Windows PowerShell. De levereras som en del av hello [Azure PowerShell-modulen](../powershell-install-configure.md) som är tillgänglig för användning i Automation-runbooks och DSC-konfigurationer.

|Cmdlet: ar|Beskrivning|
|:---|:---|
|[Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx)|Hämtar information om ett certifikat toouse i en runbook eller DSC-konfigurationen. Du kan bara hämta hello certifikatet från Get-AutomationCertificate aktivitet.|
|[Ny AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603604.aspx)|Skapar ett nytt certifikat till Azure Automation.|
[Ta bort AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603529.aspx)|Tar bort ett certifikat från Azure Automation.|Skapar ett nytt certifikat till Azure Automation.
|[Ange AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603760.aspx)|Anger hello egenskaperna för ett befintligt certifikat, inklusive överföring hello certifikatfilen och inställning hello lösenordet för en PFX-fil.|
|[Lägg till AzureCertificate](https://msdn.microsoft.com/library/azure/dn495214.aspx)|Överföringar som ett tjänstcertifikat för hello angetts tjänst i molnet.|


## <a name="creating-a-new-certificate"></a>Skapa ett nytt certifikat

När du skapar ett nytt certifikat kan du ladda upp en CER- eller PFX-filen-tooAzure Automation. Om du markerar hello certifikat kan exporteras, kan du överföra den utanför hello Azure Automation-certifikatarkivet. Om du inte kan exporteras kan sedan den endast användas för signering inom hello runbook eller DSC-konfigurationen.


### <a name="toocreate-a-new-certificate-with-hello-azure-portal"></a>toocreate ett nytt certifikat med hello Azure-portalen

1. Från ditt Automation-konto klickar du på hello **tillgångar** panelen tooopen hello **tillgångar** bladet.
1. Klicka på hello **certifikat** panelen tooopen hello **certifikat** bladet.
1. Klicka på **lägga till ett certifikat** hello överst i hello-bladet.
2. Ange ett namn för hello certifikatet i hello **namn** rutan.
2. Klicka på **Välj en fil** under **överför en certifikatfil** toobrowse för en CER- eller PFX-fil.  Om du väljer en .pfx-fil, ange ett lösenord och om den ska tillåtas toobe exporteras.
1. Klicka på **skapa** toosave hello nya certifikat tillgången.


### <a name="toocreate-a-new-certificate-with-windows-powershell"></a>toocreate ett nytt certifikat med Windows PowerShell

hello exemplet nedan visar hur toocreate en ny Automation-certifikatet och markera det kan exporteras. Detta importerar en befintlig .pfx-fil.

    $certName = 'MyCertificate'
    $certPath = '.\MyCert.pfx'
    $certPwd = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
    $ResourceGroup = "ResourceGroup01"
    
    New-AzureRmAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certName -Path $certPath –Password $certPwd -Exportable -ResourceGroupName $ResourceGroup

## <a name="using-a-certificate"></a>Använder ett certifikat

Du måste använda hello **Get-AutomationCertificate** aktiviteten toouse ett certifikat. Du kan inte använda hello [Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx) cmdlet eftersom den returnerar information om hello certifikattillgång men inte hello själva certifikaten.

### <a name="textual-runbook-sample"></a>Text-runbook-exempel

hello följande exempelkod visar hur tooadd en certifikat-tooa Molntjänsten i en runbook. I det här exemplet hämtas hello lösenord från en krypterad automation-variabel.

    $serviceName = 'MyCloudService'
    $cert = Get-AutomationCertificate -Name 'MyCertificate'
    $certPwd = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
    Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert

### <a name="graphical-runbook-sample"></a>Grafisk runbook-exempel

Du lägger till en **Get-AutomationCertificate** tooa grafisk runbook genom att högerklicka på certifikatet hello hello biblioteket ruta hello grafiska redigerare och välja **lägga till toocanvas**.

![Lägg till certifikat toohello arbetsytan](media/automation-certificates/automation-certificate-add-to-canvas.png)

hello visar följande bild ett exempel på hur du använder ett certifikat i en grafisk runbook.  Detta är hello samma exemplet ovan för att lägga till en molntjänst för tooa av certifikat från en textrepresentation runbook.

![Exempel grafiska redigering ](media/automation-certificates/graphical-runbook-add-certificate.png)


## <a name="next-steps"></a>Nästa steg

- toolearn mer om hur du arbetar med länkar toocontrol hello logiska flödet av aktiviteter din runbook är utformad tooperform, se [länkar i grafiska redigering](automation-graphical-authoring-intro.md#links-and-workflow). 
