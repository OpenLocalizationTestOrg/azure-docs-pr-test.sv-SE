---
title: "aaaQuickstart - Azure Kubernetes kluster för Windows | Microsoft Docs"
description: "Lär dig snabbt toocreate ett Kubernetes kluster för Windows-behållare i Azure Container Service med hello Azure CLI."
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 85fe65a46ae8c78797e8a8a097c2a37f06329335
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-kubernetes-cluster-for-windows-containers"></a>Distribuera Kubernets-kluster för Windows-behållare

hello Azure CLI är används toocreate och hantera Azure-resurser från hello kommandoraden eller i skript. Den här guiden information med hjälp av hello Azure CLI toodeploy en [Kubernetes](https://kubernetes.io/docs/home/) klustret i [Azure Container Service](../container-service-intro.md). När hello klustret distribueras kan du ansluta tooit med hello Kubernetes `kubectl` kommandoradsverktyget och distribuera din första Windows-behållare.

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt denna Snabbstart kräver att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

> [!NOTE]
> Stöd för Windows-behållare med Kubernetes i Azure Container Service är tillgängligt som en förhandsversion. 
>

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando. En Azure-resursgrupp är en logisk grupp där Azure-resurser distribueras och hanteras. 

hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-kubernetes-cluster"></a>Skapa Kubernetes-kluster
Skapa ett Kubernetes-kluster i Azure Container Service med hello [az acs skapa](/cli/azure/acs#create) kommando. 

hello följande exempel skapar ett kluster med namnet *myK8sCluster* med en Linux master-nod och två noder för Windows-agenten. Det här exemplet skapar SSH nycklar krävs tooconnect toohello Linux master. Det här exemplet används *azureuser* för en administrativ användarnamn och *myPassword12* som hello lösenord på hello Windows noder. Uppdatera dessa värden toosomething lämpliga tooyour miljö. 



```azurecli-interactive 
az acs create --orchestrator-type=kubernetes \
    --resource-group myResourceGroup \
    --name=myK8sCluster \
    --agent-count=2 \
    --generate-ssh-keys \
    --windows --admin-username azureuser \
    --admin-password myPassword12
```

Om några minuter hello-kommandot har slutförts och visar information om din distribution.

## <a name="install-kubectl"></a>Installera kubectl

tooconnect toohello Kubernetes kluster från din klientdator, Använd [ `kubectl` ](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes kommandorads-klient. 

Om du använder Azure CloudShell är `kubectl` redan installerad. Om du vill tooinstall lokalt, du kan använda hello [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) kommando.

Hej följande Azure CLI exempel installerar `kubectl` tooyour system. I Windows kör du kommandot som administratör.

```azurecli-interactive 
az acs kubernetes install-cli
```


## <a name="connect-with-kubectl"></a>Ansluta med kubectl

tooconfigure `kubectl` tooconnect tooyour Kubernetes klustret, kör hello [az acs kubernetes get-autentiseringsuppgifter](/cli/azure/acs/kubernetes#get-credentials) kommando. hello hämtar följande exempel hello klusterkonfiguration för Kubernetes klustret.

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

tooverify hello anslutning tooyour kluster från din dator, kör:

```azurecli-interactive
kubectl get nodes
```

`kubectl`Visar hello-hanterare och agent noder.

```azurecli-interactive
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.5.3
k8s-agent-98dc3136-1    Ready                      5m        v1.5.3
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.5.3

```

## <a name="deploy-a-windows-iis-container"></a>Distribuera en Windows-IIS-behållare

Du kan köra en dockerbehållare i en Kubernetes-*pod*, som innehåller en eller flera behållare. 

Det här enkla exemplet används en JSON-fil toospecify en behållare för Microsoft Internet Information Server (IIS) och skapar sedan hello baljor med hello `kubctl apply` kommando. 

Skapa en lokal fil med namnet `iis.json` och kopiera hello efter texten. Den här filen talar om Kubernetes toorun IIS på Windows Server 2016 Nano Server med en offentlig behållare bild från [Docker-hubb](https://hub.docker.com/r/nanoserver/iis/). hello behållare använder port 80, men är först endast tillgänglig hello klusternätverk.

 ```JSON
 {
  "apiVersion": "v1",
  "kind": "Pod",
  "metadata": {
    "name": "iis",
    "labels": {
      "name": "iis"
    }
  },
  "spec": {
    "containers": [
      {
        "name": "iis",
        "image": "nanoserver/iis",
        "ports": [
          {
          "containerPort": 80
          }
        ]
      }
    ],
    "nodeSelector": {
     "beta.kubernetes.io/os": "windows"
     }
   }
 }
 ```

toostart hello baljor, typ:
  
```azurecli-interactive
kubectl apply -f iis.json
```  

tootrack hello distribution, typ:
  
```azurecli-interactive
kubectl get pods
```

När du distribuerar hello baljor hello status är `ContainerCreating`. Det kan ta några minuter för hello behållaren tooenter hello `Running` tillstånd.

```azurecli-interactive
NAME     READY        STATUS        RESTARTS    AGE
iis      1/1          Running       0           32s
```

## <a name="view-hello-iis-welcome-page"></a>Visa hello välkomstsidan för IIS

tooexpose baljor toohello hälsningsmeddelande med en offentlig IP-adress, typen hello följande kommando:

```azurecli-interactive
kubectl expose pods iis --port=80 --type=LoadBalancer
```

Med det här kommandot Kubernetes skapar en tjänst och en [Azure belastningsutjämningsregeln](container-service-kubernetes-load-balancing.md) med en offentlig IP-adress för hello-tjänsten. 

Kör följande kommando toosee hello hello tjänstens status hello.

```azurecli-interactive
kubectl get svc
```

Hello IP-adress visas först som `pending`. Efter några minuter hello externa IP-adress hello `iis` baljor anges:
  
```azurecli-interactive
NAME         CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE       
kubernetes   10.0.0.1       <none>          443/TCP        21h       
iis          10.0.111.25    13.64.158.233   80/TCP         22m
```

Du kan använda en webbläsare för din choice toosee hello IIS välkomstsidan på hello extern IP-adress:

![Bild av surfning tooIIS](./media/container-service-kubernetes-windows-walkthrough/kubernetes-iis.png)  


## <a name="delete-cluster"></a>Ta bort klustret
När hello klustret inte längre behövs, kan du använda hello [ta bort grupp az](/cli/azure/group#delete) kommandot tooremove hello resursgrupp, behållartjänsten och alla relaterade resurser.

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>Nästa steg

I den här snabbstartsguide du har distribuerat ett Kubernetes-kluster med `kubectl` och distribuerat en pod med en IIS-behållare. toolearn mer om Azure Container Service fortsätta toohello Kubernetes kursen.

> [!div class="nextstepaction"]
> [Hantera ett ACS Kubernets-kluster](container-service-tutorial-kubernetes-prepare-app.md)
