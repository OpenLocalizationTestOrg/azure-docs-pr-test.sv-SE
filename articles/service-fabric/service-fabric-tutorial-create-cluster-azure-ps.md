---
title: aaaCreate ett Service Fabric-klustret i Azure | Microsoft Docs
description: "Lär dig hur toocreate Windows- eller Linux Service Fabric-klustret i Azure med hjälp av PowerShell."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/13/2017
ms.author: ryanwi
ms.openlocfilehash: e697e2a2e99f67cb02422efdb368c664c9fd5a97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-secure-cluster-on-azure-using-powershell"></a>Skapa en säker kluster i Azure med hjälp av PowerShell
Den här kursen visar hur toocreate ett Service Fabric-kluster (Windows eller Linux) körs i Azure. När du är klar har du ett kluster som kör i hello moln som du kan distribuera program till.

I den här guiden får du lära dig hur man:

> [!div class="checklist"]
> * Skapa en säker Service Fabric-kluster i Azure med hjälp av PowerShell
> * Säker hello kluster med ett X.509-certifikat
> * Ansluta toohello kluster med hjälp av PowerShell
> * Ta bort ett kluster

## <a name="prerequisites"></a>Krav
Innan du börjar den här kursen:
- Om du inte har en Azure-prenumeration kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Installera hello [Fabric-SDK och PowerShell-modul](service-fabric-get-started.md)
- Installera hello [Azure Powershell Modulversion 4.1 eller högre](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)

hello följer proceduren skapar en nod (virtuell dator) förhandsgranskning Service Fabric-klustret. hello kluster skyddas av ett självsignerat certifikat som hämtar skapas tillsammans med hello klustret och sedan placeras i en nyckelvalvet. Enkelnods-kluster kan inte skalas utöver en virtuell dator och förhandsgranska kluster får inte vara uppgraderade toonewer versioner.

toocalculate kostnaden genom att köra ett Service Fabric-kluster i Azure används hello [Priskalkylatorn för Azure](https://azure.microsoft.com/pricing/calculator/).
Mer information om hur du skapar Service Fabric-kluster finns [skapa ett Service Fabric-kluster med hjälp av Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).

## <a name="create-hello-cluster-using-azure-powershell"></a>Skapa hello-kluster med hjälp av Azure PowerShell
1. Hämta en lokal kopia av hello Azure Resource Manager-mall och hello parameterfilen från hello [Azure Resource Manager-mall för Service Fabric](https://aka.ms/securepreviewonelineclustertemplate) GitHub-lagringsplatsen.  *azuredeploy.JSON* är hello Azure Resource Manager-mallen som definierar ett Service Fabric-kluster. *azuredeploy.parameters.JSON* är en fil med parametrar för dig toocustomize hello klusterdistribution.

2. Anpassa följande parametrar i hello hello *azuredeploy.parameters.json* parameterfilen:

   | Parameter       | Beskrivning | Föreslaget värde |
   | --------------- | ----------- | --------------- |
   | clusterLocation | hello Azure-region toowhich toodeploy hello kluster. | *till exempel westeurope eastasia, eastus* |
   | Klusternamn     | Namn på hello klustret som du vill toocreate. | *till exempel bobs-sfpreviewcluster* |
   | adminUserName   | hello lokalt administratörskonto på hello klustra virtuella datorer. | *Alla Windows Server-användarnamn* |
   | adminPassword   | Lösenordet för hello lokalt administratörskonto på hello klustra virtuella datorer. | *Alla Windows Server-lösenord* |
   | clusterCodeVersion | hello Service Fabric version toorun. (255.255.X.255 är förhandsversioner). | **255.255.5718.255** |
   | vmInstanceCount | hello antalet virtuella datorer i klustret (kan vara 1 eller 3-99). | **1** | *Ange endast en virtuell dator för ett kluster för förhandsgranskning* |

3. Öppna ett PowerShell-konsolen, inloggning tooAzure och välj hello-prenumeration som du vill toodeploy hello klustret i:

   ```powershell
   Login-AzureRmAccount
   Select-AzureRmSubscription -SubscriptionId <subscription-id>
   ```
4. Skapa och kryptera ett lösenord för hello certifikat toobe används av Service Fabric.

   ```powershell
   $pwd = "<your password>" | ConvertTo-SecureString -AsPlainText -Force
   ```
5. Skapa hello klustret och dess certifikat genom att köra följande kommando hello:

   ```powershell
      New-AzureRmServiceFabricCluster
          -TemplateFile C:\Users\me\Desktop\azuredeploy.json `
          -ParameterFile C:\Users\me\Desktop\azuredeploy.parameters.json `
          -CertificateOutputFolder C:\Users\me\Desktop\ `
          -CertificatePassword $pwd `
          -CertificateSubjectName "mycluster.westeurope.cloudapp.azure.com" `
          -ResourceGroupName myclusterRG
   ```

   >[!NOTE]
   >Hej `-CertificateSubjectName` parametern ska justeras med hello klusternamn parameter har angetts i hello parameterfilen, samt som hello domän knutna toohello Azure-region du har valt, exempelvis: `clustername.eastus.cloudapp.azure.com`.

När hello konfigurationen är klar visar information om hello klustret skapas i Azure. Hello klustret certifikat toohello CertificateOutputFolder - katalogen på hello sökvägen för den här parametern kopieras också. Du behöver det här certifikatet tooaccess Service Fabric Explorer och visa hello hälsotillståndet för klustret.

Anteckna hello URL för klustret, vilket kan vara liknande toohello följande URL: *https://mycluster.westeurope.cloudapp.azure.com:19080*

## <a name="modify-hello-certificate--access-service-fabric-explorer"></a>Ändra certifikat för hello & åtkomst till Service Fabric Explorer ##

1. Dubbelklicka på hello certifikat tooopen hello guiden Importera certifikat.

2. Använd standardinställningar, men se till att toocheck hello **Markera den här nyckeln kan exporteras.** kryssrutan i hello **skydd av privat nyckel** steg. Visual Studio måste tooexport hello certifikat när du konfigurerar Azure Container registret tooService Fabric-kluster autentisering senare i den här kursen.

3. Nu kan du öppna Service Fabric Explorer i en webbläsare. toodo gå så toohello **ManagementEndpoint** URL för klustret med hjälp av en webbläsare och välj hello-certifikat som har sparats på din dator.

>[!NOTE]
>När du öppnar Service Fabric Explorer kan se du ett certifikat som du använder ett självsignerat certifikat. I gränsen, har du tooclick *information* och sedan hello *finns på webbsidan toohello* länk. I Chrome, har du tooclick *Avancerat* och sedan hello *fortsätta* länk.

>[!NOTE]
>Om hello klustret misslyckas, kan du alltid köra hello-kommandot, vilket uppdaterar hello resurser som redan har distribuerats. Om ett certifikat har skapats som en del av distributionen hello misslyckades, skapas en ny. tootroubleshoot klustret skapas finns [skapa ett Service Fabric-kluster med hjälp av Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).

## <a name="connect-toohello-secure-cluster"></a>Ansluta toohello säker kluster
Ansluta toohello kluster med hjälp av hello Service Fabric PowerShell module installerad med hello Service Fabric-SDK.  Installera först hello certifikat till hello Personal (min) arkivet för hello aktuella användare på datorn.  Kör följande PowerShell-kommando hello:

```powershell
$certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

Du är nu redo tooconnect tooyour säker klustret.

Hej **Service Fabric** PowerShell-modulen ger många cmdlet: ar för hantering av Service Fabric-kluster, program och tjänster.  Använd hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) cmdlet tooconnect toohello säker klustret. hello tumavtryck för certifikat och information om anslutningen som återfinns i hello utdata från föregående steg.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

Kontrollera att du är ansluten och hello klustret är felfri med hello [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) cmdlet.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a>Rensa resurser

Ett kluster består av andra Azure-resurser förutom toohello klusterresurs sig själv. hello enklaste sättet toodelete hello kluster och alla hello-resurser som den förbrukar är toodelete hello resursgruppen.

Logga in tooAzure och välj hello prenumerations-ID som du vill använda tooremove hello klustret.  Du hittar prenumerations-ID genom att logga in toohello [Azure-portalen](http://portal.azure.com). Ta bort hello resursgruppen och alla hello klusterresurser med hello [cmdlet Remove-AzureRMResourceGroup](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId "Subcription ID"

$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen lärde du dig att:

> [!div class="checklist"]
> * Skapa ett Service Fabric-kluster i Azure
> * Säker hello kluster med ett X.509-certifikat
> * Ansluta toohello kluster med hjälp av PowerShell
> * Ta bort ett kluster

Därefter i förväg toohello följa kursen toolearn hur toodeploy ett befintligt program.
> [!div class="nextstepaction"]
> [Distribuera ett befintligt .NET-program med Docker Compose](service-fabric-host-app-in-a-container.md)
