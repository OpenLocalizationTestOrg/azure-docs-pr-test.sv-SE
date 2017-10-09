---
title: aaaConnect Windows-datorer tooAzure Log Analytics | Microsoft Docs
description: "Den här artikeln visar hello steg tooconnect hello Windows-datorer i din lokala infrastruktur toohello logganalys-tjänsten med hjälp av en anpassad version av hello Microsoft Monitoring Agent (MMA)."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 932f7b8c-485c-40c1-98e3-7d4c560876d2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7e15f9eeb0440bd2f6557d7215df701526e4f9aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-windows-computers-toohello-log-analytics-service-in-azure"></a>Ansluta Windows-datorer toohello Log Analytics-tjänsten i Azure

Den här artikeln visar hello steg tooconnect Windows-datorer i din lokala infrastruktur tooOMS arbetsytor med hjälp av en anpassad version av hello Microsoft Monitoring Agent (MMA). Du behöver tooinstall och ansluta agenter för alla hello datorer som du vill tooonboard för toosend data toohello Log Analytics-tjänsten och tooview och agera på dessa data. Varje agent kan rapportera toomultiple arbetsytor.

Du kan installera agenter med installationsprogrammet, kommandorad, eller med önskad tillstånd Configuration (DSC) i Azure Automation.  

>[!NOTE]
För virtuella datorer som körs i Azure, kan du förenkla installationen med hjälp av hello [tillägg för virtuell dator](log-analytics-azure-vm-extension.md).

På datorer med Internetanslutning använder hello agenten hello anslutning toohello Internet toosend data tooOMS. För datorer som inte har Internetanslutning kan du använda en proxy- eller hello [OMS Gateway](log-analytics-oms-gateway.md).

Ansluta din Windows-datorer tooOMS är enkelt med tre enkla steg:

1. Hämta installationsfilen för hello agent från hello OMS-portalen
2. Installera hello agent med hello-metod som du väljer
3. Konfigurera hello agent eller Lägg till ytterligare arbetsytor, om det behövs

hello följande diagram visar hello förhållandet mellan din Windows-datorer och OMS när du har installerat och konfigurerat agenter.

![OMS-direct-agent-diagram](./media/log-analytics-windows-agents/oms-direct-agent-diagram.png)

Om din IT-säkerhetsprinciper inte tillåter datorer på ditt nätverk tooconnect toohello Internet, kan du konfigurera dina datorer tooconnect toohello OMS-Gateway. Mer information och anvisningar om hur tooconfigure dina servrar toocommunicate via en OMS-Gateway toohello OMS-tjänst, se [ansluta datorer tooOMS med hello OMS Gateway](log-analytics-oms-gateway.md).

## <a name="system-requirements-and-required-configuration"></a>Systemkrav och nödvändiga konfigurationsinställningar
Granska följande information tooensure hello kraven hello innan du installerar eller distribuera agenter.

- Du kan bara installera hello OMS MMA på datorer som kör Windows Server 2008 SP 1 eller senare eller Windows 7 SP1 eller senare.
- Du behöver en Azure-prenumeration.  Mer information finns i [Kom igång med logganalys](log-analytics-get-started.md).
- Alla Windows-datorer måste vara kan tooconnect toohello Internet med hjälp av HTTPS eller toohello OMS-Gateway. Den här anslutningen kan vara direkt via en proxy eller via hello OMS-Gateway.
- Du kan installera hello OMS MMA på fristående datorer, servrar och virtuella datorer. Om du vill tooconnect Azure som värd för virtuella datorer tooOMS [ansluta Azure virtuella datorer tooLog Analytics](log-analytics-azure-vm-extension.md).
- hello-agenten måste toouse TCP-port 443 för olika resurser.

### <a name="network"></a>Nätverk

För Windows-agenter tooconnect tooand registrera med hello OMS-tjänsten, måste de ha åtkomst toonetwork resurser, inklusive hello portnummer och URL: er för domänen.

- För proxy-servrar behöver du tooensure som hello lämpliga proxyserver resurser konfigureras i agentinställningarna för.
- För brandväggar som begränsar åtkomst toohello Internet du eller ditt nätverk tekniker behöver tooconfigure din brandvägg toopermit åtkomst tooOMS. Ingen åtgärd krävs i agentinställningarna.

hello följande tabell visar resurser som krävs för kommunikation.

>[!NOTE]
>Några av följande resurser hello nämnt åtgärdsinformation, som hette tidigare för Log Analytics.

| Agentresurs | Portar | Kringgå HTTPS-kontroll |
|---|---|---|
| *.ods.opinsights.azure.com | 443 | Ja |
| *.oms.opinsights.azure.com | 443 | Ja |
| *.blob.core.windows.net | 443 | Ja |
| *.azure-automation.net | 443 | Ja |



## <a name="download-hello-agent-setup-file-from-oms"></a>Hämta installationsfilen för hello agent från OMS
1. I hello OMS-portalen på hello **översikt** klickar du på hello **inställningar** panelen.  Klicka på hello **anslutna källor** fliken hello överst.  
    ![Fliken för anslutna datakällor](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)
2. Klicka på **Windows-servrar** och klicka sedan på **ladda ned Windows Agent** tillämpliga tooyour dator processor typen toodownload hello installationsfilen.
3. På hello höger i **arbetsyte-ID**och klicka på Kopiera-ikonen hello klistra in hello-ID i anteckningar.
4. På hello höger i **primärnyckel**och klicka på Kopiera-ikonen hello klistra in hello nyckel i anteckningar.     

## <a name="install-hello-agent-using-setup"></a>Installera hello-agenten med hjälp av installationsprogrammet
1. Kör installationsprogrammet tooinstall hello agent på en dator som du vill toomanage.
2. Klicka på välkomstsidan hello **nästa**.
3. På sidan Licensvillkor för hello Läs hello licens och klicka på **jag accepterar**.
4. Hello målmappen sida, ändra eller behålla hello standardinstallationsmappen och klicka sedan på **nästa**.
5. Du kan välja tooconnect hello agent tooAzure logganalys (OMS), Operations Manager, eller lämna hello val tomt om du senare vill tooconfigure hello agent hello installationsalternativ för Agent på sidan. Klicka på **Nästa**.   
    - Om du väljer tooconnect tooAzure logganalys (OMS) kan du klistra in hello **arbetsyte-ID** och **Arbetsytenyckel (primärnyckel)** som du kopierade i anteckningar i hello föregående proceduren och klicka sedan på  **Nästa**.  
        ![Klistra in arbetsyte-ID och primärnyckel](./media/log-analytics-windows-agents/connect-workspace.png)
    - Om du väljer tooconnect tooOperations Manager skriver hello **Hanteringsgruppnamn**, **hanteringsservern** namn, och **Hanteringsserverporten**, och klicka sedan på **Nästa**. Hej Agentåtgärdskontot på sidan Välj hello lokalt systemkonto eller ett domänkonto som är lokala och klicka på **nästa**.  
        ![konfiguration av hanteringsgrupp](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![agentåtgärdskontot](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)

6. Hello redo tooInstall på sidan Granska dina val och klicka sedan på **installera**.
7. På hello konfigurationen slutförts, klickar du på **Slutför**.
8. När du är färdig hello **Microsoft Monitoring Agent** visas i **Kontrollpanelen**. Du kan granska konfigurationen av det och verifiera att hello-agenten är ansluten tooOperational insikter (OMS). När du är ansluten tooOMS hello agent visas ett meddelande om: **hello Microsoft Monitoring Agent har lyckats ansluta toohello Microsoft Operations Management Suite-tjänsten.**

## <a name="configure-proxy-settings"></a>Konfigurera proxyinställningar

Du kan använda hello följa proceduren tooconfigure proxyinställningar för hello Microsoft Monitoring Agent på Kontrollpanelen. Behöver du toouse den här proceduren för varje server. Om du har många servrar du behöver tooconfigure kan det vara enklare toouse tooautomate ett skript för den här processen. I så fall, finns hello nästa procedur [tooconfigure proxyinställningar för hello Microsoft Monitoring Agent använder ett skript för](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).

### <a name="tooconfigure-proxy-settings-for-hello-microsoft-monitoring-agent-using-control-panel"></a>tooconfigure proxyinställningar för hello Microsoft Monitoring Agent med hjälp av Kontrollpanelen
1. Öppna **Kontrollpanelen**.
2. Öppna **Microsoft Monitoring Agent**.
3. Klicka på hello **proxyinställningar** fliken.  
    ![fliken proxyinställningar](./media/log-analytics-windows-agents/proxy-direct-agent-proxy.png)
4. Välj **använder en proxyserver** och ange hello URL och portnummer, om något toohello behövs, liknande exemplet som visas. Om proxyservern kräver autentisering, skriver du hello användarnamn och lösenord tooaccess hello proxyserver.


### <a name="verify-agent-connectivity-toooms"></a>Kontrollera agent connectivity tooOMS

Du kan enkelt kontrollera om dina agenter kommunicerar med OMS med hello nedan:

1.  Öppna Kontrollpanelen på hello dator med Windows hello-agenten.
2.  Öppna Microsoft Monitoring Agent.
3.  Fliken hello Azure logganalys (OMS).
4.  Du bör se hello agentens ansluten toohello Operations Management Suite-tjänsten i hello Status-kolumnen.

![Agent som är ansluten](./media/log-analytics-windows-agents/mma-connected.png)


### <a name="tooconfigure-proxy-settings-for-hello-microsoft-monitoring-agent-using-a-script"></a>tooconfigure proxyinställningar för hello Microsoft Monitoring Agent med hjälp av ett skript
Kopiera hello följande exempel, uppdatera det med information specifik tooyour miljö, spara om filen med filnamnstillägget PS1 och sedan köra hello skript på varje dator som ansluter direkt toohello OMS-tjänsten.

    param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

    # First we get hello Health Service configuration object.  We need toodetermine if we
    #have hello right update rollup with hello API we need.  If not, no need toorun hello rest of hello script.
    $healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

    $proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

    if (!$proxyMethod)
    {
         Write-Output 'Health Service proxy API not present, will not update settings.'
         return
    }

    Write-Output "Clearing proxy settings."
    $healthServiceSettings.SetProxyInfo('', '', '')

    $ProxyUserName = $cred.username

    Write-Output "Setting proxy too$ProxyDomainName with proxy username $ProxyUserName."
    $healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)



## <a name="install-hello-agent-using-hello-command-line"></a>Installera hello agent med kommandoraden hello
- Ändra och sedan använda hello följande exempel tooinstall hello agent hello kommandoraden. hello exempel utförs en fullständigt tyst installation.

    >[!NOTE]
    Om du vill tooupgrade en agent måste toouse hello logganalys scripting-API. Se hello nästa avsnitt tooupgrade en agent.

    ```dos
    MMASetup-AMD64.exe /Q:A /R:N /C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1"
    ```

hello-agenten använder IExpress som dess Self-Extractor med hello `/c` kommando. Du kan se hello kommandoradsväxlar vid [kommandoradsväxlar för IExpress](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages) och sedan hello update exempel toosuit dina behov.

|MMA-specifika alternativ                   |Anteckningar         |
|---------------------------------------|--------------|
|ADD_OPINSIGHTS_WORKSPACE               | 1 = konfigurera hello agent tooreport tooa arbetsytan                |
|OPINSIGHTS_WORKSPACE_ID                | Arbetsyte-Id (guid) för hello arbetsytan tooadd                    |
|OPINSIGHTS_WORKSPACE_KEY               | Arbetsytan nyckel används tooinitially autentisera med hello-arbetsyta |
|OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE  | Ange hello molnmiljö där hello arbetsytan finns <br> 0 = kommersiella azuremolnet (standard) <br> 1 = azure Government |
|OPINSIGHTS_PROXY_URL               | URI för hello proxy toouse |
|OPINSIGHTS_PROXY_USERNAME               | Användarnamnet tooaccess en autentiserad proxyserver |
|OPINSIGHTS_PROXY_PASSWORD               | Lösenordet tooaccess en autentiserad proxyserver |

>[!NOTE]
tooavoid hitting hello kommandorad maxlängden för IExpress, installera hello agent med Ingen arbetsyta som konfigurerats och sedan använda ett skript tooset konfiguration för hello arbetsytan.

>[!NOTE]
Om du får en `Command line option syntax error.` när du använder hello `OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE` parameter, kan du använda följande lösning hello:
```dos
MMASetup-AMD64.exe /C /T:.\MMAExtract
cd .\MMAExtract
setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1
```

## <a name="add-a-workspace-using-a-script"></a>Lägg till en arbetsyta med hjälp av ett skript
Lägg till en arbetsyta med följande exempel hello hello logganalys agent scripting-API:

```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

tooadd en arbetsyta i Azure och som tillhör amerikanska myndigheter, Använd hello följande skriptexempel:
```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey, 1)
$mma.ReloadConfiguration()
```

>[!NOTE]
Om du har använt hello kommandoraden eller skript tidigare tooinstall eller konfigurera hello agent `EnableAzureOperationalInsights` ersattes med `AddCloudWorkspace`.

## <a name="install-hello-agent-using-dsc-in-azure-automation"></a>Installera hello-agenten i Azure Automation DSC

Du kan använda hello följande skript exempel tooinstall hello agent i Azure Automation DSC. hello exempel installerar hello 64-bitars agent identifieras av hello `URI` värde. Du kan också använda hello 32-bitars version genom att ersätta hello URI-värdet. hello URI: er för båda versionerna är:

- Windows 64-bitars agent - https://go.microsoft.com/fwlink/?LinkId=828603
- Windows 32-bitars agent - https://go.microsoft.com/fwlink/?LinkId=828604


>[!NOTE]
Den här proceduren och skript exempel uppgraderar inte en befintlig agent.

1. Importera hello xPSDesiredStateConfiguration DSC-modul från [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) i Azure Automation.  
2.  Skapa en variabel Azure Automation-tillgångar för *OPSINSIGHTS_WS_ID* och *OPSINSIGHTS_WS_KEY*. Ange *OPSINSIGHTS_WS_ID* tooyour OMS logganalys arbetsyte-ID och ange *OPSINSIGHTS_WS_KEY* toohello primärnyckel för din arbetsyta.
3.  Använd hello följande skript och spara den som MMAgent.ps1
4.  Ändra och sedan använda hello följande exempel tooinstall hello agent i Azure Automation DSC. Importera MMAgent.ps1 till Azure Automation via hello Azure Automation-gränssnittet eller cmdlet.
5.  Tilldela en nod toohello konfiguration. Hello noden kontrollerar konfigurationen och hello MMA flyttas toohello nod inom 15 minuter.

```PowerShell
Configuration MMAgent
{
    $OIPackageLocalPath = "C:\MMASetup-AMD64.exe"
    $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
    $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"


    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    Node OMSnode {
        Service OIService
        {
            Name = "HealthService"
            State = "Running"
            DependsOn = "[Package]OI"
        }

        xRemoteFile OIPackage {
            Uri = "https://go.microsoft.com/fwlink/?LinkId=828603"
            DestinationPath = $OIPackageLocalPath
        }

        Package OI {
            Ensure = "Present"
            Path  = $OIPackageLocalPath
            Name = "Microsoft Monitoring Agent"
            ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
            Arguments = '/C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
            DependsOn = "[xRemoteFile]OIPackage"
        }
    }
}


```

### <a name="get-hello-latest-productid-value"></a>Hämta hello senaste ProductId värde

Hej `ProductId value` i hello MMAgent.ps1 skript är unika tooeach agent-version. När en uppdaterad version av varje agent publiceras ändras hello ProductId värde. Så, när hello ProductId ändras i framtida hello, hittar hello agent-version med ett enkelt skript. Du kan använda hello följande skript tooget hello installerat ProductId värdet när du har hello senaste agent installerade versionen på en testserver. Du kan uppdatera hello värdet i hello MMAgent.ps1 skript med hello senaste ProductId värdet.

```PowerShell
$InstalledApplications  = Get-ChildItem hklm:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall


foreach ($Application in $InstalledApplications)

{

     $Key = Get-ItemProperty $Application.PSPath

     if ($Key.DisplayName -eq "Microsoft Monitoring Agent")

     {

        $Key.DisplayName

        Write-Output ("Product ID is: " + $Key.PSChildName.Substring(1,$Key.PSChildName.Length -2))

     }

}  
```

## <a name="configure-an-agent-manually-or-add-additional-workspaces"></a>Konfigurera en agent manuellt eller lägga till ytterligare arbetsytor
Om du har installerat agenter utan att konfigurera dem eller om du vill hello agent tooreport toomultiple arbetsytor, kan du använda följande information tooenable en agent hello eller konfigurera om den. När du har konfigurerat hello agent kan registreras med hello agent-tjänsten och får nödvändiga konfigurationsinformation och hanteringspaket som innehåller information om lösning.

1. När du har installerat hello Microsoft Monitoring Agent kan öppna **Kontrollpanelen**.
2. Öppna **Microsoft Monitoring Agent** och klicka sedan på hello **Azure logganalys (OMS)** fliken.   
3. Klicka på **Lägg till** tooopen hello **lägga till en Log Analytics-arbetsyta** rutan.
4. Klistra in hello **arbetsyte-ID** och **Arbetsytenyckel (primärnyckel)** som du kopierade i anteckningar i föregående procedur för hello arbetsytan som du vill tooadd och klicka sedan på **OK**.  
    ![Konfigurera åtgärdsinformation](./media/log-analytics-windows-agents/add-workspace.png)

När data har samlats in från datorer som övervakas av hello agent, hello antalet datorer som övervakas av OMS visas i hello OMS-portalen på hello **anslutna källor** fliken i **inställningar** som  **Servrar som är anslutna**.


## <a name="toodisable-an-agent"></a>toodisable en agent
1. När du har installerat agenten hello öppna **Kontrollpanelen**.
2. Öppna Microsoft Monitoring Agent och klicka sedan på hello **Azure logganalys (OMS)** fliken.
3. Välj en arbetsyta och klicka sedan på **ta bort**. Upprepa det här steget för alla andra arbetsytor.


## <a name="optionally-configure-agents-tooreport-tooan-operations-manager-management-group"></a>Du kan också konfigurera agenter tooreport tooan Operations Manager-hanteringsgruppen

Du kan också använda hello MMA agenten som en Operations Manager-agent om du använder Operations Manager i din IT-infrastruktur.

### <a name="tooconfigure-mma-agents-tooreport-tooan-operations-manager-management-group"></a>tooconfigure MMA agenter tooreport tooan Operations Manager-hanteringsgrupp
1.  Öppna på hello dator där hello-agenten är installerad **Kontrollpanelen**.  
2.  Öppna **Microsoft Monitoring Agent** och klicka sedan på hello **Operations Manager** fliken.  
    ![Microsoft Monitoring Agent Operations Manager-fliken](./media/log-analytics-windows-agents/om-mg01.png)
3.  Om Operations Manager-servrar har integrering med Active Directory, klickar du på **automatiskt uppdatera hanteringsgrupptilldelningar från AD DS**.
4.  Klicka på **Lägg till** tooopen hello **lägga till en Hanteringsgrupp** dialogrutan.  
    ![Microsoft Monitoring Agent lägga till en Hanteringsgrupp](./media/log-analytics-windows-agents/oms-mma-om02.png)
5.  I **hanteringsgruppnamn** rutan, hello-typnamn för hanteringsgruppen.
6.  I hello **primära hanteringsserver** rutan, typen hello datornamnet på hello primära hanteringsserver.
7.  I hello **hanteringsserverporten** rutan typen hello TCP-portnummer.
8.  Under **Agentåtgärdskontot**, Välj hello lokalt systemkonto eller ett lokala domänkonto.
9.  Klicka på **OK** tooclose hello **lägga till en Hanteringsgrupp** dialogrutan och klicka sedan på **OK** tooclose hello **Microsoft Monitoring agentegenskaper**dialogrutan.


## <a name="next-steps"></a>Nästa steg

- [Lägg till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md) tooadd funktioner och samla data.
