---
title: "aaaContainer lösning för övervakning i Azure Log Analytics | Microsoft Docs"
description: "hello behållaren övervakning lösning i logganalys hjälper dig se och hantera Docker- och Windows behållaren värdar i en enda plats."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: e1e4b52b-92d5-4bfa-8a09-ff8c6b5a9f78
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: magoedte;banders
ms.openlocfilehash: 2eed1dd81c22faef78a375fca3ebece9e5300c09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="container-monitoring-solution-in-log-analytics"></a><span data-ttu-id="ea2b0-103">Lösning för övervakning av behållare i logganalys</span><span class="sxs-lookup"><span data-stu-id="ea2b0-103">Container Monitoring solution in Log Analytics</span></span>

![Behållare symbol](./media/log-analytics-containers/containers-symbol.png)

<span data-ttu-id="ea2b0-105">Den här artikeln beskriver hur tooset in och använda hello behållaren övervakning lösning i logganalys, vilket gör det möjligt att visa och hantera Docker- och Windows behållaren värdar i en enda plats.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-105">This article describes how tooset up and use hello Container Monitoring solution in Log Analytics, which helps you view and manage your Docker and Windows container hosts in a single location.</span></span> <span data-ttu-id="ea2b0-106">Docker är ett system för virtualisering av programvara används toocreate behållare som automatiserar software distribution tootheir IT infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-106">Docker is a software virtualization system used toocreate containers that automate software deployment tootheir IT infrastructure.</span></span>

<span data-ttu-id="ea2b0-107">Hej lösning visar vilka behållare som körs, vilka behållaren image som de körs och där behållare körs.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-107">hello solution shows which containers are running, what container image they’re running, and where containers are running.</span></span> <span data-ttu-id="ea2b0-108">Du kan visa detaljerad granskningsinformation visar kommandon som används i behållare.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-108">You can view detailed audit information showing commands used with containers.</span></span> <span data-ttu-id="ea2b0-109">Och du kan felsöka behållare genom att visa och söka centraliserad loggar utan tooremotely vyn Docker eller Windows-värdar.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-109">And, you can troubleshoot containers by viewing and searching centralized logs without having tooremotely view Docker or Windows hosts.</span></span> <span data-ttu-id="ea2b0-110">Du kan hitta behållare som kan vara mycket brus och mycket överflödiga resurser på en värd.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-110">You can find containers that may be noisy and consuming excess resources on a host.</span></span> <span data-ttu-id="ea2b0-111">Och du kan visa centraliserad CPU, minne, lagring och nätverksinformation om användning och prestanda för behållare.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-111">And, you can view centralized CPU, memory, storage, and network usage and performance information for containers.</span></span> <span data-ttu-id="ea2b0-112">På datorer som kör Windows, kan du centralisera och jämför loggar från Windows Server Hyper-V och Docker-behållare.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-112">On computers running Windows, you can centralize and compare logs from Windows Server, Hyper-V, and Docker containers.</span></span> <span data-ttu-id="ea2b0-113">hello-lösningen stöder följande behållare orchestrators hello:</span><span class="sxs-lookup"><span data-stu-id="ea2b0-113">hello solution supports hello following container orchestrators:</span></span>

- <span data-ttu-id="ea2b0-114">Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="ea2b0-114">Docker Swarm</span></span>
- <span data-ttu-id="ea2b0-115">DC/OS</span><span class="sxs-lookup"><span data-stu-id="ea2b0-115">DC/OS</span></span>
- <span data-ttu-id="ea2b0-116">Kubernetes</span><span class="sxs-lookup"><span data-stu-id="ea2b0-116">Kubernetes</span></span>
- <span data-ttu-id="ea2b0-117">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ea2b0-117">Service Fabric</span></span>
- <span data-ttu-id="ea2b0-118">Red Hat OpenShift</span><span class="sxs-lookup"><span data-stu-id="ea2b0-118">Red Hat OpenShift</span></span>


<span data-ttu-id="ea2b0-119">hello visar följande diagram hello relationer mellan olika värdar för behållaren och agenter med OMS.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-119">hello following diagram shows hello relationships between various container hosts and agents with OMS.</span></span>

![Behållare diagram](./media/log-analytics-containers/containers-diagram.png)

## <a name="system-requirements"></a><span data-ttu-id="ea2b0-121">Systemkrav</span><span class="sxs-lookup"><span data-stu-id="ea2b0-121">System requirements</span></span>

<span data-ttu-id="ea2b0-122">Granska hello följande information tooverify hello förutsättningar vara uppfyllda innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-122">Before starting, review hello following details tooverify you meet hello prerequisites.</span></span>

### <a name="container-monitoring-solution-support-for-docker-orchestrator-and-os-platform"></a><span data-ttu-id="ea2b0-123">Behållaren övervakningslösning som stöd för Docker Orchestrator och OS-plattformen</span><span class="sxs-lookup"><span data-stu-id="ea2b0-123">Container monitoring solution support for Docker Orchestrator and OS platform</span></span>
<span data-ttu-id="ea2b0-124">hello hello följande tabell beskrivs Docker orchestration och operativsystem som stöd för behållaren inventering, prestanda och loggar med logganalys övervakning.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-124">hello following table outlines hello Docker orchestration and operating system monitoring support of container inventory, performance, and logs with Log Analytics.</span></span>   

| | <span data-ttu-id="ea2b0-125">ACS</span><span class="sxs-lookup"><span data-stu-id="ea2b0-125">ACS</span></span> | <span data-ttu-id="ea2b0-126">Linux</span><span class="sxs-lookup"><span data-stu-id="ea2b0-126">Linux</span></span> | <span data-ttu-id="ea2b0-127">Windows</span><span class="sxs-lookup"><span data-stu-id="ea2b0-127">Windows</span></span> | <span data-ttu-id="ea2b0-128">Behållare</span><span class="sxs-lookup"><span data-stu-id="ea2b0-128">Container</span></span><br><span data-ttu-id="ea2b0-129">Inventering</span><span class="sxs-lookup"><span data-stu-id="ea2b0-129">Inventory</span></span> | <span data-ttu-id="ea2b0-130">Bild</span><span class="sxs-lookup"><span data-stu-id="ea2b0-130">Image</span></span><br><span data-ttu-id="ea2b0-131">Inventering</span><span class="sxs-lookup"><span data-stu-id="ea2b0-131">Inventory</span></span> | <span data-ttu-id="ea2b0-132">Node</span><span class="sxs-lookup"><span data-stu-id="ea2b0-132">Node</span></span><br><span data-ttu-id="ea2b0-133">Inventering</span><span class="sxs-lookup"><span data-stu-id="ea2b0-133">Inventory</span></span> | <span data-ttu-id="ea2b0-134">Behållare</span><span class="sxs-lookup"><span data-stu-id="ea2b0-134">Container</span></span><br><span data-ttu-id="ea2b0-135">Prestanda</span><span class="sxs-lookup"><span data-stu-id="ea2b0-135">Performance</span></span> | <span data-ttu-id="ea2b0-136">Behållare</span><span class="sxs-lookup"><span data-stu-id="ea2b0-136">Container</span></span><br><span data-ttu-id="ea2b0-137">Händelse</span><span class="sxs-lookup"><span data-stu-id="ea2b0-137">Event</span></span> | <span data-ttu-id="ea2b0-138">Händelse</span><span class="sxs-lookup"><span data-stu-id="ea2b0-138">Event</span></span><br><span data-ttu-id="ea2b0-139">Logga</span><span class="sxs-lookup"><span data-stu-id="ea2b0-139">Log</span></span> | <span data-ttu-id="ea2b0-140">Behållare</span><span class="sxs-lookup"><span data-stu-id="ea2b0-140">Container</span></span><br><span data-ttu-id="ea2b0-141">Logga</span><span class="sxs-lookup"><span data-stu-id="ea2b0-141">Log</span></span> |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| <span data-ttu-id="ea2b0-142">Kubernetes</span><span class="sxs-lookup"><span data-stu-id="ea2b0-142">Kubernetes</span></span> | <span data-ttu-id="ea2b0-143">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-143">&#8226;</span></span> | <span data-ttu-id="ea2b0-144">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-144">&#8226;</span></span> | | <span data-ttu-id="ea2b0-145">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-145">&#8226;</span></span> | <span data-ttu-id="ea2b0-146">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-146">&#8226;</span></span> | <span data-ttu-id="ea2b0-147">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-147">&#8226;</span></span> | <span data-ttu-id="ea2b0-148">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-148">&#8226;</span></span> | <span data-ttu-id="ea2b0-149">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-149">&#8226;</span></span> | <span data-ttu-id="ea2b0-150">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-150">&#8226;</span></span> | <span data-ttu-id="ea2b0-151">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-151">&#8226;</span></span> |
| <span data-ttu-id="ea2b0-152">Mesosphere</span><span class="sxs-lookup"><span data-stu-id="ea2b0-152">Mesosphere</span></span><br><span data-ttu-id="ea2b0-153">DC/OS</span><span class="sxs-lookup"><span data-stu-id="ea2b0-153">DC/OS</span></span> | <span data-ttu-id="ea2b0-154">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-154">&#8226;</span></span> | <span data-ttu-id="ea2b0-155">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-155">&#8226;</span></span> | | <span data-ttu-id="ea2b0-156">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-156">&#8226;</span></span> | <span data-ttu-id="ea2b0-157">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-157">&#8226;</span></span> | <span data-ttu-id="ea2b0-158">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-158">&#8226;</span></span> | <span data-ttu-id="ea2b0-159">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-159">&#8226;</span></span>| <span data-ttu-id="ea2b0-160">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-160">&#8226;</span></span> | <span data-ttu-id="ea2b0-161">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-161">&#8226;</span></span> | <span data-ttu-id="ea2b0-162">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-162">&#8226;</span></span> |
| <span data-ttu-id="ea2b0-163">Docker</span><span class="sxs-lookup"><span data-stu-id="ea2b0-163">Docker</span></span><br><span data-ttu-id="ea2b0-164">Swarm</span><span class="sxs-lookup"><span data-stu-id="ea2b0-164">Swarm</span></span> | <span data-ttu-id="ea2b0-165">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-165">&#8226;</span></span> | <span data-ttu-id="ea2b0-166">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-166">&#8226;</span></span> | <span data-ttu-id="ea2b0-167">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-167">&#8226;</span></span> | <span data-ttu-id="ea2b0-168">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-168">&#8226;</span></span> | <span data-ttu-id="ea2b0-169">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-169">&#8226;</span></span> | <span data-ttu-id="ea2b0-170">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-170">&#8226;</span></span> | <span data-ttu-id="ea2b0-171">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-171">&#8226;</span></span> | <span data-ttu-id="ea2b0-172">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-172">&#8226;</span></span> | | <span data-ttu-id="ea2b0-173">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-173">&#8226;</span></span> |
| <span data-ttu-id="ea2b0-174">Tjänst</span><span class="sxs-lookup"><span data-stu-id="ea2b0-174">Service</span></span><br><span data-ttu-id="ea2b0-175">Fabric</span><span class="sxs-lookup"><span data-stu-id="ea2b0-175">Fabric</span></span> | | | <span data-ttu-id="ea2b0-176">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-176">&#8226;</span></span> | <span data-ttu-id="ea2b0-177">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-177">&#8226;</span></span> | <span data-ttu-id="ea2b0-178">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-178">&#8226;</span></span> | <span data-ttu-id="ea2b0-179">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-179">&#8226;</span></span> | <span data-ttu-id="ea2b0-180">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-180">&#8226;</span></span> | <span data-ttu-id="ea2b0-181">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-181">&#8226;</span></span> | <span data-ttu-id="ea2b0-182">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-182">&#8226;</span></span> | <span data-ttu-id="ea2b0-183">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-183">&#8226;</span></span> |
| <span data-ttu-id="ea2b0-184">Red Hat öppna</span><span class="sxs-lookup"><span data-stu-id="ea2b0-184">Red Hat Open</span></span><br><span data-ttu-id="ea2b0-185">SKIFT</span><span class="sxs-lookup"><span data-stu-id="ea2b0-185">Shift</span></span> | | <span data-ttu-id="ea2b0-186">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-186">&#8226;</span></span> | | <span data-ttu-id="ea2b0-187">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-187">&#8226;</span></span> | <span data-ttu-id="ea2b0-188">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-188">&#8226;</span></span>| <span data-ttu-id="ea2b0-189">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-189">&#8226;</span></span> | <span data-ttu-id="ea2b0-190">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-190">&#8226;</span></span> | <span data-ttu-id="ea2b0-191">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-191">&#8226;</span></span> | | <span data-ttu-id="ea2b0-192">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-192">&#8226;</span></span> |
| <span data-ttu-id="ea2b0-193">Windows Server</span><span class="sxs-lookup"><span data-stu-id="ea2b0-193">Windows Server</span></span><br><span data-ttu-id="ea2b0-194">(fristående)</span><span class="sxs-lookup"><span data-stu-id="ea2b0-194">(standalone)</span></span> | | | <span data-ttu-id="ea2b0-195">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-195">&#8226;</span></span> | <span data-ttu-id="ea2b0-196">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-196">&#8226;</span></span> | <span data-ttu-id="ea2b0-197">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-197">&#8226;</span></span> | <span data-ttu-id="ea2b0-198">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-198">&#8226;</span></span> | <span data-ttu-id="ea2b0-199">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-199">&#8226;</span></span> | <span data-ttu-id="ea2b0-200">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-200">&#8226;</span></span> | | <span data-ttu-id="ea2b0-201">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-201">&#8226;</span></span> |
| <span data-ttu-id="ea2b0-202">Linux-server</span><span class="sxs-lookup"><span data-stu-id="ea2b0-202">Linux Server</span></span><br><span data-ttu-id="ea2b0-203">(fristående)</span><span class="sxs-lookup"><span data-stu-id="ea2b0-203">(standalone)</span></span> | | <span data-ttu-id="ea2b0-204">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-204">&#8226;</span></span> | | <span data-ttu-id="ea2b0-205">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-205">&#8226;</span></span> | <span data-ttu-id="ea2b0-206">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-206">&#8226;</span></span> | <span data-ttu-id="ea2b0-207">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-207">&#8226;</span></span> | <span data-ttu-id="ea2b0-208">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-208">&#8226;</span></span> | <span data-ttu-id="ea2b0-209">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-209">&#8226;</span></span> | | <span data-ttu-id="ea2b0-210">&#8226;</span><span class="sxs-lookup"><span data-stu-id="ea2b0-210">&#8226;</span></span> |


### <a name="docker-versions-supported-on-linux"></a><span data-ttu-id="ea2b0-211">Docker-versioner som stöds på Linux</span><span class="sxs-lookup"><span data-stu-id="ea2b0-211">Docker versions supported on Linux</span></span>

- <span data-ttu-id="ea2b0-212">Docker 1.11 too1.13</span><span class="sxs-lookup"><span data-stu-id="ea2b0-212">Docker 1.11 too1.13</span></span>
- <span data-ttu-id="ea2b0-213">Docker CE och EE v17.06</span><span class="sxs-lookup"><span data-stu-id="ea2b0-213">Docker CE and EE v17.06</span></span>

### <a name="x64-linux-distributions-supported-as-container-hosts"></a><span data-ttu-id="ea2b0-214">x64 Linux-distributioner som stöds som behållare värdar</span><span class="sxs-lookup"><span data-stu-id="ea2b0-214">x64 Linux distributions supported as container hosts</span></span>

- <span data-ttu-id="ea2b0-215">Ubuntu 14.04 LTS och 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="ea2b0-215">Ubuntu 14.04 LTS and 16.04 LTS</span></span>
- <span data-ttu-id="ea2b0-216">CoreOS(stable)</span><span class="sxs-lookup"><span data-stu-id="ea2b0-216">CoreOS(stable)</span></span>
- <span data-ttu-id="ea2b0-217">Amazon Linux 2016.09.0</span><span class="sxs-lookup"><span data-stu-id="ea2b0-217">Amazon Linux 2016.09.0</span></span>
- <span data-ttu-id="ea2b0-218">openSUSE 13.2</span><span class="sxs-lookup"><span data-stu-id="ea2b0-218">openSUSE 13.2</span></span>
- <span data-ttu-id="ea2b0-219">openSUSE LEAP 42.2</span><span class="sxs-lookup"><span data-stu-id="ea2b0-219">openSUSE LEAP 42.2</span></span>
- <span data-ttu-id="ea2b0-220">CentOS 7.2 och 7.3</span><span class="sxs-lookup"><span data-stu-id="ea2b0-220">CentOS 7.2 and 7.3</span></span>
- <span data-ttu-id="ea2b0-221">SLES 12</span><span class="sxs-lookup"><span data-stu-id="ea2b0-221">SLES 12</span></span>
- <span data-ttu-id="ea2b0-222">RHEL 7.2 och 7.3</span><span class="sxs-lookup"><span data-stu-id="ea2b0-222">RHEL 7.2 and 7.3</span></span>
- <span data-ttu-id="ea2b0-223">Red Hat OpenShift behållare plattform (OCP) 3.4 och 3.5</span><span class="sxs-lookup"><span data-stu-id="ea2b0-223">Red Hat OpenShift Container Platform (OCP) 3.4 and 3.5</span></span>
- <span data-ttu-id="ea2b0-224">ACS Mesosphere DC/OS 1.7.3 too1.8.8</span><span class="sxs-lookup"><span data-stu-id="ea2b0-224">ACS Mesosphere DC/OS 1.7.3 too1.8.8</span></span>
- <span data-ttu-id="ea2b0-225">ACS-Kubernetes 1.4.5 too1.6</span><span class="sxs-lookup"><span data-stu-id="ea2b0-225">ACS Kubernetes 1.4.5 too1.6</span></span>
- <span data-ttu-id="ea2b0-226">ACS-Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="ea2b0-226">ACS Docker Swarm</span></span>

### <a name="supported-windows-operating-system"></a><span data-ttu-id="ea2b0-227">Windows-operativsystem som stöds</span><span class="sxs-lookup"><span data-stu-id="ea2b0-227">Supported Windows operating system</span></span>

- <span data-ttu-id="ea2b0-228">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="ea2b0-228">Windows Server 2016</span></span>
- <span data-ttu-id="ea2b0-229">Windows 10 årsdagar Edition (Professional eller Enterprise)</span><span class="sxs-lookup"><span data-stu-id="ea2b0-229">Windows 10 Anniversary Edition (Professional or Enterprise)</span></span>

### <a name="docker-versions-supported-on-windows"></a><span data-ttu-id="ea2b0-230">Docker-versioner som stöds i Windows</span><span class="sxs-lookup"><span data-stu-id="ea2b0-230">Docker versions supported on Windows</span></span>

- <span data-ttu-id="ea2b0-231">Docker 1.12 och 1.13</span><span class="sxs-lookup"><span data-stu-id="ea2b0-231">Docker 1.12 and 1.13</span></span>
- <span data-ttu-id="ea2b0-232">Docker 17.03.0 och senare</span><span class="sxs-lookup"><span data-stu-id="ea2b0-232">Docker 17.03.0 and later</span></span>

## <a name="installing-and-configuring-hello-solution"></a><span data-ttu-id="ea2b0-233">Installera och konfigurera hello lösning</span><span class="sxs-lookup"><span data-stu-id="ea2b0-233">Installing and configuring hello solution</span></span>
<span data-ttu-id="ea2b0-234">Använd följande information tooinstall hello och konfigurera hello lösning.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-234">Use hello following information tooinstall and configure hello solution.</span></span>

1. <span data-ttu-id="ea2b0-235">Lägg till hello behållaren övervakning lösning tooyour OMS-arbetsyta från [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) eller genom att använda hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="ea2b0-235">Add hello Container Monitoring solution tooyour OMS workspace from [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) or by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>

2. <span data-ttu-id="ea2b0-236">Installera och använda Docker med en OMS-agent.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-236">Install and use Docker with an OMS agent.</span></span>  <span data-ttu-id="ea2b0-237">Baserat på ditt operativsystem kan välja du mellan hello följande metoder:</span><span class="sxs-lookup"><span data-stu-id="ea2b0-237">Based on your operating system, you can choose from hello following methods:</span></span>

  * <span data-ttu-id="ea2b0-238">Installera på Linux operativsystem som stöds och kör Docker och sedan installera och konfigurera hello [OMS-Agent för Linux](log-analytics-agent-linux.md).</span><span class="sxs-lookup"><span data-stu-id="ea2b0-238">On supported Linux operating systems, install and run Docker and then install and configure hello [OMS Agent for Linux](log-analytics-agent-linux.md).</span></span>  
  * <span data-ttu-id="ea2b0-239">Virtuell CoreOS, det går inte att du kör hello OMS-Agent för Linux.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-239">On CoreOS, you cannot run hello OMS Agent for Linux.</span></span> <span data-ttu-id="ea2b0-240">I stället kan du köra en av version av hello OMS-Agent för Linux.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-240">Instead, you run a containerized version of hello OMS Agent for Linux.</span></span> <span data-ttu-id="ea2b0-241">Granska [Linux behållaren värdar inklusive virtuell CoreOS](#for-all-linux-container-hosts-including-coreos) eller [Azure Government Linux behållaren värdar inklusive virtuell CoreOS](#for-all-azure-government-linux-container-hosts-including-coreos) om du arbetar med behållare i Azure Government-molnet.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-241">Review [Linux container hosts including CoreOS](#for-all-linux-container-hosts-including-coreos) or [Azure Government Linux container hosts including CoreOS](#for-all-azure-government-linux-container-hosts-including-coreos) if you are working with containers in Azure Government Cloud.</span></span>
  * <span data-ttu-id="ea2b0-242">Installera hello Docker-motorn och klienten på Windows Server 2016 och Windows 10, och sedan ansluta agenten toogather information och skicka det tooLog Analytics.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-242">On Windows Server 2016 and Windows 10, install hello Docker Engine and client then connect an agent toogather information and send it tooLog Analytics.</span></span>  

### <a name="container-services"></a><span data-ttu-id="ea2b0-243">Behållartjänster</span><span class="sxs-lookup"><span data-stu-id="ea2b0-243">Container services</span></span>

- <span data-ttu-id="ea2b0-244">Om du har en miljö med Red Hat OpenShift granska [konfigurera en OMS-agent för Red Hat OpenShift](#configure-an-oms-agent-for-red-hat-openshift).</span><span class="sxs-lookup"><span data-stu-id="ea2b0-244">If you have a Red Hat OpenShift environment, review [Configure an OMS agent for Red Hat OpenShift](#configure-an-oms-agent-for-red-hat-openshift).</span></span>
- <span data-ttu-id="ea2b0-245">Om du har ett Kubernetes-kluster med hello Azure Container Service granska [övervaka ett Azure Container Service-kluster med Microsoft Operations Management Suite (OMS)](../container-service/kubernetes/container-service-kubernetes-oms.md).</span><span class="sxs-lookup"><span data-stu-id="ea2b0-245">If you have a Kubernetes cluster using hello Azure Container Service, review [Monitor an Azure Container Service cluster with Microsoft Operations Management Suite (OMS)](../container-service/kubernetes/container-service-kubernetes-oms.md).</span></span>
- <span data-ttu-id="ea2b0-246">Om du har ett Azure Container Service DC/OS-kluster, mer information finns i [övervaka ett Azure Container Service DC/OS-kluster med Operations Management Suite](../container-service/dcos-swarm/container-service-monitoring-oms.md).</span><span class="sxs-lookup"><span data-stu-id="ea2b0-246">If you have an Azure Container Service DC/OS cluster, learn more at [Monitor an Azure Container Service DC/OS cluster with Operations Management Suite](../container-service/dcos-swarm/container-service-monitoring-oms.md).</span></span>
- <span data-ttu-id="ea2b0-247">Om du har en miljö med Docker Swarm läge, mer information finns i [konfigurera en OMS-agent för Docker Swarm](#configure-an-oms-agent-for-docker-swarm).</span><span class="sxs-lookup"><span data-stu-id="ea2b0-247">If you have a Docker Swarm mode environment, learn more at [Configure an OMS agent for Docker Swarm](#configure-an-oms-agent-for-docker-swarm).</span></span>
- <span data-ttu-id="ea2b0-248">Om du använder behållare med Service Fabric mer information finns i [översikt av Azure Service Fabric](../service-fabric/service-fabric-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ea2b0-248">If you use containers with Service Fabric, learn more at [Overview of Azure Service Fabric](../service-fabric/service-fabric-overview.md).</span></span>
- <span data-ttu-id="ea2b0-249">Granska hello [Docker-motorn på Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon) artikel för ytterligare information om hur tooinstall och konfigurera Docker-motorer på Windows-datorer.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-249">Review hello [Docker Engine on Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon) article for additional information about how tooinstall and configure your Docker Engines on computers running Windows.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ea2b0-250">Docker måste köras **innan** du installerar hello [OMS-Agent för Linux](log-analytics-agent-linux.md) på behållaren-värdar.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-250">Docker must be running **before** you install hello [OMS Agent for Linux](log-analytics-agent-linux.md) on your container hosts.</span></span> <span data-ttu-id="ea2b0-251">Om du redan har installerat hello agenten innan du installerar Docker måste tooreinstall hello OMS-Agent för Linux.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-251">If you've already installed hello agent before installing Docker, you need tooreinstall hello OMS Agent for Linux.</span></span> <span data-ttu-id="ea2b0-252">Mer information om Docker finns hello [Docker webbplats](https://www.docker.com).</span><span class="sxs-lookup"><span data-stu-id="ea2b0-252">For more information about Docker, see hello [Docker website](https://www.docker.com).</span></span>


## <a name="linux-container-hosts"></a><span data-ttu-id="ea2b0-253">Linux-behållaren värdar</span><span class="sxs-lookup"><span data-stu-id="ea2b0-253">Linux container hosts</span></span>

<span data-ttu-id="ea2b0-254">När du har installerat Docker använder du följande inställningar för din behållaren tooconfigure hello värdagenten för användning med Docker hello.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-254">After you've installed Docker, use hello following settings for your container host tooconfigure hello agent for use with Docker.</span></span> <span data-ttu-id="ea2b0-255">Du måste först din OMS arbetsyte-ID och nyckel som du hittar i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-255">First you need your OMS workspace ID and key, which you can find in hello Azure portal.</span></span> <span data-ttu-id="ea2b0-256">I arbetsytan, klickar du på **Snabbstart** > **datorer** tooview din **arbetsyte-ID** och **primärnyckel**.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-256">In your workspace, click **Quick Start** > **Computers** tooview your **Workspace ID** and **Primary Key**.</span></span>  <span data-ttu-id="ea2b0-257">Kopiera och klistra in både i din favorit-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-257">Copy and paste both into your favorite editor.</span></span>

### <a name="for-all-linux-container-hosts-except-coreos"></a><span data-ttu-id="ea2b0-258">För alla Linux-behållaren värdar utom virtuell CoreOS</span><span class="sxs-lookup"><span data-stu-id="ea2b0-258">For all Linux container hosts except CoreOS</span></span>

- <span data-ttu-id="ea2b0-259">Mer information och anvisningar om hur tooinstall hello OMS-Agent för Linux finns [ansluta din Linux-datorer tooOperations Management Suite (OMS)](log-analytics-agent-linux.md).</span><span class="sxs-lookup"><span data-stu-id="ea2b0-259">For more information and steps on how tooinstall hello OMS Agent for Linux, see [Connect your Linux Computers tooOperations Management Suite (OMS)](log-analytics-agent-linux.md).</span></span>

### <a name="for-all-linux-container-hosts-including-coreos"></a><span data-ttu-id="ea2b0-260">För alla värdar för Linux behållare inklusive virtuell CoreOS</span><span class="sxs-lookup"><span data-stu-id="ea2b0-260">For all Linux container hosts including CoreOS</span></span>

<span data-ttu-id="ea2b0-261">Starta hello OMS-behållare som du vill toomonitor.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-261">Start hello OMS container that you want toomonitor.</span></span> <span data-ttu-id="ea2b0-262">Ändra och använda hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="ea2b0-262">Modify and use hello following example:</span></span>

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25225:25225 --name="omsagent" --restart=always microsoft/oms
```

### <a name="for-all-azure-government-linux-container-hosts-including-coreos"></a><span data-ttu-id="ea2b0-263">För alla värdar för Azure Government Linux behållare inklusive virtuell CoreOS</span><span class="sxs-lookup"><span data-stu-id="ea2b0-263">For all Azure Government Linux container hosts including CoreOS</span></span>

<span data-ttu-id="ea2b0-264">Starta hello OMS-behållare som du vill toomonitor.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-264">Start hello OMS container that you want toomonitor.</span></span> <span data-ttu-id="ea2b0-265">Ändra och använda hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="ea2b0-265">Modify and use hello following example:</span></span>

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v /var/log:/var/log -e WSID="your workspace id" -e KEY="your key" -e DOMAIN="opinsights.azure.us" -p 127.0.0.1:25225:25225 -p 127.0.0.1:25224:25224/udp --name="omsagent" -h=`hostname` --restart=always microsoft/oms
```

### <a name="switching-from-using-an-installed-linux-agent-tooone-in-a-container"></a><span data-ttu-id="ea2b0-266">Växlar från att använda en installerade Linux-agenten tooone i en behållare</span><span class="sxs-lookup"><span data-stu-id="ea2b0-266">Switching from using an installed Linux agent tooone in a container</span></span>
<span data-ttu-id="ea2b0-267">Om du tidigare använt hello direkt installerad agent och vill ta tooinstead används en agent som körs i en behållare, måste du först bort hello OMS-Agent för Linux.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-267">If you previously used hello directly-installed agent and want tooinstead use an agent running in a container, you must first remove hello OMS Agent for Linux.</span></span> <span data-ttu-id="ea2b0-268">Se [avinstallerar hello OMS-Agent för Linux](log-analytics-agent-linux.md#uninstalling-the-oms-agent-for-linux) toounderstand hur toosuccessfully avinstallera hello agent.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-268">See [Uninstalling hello OMS Agent for Linux](log-analytics-agent-linux.md#uninstalling-the-oms-agent-for-linux) toounderstand how toosuccessfully uninstall hello agent.</span></span>  

### <a name="configure-an-oms-agent-for-docker-swarm"></a><span data-ttu-id="ea2b0-269">Konfigurera en OMS-agent för Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="ea2b0-269">Configure an OMS agent for Docker Swarm</span></span>

<span data-ttu-id="ea2b0-270">Du kan köra hello OMS-Agent som en global tjänst på Docker Swarm.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-270">You can run hello OMS Agent as a global service on Docker Swarm.</span></span> <span data-ttu-id="ea2b0-271">Använd följande information toocreate OMS-agenttjänsten hello.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-271">Use hello following information toocreate an OMS Agent service.</span></span> <span data-ttu-id="ea2b0-272">Du behöver tooinsert din OMS arbetsyte-ID och primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-272">You need tooinsert your OMS Workspace ID and Primary Key.</span></span>

- <span data-ttu-id="ea2b0-273">Kör följande hello på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-273">Run hello following on hello master node.</span></span>

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock  -e WSID="<WORKSPACE ID>" -e KEY="<PRIMARY KEY>" -p 25225:25225 -p 25224:25224/udp  --restart-condition=on-failure microsoft/oms
    ```

### <a name="configure-an-oms-agent-for-red-hat-openshift"></a><span data-ttu-id="ea2b0-274">Konfigurera en OMS-Agent för Red Hat OpenShift</span><span class="sxs-lookup"><span data-stu-id="ea2b0-274">Configure an OMS Agent for Red Hat OpenShift</span></span>
<span data-ttu-id="ea2b0-275">Det finns tre sätt tooadd hello OMS-Agent tooRed Hat OpenShift toostart att samla in behållaren övervakningsdata.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-275">There are three ways tooadd hello OMS Agent tooRed Hat OpenShift toostart collecting container monitoring data.</span></span>

* <span data-ttu-id="ea2b0-276">[Installera hello OMS-Agent för Linux](log-analytics-agent-linux.md) direkt på varje nod i OpenShift</span><span class="sxs-lookup"><span data-stu-id="ea2b0-276">[Install hello OMS Agent for Linux](log-analytics-agent-linux.md) directly on each OpenShift node</span></span>  
* <span data-ttu-id="ea2b0-277">[Aktivera Log Analytics VM-tillägget](log-analytics-azure-vm-extension.md) på varje nod för OpenShift som finns i Azure</span><span class="sxs-lookup"><span data-stu-id="ea2b0-277">[Enable Log Analytics VM Extension](log-analytics-azure-vm-extension.md) on each OpenShift node residing in Azure</span></span>  
* <span data-ttu-id="ea2b0-278">Installera hello OMS-Agent som en OpenShift daemon-uppsättning</span><span class="sxs-lookup"><span data-stu-id="ea2b0-278">Install hello OMS Agent as a OpenShift daemon-set</span></span>  

<span data-ttu-id="ea2b0-279">I det här avsnittet beskriver vi hello steg krävs tooinstall hello OMS-Agent som en OpenShift daemon-uppsättning.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-279">In this section we cover hello steps required tooinstall hello OMS Agent as an OpenShift daemon-set.</span></span>  

1. <span data-ttu-id="ea2b0-280">Logga in toohello OpenShift nod och kopiera hello yaml huvudfilen [ocp-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-omsagent.yaml) från GitHub tooyour nod och ändra hello värdet med din OMS arbetsyte-ID och primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-280">Sign on toohello OpenShift master node and copy hello yaml file [ocp-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-omsagent.yaml) from GitHub tooyour master node and modify hello value with your OMS Workspace ID and with your Primary Key.</span></span>
2. <span data-ttu-id="ea2b0-281">Kör följande kommandon toocreate hello ett projekt för OMS och ange hello användarkonto.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-281">Run hello following commands toocreate a project for OMS and set hello user account.</span></span>

    ```
    oadm new-project omslogging --node-selector='zone=default'
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. <span data-ttu-id="ea2b0-282">toodeploy hello daemon-mängd kör hello följande:</span><span class="sxs-lookup"><span data-stu-id="ea2b0-282">toodeploy hello daemon-set, run hello following:</span></span>

    `oc create -f ocp-omsagent.yaml`

5. <span data-ttu-id="ea2b0-283">tooverify som den är konfigurerad och fungerar korrekt, skriver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="ea2b0-283">tooverify it is configured and working correctly, type hello following:</span></span>

    `oc describe daemonset omsagent`  

    <span data-ttu-id="ea2b0-284">och hello utdata bör likna:</span><span class="sxs-lookup"><span data-stu-id="ea2b0-284">and hello output should resemble:</span></span>

    ```
    [ocpadmin@khm-0 ~]$ oc describe ds oms  
    Name:           oms  
    Image(s):       microsoft/oms  
    Selector:       name=omsagent  
    Node-Selector:  zone=default  
    Labels:         agentVersion=1.4.0-12  
                    dockerProviderVersion=10.0.0-25  
                    name=omsagent  
    Desired Number of Nodes Scheduled: 3  
    Current Number of Nodes Scheduled: 3  
    Number of Nodes Misscheduled: 0  
    Pods Status:    3 Running / 0 Waiting / 0 Succeeded / 0 Failed  
    No events.  
    ```

<span data-ttu-id="ea2b0-285">Om du vill toouse hemligheter toosecure din OMS arbetsyte-ID och primärnyckel när du använder hello OMS-Agent daemon set yaml-fil, utföra hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-285">If you want toouse secrets toosecure your OMS Workspace ID and Primary Key when using hello OMS Agent daemon-set yaml file, perform hello following steps.</span></span>

1. <span data-ttu-id="ea2b0-286">Logga in toohello OpenShift nod och kopiera hello yaml huvudfilen [ocp-ds-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-ds-omsagent.yaml) och hemlighet genererar skript [ocp-secretgen.sh](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-secretgen.sh) från GitHub.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-286">Sign on toohello OpenShift master node and copy hello yaml file [ocp-ds-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-ds-omsagent.yaml) and secret generating script [ocp-secretgen.sh](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-secretgen.sh) from GitHub.</span></span>  <span data-ttu-id="ea2b0-287">Det här skriptet genererar hello hemligheter yaml-filen för OMS arbetsyte-ID och primärnyckel toosecure din secrete information.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-287">This script will generate hello secrets yaml file for OMS Workspace ID and Primary Key toosecure your secrete information.</span></span>  
2. <span data-ttu-id="ea2b0-288">Kör följande kommandon toocreate hello ett projekt för OMS och ange hello användarkonto.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-288">Run hello following commands toocreate a project for OMS and set hello user account.</span></span> <span data-ttu-id="ea2b0-289">hello hemlighet genererar skript begär din OMS arbetsyte-ID <WSID> och primärnyckel <KEY> och efter slutförande, skapar hello ocp-secret.yaml fil.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-289">hello secret generating script asks for your OMS Workspace ID <WSID> and Primary Key <KEY> and upon completion, it creates hello ocp-secret.yaml file.</span></span>  

    ```
    oadm new-project omslogging --node-selector='zone=default'  
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. <span data-ttu-id="ea2b0-290">Distribuera hello hemliga fil genom att köra hello följande:</span><span class="sxs-lookup"><span data-stu-id="ea2b0-290">Deploy hello secret file by running hello following:</span></span>

    `oc create -f ocp-secret.yaml`

5. <span data-ttu-id="ea2b0-291">Kontrollera distributionen genom att köra hello följande:</span><span class="sxs-lookup"><span data-stu-id="ea2b0-291">Verify deployment by running hello following:</span></span>

    `oc describe secret omsagent-secret`  

    <span data-ttu-id="ea2b0-292">och hello utdata bör likna:</span><span class="sxs-lookup"><span data-stu-id="ea2b0-292">and hello  output should resemble:</span></span>  

    ```
    [ocpadmin@khocp-master-0 ~]$ oc describe ds oms  
    Name:           oms  
    Image(s):       microsoft/oms  
    Selector:       name=omsagent  
    Node-Selector:  zone=default  
    Labels:         agentVersion=1.4.0-12  
                    dockerProviderVersion=10.0.0-25  
                    name=omsagent  
    Desired Number of Nodes Scheduled: 3  
    Current Number of Nodes Scheduled: 3  
    Number of Nodes Misscheduled: 0  
    Pods Status:    3 Running / 0 Waiting / 0 Succeeded / 0 Failed  
    No events.  
    ```

6. <span data-ttu-id="ea2b0-293">Distribuera hello OMS-Agent daemon set yaml fil genom att köra hello följande:</span><span class="sxs-lookup"><span data-stu-id="ea2b0-293">Deploy hello OMS Agent daemon-set yaml file by running hello following:</span></span>

    `oc create -f ocp-ds-omsagent.yaml`  

7. <span data-ttu-id="ea2b0-294">Kontrollera distributionen genom att köra hello följande:</span><span class="sxs-lookup"><span data-stu-id="ea2b0-294">Verify deployment by running hello following:</span></span>

    `oc describe ds oms`

    <span data-ttu-id="ea2b0-295">och hello utdata bör likna:</span><span class="sxs-lookup"><span data-stu-id="ea2b0-295">and hello output should resemble:</span></span>

    ```
    [ocpadmin@khocp-master-0 ~]$ oc describe secret omsagent-secret  
    Name:           omsagent-secret  
    Namespace:      omslogging  
    Labels:         <none>  
    Annotations:    <none>  

    Type:   Opaque  

     Data  
     ====  
     KEY:    89 bytes  
     WSID:   37 bytes  
    ```

### <a name="secure-your-secret-information-for-docker-swarm-and-kubernetes"></a><span data-ttu-id="ea2b0-296">Skydda din hemliga information för Docker Swarm och Kubernetes</span><span class="sxs-lookup"><span data-stu-id="ea2b0-296">Secure your secret information for Docker Swarm and Kubernetes</span></span>

<span data-ttu-id="ea2b0-297">Du kan skydda din hemliga OMS arbetsyte-ID och primärnycklar för Docker Swarm och Kubernetes behållartjänster.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-297">You can secure your secret OMS Workspace ID and Primary Keys for Docker Swarm and Kubernetes container services.</span></span>

#### <a name="secure-secrets-for-docker-swarm"></a><span data-ttu-id="ea2b0-298">Säker hemligheter för Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="ea2b0-298">Secure secrets for Docker Swarm</span></span>

<span data-ttu-id="ea2b0-299">För Docker Swarm när hello hemligheten för arbetsyte-ID och primärnyckel har skapats kan du köra hello skapa hello Docker-tjänst för OMSagent.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-299">For Docker Swarm, once hello secret for Workspace ID and Primary Key is created, you can run hello create hello Docker service for OMSagent.</span></span> <span data-ttu-id="ea2b0-300">Använd följande information toocreate hello din hemliga information.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-300">Use hello following information toocreate your secret information.</span></span>

1. <span data-ttu-id="ea2b0-301">Kör följande hello på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-301">Run hello following on hello master node.</span></span>

    ```
    echo "WSID" | docker secret create WSID -
    echo "KEY" | docker secret create KEY -
    ```

2. <span data-ttu-id="ea2b0-302">Kontrollera att hemligheter har skapats korrekt.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-302">Verify that secrets were created properly.</span></span>

    ```
    keiko@swarmm-master-13957614-0:/run# sudo docker secret ls
    ```

    ```
    ID                          NAME                CREATED             UPDATED
    j2fj153zxy91j8zbcitnjxjiv   WSID                43 minutes ago      43 minutes ago
    l9rh3n987g9c45zffuxdxetd9   KEY                 38 minutes ago      38 minutes ago
    ```

3. <span data-ttu-id="ea2b0-303">Container kör hello efter kommandot toomount hello hemligheter toohello OMS-Agent.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-303">Run hello following command toomount hello secrets toohello containerized OMS Agent.</span></span>

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock --secret source=WSID,target=WSID --secret source=KEY,target=KEY  -p 25225:25225 -p 25224:25224/udp --restart-condition=on-failure microsoft/oms
    ```

#### <a name="secure-secrets-for-kubernetes-with-yaml-files"></a><span data-ttu-id="ea2b0-304">Säker hemligheter för Kubernetes med yaml-filer</span><span class="sxs-lookup"><span data-stu-id="ea2b0-304">Secure secrets for Kubernetes with yaml files</span></span>

<span data-ttu-id="ea2b0-305">För Kubernetes använder du en toogenerate hello hemligheter yaml-skriptfil för ditt arbetsyte-ID och primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-305">For Kubernetes, you use a script toogenerate hello secrets yaml file for your Workspace ID and Primary Key.</span></span> <span data-ttu-id="ea2b0-306">Vid hello [OMS Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) , finns på filer som du kan använda med eller utan din hemliga information.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-306">At hello [OMS Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) page, there are files that you can use with or without your secret information.</span></span>

- <span data-ttu-id="ea2b0-307">Hej DaemonSet för standard OMS-Agent har inte hemlig information (omsagent.yaml)</span><span class="sxs-lookup"><span data-stu-id="ea2b0-307">hello Default OMS Agent DaemonSet does not have secret information (omsagent.yaml)</span></span>
- <span data-ttu-id="ea2b0-308">hello OMS-agenten DaemonSet yaml filen använder hemlig information (omsagent-ds-secrets.yaml) med hemliga skript toogenerate hello hemligheter yaml (omsagentsecret.yaml)-fil för generation.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-308">hello OMS Agent DaemonSet yaml file uses secret information (omsagent-ds-secrets.yaml) with secret generation scripts toogenerate hello secrets yaml (omsagentsecret.yaml) file.</span></span>

<span data-ttu-id="ea2b0-309">Du kan välja toocreate omsagent DaemonSets med eller utan hemligheter.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-309">You can choose toocreate omsagent DaemonSets with or without secrets.</span></span>

##### <a name="default-omsagent-daemonset-yaml-file-without-secrets"></a><span data-ttu-id="ea2b0-310">Standardfilen OMSagent DaemonSet yaml utan hemligheter</span><span class="sxs-lookup"><span data-stu-id="ea2b0-310">Default OMSagent DaemonSet yaml file without secrets</span></span>

- <span data-ttu-id="ea2b0-311">För hello OMS-agenten DaemonSet yaml standardfil, ersätter du hello `<WSID>` och `<KEY>` tooyour WSID och nyckel.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-311">For hello default OMS Agent DaemonSet yaml file, replace hello `<WSID>` and `<KEY>` tooyour WSID and KEY.</span></span> <span data-ttu-id="ea2b0-312">Kopiera filen hello tooyour huvudnod och kör hello följande:</span><span class="sxs-lookup"><span data-stu-id="ea2b0-312">Copy hello file tooyour master node and run hello following:</span></span>

    ```
    sudo kubectl create -f omsagent.yaml
    ```

##### <a name="default-omsagent-daemonset-yaml-file-with-secrets"></a><span data-ttu-id="ea2b0-313">Standardfilen OMSagent DaemonSet yaml med hemligheter</span><span class="sxs-lookup"><span data-stu-id="ea2b0-313">Default OMSagent DaemonSet yaml file with secrets</span></span>

1. <span data-ttu-id="ea2b0-314">toouse OMS-agenten DaemonSet med hemliga information, skapa hello hemligheter först.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-314">toouse OMS Agent DaemonSet using secret information, create hello secrets first.</span></span>
    1. <span data-ttu-id="ea2b0-315">Kopiera hello skript och hemliga mallfilen och kontrollera att de är på hello samma katalog.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-315">Copy hello script and secret template file and make sure they are on hello same directory.</span></span>
        - <span data-ttu-id="ea2b0-316">Hemligt genererar skript - hemlighet gen.sh</span><span class="sxs-lookup"><span data-stu-id="ea2b0-316">Secret generating script - secret-gen.sh</span></span>
        - <span data-ttu-id="ea2b0-317">Hemlig mall - hemlighet template.yaml</span><span class="sxs-lookup"><span data-stu-id="ea2b0-317">secret template - secret-template.yaml</span></span>
    2. <span data-ttu-id="ea2b0-318">Kör hello skript som hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-318">Run hello script, like hello following example.</span></span> <span data-ttu-id="ea2b0-319">hello skriptet begär hello OMS arbetsyte-ID och primärnyckel och när du har angett dem hello skriptet skapar en hemlig yaml-fil så att du kan köra den.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-319">hello script asks for hello OMS Workspace ID and Primary Key and after you enter them, hello script creates a secret yaml file so you can run it.</span></span>   

        ```
        #> sudo bash ./secret-gen.sh
        ```

    3. <span data-ttu-id="ea2b0-320">Skapa hello hemligheter baljor genom att köra hello följande:</span><span class="sxs-lookup"><span data-stu-id="ea2b0-320">Create hello secrets pod by running hello following:</span></span>
        ```
        sudo kubectl create -f omsagentsecret.yaml
        ```

    4. <span data-ttu-id="ea2b0-321">tooverify kör hello följande:</span><span class="sxs-lookup"><span data-stu-id="ea2b0-321">tooverify, run hello following:</span></span>

        ```
        keiko@ubuntu16-13db:~# sudo kubectl get secrets
        ```

        <span data-ttu-id="ea2b0-322">Resultatet bör likna:</span><span class="sxs-lookup"><span data-stu-id="ea2b0-322">Output should resemble:</span></span>

        ```
        NAME                  TYPE                                  DATA      AGE
        default-token-gvl91   kubernetes.io/service-account-token   3         50d
        omsagent-secret       Opaque                                2         1d
        ```

        ```
        keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
        ```

        <span data-ttu-id="ea2b0-323">Resultatet bör likna:</span><span class="sxs-lookup"><span data-stu-id="ea2b0-323">Output should resemble:</span></span>

        ```
        Name:           omsagent-secret
        Namespace:      default
        Labels:         <none>
        Annotations:    <none>

        Type:   Opaque

        Data
        ====
        WSID:   36 bytes
        KEY:    88 bytes
        ```

    5. <span data-ttu-id="ea2b0-324">Skapa din omsagent daemon set genom att köra``` sudo kubectl create -f omsagent-ds-secrets.yaml ```</span><span class="sxs-lookup"><span data-stu-id="ea2b0-324">Create your omsagent daemon-set by running ``` sudo kubectl create -f omsagent-ds-secrets.yaml ```</span></span>

2. <span data-ttu-id="ea2b0-325">Kontrollera att hello OMS-agenten DaemonSet körs liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="ea2b0-325">Verify that hello OMS Agent DaemonSet is running, similar toohello following:</span></span>

    ```
    keiko@ubuntu16-13db:~# sudo kubectl get ds omsagent
    ```

    ```
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   3         3         <none>          1h
    ```


<span data-ttu-id="ea2b0-326">Använd en toogenerate hello hemligheter yaml-skriptfil för arbetsyte-ID och primärnyckel för Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-326">For Kubernetes, use a script toogenerate hello secrets yaml file for Workspace ID and Primary Key.</span></span> <span data-ttu-id="ea2b0-327">Använd hello följande exempelinformationen med hello [omsagent yaml filen](https://github.com/Microsoft/OMS-docker/blob/master/Kubernetes/omsagent.yaml) toosecure din hemliga information.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-327">Use hello following example information with hello [omsagent yaml file](https://github.com/Microsoft/OMS-docker/blob/master/Kubernetes/omsagent.yaml) toosecure your secret information.</span></span>

```
keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
Name:           omsagent-secret
Namespace:      default
Labels:         <none>
Annotations:    <none>

Type:   Opaque

Data
====
WSID:   36 bytes
KEY:    88 bytes
```

## <a name="windows-container-hosts"></a><span data-ttu-id="ea2b0-328">Värdar för Windows-behållare</span><span class="sxs-lookup"><span data-stu-id="ea2b0-328">Windows container hosts</span></span>

### <a name="preparation-before-installing-windows-agents"></a><span data-ttu-id="ea2b0-329">Förberedelser innan du installerar Windows-agenter</span><span class="sxs-lookup"><span data-stu-id="ea2b0-329">Preparation before installing Windows agents</span></span>

<span data-ttu-id="ea2b0-330">Innan du installerar agenter på datorer som kör Windows, behöver du tooconfigure hello Docker-tjänst.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-330">Before you install agents on computers running Windows, you need tooconfigure hello Docker service.</span></span> <span data-ttu-id="ea2b0-331">hello konfiguration kan hello Windows-agenten eller hello logganalys virtuella tillägget toouse hello Docker TCP socket så att hello agenter kan fjärransluta till Docker-daemon för hello och toocapture data för övervakning.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-331">hello configuration allows hello Windows agent or hello Log Analytics virtual machine extension toouse hello Docker TCP socket so that hello agents can access hello Docker daemon remotely and toocapture data for monitoring.</span></span>

#### <a name="toostart-docker-and-verify-its-configuration"></a><span data-ttu-id="ea2b0-332">toostart Docker och verifiera konfigurationen</span><span class="sxs-lookup"><span data-stu-id="ea2b0-332">toostart Docker and verify its configuration</span></span>

<span data-ttu-id="ea2b0-333">Det finns steg som krävs för tooset in TCP namngiven pipe för Windows Server:</span><span class="sxs-lookup"><span data-stu-id="ea2b0-333">There are steps needed tooset up TCP named pipe for Windows Server:</span></span>

1. <span data-ttu-id="ea2b0-334">Aktivera TCP pipe och namngiven pipe i Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-334">In Windows PowerShell, enable TCP pipe and named pipe.</span></span>

    ```
    Stop-Service docker
    dockerd --unregister-service
    dockerd --register-service -H npipe:// -H 0.0.0.0:2375  
    Start-Service docker
    ```

2. <span data-ttu-id="ea2b0-335">Konfigurera Docker med hello konfigurationsfilen för TCP pipe och namngiven pipe.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-335">Configure Docker with hello configuration file for TCP pipe and named pipe.</span></span> <span data-ttu-id="ea2b0-336">hello-konfigurationsfilen finns på C:\ProgramData\docker\config\daemon.json.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-336">hello configuration file is located at C:\ProgramData\docker\config\daemon.json.</span></span>

    <span data-ttu-id="ea2b0-337">Hej daemon.json filen måste hello följande:</span><span class="sxs-lookup"><span data-stu-id="ea2b0-337">In hello daemon.json file, you will need hello following:</span></span>

    ```
    {
    "hosts": ["tcp://0.0.0.0:2375", "npipe://"]
    }
    ```

<span data-ttu-id="ea2b0-338">Mer information om hello Docker daemon konfiguration används med Windows-behållare finns [Docker-motorn på Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).</span><span class="sxs-lookup"><span data-stu-id="ea2b0-338">For more information about hello Docker daemon configuration used with Windows Containers, see [Docker Engine on Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).</span></span>


### <a name="install-windows-agents"></a><span data-ttu-id="ea2b0-339">Installera Windows-agenter</span><span class="sxs-lookup"><span data-stu-id="ea2b0-339">Install Windows agents</span></span>

<span data-ttu-id="ea2b0-340">tooenable Windows och Hyper-V-behållare som övervakning, installera hello Microsoft Monitoring Agent (MMA) på Windows-datorer som är värdar för behållaren.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-340">tooenable Windows and Hyper-V container monitoring, install hello Microsoft Monitoring Agent (MMA) on Windows computers that are container hosts.</span></span> <span data-ttu-id="ea2b0-341">Windows-datorer i din lokala miljö, se [ansluta Windows-datorer tooLog Analytics](log-analytics-windows-agents.md).</span><span class="sxs-lookup"><span data-stu-id="ea2b0-341">For computers running Windows in your on-premises environment, see [Connect Windows computers tooLog Analytics](log-analytics-windows-agents.md).</span></span> <span data-ttu-id="ea2b0-342">För virtuella datorer körs i Azure och Anslut dem tooLog Analytics med hjälp av hello [tillägg för virtuell dator](log-analytics-azure-vm-extension.md).</span><span class="sxs-lookup"><span data-stu-id="ea2b0-342">For virtual machines running in Azure, connect them tooLog Analytics using hello [virtual machine extension](log-analytics-azure-vm-extension.md).</span></span>

<span data-ttu-id="ea2b0-343">Du kan övervaka Windows-behållare som körs på Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-343">You can monitor Windows containers running on Service Fabric.</span></span> <span data-ttu-id="ea2b0-344">Dock endast [virtuella datorer som körs i Azure](log-analytics-azure-vm-extension.md) och [Windows-datorer i din lokala miljö](log-analytics-windows-agents.md) stöds för närvarande för Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-344">However, only [virtual machines running in Azure](log-analytics-azure-vm-extension.md) and [computers running Windows in your on-premises environment](log-analytics-windows-agents.md) are currently supported for Service Fabric.</span></span>

<span data-ttu-id="ea2b0-345">Du kan kontrollera att hello behållaren övervakning är korrekt inställda för Windows.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-345">You can verify that hello Container Monitoring solution is set correctly for Windows.</span></span> <span data-ttu-id="ea2b0-346">toocheck om hello management pack var download korrekt, leta efter *ContainerManagement.xxx*.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-346">toocheck whether hello management pack was download properly, look for *ContainerManagement.xxx*.</span></span> <span data-ttu-id="ea2b0-347">hello filer måste vara i hello C:\Program Files\Microsoft övervakning Agent\Agent\Health State\Management servicepack mapp.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-347">hello files should be in hello C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs folder.</span></span>


## <a name="solution-components"></a><span data-ttu-id="ea2b0-348">Lösningskomponenter</span><span class="sxs-lookup"><span data-stu-id="ea2b0-348">Solution components</span></span>

<span data-ttu-id="ea2b0-349">Om du använder Windows-agenter är sedan hello följande hanteringspaket installerat på varje dator med en agent när du lägger till den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-349">If you are using Windows agents, then hello following management pack is installed on each computer with an agent when you add this solution.</span></span> <span data-ttu-id="ea2b0-350">Ingen konfiguration eller underhåll krävs för hello management pack.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-350">No configuration or maintenance is required for hello management pack.</span></span>

- <span data-ttu-id="ea2b0-351">*ContainerManagement.xxx* installerats i C:\Program Files\Microsoft övervakning Agent\Agent\Health State\Management servicepack</span><span class="sxs-lookup"><span data-stu-id="ea2b0-351">*ContainerManagement.xxx* installed in C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs</span></span>

## <a name="container-data-collection-details"></a><span data-ttu-id="ea2b0-352">Behållaren information för samlingen</span><span class="sxs-lookup"><span data-stu-id="ea2b0-352">Container data collection details</span></span>
<span data-ttu-id="ea2b0-353">hello behållaren övervakning samlar in olika mått och logga prestandadata från behållaren värdar och behållare med agenter som du aktiverar.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-353">hello Container Monitoring solution collects various performance metrics and log data from container hosts and containers using agents that you enable.</span></span>

<span data-ttu-id="ea2b0-354">Data samlas in varje tre minuter av hello följande typer av agenten.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-354">Data is collected every three minutes by hello following agent types.</span></span>

- [<span data-ttu-id="ea2b0-355">OMS-Agent för Linux</span><span class="sxs-lookup"><span data-stu-id="ea2b0-355">OMS Agent for Linux</span></span>](log-analytics-linux-agents.md)
- [<span data-ttu-id="ea2b0-356">Windows-agenten</span><span class="sxs-lookup"><span data-stu-id="ea2b0-356">Windows agent</span></span>](log-analytics-windows-agents.md)
- [<span data-ttu-id="ea2b0-357">Log Analytics VM-tillägget</span><span class="sxs-lookup"><span data-stu-id="ea2b0-357">Log Analytics VM extension</span></span>](log-analytics-azure-vm-extension.md)


### <a name="container-records"></a><span data-ttu-id="ea2b0-358">Behållaren innehåller</span><span class="sxs-lookup"><span data-stu-id="ea2b0-358">Container records</span></span>

<span data-ttu-id="ea2b0-359">hello visas följande tabell exempel på poster som samlas in av hello behållaren övervakning lösningen och hello-datatyper som visas i sökresultaten för loggen.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-359">hello following table shows examples of records collected by hello Container Monitoring solution and hello data types that appear in log search results.</span></span>

| <span data-ttu-id="ea2b0-360">Datatyp</span><span class="sxs-lookup"><span data-stu-id="ea2b0-360">Data type</span></span> | <span data-ttu-id="ea2b0-361">Datatypen i loggen Sök</span><span class="sxs-lookup"><span data-stu-id="ea2b0-361">Data type in Log Search</span></span> | <span data-ttu-id="ea2b0-362">Fält</span><span class="sxs-lookup"><span data-stu-id="ea2b0-362">Fields</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ea2b0-363">Prestanda för värdar och -behållare</span><span class="sxs-lookup"><span data-stu-id="ea2b0-363">Performance for hosts and containers</span></span> | `Type=Perf` | <span data-ttu-id="ea2b0-364">% Processortid för datorn, objektnamn, CounterName &#40; disken läser MB, Disk skriver MB minne användning MB nätverket tar emot byte, nätverket skicka byte, Processor användning sek nätverket &#41; CounterValue, TimeGenerated, räknarsökväg, SourceSystem</span><span class="sxs-lookup"><span data-stu-id="ea2b0-364">Computer, ObjectName, CounterName &#40;%Processor Time, Disk Reads MB, Disk Writes MB, Memory Usage MB, Network Receive Bytes, Network Send Bytes, Processor Usage sec, Network&#41;, CounterValue,TimeGenerated, CounterPath, SourceSystem</span></span> |
| <span data-ttu-id="ea2b0-365">Behållaren inventering</span><span class="sxs-lookup"><span data-stu-id="ea2b0-365">Container inventory</span></span> | `Type=ContainerInventory` | <span data-ttu-id="ea2b0-366">TimeGenerated, dator, behållarnamn ContainerHostname, bild, ImageTag, ContainerState, ExitCode, EnvironmentVar, kommandot, CreatedTime, StartedTime, FinishedTime, SourceSystem, behållar-ID, ImageID</span><span class="sxs-lookup"><span data-stu-id="ea2b0-366">TimeGenerated, Computer, container name, ContainerHostname, Image, ImageTag, ContainerState, ExitCode, EnvironmentVar, Command, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID</span></span> |
| <span data-ttu-id="ea2b0-367">Behållaren image inventering</span><span class="sxs-lookup"><span data-stu-id="ea2b0-367">Container image inventory</span></span> | `Type=ContainerImageInventory` | <span data-ttu-id="ea2b0-368">TimeGenerated, dator, bild, ImageTag, ImageSize, VirtualSize, körs, pausas, stoppas, misslyckades, SourceSystem, ImageID, TotalContainer</span><span class="sxs-lookup"><span data-stu-id="ea2b0-368">TimeGenerated, Computer, Image, ImageTag, ImageSize, VirtualSize, Running, Paused, Stopped, Failed, SourceSystem, ImageID, TotalContainer</span></span> |
| <span data-ttu-id="ea2b0-369">Behållaren logg</span><span class="sxs-lookup"><span data-stu-id="ea2b0-369">Container log</span></span> | `Type=ContainerLog` | <span data-ttu-id="ea2b0-370">TimeGenerated, dator, avbildnings-ID, behållarnamn LogEntrySource, LogEntry, SourceSystem, behållar-ID</span><span class="sxs-lookup"><span data-stu-id="ea2b0-370">TimeGenerated, Computer, image ID, container name, LogEntrySource, LogEntry, SourceSystem, ContainerID</span></span> |
| <span data-ttu-id="ea2b0-371">Behållaren loggfiler</span><span class="sxs-lookup"><span data-stu-id="ea2b0-371">Container service log</span></span> | `Type=ContainerServiceLog`  | <span data-ttu-id="ea2b0-372">TimeGenerated, dator, TimeOfCommand, bild, kommandot, SourceSystem, behållar-ID</span><span class="sxs-lookup"><span data-stu-id="ea2b0-372">TimeGenerated, Computer, TimeOfCommand, Image, Command, SourceSystem, ContainerID</span></span> |
| <span data-ttu-id="ea2b0-373">Behållaren nod inventering</span><span class="sxs-lookup"><span data-stu-id="ea2b0-373">Container node inventory</span></span> | `Type=ContainerNodeInventory_CL`| <span data-ttu-id="ea2b0-374">TimeGenerated, dator, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem</span><span class="sxs-lookup"><span data-stu-id="ea2b0-374">TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem</span></span>|
| <span data-ttu-id="ea2b0-375">Kubernetes inventering</span><span class="sxs-lookup"><span data-stu-id="ea2b0-375">Kubernetes inventory</span></span> | `Type=KubePodInventory_CL` | <span data-ttu-id="ea2b0-376">TimeGenerated, dator, PodLabel_deployment_s, PodLabel_deploymentconfig_s, PodLabel_docker_registry_s, Name_s, Namespace_s, PodStatus_s, PodIp_s, PodUid_g, PodCreationTimeStamp_t, SourceSystem</span><span class="sxs-lookup"><span data-stu-id="ea2b0-376">TimeGenerated, Computer, PodLabel_deployment_s, PodLabel_deploymentconfig_s, PodLabel_docker_registry_s, Name_s, Namespace_s, PodStatus_s, PodIp_s, PodUid_g, PodCreationTimeStamp_t, SourceSystem</span></span> |
| <span data-ttu-id="ea2b0-377">Behållaren process</span><span class="sxs-lookup"><span data-stu-id="ea2b0-377">Container process</span></span> | `Type=ContainerProcess_CL` | <span data-ttu-id="ea2b0-378">TimeGenerated, dator, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem</span><span class="sxs-lookup"><span data-stu-id="ea2b0-378">TimeGenerated, Computer, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem</span></span> |
| <span data-ttu-id="ea2b0-379">Kubernetes händelser</span><span class="sxs-lookup"><span data-stu-id="ea2b0-379">Kubernetes events</span></span> | `Type=KubeEvents_CL` | <span data-ttu-id="ea2b0-380">TimeGenerated, dator, Name_s, ObjectKind_s, Namespace_s, Reason_s, Type_s, SourceComponent_s, SourceSystem, meddelande</span><span class="sxs-lookup"><span data-stu-id="ea2b0-380">TimeGenerated, Computer, Name_s, ObjectKind_s, Namespace_s, Reason_s, Type_s, SourceComponent_s, SourceSystem, Message</span></span> |

<span data-ttu-id="ea2b0-381">Etiketter för läggs*PodLabel* datatyper är egna etiketter.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-381">Labels appended too*PodLabel* data types are your own custom labels.</span></span> <span data-ttu-id="ea2b0-382">hello är tillagda PodLabel etiketter hello tabell exempel.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-382">hello appended PodLabel labels shown in hello table are examples.</span></span> <span data-ttu-id="ea2b0-383">Därför `PodLabel_deployment_s`, `PodLabel_deploymentconfig_s`, `PodLabel_docker_registry_s` kommer skiljer sig åt i din miljö datauppsättning och allmänt likna `PodLabel_yourlabel_s`.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-383">So, `PodLabel_deployment_s`, `PodLabel_deploymentconfig_s`, `PodLabel_docker_registry_s` will differ in your environment's data set and generically resemble `PodLabel_yourlabel_s`.</span></span>


## <a name="monitor-containers"></a><span data-ttu-id="ea2b0-384">Övervakaren behållare</span><span class="sxs-lookup"><span data-stu-id="ea2b0-384">Monitor containers</span></span>
<span data-ttu-id="ea2b0-385">När du har hello lösning aktiverat i hello OMS-portalen hello **behållare** innehåller översiktlig information om dina behållaren värdar och hello-behållare som körs på värdar.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-385">After you have hello solution enabled in hello OMS portal, hello **Containers** tile shows summary information about your container hosts and hello containers running in hosts.</span></span>

![Behållare sida vid sida](./media/log-analytics-containers/containers-title.png)

<span data-ttu-id="ea2b0-387">hello innehåller en översikt över hur många behållare som du har i hello-miljö och om de är misslyckades, igång eller stoppad.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-387">hello tile shows an overview of how many containers you have in hello environment and whether they're failed, running, or stopped.</span></span>

### <a name="using-hello-containers-dashboard"></a><span data-ttu-id="ea2b0-388">Med hjälp av instrumentpanelen för hello-behållare</span><span class="sxs-lookup"><span data-stu-id="ea2b0-388">Using hello Containers dashboard</span></span>
<span data-ttu-id="ea2b0-389">Klicka på hello **behållare** panelen.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-389">Click hello **Containers** tile.</span></span> <span data-ttu-id="ea2b0-390">Därifrån ser du vyer som är ordnad efter:</span><span class="sxs-lookup"><span data-stu-id="ea2b0-390">From there you'll see views organized by:</span></span>

- <span data-ttu-id="ea2b0-391">**Behållaren händelser** – visar status för behållaren och datorer med misslyckade behållare.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-391">**Container Events** - Shows container status and computers with failed containers.</span></span>
- <span data-ttu-id="ea2b0-392">**Loggar för behållaren** – visar ett diagram över behållaren loggfiler som skapas med tiden och en lista med datorer med hello högsta antal loggfiler.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-392">**Container Logs** - Shows a chart of container log files generated over time and a list of computers with hello highest number of log files.</span></span>
- <span data-ttu-id="ea2b0-393">**Kubernetes händelser** -visas ett diagram över Kubernetes händelser som genererats med tiden och en lista över hello orsaker varför skida genererat hello händelser.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-393">**Kubernetes Events** - Shows a chart of Kubernetes events generated over time and a list of hello reasons why pods generated hello events.</span></span> <span data-ttu-id="ea2b0-394">*Den här datamängden används endast i Linux-miljöer.*</span><span class="sxs-lookup"><span data-stu-id="ea2b0-394">*This data set is used only in Linux environments.*</span></span>
- <span data-ttu-id="ea2b0-395">**Kubernetes Namespace inventering** – visar hello antalet namnområden och skida och deras hierarki.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-395">**Kubernetes Namespace Inventory** - Shows hello number of namespaces and pods and shows their hierarchy.</span></span> <span data-ttu-id="ea2b0-396">*Den här datamängden används endast i Linux-miljöer.*</span><span class="sxs-lookup"><span data-stu-id="ea2b0-396">*This data set is used only in Linux environments.*</span></span>
- <span data-ttu-id="ea2b0-397">**Behållaren nod inventering** -visar hello antalet orchestration-typer som används på behållaren noder/värdar.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-397">**Container Node Inventory** - Shows hello number of orchestration types used on container nodes/hosts.</span></span> <span data-ttu-id="ea2b0-398">hello datorn noder/värdarna visas också av hello antal behållare.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-398">hello computer nodes/hosts are also listed by hello number of containers.</span></span> <span data-ttu-id="ea2b0-399">*Den här datamängden används endast i Linux-miljöer.*</span><span class="sxs-lookup"><span data-stu-id="ea2b0-399">*This data set is used only in Linux environments.*</span></span>
- <span data-ttu-id="ea2b0-400">**Behållaren bilder inventering** -visar hello Totalt antal behållare bilder som används och antalet bildtyper.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-400">**Container Images Inventory** - Shows hello total number of container images used and number of image types.</span></span> <span data-ttu-id="ea2b0-401">hello antalet avbildningar som också anges med hello bildtagg.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-401">hello number of images are also listed by hello image tag.</span></span>
- <span data-ttu-id="ea2b0-402">**Behållare Status** -visar hello Totalt antal behållare noder/värddatorer behållare som körs.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-402">**Containers Status** - Shows hello total number of container nodes/host computers that have running containers.</span></span> <span data-ttu-id="ea2b0-403">Datorer visas också av hello antalet värdar som körs.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-403">Computers are also listed by hello number of running hosts.</span></span>
- <span data-ttu-id="ea2b0-404">**Behållaren processen** – visar ett linjediagram för behållaren processer som körs över tid.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-404">**Container Process** - Shows a line chart of container processes running over time.</span></span> <span data-ttu-id="ea2b0-405">Behållare visas också genom att köra kommandot/processen i behållare.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-405">Containers are also listed by running command/process within containers.</span></span> <span data-ttu-id="ea2b0-406">*Den här datamängden används endast i Linux-miljöer.*</span><span class="sxs-lookup"><span data-stu-id="ea2b0-406">*This data set is used only in Linux environments.*</span></span>
- <span data-ttu-id="ea2b0-407">**Behållaren CPU-prestanda** – visar ett linjediagram av hello genomsnittliga processoranvändningen över tid för datorn är noder/värd.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-407">**Container CPU Performance** - Shows a line chart of hello average CPU utilization over time for computer nodes/hosts.</span></span> <span data-ttu-id="ea2b0-408">Även baserat visar hello datorn noder/värdar på Genomsnittlig CPU-användning.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-408">Also lists hello computer nodes/hosts based on average CPU utilization.</span></span>
- <span data-ttu-id="ea2b0-409">**Behållaren minnesprestanda** – visar ett linjediagram minnesanvändningen över tid.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-409">**Container Memory Performance** - Shows a line chart of memory usage over time.</span></span> <span data-ttu-id="ea2b0-410">Visar också minne användning baserat på instansnamn.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-410">Also lists computer memory utilization based on instance name.</span></span>
- <span data-ttu-id="ea2b0-411">**Datorprestanda** -visar linjediagram hello procent av CPU-prestanda över tiden, procent av minnesanvändning över tid och MB ledigt diskutrymme över tid.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-411">**Computer Performance** - Shows line charts of hello percent of CPU performance over time, percent of memory usage over time, and megabytes of free disk space over time.</span></span> <span data-ttu-id="ea2b0-412">Du kan hovra över en rad i en diagrammet tooview mer information.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-412">You can hover over any line in a chart tooview more details.</span></span>


<span data-ttu-id="ea2b0-413">Varje område av hello instrumentpanelen är en bild av en sökning som körs på insamlade data.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-413">Each area of hello dashboard is a visual representation of a search that is run on collected data.</span></span>

![Behållare instrumentpanelen](./media/log-analytics-containers/containers-dash01.png)

![Behållare instrumentpanelen](./media/log-analytics-containers/containers-dash02.png)

<span data-ttu-id="ea2b0-416">I hello **behållare Status** området klickar du på hello ytan, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-416">In hello **Container Status** area, click hello top area, as shown below.</span></span>

![Behållare status](./media/log-analytics-containers/containers-status.png)

<span data-ttu-id="ea2b0-418">Sök i loggfilen öppnas och visar information om hello tillstånd för en behållare.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-418">Log Search opens, displaying information about hello state of your containers.</span></span>

![Logga Sök efter behållare](./media/log-analytics-containers/containers-log-search.png)

<span data-ttu-id="ea2b0-420">Härifrån kan du redigera hello Sök frågan toomodify den toofind hello specifik information du är intresserad av.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-420">From here, you can edit hello search query toomodify it toofind hello specific information you're interested in.</span></span> <span data-ttu-id="ea2b0-421">Mer information om loggen sökningar finns [logga sökningar i logganalys](log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="ea2b0-421">For more information about Log Searches, see [Log searches in Log Analytics](log-analytics-log-searches.md).</span></span>

## <a name="troubleshoot-by-finding-a-failed-container"></a><span data-ttu-id="ea2b0-422">Felsöka genom att söka efter en misslyckad behållare</span><span class="sxs-lookup"><span data-stu-id="ea2b0-422">Troubleshoot by finding a failed container</span></span>

<span data-ttu-id="ea2b0-423">Logganalys markerar en behållare som **misslyckades** om den har avslutats med slutkoden noll.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-423">Log Analytics marks a container as **Failed** if it has exited with a non-zero exit code.</span></span> <span data-ttu-id="ea2b0-424">Du kan se en översikt över hello fel och problem i hello miljö hello **misslyckades behållare** område.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-424">You can see an overview of hello errors and failures in hello environment in hello **Failed Containers** area.</span></span>

### <a name="toofind-failed-containers"></a><span data-ttu-id="ea2b0-425">toofind misslyckades behållare</span><span class="sxs-lookup"><span data-stu-id="ea2b0-425">toofind failed containers</span></span>
1. <span data-ttu-id="ea2b0-426">Klicka på hello **behållare Status** område.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-426">Click hello **Container Status** area.</span></span>  
   <span data-ttu-id="ea2b0-427">![behållare status](./media/log-analytics-containers/containers-status.png)</span><span class="sxs-lookup"><span data-stu-id="ea2b0-427">![containers status](./media/log-analytics-containers/containers-status.png)</span></span>
2. <span data-ttu-id="ea2b0-428">Sök i loggfilen öppnas och visar hello tillståndet för din behållare, liknande toohello följande.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-428">Log Search opens and displays hello state of your containers, similar toohello following.</span></span>  
   ![behållare tillstånd](./media/log-analytics-containers/containers-log-search.png)
3. <span data-ttu-id="ea2b0-430">Klicka sedan på hello visar det sammanlagda värdet för misslyckade behållare tooview ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-430">Next, click hello aggregated value of failed containers tooview additional information.</span></span> <span data-ttu-id="ea2b0-431">Expandera **visa fler** tooview hello image-ID.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-431">Expand **show more** tooview hello image ID.</span></span>  
   <span data-ttu-id="ea2b0-432">![misslyckade behållare](./media/log-analytics-containers/containers-state-failed.png)</span><span class="sxs-lookup"><span data-stu-id="ea2b0-432">![failed containers](./media/log-analytics-containers/containers-state-failed.png)</span></span>  
4. <span data-ttu-id="ea2b0-433">Skriv sedan följande hello i hello sökfråga.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-433">Next, type hello following in hello search query.</span></span> <span data-ttu-id="ea2b0-434">`Type=ContainerInventory <ImageID>`toosee information om hello avbildning, till exempel avbildningens storlek och antal bilder har stoppats och misslyckade.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-434">`Type=ContainerInventory <ImageID>` toosee details about hello image such as image size and number of stopped and failed images.</span></span>  
   <span data-ttu-id="ea2b0-435">![misslyckade behållare](./media/log-analytics-containers/containers-failed04.png)</span><span class="sxs-lookup"><span data-stu-id="ea2b0-435">![failed containers](./media/log-analytics-containers/containers-failed04.png)</span></span>

## <a name="search-logs-for-container-data"></a><span data-ttu-id="ea2b0-436">Sökloggar för behållardata</span><span class="sxs-lookup"><span data-stu-id="ea2b0-436">Search logs for container data</span></span>
<span data-ttu-id="ea2b0-437">När du felsöker ett visst fel kan det hjälpa toosee där uppstår i din miljö.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-437">When you're troubleshooting a specific error, it can help toosee where it is occurring in your environment.</span></span> <span data-ttu-id="ea2b0-438">hello följande typer av loggen kan du skapa frågor tooreturn hello information som du vill.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-438">hello following log types will help you create queries tooreturn hello information you want.</span></span>


- <span data-ttu-id="ea2b0-439">**ContainerImageInventory** – Använd den här typen när du försöker toofind information som är ordnad efter avbildningen och tooview avbildningsinformationen som bild-ID: N eller storlekar.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-439">**ContainerImageInventory** – Use this type when you're trying toofind information organized by image and tooview image information such as image IDs or sizes.</span></span>
- <span data-ttu-id="ea2b0-440">**ContainerInventory** – Använd den här typen om du vill ha information om behållarens plats deras namn är och vad de kör bilder.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-440">**ContainerInventory** – Use this type when you want information about container location, what their names are, and what images they're running.</span></span>
- <span data-ttu-id="ea2b0-441">**ContainerLog** – Använd den här typen om du vill toofind specifika informationen i felloggen och poster.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-441">**ContainerLog** – Use this type when you want toofind specific error log information and entries.</span></span>
- <span data-ttu-id="ea2b0-442">**ContainerNodeInventory_CL** Använd den här typen om du vill hello information om värd-nod där behållare finns.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-442">**ContainerNodeInventory_CL**  Use this type when you want hello information about host/node where containers are residing.</span></span> <span data-ttu-id="ea2b0-443">Det ger dig Docker-version, orchestration-typ., lagring och nätverksinformation.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-443">It provides you Docker version, orchestration type, storage, and network information.</span></span>
- <span data-ttu-id="ea2b0-444">**ContainerProcess_CL** Använd den här typen tooquickly finns hello process som körs i hello behållaren.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-444">**ContainerProcess_CL** Use this type tooquickly see hello process running within hello container.</span></span>
- <span data-ttu-id="ea2b0-445">**ContainerServiceLog** – Använd den här typen när du försöker toofind information om redovisningsspårning för hello Docker-daemon, till exempel starta, stoppa, ta bort eller pull-kommandon.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-445">**ContainerServiceLog** – Use this type when you're trying toofind audit trail information for hello Docker daemon, such as start, stop, delete, or pull commands.</span></span>
- <span data-ttu-id="ea2b0-446">**KubeEvents_CL** använder den här typen toosee hello Kubernetes händelser.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-446">**KubeEvents_CL**  Use this type toosee hello Kubernetes events.</span></span>
- <span data-ttu-id="ea2b0-447">**KubePodInventory_CL** Använd den här typen om du vill toounderstand hello klusterinformation hierarki.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-447">**KubePodInventory_CL**  Use this type when you want toounderstand hello cluster hierarchy information.</span></span>


### <a name="toosearch-logs-for-container-data"></a><span data-ttu-id="ea2b0-448">toosearch loggar för behållardata</span><span class="sxs-lookup"><span data-stu-id="ea2b0-448">toosearch logs for container data</span></span>
* <span data-ttu-id="ea2b0-449">Välj en bild som du vet misslyckades nyligen och hitta hello felloggar för den.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-449">Choose an image that you know has failed recently and find hello error logs for it.</span></span> <span data-ttu-id="ea2b0-450">Börja med att hitta ett behållarnamn som körs på avbildningen med en **ContainerInventory** sökning.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-450">Start by finding a container name that is running that image with a **ContainerInventory** search.</span></span> <span data-ttu-id="ea2b0-451">Till exempel söka efter`Type=ContainerInventory ubuntu Failed`</span><span class="sxs-lookup"><span data-stu-id="ea2b0-451">For example, search for `Type=ContainerInventory ubuntu Failed`</span></span>  
    <span data-ttu-id="ea2b0-452">![Sök efter Ubuntu behållare](./media/log-analytics-containers/search-ubuntu.png)</span><span class="sxs-lookup"><span data-stu-id="ea2b0-452">![Search for Ubuntu containers](./media/log-analytics-containers/search-ubuntu.png)</span></span>

  <span data-ttu-id="ea2b0-453">Hej namnet på behållaren hello bredvid för**namn**, och Sök efter dessa loggar.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-453">hello name of hello container next too**Name**, and search for those logs.</span></span> <span data-ttu-id="ea2b0-454">I det här exemplet är det `Type=ContainerLog cranky_stonebreaker`.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-454">In this example, it is `Type=ContainerLog cranky_stonebreaker`.</span></span>

<span data-ttu-id="ea2b0-455">**Visa information om prestanda**</span><span class="sxs-lookup"><span data-stu-id="ea2b0-455">**View performance information**</span></span>

<span data-ttu-id="ea2b0-456">När du från tooconstruct frågor kan hjälpa det toosee vad du kan göra först.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-456">When you're beginning tooconstruct queries, it can help toosee what's possible first.</span></span> <span data-ttu-id="ea2b0-457">Till exempel toosee Sök alla prestandadata, försök en bred fråga genom att skriva hello följande fråga.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-457">For example, toosee all performance data, try a broad query by typing hello following search query.</span></span>

```
Type=Perf
```

![behållare prestanda](./media/log-analytics-containers/containers-perf01.png)

<span data-ttu-id="ea2b0-459">Du kan definiera hello prestandadata det uppstår tooa specifik behållare genom att skriva hello namnet på den toohello höger i frågan.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-459">You can scope hello performance data you're seeing tooa specific container by typing hello name of it toohello right of your query.</span></span>

```
Type=Perf <containerName>
```

<span data-ttu-id="ea2b0-460">Som visar hello lista över prestandamått som samlas in för en enskild behållare.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-460">That shows hello list of performance metrics that are collected for an individual container.</span></span>

![behållare prestanda](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a><span data-ttu-id="ea2b0-462">Exempel loggen sökfrågor</span><span class="sxs-lookup"><span data-stu-id="ea2b0-462">Example log search queries</span></span>
<span data-ttu-id="ea2b0-463">Ofta användbara toobuild frågar börjar med ett exempel eller två och sedan ändra dem toofit din miljö.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-463">It's often useful toobuild queries starting with an example or two and then modifying them toofit your environment.</span></span> <span data-ttu-id="ea2b0-464">Som en startpunkt som du kan experimentera med hello **exempelfrågor** område toohelp du skapa mer avancerade frågor.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-464">As a starting point, you can experiment with hello **Sample Queries** area toohelp you build more advanced queries.</span></span>

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Behållare frågor](./media/log-analytics-containers/containers-queries.png)


## <a name="saving-log-search-queries"></a><span data-ttu-id="ea2b0-466">Spara loggfilen sökresultat</span><span class="sxs-lookup"><span data-stu-id="ea2b0-466">Saving log search queries</span></span>
<span data-ttu-id="ea2b0-467">Spara frågor är en funktion som standard i logganalys.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-467">Saving queries is a standard feature in Log Analytics.</span></span> <span data-ttu-id="ea2b0-468">Genom att spara dem, har du de som du har hittat användbar praktiska för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-468">By saving them, you'll have those that you've found useful handy for future use.</span></span>

<span data-ttu-id="ea2b0-469">När du skapar en fråga som vara användbara sparar du genom att klicka på **Favoriter** hello överst i hello loggen söksidan.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-469">After you create a query that you find useful, save it by clicking **Favorites** at hello top of hello Log Search page.</span></span> <span data-ttu-id="ea2b0-470">Du kan enkelt använda det senare från hello **min instrumentpanel** sidan.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-470">Then you can easily access it later from hello **My Dashboard** page.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ea2b0-471">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ea2b0-471">Next steps</span></span>
* <span data-ttu-id="ea2b0-472">[Söka i loggar](log-analytics-log-searches.md) tooview detaljerad dataposter för behållaren.</span><span class="sxs-lookup"><span data-stu-id="ea2b0-472">[Search logs](log-analytics-log-searches.md) tooview detailed container data records.</span></span>
