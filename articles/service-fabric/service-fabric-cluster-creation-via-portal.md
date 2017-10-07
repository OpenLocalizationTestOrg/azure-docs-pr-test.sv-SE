---
title: aaaCreate Service Fabric-klustret i hello Azure-portalen | Microsoft Docs
description: "Den här artikeln beskriver hur tooset upp en säker Service Fabric-kluster i Azure med hjälp av hello Azure-portalen och Azure Key Vault."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: vturecek
ms.assetid: 426c3d13-127a-49eb-a54c-6bde7c87a83b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/21/2017
ms.author: chackdan
ms.openlocfilehash: 045f71b491260e741ce7a54a75c440e1b33059a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-fabric-cluster-in-azure-using-hello-azure-portal"></a>Skapa ett Service Fabric-kluster i Azure med hjälp av hello Azure-portalen
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure Portal](service-fabric-cluster-creation-via-portal.md)
> 
> 

Det här är en stegvis guide som vägleder dig genom hello stegen för att skapa en säker Service Fabric-kluster i Azure med hjälp av hello Azure-portalen. Den här guiden vägleder dig genom hello följande steg:

* Ställ in Key Vault toomanage nycklar för säkerhet för klustret.
* Skapa ett skyddat kluster i Azure via hello Azure-portalen.
* Autentisera administratörer som använder certifikat.

> [!NOTE]
> För mer avancerade säkerhetsalternativ, till exempel autentisering med Azure Active Directory och hur du konfigurerar certifikat för programmet säkerhet [skapar ett kluster med Azure Resource Manager][create-cluster-arm].
> 
> 

En säker klustret är ett kluster som förhindrar obehörig åtkomst toomanagement åtgärder, vilket innefattar distribution, uppgradera och ta bort program, tjänster och hello data de innehåller. Ett oskyddat klustret är ett kluster alla kan ansluta tooat helst och utföra hanteringsåtgärder. Men det är möjligt toocreate ett oskyddade kluster, det är **rekommenderar toocreate säker klustret**. Ett oskyddade kluster **går inte att senare säkra** -måste du skapa ett nytt kluster.

hello begrepp är hello samma för att skapa skyddade kluster, oavsett om hello kluster är Linux-kluster eller Windows-kluster. Mer information och hjälp skript för att skapa säkra Linux-kluster, se [att skapa skyddade kluster på Linux](service-fabric-cluster-creation-via-arm.md#secure-linux-clusters). hello parametrar som erhålls genom hello helper skriptet kan vara in direkt hello portal enligt beskrivningen i avsnittet hello [skapar ett kluster i hello Azure-portalen](#create-cluster-portal).

## <a name="log-in-tooazure"></a>Logga in tooAzure
Den här guiden använder [Azure PowerShell][azure-powershell]. När du startar en ny PowerShell-session, logga in tooyour Azure-konto och välja din prenumeration innan du kör kommandon för Azure.

Logga in tooyour azure-konto:

```powershell
Login-AzureRmAccount
```

Välj din prenumeration:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-key-vault"></a>Konfigurera Key Vault
Den här delen av hello guiden vägleder dig genom att skapa ett Nyckelvalv för Service Fabric-kluster i Azure och för Service Fabric-program. En fullständig guide på Key Vault finns hello [Key Vault Kom igång med][key-vault-get-started].

Service Fabric använder X.509-certifikat toosecure ett kluster. Azure Key Vault är används toomanage certifikat för Service Fabric-kluster i Azure. När ett kluster distribueras i Azure hämtar certifikat från Nyckelvalvet hello Azure-resursprovider ansvarar för att skapa Service Fabric-kluster och installerar dem på hello klustrets virtuella datorer.

hello illustrerar följande diagram hello förhållandet mellan Nyckelvalvet, Service Fabric-kluster och hello Azure-resursprovider som använder certifikat som lagras i Nyckelvalvet när den skapar ett kluster:

![Certifikatinstallation][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a>Skapa en resursgrupp
hello första steget är toocreate en ny resursgrupp specifikt för Nyckelvalvet. Placera Nyckelvalvet i sin egen resursgruppen rekommenderas så att du kan ta bort beräkning och lagring resursgrupper – exempelvis hello resursgrupp som har Service Fabric-kluster – utan att förlora dina nycklar och hemligheter. hello resursgruppen som innehåller ditt Nyckelvalv måste vara i hello samma region som hello-kluster som använder den.

```powershell

    PS C:\Users\vturecek> New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: hello output object type of this cmdlet will be modified in a future release.

    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

### <a name="create-key-vault"></a>Skapa Nyckelvalvet
Skapa ett Nyckelvalv i hello ny resursgrupp. hello Key Vault **måste vara aktiverat för distribution** tooallow hello Service Fabric resource provider tooget certifikat från den och installera på klusternoder:

```powershell

    PS C:\Users\vturecek> New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment


    Vault Name                       : myvault
    Resource Group Name              : mycluster-keyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault
    Vault URI                        : https://myvault.vault.azure.net
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

Om du har en befintlig Key Vault kan aktivera du den för distribution med Azure CLI:

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```


## <a name="add-certificates-tookey-vault"></a>Lägg till certifikat tooKey valvet
Certifikat används i Service Fabric tooprovide autentisering och kryptering toosecure olika aspekter av ett kluster och dess program. Mer information om hur du använder certifikat i Service Fabric finns [säkerhetsscenarier för Service Fabric-klustret][service-fabric-cluster-security].

### <a name="cluster-and-server-certificate-required"></a>Kluster- och certifikat (krävs)
Det här certifikatet är obligatoriska toosecure ett kluster och förhindra obehörig åtkomst tooit. Det ger säkerhet i klustret på ett par olika sätt:

* **Autentisering för klustret:** autentiserar nod till nod kommunikation för klustret. Endast noder som kan bevisa sin identitet med det här certifikatet kan gå hello klustret.
* **Serverautentisering:** autentiserar hello cluster management slutpunkter tooa management-klienten, så att hello Hanteringsklient vet pratar toohello verkliga klustret. Det här certifikatet ger också SSL för hello HTTPS hanterings-API och för Service Fabric Explorer via HTTPS.

tooserve dessa ändamål hello certifikat måste uppfylla följande krav hello:

* hello certifikatet måste innehålla en privat nyckel.
* hello certifikat måste skapas för nyckelutbyte, exportera tooa Personal Information Exchange (.pfx)-fil.
* hello måste certifikatets ämnesnamn matcha hello domän används tooaccess hello Service Fabric-klustret. Detta är obligatorisk tooprovide SSL för hello klustrets HTTPS management slutpunkter och Service Fabric Explorer. Du kan skaffa ett SSL-certifikat från en certifikatutfärdare (CA) för hello `.cloudapp.azure.com` domän. Hämta ett anpassat domännamn för klustret. När du begär måste ett certifikat från en Certifikatutfärdare hello certifikatets ämnesnamn matcha hello domännamn som används för klustret.

### <a name="client-authentication-certificates"></a>Certifikat för klientautentisering
Ytterligare klientcertifikat autentisera administratörer för hanteringsuppgifter för klustret. Service Fabric har två åtkomstnivåer: **admin** och **skrivskyddade användaren**. Minst ska ett enda certifikat för administrativ åtkomst användas. För ytterligare användarrättigheter, måste ett separat certifikat tillhandahållas. Mer information om åtkomst roller finns [rollbaserad åtkomstkontroll för Service Fabric-klienter][service-fabric-cluster-security-roles].

Du behöver inte tooupload klienten autentisering certifikat tooKey valvet toowork med Service Fabric. Dessa certifikat behöver bara toobe angetts toousers som har behörighet för hantering av kluster. 

> [!NOTE]
> Azure Active Directory är hello rekommenderade sätt tooauthenticate klienter för klustret hanteringsåtgärder. toouse Azure Active Directory, måste du [skapa ett kluster med Azure Resource Manager][create-cluster-arm].
> 
> 

### <a name="application-certificates-optional"></a>Certifikat för programmet (valfritt)
Valfritt antal ytterligare certifikat kan installeras på ett kluster av säkerhetsskäl för programmet. Tänk hello applikationssäkerhets-scenarier som kräver ett certifikat toobe som har installerats på hello noder innan du skapar klustret:

* Kryptering och dekryptering av konfigurationsvärden för programmet
* Kryptering av data mellan noderna vid replikering 

Certifikat för programmet kan inte konfigureras när du skapar ett kluster via hello Azure-portalen. tooconfigure programcertifikat vid klustret konfigurationen, måste du [skapa ett kluster med Azure Resource Manager][create-cluster-arm]. Du kan också lägga till programmet certifikat toohello klustret när den har skapats.

### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Formatering certifikat för användning av Azure resource provider
Privata nyckelfiler (.pfx) kan läggas till och användas direkt via Nyckelvalvet. Hello Azure-resursprovider kräver dock nycklar toobe lagras i en särskild JSON-format som innehåller hello .pfx som en Base64-kodad sträng och hello lösenordet privata nyckeln. tooaccommodate kraven nycklar måste placeras i en JSON-sträng och sedan lagras som *hemligheter* i Nyckelvalvet.

toomake detta bearbeta lättare, en PowerShell-modul är [finns på GitHub][service-fabric-rp-helpers]. Följ dessa steg toouse hello-modulen:

1. Hämta hello hela innehållet i hello lagringsplatsen till en lokal katalog. 
2. Importera hello-modul i PowerShell-fönstret:

```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
```

Hej `Invoke-AddCertToKeyVault` kommandot i PowerShell-modulen automatiskt formaterar en certifikatets privata nyckel till en JSON-sträng och överförs tooKey valvet. Använda den tooadd hello klustret certifikat och några ytterligare certifikat tooKey valvet. Upprepa det här steget för några ytterligare certifikat som du vill använda tooinstall i klustret.

```powershell
PS C:\Users\vturecek> Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

    Switching context tooSubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: hello output object type of this cmdlet will be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret toomyvault in vault myvault


Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

Dessa är alla hello Key Vault-krav för att konfigurera ett Service Fabric-kluster Resource Manager-mall som installerar certifikat för noden autentisering, hantering av slutpunktssäkerhet och autentisering och eventuella ytterligare programsäkerhet funktioner som använder X.509-certifikat. Nu är bör du nu ha hello efter installationen i Azure:

* Key Vault resursgruppen.
  * Key Vault
    * Klustret certifikat för serverautentisering

< /a ”Skapa kluster-portal” ></a>

## <a name="create-cluster-in-hello-azure-portal"></a>Skapa kluster i hello Azure-portalen
### <a name="search-for-hello-service-fabric-cluster-resource"></a>Sök efter hello klusterresurs för Service Fabric
![Sök efter mallen för Service Fabric-kluster på hello Azure-portalen.][SearchforServiceFabricClusterTemplate]

1. Logga in toohello [Azure-portalen][azure-portal].
2. Klicka på **ny** tooadd en ny resursmall för. Sök efter hello Service Fabric-kluster mallen i hello **Marketplace** under **allt**.
3. Välj **Service Fabric-kluster** hello-listan.
4. Navigera toohello **Service Fabric-kluster** bladet, klickar du på **skapa**,
5. Hej **skapar Service Fabric-kluster** bladet har hello följande fyra steg.

#### <a name="1-basics"></a>1. Grundläggande inställningar
![Skärmbild som visar hur du skapar en ny resursgrupp.][CreateRG]

I bladet för grundläggande hello måste tooprovide hello grundläggande information för klustret.

1. Ange hello namnet på klustret.
2. Ange en **användarnamn** och **lösenord** Fjärrskrivbord för hello virtuella datorer.
3. Se till att tooselect hello **prenumeration** som du vill ditt kluster toobe som distribuerats till, särskilt om du har flera prenumerationer.
4. Skapa en **ny resursgrupp**. Det är bästa toogive den hello samma namn som hello klustret, eftersom det hjälper dig att hitta dem senare, särskilt när du försöker toomake ändringar tooyour distribution eller ta bort klustret.
   
   > [!NOTE]
   > Även om du kan bestämma toouse en befintlig resursgrupp är en bra idé toocreate en ny resursgrupp. Detta gör det enkelt toodelete kluster som du inte behöver.
   > 
   > 
5. Välj hello **region** som du vill toocreate hello klustret. Du måste använda hello tillhör samma region som din nyckel valvet.

#### <a name="2-cluster-configuration"></a>2. Klusterkonfiguration
![Skapa en nodtyp][CreateNodeType]

Konfigurera klusternoderna. Nodtyper definiera hello VM-storlekar, hello antal virtuella datorer och deras egenskaper. Klustret kan ha fler än en nodtyp, men hello primära nodtypen (hello förstnämnda som du definierar i hello portal) måste ha minst fem virtuella datorer, eftersom det är hello nodtypen där Service Fabric systemtjänster placeras. Konfigurera inte **placeringsegenskaper** eftersom en standardegenskap placering av ”NodeTypeName” läggs till automatiskt.

> [!NOTE]
> Ett vanligt scenario för flera nodtyper är ett program som innehåller en frontend-tjänst och en backend tjänst. Du vill tooput hello frontend-tjänst på mindre virtuella datorer (VM-storlekar som D2) med portar öppna toohello Internet men du vill tooput hello backend-tjänst på större virtuella datorer (med VM-storlekar som D4, D6, D15 och så vidare) utan Internet-riktade öppna portar.
> 
> 

1. Välj ett namn för din nodtypen (1 too12 tecken som innehåller endast bokstäver och siffror).
2. hello minsta **storlek** för virtuella datorer för hello primära noden typen drivs av hello **hållbarhet** nivå som du väljer för hello klustret. hello standardvärdet för hello hållbarhetsnivån är Brons. Mer information om hållbarhet finns [hur toochoose hello Service Fabric-kluster tillförlitlighet och hållbarhet][service-fabric-cluster-capacity].
3. Välj hello VM-storlek och prisnivå. D-serien virtuella datorer ha SSD-enheterna och rekommenderas för tillståndskänsliga program. Använd inte VM-SKU som har delvis kärnor eller vara mindre än 7 GB tillgängligt diskutrymme. 
4. hello minsta **nummer** för virtuella datorer för hello primära noden typen drivs av hello **tillförlitlighet** nivå som du väljer. hello standardvärdet för hello tillförlitlighetsnivån är Silver. Mer information om tillförlitlighet finns [hur toochoose hello Service Fabric-kluster tillförlitlighet och hållbarhet][service-fabric-cluster-capacity].
5. Välj hello antal virtuella datorer för hello nodtypen. Du kan skala upp eller ned hello antal virtuella datorer i en nodtyp vid ett senare tillfälle, men på hello primära nodtypen hello minsta drivs av hello tillförlitlighet nivå som du har valt. Andra nodtyper kan ha minst 1 VM.
6. Konfigurera anpassade slutpunkter. Det här fältet kan tooenter en kommaavgränsad lista över portar som du vill tooexpose via hello Azure belastningsutjämnare toohello offentliga Internet för dina program. Om du planerar toodeploy ett web application tooyour kluster, t.ex ”80” här tooallow trafik på port 80 till klustret. Läs mer på slutpunkter [kommunicera med program][service-fabric-connect-and-communicate-with-services]
7. Konfigurera klustret **diagnostik**. Som standard aktiveras diagnostik på ditt kluster tooassist med felsökning av problem. Om du vill toodisable diagnostik ändra hello **Status** växla för**av**. Om du inaktiverar diagnostik är **inte** rekommenderas.
8. Välj hello Fabric Uppgraderingsläge som du vill ange ditt kluster till. Välj **automatisk**, om du vill hello system tooautomatically Välj in hello senaste tillgängliga versionen och försök tooupgrade kluster-tooit. Ange hello-läge för**manuell**om du vill toochoose en version som stöds.

> [!NOTE]
> Vi stöder endast kluster som kör versioner av service Fabric stöds. Genom att välja hello **manuell** läge du tar på hello ansvar tooupgrade ditt kluster tooa som stöds av version. Mer information om hello Fabric Uppgraderingsläge finns hello [service fabric-kluster-uppgradering dokumentet.][service-fabric-cluster-upgrade]
> 
> 

#### <a name="3-security"></a>3. Säkerhet
![Skärmbild som visar säkerhetskonfigurationer på Azure-portalen.][SecurityConfigs]

hello sista steget är tooprovide certifikat information toosecure hello kluster med hjälp av hello Key Vault och certifikat information skapade tidigare.

* Fylla i hello certifikatet för primär fält med hello utdata från överför hello **klustret certifikat** tooKey valvet med hello `Invoke-AddCertToKeyVault` PowerShell-kommando.

```powershell
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47
```

* Kontrollera hello **konfigurera avancerade inställningar** rutan tooenter klientcertifikat för **administratörsklient** och **skrivskyddad klient**. I de här fälten att ange hello tumavtryck klientcertifikatet admin och hello tumavtryck klientcertifikatet skrivskyddade användaren, om tillämpligt. När administratörer tooconnect toohello klustret kan beviljas åtkomst endast om de har ett certifikat med ett tumavtryck som matchar hello tumavtrycket värden anges här.  

#### <a name="4-summary"></a>4. Sammanfattning
![Skärmbild som visar hello start board visa ”distribuera Service Fabric-kluster”. ][Notifications]

toocomplete hello klustret har skapats, klickar du på **sammanfattning** toosee hello konfigurationer som du har angett eller hämta hello Azure Resource Manager-mall som används toodeploy klustret. När du har angett hello obligatoriska inställningar hello **OK** knappen blir grön och du kan starta hello klusterskapandeprocessen genom att klicka på den.

Du kan se hello förlopp i hello meddelanden. (Klicka på hello ”” klockikonen nära hello hello övre högra hörnet på skärmen i statusfältet.) Om du klickade på **PIN-kod tooStartboard** när du skapar klustret hello visas **distribuerar Service Fabric-kluster** Fäst toohello **starta** kort.

### <a name="view-your-cluster-status"></a>Visa klusterstatus för
![Skärmbild som visar information om kluster hello instrumentpanelen.][ClusterDashboard]

När klustret har skapats kan du granska klustret i hello portal:

1. Gå för**Bläddra** och på **Service Fabric-kluster**.
2. Leta upp ditt kluster och klicka på den.
3. Du kan nu se hello information på klustret i hello instrumentpanel, inklusive hello klustrets offentlig slutpunkt och en länk tooService Fabric Explorer.

Hej **nod övervakaren** avsnitt på hello klustrets instrumentpanel blad anger hello antal virtuella datorer som är felfria och inte felfri. Du hittar mer information om hello klustrets hälsa i [Service Fabric hälsa modellen introduktion][service-fabric-health-introduction].

> [!NOTE]
> Service Fabric-kluster kräver ett visst antal noder toobe alltid toomaintain tillgänglighet och bevara tillstånd - refererad tooas ”underhålla kvorum”. Therfore, det är vanligtvis inte säker tooshut ned alla datorer i hello kluster om du först har utfört en [fullständig säkerhetskopiering av din tillstånd][service-fabric-reliable-services-backup-restore].
> 
> 

## <a name="remote-connect-tooa-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Fjärranslutning tooa Virtual Machine Scale Set-instans eller en klusternod
Var och en av hello nodetypes får du ange i klustret resultaten i en virtuell dator Skaluppsättning komma installation. Se [fjärranslutning tooa Skaluppsättning för virtuell dator instans] [ remote-connect-to-a-vm-scale-set] mer information.

## <a name="next-steps"></a>Nästa steg
Du har nu ett säker kluster som använder certifikat för autentisering för hantering. Nästa [ansluta tooyour klustret](service-fabric-connect-to-secure-cluster.md) och lära dig hur för[hantera program hemligheter](service-fabric-application-secret-management.md).  Lär dig dessutom [Service Fabric supportalternativ](service-fabric-support.md).

<!-- Links -->
[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[azure-portal]: https://portal.azure.com/
[key-vault-get-started]: ../key-vault/key-vault-get-started.md
[create-cluster-arm]: service-fabric-cluster-creation-via-arm.md
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[service-fabric-cluster-security-roles]: service-fabric-cluster-security-roles.md
[service-fabric-cluster-capacity]: service-fabric-cluster-capacity.md
[service-fabric-connect-and-communicate-with-services]: service-fabric-connect-and-communicate-with-services.md
[service-fabric-health-introduction]: service-fabric-health-introduction.md
[service-fabric-reliable-services-backup-restore]: service-fabric-reliable-services-backup-restore.md
[remote-connect-to-a-vm-scale-set]: service-fabric-cluster-nodetypes.md#remote-connect-to-a-vm-scale-set-instance-or-a-cluster-node
[service-fabric-cluster-upgrade]: service-fabric-cluster-upgrade.md

<!--Image references-->
[SearchforServiceFabricClusterTemplate]: ./media/service-fabric-cluster-creation-via-portal/SearchforServiceFabricClusterTemplate.png
[CreateRG]: ./media/service-fabric-cluster-creation-via-portal/CreateRG.png
[CreateNodeType]: ./media/service-fabric-cluster-creation-via-portal/NodeType.png
[SecurityConfigs]: ./media/service-fabric-cluster-creation-via-portal/SecurityConfigs.png
[Notifications]: ./media/service-fabric-cluster-creation-via-portal/notifications.png
[ClusterDashboard]: ./media/service-fabric-cluster-creation-via-portal/ClusterDashboard.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
