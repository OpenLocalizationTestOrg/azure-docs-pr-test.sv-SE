---
title: "aaaSetting in ett Service Fabric-kluster med hjälp av Visual Studio | Microsoft Docs"
description: "Beskriver hur tooset upp ett Service Fabric-kluster med hjälp av Azure Resource Manager-mall som skapats av en Azure-resursgrupp-projekt i Visual Studio"
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: adegeo
editor: 
ms.assetid: bd2c0511-36c9-4828-8dc3-69e4b6a70567
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/21/2017
ms.author: mikhegn
redirect_url: /azure/service-fabric/service-fabric-cluster-creation-via-arm
ms.openlocfilehash: adb0dd2169a28b46e832c6f06c998cbed0c473f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-service-fabric-cluster-by-using-visual-studio"></a>Konfigurera ett Service Fabric-kluster med hjälp av Visual Studio
Den här artikeln beskriver hur tooset upp ett Azure Service Fabric-kluster med hjälp av Visual Studio och en Azure Resource Manager-mall. Vi använder en Visual Studio Azure resource group toocreate hello projektmall. Kan distribueras efter hello mallen har skapats direkt tooAzure från Visual Studio. Det kan också användas från ett skript eller som en del av kontinuerlig integration (KO).

## <a name="create-a-service-fabric-cluster-template-by-using-an-azure-resource-group-project"></a>Skapa en mall för Service Fabric-kluster med hjälp av en Azure resursgruppsprojektet
tooget igång, öppna Visual Studio och skapa ett Azure resursgruppsprojektet (tillgänglig i hello **moln** mapp):

![Dialogrutan Nytt projekt med Azure-resursgrupp projektet som du valt][1]

Du kan skapa en ny Visual Studio-lösning för det här projektet eller lägga till den tooan befintlig lösning.

> [!NOTE]
> Om du inte ser hello Azure resursgruppsprojektet under hello molnet noden har inte hello Azure SDK är installerat. Starta installationsprogrammet för webbplattform ([installera nu](http://www.microsoft.com/web/downloads/platform.aspx) om du inte redan har), och Sök efter ”Azure SDK för .NET” och installera hello-version som är kompatibel med din version av Visual Studio.
> 
> 

När du klickar på OK hello-knappen, ber Visual Studio dig tooselect hello Resource Manager-mall som du vill toocreate:

![Välj dialogrutan mall i Azure med valda Service Fabric-kluster-mallen][2]

Välj mall för hello Service Fabric-kluster och träffar hello OK-knappen igen. hello-projektet och hello Resource Manager-mall har skapats.

## <a name="prepare-hello-template-for-deployment"></a>Förbereda hello mall för distribution
Innan hello mallen är distribuerade toocreate hello kluster, måste du ange värden för mallparametrar hello krävs. Dessa parametervärden läses från hello `ServiceFabricCluster.parameters.json` -filen i hello `Templates` hello resursgruppsprojektet mapp. Öppna hello-filen och ange hello följande värden:

| Parameternamn | Beskrivning |
| --- | --- |
| adminUserName |hello namnet på hello administratörskontot för Service Fabric-datorer (noder). |
| certificateThumbprint |hello tumavtrycket för certifikatet för hello som skyddar hello klustret. |
| sourceVaultResourceId |Hej *resurs-ID* av hello nyckelvalv där hello-certifikatet som skyddar hello kluster lagras. |
| certificateUrlValue |hello-URL för hello klustret säkerhetscertifikat. |

hello Visual Studio Service Fabric Resource Manager-mall skapar en säker kluster som skyddas av ett certifikat. Det här certifikatet har identifierats av hello senaste tre mallparametrar (`certificateThumbprint`, `sourceVaultValue`, och `certificateUrlValue`), och det måste finnas i en **Azure Key Vault**. Mer information om hur toocreate hello klustret säkerhetscertifikat finns [säkerhetsscenarier för Service Fabric-klustret](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) artikel.

## <a name="optional-change-hello-cluster-name"></a>Valfritt: ändra hello klustrets namn
Varje Service Fabric-klustret har ett namn. När en Fabric-klustret har skapats i Azure, klusternamnet avgör (tillsammans med hello Azure-region) hello Domain Name System (DNS) namn för hello-kluster. Om du namnger klustret exempelvis `myBigCluster`, och hello (Azure-region) hello resursgruppen som är värd för hello nya klustret finns östra USA blir hello DNS-namnet på klustret hello `myBigCluster.eastus.cloudapp.azure.com`.

Som standard är hello klusternamnet genereras automatiskt och blir unikt genom att koppla ett slumpmässigt suffix tooa ”cluster” prefix. Detta gör det mycket enkelt toouse hello mallen som en del av en **kontinuerlig integration** (KO)-system. Om du vill toouse ett specifikt namn för klustret, ett som är meningsfullt tooyou anger hello värdet för hello `clusterName` variabeln i hello Resource Manager mallfilen (`ServiceFabricCluster.json`) tooyour valt namn. Det är första hello variabel som anges i den filen.

## <a name="optional-add-public-application-ports"></a>Valfritt: lägga till offentliga program portar
Du kan också toochange hello offentliga program portar för hello klustret innan du distribuerar den. Som standard öppnas hello mallen två offentliga TCP-portar (80 och 8081). Om du behöver mer för dina program, ändra hello Azure belastningsutjämnare definition i hello mallen. hello definition lagras i hello Huvudmall filen (`ServiceFabricCluster.json`). Öppna filen och Sök efter `loadBalancedAppPort`. Varje port är kopplat till tre artefakter:

1. Mallvariabeln som definierar hello TCP-portvärde för hello port:
   
    ```json
    "loadBalancedAppPort1": "80"
    ```
2. En *avsökningen* som definierar hur ofta och hur länge hello Azure belastningsutjämnare försöker toouse en specifik Service Fabric-nod innan åtgärden misslyckas över tooanother en. hello-avsökningar som ingår i hello belastningsutjämnaren resurs. Här är hello avsökningen definition för hello första programmet standardporten:
   
    ```json
    {
        "name": "AppPortProbe1",
        "properties": {
            "intervalInSeconds": 5,
            "numberOfProbes": 2,
            "port": "[variables('loadBalancedAppPort1')]",
            "protocol": "Tcp"
        }
    }
    ```
3. En *belastningsutjämning regeln* som binder samman hello port och hello avsökning, vilket möjliggör belastningsutjämning över en uppsättning noder i Service Fabric:
   
    ```json
    {
        "name": "AppPortLBRule1",
        "properties": {
            "backendAddressPool": {
                "id": "[variables('lbPoolID0')]"
            },
            "backendPort": "[variables('loadBalancedAppPort1')]",
            "enableFloatingIP": false,
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPort": "[variables('loadBalancedAppPort1')]",
            "idleTimeoutInMinutes": 5,
            "probe": {
                "id": "[concat(variables('lbID0'),'/probes/AppPortProbe1')]"
            },
            "protocol": "Tcp"
        }
    }
    ```
   Om fler portar hello-program som du planerar toodeploy toohello klustret måste du lägga till dem genom att skapa ytterligare avsökning och Regeldefinitioner för belastningsutjämning. Mer information om hur toowork med Azure belastningsutjämnare via Resource Manager-mallar finns [komma igång med en intern belastningsutjämnare använder en mall](../load-balancer/load-balancer-get-started-ilb-arm-template.md).

## <a name="deploy-hello-template-by-using-visual-studio"></a>Distribuera hello-mallen med hjälp av Visual Studio
När du har sparat alla hello obligatorisk parametervärdena i den`ServiceFabricCluster.param.dev.json` filen som du är klar toodeploy hello mallen och skapar Service Fabric-kluster. Högerklicka på hello resursgruppsprojektet i Visual Studio Solution Explorer och välj **distribuera | Ny distribution...** . Om nödvändigt, Visual Studio visar hello **distribuera tooResource grupp** i dialogrutan där du tooauthenticate tooAzure:

![Distribuera tooResource Gruppdialogrutan][3]

hello dialogrutan kan du välja en befintlig resursgrupp för Resource Manager för hello kluster och ger du hello alternativet toocreate en ny. Normalt gör det klokt toouse en separat resursgrupp för Service Fabric-klustret.

När du träffar hello distribuera knappen, uppmanas Visual Studio du tooconfirm hello parametervärden för mallen. Träffar hello **spara** knappen. En parameter har inte en beständiga värde: hello administrativa kontolösenord för hello-kluster. Du behöver tooprovide lösenordsvärdet när Visual Studio efterfrågar en.

> [!NOTE]
> Från och med Azure SDK 2.9, Visual Studio stöder läsning lösenord från **Azure Key Vault** under distributionen. Observera att hello i dialogrutan för hello mall parametrar `adminPassword` parametern textruta har en liten ”key”-ikon på hello rätt. Den här ikonen kan tooselect en befintlig hemlighet i nyckelvalvet som hello administratörslösenord för hello klustret. Kontrollera att toofirst aktivera Azure Resource Manager-åtkomst för malldistribution i hello avancerade åtkomstprinciper för nyckelvalvet. 
> 
> 

Du kan övervaka hello hello distributionsprocessen i utdatafönstret för hello Visual Studio. När hello mallen distributionen är klar, är det nya klustret redo toouse!

> [!NOTE]
> Om PowerShell aldrig har använt tooadminister Azure från hello-dator som du använder nu måste toodo lite underhåll.
> 
> 1. Aktivera PowerShell-skript genom att köra hello [ `Set-ExecutionPolicy` ](https://technet.microsoft.com/library/hh849812.aspx) kommando. ”Obegränsad” principen är oftast giltiga för utveckling datorer.
> 2. Bestäm om tooallow Diagnostisk datainsamling från Azure PowerShell-kommandon och kör [ `Enable-AzureRmDataCollection` ](https://msdn.microsoft.com/library/mt619303.aspx) eller [ `Disable-AzureRmDataCollection` ](https://msdn.microsoft.com/library/mt619236.aspx) efter behov. På så sätt undviker onödiga frågor när mallen distribueras.
> 
> 

Om det finns några fel, gå toohello [Azure-portalen](https://portal.azure.com/) och öppna hello resursgruppen som du har distribuerat till. Klicka på **alla inställningar**, klicka på **distributioner** på inställningsbladet för hello. Distributionen misslyckades-resursgrupp lämnar det detaljerad diagnostisk information.

> [!NOTE]
> Service Fabric-kluster kräver ett visst antal noder toobe toomaintain tillgänglighet och bevara tillstånd - refererad tooas ”för att underhålla kvorum”. Det är därför inte säker tooshut ned alla hello datorer i hello kluster om du först har utfört en [fullständig säkerhetskopiering av din tillstånd](service-fabric-reliable-services-backup-restore.md).
> 
> 

## <a name="next-steps"></a>Nästa steg
* [Lär dig mer om hur du konfigurerar Service Fabric-kluster med hjälp av hello Azure-portalen](service-fabric-cluster-creation-via-portal.md)
* [Lär dig hur toomanage och distribuerar Service Fabric-program med Visual Studio](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[1]: ./media/service-fabric-cluster-creation-via-visual-studio/azure-resource-group-project-creation.png
[2]: ./media/service-fabric-cluster-creation-via-visual-studio/selecting-azure-template.png
[3]: ./media/service-fabric-cluster-creation-via-visual-studio/deploy-to-azure.png
