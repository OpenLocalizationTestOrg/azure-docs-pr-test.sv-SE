---
title: "aaaAzure Apptjänstmiljö management adresser"
description: "Visar hello management adresser används toocommand en Apptjänst-miljö"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a7738a24-89ef-43d3-bff1-77f43d5a3952
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: b34b6266dc3a35915421b14bf34eddc07c2825c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="app-service-environment-management-addresses"></a><span data-ttu-id="9ac85-103">App-miljö management adresser</span><span class="sxs-lookup"><span data-stu-id="9ac85-103">App Service Environment management addresses</span></span>

<span data-ttu-id="9ac85-104">Hej Environment(ASE) för App Service är en distribution av hello Azure App Service till ett undernät i ditt virtuella Azure-nätverk (VNet).</span><span class="sxs-lookup"><span data-stu-id="9ac85-104">hello App Service Environment(ASE) is a deployment of hello Azure App Service into a subnet in your Azure Virtual Network (VNet).</span></span>  <span data-ttu-id="9ac85-105">Hej ASE måste vara tillgänglig från hello Azure App Service så att den kan hanteras.</span><span class="sxs-lookup"><span data-stu-id="9ac85-105">hello ASE must be accessible from hello Azure App Service so that it can be managed.</span></span>  <span data-ttu-id="9ac85-106">Den här ASE hantering av trafik passerar hello användarstyrd nätverk.</span><span class="sxs-lookup"><span data-stu-id="9ac85-106">This ASE management traffic traverses hello user controlled network.</span></span>  <span data-ttu-id="9ac85-107">Det kommer från Azure App Service management-servrar toohello offentliga VIP som är associerad med hello ASE.</span><span class="sxs-lookup"><span data-stu-id="9ac85-107">It comes from Azure App Service management servers toohello public VIP that is associated with hello ASE.</span></span>  <span data-ttu-id="9ac85-108">Mer information om hello ASE nätverk beroenden läsa [nätverk överväganden och hello Apptjänstmiljö][networking].</span><span class="sxs-lookup"><span data-stu-id="9ac85-108">For details on hello ASE networking dependencies read [Networking considerations and hello App Service Environment][networking].</span></span>  <span data-ttu-id="9ac85-109">Allmän information om hello ASE börjar du med [introduktion toohello Apptjänstmiljö][intro].</span><span class="sxs-lookup"><span data-stu-id="9ac85-109">For general information on hello ASE you can start with [Introduction toohello App Service Environment][intro].</span></span>

<span data-ttu-id="9ac85-110">Det här dokumentet innehåller hello källa IP-adresser för hantering av trafik toohello ASE.</span><span class="sxs-lookup"><span data-stu-id="9ac85-110">This document lists hello source IPs for management traffic toohello ASE.</span></span> <span data-ttu-id="9ac85-111">Du kan använda dessa adresser toocreate Nätverkssäkerhetsgrupper toolock ned inkommande trafik eller använda dem i vägtabeller efter behov.</span><span class="sxs-lookup"><span data-stu-id="9ac85-111">You can use these addresses toocreate Network Security Groups toolock down incoming traffic or use them in Route Tables as needed.</span></span>  <span data-ttu-id="9ac85-112">toouse denna information som du behöver toouse:</span><span class="sxs-lookup"><span data-stu-id="9ac85-112">toouse this information you need toouse:</span></span>

* <span data-ttu-id="9ac85-113">hello IP-adresser som anges för alla regioner</span><span class="sxs-lookup"><span data-stu-id="9ac85-113">hello IP addresses that are listed for All regions</span></span>
* <span data-ttu-id="9ac85-114">hello IP-adresser som matchar toohello region som din ASE distribueras till.</span><span class="sxs-lookup"><span data-stu-id="9ac85-114">hello IP addresses that match toohello region that your ASE is deployed into.</span></span>

<span data-ttu-id="9ac85-115">hello inkommande hantering av trafik som kommer in från dessa IP-adresser tooports 454 och 455.</span><span class="sxs-lookup"><span data-stu-id="9ac85-115">hello incoming management traffic comes in from these IP addresses tooports 454 and 455.</span></span>

| <span data-ttu-id="9ac85-116">Region</span><span class="sxs-lookup"><span data-stu-id="9ac85-116">Region</span></span> | <span data-ttu-id="9ac85-117">Adresser</span><span class="sxs-lookup"><span data-stu-id="9ac85-117">Addresses</span></span> |
|--------|-----------|
| <span data-ttu-id="9ac85-118">Alla regioner</span><span class="sxs-lookup"><span data-stu-id="9ac85-118">All regions</span></span> | <span data-ttu-id="9ac85-119">70.37.57.58, 157.55.208.185, 52.174.22.21,13.94.149.179,13.94.143.126,13.94.141.115, 52.178.195.197, 52.178.190.65, 52.178.184.149, 52.178.177.147, 13.75.127.117, 40.83.125.161, 40.83.121.56, 40.83.120.64, 52.187.56.50, 52.187.63.37, 52.187.59.251, 52.187.63.19, 52.165.158.140, 52.165.152.214, 52.165.154.193, 52.165.153.122, 104.44.129.255, 104.44.134.255, 104.44.129.243, 104.44.129.141</span><span class="sxs-lookup"><span data-stu-id="9ac85-119">70.37.57.58, 157.55.208.185, 52.174.22.21,13.94.149.179,13.94.143.126,13.94.141.115, 52.178.195.197, 52.178.190.65, 52.178.184.149, 52.178.177.147, 13.75.127.117, 40.83.125.161, 40.83.121.56, 40.83.120.64, 52.187.56.50, 52.187.63.37, 52.187.59.251, 52.187.63.19, 52.165.158.140, 52.165.152.214, 52.165.154.193, 52.165.153.122, 104.44.129.255, 104.44.134.255, 104.44.129.243, 104.44.129.141</span></span> |
| <span data-ttu-id="9ac85-120">Södra centrala USA & norra centrala USA</span><span class="sxs-lookup"><span data-stu-id="9ac85-120">South Central US & North Central US</span></span> | <span data-ttu-id="9ac85-121">23.102.188.65, 191.236.154.88</span><span class="sxs-lookup"><span data-stu-id="9ac85-121">23.102.188.65, 191.236.154.88</span></span> |
| <span data-ttu-id="9ac85-122">Australien, sydost & Östra Australien</span><span class="sxs-lookup"><span data-stu-id="9ac85-122">Australia Southeast & Australia East</span></span> | <span data-ttu-id="9ac85-123">23.101.234.41, 104.210.90.65</span><span class="sxs-lookup"><span data-stu-id="9ac85-123">23.101.234.41, 104.210.90.65</span></span> |
| <span data-ttu-id="9ac85-124">USA, Väst & USA, Öst</span><span class="sxs-lookup"><span data-stu-id="9ac85-124">US West & US East</span></span> | <span data-ttu-id="9ac85-125">104.45.227.37, 191.236.60.72</span><span class="sxs-lookup"><span data-stu-id="9ac85-125">104.45.227.37, 191.236.60.72</span></span> |
| <span data-ttu-id="9ac85-126">Västra Europa & Norra Europa</span><span class="sxs-lookup"><span data-stu-id="9ac85-126">West Europe & North Europe</span></span> | <span data-ttu-id="9ac85-127">191.233.94.45, 191.237.222.191</span><span class="sxs-lookup"><span data-stu-id="9ac85-127">191.233.94.45, 191.237.222.191</span></span> |
| <span data-ttu-id="9ac85-128">West centrala USA & västra USA 2</span><span class="sxs-lookup"><span data-stu-id="9ac85-128">West Central US & West US 2</span></span> | <span data-ttu-id="9ac85-129">13.78.148.75, 13.66.225.188</span><span class="sxs-lookup"><span data-stu-id="9ac85-129">13.78.148.75, 13.66.225.188</span></span> |
| <span data-ttu-id="9ac85-130">Centrala USA & östra USA 2</span><span class="sxs-lookup"><span data-stu-id="9ac85-130">Central US & East US 2</span></span> | <span data-ttu-id="9ac85-131">104.43.165.73, 104.46.108.135</span><span class="sxs-lookup"><span data-stu-id="9ac85-131">104.43.165.73, 104.46.108.135</span></span> |
| <span data-ttu-id="9ac85-132">Östasien & Sydostasien</span><span class="sxs-lookup"><span data-stu-id="9ac85-132">East Asia & Southeast Asia</span></span> | <span data-ttu-id="9ac85-133">23.99.115.5, 104.215.158.33</span><span class="sxs-lookup"><span data-stu-id="9ac85-133">23.99.115.5, 104.215.158.33</span></span> |
| <span data-ttu-id="9ac85-134">Östra & Japan, Väst</span><span class="sxs-lookup"><span data-stu-id="9ac85-134">Japan East & Japan West</span></span> | <span data-ttu-id="9ac85-135">104.41.185.116, 191.239.104.48</span><span class="sxs-lookup"><span data-stu-id="9ac85-135">104.41.185.116, 191.239.104.48</span></span> |
| <span data-ttu-id="9ac85-136">Kanada Central & Kanada, Öst</span><span class="sxs-lookup"><span data-stu-id="9ac85-136">Canada Central & Canada East</span></span> | <span data-ttu-id="9ac85-137">40.85.230.101, 40.86.229.100</span><span class="sxs-lookup"><span data-stu-id="9ac85-137">40.85.230.101, 40.86.229.100</span></span> |
| <span data-ttu-id="9ac85-138">Storbritannien, Väst & Storbritannien, Syd</span><span class="sxs-lookup"><span data-stu-id="9ac85-138">UK West & UK South</span></span> | <span data-ttu-id="9ac85-139">51.141.8.34, 51.140.185.75</span><span class="sxs-lookup"><span data-stu-id="9ac85-139">51.141.8.34, 51.140.185.75</span></span> |
| <span data-ttu-id="9ac85-140">Sydkorea söder & Korea Central</span><span class="sxs-lookup"><span data-stu-id="9ac85-140">Korea South & Korea Central</span></span> | <span data-ttu-id="9ac85-141">52.231.200.177, 52.231.32.117</span><span class="sxs-lookup"><span data-stu-id="9ac85-141">52.231.200.177, 52.231.32.117</span></span> |
| <span data-ttu-id="9ac85-142">Södra & södra centrala USA</span><span class="sxs-lookup"><span data-stu-id="9ac85-142">Brazil South & South Central US</span></span>| <span data-ttu-id="9ac85-143">104.41.46.178, 23.102.188.65</span><span class="sxs-lookup"><span data-stu-id="9ac85-143">104.41.46.178, 23.102.188.65</span></span> |
| <span data-ttu-id="9ac85-144">Centrala Indien & södra Indien</span><span class="sxs-lookup"><span data-stu-id="9ac85-144">Central India & South India</span></span> | <span data-ttu-id="9ac85-145">104.211.98.24, 104.211.225.66</span><span class="sxs-lookup"><span data-stu-id="9ac85-145">104.211.98.24, 104.211.225.66</span></span> |
| <span data-ttu-id="9ac85-146">Västra Indien & södra Indien</span><span class="sxs-lookup"><span data-stu-id="9ac85-146">West India & South India</span></span> | <span data-ttu-id="9ac85-147">104.211.160.229, 104.211.225.66</span><span class="sxs-lookup"><span data-stu-id="9ac85-147">104.211.160.229, 104.211.225.66</span></span> |


<!-- LINKS -->
[networking]: ./network-info.md
[intro]: ./intro.md
