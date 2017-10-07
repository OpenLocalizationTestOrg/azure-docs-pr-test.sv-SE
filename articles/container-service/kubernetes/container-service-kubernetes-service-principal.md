---
title: "aaaService huvudnamn för Azure Kubernetes klustret | Microsoft Docs"
description: "Skapa och hantera ett tjänstobjekt för Azure Active Directory för ett Kubernetes-kluster i Azure Container Service"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 7a01624c5ac3fa717dbcbd570e05ceb4d917c53a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-an-azure-ad-service-principal-for-a-kubernetes-cluster-in-container-service"></a>Konfigurera ett Azure AD-tjänstobjekt för ett Kubernetes-kluster i Container Service


I Azure Container Service en Kubernetes klustret kräver en [Azure Active Directory-tjänstens huvudnamn](../../active-directory/develop/active-directory-application-objects.md) toointeract med Azure API: er. hello tjänstens huvudnamn behövs toodynamically hantera resurser som [användardefinierade vägar](../../virtual-network/virtual-networks-udr-overview.md) och hello [lager 4 Azure belastningsutjämnare](../../load-balancer/load-balancer-overview.md). 


Den här artikeln visar olika alternativ tooset upp en tjänst huvudnamn för Kubernetes klustret. Om du har installerat och konfigurerat hello exempelvis [Azure CLI 2.0](/cli/azure/install-az-cli2), kan du köra hello [ `az acs create` ](/cli/azure/acs#create) kommandot toocreate hello Kubernetes klustret och hello tjänstens huvudnamn på hello samtidigt.


## <a name="requirements-for-hello-service-principal"></a>Krav för hello tjänstens huvudnamn

Du kan använda en befintlig Azure AD-tjänstobjekt som uppfyller hello följande krav eller skapa en ny.

* **Scope**: hello resursgrupp i hello prenumerationen används toodeploy hello Kubernetes klustret eller (mindre restriktivt) hello prenumerationen används toodeploy hello klustret.

* **Roll**: **Deltagare**

* **Klienthemlighet**: måste vara ett lösenord. För närvarande kan du inte använda ett huvudnamn för tjänsten som konfigurerats för certifikatautentisering.

> [!IMPORTANT] 
> toocreate en tjänstens huvudnamn, måste du ha behörigheter tooregister ett program med Azure AD-klient och tooassign hello tooa programrollen i din prenumeration. toosee om du har hello krävs behörighet [checka in hello Portal](../../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions). 
>

## <a name="option-1-create-a-service-principal-in-azure-ad"></a>Alternativ 1: Skapa ett tjänstobjekt i Azure AD

Om du vill toocreate ett huvudnamn för Azure AD-tjänsten innan du distribuerar Kubernetes klustret tillhandahåller Azure flera olika metoder. 

hello följande exempelkommandon visar hur toodo detta med hello [Azure CLI 2.0](../../azure-resource-manager/resource-group-authenticate-service-principal-cli.md). Du kan också skapa ett service principal med [Azure PowerShell](../../azure-resource-manager/resource-group-authenticate-service-principal.md), hello [portal](../../azure-resource-manager/resource-group-create-service-principal-portal.md), eller andra metoder.

```azurecli
az login

az account set --subscription "mySubscriptionID"

az group create -n "myResourceGroupName" -l "westus"

az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/mySubscriptionID/resourceGroups/myResourceGroupName"
```

Utdata är liknande toohello följande (visas här omarbetade):

![Skapa ett huvudnamn för tjänsten](./media/container-service-kubernetes-service-principal/service-principal-creds.png)

Markerat är hello **klient-ID** (`appId`) och hello **klienthemlighet** (`password`) som du använder som service principal parametrar för klusterdistribution.


### <a name="specify-service-principal-when-creating-hello-kubernetes-cluster"></a>Ange tjänstens huvudnamn när du skapar hello Kubernetes kluster

Ange hello **klient-ID** (kallas även hello `appId`, för program-ID) och **klienthemlighet** (`password`) en befintlig tjänstens huvudnamn som parametrar när du skapar hello Kubernetes kluster. Kontrollera att hello tjänstens huvudnamn uppfyller hello på hello från den här artikeln.

Du kan ange dessa parametrar när du distribuerar hello Kubernetes kluster med hjälp av hello [Azure kommandoradsgränssnittet (CLI) 2.0](container-service-kubernetes-walkthrough.md), [Azure-portalen](../dcos-swarm/container-service-deployment.md), eller andra metoder.

>[!TIP] 
>När du anger hello **klient-ID**, vara säker på att toouse hello `appId`, inte hello `ObjectId`, av hello tjänstens huvudnamn.
>

hello följande exempel visas ett sätt toopass hello parametrar med hello Azure CLI 2.0. Det här exemplet används hello [Kubernetes quickstart mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes).

1. [Hämta](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.parameters.json) hello mallfilen parametrar `azuredeploy.parameters.json` från GitHub.

2. toospecify hello tjänstens huvudnamn, ange värden för `servicePrincipalClientId` och `servicePrincipalClientSecret` i hello-filen. (Du måste också tooprovide egna värden för `dnsNamePrefix` och `sshRSAPublicKey`. hello senare är hello SSH offentlig nyckel tooaccess hello kluster.) Spara hello-filen.

    ![Skicka parametrar för tjänstens huvudnamn](./media/container-service-kubernetes-service-principal/service-principal-params.png)

3. Kör hello följande kommando, med `--parameters` tooset hello sökväg toohello azuredeploy.parameters.json fil. Det här kommandot distribuerar hello-kluster i en resursgrupp som du skapar kallas `myResourceGroup` i hello västra USA region.

    ```azurecli
    az login

    az account set --subscription "mySubscriptionID"

    az group create --name "myResourceGroup" --location "westus" 
    
    az group deployment create -g "myResourceGroup" --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.json" --parameters @azuredeploy.parameters.json
    ```


## <a name="option-2-generate-a-service-principal-when-creating-hello-cluster-with-az-acs-create"></a>Alternativ 2: Skapa ett huvudnamn för tjänsten när du skapar hello kluster med`az acs create`

Om du kör hello [ `az acs create` ](/cli/azure/acs#create) kommandot toocreate hello Kubernetes klustret måste du hello alternativet toogenerate ett huvudnamn för tjänsten automatiskt.

Som med andra alternativ för att skapa Kubernetes-kluster, kan du ange parametrar för en befintlig tjänsts huvudnamn när du kör `az acs create`. Men om du utelämnar parametrarna skapar hello Azure CLI en automatiskt för användning med Container Service. Detta sker transparent under hello distributionen. 

hello följande kommando skapar ett Kubernetes kluster och genererar både SSH-nycklar och tjänstens huvudnamn autentiseringsuppgifter:

```console
az acs create -n myClusterName -d myDNSPrefix -g myResourceGroup --generate-ssh-keys --orchestrator-type kubernetes
```

> [!IMPORTANT]
> Om ditt konto inte har hello Azure AD och prenumeration behörigheter toocreate ett huvudnamn för tjänsten, genererar hello kommandot ett felmeddelande för`Insufficient privileges toocomplete hello operation.`
> 

## <a name="additional-considerations"></a>Annat som är bra att tänka på

* Om du inte har behörigheter toocreate ett huvudnamn för tjänsten i din prenumeration kan du behöva tooask din Azure AD eller prenumeration administratören tooassign hello behörighet och be dem att ge en service principal toouse med Azure Container Service. 

* hello tjänstens huvudnamn för Kubernetes är en del av hello klusterkonfigurationen. Dock inte använda hello identitet toodeploy hello kluster.

* Varje tjänstobjekt är associerat med ett Azure AD-program. hello tjänstens huvudnamn för ett kluster med Kubernetes kan vara kopplad till någon giltig Azure AD-programnamn (till exempel: `https://www.contoso.org/example`). hello-URL för programmet hello saknar toobe en verklig slutpunkt.

* Ange när hello tjänstens huvudnamn **klient-ID**, kan du använda hello värde av hello `appId` (som visas i den här artikeln) eller hello motsvarande tjänstens huvudnamn `name` (till exempel`https://www.contoso.org/example`).

* På hello master och agenten virtuella datorer i hello Kubernetes kluster lagras hello service principal autentiseringsuppgifterna i hello filen /etc/kubernetes/azure.json.

* När du använder hello `az acs create` kommandot toogenerate hello tjänstens huvudnamn automatiskt, huvudnamn hello Tjänstereferenser skrivs toohello filen ~/.azure/acsServicePrincipal.json på hello datorn används toorun hello kommando. 

* När du använder hello `az acs create` kommandot toogenerate hello tjänstens huvudnamn automatiskt, hello tjänstens huvudnamn kan också autentisera med ett [Azure-behållaren registret](../../container-registry/container-registry-intro.md) skapas i hello samma prenumeration.




## <a name="next-steps"></a>Nästa steg

* [Kom igång med Kubernetes](container-service-kubernetes-walkthrough.md) i behållaren för tjänsteklustret.

* tootroubleshoot hello-tjänstens huvudnamn för Kubernetes, se hello [ACS-motorn dokumentationen](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes.md#troubleshooting).


