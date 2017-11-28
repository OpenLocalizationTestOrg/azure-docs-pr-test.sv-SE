---
title: "Konfigurera nätverk lägen för Azure Service Fabric-behållartjänster | Microsoft Docs"
description: "Lär dig hur du ställer in de olika lägena för nätverk som har stöd för Azure Service Fabric."
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
ms.openlocfilehash: f792f9604a5d6e62551ed92c1049d6e2b4216417
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="service-fabric-container-networking-modes"></a><span data-ttu-id="29432-103">Service Fabric-behållaren nätverk lägen</span><span class="sxs-lookup"><span data-stu-id="29432-103">Service Fabric container networking modes</span></span>

<span data-ttu-id="29432-104">Nätverk läge erbjuds i Service Fabric-klustret för behållartjänster som standard används den `nat` nätverk läge.</span><span class="sxs-lookup"><span data-stu-id="29432-104">The default networking mode offered in the Service Fabric cluster for container services is the `nat` networking mode.</span></span> <span data-ttu-id="29432-105">Med den `nat` nätverk läge, med mer än en behållare tjänsten lyssnar på samma port resultat i distributionsfel.</span><span class="sxs-lookup"><span data-stu-id="29432-105">With the `nat` networking mode, having more than one containers service listening to the same port results in deployment errors.</span></span> <span data-ttu-id="29432-106">För att lyssna på samma port med flera tjänster, Service Fabric stöder den `open` nätverk läge (version 5.7 eller högre).</span><span class="sxs-lookup"><span data-stu-id="29432-106">For running several services that listen on the same port, Service Fabric supports the `open` networking mode (version 5.7 or higher).</span></span> <span data-ttu-id="29432-107">Med den `open` nätverk läge varje behållartjänst hämtar en dynamiskt tilldelad IP-adress som internt så att flera tjänster att lyssna på samma port.</span><span class="sxs-lookup"><span data-stu-id="29432-107">With the `open` networking mode, each container service gets a dynamically assigned IP address internally allowing multiple services to listen to the same port.</span></span>   

<span data-ttu-id="29432-108">Därför med en enda typ med en statisk slutpunkt som definierats i service manifest nya tjänster kan skapas och tas bort utan distributionsfel med hjälp av den `open` nätverk läge.</span><span class="sxs-lookup"><span data-stu-id="29432-108">Thus, with a single service type with a static endpoint defined in the service manifest, new services may be created and deleted without deployment errors using the `open` networking mode.</span></span> <span data-ttu-id="29432-109">På liknande sätt kan använda samma `docker-compose.yml` fil med statisk portmappningar för att skapa flera tjänster.</span><span class="sxs-lookup"><span data-stu-id="29432-109">Similarly, one can use the same `docker-compose.yml` file with static port mappings for creating multiple services.</span></span>

<span data-ttu-id="29432-110">Med dynamiskt tilldelade IP-Adressen för att identifiera tjänster inte rekommenderas eftersom IP-adress ändras när tjänsten startas om eller flyttas till en annan nod.</span><span class="sxs-lookup"><span data-stu-id="29432-110">Using the dynamically assigned IP to discover services is not advisable since the IP address changes when the service restarts or moves to another node.</span></span> <span data-ttu-id="29432-111">Använd bara den **namngivningstjänst för Service Fabric** eller **DNS-tjänsten** för identifiering av tjänst.</span><span class="sxs-lookup"><span data-stu-id="29432-111">Only use the **Service Fabric Naming Service**  or the **DNS Service** for service discovery.</span></span> 


> [!WARNING]
> <span data-ttu-id="29432-112">Endast totalt 4096 IP-adresser tillåts per virtuellt nätverk i Azure.</span><span class="sxs-lookup"><span data-stu-id="29432-112">Only a total of 4096 IPs are allowed per vNET in Azure.</span></span> <span data-ttu-id="29432-113">Därför summan av antalet noder och antalet behållare tjänstinstanser (med `open` nätverk) får inte överstiga 4096 inom ett vNET.</span><span class="sxs-lookup"><span data-stu-id="29432-113">Thus, the sum of the number of nodes and the number of container service instances (with `open` networking) cannot exceed 4096 within a vNET.</span></span> <span data-ttu-id="29432-114">Dessa scenarier med hög densitet i `nat` nätverk läge rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="29432-114">For such high-density scenarios, the `nat` networking mode is recommended.</span></span>
>

## <a name="setting-up-open-networking-mode"></a><span data-ttu-id="29432-115">Konfigurera nätverk öppningsläge</span><span class="sxs-lookup"><span data-stu-id="29432-115">Setting up open networking mode</span></span>

1. <span data-ttu-id="29432-116">Konfigurera Azure Resource Manager-mallen genom att aktivera DNS-tjänsten och IP-providern under `fabricSettings`.</span><span class="sxs-lookup"><span data-stu-id="29432-116">Set up the Azure Resource Manager template by enabling DNS Service and the IP Provider under `fabricSettings`.</span></span> 

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

2. <span data-ttu-id="29432-117">Konfigurera avsnittet network profilen så att flera IP-adresser som ska konfigureras på varje nod i klustret.</span><span class="sxs-lookup"><span data-stu-id="29432-117">Set up the network profile section to allow multiple IP addresses to be configured on each node of the cluster.</span></span> <span data-ttu-id="29432-118">I följande exempel ställer in fem IP-adresser per nod (du kan därför ha fem instanser av tjänsten lyssnar på porten på varje nod) för ett Windows Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="29432-118">The following example sets up five IP addresses per node (thus you can have five service instances listening to the port on each node) for a Windows Service Fabric cluster.</span></span>

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

    <span data-ttu-id="29432-119">För Linux-kluster läggs en extra offentliga IP-konfiguration som tillåter utgående anslutningar.</span><span class="sxs-lookup"><span data-stu-id="29432-119">For Linux clusters, an additional public IP configuration is added to allow outbound connectivity.</span></span> <span data-ttu-id="29432-120">Följande kodavsnitt ställer in fem IP-adresser per nod för ett Linux-kluster:</span><span class="sxs-lookup"><span data-stu-id="29432-120">The following snippet sets up five IP addresses per node for a Linux cluster:</span></span>

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

3. <span data-ttu-id="29432-121">För Windows-kluster, ställa in en NSG regel öppna port UDP/53 för vNET med följande värden:</span><span class="sxs-lookup"><span data-stu-id="29432-121">For Windows clusters only, set up an NSG rule opening up port UDP/53 for the vNET with the following values:</span></span>

   | <span data-ttu-id="29432-122">Prioritet</span><span class="sxs-lookup"><span data-stu-id="29432-122">Priority</span></span> |    <span data-ttu-id="29432-123">Namn</span><span class="sxs-lookup"><span data-stu-id="29432-123">Name</span></span>    |    <span data-ttu-id="29432-124">Källa</span><span class="sxs-lookup"><span data-stu-id="29432-124">Source</span></span>      |  <span data-ttu-id="29432-125">Mål</span><span class="sxs-lookup"><span data-stu-id="29432-125">Destination</span></span>   |   <span data-ttu-id="29432-126">Tjänst</span><span class="sxs-lookup"><span data-stu-id="29432-126">Service</span></span>    | <span data-ttu-id="29432-127">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="29432-127">Action</span></span> |
   |:--------:|:----------:|:--------------:|:--------------:|:------------:|:------:|
   |     <span data-ttu-id="29432-128">2000</span><span class="sxs-lookup"><span data-stu-id="29432-128">2000</span></span> | <span data-ttu-id="29432-129">Custom_Dns</span><span class="sxs-lookup"><span data-stu-id="29432-129">Custom_Dns</span></span> | <span data-ttu-id="29432-130">VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="29432-130">VirtualNetwork</span></span> | <span data-ttu-id="29432-131">VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="29432-131">VirtualNetwork</span></span> | <span data-ttu-id="29432-132">DNS (UDP/53)</span><span class="sxs-lookup"><span data-stu-id="29432-132">DNS (UDP/53)</span></span> | <span data-ttu-id="29432-133">Tillåt</span><span class="sxs-lookup"><span data-stu-id="29432-133">Allow</span></span>  |


4. <span data-ttu-id="29432-134">Ange det nätverk läget i appmanifestet för varje tjänst `<NetworkConfig NetworkType="open">`.</span><span class="sxs-lookup"><span data-stu-id="29432-134">Specify the networking mode in the app manifest for each service `<NetworkConfig NetworkType="open">`.</span></span>  <span data-ttu-id="29432-135">Läget `open` resulterar i tjänsten hämtar en dedicerad IP-adress.</span><span class="sxs-lookup"><span data-stu-id="29432-135">The mode `open` results in the service getting a dedicated IP address.</span></span> <span data-ttu-id="29432-136">Om ett läge inte anges används som standard grundläggande `nat` läge.</span><span class="sxs-lookup"><span data-stu-id="29432-136">If a mode isn't specified, it defaults to the basic `nat` mode.</span></span> <span data-ttu-id="29432-137">Därför i manifestet exemplet `NodeContainerServicePackage1` och `NodeContainerServicePackage2` kan varje lyssna på samma port (båda tjänsterna lyssnar på `Endpoint1`).</span><span class="sxs-lookup"><span data-stu-id="29432-137">Thus, in the following manifest example, `NodeContainerServicePackage1` and `NodeContainerServicePackage2` can each listen to the same port (both services are listening on `Endpoint1`).</span></span>

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
<span data-ttu-id="29432-138">Du kan blanda och matcha olika lägen för nätverk för tjänster i ett program för Windows-kluster.</span><span class="sxs-lookup"><span data-stu-id="29432-138">You can mix and match different networking modes across services within an application for a Windows cluster.</span></span> <span data-ttu-id="29432-139">Du kan därför ha vissa tjänster på `open` läge och vissa på `nat` nätverk läge.</span><span class="sxs-lookup"><span data-stu-id="29432-139">Thus, you can have some services on `open` mode and some on `nat` networking mode.</span></span> <span data-ttu-id="29432-140">När en tjänst har konfigurerats med `nat`, den lyssnar på porten måste vara unika.</span><span class="sxs-lookup"><span data-stu-id="29432-140">When a service is configured with `nat`, the port it is listening to must be unique.</span></span> <span data-ttu-id="29432-141">Blanda nätverk lägen för olika tjänster stöds inte på Linux-kluster.</span><span class="sxs-lookup"><span data-stu-id="29432-141">Mixing networking modes for different services isn't supported on Linux clusters.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="29432-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="29432-142">Next steps</span></span>
<span data-ttu-id="29432-143">I den här artikeln har du lärt dig om networking lägen som erbjuds av Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="29432-143">In this article, you learned about networking modes offered by Service Fabric.</span></span>  

* [<span data-ttu-id="29432-144">Service Fabric programmodell</span><span class="sxs-lookup"><span data-stu-id="29432-144">Service Fabric application model</span></span>](service-fabric-application-model.md)
* [<span data-ttu-id="29432-145">Service Fabric service manifest resurser</span><span class="sxs-lookup"><span data-stu-id="29432-145">Service Fabric service manifest resources</span></span>](service-fabric-application-model.md)
* [<span data-ttu-id="29432-146">Distribuera en Windows-behållare till Service Fabric på Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="29432-146">Deploy a Windows container to Service Fabric on Windows Server 2016</span></span>](service-fabric-get-started-containers.md)
* [<span data-ttu-id="29432-147">Distribuera en dockerbehållare till Service Fabric på Linux</span><span class="sxs-lookup"><span data-stu-id="29432-147">Deploy a Docker container to Service Fabric on Linux</span></span>](service-fabric-get-started-containers-linux.md)
