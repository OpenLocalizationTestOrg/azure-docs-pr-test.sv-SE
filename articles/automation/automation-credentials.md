---
title: "aaaCredential tillgångar i Azure Automation | Microsoft Docs"
description: "Inloggningstillgångar i Azure Automation innehålla säkerhetsreferenser som kan vara används tooauthenticate tooresources nås av hello runbook eller DSC-konfigurationen. Den här artikeln beskrivs hur toocreate autentiseringsuppgifter tillgångar och använda dem i en runbook eller DSC-konfigurationen."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 3209bf73-c208-425e-82b6-df49860546dd
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/14/2017
ms.author: bwren
ms.openlocfilehash: 46f23a8f79d5863265af9cf84f6003e30f8e7d39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="credential-assets-in-azure-automation"></a>Inloggningstillgångar i Azure Automation
Ett Automation-autentiseringsuppgiftstillgång innehåller en [PSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential) objekt som innehåller säkerhetsreferenser, till exempel användarnamn och lösenord. Runbooks och DSC-konfigurationer kan använda cmdlet: ar som accepterar ett PSCredential-objekt för autentisering eller de kan extrahera hello användarnamn och lösenord för hello PSCredential-objekt tooprovide toosome program eller tjänster som kräver autentisering. hello egenskaperna för en autentiseringsuppgift lagras säkert i Azure Automation och kan nås i hello runbook eller DSC-konfigurationen med hello [Get-AutomationPSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential.aspx) aktivitet.

> [!NOTE]
> Säkra tillgångar i Azure Automation inkluderar autentiseringsuppgifter, certifikat, anslutningar och krypterade variabler. Dessa tillgångar krypteras och lagras i hello Azure Automation med en unik nyckel som skapas för varje automation-konto. Den här nyckeln är krypterad med ett certifikat för master och lagras i Azure Automation. Innan de lagras en säker tillgång hello nyckel för hello automation-konto dekrypteras med hello master certifikatet och sedan används tooencrypt hello tillgången.  

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell-cmdlets
hello-cmdlets i följande tabell hello är används toocreate och hantera automatisering inloggningstillgångar med Windows PowerShell.  De levereras som en del av hello [Azure PowerShell-modulen](/powershell/azure/overview) som är tillgänglig för användning i Automation-runbooks och DSC-konfigurationer.

| Cmdlet: ar | Beskrivning |
|:--- |:--- |
| [Get-AzureAutomationCredential](/powershell/module/azure/get-azureautomationcredential?view=azuresmps-3.7.0) |Hämtar information om en autentiseringstillgång. Du kan bara hämta hello autentiseringsuppgifter sig själv från **Get-AutomationPSCredential** aktivitet. |
| [Ny AzureAutomationCredential](/powershell/module/azure/new-azureautomationcredential?view=azuresmps-3.7.0) |Skapar en ny Automation-autentiseringsuppgifter. |
| [Ta bort - AzureAutomationCredential](/powershell/module/azure/new-azureautomationcredential?view=azuresmps-3.7.0) |Tar bort autentiseringsuppgifter för Automation. |
| [Set - AzureAutomationCredential](/powershell/module/azure/new-azureautomationcredential?view=azuresmps-3.7.0) |Anger hello egenskaper för ett befintligt Automation-autentiseringsuppgifter. |

## <a name="runbook-activities"></a>Runbook-aktiviteter
hello aktiviteter i den följande tabellen hello är används tooaccess autentiseringsuppgifter i en runbook och DSC-konfigurationer.

| Aktiviteter | Beskrivning |
|:--- |:--- |
| Get-AutomationPSCredential |Hämtar en autentiseringsuppgift toouse i en runbook eller DSC-konfigurationen. Returnerar en [System.Management.Automation.PSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential) objekt. |

> [!NOTE]
> Du bör undvika att använda variabler i hello – Name-parametern i Get-AutomationPSCredential eftersom detta kan göra det svårare att hitta beroenden mellan runbooks eller DSC-konfigurationer och autentiseringsuppgifter tillgångar i designläge.
> 
> 

## <a name="creating-a-new-credential-asset"></a>Skapa en ny autentiseringstillgång

### <a name="toocreate-a-new-credential-asset-with-hello-azure-portal"></a>toocreate en ny autentiseringstillgång med hello Azure-portalen
1. Från ditt automation-konto klickar du på hello **tillgångar** del tooopen hello **tillgångar** bladet.
2. Klicka på hello **autentiseringsuppgifter** del tooopen hello **autentiseringsuppgifter** bladet.
3. Klicka på **lägga till en autentiseringsuppgift** hello överst i hello-bladet.
4. Slutför hello formuläret och klickar på **skapa** toosave hello nya autentiseringsuppgifter.

### <a name="toocreate-a-new-credential-asset-with-windows-powershell"></a>toocreate en ny autentiseringstillgång med Windows PowerShell
hello följande exempelkommandon visar hur toocreate en ny automation-autentiseringen. Ett PSCredential-objekt skapas med hello namn och lösenord och sedan används toocreate hello autentiseringsuppgiftstillgång. Du kan också använda hello **Get-Credential** cmdlet toobe uppmanas tootype i ett namn och lösenord.

    $user = "MyDomain\MyUser"
    $pw = ConvertTo-SecureString "PassWord!" -AsPlainText -Force
    $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $user, $pw
    New-AzureAutomationCredential -AutomationAccountName "MyAutomationAccount" -Name "MyCredential" -Value $cred

### <a name="toocreate-a-new-credential-asset-with-hello-azure-classic-portal"></a>toocreate en ny autentiseringstillgång med hello klassiska Azure-portalen
1. Från ditt automation-konto klickar du på **tillgångar** hello överst i fönstret hello.
2. Hello längst ned i hello-fönstret klickar du på **Lägg till inställning**.
3. Klicka på **Lägg till autentiseringsuppgift**.
4. I hello **autentiseringstypen** listrutan **PowerShell-autentiseringsuppgift**.
5. Slutför guiden hello och klicka på hello kryssrutan toosave hello nya autentiseringsuppgifter.

## <a name="using-a-powershell-credential"></a>Med hjälp av en PowerShell-autentiseringsuppgift
Du kan hämta en autentiseringstillgång i en runbook eller DSC-konfigurationen med hello **Get-AutomationPSCredential** aktivitet. Detta returnerar en [PSCredential-objekt](http://msdn.microsoft.com/library/system.management.automation.pscredential.aspx) som du kan använda med en aktivitet eller cmdlet som kräver en PSCredential-parameter. Du kan också hämta hello egenskaper för hello autentiseringsuppgifter objektet toouse individuellt. hello objekt har en egenskap för hello användarnamn och hello säkert lösenord eller använda hello **GetNetworkCredential** metoden tooreturn en [NetworkCredential](http://msdn.microsoft.com/library/system.net.networkcredential.aspx) objekt som kommer att ge ett oskyddat version av hello lösenord.

### <a name="textual-runbook-sample"></a>Text-runbook-exempel
hello följande exempelkommandon visar hur toouse en PowerShell autentiseringsuppgifter i en runbook. I det här exemplet hello autentiseringsuppgifter hämtas och tilldelade toovariables sitt användarnamn och lösenord.

    $myCredential = Get-AutomationPSCredential -Name 'MyCredential'
    $userName = $myCredential.UserName
    $securePassword = $myCredential.Password
    $password = $myCredential.GetNetworkCredential().Password


### <a name="graphical-runbook-sample"></a>Grafisk runbook-exempel
Du lägger till en **Get-AutomationPSCredential** aktiviteten tooa grafisk runbook genom att högerklicka på hello autentiseringsuppgifter i hello biblioteket rutan hello grafiska redigerare och välja **lägga till toocanvas**.

![Lägg till autentiseringsuppgifter toocanvas](media/automation-credentials/credential-add-canvas.png)

hello visar följande bild ett exempel på hur du använder en autentiseringsuppgift i en grafisk runbook.  I det här fallet den håller på att använda tooprovide autentisering för en runbook tooAzure resurser som beskrivs i [autentisera Runbooks med Azure AD-användarkonto](automation-create-aduser-account.md).  hello första aktiviteten hämtar hello autentiseringsuppgift som inte har åtkomst toohello Azure-prenumeration.  Hej **Add-AzureAccount** aktiviteten använder sedan den här tooprovide behörighetsautentisering för alla aktiviteter som kommer efter den.  En [pipelinelänk](automation-graphical-authoring-intro.md#links-and-workflow) är här sedan **Get-AutomationPSCredential** förväntar sig ett enskilt objekt.  

![Lägg till autentiseringsuppgifter toocanvas](media/automation-credentials/get-credential.png)

## <a name="using-a-powershell-credential-in-dsc"></a>Med hjälp av en PowerShell-autentiseringsuppgift i DSC
När DSC-konfigurationer i Azure Automation kan referera till inloggningstillgångar med **Get-AutomationPSCredential**, inloggningstillgångar kan också överföras via parametrar, om så önskas. Mer information finns i [kompilering konfigurationer i Azure Automation DSC](automation-dsc-compile.md#credential-assets).

## <a name="next-steps"></a>Nästa steg
* toolearn mer om länkar i grafiska redigering finns [länkar i grafiska redigering](automation-graphical-authoring-intro.md#links-and-workflow)
* toounderstand hello olika autentiseringsmetoder med automatisering finns [Azure Automation-säkerhet](automation-security-overview.md)
* tooget igång med grafiska runbooks finns [min första grafisk runbook](automation-first-runbook-graphical.md)
* tooget igång med PowerShell arbetsflöde runbooks finns [min första PowerShell-arbetsflödesrunbook](automation-first-runbook-textual.md) 

