---
title: "aaaUse paket avbilda toodo proaktiv nätverk övervakning med aviseringar och Azure Functions | Microsoft Docs"
description: "Den här artikeln beskriver hur toocreate en avisering utlöses paketinsamling med Azure Nätverksbevakaren"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 75e6e7c4-b3ba-4173-8815-b00d7d824e11
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 4722a831f3a9d5537c0e6f53daba4dfc35d0cf24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-packet-capture-for-proactive-network-monitoring-with-alerts-and-azure-functions"></a>Använd paketinsamling för proaktiv nätverksövervakning med varningar och Azure Functions

Nätverket Watcher paketinsamling skapar avbilda sessioner tootrack trafik till och från virtuella datorer. hello filen kan ha ett filter som definieras tootrack hello endast trafik som du vill toomonitor. Dessa data lagras sedan i en lagringsblob-eller lokalt på hello gästdatorn.

Den här funktionen kan startas från en fjärrdator från andra automatiseringsscenarier, till exempel Azure Functions. Paketet avbilda ger du hello kapaciteten toorun proaktiv insamlingar baserat på definierad avvikelser i nätverket. Andra användningsområden omfattar att samla in nätverksstatistik för att hämta information om nätverket intrång och felsökning klient-/ serverkommunikation.

Resurser som distribueras i Azure kör 24/7. Du och din personal kan inte aktivt övervaka hello status för alla resurser 24/7. Till exempel vad händer om ett problem inträffar kl 2?

Genom att använda Nätverksbevakaren, aviseringar och funktioner från inom hello Azure-ekosystemet, kan du proaktivt svara med hello data och verktyg toosolve problem i nätverket.

![Scenario][scenario]

## <a name="prerequisites"></a>Krav

* hello senaste versionen av [Azure PowerShell](/powershell/azure/install-azurerm-ps).
* En befintlig instans av Nätverksbevakaren. Om du inte redan har en, [skapa en instans av Nätverksbevakaren](network-watcher-create.md).
* En befintlig virtuell dator i hello samma region som Nätverksbevakaren med hello [Windows tillägget](../virtual-machines/windows/extensions-nwa.md) eller [Linux-tillägg för virtuell dator](../virtual-machines/linux/extensions-nwa.md).

## <a name="scenario"></a>Scenario

Din virtuella dator skickar flera TCP-segment än vanligt i det här exemplet och du vill toobe aviserad om. TCP-segment som används som exempel här, men du kan använda alla aviseringstillståndet.

När du meddelas du tooreceive på paketnivå data toounderstand varför kommunikation har ökat. Du kan sedan vidta åtgärder tooreturn hello virtuella tooregular kommunikation.

Det här scenariot förutsätter att du har en befintlig instans av Nätverksbevakaren och en resursgrupp med en giltig virtuell dator.

hello följande lista är en översikt över hello arbetsflöde som äger rum:

1. En avisering utlöses på den virtuella datorn.
1. hello avisering anropar din Azure-funktion via en webhook.
1. Din Azure-funktion bearbetar hello aviseringen och startar en Nätverksbevakaren paket avbildningssessionen.
1. hello paketinsamling körs på hello VM och samlar in trafik.
1. hello paket filen har överförts tooa storage-konto för granskning och diagnos.

tooautomate den här processen kan vi skapa och ansluta en avisering på vår VM tootrigger när hello incident inträffar. Vi kan också skapa en funktion toocall i Nätverksbevakaren.

Det här scenariot hello följande:

* Skapar en Azure-funktion som startar en paketinsamling.
* Skapar en aviseringsregel på en virtuell dator och konfigurerar hello varningsregeln toocall hello Azure-funktion.

## <a name="create-an-azure-function"></a>Skapa en Azure-funktion

hello första steget är toocreate en Azure-funktionen tooprocess hello avisering och skapa en paketinsamling.

1. I hello [Azure-portalen](https://portal.azure.com)väljer **ny** > **Compute** > **Funktionsapp**.

    ![Skapa en funktionsapp][1-1]

2. På hello **Funktionsapp** bladet ange hello följande värden och markera sedan **OK** toocreate hello app:

    |**Inställning** | **Värde** | **Detaljer** |
    |---|---|---|
    |**Appens namn**|PacketCaptureExample|hello namnet på hello funktionsapp.|
    |**Prenumeration**|[Din prenumeration] hello prenumerationen för vilken toocreate hello funktionsapp.||
    |**Resursgrupp**|PacketCaptureRG|hello resurs grupp toocontain hello funktionsapp.|
    |**Värd för planen**|Planera för användning| hello typ av planera din app använder för funktionen. Alternativen är förbrukning eller Azure App Service-plan. |
    |**Plats**|Centrala USA| hello region i vilken toocreate hello funktionsapp.|
    |**Storage-konto**|{namn} automatiskt| Hej lagringskonto som Azure Functions måste för allmänna lagring.|

3. På hello **PacketCaptureExample funktionen appar** bladet väljer **funktioner** > **anpassad funktionen**  >  **+**.

4. Välj **HttpTrigger Powershell**, och ange sedan hello återstående information. Slutligen toocreate hello funktion, Välj **skapa**.

    |**Inställning** | **Värde** | **Detaljer** |
    |---|---|---|
    |**Scenario**|experiment|Typen av scenario|
    |**Namnge din funktion**|AlertPacketCapturePowerShell|Namnet på hello-funktion|
    |**Åtkomstnivå**|Funktionen|Åtkomstnivå för hello-funktion|

![Exempel på funktioner][functions1]

> [!NOTE]
> hello PowerShell mallen är experiment och har inte fullständigt stöd.

Anpassningar som krävs för det här exemplet och beskrivs i följande steg hello.

### <a name="add-modules"></a>Lägg till moduler

toouse nätverk Watcher PowerShell-cmdletarna överför hello senaste PowerShell-modulen toohello funktionsapp.

1. Kör följande PowerShell-kommando hello på den lokala datorn med hello senaste Azure PowerShell-moduler installeras:

    ```powershell
    (Get-Module AzureRM.Network).Path
    ```

    Det här exemplet ger du hello lokala sökvägen för dina Azure PowerShell-moduler. Dessa mappar som används i ett senare steg. hello-moduler som används i det här scenariot är:

    * AzureRM.Network

    * AzureRM.Profile

    * AzureRM.Resources

    ![PowerShell-mappar][functions5]

1. Välj **fungerar appinställningar** > **gå tooApp Service Editor**.

    ![Funktionen app-inställningar][functions2]

1. Högerklicka på hello **AlertPacketCapturePowershell** mapp, och sedan skapa en mapp med namnet **azuremodules**. 

4. Skapa en undermapp för varje modul som du behöver.

    ![Mappen och undermappar][functions3]

    * AzureRM.Network

    * AzureRM.Profile

    * AzureRM.Resources

1. Högerklicka på hello **AzureRM.Network** undermapp och välj sedan **Överför filer**. 

6. Gå tooyour Azure moduler. I hello lokala **AzureRM.Network** mapp, markera alla hello-filer i mappen hello. Välj sedan **OK**. 

7. Upprepa dessa steg för **AzureRM.Profile** och **AzureRM.Resources**.

    ![Överföra filer][functions6]

1. När du är klar, var mappen bör ha hello PowerShell-modulen filer från din lokala dator.

    ![PowerShell-filer][functions7]

### <a name="authentication"></a>Autentisering

toouse hello PowerShell-cmdletar som du måste autentisera. Du kan konfigurera autentisering i hello funktionsapp. tooconfigure autentisering måste du konfigurera miljövariabler och överföra en krypterad nyckelfilen toohello funktionsapp.

> [!NOTE]
> Det här scenariot innehåller bara ett exempel på hur tooimplement autentisering med Azure Functions. Det finns andra sätt toodo detta.

#### <a name="encrypted-credentials"></a>Krypterade autentiseringsuppgifter

hello följande PowerShell-skript skapar en nyckelfil som kallas **PassEncryptKey.key**. Det ger också en krypterad version av hello lösenord som har angetts. Lösenordet är hello samma lösenord som har definierats för hello Azure Active Directory-program som används för autentisering.

```powershell
#Variables
$keypath = "C:\temp\PassEncryptKey.key"
$AESKey = New-Object Byte[] 32
$Password = "<insert a password here>"

#Keys
[Security.Cryptography.RNGCryptoServiceProvider]::Create().GetBytes($AESKey) 
Set-Content $keypath $AESKey

#Get encrypted password
$secPw = ConvertTo-SecureString -AsPlainText $Password -Force
$AESKey = Get-content $KeyPath
$Encryptedpassword = $secPw | ConvertFrom-SecureString -Key $AESKey
$Encryptedpassword
```

Skapa en mapp med namnet i hello App Service redigeringsprogram hello funktionsapp **nycklar** under **AlertPacketCapturePowerShell**. Överför hello **PassEncryptKey.key** -fil som du skapade i hello tidigare PowerShell-exempel.

![Funktioner nyckel][functions8]

### <a name="retrieve-values-for-environment-variables"></a>Hämta värden för miljövariabler

hello slutliga kravet är tooset in hello miljövariabler som är nödvändiga tooaccess hello värden för autentisering. hello visar följande lista hello miljövariabler som har skapats:

* AzureClientID

* AzureTenant

* AzureCredPassword


#### <a name="azureclientid"></a>AzureClientID

hello klient-ID är hello program-ID för ett program i Azure Active Directory.

1. Om du inte redan har ett program toouse, kör du följande exempel toocreate hello ett program.

    ```powershell
    $app = New-AzureRmADApplication -DisplayName "ExampleAutomationAccount_MF" -HomePage "https://exampleapp.com" -IdentifierUris "https://exampleapp1.com/ExampleFunctionsAccount" -Password "<same password as defined earlier>"
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    Start-Sleep 15
    New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $app.ApplicationId
    ```

   > [!NOTE]
   > hello-lösenord som du använder när du skapar hello program ska vara hello samma lösenord som du skapade tidigare när du sparar hello nyckelfilen.

1. Välj i hello Azure-portalen, **prenumerationer**. Välj hello prenumeration toouse och välj sedan **åtkomstkontroll (IAM)**.

    ![Funktioner IAM][functions9]

1. Välj hello konto toouse och välj sedan **egenskaper**. Kopiera hello program-ID.

    ![Funktioner program-ID][functions10]

#### <a name="azuretenant"></a>AzureTenant

Hämta hello klient-ID genom att köra följande PowerShell-exempel hello:

```powershell
(Get-AzureRmSubscription -SubscriptionName "<subscriptionName>").TenantId
```

#### <a name="azurecredpassword"></a>AzureCredPassword

hello-värdet för hello AzureCredPassword miljövariabeln är hello-värde som du får från att köra följande PowerShell-exempel hello. Det här exemplet är hello samma som visas i föregående hello **krypterade autentiseringsuppgifter** avsnitt. hello-värde som behövs är hello utdata från hello `$Encryptedpassword` variabeln.  Detta är hello service principal lösenord som du har krypterats med hjälp av hello PowerShell-skript.

```powershell
#Variables
$keypath = "C:\temp\PassEncryptKey.key"
$AESKey = New-Object Byte[] 32
$Password = "<insert a password here>"

#Keys
[Security.Cryptography.RNGCryptoServiceProvider]::Create().GetBytes($AESKey) 
Set-Content $keypath $AESKey

#Get encrypted password
$secPw = ConvertTo-SecureString -AsPlainText $Password -Force
$AESKey = Get-content $KeyPath
$Encryptedpassword = $secPw | ConvertFrom-SecureString -Key $AESKey
$Encryptedpassword
```

### <a name="store-hello-environment-variables"></a>Lagra hello miljövariabler

1. Gå toohello funktionsapp. Välj sedan **fungerar appinställningar** > **konfigurera appinställningar**.

    ![Konfigurera appinställningar][functions11]

1. Lägg till hello miljövariabler och deras värden toohello app-inställningar och välj sedan **spara**.

    ![App-inställningar][functions12]

### <a name="add-powershell-toohello-function"></a>Lägg till PowerShell toohello funktion

Det är nu tid toomake anrop till Nätverksbevakaren från inom hello Azure-funktion. Hello implementering av den här funktionen kan variera beroende på hello krav. Dock är hello allmänna flödet av hello koden följande:

1. Processen indataparametrar.
2. Frågan befintliga paket avbildas tooverify gränser och lösa namnkonflikter.
3. Skapa en paketinsamling med lämpliga parametrar.
4. Avsökningen paket avbilda regelbundet tills den är klar.
5. Meddela användaren hello att hello paket avbildningssessionen har slutförts.

hello är följande exempel PowerShell-kod som kan användas i hello-funktionen. Det finns värden som behöver ersättas för toobe **subscriptionId**, **resourceGroupName**, och **storageAccountName**.

```powershell
            #Import Azure PowerShell modules required toomake calls tooNetwork Watcher
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Profile\AzureRM.Profile.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Network\AzureRM.Network.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Resources\AzureRM.Resources.psd1" -Global

            #Process alert request body
            $requestBody = Get-Content $req -Raw | ConvertFrom-Json

            #Storage account ID toosave captures in
            $storageaccountid = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}"

            #Packet capture vars
            $packetcapturename = "PSAzureFunction"
            $packetCaptureLimit = 10
            $packetCaptureDuration = 10

            #Credentials
            $tenant = $env:AzureTenant
            $pw = $env:AzureCredPassword
            $clientid = $env:AzureClientId
            $keypath = "D:\home\site\wwwroot\AlertPacketCapturePowerShell\keys\PassEncryptKey.key"

            #Authentication
            $secpassword = $pw | ConvertTo-SecureString -Key (Get-Content $keypath)
            $credential = New-Object System.Management.Automation.PSCredential ($clientid, $secpassword)
            Add-AzureRMAccount -ServicePrincipal -Tenant $tenant -Credential $credential #-WarningAction SilentlyContinue | out-null


            #Get hello VM that fired hello alert
            if($requestBody.context.resourceType -eq "Microsoft.Compute/virtualMachines")
            {
                Write-Output ("Subscription ID: {0}" -f $requestBody.context.subscriptionId)
                Write-Output ("Resource Group:  {0}" -f $requestBody.context.resourceGroupName)
                Write-Output ("Resource Name:  {0}" -f $requestBody.context.resourceName)
                Write-Output ("Resource Type:  {0}" -f $requestBody.context.resourceType)

                #Get hello Network Watcher in hello VM's region
                $nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $requestBody.context.resourceRegion}
                $networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

                #Get existing packetCaptures
                $packetCaptures = Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher

                #Remove existing packet capture created by hello function (if it exists)
                $packetCaptures | %{if($_.Name -eq $packetCaptureName)
                { 
                    Remove-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName $packetCaptureName
                }}

                #Initiate packet capture on hello VM that fired hello alert
                if ((Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher).Count -lt $packetCaptureLimit){
                    echo "Initiating Packet Capture"
                    New-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $requestBody.context.resourceId -PacketCaptureName $packetCaptureName -StorageAccountId $storageaccountid -TimeLimitInSeconds $packetCaptureDuration
                    Out-File -Encoding Ascii -FilePath $res -inputObject "Packet Capture created on ${requestBody.context.resourceID}"
                }
            } 
 ``` 
#### <a name="retrieve-hello-function-url"></a>Hämta hello funktions-URL 
1. När du har skapat din funktion, konfigurera aviseringen toocall hello URL: en som är kopplad till hello-funktionen. tooget detta värde, kopiera hello funktions-URL från din funktion.

    ![Hitta hello funktions-URL][functions13]

2. Kopiera hello funktions-URL för din funktionsapp.

    ![Kopiera hello funktions-URL][2]

Om du vill använda anpassade egenskaper i hello nyttolast hello webhook POST-begäran, se för[konfigurera en webhook på en Azure mått avisering](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

## <a name="configure-an-alert-on-a-vm"></a>Konfigurera en avisering på en virtuell dator

Aviseringar kan vara konfigurerade toonotify enskilda användare när ett specifikt mått överskrider ett tröskelvärde som är tilldelad tooit. I det här exemplet hello aviseringen är på hello TCP-segment som skickas, men hello avisering kan aktiveras för många andra mått. I det här exemplet är en avisering konfigurerade toocall en webhook toocall hello-funktion.

### <a name="create-hello-alert-rule"></a>Skapa hello varningsregel

Gå tooan befintlig virtuell dator och sedan lägga till en varningsregel. Mer detaljerad dokumentation om hur du konfigurerar aviseringar finns på [skapa aviseringar i Azure-Monitor för Azure-tjänster - Azure-portalen](../monitoring-and-diagnostics/insights-alerts-portal.md). Ange följande värden i hello hello **varningsregeln** bladet och väljer sedan **OK**.

  |**Inställning** | **Värde** | **Detaljer** |
  |---|---|---|
  |**Namn**|TCP_Segments_Sent_Exceeded|Namnet på hello varningsregel.|
  |**Beskrivning**|TCP-segment skickas överskred tröskeln|hello beskrivning hello varningsregel.||
  |**Mått**|TCP-segment som skickats| hello mått toouse tootrigger hello avisering. |
  |**Villkor**|Större än| hello villkoret toouse vid utvärdering av hello mått.|
  |**Tröskelvärde**|100| hello-värdet för hello mått som utlöser hello varning. Det här värdet ska anges tooa giltigt värde för din miljö.|
  |**Period**|Över hello senaste fem minuterna| Anger hello period i vilka toolook för hello tröskelvärdet för hello mått.|
  |**Webhook**|[Webhooksadressen från funktionsapp]| Hej Webhooksadressen från hello funktionsapp som skapades i föregående steg i hello.|

> [!NOTE]
> hello TCP-segment måttet är inte aktiverad som standard. Läs mer om hur tooenable ytterligare mått genom att besöka [aktivera övervakning och diagnostik](../monitoring-and-diagnostics/insights-how-to-use-diagnostics.md).

## <a name="review-hello-results"></a>Granska resultatet från hello

Efter hello kriterier för hello avisering utlösare skapas en paketinsamling. Gå tooNetwork Watcher och välj sedan **paketinsamling**. Du kan välja hello paket avbilda filen länken toodownload hello paketinsamling på den här sidan.

![Visa paketinsamling][functions14]

Om filen hello lagras lokalt, kan du hämta det genom att logga in toohello virtuella datorn.

Anvisningar om att hämta filer från Azure storage-konton finns [komma igång med Azure Blob storage med hjälp av .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Ett annat verktyg som du kan använda är [Lagringsutforskaren](http://storageexplorer.com/).

När din avbildning har hämtats, du kan visa den med ett verktyg som kan läsa en **CAP** fil. Följande är länkar tootwo av dessa verktyg:

- [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx)
- [WireShark](https://www.wireshark.org/)

## <a name="next-steps"></a>Nästa steg

Lär dig hur tooview dina paket samlar in genom att besöka [paket avbilda analys med Wireshark](network-watcher-deep-packet-inspection.md).


[1]: ./media/network-watcher-alert-triggered-packet-capture/figure1.png
[1-1]: ./media/network-watcher-alert-triggered-packet-capture/figure1-1.png
[2]: ./media/network-watcher-alert-triggered-packet-capture/figure2.png
[3]: ./media/network-watcher-alert-triggered-packet-capture/figure3.png
[functions1]:./media/network-watcher-alert-triggered-packet-capture/functions1.png
[functions2]:./media/network-watcher-alert-triggered-packet-capture/functions2.png
[functions3]:./media/network-watcher-alert-triggered-packet-capture/functions3.png
[functions4]:./media/network-watcher-alert-triggered-packet-capture/functions4.png
[functions5]:./media/network-watcher-alert-triggered-packet-capture/functions5.png
[functions6]:./media/network-watcher-alert-triggered-packet-capture/functions6.png
[functions7]:./media/network-watcher-alert-triggered-packet-capture/functions7.png
[functions8]:./media/network-watcher-alert-triggered-packet-capture/functions8.png
[functions9]:./media/network-watcher-alert-triggered-packet-capture/functions9.png
[functions10]:./media/network-watcher-alert-triggered-packet-capture/functions10.png
[functions11]:./media/network-watcher-alert-triggered-packet-capture/functions11.png
[functions12]:./media/network-watcher-alert-triggered-packet-capture/functions12.png
[functions13]:./media/network-watcher-alert-triggered-packet-capture/functions13.png
[functions14]:./media/network-watcher-alert-triggered-packet-capture/functions14.png
[scenario]:./media/network-watcher-alert-triggered-packet-capture/scenario.png
