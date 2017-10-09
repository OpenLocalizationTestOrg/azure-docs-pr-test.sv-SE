---
title: aaaManage certifikat i ett Azure Service Fabric-kluster | Microsoft Docs
description: "Beskriver hur tooadd nya certifikat, förnyelsecertifikat och ta bort certifikat tooor från ett Service Fabric-kluster."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 91adc3d3-a4ca-46cf-ac5f-368fb6458d74
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/09/2017
ms.author: chackdan
ms.openlocfilehash: 8e57bd95dbb800ecc04cf6988047e3abdc2fe56a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-or-remove-certificates-for-a-service-fabric-cluster-in-azure"></a>Lägg till eller ta bort certifikat för Service Fabric-kluster i Azure
Vi rekommenderar att du bekanta dig med hur Service Fabric använder X.509-certifikat och känna till hello [kluster säkerhetsscenarier](service-fabric-cluster-security.md). Du måste förstå vad ett certifikat för klustret och som används för, innan du fortsätter ytterligare.

Service fabric kan du ange två kluster certifikat, en primär och en sekundär när du konfigurerar certifikat säkerhet när klustret skapas i tillägg tooclient certifikat. Se för[skapar ett azure-kluster via portalen](service-fabric-cluster-creation-via-portal.md) eller [skapar ett azure-kluster via Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) för information om att ställa in dem på Skapa tid. Om du anger bara ett certifikat för kluster på Skapa tid, som används som primär hello-certifikat. Du kan lägga till ett nytt certifikat som en sekundär när klustret skapades.

> [!NOTE]
> För en säker kluster behöver du alltid minst en giltig (inte återkallat och inte har upphört att gälla) kluster certifikat (primära eller sekundära) distribueras (om inte, hello klustret slutar fungera). 90 dagar innan alla giltiga certifikat når förfallodatum, genereras hello en varning-spårning och en varningshändelse hälsa på hello-nod. Det finns för närvarande ingen e-post eller andra notification service fabric skickar ut om det här avsnittet. 
> 
> 

## <a name="add-a-secondary-cluster-certificate-using-hello-portal"></a>Lägga till ett sekundärt kluster-certifikat med hjälp av hello portal

Sekundära kluster certifikat kan inte läggas till hello Azure-portalen. Du har toouse Azure powershell för den. hello process beskrivs senare i det här dokumentet.

## <a name="swap-hello-cluster-certificates-using-hello-portal"></a>Växla hello klustret certifikat med hjälp av hello-portalen

När du har distribuerat ett sekundärt kluster-certifikat har om du vill tooswap hello primära och sekundära, sedan navigera toohello säkerhet bladet och välj alternativet för hello 'Växlingen med primära' från hello kontexten menyn tooswap hello sekundära certifikat med hello primära certifikat.

![Växla certifikat][Delete_Swap_Cert]

## <a name="remove-a-cluster-certificate-using-hello-portal"></a>Ta bort ett certifikat för kluster med hjälp av hello portal

För en säker kluster behöver alltid du minst en giltig (inte återkallat och inte har upphört att gälla) certifikat (primära eller sekundära) distribueras om inte, hello sluta fungera.

tooremove ett sekundärt certifikat används för klustret säkerhet, analysera toohello säkerhet bladet och väljer hello 'Radera' alternativet hello snabbmenyn på hello sekundärt certifikat.

Om din avsikt är tooremove hello-certifikat som har markerats primära, behöver du tooswap med hello sekundära först och tar bort hello sekundära när hello uppgraderingen har slutförts.

## <a name="add-a-secondary-certificate-using-resource-manager-powershell"></a>Lägg till ett sekundärt certifikat med hjälp av hanteraren för filserverresurser Powershell

Dessa instruktioner förutsätter att du känner till hur Resource Manager fungerar och har distribuerat minst en Service Fabric-kluster med hjälp av en Resource Manager-mall och har hello mall som du använde tooset in hello kluster praktiska. Det förutsätts även att du är nöjd med JSON.

> [!NOTE]
> Om du letar efter en exempelmall och parametrar som du kan använda toofollow längs eller som en startpunkt sedan ladda ned det från den här [git-lagringsplatsen](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample). 
> 
> 

### <a name="edit-your-resource-manager-template"></a>Redigera Resource Manager-mall

För att underlätta av följande längs innehåller exempel 5-VM-1-nodetypes får-Secure_Step2.JSON alla hello redigeringar vi kommer att göra. hello-exempel finns på [git-lagringsplatsen](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).

**Se till att toofollow alla hello steg**

**Steg 1:** öppna hello Resource Manager-mall som du använde toodeploy du ingå i klustret. (Om du har hämtat hello exempel från hello ovan lagringsplatsen, använda 5-VM-1-nodetypes får-Secure_Step1.JSON toodeploy säker klustret och sedan öppna mallen).

**Steg 2:** Lägg till **två nya parametrar** ”secCertificateThumbprint” och ”secCertificateUrlValue” av typen ”sträng” toohello parametern avsnitt i mallen. Du kan kopiera hello följande kodavsnitt och lägga till den toohello mall. Beroende på hello källa i mallen, kan du redan dessa definierats, i så fall flytta toohello nästa steg. 
 
```JSON
   "secCertificateThumbprint": {
      "type": "string",
      "metadata": {
        "description": "Certificate Thumbprint"
      }
    },
    "secCertificateUrlValue": {
      "type": "string",
      "metadata": {
        "description": "Refers toohello location URL in your key vault where hello certificate was uploaded, it is should be in hello format of https://<name of hello vault>.vault.azure.net:443/secrets/<exact location>"
      }
    },

```

**Steg 3:** göra ändringar toohello **Microsoft.ServiceFabric/clusters** resurs, leta upp hello ”Microsoft.ServiceFabric/clusters” resursdefinitionen i mallen. Under Egenskaper för den definitionen av du hittar ”certifikat” JSON-tagg som ska se ut ungefär hello följande JSON-fragment:

   
```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('certificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 

Lägg till en ny tagg ”thumbprintSecondary” och ge det värdet ”[parameters('secCertificateThumbprint')]”.  

Nu hello resursdefinitionen bör se ut hello följande (beroende på din källan för hello mallen det kanske inte precis som hello kodfragmentet nedan). 

```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('certificateThumbprint')]",
          "thumbprintSecondary": "[parameters('secCertificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 

Om du vill använda för**förnyelse hello cert**, ange hello nya certifikat som primär och flytta hello aktuella primära som sekundär. Detta resulterar i hello förnyelsen av det aktuella certifikat för primär toohello nya certifikatet i ett steg i distributionen.

```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('secCertificateThumbprint')]",
          "thumbprintSecondary": "[parameters('certificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 


**Steg 4:** göra ändringar för**alla** hello **Microsoft.Compute/virtualMachineScaleSets** resursdefinitionerna - hitta hello Microsoft.Compute/virtualMachineScaleSets resurs definition. Rulla toohello ”publisher”: ”Microsoft.Azure.ServiceFabric” under ”virtualMachineProfile”.

Du bör se ut så här i hello service fabric publisher inställningar.

![Json_Pub_Setting1][Json_Pub_Setting1]

Lägg till hello nya cert poster tooit

```JSON
               "certificateSecondary": {
                    "thumbprint": "[parameters('secCertificateThumbprint')]",
                    "x509StoreName": "[parameters('certificateStoreValue')]"
                    }
                  },

```

hello egenskaper bör nu se ut så här

![Json_Pub_Setting2][Json_Pub_Setting2]

Om du vill använda för**förnyelse hello cert**, ange hello nya certifikat som primär och flytta hello aktuella primära som sekundär. Detta resulterar i hello förnyelsen av det aktuella certifikat toohello nya certifikatet i ett steg i distributionen. 


```JSON
               "certificate": {
                   "thumbprint": "[parameters('secCertificateThumbprint')]",
                   "x509StoreName": "[parameters('certificateStoreValue')]"
                     },
               "certificateSecondary": {
                    "thumbprint": "[parameters('certificateThumbprint')]",
                    "x509StoreName": "[parameters('certificateStoreValue')]"
                    }
                  },

```
hello egenskaper bör nu se ut så här

![Json_Pub_Setting3][Json_Pub_Setting3]


**Steg 5:** göra ändringar för**alla** hello **Microsoft.Compute/virtualMachineScaleSets** resursdefinitionerna - hitta hello Microsoft.Compute/virtualMachineScaleSets resurs definition. Rulla toohello ”vaultCertificates”:, under ”OSProfile”. Det bör se ut ungefär så här.


![Json_Pub_Setting4][Json_Pub_Setting4]

Lägg till hello secCertificateUrlValue tooit. Använd följande kodavsnitt hello:

```Json
                  {
                    "certificateStore": "[parameters('certificateStoreValue')]",
                    "certificateUrl": "[parameters('secCertificateUrlValue')]"
                  }

```
Nu hello resulterande Json bör se ut ungefär så här.
![Json_Pub_Setting5][Json_Pub_Setting5]


> [!NOTE]
> Kontrollera att du har upprepas steg 4 och 5 för alla hello Nodetypes/Microsoft.Compute/virtualMachineScaleSets resursdefinitionerna i mallen. Om du missar någon av dem hello certifikat installeras inte på den VMSS och du måste oväntade resultat i klustret, inklusive hello klustret gå (om du får inga giltiga certifikat hello klustret kan använda för säkerhet. Så kontrollera double, innan du fortsätter.
> 
> 


### <a name="edit-your-template-file-tooreflect-hello-new-parameters-you-added-above"></a>Redigera din fil tooreflect hello nya mallparametrar du lades till ovan
Om du använder hello exempel hello [git-lagringsplatsen](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) toofollow tillsammans, kan du starta toomake ändringar i hello exempel 5-VM-1-nodetypes får-Secure.paramters_Step2.JSON 

Redigera Resource Manager-mall parametern filen, lägga till hello två nya parametrar för secCertificateThumbprint och secCertificateUrlValue. 

```JSON
    "secCertificateThumbprint": {
      "value": "thumbprint value"
    },
    "secCertificateUrlValue": {
      "value": "Refers toohello location URL in your key vault where hello certificate was uploaded, it is should be in hello format of https://<name of hello vault>.vault.azure.net:443/secrets/<exact location>"
     },

```

### <a name="deploy-hello-template-tooazure"></a>Distribuera hello mall tooAzure

- Du är nu redo toodeploy mall-tooAzure. Öppna en kommandotolk för Azure PS version 1 +.
- Logga in tooyour Azure-konto och välj hello specifika azure-prenumeration. Det här är ett viktigt steg för avdelningen som har åtkomst toomore än en azure-prenumeration.

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId <Subcription ID> 

```

Testa hello mallen tidigare toodeploying den. Använd hello samma resursgrupp som klustret för närvarande har distribuerats till.

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>

```

Distribuera hello mallen tooyour resursgruppen. Använd hello samma resursgrupp som klustret för närvarande har distribuerats till. Kör hello ny AzureRmResourceGroupDeployment kommando. Du behöver inte toospecify hello-läge eftersom hello standardvärdet är **inkrementella**.

> [!NOTE]
> Om du ställer in läget tooComplete du av misstag tar bort resurser som inte finns i mallen. Därför inte använda den i det här scenariot.
> 
> 

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>
```

Här är en fylld ut exempel på hello samma powershell.

```powershell
$ResouceGroup2 = "chackosecure5"
$TemplateFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure_Step2.json"
$TemplateParmFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure.parameters_Step2.json"

New-AzureRmResourceGroupDeployment -ResourceGroupName $ResouceGroup2 -TemplateParameterFile $TemplateParmFile -TemplateUri $TemplateFile -clusterName $ResouceGroup2

```

När hello distributionen är klar kan ansluta med tooyour hello nytt certifikat och köra några frågor. Om du kan toodo. Du kan ta bort hello gammalt certifikat. 

Om du använder ett självsignerat certifikat Glöm inte tooimport dem i din lokala certifikatarkivet för TrustedPeople.

```powershell
######## Set up hello certs on your local box
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)

```
Här är hello kommandot tooconnect tooa säker kluster för Snabbreferens 

```powershell
$ClusterName= "chackosecure5.westus.cloudapp.azure.com:19000"
$CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7SD1D630F8F3" 

Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $CertThumbprint  `
    -FindType FindByThumbprint `
    -FindValue $CertThumbprint `
    -StoreLocation CurrentUser `
    -StoreName My
```
Här är hello kommandot tooget klustret hälsa för Snabbreferens

```powershell
Get-ServiceFabricClusterHealth 
```

## <a name="deploying-application-certificates-toohello-cluster"></a>Distribuera programmet certifikat toohello kluster.

Du kan använda hello samma steg som beskrivs i steg 5 ovan toohave hello certifikat distribueras från en keyvault toohello noder. Du behöver bara ange och använda olika parametrar.


## <a name="adding-or-removing-client-certificates"></a>Tillägg eller borttagning av klientcertifikat

Du kan lägga till certifikat klienten tooperform hanteringsåtgärder på ett service fabric-kluster i tillägg toohello klustret certifikat.

Du kan lägga till två typer av certifikat - administratör eller skrivskyddat. Dessa kan sedan vara används toocontrol åtkomståtgärder toohello administratör och fråga åtgärder på hello klustret. Som standard läggs hello klustercertifikat toohello Admin certifikat listan över tillåtna.

Du kan ange valfritt antal klientcertifikat. Varje tillägg/borttagning resulterar i en konfiguration uppdatering toohello service fabric-kluster


### <a name="adding-client-certificates---admin-or-read-only-via-portal"></a>Lägga till klientcertifikat - administratör eller skrivskyddad via portalen

1. Navigera toohello säkerhet bladet och markera hello ”+ autentisering” knappen ovanpå hello säkerhet bladet.
2. På bladet Lägg till autentisering hello Välj hello 'Autentiseringstyp' - 'Skrivskyddad client' eller 'Admin client'
3. Nu välja hello auktoriseringsmetod. Detta anger tooService Fabric om det ska sökas upp det här certifikatet med hjälp av hello ämnesnamn eller hello tumavtryck. Det är i allmänhet inte en god praxis toouse hello auktorisering säkerhetsmetod Ämnesnamnets. 

![Lägg till klientcertifikat][Add_Client_Cert]

### <a name="deletion-of-client-certificates---admin-or-read-only-using-hello-portal"></a>Borttagning av klientcertifikat - administratör eller med hjälp av skrivskyddad hello portal

tooremove ett sekundärt certifikat används för klustret säkerhet, analysera toohello säkerhet bladet och väljer hello 'Radera' alternativet hello snabbmenyn på hello specifika certifikat.



## <a name="next-steps"></a>Nästa steg
Läs artiklarna mer information om hantering av kluster:

* [Uppgraderingsprocessen för Service Fabric-kluster och förväntningar från dig](service-fabric-cluster-upgrade.md)
* [Konfigurera rollbaserad åtkomst för klienter](service-fabric-cluster-security-roles.md)

<!--Image references-->
[Delete_Swap_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_09.PNG
[Add_Client_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_13.PNG
[Json_Pub_Setting1]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_14.PNG
[Json_Pub_Setting2]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_15.PNG
[Json_Pub_Setting3]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_16.PNG
[Json_Pub_Setting4]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_17.PNG
[Json_Pub_Setting5]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_18.PNG


