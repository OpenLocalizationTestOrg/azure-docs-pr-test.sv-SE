---
title: "Tjänstens huvudnamn för Azure Kubernetes-kluster | Microsoft- Docs"
description: "Skapa och hantera ett tjänstobjekt för Azure Active Directory för ett Kubernetes-kluster i Azure Container Service"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/26/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 14975454cbc0afcfbdbd3aa6b52983be4d4b1785
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-an-azure-ad-service-principal-for-a-kubernetes-cluster-in-container-service"></a>Konfigurera ett Azure AD-tjänstobjekt för ett Kubernetes-kluster i Container Service


I Azure Container Service kräver ett Kubernetes-kluster ett [Azure Active Directory-tjänstobjekt](../../active-directory/develop/active-directory-application-objects.md) för att kunna interagera med Azure-API:er. Tjänstens huvudnamn krävs för att dynamiskt hantera resurser som [användardefinierade vägar](../../virtual-network/virtual-networks-udr-overview.md) och [lager 4 för Azure Load Balancer](../../load-balancer/load-balancer-overview.md).


Den här artikeln beskriver hur du konfigurerar ett tjänstobjekt för ett Kubernetes-kluster på olika sätt. Om du till exempel har installerat och konfigurerat [Azure CLI 2.0](/cli/azure/install-az-cli2) kan du köra kommandot [`az acs create`](/cli/azure/acs#create) för att skapa Kubernetes-klustret och tjänstobjektet samtidigt.


## <a name="requirements-for-the-service-principal"></a>Krav för tjänstens huvudnamn

Du kan använda ett befintligt Azure AD-tjänstobjekt som uppfyller kraven nedan, eller skapa ett nytt.

* **Omfattning**: den prenumeration som används för att distribuera klustret.

* **Roll**: **Deltagare**

* **Klienthemlighet**: måste vara ett lösenord. För närvarande kan du inte använda ett huvudnamn för tjänsten som konfigurerats för certifikatautentisering.

> [!IMPORTANT]
> För att skapa ett tjänstobjekt måste du ha behörighet att registrera ett program med din Azure AD-klientorganisation, samt behörighet att tilldela programmet till en roll i din prenumeration. Du kan kontrollera om du har nödvändig behörighet [på portalen](../../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions).
>

## <a name="option-1-create-a-service-principal-in-azure-ad"></a>Alternativ 1: Skapa ett tjänstobjekt i Azure AD

Om du vill skapa ett Azure AD-tjänstobjekt innan du distribuerar Kubernetes-klustret kan du välja mellan flera metoder i Azure.

Kommandona i följande exempel visar hur du gör detta med [Azure CLI 2.0](../../azure-resource-manager/resource-group-authenticate-service-principal-cli.md). Du kan också skapa ett tjänstobjekt med [Azure PowerShell](../../azure-resource-manager/resource-group-authenticate-service-principal.md), [portalen](../../azure-resource-manager/resource-group-create-service-principal-portal.md) eller andra metoder.

```azurecli
az login

az account set --subscription "mySubscriptionID"

az group create --name "myResourceGroup" --location "westus"

az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/mySubscriptionID"
```

De utdata som returneras ser ut ungefär så här (redigerat i bilden):

![Skapa ett huvudnamn för tjänsten](./media/container-service-kubernetes-service-principal/service-principal-creds.png)

De **klient-ID** (`appId`) och **klienthemligheten** (`password`) som du använder som parametrar för tjänstens huvudnamn för klusterdistribution är markerade.


### <a name="specify-service-principal-when-creating-the-kubernetes-cluster"></a>Ange tjänstobjektet när du skapar Kubernetes-klustret

Ange **klient-ID:t** (kallas även `appId`, dvs. program-ID) och **klienthemligheten** (`password`) för ett befintligt tjänstobjekt som parametrar när du skapar Kubernetes-klustret. Kontrollera att tjänstobjektet uppfyller kraven som anges i början av den här artikeln.

Du kan ange dessa parametrar när du distribuerar Kubernetes-klustret med hjälp av [Azure Command-Line Interface (CLI) 2.0](container-service-kubernetes-walkthrough.md), [Azure Portal](../dcos-swarm/container-service-deployment.md) eller andra metoder.

>[!TIP]
>När du anger **klient-ID**, måste du använda `appId`, och inte `ObjectId`, av huvudnamnet för tjänsten.
>

Följande exempel beskriver ett sätt att överföra parametrarna med Azure CLI 2.0. I det här exemplet används [mallen Kubernetes-snabbstart](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes).

1. [Hämta](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.parameters.json) filen med mallparametrar `azuredeploy.parameters.json` från GitHub.

2. För att ange tjänstens huvudnamn, anger du värden för `servicePrincipalClientId` och `servicePrincipalClientSecret` i filen. (Du måste också ange dina egna värden för `dnsNamePrefix` och `sshRSAPublicKey`. Dessa är den offentliga SSH-nyckeln till klustret.) Spara filen.

    ![Skicka parametrar för tjänstens huvudnamn](./media/container-service-kubernetes-service-principal/service-principal-params.png)

3. Kör följande kommando, med hjälp av `--parameters` för att ange sökvägen till filen azuredeploy.parameters.json. Det här kommandot distribuerar i klustret i en resursgrupp du skapar med namnet `myResourceGroup` i regionen USA, västra.

    ```azurecli
    az login

    az account set --subscription "mySubscriptionID"

    az group create --name "myResourceGroup" --location "westus"

    az group deployment create -g "myResourceGroup" --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.json" --parameters @azuredeploy.parameters.json
    ```


## <a name="option-2-generate-a-service-principal-when-creating-the-cluster-with-az-acs-create"></a>Alternativ 2: Generera ett tjänstobjekt när du skapar klustret med `az acs create`

Om du kör kommandot [`az acs create`](/cli/azure/acs#create) för att skapa Kubernetes-klustret kan du välja att generera ett tjänstobjekt automatiskt.

Som med andra alternativ för att skapa Kubernetes-kluster, kan du ange parametrar för en befintlig tjänsts huvudnamn när du kör `az acs create`. Om du utelämnar dessa parametrar skapar Azure CLI ett automatiskt för användning med Container Service. Detta sker transparent under distributionen.

Följande kommando skapar ett Kubernetes-kluster och genererar både SSH-nycklar och autentiseringsuppgifter för tjänstens huvudnamn:

```console
az acs create -n myClusterName -d myDNSPrefix -g myResourceGroup --generate-ssh-keys --orchestrator-type kubernetes
```

> [!IMPORTANT]
> Om ditt konto saknar de Azure AD- och prenumerationsbehörigheter som krävs för att skapa ett tjänstobjekt genererar kommandot ett fel liknande `Insufficient privileges to complete the operation.`
>

## <a name="additional-considerations"></a>Annat som är bra att tänka på

* Om du inte har behörighet att skapa ett tjänstobjekt i din prenumeration kan du behöva be din Azure AD- eller prenumerationsadministratör att tilldela nödvändiga behörigheter, eller be om ett tjänstobjekt som kan användas med Azure Container Service.

* Tjänstobjektet för Kubernetes är en del av klusterkonfigurationen. Men använd inte identiteten för att distribuera klustret.

* Varje tjänstobjekt är associerat med ett Azure AD-program. Tjänstobjektet för ett Kubernetes-kluster kan associeras med ett giltigt Azure Active Directory-programnamn (till exempel: `https://www.contoso.org/example`). URL:en för programmet behöver inte vara en verklig slutpunkt.

* När du anger **klient-ID:t** för tjänstobjektet kan du använda värdet för `appId` (som anges i den här artikeln) eller motsvarande `name` för tjänstobjektet (till exempel `https://www.contoso.org/example`).

* På virtuella huvud- och agentdatorer i Kubernetes-klustret lagras autentiseringsuppgifterna för tjänstobjektet i filen /etc/kubernetes/azure.json.

* Om du använder kommandot `az acs create` för att generera tjänstobjektet automatiskt, skrivs autentiseringsuppgifterna för tjänstobjektet till filen ~/.azure/acsServicePrincipal.json på den dator som används för att köra kommandot.

* Om du använder kommandot `az acs create` för att generera tjänstobjektet automatiskt, kan tjänstobjektet även autentisera med ett [Azure-behållarregister](../../container-registry/container-registry-intro.md) som skapats i samma prenumeration.

## <a name="next-steps"></a>Nästa steg

* [Kom igång med Kubernetes](container-service-kubernetes-walkthrough.md) i behållaren för tjänsteklustret.

* Information om hur du felsöker tjänstobjektet för Kubernetes finns i [ACS Engine-dokumentationen](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes.md#troubleshooting).