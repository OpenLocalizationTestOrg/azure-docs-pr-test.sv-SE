---
title: "aaaCreate ett Azure Service Fabric-kluster från en mall | Microsoft Docs"
description: "Den här artikeln beskriver hur tooset upp en säker Service Fabric-kluster i Azure med Azure Resource Manager, Azure Key Vault och Azure Active Directory (Azure AD) för klientautentisering."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2017
ms.author: chackdan
ms.openlocfilehash: a4563c58a68127720a8290c3be0df9d026833eb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-fabric-cluster-by-using-azure-resource-manager"></a>Skapa ett Service Fabric-kluster med hjälp av Azure Resource Manager
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure Portal](service-fabric-cluster-creation-via-portal.md)
>
>

Den här stegvisa guiden vägleder dig genom att skapa en säker Azure Service Fabric-kluster i Azure med hjälp av Azure Resource Manager. Vi bekräftar den hello artikeln är långt. Dock om du redan är noggrant bekant med hello innehåll vara säker på att toofollow varje steg noggrant.

hello handboken innehåller hello följande procedurer:

* Skapa ett Azure key vault tooupload certifikat för kluster- och säkerhet
* Skapar en skyddad kluster i Azure med hjälp av Azure Resource Manager
* Autentisering av användare med hjälp av Azure Active Directory (Azure AD) för hantering av kluster

En säker klustret är ett kluster som förhindrar obehörig åtkomst toomanagement åtgärder. Detta inkluderar att distribuera, uppgradering, och ta bort program, tjänster och hello data de innehåller. Ett oskyddat klustret är ett kluster alla kan ansluta tooat helst och utföra hanteringsåtgärder. Men det är möjligt toocreate ett oskyddade kluster, rekommenderar vi starkt att du skapar en säker kluster från hello början. Eftersom ett oskyddade kluster inte kan säkras senare, måste du skapa ett nytt kluster.

hello begreppet skapa skyddade kluster är hello samma, oavsett om de är Linux eller Windows-kluster. Mer information och hjälp skript för att skapa säkra Linux-kluster, se [att skapa skyddade kluster på Linux](#secure-linux-clusters).

## <a name="sign-in-tooyour-azure-account"></a>Logga in tooyour Azure-konto
Den här guiden använder [Azure PowerShell][azure-powershell]. När du startar en ny PowerShell-session, logga in tooyour Azure-konto och välja din prenumeration innan du kan köra kommandon för Azure.

Logga in tooyour Azure-konto:

```powershell
Login-AzureRmAccount
```

Välj din prenumeration:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-a-key-vault"></a>Ställ in ett nyckelvalv
Det här avsnittet beskrivs skapa nyckelvalvet för Service Fabric-kluster i Azure och för Service Fabric-program. Fullständig guide-tooAzure Key Vault finns toohello [Key Vault Kom igång med][key-vault-get-started].

Service Fabric använder X.509-certifikat toosecure ett kluster och säkerhetsfunktioner för programmet. Du använder Key Vault toomanage certifikat för Service Fabric-kluster i Azure. När ett kluster distribueras i Azure hämtar certifikat från Nyckelvalvet hello Azure-resursprovider som ansvarar för att skapa Service Fabric-kluster och installerar dem på hello klustrets virtuella datorer.

hello illustrerar följande diagram hello förhållandet mellan Azure Key Vault, Service Fabric-kluster och hello Azure-resursprovider som använder certifikat som lagras i en nyckelvalvet när den skapar ett kluster:

![Diagram över certifikatinstallation][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a>Skapa en resursgrupp
hello första steget är toocreate en resursgrupp för nyckelvalvet. Vi rekommenderar att du placerar hello nyckelvalv i sin egen resursgrupp. Den här åtgärden kan du ta bort hello beräkning och lagring resursgrupper, inklusive hello resursgruppen som innehåller Service Fabric-klustret utan att förlora dina nycklar och hemligheter. hello resursgruppen som innehåller nyckelvalvet _hello måste ha samma region_ som hello-kluster som använder den.

Om du planerar toodeploy kluster i flera områden, föreslår vi att du namnger hello resursgrupp och hello nyckelvalv på ett sätt som anger vilken region som den tillhör.  

```powershell

    New-AzureRmResourceGroup -Name westus-mykeyvault -Location 'West US'
```
hello utdata ska se ut så här:

```powershell

    WARNING: hello output object type of this cmdlet is going toobe modified in a future release.

    ResourceGroupName : westus-mykeyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/westus-mykeyvault

```
<a id="new-key-vault"></a>

### <a name="create-a-key-vault-in-hello-new-resource-group"></a>Skapa en nyckelvalvet i hello ny resursgrupp
Hej nyckelvalv _måste vara aktiverat för distribution_ tooallow hello beräkning resource provider tooget certifikat från den och installera det på instanser för virtuella datorer:

```powershell

    New-AzureRmKeyVault -VaultName 'mywestusvault' -ResourceGroupName 'westus-mykeyvault' -Location 'West US' -EnabledForDeployment

```

hello utdata ska se ut så här:

```powershell

    Vault Name                       : mywestusvault
    Resource Group Name              : westus-mykeyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/westus-mykeyvault/providers/Microsoft.KeyVault/vaults/mywestusvault
    Vault URI                        : https://mywestusvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions tooKeys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions tooSecrets   :    all


    Tags                             :
```
<a id="existing-key-vault"></a>

## <a name="use-an-existing-key-vault"></a>Använd en befintlig nyckelvalv

toouse en befintlig nyckelvalv du _måste aktivera den för distribution_ tooallow hello beräkning resource provider tooget certifikat från den och installera den på klusternoder:

```powershell

Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

```

<a id="add-certificate-to-key-vault"></a>

## <a name="add-certificates-tooyour-key-vault"></a>Lägg till certifikat tooyour nyckelvalv

Certifikat används i Service Fabric tooprovide autentisering och kryptering toosecure olika aspekter av ett kluster och dess program. Mer information om hur du använder certifikat i Service Fabric finns [säkerhetsscenarier för Service Fabric-klustret][service-fabric-cluster-security].

### <a name="cluster-and-server-certificate-required"></a>Kluster- och certifikat (krävs)
Det här certifikatet är obligatoriska toosecure ett kluster och förhindra obehörig åtkomst tooit. Det ger säkerhet i klustret på två sätt:

* Autentisering för klustret: autentiserar nod till nod kommunikation för klustret. Endast noder som kan bevisa sin identitet med det här certifikatet kan gå hello klustret.
* Serverautentisering: autentiserar hello cluster management slutpunkter tooa management-klienten, så att hello Hanteringsklient vet pratar toohello verkliga klustret. Det här certifikatet ger också en SSL för hello HTTPS hanterings-API och för Service Fabric Explorer via HTTPS.

tooserve dessa ändamål hello certifikat måste uppfylla följande krav hello:

* hello certifikatet måste innehålla en privat nyckel.
* hello certifikat måste skapas för nyckelutbyte, som kan exporteras tooa Personal Information Exchange (.pfx)-fil.
* hello certifikatets ämnesnamn måste matcha hello-domän som du använder tooaccess hello Service Fabric-klustret. Den här matchning är obligatoriska tooprovide en SSL för hello klustrets HTTPS management slutpunkter och Service Fabric Explorer. Du kan skaffa ett SSL-certifikat från en certifikatutfärdare (CA) för hello. cloudapp.azure.com domän. Du måste skaffa ett anpassat domännamn för klustret. När du begär ett certifikat från en Certifikatutfärdare måste hello certifikatets ämnesnamn matcha hello domännamn som du använder för klustret.

### <a name="application-certificates-optional"></a>Certifikat för programmet (valfritt)
Valfritt antal ytterligare certifikat kan installeras på ett kluster av säkerhetsskäl för programmet. Tänk hello applikationssäkerhets-scenarier som kräver ett certifikat toobe som har installerats på hello noder innan du skapar klustret:

* Kryptering och dekryptering av konfigurationsvärden för programmet.
* Kryptering av data mellan noderna under replikeringen.

### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Formatering certifikat för användning av Azure resource provider
Du kan lägga till och använda privata nyckelfiler (.pfx) direkt via nyckelvalvet. Hello Azure compute-resursprovidern kräver dock nycklar toobe lagras i ett specialformat JavaScript Object Notation (JSON). hello-formatet innehåller hello .pfx-fil som en base 64-kodad sträng och lösenordet för hello privata nyckeln. tooaccommodate kraven hello nycklar måste placeras i en JSON-sträng och sedan lagras som ”hemligheter” i hello nyckeln valvet.

toomake detta enklare och bearbeta en [PowerShell-modulen finns på GitHub][service-fabric-rp-helpers]. toouse hello modulen hello följande:

1. Hämta hello hela innehållet i hello lagringsplatsen till en lokal katalog.
2. Gå toohello lokal katalog.
2. Importera hello ServiceFabricRPHelpers modul i PowerShell-fönstret:

```powershell

 Import-Module "C:\..\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

```

Hej `Invoke-AddCertToKeyVault` kommandot i PowerShell-modulen automatiskt formaterar en certifikatets privata nyckel till en JSON-sträng och överförs toohello nyckelvalvet. Använd hello kommandot tooadd hello klustret certifikat och nyckelvalv några ytterligare certifikat toohello. Upprepa det här steget för några ytterligare certifikat som du vill använda tooinstall i klustret.

#### <a name="uploading-an-existing-certificate"></a>Ladda upp ett befintligt certifikat

```powershell

 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName westus-mykeyvault -Location "West US" -VaultName mywestusvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

```

Om du får ett felmeddelande, till exempel hello som visas här är oftast det att det finns en konflikt för resurs-URL. tooresolve hello konflikt, ändra hello nyckelvalv namn.

```
Set-AzureKeyVaultSecret : hello remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

När hello konflikten har lösts hello utdata ska se ut så här:

```

    Switching context tooSubscriptionId <guid>
    Ensuring ResourceGroup westus-mykeyvault in West US
    WARNING: hello output object type of this cmdlet is going toobe modified in a future release.
    Using existing value mywestusvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret toomywestusvault in vault mywestusvault


Name  : CertificateThumbprint
Value : E21DBC64B183B5BF355C34C46E03409FEEAEF58D

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/westus-mykeyvault/providers/Microsoft.KeyVault/vaults/mywestusvault

Name  : CertificateURL
Value : https://mywestusvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

>[!NOTE]
>Du måste hello tre föregående strängar, CertificateThumbprint och SourceVault CertificateURL tooset av säker Service Fabric-kluster och tooobtain programcertifikat som du kan använda för programsäkerhet. Om du inte sparar hello strängar kan vara det svårt tooretrieve dem genom att fråga hello nyckeln valvet senare.

<a id="add-self-signed-certificate-to-key-vault"></a>

#### <a name="creating-a-self-signed-certificate-and-uploading-it-toohello-key-vault"></a>Skapa ett självsignerat certifikat och överföra den toohello nyckelvalv

Hoppa över det här steget om du redan har överfört certifikat toohello nyckelvalvet. Det här steget är för att generera ett nytt självsignerat certifikat och överföra den tooyour nyckelvalvet. När du ändrar hello parametrar i hello följande skript och kör sedan det ska efterfrågas ett lösenord för certifikatet.  

```powershell

$ResourceGroup = "chackowestuskv"
$VName = "chackokv2"
$SubID = "6c653126-e4ba-42cd-a1dd-f7bf96ae7a47"
$locationRegion = "westus"
$newCertName = "chackotestcertificate1"
$dnsName = "www.mycluster.westus.mydomain.com" #hello certificate's subject name must match hello domain used tooaccess hello Service Fabric cluster.
$localCertPath = "C:\MyCertificates" # location where you want hello .PFX toobe stored

 Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResourceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath

```

Om du får ett felmeddelande, till exempel hello som visas här är oftast det att det finns en konflikt för resurs-URL. tooresolve hello konflikt, ändra hello nyckelvalv namn, RG namn och så vidare.

```
Set-AzureKeyVaultSecret : hello remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

När hello konflikten har lösts hello utdata ska se ut så här:

```
PS C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers> Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResouceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -Password $certPassword -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath
Switching context tooSubscriptionId 6c343126-e4ba-52cd-a1dd-f8bf96ae7a47
Ensuring ResourceGroup chackowestuskv in westus
WARNING: hello output object type of this cmdlet will be modified in a future release.
Creating new vault westuskv1 in westus
Creating new self signed certificate at C:\MyCertificates\chackonewcertificate1.pfx
Reading pfx file from C:\MyCertificates\chackonewcertificate1.pfx
Writing secret toochackonewcertificate1 in vault westuskv1


Name  : CertificateThumbprint
Value : 96BB3CC234F9D43C25D4B547sd8DE7B569F413EE

Name  : SourceVault
Value : /subscriptions/6c653126-e4ba-52cd-a1dd-f8bf96ae7a47/resourceGroups/chackowestuskv/providers/Microsoft.KeyVault/vaults/westuskv1

Name  : CertificateURL
Value : https://westuskv1.vault.azure.net:443/secrets/chackonewcertificate1/ee247291e45d405b8c8bbf81782d12bd

```

>[!NOTE]
>Du måste hello tre föregående strängar, CertificateThumbprint och SourceVault CertificateURL tooset av säker Service Fabric-kluster och tooobtain programcertifikat som du kan använda för programsäkerhet. Om du inte sparar hello strängar kan vara det svårt tooretrieve dem genom att fråga hello nyckeln valvet senare.

 Du bör nu ha hello följande element på plats:

* Hej nyckelvalv resursgrupp.
* Hej nyckelvalvet och Webbadressen (kallas SourceVault i hello föregående PowerShell-utdata).
* hello klustret certifikat för serverautentisering och URL: en i hello nyckelvalvet.
* hello programmet certifikat och deras URL: er i hello nyckelvalvet.


<a id="add-AAD-for-client"></a>

## <a name="set-up-azure-active-directory-for-client-authentication"></a>Konfigurera Azure Active Directory för klientautentisering

Azure AD kan organisationer (kallas klienter) toomanage användaren åtkomst tooapplications. Program är indelade i dem med en webbaserad inloggning användargränssnitt och de med inbyggda klientupplevelsen. I den här artikeln förutsätter vi att du redan har skapat en klient. Om du inte har börja med att läsa [hur tooget ett Azure Active Directory-klient][active-directory-howto-tenant].

Ett Service Fabric-kluster erbjuder flera posten pekar tooits hanteringsfunktioner, inklusive hello webbaserade [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] och [Visual Studio] [service-fabric-manage-application-in-visual-studio]. Därför kan skapa du två Azure AD-program toocontrol åtkomst toohello klustret, ett webbprogram och en programspecifika.

toosimplify några av hello stegen som är involverad i Konfigurera Azure AD med ett Service Fabric-kluster, vi har skapat en uppsättning Windows PowerShell-skript.

> [!NOTE]
> Du måste slutföra följande steg innan du skapar klustret hello hello. Eftersom hello skript räknar klusternamn och slutpunkter, bör planeras hello värden och värden inte som du redan har skapat.

1. [Hämta hello skript] [ sf-aad-ps-script-download] tooyour dator.
2. Högerklicka på hello zip-filen, Välj **egenskaper**väljer hello **avblockera** kryssrutan och klicka sedan på **tillämpa**.
3. Extrahera hello zip-filen.
4. Kör `SetupApplications.ps1`, och ange hello TenantId, klusternamn och WebApplicationReplyUrl som parametrar. Exempel:

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    Du kan hitta din TenantId genom att köra PowerShell-kommando för hello `Get-AzureSubscription`. Kör det här kommandot visar hello TenantId för varje prenumeration.

    Klusternamn är används tooprefix hello Azure AD-program som skapats av hello-skriptet. Det behöver inte toomatch hello faktiska klusternamnet exakt. Det är avsedda enbart toomake den enklare toomap Azure AD-artefakter toohello Service Fabric-kluster som de som används med.

    WebApplicationReplyUrl är hello standardslutpunkt som Azure AD returnerar tooyour användare när de har loggat in. Ange den här slutpunkten som hello Service Fabric Explorer slutpunkt för klustret, vilket som standard är:

    https://&lt;cluster_domain&gt;: 19080/Explorer

    Du kan ange toosign i tooan konto som har administratörsbehörighet för hello Azure AD-klient. När du loggar in hello skriptet skapar hello webb- och interna program toorepresent Service Fabric-klustret. Om du tittar på hello klient program i hello [klassiska Azure-portalen][azure-classic-portal], bör du se två nya poster:

   * *Klusternamn*\_kluster
   * *Klusternamn*\_klienten

   hello skript skrivs hello JSON som krävs av hello Azure Resource Manager-mall när du skapar hello kluster i hello nästa avsnitt, så det är en bra idé tookeep hello PowerShell-fönstret öppnas.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Skapa en mall för Service Fabric-kluster Resource Manager
I det här avsnittet hello matar ut av hello föregående PowerShell-kommandon som används i en Service Fabric-kluster Resource Manager-mall.

Exempel Resource Manager-mallar är tillgängliga i hello [Azure Snabbkurs mallgalleriet på GitHub][azure-quickstart-templates]. Dessa mallar kan användas som en startpunkt för mallen för klustret.

### <a name="create-hello-resource-manager-template"></a>Skapa hello Resource Manager-mall
Den här guiden använder hello [säker kluster med 5] [ service-fabric-secure-cluster-5-node-1-nodetype] exempel mallen och mallparametrar. Hämta `azuredeploy.json` och `azuredeploy.parameters.json` tooyour datorn och öppna filer i valfri textredigerare.

### <a name="add-certificates"></a>Lägg till certifikat
Du kan lägga till certifikat tooa klustret Resource Manager-mall genom att referera hello nyckelvalv som innehåller hello certifikatnycklar. Vi rekommenderar att du placerar hello nyckelvalvet värden i en parameterfil för Resource Manager-mall. Då behåller hello Resource Manager mallfilen återanvändbara och utan värden specifika tooa distribution.

#### <a name="add-all-certificates-toohello-virtual-machine-scale-set-osprofile"></a>Lägg till alla certifikat toohello virtuella skaluppsättning osProfile
Alla certifikat som har installerats i hello klustret måste konfigureras under hello osProfile i hello scale set resurs (Microsoft.Compute/virtualMachineScaleSets). Den här åtgärden instruerar hello resource provider tooinstall hello certifikatet på hello virtuella datorer. Den här installationen innehåller både hello klustret certifikat och några security-certifikat för programmet som du planerar toouse för dina program:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

#### <a name="configure-hello-service-fabric-cluster-certificate"></a>Konfigurera certifikat för hello Service Fabric-kluster
hello autentiseringscertifikat för klustret måste konfigureras i båda hello Service Fabric-klusterresursen (Microsoft.ServiceFabric/clusters) och hello Service Fabric-tillägget för virtuell dator skala anger i hello virtuella scale set datorresurser. Den här ordningen tillåter hello Service Fabric resource provider tooconfigure den som ska användas för autentisering för klustret och serverautentisering för av hanteringsslutpunkter.

##### <a name="virtual-machine-scale-set-resource"></a>Virtuella skaluppsättning för resursen:
```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

##### <a name="service-fabric-resource"></a>Service Fabric-resurs:
```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="insert-azure-ad-configuration"></a>Infoga Azure AD-konfiguration
hello Azure AD-konfiguration som du skapade tidigare kan infogas direkt i Resource Manager-mall. Vi rekommenderar dock att du först extrahera hello värden till en parametrar filen tookeep hello Resource Manager-mall kan återanvändas och utan värden specifika tooa distribution.

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### < en ”konfigurera arm” ></a>konfigurera Hanteraren för filserverresurser mallparametrar
<!--- Loc Comment: It seems that <a "configure-arm" > must be replaced with <a name="configure-arm"></a> since hello link seems not toobe redirecting correctly --->
Använd slutligen hello utdatavärden från hello nyckelvalvet och Azure AD PowerShell-kommandon toopopulate hello parameterfilen:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```
Du bör nu ha hello följande element på plats:

* Nyckelvalv resursgruppen.
  * Nyckelvalv
  * Klustret certifikat för serverautentisering
  * Data chiffrering av certifikat
* Azure Active Directory-klient
  * Azure AD-program för Webbaserad hantering och Service Fabric Explorer
  * Azure AD-program för interna klienthantering
  * Användare och deras tilldelade roller
* Service Fabric-kluster Resource Manager-mall
  * Certifikat konfigureras via nyckelvalv
  * Azure Active Directory konfigurerat

hello följande diagram illustrerar där ditt nyckelvalv och Azure AD-konfiguration passar i Resource Manager-mall.

![Hanteraren för filserverresurser beroendekarta för anslutningar][cluster-security-arm-dependency-map]

## <a name="create-hello-cluster"></a>Skapa hello-kluster
Nu är du redo toocreate hello kluster med hjälp av [Azure-resurs malldistribution][resource-group-template-deploy].

#### <a name="test-it"></a>testa den
Använd följande PowerShell-kommandot tootest hello Resource Manager-mall med en fil med parametrar:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### <a name="deploy-it"></a>distribuera den
Om hello Resource Manager-mall testet lyckas, använder du följande PowerShell-kommandot toodeploy hello Resource Manager-mall med en fil med parametrar:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>

## <a name="assign-users-tooroles"></a>Tilldela användare tooroles
När du har skapat hello program toorepresent klustret, tilldela dina användare toohello roller som stöds av Service Fabric: skrivskyddade och administratör. Du kan tilldela hello roller med hjälp av hello [klassiska Azure-portalen][azure-classic-portal].

1. Gå tooyour klient i hello Azure-portalen, och välj sedan **program**.
2. Välj hello webbprogram som har ett namn som `myTestCluster_Cluster`.
3. Klicka på hello **användare** fliken.
4. Välj en användare tooassign och klicka sedan på hello **tilldela** knappen längst ned hello hello-skärmen.

    ![Tilldela användare tooroles knappen][assign-users-to-roles-button]
5. Välj hello rollen tooassign toohello användare.

    ![”Tilldela användare” dialogrutan][assign-users-to-roles-dialog]

> [!NOTE]
> Mer information om roller i Service Fabric finns [rollbaserad åtkomstkontroll för Service Fabric-klienter](service-fabric-cluster-security-roles.md).
>
>

 <a name="secure-linux-clusters"></a>
 <!--- Loc Comment: It seems that letter S in cluster was missing, which caused hello wrong redirection of hello link --->

## <a name="create-secure-clusters-on-linux"></a>Skapa skyddade kluster på Linux
toomake hello processen enklare, som vi har angett en [helper skriptet](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux). Innan du använder skriptet helper se till att du redan har Azure-kommandoradsgränssnittet (CLI) installerad och den är i din sökväg. Se till att hello skript har behörigheter tooexecute genom att köra `chmod +x cert_helper.py` när du har hämtat den. hello första steget är toosign i tooyour Azure-konto med hjälp av CLI med hello `azure login` kommando. När du har registrerat i tooyour Azure-konto, signerade Använd hello helper skript med Certifikatutfärdaren certifikat, som följande kommando visar hello:

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]
```

hello - ifile parameter kan ta en .pfx-fil eller en PEM-filen som indata med hello certifikattyp (pfx eller pem eller ss om det är ett självsignerat certifikat).
hello parametern -h skriver ut hello hjälptexten.


Det här kommandot returnerar hello följande tre strängar som hello utdata:

* SourceVaultID som är hello-ID för hello nya KeyVault ResourceGroup den skapas automatiskt
* CertificateUrl för att komma åt hello certifikat
* CertificateThumbprint som används för autentisering

hello som följande exempel visar hur toouse hello kommando:

```sh
./cert_helper.py pfx -sub "fffffff-ffff-ffff-ffff-ffffffffffff"  -rgname "mykvrg" -kv "mykevname" -ifile "/home/test/cert.pfx" -sname "mycert" -l "East US" -p "pfxtest"
```
Köra hello före kommandot ger du hello tre strängar på följande sätt:

```sh
SourceVault: /subscriptions/fffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/mykvrg/providers/Microsoft.KeyVault/vaults/mykvname
CertificateUrl: https://myvault.vault.azure.net/secrets/mycert/00000000000000000000000000000000
CertificateThumbprint: 0xfffffffffffffffffffffffffffffffffffffffff
```

hello certifikatets ämnesnamn måste matcha hello-domän som du använder tooaccess hello Service Fabric-klustret. Matchningen är obligatoriska tooprovide en SSL för hello klustrets HTTPS management slutpunkter och Service Fabric Explorer. Du kan skaffa ett SSL-certifikat från en Certifikatutfärdare för hello `.cloudapp.azure.com` domän. Du måste skaffa ett anpassat domännamn för klustret. När du begär ett certifikat från en Certifikatutfärdare måste hello certifikatets ämnesnamn matcha hello domännamn som du använder för klustret.

Dessa ämnesnamn är hello poster du behöver toocreate säker Service Fabric-klustret (utan Azure AD), enligt beskrivningen i [konfigurera Hanteraren för filserverresurser mallparametrar](#configure-arm). Du kan ansluta toohello säker klustret genom att följa hello anvisningar för [autentisera klient åtkomst tooa kluster](service-fabric-connect-to-secure-cluster.md). Linux preview kluster stöder inte Azure AD-autentisering. Du kan tilldela roller admin och klienten enligt beskrivningen i hello [tilldela roller toousers](#assign-roles) avsnitt. När du anger admin och klienten roller för en Linux-kluster i förhandsgranskningen har tooprovide certifikattumavtryck för autentisering. (Du inte anger hello ämnesnamn eftersom ingen verifiering av certifikatkedjan eller återkallade utförs i den här förhandsversionen.)

Om du vill toouse ett självsignerat certifikat för testning, kan du använda hello samma skript toogenerate en. Du kan sedan ladda upp certifikatet hello tooyour nyckelvalv genom att tillhandahålla hello flaggan `ss` i stället för att tillhandahålla hello certifikatets sökväg och certifikatet namn. Se exempelvis hello följande kommando för att skapa och ladda upp ett självsignerat certifikat:

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net"
```
Det här kommandot returnerar hello samma tre strängar: SourceVault, CertificateUrl och CertificateThumbprint. Du kan sedan använda hello strängar toocreate både en säker Linux-kluster och en plats där hello självsignerat certifikat är placerad. Du måste hello självsignerat certifikat tooconnect toohello klustret. Du kan ansluta toohello säker klustret genom att följa hello anvisningar för [autentisera klient åtkomst tooa kluster](service-fabric-connect-to-secure-cluster.md).

hello certifikatets ämnesnamn måste matcha hello-domän som du använder tooaccess hello Service Fabric-klustret. Matchningen är obligatoriska tooprovide en SSL för hello klustrets HTTPS management slutpunkter och Service Fabric Explorer. Du kan skaffa ett SSL-certifikat från en Certifikatutfärdare för hello `.cloudapp.azure.com` domän. Du måste skaffa ett anpassat domännamn för klustret. När du begär ett certifikat från en Certifikatutfärdare måste hello certifikatets ämnesnamn matcha hello domännamn som du använder för klustret.

Du kan fylla hello parametrar från hello helper skriptet i hello Azure-portalen, enligt beskrivningen i hello [skapar ett kluster i hello Azure-portalen](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal) avsnitt.

## <a name="next-steps"></a>Nästa steg
Du har nu en säker kluster med Azure Active Directory med management-autentisering. Nästa [ansluta tooyour klustret](service-fabric-connect-to-secure-cluster.md) och lära dig hur för[hantera program hemligheter](service-fabric-application-secret-management.md).

## <a name="troubleshoot-setting-up-azure-active-directory-for-client-authentication"></a>Felsöka ställa in Azure Active Directory för klientautentisering
Om du stöter på ett problem när du konfigurerar Azure AD för klientautentisering, granska hello möjliga lösningar i det här avsnittet.

### <a name="service-fabric-explorer-prompts-you-tooselect-a-certificate"></a>Service Fabric Explorer uppmanar tooselect ett certifikat
#### <a name="problem"></a>Problem
När du loggar in har tooAzure AD i Service Fabric Explorer hello webbläsare returnerar toohello startsidan men får ett meddelande om tooselect ett certifikat.

![Dialogrutan för SFX Välj certifikat][sfx-select-certificate-dialog]

#### <a name="reason"></a>Orsak
hello användaren inte är tilldelad en roll i hello Azure AD-program för klustret. Azure AD-autentiseringen misslyckas därför på Service Fabric-klustret. Service Fabric Explorer använder toocertificate autentisering.

#### <a name="solution"></a>Lösning
Följ hello instruktioner för att konfigurera Azure AD och tilldela användarroller. Dessutom vi rekommenderar att du aktiverar ”användare tilldelning krävs tooaccess app” som `SetupApplications.ps1` har.

### <a name="connection-with-powershell-fails-with-an-error-hello-specified-credentials-are-invalid"></a>Anslutning med PowerShell misslyckas med felmeddelandet: ”hello angivna autentiseringsuppgifterna är ogiltiga”
#### <a name="problem"></a>Problem
När du använder PowerShell tooconnect toohello kluster med hjälp av ”AzureActiveDirectory” säkerhetsläget när du loggar in har tooAzure AD hello anslutningen misslyckas med felmeddelandet: ”hello angivna autentiseringsuppgifterna är ogiltiga”.

#### <a name="solution"></a>Lösning
Den här lösningen är hello samma som hello före en.

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a>Service Fabric Explorer returnerar ett fel när du loggar in: ”AADSTS50011”
#### <a name="problem"></a>Problem
När du försöker toosign i tooAzure AD i Service Fabric Explorer hello sidan returnerar ett fel ”: AADSTS50011: hello svarsadressen &lt;url&gt; matchar inte hello svar adresser som har konfigurerats för programmet hello: &lt;guid&gt;."

![SFX svarsadressen matchar inte][sfx-reply-address-not-match]

#### <a name="reason"></a>Orsak
försöker hello kluster (webb) program som representerar Service Fabric Explorer tooauthenticate mot Azure AD och som en del av hello begäran ger RETUR hello omdirigerings-URL. Men hello URL visas i hello Azure AD-program **REPLY URL** lista.

#### <a name="solution"></a>Lösning
På hello **konfigurera** för hello (webbprogram) bör du lägga till hello URL för Service Fabric Explorer toohello **REPLY URL** listan eller ersätta en hello objekt i hello lista. När du är klar kan du spara ändringen.

![Url till webbprogram svar][web-application-reply-url]

### <a name="connect-hello-cluster-by-using-azure-ad-authentication-via-powershell"></a>Ansluta hello kluster med hjälp av Azure AD-autentisering via PowerShell
tooconnect hello Service Fabric-kluster använder hello följande exempel för PowerShell-kommando:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

toolearn om hello Connect-ServiceFabricCluster: se [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx).

### <a name="can-i-reuse-hello-same-azure-ad-tenant-in-multiple-clusters"></a>Kan jag återanvända hello samma Azure AD-klient i flera kluster?
Ja. Men kom ihåg tooadd hello URL för Service Fabric Explorer tooyour kluster (webb) program. Annars fungerar Service Fabric Explorer inte.

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a>Varför måste jag göra fortfarande ett servercertifikat medan Azure AD är aktiverad?
FabricClient och FabricGateway utför en ömsesidig autentisering. Integrering med Azure AD tillhandahåller en identitet toohello server under Azure AD-autentisering och hello servercertifikatet är används tooverify hello-serveridentitet. Mer information om Service Fabric-certifikat finns [X.509-certifikat och Service Fabric][x509-certificates-and-service-fabric].

<!-- Links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype/
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[assign-users-to-roles-button]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png

