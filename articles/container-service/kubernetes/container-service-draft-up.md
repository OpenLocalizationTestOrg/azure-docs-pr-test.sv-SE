---
title: aaaUse utkast med Azure Container Service och Azure Container register | Microsoft Docs
description: "Skapa ett Kubernetes ACS-kluster och ett Azure Container registret toocreate ditt första program i Azure med utkastet."
services: container-service
documentationcenter: 
author: squillace
manager: gamonroy
editor: 
tags: draft, helm, acs, azure-container-service
keywords: Docker, Containers, microservices, Kubernetes, Draft, Azure
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: rasquill
ms.custom: mvc
ms.openlocfilehash: f5e21cda01e5e8452bf86a5c8fa458904d89f451
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-draft-with-azure-container-service-and-azure-container-registry-toobuild-and-deploy-an-application-tookubernetes"></a>Använd utkast med Azure Container Service och Azure Container registret toobuild och distribuera ett program tooKubernetes

[Utkast](https://aka.ms/draft) är ett nytt verktyg för öppen källkod som gör det enkelt toodevelop behållaren-baserade program och distribuera dem tooKubernetes kluster utan att känna till mycket om Docker och Kubernetes-- eller installera dem även. Med hjälp av verktyg som utkast gör att du och ditt team fokus bygga hello program med Kubernetes, inte betalar så mycket uppmärksamhet tooinfrastructure.

Du kan använda Draft med alla Docker-avbildningsregister och Kubernetes-kluster, även lokalt. Den här kursen visar hur toouse ACS med Kubernetes, ACR och Azure DNS toocreate live CI/CD-utvecklare pipeline med utkastet.


## <a name="create-an-azure-container-registry"></a>Skapa ett Azure Container Registry
Du kan enkelt [skapa en ny Azure-behållare registernyckel](../../container-registry/container-registry-get-started-azure-cli.md), men hello stegen är som följer:

1. Skapa en Azure-resurs grupp toomanage ACR registret och hello Kubernetes klustret i ACS.
      ```azurecli
      az group create --name draft --location eastus
      ```

2. Skapa ett ACR-avbildningsregister med hjälp av [az acr create](/cli/azure/acr#create)
      ```azurecli
      az acr create -g draft -n draftacs --sku Basic --admin-enabled true -l eastus
      ```


## <a name="create-an-azure-container-service-with-kubernetes"></a>Skapa en Azure Container Service med Kubernetes

Nu är du redo toouse [az acs skapa](/cli/azure/acs#create) toocreate en ACS-kluster med hjälp av Kubernetes som hello `--orchestrator-type` värde.
```azurecli
az acs create --resource-group draft --name draft-kube-acs --dns-prefix draft-cluster --orchestrator-type kubernetes
```

> [!NOTE]
> Eftersom Kubernetes inte hello standardtypen för orchestrator, bör du använda hello `--orchestrator-type kubernetes` växla.

hello utdata när lyckade verkar liknande toohello följande.

```json
waiting for AAD role toopropagate.done
{
  "id": "/subscriptions/<guid>/resourceGroups/draft/providers/Microsoft.Resources/deployments/azurecli14904.93snip09",
  "name": "azurecli1496227204.9323909",
  "properties": {
    "correlationId": "<guid>",
    "debugSetting": null,
    "dependencies": [],
    "mode": "Incremental",
    "outputs": null,
    "parameters": {
      "clientSecret": {
        "type": "SecureString"
      }
    },
    "parametersLink": null,
    "providers": [
      {
        "id": null,
        "namespace": "Microsoft.ContainerService",
        "registrationState": null,
        "resourceTypes": [
          {
            "aliases": null,
            "apiVersions": null,
            "locations": [
              "westus"
            ],
            "properties": null,
            "resourceType": "containerServices"
          }
        ]
      }
    ],
    "provisioningState": "Succeeded",
    "template": null,
    "templateLink": null,
    "timestamp": "2017-05-31T10:46:29.434095+00:00"
  },
  "resourceGroup": "draft"
}
```

Nu när du har ett kluster, kan du importera hello autentiseringsuppgifter med hello [az acs kubernetes get-autentiseringsuppgifter](/cli/azure/acs/kubernetes#get-credentials) kommando. Nu har du en lokal fil för klustret, vilket är vilka Helm och utkast måste tooget sitt arbete.

## <a name="install-and-configure-draft"></a>Installera och konfigurera Draft
hello Installationsinstruktioner för utkast finns i hello [utkast databasen](https://github.com/Azure/draft/blob/master/docs/install.md). De är relativt enkla men kräver viss konfiguration eftersom det beror på [Helm](https://aka.ms/helm) toocreate och distribuera ett Helm diagram i hello Kubernetes kluster.

1. [Ladda ned och installera Helm](https://aka.ms/helm#install).
2. Använd Helm toosearch för och installera `stable/traefik`, och meddelanden om ingångs-styrenhet tooenable inkommande begäranden för din versioner.
    ```bash
    $ helm search traefik
    NAME            VERSION DESCRIPTION
    stable/traefik  1.3.0   A Traefik based Kubernetes ingress controller w...

    $ helm install stable/traefik --name ingress
    ```
    Ange en klockan på hello `ingress` domänkontrollant toocapture hello externa IP-värdet när den distribueras. Den här IP-adressen blir hello en [mappas tooyour distribution domän](#wire-up-deployment-domain) i nästa avsnitt om hello.

    ```bash
    kubectl get svc -w
    NAME                          CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
    ingress-traefik               10.0.248.104   13.64.108.240   80:31046/TCP,443:32556/TCP   1h
    kubernetes                    10.0.0.1       <none>          443/TCP                      7h
    ```

    I det här fallet hello extern IP för hello distribution domän är `13.64.108.240`. Nu kan du mappa dina toothat IP-domän.

## <a name="wire-up-deployment-domain"></a>Ställa in distributionsdomän

Draft skapar en version för varje Helm-diagram som skapas (varje program du arbetar med). Var och en hämtar ett genererat namn som används av ett utkast till som en _underdomän_ ovanpå hello rot _distribution domän_ som du bestämmer. (I det här exemplet använder vi `squillace.io` som hello distribution domän.) tooenable problemet underdomän måste du skapa en A-post för `'*'` i din DNS-posterna för din distribution domän så att alla genererade underdomän är routade toohello Kubernetes klustrets ingång-styrenhet.

En egen domän-provider har egna sätt tooassign DNS-servrar; för[Delegera din domän nameservers tooAzure DNS](../../dns/dns-delegate-domain-azure-dns.md), du vidta hello följande steg:

1. Skapa en resursgrupp för din zon.
    ```azurecli
    az group create --name squillace.io --location eastus
    {
      "id": "/subscriptions/<guid>/resourceGroups/squillace.io",
      "location": "eastus",
      "managedBy": null,
      "name": "zones",
      "properties": {
        "provisioningState": "Succeeded"
      },
      "tags": null
    }
    ```

2. Skapa en DNS-zon för din domän.
Använd hello [az nätverket DNS-zon skapa](/cli/azure/network/dns/zone#create) kommandot tooobtain hello nameservers toodelegate DNS styra tooAzure DNS för din domän.
    ```azurecli
    az network dns zone create --resource-group squillace.io --name squillace.io
    {
      "etag": "<guid>",
      "id": "/subscriptions/<guid>/resourceGroups/zones/providers/Microsoft.Network/dnszones/squillace.io",
      "location": "global",
      "maxNumberOfRecordSets": 5000,
      "name": "squillace.io",
      "nameServers": [
        "ns1-09.azure-dns.com.",
        "ns2-09.azure-dns.net.",
        "ns3-09.azure-dns.org.",
        "ns4-09.azure-dns.info."
      ],
      "numberOfRecordSets": 2,
      "resourceGroup": "squillace.io",
      "tags": {},
      "type": "Microsoft.Network/dnszones"
    }
    ```
3. Lägg till hello DNS-servrar du ges toohello domän provider för din distribution domän där du toouse Azure DNS toorepoint din domän som du vill.
4. Skapa en A-postuppsättningen post för din distribution domän mappning toohello `ingress` IP från steg 2 av hello föregående avsnitt.
    ```azurecli
    az network dns record-set a add-record --ipv4-address 13.64.108.240 --record-set-name '*' -g squillace.io -z squillace.io
    ```
hello utdata ser ut ungefär så:
    ```json
    {
      "arecords": [
        {
          "ipv4Address": "13.64.108.240"
        }
      ],
      "etag": "<guid>",
      "id": "/subscriptions/<guid>/resourceGroups/squillace.io/providers/Microsoft.Network/dnszones/squillace.io/A/*",
      "metadata": null,
      "name": "*",
      "resourceGroup": "squillace.io",
      "ttl": 3600,
      "type": "Microsoft.Network/dnszones/A"
    }
    ```

5. Konfigurera utkast toouse registret och skapa underdomäner för varje Helm diagram skapas. tooconfigure utkast, behöver du:
  - ditt Azure Container Registry-namn (`draft` i det här exemplet)
  - din registernyckel, eller ditt lösenord, från `az acr credential show -n <registry name> --output tsv --query "passwords[0].value"`.
  - hello distribution rotdomänen som du har konfigurerat toomap toohello Kubernetes ingång extern IP-adress (här, `squillace.io`)

  Anropa `draft init` och hello konfigurationsprocessen efterfrågar hello värdena ovan. hello processen ser ut ungefär som följande hello hello första gången du kör den.
 ```bash
    $ draft init
    Creating pack ruby...
    Creating pack node...
    Creating pack gradle...
    Creating pack maven...
    Creating pack php...
    Creating pack python...
    Creating pack dotnetcore...
    Creating pack golang...
    $DRAFT_HOME has been configured at /Users/ralphsquillace/.draft.

    In order tooinstall Draft, we need a bit more information...

    1. Enter your Docker registry URL (e.g. docker.io, quay.io, myregistry.azurecr.io): draft.azurecr.io
    2. Enter your username: draft
    3. Enter your password:
    4. Enter your org where Draft will push images [draft]: draft
    5. Enter your top-level domain for ingress (e.g. draft.example.com): squillace.io
    Draft has been installed into your Kubernetes Cluster.
    Happy Sailing!
    ```

Nu är du redo toodeploy ett program.


## <a name="build-and-deploy-an-application"></a>Skapa och distribuera ett program

I hello utkast lagringsplatsen [sex enkelt exempelprogram](https://github.com/Azure/draft/tree/master/examples). Klona lagringsplatsen hello och vi ska använda hello [Python exempel](https://github.com/Azure/draft/tree/master/examples/python). Ändra till katalogen för hello exempel/Python och skriv `draft create` toobuild hello program. Det bör se ut som följande exempel hello.
```bash
$ draft create
--> Python app detected
--> Ready toosail
```

hello utdata innehåller en Dockerfile och ett Helm diagram. toobuild och distribuera kan du börja skriva `draft up`. hello-utdata är omfattande, men börjar som hello följande exempel.
```bash
$ draft up
--> Building Dockerfile
Step 1 : FROM python:onbuild
onbuild: Pulling from library/python
10a267c67f42: Pulling fs layer
fb5937da9414: Pulling fs layer
9021b2326a1e: Pulling fs layer
dbed9b09434e: Pulling fs layer
ea8a37f15161: Pulling fs layer
<snip>
```

och när lyckade avslutas med något liknande toohello följande exempel.
```bash
ab68189731eb: Pushed
53c0ab0341bee12d01be3d3c192fbd63562af7f1: digest: sha256:bb0450ec37acf67ed461c1512ef21f58a500ff9326ce3ec623ce1e4427df9765 size: 2841
--> Deploying tooKubernetes
--> Status: DEPLOYED
--> Notes:

  http://gangly-bronco.squillace.io tooaccess your application

Watching local files for changes...
```

Oavsett ditt diagram namnet är, kan du nu `curl http://gangly-bronco.squillace.io` tooreceive hello svar `Hello World!`.

## <a name="next-steps"></a>Nästa steg

Nu när du har ett Kubernetes ACS-kluster kan du undersöka med [Azure Container registret](../../container-registry/container-registry-intro.md) toocreate mer och olika distributioner av det här scenariot. Du kan till exempel skapa en DNS-postuppsättning för draft._basedomain.toplevel_ som styr saker från en djupare underdomän för specifika ACS-distributioner.






