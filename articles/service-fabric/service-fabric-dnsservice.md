---
title: "aaaAzure Service Fabric DNS-tjänsten | Microsoft Docs"
description: "Använda Service Fabric DNS-tjänsten för identifiering av mikrotjänster från i hello kluster."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: vturecek
ms.assetid: 47f5c1c1-8fc8-4b80-a081-bc308f3655d3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/27/2017
ms.author: msfussell
ms.openlocfilehash: fa536f0e41f52c4942702d0a1bdcd3ed7d418d6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="dns-service-in-azure-service-fabric"></a>DNS-tjänsten i Azure Service Fabric
hello DNS-tjänsten är en valfri systemtjänst som du kan aktivera i klustret-toodiscover andra tjänster som använder hello DNS-protokollet.

Många tjänster, särskilt av tjänster, kan ha ett befintligt URL-namn och som kan tooresolve dem med hello standard DNS-protokollet (i stället för hello namngivningstjänst protocol) är önskvärt, särskilt i ”lyfta och flytta” scenarier. hello DNS-tjänsten kan du toomap DNS-namn tooa tjänstnamn och därför matcha IP-adresser för slutpunkt. 

hello DNS-tjänsten matchar DNS-namn tooservice namn, som i sin tur kan matchas med hello namngivningstjänst tooreturn hello tjänstslutpunkten. hello DNS-namn för hello tjänsten tillhandahålls för närvarande hello skapas. 

![slutpunkter][0]

## <a name="enabling-hello-dns-service"></a>Aktivera hello DNS-tjänsten
Du måste först tooenable hello DNS-tjänsten i klustret. Hämta hello mall för hello kluster som du vill toodeploy. Du kan antingen använda hello [exempel mallar](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) eller skapa en Resource Manager-mall. Du kan aktivera hello DNS-tjänsten med hello följande steg:

1. Kontrollera att hello `apiversion` har angetts för`2017-07-01-preview` för hello `Microsoft.ServiceFabric/clusters` resurs, och om inte, uppdatera det som visas i följande fragment hello:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Nu aktivera hello DNS-tjänsten genom att lägga till följande hello `addonFeatures` efter hello `fabricSettings` avsnittet som visas i följande fragment hello: 

    ```json
        "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "DnsService"
        ],
    ```

3. När du har uppdaterat mallen för kluster med hello föregående ändringar, använda dem och låt hello uppgradering slutförd. När installationen är klar startar hello DNS systemtjänst körs i klustret som kallas `fabric:/System/DnsService` under system service-avsnittet i hello Service Fabric explorer. 

Alternativt kan du aktivera hello DNS-tjänsten via hello portal Hej när klustret skapas. hello DNS-tjänsten kan aktiveras genom att kontrollera hello rutan för `Include DNS service` i hello `Cluster configuration` menyn som visas i följande skärmbild hello:

![Aktivera DNS-tjänsten via hello portal][2]


## <a name="setting-hello-dns-name-for-your-service"></a>Ange hello DNS-namn för tjänsten
När hello DNS-tjänsten körs i klustret kan du ange ett DNS-namn för dina tjänster deklarativt för standardtjänster i hello `ApplicationManifest.xml` eller via Powershell-kommandon.

### <a name="setting-hello-dns-name-for-a-default-service-in-hello-applicationmanifestxml"></a>Ange hello DNS-namnet för en standardtjänst i hello ApplicationManifest.xml
Öppna projektet i Visual Studio eller din favorit redigerare och hello `ApplicationManifest.xml` fil. Gå toohello standard services avsnittet och för varje service Lägg till hello `ServiceDnsName` attribut. hello som följande exempel visar hur tooset hello hello tjänsten DNS-namn för`service1.application1`

```xml
    <Service Name="Stateless1" ServiceDnsName="service1.application1">
    <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
        <SingletonPartition />
    </StatelessService>
    </Service>
```
När hello programmet distribueras, visar hello tjänstinstansen i hello Service Fabric explorer hello DNS-namn för den här instansen, som visas i följande bild hello: 

![slutpunkter][1]

### <a name="setting-hello-dns-name-for-a-service-using-powershell"></a>Ange hello DNS-namn för en tjänst med Powershell
Du kan ange hello DNS-namnet för en tjänst när du skapar den med hjälp av hello `New-ServiceFabricService` Powershell. hello följande exempel skapas en ny tillståndslös tjänst med hello DNS-namn`service1.application1`

```powershell
    New-ServiceFabricService `
    -Stateless `
    -PartitionSchemeSingleton `
    -ApplicationName `fabric:/application1 `
    -ServiceName fabric:/application1/service1 `
    -ServiceTypeName Service1Type `
    -InstanceCount 1 `
    -ServiceDnsName service1.application1
```

## <a name="using-dns-in-your-services"></a>Med hjälp av DNS i dina tjänster
Om du distribuerar mer än en tjänst hittar du hello slutpunkter av andra tjänster toocommunicate med genom att använda en DNS-namn. hello DNS-tjänsten är endast tillämplig toostateless tjänster, eftersom hello DNS-protokollet inte kan kommunicera med tillståndskänsliga tjänster. Du kan använda hello inbyggda omvänd proxy för HTTP-anrop toocall en viss tjänst partition för tillståndskänsliga tjänster.

hello följande kod visar hur toocall en annan tjänst som är helt enkelt en vanlig http samtal där du tillhandahålla hello port och en valfri sökväg som en del av hello-URL.

```csharp
public class ValuesController : Controller
{
    // GET api
    [HttpGet]
    public async Task<string> Get()
    {
        string result = "";
        try
        {
            Uri uri = new Uri("http://service1.application1:8080/api/values");
            HttpClient client = new HttpClient();
            var response = await client.GetAsync(uri);
            result = await response.Content.ReadAsStringAsync();
            
        }
        catch (Exception e)
        {
            Console.Write(e.Message);
        }

        return result;
    }
}
```

## <a name="next-steps"></a>Nästa steg
Mer information om kommunikation inom hello kluster med [ansluta och kommunicera med tjänster](service-fabric-connect-and-communicate-with-services.md)

[0]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[1]: ./media/service-fabric-dnsservice/servicefabric-explorer-dns.PNG
[2]: ./media/service-fabric-dnsservice/DNSService.PNG
