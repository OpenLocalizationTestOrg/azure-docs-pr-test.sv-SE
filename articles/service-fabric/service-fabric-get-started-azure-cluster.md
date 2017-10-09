---
title: aaaSet upp ett Azure Service Fabric-kluster | Microsoft Docs
description: "Snabbstart – skapa ett Service Fabric-kluster i Azure för Windows eller Linux."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/24/2017
ms.author: ryanwi
ms.openlocfilehash: 13c60e293d19d607bb41ee4859706508c219a833
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-service-fabric-cluster-on-azure"></a>Skapa ditt första Service Fabric-kluster i Azure
Ett [Service Fabric-kluster](service-fabric-deploy-anywhere.md) är en nätverksansluten uppsättning virtuella eller fysiska datorer som dina mikrotjänster distribueras till och hanteras från. Den här snabbstarten hjälper dig att toocreate ett kluster med fem noder, körs på Windows- eller Linux, via hello [Azure PowerShell](https://msdn.microsoft.com/library/dn135248) eller [Azure-portalen](http://portal.azure.com) i bara några minuter.  

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.


## <a name="use-hello-azure-portal"></a>Använd hello Azure-portalen

Logga in toohello Azure-portalen på [http://portal.azure.com](http://portal.azure.com).

### <a name="create-hello-cluster"></a>Skapa hello-kluster

1. Klicka på hello **ny** knappen hittades på hello övre vänstra hörnet av hello Azure-portalen.
2. Välj **Compute** från hello **ny** bladet och väljer sedan **Service Fabric-kluster** från hello **Compute** bladet.
3. Fyll i hello Service Fabric **grunderna** formuläret. För **operativsystemet**väljer hello version av Windows eller Linux som du vill hello toorun för noder av klustret. hello-användarnamn och lösenord som anges här är används toolog i toohello virtuella datorn. Skapa en ny för **Resursgrupp**. En resursgrupp är en logisk behållare där Azure-resurser skapas och hanteras gemensamt. När du är klar klickar du på **OK**.

    ![Utdata efter klusterinstallationen][cluster-setup-basics]

4. Fyll i hello **klusterkonfigurationen** formuläret.  För **Antal nodtyper** anger du "1".

5. Välj **nodtypen 1 (primär)** och fylla i hello **typen nodkonfiguration** formuläret.  Ange ett typnamn för noden och ange hello [hållbarhetsnivån](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) för ”Brons”.  Välj en VM-storlek.

    Nodtyper definiera hello VM-storlek, antal virtuella datorer, anpassade slutpunkter och andra inställningar för hello virtuella datorer av den typen. Varje nodtyp definierats ställs in som en separat virtuell dator skaluppsättning som används toodeploy och hanterade virtuella datorer som en uppsättning. Varje nodtyp kan skalas upp eller ned oberoende av de andra, ha olika portar öppna och ha olika kapacitet.  hello första eller primära nodtypen är där Service Fabric systemtjänster finns och måste ha fem eller fler virtuella datorer.

    Vid distribution till en produktionsmiljö är det viktigt med [kapacitetsplanering](service-fabric-cluster-capacity.md).  I den här snabbstarten kör du däremot inga program, så välj VM-storleken *DS1_v2 Standard*.  Välj ”Silver” för hello [tillförlitlighetsnivån](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) och en inledande virtuella skaluppsättning kapacitet på 5.  

    Anpassade slutpunkter öppna portar i hello Azure belastningsutjämnare så att du kan ansluta med program som körs på hello klustret.  Ange ”80, 8172” tooopen in portarna 80 och 8172.

    Sök inte hello **konfigurera avancerade inställningar** som används för att anpassa TCP/HTTP-slutpunkter för hantering, programmet portintervall [placeringsbegränsningar](service-fabric-cluster-resource-manager-configure-services.md#placement-constraints), och [kapacitet Egenskaper för](service-fabric-cluster-resource-manager-metrics.md).    

    Välj **OK**.

6. I hello **klusterkonfigurationen** formuläret genom att ange **diagnostik** för**på**.  För Snabbstart, behöver du inte tooenter alla [fabric inställningen](service-fabric-cluster-fabric-settings.md) egenskaper.  I **Fabric-versionen**väljer **automatisk** Uppgraderingsläge så att Microsoft uppdaterar automatiskt hello version av hello fabric-kod som körs hello klustret.  Ange hello-läge för**manuell** om du vill använda för[väljer en version som stöds](service-fabric-cluster-upgrade.md) tooupgrade till. 

    ![Konfiguration av nodtyp][node-type-config]

    Välj **OK**.

7. Fyll i hello **säkerhet** formuläret.  I den här snabbstarten kan du välja **Ta bort skydd**.  Det är mycket rekommenderas toocreate ett säker kluster för produktionsarbetsbelastningar, men eftersom alla anonymt kan ansluta tooan oskyddade kluster och utföra hanteringsåtgärder.  

    Certifikat används i Service Fabric tooprovide autentisering och kryptering toosecure olika aspekter av ett kluster och dess program. Mer information om hur du använder certifikat i Service Fabric finns i [Service Fabric cluster security scenarios](service-fabric-cluster-security.md) (Säkerhet för Service Fabric-kluster).  tooenable användarautentisering med Azure Active Directory eller tooset in certifikat för programsäkerhet, [skapa ett kluster från en Resource Manager-mall](service-fabric-cluster-creation-via-arm.md).

    Välj **OK**.

8. Granska hello sammanfattning.  Om du vill att toodownload en Resource Manager-mall som bygger på hello inställningar som du angett, Välj **ladda ned mall och parametrar**.  Välj **skapa** toocreate hello klustret.

    Du kan se hello förlopp i hello meddelanden. (Klicka på hello ”” klockikonen nära hello hello övre högra hörnet på skärmen i statusfältet.) Om du klickade på **PIN-kod tooStartboard** när du skapar hello klustret måste du se **distribuerar Service Fabric-kluster** Fäst toohello **starta** kort.

### <a name="view-cluster-status"></a>Visa klusterstatus
När klustret har skapats kan du granska klustret i hello **översikt** bladet i hello portal. Du kan nu se hello information på klustret i hello instrumentpanel, inklusive hello klustrets offentlig slutpunkt och en länk tooService Fabric Explorer.

![Klusterstatus][cluster-status]

### <a name="visualize-hello-cluster-using-service-fabric-explorer"></a>Visualisera hello-kluster med hjälp av Service Fabric explorer
[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) är ett bra verktyg för att visualisera klustret och hantera program.  Service Fabric Explorer är en tjänst som körs i hello klustret.  Åtkomst till den i en webbläsare genom att klicka på hello **Service Fabric Explorer** länk hello klustret **översikt** sidan hello-portalen.  Du kan också ange hello-adressen direkt i webbläsaren hello: [http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer](http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer)

Hej klusterinstrumentpanel innehåller en översikt över klustret, inklusive en sammanfattning av program och noden hälsa. hello nod vyn visar hello fysiska struktur hello klustret. För en viss nod kan du inspektera vilka program som har kod distribuerad på noden.

![Service Fabric Explorer][service-fabric-explorer]

### <a name="connect-toohello-cluster-using-powershell"></a>Ansluta toohello kluster med hjälp av PowerShell
Kontrollera att hello-kluster körs genom att ansluta med hjälp av PowerShell.  Hej ServiceFabric PowerShell-modulen är installerad med hello [Service Fabric SDK](service-fabric-get-started.md).  Hej [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet upprättar en anslutning toohello klustret.   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint quickstartcluster.westus2.cloudapp.azure.com:19000
```
Se [Anslut tooa säker klustret](service-fabric-connect-to-secure-cluster.md) andra exempel på den anslutande tooa klustret. När du ansluter toohello kluster, använda hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet toodisplay en lista över noderna i hello kluster och status för varje nod. **HealthState** bör vara *OK* för varje nod.

```powershell
PS C:\Users\sfuser> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName     IpAddressOrFQDN NodeType  CodeVersion  ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- --------     --------------- --------  -----------  ------------- ---------- ---------- ------------ -----------
                     _nodetype1_2 10.0.0.6        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_1 10.0.0.5        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_0 10.0.0.4        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_4 10.0.0.8        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_3 10.0.0.7        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
```

### <a name="remove-hello-cluster"></a>Ta bort hello kluster
Service Fabric-klustret består av andra Azure-resurser förutom toohello klusterresurs sig själv. Så toocompletely ta bort Service Fabric-kluster behöver du också toodelete alla hello av resurser. hello enklaste sättet toodelete hello kluster och alla hello-resurser som den förbrukar är toodelete hello resursgruppen. För andra sätt toodelete ett kluster eller toodelete vissa (men inte alla) hello resurser i en resursgrupp, se [tar bort ett kluster](service-fabric-cluster-delete.md)

Ta bort en resursgrupp i hello Azure-portalen:
1. Navigera toohello Service Fabric-kluster som du vill toodelete.
2. Klicka på hello **resursgruppen** namn på hello klustret essentials sida.
3. I hello **resurs grupp Essentials** klickar du på **ta bort resursgruppen** och följer instruktionerna för hello på sidan toocomplete hello borttagningen av hello resursgruppen.
    ![Ta bort hello resursgruppen][cluster-delete]


## <a name="use-azure-powershell-toodeploy-a-secure-cluster"></a>Använda Azure Powershell toodeploy säker kluster
1. Hämta hello [Azure Powershell Modulversion 4.0 eller högre](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) på din dator.

2. Öppna Windows PowerShell-fönstret, kör hello följande kommando. 
    
    ```powershell

    Get-Command -Module AzureRM.ServiceFabric 
    ```

    Du bör se en liknande toohello följande i utdata.

    ![ps-list][ps-list]

3. Inloggningen tooAzure och välj hello prenumeration toowhich som du vill toocreate hello kluster

    ```powershell

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionId "Subcription ID" 
    ```

4. Kör hello efter kommandot toonow skapa en säker kluster. Glöm inte toocustomize hello parametrar. 

    ```powershell
    $certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
    $RDPpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force 
    $RDPuser="vmadmin"
    $RGname="mycluster" # this is also hello name of your cluster
    $clusterloc="SouthCentralUS"
    $subname="$RGname.$clusterloc.cloudapp.azure.com"
    $certfolder="c:\mycertificates\"
    $clustersize=1 # can take values 1, 3-99

    New-AzureRmServiceFabricCluster -ResourceGroupName $RGname -Location $clusterloc -ClusterSize $clustersize -VmUserName $RDPuser -VmPassword $RDPpwd -CertificateSubjectName $subname -CertificatePassword $certpwd -CertificateOutputFolder $certfolder
    ```

    hello-kommandot kan ta allt från 10 minuter too30 minuter toocomplete, hello slutet av det, du bör få en liknande toohello följande i utdata. hello utdata innehåller information om hello certifikat, hello KeyVault där paketet har överförts till, och hello lokala mappen där hello certifikat kopieras. 

    ![ps-out][ps-out]

5. Kopiera hela hello-utdata och spara tooa textfil som vi behöver toorefer tooit. Anteckna hello följande information från hello utdata. 

    - **CertificateSavedLocalPath** : c:\mycertificates\mycluster20170504141137.pfx
    - **CertificateThumbprint** : C4C1E541AD512B8065280292A8BA6079C3F26F10
    - **ManagementEndpoint** : https://mycluster.southcentralus.cloudapp.azure.com:19080
    - **ClientConnectionEndpointPort** : 19000

### <a name="install-hello-certificate-on-your-local-machine"></a>Installera hello certifikat på den lokala datorn
  
tooconnect toohello klustret, behöver du tooinstall hello certifikat till hello Personal (min) arkivet för hello aktuella användaren. 

Kör följande PowerShell hello

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\hello name of hello cert.pfx `
        -Password (ConvertTo-SecureString -String certpwd -AsPlainText -Force)
```

Du är nu redo tooconnect tooyour säker klustret.

### <a name="connect-tooa-secure-cluster"></a>Ansluta tooa säker kluster 

Kör följande PowerShell-kommandot tooconnect tooa säker klustret hello. information om hello certifikat måste matcha ett certifikat som har använt tooset in hello kluster. 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```


följande exempel visar hello hello slutförts parametrar: 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mycluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

Kör hello efter kommandot toocheck att du är ansluten och hello klustret är felfri.

```powershell

Get-ServiceFabricClusterHealth

```
### <a name="publish-your-apps-tooyour-cluster-from-visual-studio"></a>Publicera appar tooyour klustret från Visual Studio

Nu när du har skapat ett Azure-kluster, kan du publicera dina program tooit från Visual Studio genom följande hello [publicera tooan klustret](service-fabric-publish-app-remote-cluster.md) dokumentet. 

### <a name="remove-hello-cluster"></a>Ta bort hello kluster
Ett kluster består av andra Azure-resurser förutom toohello klusterresurs sig själv. hello enklaste sättet toodelete hello kluster och alla hello-resurser som den förbrukar är toodelete hello resursgruppen. 

```powershell

Remove-AzureRmResourceGroup -Name $RGname -Force

```

## <a name="next-steps"></a>Nästa steg
Nu när du har skapat ett kluster för utveckling, försök hello följande:
* [Skapa en säker kluster i hello-portalen](service-fabric-cluster-creation-via-portal.md)
* [Create a cluster from a template](service-fabric-cluster-creation-via-arm.md) (Skapa ett kluster från en mall) 
* [Distribuera appar med hjälp av PowerShell](service-fabric-deploy-remove-applications.md)


[cluster-setup-basics]: ./media/service-fabric-get-started-azure-cluster/basics.png
[node-type-config]: ./media/service-fabric-get-started-azure-cluster/nodetypeconfig.png
[cluster-status]: ./media/service-fabric-get-started-azure-cluster/clusterstatus.png
[service-fabric-explorer]: ./media/service-fabric-get-started-azure-cluster/sfx.png
[cluster-delete]: ./media/service-fabric-get-started-azure-cluster/delete.png
[ps-list]: ./media/service-fabric-get-started-azure-cluster/pslist.PNG
[ps-out]: ./media/service-fabric-get-started-azure-cluster/psout.PNG
