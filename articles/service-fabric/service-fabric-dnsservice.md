---
title: "Azure Service Fabric DNS-tjänsten | Microsoft Docs"
description: "Använda Service Fabric DNS-tjänsten för identifiering av mikrotjänster från i klustret."
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
ms.openlocfilehash: 9871bc5aa4e74ab0faef401d67c4e9558eb5e14b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="dns-service-in-azure-service-fabric"></a><span data-ttu-id="ce102-103">DNS-tjänsten i Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ce102-103">DNS Service in Azure Service Fabric</span></span>
<span data-ttu-id="ce102-104">DNS-tjänsten är en valfri systemtjänst som du kan aktivera i klustret för att identifiera andra tjänster med hjälp av DNS-protokollet.</span><span class="sxs-lookup"><span data-stu-id="ce102-104">The DNS Service is an optional system service that you can enable in your cluster to discover other services using the DNS protocol.</span></span>

<span data-ttu-id="ce102-105">Många tjänster, särskilt av tjänster, kan ha ett befintligt URL-namn och att kunna lösa dem med hjälp av DNS-standardprotokollet (i stället för protokollet Naming Service) är önskvärt, särskilt i ”lyfta och flytta” scenarier.</span><span class="sxs-lookup"><span data-stu-id="ce102-105">Many services, especially containerized services, can have an existing URL name, and being able to resolve them using the standard DNS protocol (rather than the Naming Service protocol) is desirable, particularly in "lift and shift" scenarios.</span></span> <span data-ttu-id="ce102-106">DNS-tjänsten kan du mappa DNS-namn till ett namn och därför matcha IP-adresser för slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="ce102-106">The DNS service enables you to map DNS names to a service name and hence resolve endpoint IP addresses.</span></span> 

<span data-ttu-id="ce102-107">Tjänsten DNS matchar DNS-namn till tjänstnamn som i sin tur kan matchas med namngivningstjänst att returnera tjänstslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="ce102-107">The DNS service maps DNS names to service names, which in turn are resolved by the Naming Service to return the service endpoint.</span></span> <span data-ttu-id="ce102-108">DNS-namn för tjänsten tillhandahålls vid tidpunkten för skapandet.</span><span class="sxs-lookup"><span data-stu-id="ce102-108">The DNS name for the service is provided at the time of creation.</span></span> 

![slutpunkter][0]

## <a name="enabling-the-dns-service"></a><span data-ttu-id="ce102-110">Aktivera DNS-tjänsten</span><span class="sxs-lookup"><span data-stu-id="ce102-110">Enabling the DNS service</span></span>
<span data-ttu-id="ce102-111">Du måste först aktivera tjänsten DNS i klustret.</span><span class="sxs-lookup"><span data-stu-id="ce102-111">First you need to enable the DNS service in your cluster.</span></span> <span data-ttu-id="ce102-112">Hämta mallen för det kluster som du vill distribuera.</span><span class="sxs-lookup"><span data-stu-id="ce102-112">Get the template for the cluster that you want to deploy.</span></span> <span data-ttu-id="ce102-113">Du kan använda den [exempel mallar](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) eller skapa en Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="ce102-113">You can either use the [sample templates](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype)  or create a Resource Manager template.</span></span> <span data-ttu-id="ce102-114">Du kan aktivera DNS-tjänsten med följande steg:</span><span class="sxs-lookup"><span data-stu-id="ce102-114">You can enable the DNS service with the following steps:</span></span>

1. <span data-ttu-id="ce102-115">Kontrollera att den `apiversion` är inställd på `2017-07-01-preview` för den `Microsoft.ServiceFabric/clusters` resursen, och om inte, uppdatera det som visas i följande utdrag:</span><span class="sxs-lookup"><span data-stu-id="ce102-115">Check that the `apiversion` is set to `2017-07-01-preview` for the `Microsoft.ServiceFabric/clusters` resource, and if not, update it as shown in the following snippet:</span></span>

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. <span data-ttu-id="ce102-116">Nu aktivera DNS-tjänsten genom att lägga till följande `addonFeatures` avsnittet efter den `fabricSettings` avsnittet som visas i följande utdrag:</span><span class="sxs-lookup"><span data-stu-id="ce102-116">Now enable the DNS service by adding the following `addonFeatures` section after the `fabricSettings` section as shown in the following snippet:</span></span> 

    ```json
        "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "DnsService"
        ],
    ```

3. <span data-ttu-id="ce102-117">När du har uppdaterat mallen för kluster med föregående ändringarna kan använda dem och låta uppgraderingen slutförts.</span><span class="sxs-lookup"><span data-stu-id="ce102-117">Once you have updated your cluster template with the preceding changes, apply them and let the upgrade complete.</span></span> <span data-ttu-id="ce102-118">När installationen är klar startas tjänsten DNS-systemet körs i klustret som kallas `fabric:/System/DnsService` under system service-avsnittet i Service Fabric explorer.</span><span class="sxs-lookup"><span data-stu-id="ce102-118">Once complete, the DNS system service starts running in your cluster that is called `fabric:/System/DnsService` under system service section in the Service Fabric explorer.</span></span> 

<span data-ttu-id="ce102-119">Du kan också aktivera DNS-tjänsten via portalen när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="ce102-119">Alternatively, you can enable the DNS service through the portal at the time of cluster creation.</span></span> <span data-ttu-id="ce102-120">DNS-tjänsten kan aktiveras med en kryssruta för `Include DNS service` i den `Cluster configuration` menyn som visas i följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="ce102-120">The DNS service can be enabled by checking the box for `Include DNS service` in the `Cluster configuration` menu as shown in the following screenshot:</span></span>

![Aktivera DNS-tjänsten via portalen][2]


## <a name="setting-the-dns-name-for-your-service"></a><span data-ttu-id="ce102-122">Ange ett DNS-namn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="ce102-122">Setting the DNS name for your service</span></span>
<span data-ttu-id="ce102-123">När DNS-tjänsten körs i klustret, kan du ange ett DNS-namn för dina tjänster deklarativt för standardtjänster i antingen den `ApplicationManifest.xml` eller via Powershell-kommandon.</span><span class="sxs-lookup"><span data-stu-id="ce102-123">Once the DNS service is running in your cluster, you can set a DNS name for your services either declaratively for default services in the `ApplicationManifest.xml` or through Powershell commands.</span></span>

### <a name="setting-the-dns-name-for-a-default-service-in-the-applicationmanifestxml"></a><span data-ttu-id="ce102-124">Ange DNS-namnet för en standardtjänst i ApplicationManifest.xml</span><span class="sxs-lookup"><span data-stu-id="ce102-124">Setting the DNS name for a default service in the ApplicationManifest.xml</span></span>
<span data-ttu-id="ce102-125">Öppna projektet i Visual Studio eller ditt favoritprogram redigerare och öppna den `ApplicationManifest.xml` filen.</span><span class="sxs-lookup"><span data-stu-id="ce102-125">Open your project in Visual Studio, or your favorite editor, and open the `ApplicationManifest.xml` file.</span></span> <span data-ttu-id="ce102-126">Gå till avsnittet standard tjänster och för varje tjänst lägga till den `ServiceDnsName` attribut.</span><span class="sxs-lookup"><span data-stu-id="ce102-126">Go to the default services section, and for each service add the `ServiceDnsName` attribute.</span></span> <span data-ttu-id="ce102-127">I följande exempel visas hur du anger DNS-namnet på tjänsten`service1.application1`</span><span class="sxs-lookup"><span data-stu-id="ce102-127">The following example shows how to set the DNS name of the service to `service1.application1`</span></span>

```xml
    <Service Name="Stateless1" ServiceDnsName="service1.application1">
    <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
        <SingletonPartition />
    </StatelessService>
    </Service>
```
<span data-ttu-id="ce102-128">När programmet har distribuerats, service-instans i Service Fabric explorer visar DNS-namn för den här instansen, som visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="ce102-128">Once the application is deployed, the service instance in the Service Fabric explorer shows the DNS name for this instance, as shown in the following figure:</span></span> 

![slutpunkter][1]

### <a name="setting-the-dns-name-for-a-service-using-powershell"></a><span data-ttu-id="ce102-130">Ange DNS-namnet för en tjänst med Powershell</span><span class="sxs-lookup"><span data-stu-id="ce102-130">Setting the DNS name for a service using Powershell</span></span>
<span data-ttu-id="ce102-131">Du kan ange DNS-namnet för en tjänst när du skapar den med hjälp av den `New-ServiceFabricService` Powershell.</span><span class="sxs-lookup"><span data-stu-id="ce102-131">You can set the DNS name for a service when creating it using the `New-ServiceFabricService` Powershell.</span></span> <span data-ttu-id="ce102-132">I följande exempel skapas en ny tillståndslös tjänst med DNS-namn`service1.application1`</span><span class="sxs-lookup"><span data-stu-id="ce102-132">The following example creates a new stateless service with the DNS name `service1.application1`</span></span>

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

## <a name="using-dns-in-your-services"></a><span data-ttu-id="ce102-133">Med hjälp av DNS i dina tjänster</span><span class="sxs-lookup"><span data-stu-id="ce102-133">Using DNS in your services</span></span>
<span data-ttu-id="ce102-134">Om du distribuerar mer än en tjänst hittar du slutpunkter av andra tjänster kan kommunicera med genom att använda en DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="ce102-134">If you deploy more than one service, you can find the endpoints of other services to communicate with  by using a DNS name.</span></span> <span data-ttu-id="ce102-135">DNS-tjänsten kan bara användas för tillståndslösa tjänster, eftersom DNS-protokollet inte kan kommunicera med tillståndskänsliga tjänster.</span><span class="sxs-lookup"><span data-stu-id="ce102-135">The DNS service is only applicable to stateless services, since the DNS protocol cannot communicate with stateful services.</span></span> <span data-ttu-id="ce102-136">Du kan använda inbyggda omvänd proxy för http-anrop för tillståndskänsliga tjänster för att anropa en viss tjänst partition.</span><span class="sxs-lookup"><span data-stu-id="ce102-136">For stateful services, you can use the built-in reverse proxy for http calls to call a particular service partition.</span></span>

<span data-ttu-id="ce102-137">Följande kod visar hur du anropar en annan tjänst som ger helt enkelt en vanliga http-anropet du porten och en valfri sökväg som en del av URL: en.</span><span class="sxs-lookup"><span data-stu-id="ce102-137">The following code shows how to call another service, which is simply a regular http call where you provide the port and any optional path as part of the URL.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="ce102-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ce102-138">Next steps</span></span>
<span data-ttu-id="ce102-139">Mer information om kommunikation inom klustret med [ansluta och kommunicera med tjänster](service-fabric-connect-and-communicate-with-services.md)</span><span class="sxs-lookup"><span data-stu-id="ce102-139">Learn more about service communication within the cluster with  [connect and communicate with services](service-fabric-connect-and-communicate-with-services.md)</span></span>

[0]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[1]: ./media/service-fabric-dnsservice/servicefabric-explorer-dns.PNG
[2]: ./media/service-fabric-dnsservice/DNSService.PNG
