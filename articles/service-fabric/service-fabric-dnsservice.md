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
# <a name="dns-service-in-azure-service-fabric"></a><span data-ttu-id="42ed1-103">DNS-tjänsten i Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="42ed1-103">DNS Service in Azure Service Fabric</span></span>
<span data-ttu-id="42ed1-104">hello DNS-tjänsten är en valfri systemtjänst som du kan aktivera i klustret-toodiscover andra tjänster som använder hello DNS-protokollet.</span><span class="sxs-lookup"><span data-stu-id="42ed1-104">hello DNS Service is an optional system service that you can enable in your cluster toodiscover other services using hello DNS protocol.</span></span>

<span data-ttu-id="42ed1-105">Många tjänster, särskilt av tjänster, kan ha ett befintligt URL-namn och som kan tooresolve dem med hello standard DNS-protokollet (i stället för hello namngivningstjänst protocol) är önskvärt, särskilt i ”lyfta och flytta” scenarier.</span><span class="sxs-lookup"><span data-stu-id="42ed1-105">Many services, especially containerized services, can have an existing URL name, and being able tooresolve them using hello standard DNS protocol (rather than hello Naming Service protocol) is desirable, particularly in "lift and shift" scenarios.</span></span> <span data-ttu-id="42ed1-106">hello DNS-tjänsten kan du toomap DNS-namn tooa tjänstnamn och därför matcha IP-adresser för slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="42ed1-106">hello DNS service enables you toomap DNS names tooa service name and hence resolve endpoint IP addresses.</span></span> 

<span data-ttu-id="42ed1-107">hello DNS-tjänsten matchar DNS-namn tooservice namn, som i sin tur kan matchas med hello namngivningstjänst tooreturn hello tjänstslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="42ed1-107">hello DNS service maps DNS names tooservice names, which in turn are resolved by hello Naming Service tooreturn hello service endpoint.</span></span> <span data-ttu-id="42ed1-108">hello DNS-namn för hello tjänsten tillhandahålls för närvarande hello skapas.</span><span class="sxs-lookup"><span data-stu-id="42ed1-108">hello DNS name for hello service is provided at hello time of creation.</span></span> 

![slutpunkter][0]

## <a name="enabling-hello-dns-service"></a><span data-ttu-id="42ed1-110">Aktivera hello DNS-tjänsten</span><span class="sxs-lookup"><span data-stu-id="42ed1-110">Enabling hello DNS service</span></span>
<span data-ttu-id="42ed1-111">Du måste först tooenable hello DNS-tjänsten i klustret.</span><span class="sxs-lookup"><span data-stu-id="42ed1-111">First you need tooenable hello DNS service in your cluster.</span></span> <span data-ttu-id="42ed1-112">Hämta hello mall för hello kluster som du vill toodeploy.</span><span class="sxs-lookup"><span data-stu-id="42ed1-112">Get hello template for hello cluster that you want toodeploy.</span></span> <span data-ttu-id="42ed1-113">Du kan antingen använda hello [exempel mallar](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) eller skapa en Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="42ed1-113">You can either use hello [sample templates](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype)  or create a Resource Manager template.</span></span> <span data-ttu-id="42ed1-114">Du kan aktivera hello DNS-tjänsten med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="42ed1-114">You can enable hello DNS service with hello following steps:</span></span>

1. <span data-ttu-id="42ed1-115">Kontrollera att hello `apiversion` har angetts för`2017-07-01-preview` för hello `Microsoft.ServiceFabric/clusters` resurs, och om inte, uppdatera det som visas i följande fragment hello:</span><span class="sxs-lookup"><span data-stu-id="42ed1-115">Check that hello `apiversion` is set too`2017-07-01-preview` for hello `Microsoft.ServiceFabric/clusters` resource, and if not, update it as shown in hello following snippet:</span></span>

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. <span data-ttu-id="42ed1-116">Nu aktivera hello DNS-tjänsten genom att lägga till följande hello `addonFeatures` efter hello `fabricSettings` avsnittet som visas i följande fragment hello:</span><span class="sxs-lookup"><span data-stu-id="42ed1-116">Now enable hello DNS service by adding hello following `addonFeatures` section after hello `fabricSettings` section as shown in hello following snippet:</span></span> 

    ```json
        "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "DnsService"
        ],
    ```

3. <span data-ttu-id="42ed1-117">När du har uppdaterat mallen för kluster med hello föregående ändringar, använda dem och låt hello uppgradering slutförd.</span><span class="sxs-lookup"><span data-stu-id="42ed1-117">Once you have updated your cluster template with hello preceding changes, apply them and let hello upgrade complete.</span></span> <span data-ttu-id="42ed1-118">När installationen är klar startar hello DNS systemtjänst körs i klustret som kallas `fabric:/System/DnsService` under system service-avsnittet i hello Service Fabric explorer.</span><span class="sxs-lookup"><span data-stu-id="42ed1-118">Once complete, hello DNS system service starts running in your cluster that is called `fabric:/System/DnsService` under system service section in hello Service Fabric explorer.</span></span> 

<span data-ttu-id="42ed1-119">Alternativt kan du aktivera hello DNS-tjänsten via hello portal Hej när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="42ed1-119">Alternatively, you can enable hello DNS service through hello portal at hello time of cluster creation.</span></span> <span data-ttu-id="42ed1-120">hello DNS-tjänsten kan aktiveras genom att kontrollera hello rutan för `Include DNS service` i hello `Cluster configuration` menyn som visas i följande skärmbild hello:</span><span class="sxs-lookup"><span data-stu-id="42ed1-120">hello DNS service can be enabled by checking hello box for `Include DNS service` in hello `Cluster configuration` menu as shown in hello following screenshot:</span></span>

![Aktivera DNS-tjänsten via hello portal][2]


## <a name="setting-hello-dns-name-for-your-service"></a><span data-ttu-id="42ed1-122">Ange hello DNS-namn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="42ed1-122">Setting hello DNS name for your service</span></span>
<span data-ttu-id="42ed1-123">När hello DNS-tjänsten körs i klustret kan du ange ett DNS-namn för dina tjänster deklarativt för standardtjänster i hello `ApplicationManifest.xml` eller via Powershell-kommandon.</span><span class="sxs-lookup"><span data-stu-id="42ed1-123">Once hello DNS service is running in your cluster, you can set a DNS name for your services either declaratively for default services in hello `ApplicationManifest.xml` or through Powershell commands.</span></span>

### <a name="setting-hello-dns-name-for-a-default-service-in-hello-applicationmanifestxml"></a><span data-ttu-id="42ed1-124">Ange hello DNS-namnet för en standardtjänst i hello ApplicationManifest.xml</span><span class="sxs-lookup"><span data-stu-id="42ed1-124">Setting hello DNS name for a default service in hello ApplicationManifest.xml</span></span>
<span data-ttu-id="42ed1-125">Öppna projektet i Visual Studio eller din favorit redigerare och hello `ApplicationManifest.xml` fil.</span><span class="sxs-lookup"><span data-stu-id="42ed1-125">Open your project in Visual Studio, or your favorite editor, and open hello `ApplicationManifest.xml` file.</span></span> <span data-ttu-id="42ed1-126">Gå toohello standard services avsnittet och för varje service Lägg till hello `ServiceDnsName` attribut.</span><span class="sxs-lookup"><span data-stu-id="42ed1-126">Go toohello default services section, and for each service add hello `ServiceDnsName` attribute.</span></span> <span data-ttu-id="42ed1-127">hello som följande exempel visar hur tooset hello hello tjänsten DNS-namn för`service1.application1`</span><span class="sxs-lookup"><span data-stu-id="42ed1-127">hello following example shows how tooset hello DNS name of hello service too`service1.application1`</span></span>

```xml
    <Service Name="Stateless1" ServiceDnsName="service1.application1">
    <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
        <SingletonPartition />
    </StatelessService>
    </Service>
```
<span data-ttu-id="42ed1-128">När hello programmet distribueras, visar hello tjänstinstansen i hello Service Fabric explorer hello DNS-namn för den här instansen, som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="42ed1-128">Once hello application is deployed, hello service instance in hello Service Fabric explorer shows hello DNS name for this instance, as shown in hello following figure:</span></span> 

![slutpunkter][1]

### <a name="setting-hello-dns-name-for-a-service-using-powershell"></a><span data-ttu-id="42ed1-130">Ange hello DNS-namn för en tjänst med Powershell</span><span class="sxs-lookup"><span data-stu-id="42ed1-130">Setting hello DNS name for a service using Powershell</span></span>
<span data-ttu-id="42ed1-131">Du kan ange hello DNS-namnet för en tjänst när du skapar den med hjälp av hello `New-ServiceFabricService` Powershell.</span><span class="sxs-lookup"><span data-stu-id="42ed1-131">You can set hello DNS name for a service when creating it using hello `New-ServiceFabricService` Powershell.</span></span> <span data-ttu-id="42ed1-132">hello följande exempel skapas en ny tillståndslös tjänst med hello DNS-namn`service1.application1`</span><span class="sxs-lookup"><span data-stu-id="42ed1-132">hello following example creates a new stateless service with hello DNS name `service1.application1`</span></span>

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

## <a name="using-dns-in-your-services"></a><span data-ttu-id="42ed1-133">Med hjälp av DNS i dina tjänster</span><span class="sxs-lookup"><span data-stu-id="42ed1-133">Using DNS in your services</span></span>
<span data-ttu-id="42ed1-134">Om du distribuerar mer än en tjänst hittar du hello slutpunkter av andra tjänster toocommunicate med genom att använda en DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="42ed1-134">If you deploy more than one service, you can find hello endpoints of other services toocommunicate with  by using a DNS name.</span></span> <span data-ttu-id="42ed1-135">hello DNS-tjänsten är endast tillämplig toostateless tjänster, eftersom hello DNS-protokollet inte kan kommunicera med tillståndskänsliga tjänster.</span><span class="sxs-lookup"><span data-stu-id="42ed1-135">hello DNS service is only applicable toostateless services, since hello DNS protocol cannot communicate with stateful services.</span></span> <span data-ttu-id="42ed1-136">Du kan använda hello inbyggda omvänd proxy för HTTP-anrop toocall en viss tjänst partition för tillståndskänsliga tjänster.</span><span class="sxs-lookup"><span data-stu-id="42ed1-136">For stateful services, you can use hello built-in reverse proxy for http calls toocall a particular service partition.</span></span>

<span data-ttu-id="42ed1-137">hello följande kod visar hur toocall en annan tjänst som är helt enkelt en vanlig http samtal där du tillhandahålla hello port och en valfri sökväg som en del av hello-URL.</span><span class="sxs-lookup"><span data-stu-id="42ed1-137">hello following code shows how toocall another service, which is simply a regular http call where you provide hello port and any optional path as part of hello URL.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="42ed1-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="42ed1-138">Next steps</span></span>
<span data-ttu-id="42ed1-139">Mer information om kommunikation inom hello kluster med [ansluta och kommunicera med tjänster](service-fabric-connect-and-communicate-with-services.md)</span><span class="sxs-lookup"><span data-stu-id="42ed1-139">Learn more about service communication within hello cluster with  [connect and communicate with services](service-fabric-connect-and-communicate-with-services.md)</span></span>

[0]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[1]: ./media/service-fabric-dnsservice/servicefabric-explorer-dns.PNG
[2]: ./media/service-fabric-dnsservice/DNSService.PNG
