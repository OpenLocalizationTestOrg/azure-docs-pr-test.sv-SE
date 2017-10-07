---
title: "aaaIntegrate loggar från Azure Key Vault med Händelsehubbar | Microsoft Docs"
description: "Kursen som tillhandahåller hello nödvändiga åtgärder toomake Key Vault loggar tillgängliga tooa SIEM med hjälp av Azure Log-integrering"
services: security
author: barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.topic: article
ms.date: 08/07/2017
ms.author: Barclayn
ms.custom: AzLog
ms.openlocfilehash: ada2fc846cc6bf09e12cc2c016815b27afef0d50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-log-integration-tutorial-process-azure-key-vault-events-by-using-event-hubs"></a>Azure Log Integration Självstudier: processen Azure Key Vault-händelser med hjälp av Händelsehubbar

Du kan använda Azure Log-integrering tooretrieve loggas händelser och göra dem tillgängliga tooyour information och händelse (SIEM) hanteringssystem för informationssäkerhet. Den här kursen visar ett exempel på hur Azure Log-integrering kan vara används tooprocess loggar som skaffas genom Azure Event Hubs.
 
Använd den här självstudiekursen tooget bekanta dig med hur hello Azure Log-integrering och Händelsehubbar fungerar tillsammans med följande exempel steg och förstå hur varje steg stöder hello lösning. Sedan kan du dra vad du har lärt dig här toocreate egna steg toosupport ditt företags unika krav.

>[!WARNING]
hello steg och kommandon i den här självstudiekursen är inte avsedd toobe kopieras och klistras in. Det är bara exempel. Använd inte hello PowerShell-kommandon ”i befintligt skick” i live-miljön. Du måste anpassa dem utifrån din miljö.


Den här självstudiekursen vägleder dig genom hello av tar Azure Key Vault aktivitet loggade tooan händelsehubb och göra den tillgänglig som JSON-filer tooyour SIEM-system. Du kan sedan konfigurera SIEM tooprocess hello JSON systemfiler.

>[!NOTE]
>De flesta av hello stegen i den här kursen omfattar konfigurera nyckelvalv, lagringskonton och händelsehubbar. hello specifika Azure Log integreringen är hello slutet av den här kursen. Utför inte de här stegen i en produktionsmiljö. De är avsedda för en labbmiljö. Du måste anpassa hello steg innan du använder dem i produktion.

Informationen längs hello sätt hjälper till att du förstår hello orsaker bakom varje steg. Länkar tooother artiklar får du mer information på vissa avsnitt.

Mer information om hello-tjänster som nämns i den här kursen finns: 

- [Azure Key Vault](../key-vault/key-vault-whatis.md)
- [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Azure logganalys-integrering](security-azure-log-integration-overview.md)


## <a name="initial-setup"></a>Installationen

Innan du kan slutföra hello stegen i den här artikeln, måste du hello följande:

1. En Azure-prenumeration och konto i den prenumerationen med administratörsrättigheter. Om du inte har en prenumeration kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/).
 
2. Ett system med åtkomst toohello internet som uppfyller hello för att installera Azure Log-integrering. hello system kan vara på en molnbaserad tjänst eller lagras lokalt.

3. [Azure Log-integrering](https://www.microsoft.com/download/details.aspx?id=53324) installerad. tooinstall den:

   a. Använd Fjärrskrivbord tooconnect toohello system som anges i steg 2.   
   b. Kopiera hello Azure Log-integrering installer toohello system. Du kan [ladda ned installationsfilerna för hello](https://www.microsoft.com/download/details.aspx?id=53324).   
   c. Starta hello installer och acceptera licensvillkoren för hello.   
   d. Om du ger telemetri information, lämna hello kryssrutan är markerad. Om du inte skickar hellre användning information tooMicrosoft, avmarkerar du kryssrutan för hello.
   
   Mer information om Azure Log-integrering och hur tooinstall, se [Azure Log integrering med Azure diagnostikloggning och vidarebefordran av Windows-händelse](security-azure-log-integration-get-started.md).

4. hello senaste PowerShell-version.
 
   Om du har Windows Server 2016 installerad, så att du har minst PowerShell 5.0. Om du använder någon annan version av Windows Server, kanske en tidigare version av PowerShell som är installerad. Du kan kontrollera hello version genom att ange ```get-host``` i PowerShell-fönstret. Om du inte har PowerShell 5.0 installerat, kan du [ladda ned den](https://www.microsoft.com/download/details.aspx?id=50395).

   När du har minst PowerShell 5.0, kan du fortsätta tooinstall hello senaste versionen:
   
   a. Ange i ett PowerShell-fönster hello ```Install-Module Azure``` kommando. Slutför hello installationssteg.    
   b. Ange hello ```Install-Module AzureRM``` kommando. Slutför hello installationssteg.

   Mer information finns i [installera Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).


## <a name="create-supporting-infrastructure-elements"></a>Skapa stödjande infrastrukturelement

1. Öppna ett upphöjt PowerShell-fönster och gå för**C:\Program Files\Microsoft Azure Log integrering**.
2. Importera hello AzLog cmdlets genom att köra skript hello LoadAzLogModule.ps1. Ange hello `.\LoadAzLogModule.ps1` kommando. (Observera hello ”. \” i det aktuella kommandot.) Du bör se något liknande följande:</br>

   ![Lista med inlästa moduler](./media/security-azure-log-integration-keyvault-eventhub/loaded-modules.png)

3. Ange hello `Login-AzureRmAccount` kommando. Ange hello autentiseringsuppgifter för hello-prenumeration som du ska använda för den här kursen i hello inloggningen fönster.

   >[!NOTE]
   >Om detta är hello första gången du loggar i tooAzure från den här datorn, visas ett meddelande om att tillåta att Microsoft toocollect PowerShell användningsdata. Vi rekommenderar att du aktiverar den här Datasamlingen eftersom den kommer att använda tooimprove Azure PowerShell.

4. Du är inloggad och du ser hello informationen i följande skärmbild hello efter en lyckad autentisering. Anteckna hello-ID och din prenumeration prenumerationsnamn, eftersom du behöver dem toocomplete steg senare.

   ![PowerShell-fönster](./media/security-azure-log-integration-keyvault-eventhub/login-azurermaccount.png)
5. Skapa variabler toostore värden som kommer att användas senare. Ange var och en av följande PowerShell raderna hello. Du kan behöva tooadjust hello värden toomatch din miljö.
    - ```$subscriptionName = ‘Visual Studio Ultimate with MSDN’```(Namnet på din prenumeration kan vara olika. Du kan se det som en del av hello utdata från hello föregående kommandot.)
    - ```$location = 'West US'```(Den här variabeln kommer att använda toopass hello plats där resurser ska skapas. Du kan ändra den här variabeln toobe valfri plats som du väljer.)
    - ```$random = Get-Random```
    - ``` $name = 'azlogtest' + $random```(hello namn kan vara allt, men den bör innehålla endast små bokstäver och siffror.)
    - ``` $storageName = $name```(Den här variabeln används för hello lagringskontonamnet.)
    - ```$rgname = $name ```(Den här variabeln används för hello resursgruppens namn.)
    - ``` $eventHubNameSpaceName = $name```(Detta är hello namnet på hello event hub namnområde.)
6. Ange hello-prenumeration som du kommer att arbeta med:
    
    ```Select-AzureRmSubscription -SubscriptionName $subscriptionName```
7. Skapa en resursgrupp:
    
    ```$rg = New-AzureRmResourceGroup -Name $rgname -Location $location```
    
   Om du anger `$rg` du bör nu se utdata liknande toothis skärmbild:

   ![Utdata när den har skapats av en resursgrupp](./media/security-azure-log-integration-keyvault-eventhub/create-rg.png)
8. Skapa ett lagringskonto som kommer att använda tookeep reda på statusinformation:
    
    ```$storage = New-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename -Location $location -SkuName Standard_LRS```
9. Skapa hello event hub namnområde. Detta är obligatorisk toocreate en händelsehubb.
    
    ```$eventHubNameSpace = New-AzureRmEventHubNamespace -ResourceGroupName $rgname -NamespaceName $eventHubnamespaceName -Location $location```
10. Hämta hello regel-ID som ska användas med hello insikter providern:
    
    ```$sbruleid = $eventHubNameSpace.Id +'/authorizationrules/RootManageSharedAccessKey' ```
11. Hämta alla möjliga Azure platser och lägga till hello namn tooa variabel som kan användas i ett senare steg:
    
    a. ```$locationObjects = Get-AzureRMLocation```    
    b. ```$locations = @('global') + $locationobjects.location```
    
    Om du anger `$locations` nu du se hello platsnamn utan hello ytterligare information som returneras av Get-AzureRmLocation.
12. Skapa en profil för Azure Resource Manager-loggen: 
    
    ```Add-AzureRmLogProfile -Name $name -ServiceBusRuleId $sbruleid -Locations $locations```
    
    Mer information om hello Azure logganalys-profilen finns [översikt över hello Azure-aktivitetsloggen](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

> [!NOTE]
> Du kan få ett felmeddelande när du försöker toocreate en logg-profil. Sedan kan du granska hello-dokumentation för Get-AzureRmLogProfile och ta bort AzureRmLogProfile. Om du kör Get-AzureRmLogProfile finns information om hello log-profilen. Du kan ta bort hello befintlig logg profil genom att ange hello ```Remove-AzureRmLogProfile -name 'Log Profile Name' ``` kommando.
>
>![Fel för Resource Manager-profil](./media/security-azure-log-integration-keyvault-eventhub/rm-profile-error.png)

## <a name="create-a-key-vault"></a>Skapa ett nyckelvalv

1. Skapa hello nyckelvalv:

   ```$kv = New-AzureRmKeyVault -VaultName $name -ResourceGroupName $rgname -Location $location ```

2. Konfigurera loggning för hello nyckelvalv:

   ```Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -ServiceBusRuleId $sbruleid -Enabled $true ```

## <a name="generate-log-activity"></a>Generera loggaktivitet

Förfrågningar måste toobe skickas tooKey valvet toogenerate loggaktivitet. Åtgärder som nyckelgenerering, lagring av hemligheter, eller läsa hemligheter från Nyckelvalvet skapas loggposter.

1. Visar hello aktuella lagringsnycklar:
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
2. Generera en ny **key2**:
    
   ```New-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname -KeyName key2```
3. Visa hello nycklar igen och se som **key2** innehåller ett annat värde:
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
4. Ange och läsa hemliga toogenerate ytterligare loggposter:
    
   a. ```Set-AzureKeyVaultSecret -VaultName $name -Name TestSecret -SecretValue (ConvertTo-SecureString -String 'Hi There!' -AsPlainText -Force)``` b. ```(Get-AzureKeyVaultSecret -VaultName $name -Name TestSecret).SecretValueText```

   ![Returnerade hemliga](./media/security-azure-log-integration-keyvault-eventhub/keyvaultsecret.png)


## <a name="configure-azure-log-integration"></a>Konfigurera Azure logganalys-integrering

Nu när du har konfigurerat alla händelsehubb för hello obligatoriska element toohave Key Vault-loggning tooan, behöver du tooconfigure Azure Log-integrering:

1. ```$storage = Get-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename```
2. ```$eventHubKey = Get-AzureRmEventHubNamespaceKey -ResourceGroupName $rgname -NamespaceName $eventHubNamespace.name -AuthorizationRuleName RootManageSharedAccessKey```
3. ```$storagekeys = Get-AzureRmStorageAccountKey -ResourceGroupName $rgname -Name $storagename```
4. ``` $storagekey = $storagekeys[0].Value```

Kör hello AzLog kommando för varje händelsehubb:

1. ```$eventhubs = Get-AzureRmEventHub -ResourceGroupName $rgname -NamespaceName $eventHubNamespaceName```
2. ```$eventhubs.Name | %{Add-AzLogEventSource -Name $sub' - '$_ -StorageAccount $storage.StorageAccountName -StorageKey $storageKey -EventHubConnectionString $eventHubKey.PrimaryConnectionString -EventHubName $_}```

Du bör se JSON-filer som skapas när en minut eller så körs hello senast två kommandon. Du kan bekräfta att genom att övervaka hello directory **C:\users\AzLog\EventHubJson**.

## <a name="next-steps"></a>Nästa steg

- [Azure logganalys Integration vanliga frågor och svar](security-azure-log-integration-faq.md)
- [Kom igång med Azure Log-integrering](security-azure-log-integration-get-started.md)
- [Integrera loggar från Azure-resurser i din SIEM-system](security-azure-log-integration-overview.md)
