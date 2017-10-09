---
title: "aaaConfigure nätverk lägen för Azure Service Fabric-behållartjänster | Microsoft Docs"
description: "Lär dig hur toosetup hello olika nätverk lägen som Azure Service Fabric stöder."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 5c5dd4c590c7698a947503cbe8ef66ff7d6b503a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-container-networking-modes"></a><span data-ttu-id="2dbdd-103">Service Fabric-behållaren nätverk lägen</span><span class="sxs-lookup"><span data-stu-id="2dbdd-103">Service Fabric container networking modes</span></span>

<span data-ttu-id="2dbdd-104">hello standardläget för nätverk som erbjuds i hello Service Fabric-klustret för behållartjänster är hello `nat` nätverk läge.</span><span class="sxs-lookup"><span data-stu-id="2dbdd-104">hello default networking mode offered in hello Service Fabric cluster for container services is hello `nat` networking mode.</span></span> <span data-ttu-id="2dbdd-105">Med hello `nat` nätverk läge, med mer än en behållare tjänsten lyssnar toohello samma port resulterar i distributionsfel.</span><span class="sxs-lookup"><span data-stu-id="2dbdd-105">With hello `nat` networking mode, having more than one containers service listening toohello same port results in deployment errors.</span></span> <span data-ttu-id="2dbdd-106">För att köra flera tjänster som lyssnar på samma port, Service Fabric stöder hello hello `open` nätverk läge (version 5.7 eller högre).</span><span class="sxs-lookup"><span data-stu-id="2dbdd-106">For running several services that listen on hello same port, Service Fabric supports hello `open` networking mode (version 5.7 or higher).</span></span> <span data-ttu-id="2dbdd-107">Med hello `open` nätverk läge varje behållartjänst hämtar en dynamiskt tilldelad IP-adress som internt så att flera tjänster toolisten toohello samma port.</span><span class="sxs-lookup"><span data-stu-id="2dbdd-107">With hello `open` networking mode, each container service gets a dynamically assigned IP address internally allowing multiple services toolisten toohello same port.</span></span>   

<span data-ttu-id="2dbdd-108">Därför med en enda typ med en statisk slutpunkt som definierats i hello tjänstmanifestet nya tjänster kan skapas och tas bort utan distributionsfel med hello `open` nätverk läge.</span><span class="sxs-lookup"><span data-stu-id="2dbdd-108">Thus, with a single service type with a static endpoint defined in hello service manifest, new services may be created and deleted without deployment errors using hello `open` networking mode.</span></span> <span data-ttu-id="2dbdd-109">På liknande sätt kan använda hello samma `docker-compose.yml` fil med statisk portmappningar för att skapa flera tjänster.</span><span class="sxs-lookup"><span data-stu-id="2dbdd-109">Similarly, one can use hello same `docker-compose.yml` file with static port mappings for creating multiple services.</span></span>

<span data-ttu-id="2dbdd-110">Med hjälp av hello dynamiskt tilldelade IP-toodiscover tjänster rekommenderas inte eftersom hello IP-adress ändras när hello-tjänsten startas om eller flyttas tooanother nod.</span><span class="sxs-lookup"><span data-stu-id="2dbdd-110">Using hello dynamically assigned IP toodiscover services is not advisable since hello IP address changes when hello service restarts or moves tooanother node.</span></span> <span data-ttu-id="2dbdd-111">Använd bara hello **namngivningstjänst för Service Fabric** eller hello **DNS-tjänsten** för identifiering av tjänst.</span><span class="sxs-lookup"><span data-stu-id="2dbdd-111">Only use hello **Service Fabric Naming Service**  or hello **DNS Service** for service discovery.</span></span> 


> [!WARNING]
> <span data-ttu-id="2dbdd-112">Endast totalt 4096 IP-adresser tillåts per virtuellt nätverk i Azure.</span><span class="sxs-lookup"><span data-stu-id="2dbdd-112">Only a total of 4096 IPs are allowed per vNET in Azure.</span></span> <span data-ttu-id="2dbdd-113">Därför hello summan av hello antalet noder och hello antal behållare tjänstinstanser (med `open` nätverk) får inte överstiga 4096 inom ett vNET.</span><span class="sxs-lookup"><span data-stu-id="2dbdd-113">Thus, hello sum of hello number of nodes and hello number of container service instances (with `open` networking) cannot exceed 4096 within a vNET.</span></span> <span data-ttu-id="2dbdd-114">Dessa scenarier med hög densitet hello `nat` nätverk läge rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="2dbdd-114">For such high-density scenarios, hello `nat` networking mode is recommended.</span></span>
>

## <a name="setting-up-open-networking-mode"></a><span data-ttu-id="2dbdd-115">Konfigurera nätverk öppningsläge</span><span class="sxs-lookup"><span data-stu-id="2dbdd-115">Setting up open networking mode</span></span>

1. <span data-ttu-id="2dbdd-116">Ställ in hello Azure Resource Manager-mall genom att aktivera DNS-tjänsten och hello IP-providern under `fabricSettings`.</span><span class="sxs-lookup"><span data-stu-id="2dbdd-116">Set up hello Azure Resource Manager template by enabling DNS Service and hello IP Provider under `fabricSettings`.</span></span> 

    ```json
    "fabricSettings": [
                {
                    "name": "DnsService",
                    "parameters": [
                       {
                            "name": "IsEnabled",
                            "value": "true"
                      }
                    ]
                },
                {
                    "name": "Hosting",
                    "parameters": [
                      { 
                            "name": "IPProviderEnabled",
                            "value": "true"
                      }
                    ]
                },
                {
                    "name":  "Trace/Etw", 
                    "parameters": [
                    {
                            "name": "Level",
                            "value": "5"
                    }
                    ]
                },
                {
                    "name": "Setup",
                    "parameters": [
                    {
                            "name": "ContainerNetworkSetup",
                            "value": "true"
                    }
                    ]
                }
            ],
    ```

2. <span data-ttu-id="2dbdd-117">Ställ in hello nätverket profil avsnittet tooallow flera IP-adresser toobe som konfigurerats på varje nod i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="2dbdd-117">Set up hello network profile section tooallow multiple IP addresses toobe configured on each node of hello cluster.</span></span> <span data-ttu-id="2dbdd-118">hello följande exempel ställer in fem IP-adresser per nod (du kan därför ha fem tjänstinstanser lyssningsport toohello på varje nod) för ett Windows Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="2dbdd-118">hello following example sets up five IP addresses per node (thus you can have five service instances listening toohello port on each node) for a Windows Service Fabric cluster.</span></span>

    ```json
    "variables": {
        "nicName": "NIC",
        "vmName": "vm",
        "virtualNetworkName": "VNet",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
        "vmNodeType0Name": "[toLower(concat('NT1', variables('vmName')))]",
        "subnet0Name": "Subnet-0",
        "subnet0Prefix": "10.0.0.0/24",
        "subnet0Ref": "[concat(variables('vnetID'),'/subnets/',variables('subnet0Name'))]",
        "lbID0": "[resourceId('Microsoft.Network/loadBalancers',concat('LB','-', parameters('clusterName'),'-',variables('vmNodeType0Name')))]",
        "lbIPConfig0": "[concat(variables('lbID0'),'/frontendIPConfigurations/LoadBalancerIPConfig')]",
        "lbPoolID0": "[concat(variables('lbID0'),'/backendAddressPools/LoadBalancerBEAddressPool')]",
        "lbProbeID0": "[concat(variables('lbID0'),'/probes/FabricGatewayProbe')]",
        "lbHttpProbeID0": "[concat(variables('lbID0'),'/probes/FabricHttpGatewayProbe')]",
        "lbNatPoolID0": "[concat(variables('lbID0'),'/inboundNatPools/LoadBalancerBEAddressNatPool')]"
    }
    "networkProfile": {
                "networkInterfaceConfigurations": [
                  {
                    "name": "[concat(parameters('nicName'), '-0')]",
                    "properties": {
                      "ipConfigurations": [
                        {
                          "name": "[concat(parameters('nicName'),'-',0)]",
                          "properties": {
                            "primary": "true",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "loadBalancerInboundNatPools": [
                              {
                                "id": "[variables('lbNatPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 1)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 2)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 3)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 4)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 5)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        }
                      ],
                      "primary": true
                    }
                  }
                ]
              }
    ```

    <span data-ttu-id="2dbdd-119">För Linux-kluster är en extra offentliga IP-konfiguration till tooallow utgående anslutning.</span><span class="sxs-lookup"><span data-stu-id="2dbdd-119">For Linux clusters, an additional public IP configuration is added tooallow outbound connectivity.</span></span> <span data-ttu-id="2dbdd-120">hello ställer följande kodavsnitt in fem IP-adresser per nod för ett Linux-kluster:</span><span class="sxs-lookup"><span data-stu-id="2dbdd-120">hello following snippet sets up five IP addresses per node for a Linux cluster:</span></span>

    ```json
    "networkProfile": {
                "networkInterfaceConfigurations": [
                  {
                    "name": "[concat(parameters('nicName'), '-0')]",
                    "properties": {
                      "ipConfigurations": [
                        {
                          "name": "[concat(parameters('nicName'),'-',0)]",
                          "properties": {
                            "primary": "true",
                            "publicipaddressconfiguration": {
                              "name": "devpub",
                              "properties": {
                                "idleTimeoutInMinutes": 15
                              }
                            },
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "loadBalancerInboundNatPools": [
                              {
                                "id": "[variables('lbNatPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 1)]",
                          "properties": {
                            "primary": "false",
                            "publicipaddressconfiguration": {
                              "name": "[concat('devpub', 1)]",
                              "properties": {
                                "idleTimeoutInMinutes": 15
                              }
                            },
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 2)]",
                          "properties": {
                            "primary": "false",
                            "publicipaddressconfiguration": {
                              "name": "[concat('devpub', 2)]",
                              "properties": {
                                "idleTimeoutInMinutes": 15
                              }
                            },
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 3)]",
                          "properties": {
                            "primary": "false",
                            "publicipaddressconfiguration": {
                              "name": "[concat('devpub', 3)]",
                              "properties": {
                                "idleTimeoutInMinutes": 15
                              }
                            },
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 4)]",
                          "properties": {
                            "primary": "false",
                            "publicipaddressconfiguration": {
                              "name": "[concat('devpub', 4)]",
                              "properties": {
                                "idleTimeoutInMinutes": 15
                              }
                            },
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 5)]",
                          "properties": {
                            "primary": "false",
                            "publicipaddressconfiguration": {
                              "name": "[concat('devpub', 5)]",
                              "properties": {
                                "idleTimeoutInMinutes": 15
                              }
                            },
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        }
                      ],
                      "primary": true
                    }
                  }
                ]
              }
    ```

3. <span data-ttu-id="2dbdd-121">Windows-kluster kan konfigurera en NSG regel i öppna port UDP/53 för hello vNET med hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="2dbdd-121">For Windows clusters only, set up an NSG rule opening up port UDP/53 for hello vNET with hello following values:</span></span>

   | <span data-ttu-id="2dbdd-122">Prioritet</span><span class="sxs-lookup"><span data-stu-id="2dbdd-122">Priority</span></span> |    <span data-ttu-id="2dbdd-123">Namn</span><span class="sxs-lookup"><span data-stu-id="2dbdd-123">Name</span></span>    |    <span data-ttu-id="2dbdd-124">Källa</span><span class="sxs-lookup"><span data-stu-id="2dbdd-124">Source</span></span>      |  <span data-ttu-id="2dbdd-125">Mål</span><span class="sxs-lookup"><span data-stu-id="2dbdd-125">Destination</span></span>   |   <span data-ttu-id="2dbdd-126">Tjänst</span><span class="sxs-lookup"><span data-stu-id="2dbdd-126">Service</span></span>    | <span data-ttu-id="2dbdd-127">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="2dbdd-127">Action</span></span> |
   |:--------:|:----------:|:--------------:|:--------------:|:------------:|:------:|
   |     <span data-ttu-id="2dbdd-128">2000</span><span class="sxs-lookup"><span data-stu-id="2dbdd-128">2000</span></span> | <span data-ttu-id="2dbdd-129">Custom_Dns</span><span class="sxs-lookup"><span data-stu-id="2dbdd-129">Custom_Dns</span></span> | <span data-ttu-id="2dbdd-130">VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="2dbdd-130">VirtualNetwork</span></span> | <span data-ttu-id="2dbdd-131">VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="2dbdd-131">VirtualNetwork</span></span> | <span data-ttu-id="2dbdd-132">DNS (UDP/53)</span><span class="sxs-lookup"><span data-stu-id="2dbdd-132">DNS (UDP/53)</span></span> | <span data-ttu-id="2dbdd-133">Tillåt</span><span class="sxs-lookup"><span data-stu-id="2dbdd-133">Allow</span></span>  |


4. <span data-ttu-id="2dbdd-134">Ange hello nätverk läge i hello appmanifestet för varje tjänst `<NetworkConfig NetworkType="open">`.</span><span class="sxs-lookup"><span data-stu-id="2dbdd-134">Specify hello networking mode in hello app manifest for each service `<NetworkConfig NetworkType="open">`.</span></span>  <span data-ttu-id="2dbdd-135">hello läge `open` resulterar i hello tjänst hämtar en dedicerad IP-adress.</span><span class="sxs-lookup"><span data-stu-id="2dbdd-135">hello mode `open` results in hello service getting a dedicated IP address.</span></span> <span data-ttu-id="2dbdd-136">Om ett läge inte anges används som standard toohello grundläggande `nat` läge.</span><span class="sxs-lookup"><span data-stu-id="2dbdd-136">If a mode isn't specified, it defaults toohello basic `nat` mode.</span></span> <span data-ttu-id="2dbdd-137">Därför i hello följande manifestet exempelvis `NodeContainerServicePackage1` och `NodeContainerServicePackage2` kan varje lyssna toohello samma port (båda tjänsterna lyssnar på `Endpoint1`).</span><span class="sxs-lookup"><span data-stu-id="2dbdd-137">Thus, in hello following manifest example, `NodeContainerServicePackage1` and `NodeContainerServicePackage2` can each listen toohello same port (both services are listening on `Endpoint1`).</span></span>

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ApplicationManifest ApplicationTypeName="NodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <Description>Calculator Application</Description>
      <Parameters>
        <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
        <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
      </Parameters>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeContainerServicePackage1" ServiceManifestVersion="1.0"/>
        <Policies>
          <ContainerHostPolicies CodePackageRef="NodeContainerService1.Code" Isolation="hyperv">
           <NetworkConfig NetworkType="open"/>
           <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeContainerServicePackage2" ServiceManifestVersion="1.0"/>
        <Policies>
          <ContainerHostPolicies CodePackageRef="NodeContainerService2.Code" Isolation="default">
            <NetworkConfig NetworkType="open"/>
            <PortBinding ContainerPort="8910" EndpointRef="Endpoint1"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
    </ApplicationManifest>
    ```
<span data-ttu-id="2dbdd-138">Du kan blanda och matcha olika lägen för nätverk för tjänster i ett program för Windows-kluster.</span><span class="sxs-lookup"><span data-stu-id="2dbdd-138">You can mix and match different networking modes across services within an application for a Windows cluster.</span></span> <span data-ttu-id="2dbdd-139">Du kan därför ha vissa tjänster på `open` läge och vissa på `nat` nätverk läge.</span><span class="sxs-lookup"><span data-stu-id="2dbdd-139">Thus, you can have some services on `open` mode and some on `nat` networking mode.</span></span> <span data-ttu-id="2dbdd-140">När en tjänst har konfigurerats med `nat`, hello port är det lyssnande toomust vara unika.</span><span class="sxs-lookup"><span data-stu-id="2dbdd-140">When a service is configured with `nat`, hello port it is listening toomust be unique.</span></span> <span data-ttu-id="2dbdd-141">Blanda nätverk lägen för olika tjänster stöds inte på Linux-kluster.</span><span class="sxs-lookup"><span data-stu-id="2dbdd-141">Mixing networking modes for different services isn't supported on Linux clusters.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="2dbdd-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2dbdd-142">Next steps</span></span>
<span data-ttu-id="2dbdd-143">I den här artikeln har du lärt dig om networking lägen som erbjuds av Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="2dbdd-143">In this article, you learned about networking modes offered by Service Fabric.</span></span>  

* [<span data-ttu-id="2dbdd-144">Service Fabric programmodell</span><span class="sxs-lookup"><span data-stu-id="2dbdd-144">Service Fabric application model</span></span>](service-fabric-application-model.md)
* [<span data-ttu-id="2dbdd-145">Service Fabric service manifest resurser</span><span class="sxs-lookup"><span data-stu-id="2dbdd-145">Service Fabric service manifest resources</span></span>](service-fabric-application-model.md)
* [<span data-ttu-id="2dbdd-146">Distribuera en Windows-behållaren tooService Fabric på Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="2dbdd-146">Deploy a Windows container tooService Fabric on Windows Server 2016</span></span>](service-fabric-get-started-containers.md)
* [<span data-ttu-id="2dbdd-147">Distribuera en Docker-behållare tooService Fabric på Linux</span><span class="sxs-lookup"><span data-stu-id="2dbdd-147">Deploy a Docker container tooService Fabric on Linux</span></span>](service-fabric-get-started-containers-linux.md)
