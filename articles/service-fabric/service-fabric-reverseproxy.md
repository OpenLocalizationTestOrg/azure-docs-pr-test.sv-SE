---
title: "aaaAzure Service Fabric omvänd proxy | Microsoft Docs"
description: "Använda Service Fabric omvänd proxy för kommunikation toomicroservices från inom och utanför hello-kluster."
services: service-fabric
documentationcenter: .net
author: BharatNarasimman
manager: timlt
editor: vturecek
ms.assetid: 47f5c1c1-8fc8-4b80-a081-bc308f3655d3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 08/08/2017
ms.author: bharatn
ms.openlocfilehash: 0e7835a64ccd74293c7bdd8b41deae414c83dffa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reverse-proxy-in-azure-service-fabric"></a>Omvänd proxy i Azure Service Fabric
hello omvänd proxy som är inbyggd i Azure Service Fabric-adresser mikrotjänster i hello Service Fabric-kluster som exponerar HTTP-slutpunkter.

## <a name="microservices-communication-model"></a>Mikrotjänster kommunikation modellen
Mikrotjänster i Service Fabric normalt körs på en delmängd av virtuella datorer i hello klustret och kan flytta från en virtuell dator tooanother av olika skäl. Därför kan hello slutpunkter för mikrotjänster ändras dynamiskt. hello typiskt mönster toocommunicate toohello mikrotjänster är hello följande lösa slinga:

1. Lös hello tjänstlokalisering ursprungligen via hello naming service.
2. Ansluta toohello service.
3. Ta reda på hello orsaken för anslutningsfel och Lös hello tjänstlokalisering igen vid behov.

Den här processen omfattar vanligtvis wrapping klientsidan kommunikations-bibliotek i en omförsöksslinga som implementerar hello-upplösning och försök igen principer.
Mer information finns i [Connect och kommunicera med tjänster](service-fabric-connect-and-communicate-with-services.md).

### <a name="communicating-by-using-hello-reverse-proxy"></a>Kommunicerar med hjälp av hello omvänd proxy
hello omvänd proxy i Service Fabric körs på alla hello-noder i klustret hello. Den utför hello hela tjänsten lösningsprocessen för klientens räkning och vidarebefordrar hello klientbegäran. Klienter som körs på klustret hello kan så använder alla klientsidan http-kommunikation bibliotek tootalk toohello Måltjänsten med hjälp hello omvänd proxy att hello körs lokalt på samma nod.

![Intern kommunikation][1]

## <a name="reaching-microservices-from-outside-hello-cluster"></a>Nå mikrotjänster från utanför hello-kluster
hello extern kommunikation standardmodell för mikrotjänster är en opt-in-modell där varje tjänst inte kan nås direkt från externa klienter. [Azure belastningsutjämnare](../load-balancer/load-balancer-overview.md), vilket är en nätverksgräns mellan mikrotjänster och externa klienter utför nätverksadresser och vidarebefordrar externa begär toointernal IP:port slutpunkter. toomake en mikrotjänster endpoint direkt åtkomliga tooexternal klienter måste du först konfigurera belastningsutjämnaren tooforward trafik tooeach port som hello tjänsten använder i hello kluster. De flesta mikrotjänster, särskilt tillståndskänslig mikrotjänster Direktmigrering inte dessutom på alla noder i klustret hello. Hej mikrotjänster kan flytta mellan noder på redundanskluster. I sådana fall belastningsutjämnaren effektivt kan inte fastställa hello plats för hello målnoden för hello repliker toowhich den ska vidarebefordra trafik.

### <a name="reaching-microservices-via-hello-reverse-proxy-from-outside-hello-cluster"></a>Nå mikrotjänster via hello omvänd proxy från utanför hello kluster
Du kan konfigurera precis hello port hello omvänd proxy i belastningsutjämnaren i stället för att konfigurera hello-port för en enskild tjänst i belastningsutjämnaren. Den här konfigurationen kan klienter utanför hello kluster nå tjänster i hello kluster med hjälp av hello omvänd proxy utan ytterligare konfiguration.

![Extern kommunikation][0]

> [!WARNING]
> När du konfigurerar hello omvänd proxy-port i belastningsutjämnaren adresseras alla mikrotjänster i hello kluster som Exponerar en HTTP-slutpunkt från utanför hello kluster.
>
>


## <a name="uri-format-for-addressing-services-by-using-hello-reverse-proxy"></a>URI-format för adressering tjänster med hjälp av hello omvänd proxy
hello omvänd proxy använder en specifik uniform resource identifier (URI) format tooidentify hello partition toowhich hello inkommande tjänstbegäran ska vidarebefordras:

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&ListenerName=<listenerName>&TargetReplicaSelector=<targetReplicaSelector>&Timeout=<timeout_in_seconds>
```

* **http (s):** hello omvänd proxy kan vara konfigurerade tooaccept HTTP eller HTTPS-trafik. HTTPS-vidarebefordran finns för[ansluta tooa säker service med hello omvänd proxy](service-fabric-reverseproxy-configure-secure-communication.md) när du har en omvänd proxy installationsprogrammet toolisten på HTTPS.
* **Klustret fullständigt kvalificerade domännamnet (FQDN) | intern IP-adress:** för externa klienter kan du konfigurera hello omvänd proxy så att den kan nås via hello klustret domän, till exempel mycluster.eastus.cloudapp.azure.com. Som standard körs hello omvänd proxy på varje nod. För intern trafik kan hello omvänd proxy nås på localhost eller på alla interna noden IP-adresser, t.ex 10.0.0.1.
* **Port:** hello porten, till exempel 19081, som har angetts för hello omvänd proxy.
* **ServiceInstanceName:** är hello fullständigt kvalificerade namnet på hello distribuerat service-instans som du försöker tooreach utan hello ”fabric: /” schema. Till exempel tooreach hello *fabric: / myapp/myservice/* tjänsten, som du vill använda *myapp/myservice*.

    hello service-instansen är skiftlägeskänsliga. Med hjälp av ett annat skiftläge för hello service instansnamn i hello URL orsakar hello begäranden toofail med 404 (inget hittas).
* **Suffix sökväg:** detta är hello faktiska URL-sökväg som *myapi/värden/Lägg till/3*, för hello-tjänst som du vill tooconnect till.
* **PartitionKey:** för en partitionerad tjänst är hello beräknade partitionsnyckel för hello-partition som du vill tooreach. Observera att detta *inte* hello partitions-ID-GUID. Den här parametern krävs inte för tjänster som använder hello singleton-partitionsschema.
* **PartitionKind:** detta är hello partitionsschema för tjänsten. Detta kan vara 'Int64Range' eller 'Med namnet'. Den här parametern krävs inte för tjänster som använder hello singleton-partitionsschema.
* **ListenerName** hello slutpunkter från hello-tjänsten är hello formatet {”slutpunkter”: {”Listener1”: ”slutpunkt 1”, ”Listener2”: ”Endpoint2”...}}. När hello-tjänsten visar flera slutpunkter, identifierar hello slutpunkt som hello klientbegäran ska vidarebefordras till. Detta kan utelämnas om hello-tjänsten har endast en lyssnare.
* **TargetReplicaSelector** anger hur hello replikuppsättningens eller instans måste väljas.
  * När hello Måltjänsten är tillståndskänslig hello TargetReplicaSelector kan vara något av följande hello: 'PrimaryReplica', 'RandomSecondaryReplica' eller 'RandomReplica'. Om den här parametern anges är hello standardvärdet 'PrimaryReplica'.
  * När hello Måltjänsten är tillståndslös hämtar en slumpmässig instans av hello partition tooforward hello tjänstbegäran för omvänd proxy.
* **Timeout:** anger hello timeout för hello HTTP-begäran som skapats av hello omvänd proxy toohello tjänst på uppdrag av hello klientbegäran. hello standardvärdet är 60 sekunder. Det här är en valfri parameter.

### <a name="example-usage"></a>Exempel på användning
Låt oss ta hello exempelvis *fabric: / MyApp/MyService* tjänst som öppnar en HTTP-lyssnare på hello följande URL:

```
http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

Följande är hello resurser för hello-tjänsten:

* `/index.html`
* `/api/users/<userId>`

Om hello tjänsten använder hello singleton partitioneringsschema, hello *PartitionKey* och *PartitionKind* frågan string-parametrar är inte obligatoriska och hello-tjänsten kan nås med hjälp av hello-gateway som:

* Externt:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService`
* Internt:`http://localhost:19081/MyApp/MyService`

Om hello tjänsten använder hello Uniform Int64 partitioneringsschema, hello *PartitionKey* och *PartitionKind* frågan string-parametrar måste vara används tooreach en partition av hello-tjänsten:

* Externt:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`
* Internt:`http://localhost:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`

tooreach hello resurser som hello-tjänsten visar Placera bara hello resursens sökväg efter hello tjänstnamnet i hello-URL:

* Externt:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`
* Internt:`http://localhost:19081/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`

hello gateway kommer sedan att vidarebefordra dessa begäranden toohello tjänst-URL:

* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a>Särskild hantering för delning av port tjänster
Azure Application Gateway försöker tooresolve en tjänst adressen igen och försök hello begäran när en tjänst inte kan nås. Detta är en större fördel av Programgateway eftersom klientkod inte behöver tooimplement sin egen service-upplösning och lösa loop.

I allmänhet när en tjänst inte kan nås, har hello tjänstinstansen eller replik flyttats tooa annan nod som en del av sin normala livscykel. När detta sker få Programgateway ett nätverk anslutning fel som anger att en slutpunkt är inte längre öppna på hello ursprungligen matcha adress.

Dock replikeringar eller instanser av tjänsten kan dela en värdprocess och kan också dela en port när finns en http.sys-baserade webbservern, inklusive:

* [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
* [ASP.NET Core WebListener](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
* [Katana](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

I det här fallet är det troligt webbservern hello finns i hello värdprocess och svarar toorequests, men hello löst tjänstinstansen eller repliken är inte längre tillgänglig på hello värden. I det här fallet får hello gateway ett HTTP 404-svar från hello webbservern. Därför har en HTTP 404 två distinkta innebörd:

- Fall #1: hello-adress är korrekt, men hello-resurs som hello begärd användare finns inte.
- Fall #2: hello-adress är felaktig och hello-resurs som hello begärd användare kan finnas på en annan nod.

hello första fall har en normal HTTP 404 som anses vara ett användarfel. I andra fall hello har dock hello användaren begärde en resurs som finns. Programgateway kunde toolocate den eftersom hello-tjänsten har flyttats. Programmet måste tooresolve hello gatewayadress igen och försök igen hello begäran.

Programgateway innebär behöver ett sätt toodistinguish mellan dessa två fall. toomake att distinktion en ledtråd från hello-server krävs.

* Som standard Programgateway förutsätter fall #2 och försöker tooresolve och utfärda hello begäran igen.
* tooindicate fall #1 tooApplication Gateway hello-tjänsten ska returnera följande HTTP-Svarsrubrik hello:

  `X-ServiceFabric : ResourceNotFound`

Den här HTTP-Svarsrubrik visar en normal HTTP 404-situation i vilken hello begärda resursen inte finns och Programgateway försöker inte tooresolve hello-adress igen.

## <a name="setup-and-configuration"></a>Installation och konfiguration

### <a name="enable-reverse-proxy-via-azure-portal"></a>Aktivera omvänd proxy via Azure-portalen

Azure-portalen innehåller en omvänd proxy för alternativet-tooenable när du skapar ett nytt Service Fabric-kluster.
Under **skapar Service Fabric-kluster**, steg 2: klusterkonfiguration, konfiguration av noden typ, markera kryssrutan för hello för ”aktivera omvänd proxy”.
För att konfigurera säker omvänd proxy, SSL-certifikat kan anges i steg3: säkerhet, konfigurera säkerhetsinställningar för klustret, väljer hello kryssrutan för ”innehåller ett SSL-certifikat för omvänd proxy” och ange hello-certifikatinformation.

### <a name="enable-reverse-proxy-via-azure-resource-manager-templates"></a>Aktivera omvänd proxy via Azure Resource Manager-mallar

Du kan använda hello [Azure Resource Manager-mall](service-fabric-cluster-creation-via-arm.md) tooenable hello omvänd proxy i Service Fabric för hello klustret.

Se för[konfigurera HTTPS omvänd Proxy i ett kluster för säker](https://github.com/ChackDan/Service-Fabric/tree/master/ARM Templates/ReverseProxySecureSample#configure-https-reverse-proxy-in-a-secure-cluster) för Azure Resource Manager mallen exempel tooconfigure säker omvänd proxy med förnya ett certifikat och hantering av certifikatet.

Först får du hello mall för hello klustret som du vill toodeploy. Du kan använda hello exempelmallarna, eller så kan du skapa en anpassad mall för hanteraren för filserverresurser. Sedan kan du aktivera hello omvänd proxy genom att använda hello följande steg:

1. Definiera en port för hello omvänd proxy i hello [parametrar avsnittet](../azure-resource-manager/resource-group-authoring-templates.md) för hello mall.

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19081,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. Ange hello-port för varje hello nodetype objekt i hello **klustret** [typen avsnittet](../azure-resource-manager/resource-group-authoring-templates.md).

    hello port identifieras av hello parameternamn, reverseProxyEndpointPort.

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "reverseProxyEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```
3. tooaddress hello omvänd proxy från utanför hello Azure kluster, ange hello Azure belastningsutjämnare regler för hello-port som du angav i steg 1.

    ```json
    {
        "apiVersion": "[variables('lbApiVersion')]",
        "type": "Microsoft.Network/loadBalancers",
        ...
        ...
        "loadBalancingRules": [
            ...
            {
                "name": "LBSFReverseProxyRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID0')]"
                    },
                    "backendPort": "[parameters('SFReverseProxyPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig0')]"
                    },
                    "frontendPort": "[parameters('SFReverseProxyPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID0'),'/probes/SFReverseProxyProbe')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            ...
            {
                "name": "SFReverseProxyProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port":     "[parameters('SFReverseProxyPort')]",
                    "protocol": "tcp"
                }
            }  
        ]
    }
    ```
4. tooconfigure SSL-certifikat på hello port hello omvänd proxy, lägga till hello certifikat toohello ***reverseProxyCertificate*** egenskap i hello **klustret** [typen avsnittet](../resource-group-authoring-templates.md).

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "reverseProxyCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

### <a name="supporting-a-reverse-proxy-certificate-thats-different-from-hello-cluster-certificate"></a>Stöd för en omvänd proxy-certifikat som skiljer sig från hello klustret certifikatet
 Om hello omvänd proxycertifikatet skiljer sig från hello-certifikat som skyddar hello klustret sedan angiven hello tidigare certifikatet ska installeras på den virtuella datorn hello och lagt till toohello åtkomstkontrollistan (ACL) så att Service Fabric kan komma åt den. Detta kan göras i hello **virtualMachineScaleSets** [typen avsnittet](../resource-group-authoring-templates.md). Lägg till att certifikatet toohello osProfile för installation. hello tillägget avsnitt i hello mall kan uppdatera hello certifikat i hello ACL.

  ```json
  {
    "apiVersion": "[variables('vmssApiVersion')]",
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    ....
      "osProfile": {
          "adminPassword": "[parameters('adminPassword')]",
          "adminUsername": "[parameters('adminUsername')]",
          "computernamePrefix": "[parameters('vmNodeType0Name')]",
          "secrets": [
            {
              "sourceVault": {
                "id": "[parameters('sfReverseProxySourceVaultValue')]"
              },
              "vaultCertificates": [
                {
                  "certificateStore": "[parameters('sfReverseProxyCertificateStoreValue')]",
                  "certificateUrl": "[parameters('sfReverseProxyCertificateUrlValue')]"
                }
              ]
            }
          ]
        }
   ....
   "extensions": [
          {
              "name": "[concat(parameters('vmNodeType0Name'),'_ServiceFabricNode')]",
              "properties": {
                      "type": "ServiceFabricNode",
                      "autoUpgradeMinorVersion": false,
                      ...
                      "publisher": "Microsoft.Azure.ServiceFabric",
                      "settings": {
                        "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                        "nodeTypeRef": "[parameters('vmNodeType0Name')]",
                        "dataPath": "D:\\\\SvcFab",
                        "durabilityLevel": "Bronze",
                        "testExtension": true,
                        "reverseProxyCertificate": {
                          "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                          "x509StoreName": "[parameters('sfReverseProxyCertificateStoreValue')]"
                        },
                  },
                  "typeHandlerVersion": "1.0"
              }
          },
      ]
    }
  ```
> [!NOTE]
> När du använder certifikat som skiljer sig från hello klustret certifikat tooenable hello omvänd proxy på ett befintligt kluster, installera hello omvänd proxycertifikatet och uppdatera hello ACL på hello klustret innan du aktiverar hello omvänd proxy. Fullständig hello [Azure Resource Manager-mall](service-fabric-cluster-creation-via-arm.md) distribution genom att använda hello-inställningar som anges tidigare innan du startar en omvänd proxy för distribution tooenable hello i steg 1 – 4.

## <a name="next-steps"></a>Nästa steg
* Se ett exempel på HTTP-kommunikation mellan tjänster i en [exempelprojektet på GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).
* [Vidarebefordran toosecure HTTP-tjänsten med hello omvänd proxy](service-fabric-reverseproxy-configure-secure-communication.md)
* [RPC-anrop med Reliable Services fjärrkommunikation](service-fabric-reliable-services-communication-remoting.md)
* [Webb-API som använder OWIN i Reliable Services](service-fabric-reliable-services-communication-webapi.md)
* [WCF-kommunikation med hjälp av Reliable Services](service-fabric-reliable-services-communication-wcf.md)
* Ytterligare omvänd proxy konfigurationsalternativ finns ApplicationGateway/http-avsnittet i [anpassa Service Fabric-klusterinställningar](service-fabric-cluster-fabric-settings.md).

[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png
