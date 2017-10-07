---
title: "aaaSelecting användarnamn för Linux | Microsoft Docs"
description: "Lär dig hur tooselect användaren namn för en virtuell Linux-dator i Azure."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 33b50c97-92f1-46c9-a623-e37f67459c5c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: c65e2ac46f40bb8c9d74cccbaf248a070c0fa6cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="selecting-user-names-for-linux-on-azure"></a><span data-ttu-id="ec498-103">Välj användarnamn för Linux på Azure</span><span class="sxs-lookup"><span data-stu-id="ec498-103">Selecting User Names for Linux on Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="ec498-104">När du etablerar en Linux-dator på Azure måste du ange hello namnet på en icke-rotanvändare som du senare kan använda toolog i hello VM.</span><span class="sxs-lookup"><span data-stu-id="ec498-104">When you provision a Linux virtual machine on Azure you must specify hello name of a non-root user that you can later use toolog into hello VM.</span></span> <span data-ttu-id="ec498-105">Du kan välja hello namn för den nya användaren i hello eller om etablering hello klassiska Azure-portalen kan accepteras hello standard namnet ”azureuser”.</span><span class="sxs-lookup"><span data-stu-id="ec498-105">You may choose hello name of hello new user, or if provisioning via hello Azure classic portal you can accept hello default name "azureuser".</span></span>

<span data-ttu-id="ec498-106">I de flesta fall den här användaren finns inte på hello basavbildning och kommer att skapas under hello etableringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="ec498-106">In most cases this user won't exist on hello base image and will be created during hello provisioning process.</span></span> <span data-ttu-id="ec498-107">Om hello användaren finns på hello VM basavbildning, konfigurerar hello Azure Linux-agenten bara hello lösenord och/eller SSH-nyckeln för den användare som är baserade på hello information som du angav när du skapar hello VM.</span><span class="sxs-lookup"><span data-stu-id="ec498-107">If hello user exists on hello base VM image, then hello Azure Linux agent simply configures hello password and/or SSH key for that user based on hello information you specified when creating hello VM.</span></span>

<span data-ttu-id="ec498-108">**Men**, Linux definierar en uppsättning användarnamn som inte ska användas.</span><span class="sxs-lookup"><span data-stu-id="ec498-108">**However**, Linux defines a set of user names that should not be used.</span></span> <span data-ttu-id="ec498-109">hello etablering processen kommer **misslyckas** om du försöker tooprovision en Linux VM som använder en befintlig systemanvändare definieras som en användare med UID 0-99.</span><span class="sxs-lookup"><span data-stu-id="ec498-109">hello provisioning process will **fail** if you try tooprovision a Linux VM using an existing system user, which is defined as a user with UID 0-99.</span></span> <span data-ttu-id="ec498-110">Ett typiskt exempel är hello `root` användare som har UID 0.</span><span class="sxs-lookup"><span data-stu-id="ec498-110">A typical example is hello `root` user, which has UID 0.</span></span>

* <span data-ttu-id="ec498-111">Se även: [Linux Standard Base - användar-ID-intervall](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)</span><span class="sxs-lookup"><span data-stu-id="ec498-111">See also: [Linux Standard Base - User ID Ranges](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)</span></span>

<span data-ttu-id="ec498-112">hello följer en lista över vanliga inbyggda systemanvändare för CentOS och Ubuntu att bör du undvika att använda vid etablering av en virtuell Linux-dator på Azure.</span><span class="sxs-lookup"><span data-stu-id="ec498-112">hello following is a list of common built-in system users for CentOS and Ubuntu that you should avoid using when provisioning a Linux virtual machine on Azure.</span></span> <span data-ttu-id="ec498-113">Den här listan är bara ett exempel, se toohello dokumentationen för din distribution tooensure som hello användarnamn som du väljer inte står i konflikt med en befintlig systemanvändare.</span><span class="sxs-lookup"><span data-stu-id="ec498-113">This list is just an example, please refer toohello documentation for your distribution tooensure that hello username you choose does not conflict with an existing system user.</span></span>

## <a name="centos"></a><span data-ttu-id="ec498-114">CentOS</span><span class="sxs-lookup"><span data-stu-id="ec498-114">CentOS</span></span>
* <span data-ttu-id="ec498-115">abrt</span><span class="sxs-lookup"><span data-stu-id="ec498-115">abrt</span></span>
* <span data-ttu-id="ec498-116">ADM</span><span class="sxs-lookup"><span data-stu-id="ec498-116">adm</span></span>
* <span data-ttu-id="ec498-117">ljud</span><span class="sxs-lookup"><span data-stu-id="ec498-117">audio</span></span>
* <span data-ttu-id="ec498-118">bin</span><span class="sxs-lookup"><span data-stu-id="ec498-118">bin</span></span>
* <span data-ttu-id="ec498-119">CDROM</span><span class="sxs-lookup"><span data-stu-id="ec498-119">cdrom</span></span>
* <span data-ttu-id="ec498-120">cgred</span><span class="sxs-lookup"><span data-stu-id="ec498-120">cgred</span></span>
* <span data-ttu-id="ec498-121">Daemon</span><span class="sxs-lookup"><span data-stu-id="ec498-121">daemon</span></span>
* <span data-ttu-id="ec498-122">dbus</span><span class="sxs-lookup"><span data-stu-id="ec498-122">dbus</span></span>
* <span data-ttu-id="ec498-123">antingen</span><span class="sxs-lookup"><span data-stu-id="ec498-123">dialout</span></span>
* <span data-ttu-id="ec498-124">DIP</span><span class="sxs-lookup"><span data-stu-id="ec498-124">dip</span></span>
* <span data-ttu-id="ec498-125">disk</span><span class="sxs-lookup"><span data-stu-id="ec498-125">disk</span></span>
* <span data-ttu-id="ec498-126">diskettenheter</span><span class="sxs-lookup"><span data-stu-id="ec498-126">floppy</span></span>
* <span data-ttu-id="ec498-127">FTP</span><span class="sxs-lookup"><span data-stu-id="ec498-127">ftp</span></span>
* <span data-ttu-id="ec498-128">FTP</span><span class="sxs-lookup"><span data-stu-id="ec498-128">ftp</span></span>
* <span data-ttu-id="ec498-129">spel</span><span class="sxs-lookup"><span data-stu-id="ec498-129">games</span></span>
* <span data-ttu-id="ec498-130">Gopher</span><span class="sxs-lookup"><span data-stu-id="ec498-130">gopher</span></span>
* <span data-ttu-id="ec498-131">haldaemon</span><span class="sxs-lookup"><span data-stu-id="ec498-131">haldaemon</span></span>
* <span data-ttu-id="ec498-132">Stoppa</span><span class="sxs-lookup"><span data-stu-id="ec498-132">halt</span></span>
* <span data-ttu-id="ec498-133">kmem</span><span class="sxs-lookup"><span data-stu-id="ec498-133">kmem</span></span>
* <span data-ttu-id="ec498-134">Lås</span><span class="sxs-lookup"><span data-stu-id="ec498-134">lock</span></span>
* <span data-ttu-id="ec498-135">LP</span><span class="sxs-lookup"><span data-stu-id="ec498-135">lp</span></span>
* <span data-ttu-id="ec498-136">E-post</span><span class="sxs-lookup"><span data-stu-id="ec498-136">mail</span></span>
* <span data-ttu-id="ec498-137">man</span><span class="sxs-lookup"><span data-stu-id="ec498-137">man</span></span>
* <span data-ttu-id="ec498-138">med</span><span class="sxs-lookup"><span data-stu-id="ec498-138">mem</span></span>
* <span data-ttu-id="ec498-139">nfsnobody</span><span class="sxs-lookup"><span data-stu-id="ec498-139">nfsnobody</span></span>
* <span data-ttu-id="ec498-140">Ingen</span><span class="sxs-lookup"><span data-stu-id="ec498-140">nobody</span></span>
* <span data-ttu-id="ec498-141">NTP</span><span class="sxs-lookup"><span data-stu-id="ec498-141">ntp</span></span>
* <span data-ttu-id="ec498-142">Operatorn</span><span class="sxs-lookup"><span data-stu-id="ec498-142">operator</span></span>
* <span data-ttu-id="ec498-143">oprofile</span><span class="sxs-lookup"><span data-stu-id="ec498-143">oprofile</span></span>
* <span data-ttu-id="ec498-144">postdrop</span><span class="sxs-lookup"><span data-stu-id="ec498-144">postdrop</span></span>
* <span data-ttu-id="ec498-145">UserName@Domain</span><span class="sxs-lookup"><span data-stu-id="ec498-145">postfix</span></span>
* <span data-ttu-id="ec498-146">qpidd</span><span class="sxs-lookup"><span data-stu-id="ec498-146">qpidd</span></span>
* <span data-ttu-id="ec498-147">rot</span><span class="sxs-lookup"><span data-stu-id="ec498-147">root</span></span>
* <span data-ttu-id="ec498-148">RPC</span><span class="sxs-lookup"><span data-stu-id="ec498-148">rpc</span></span>
* <span data-ttu-id="ec498-149">rpcuser</span><span class="sxs-lookup"><span data-stu-id="ec498-149">rpcuser</span></span>
* <span data-ttu-id="ec498-150">saslauth</span><span class="sxs-lookup"><span data-stu-id="ec498-150">saslauth</span></span>
* <span data-ttu-id="ec498-151">avstängning</span><span class="sxs-lookup"><span data-stu-id="ec498-151">shutdown</span></span>
* <span data-ttu-id="ec498-152">slocate</span><span class="sxs-lookup"><span data-stu-id="ec498-152">slocate</span></span>
* <span data-ttu-id="ec498-153">sshd</span><span class="sxs-lookup"><span data-stu-id="ec498-153">sshd</span></span>
* <span data-ttu-id="ec498-154">stapdev</span><span class="sxs-lookup"><span data-stu-id="ec498-154">stapdev</span></span>
* <span data-ttu-id="ec498-155">stapusr</span><span class="sxs-lookup"><span data-stu-id="ec498-155">stapusr</span></span>
* <span data-ttu-id="ec498-156">Synkronisering</span><span class="sxs-lookup"><span data-stu-id="ec498-156">sync</span></span>
* <span data-ttu-id="ec498-157">sys</span><span class="sxs-lookup"><span data-stu-id="ec498-157">sys</span></span>
* <span data-ttu-id="ec498-158">band</span><span class="sxs-lookup"><span data-stu-id="ec498-158">tape</span></span>
* <span data-ttu-id="ec498-159">Test</span><span class="sxs-lookup"><span data-stu-id="ec498-159">test</span></span>
* <span data-ttu-id="ec498-160">tcpdump</span><span class="sxs-lookup"><span data-stu-id="ec498-160">tcpdump</span></span>
* <span data-ttu-id="ec498-161">TTY</span><span class="sxs-lookup"><span data-stu-id="ec498-161">tty</span></span>
* <span data-ttu-id="ec498-162">användare</span><span class="sxs-lookup"><span data-stu-id="ec498-162">users</span></span>
* <span data-ttu-id="ec498-163">utempter</span><span class="sxs-lookup"><span data-stu-id="ec498-163">utempter</span></span>
* <span data-ttu-id="ec498-164">utmp</span><span class="sxs-lookup"><span data-stu-id="ec498-164">utmp</span></span>
* <span data-ttu-id="ec498-165">UUCP</span><span class="sxs-lookup"><span data-stu-id="ec498-165">uucp</span></span>
* <span data-ttu-id="ec498-166">vcsa</span><span class="sxs-lookup"><span data-stu-id="ec498-166">vcsa</span></span>
* <span data-ttu-id="ec498-167">video</span><span class="sxs-lookup"><span data-stu-id="ec498-167">video</span></span>
* <span data-ttu-id="ec498-168">hjul</span><span class="sxs-lookup"><span data-stu-id="ec498-168">wheel</span></span>

## <a name="ubuntu"></a><span data-ttu-id="ec498-169">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="ec498-169">Ubuntu</span></span>
* <span data-ttu-id="ec498-170">ADM</span><span class="sxs-lookup"><span data-stu-id="ec498-170">adm</span></span>
* <span data-ttu-id="ec498-171">Admin</span><span class="sxs-lookup"><span data-stu-id="ec498-171">admin</span></span>
* <span data-ttu-id="ec498-172">ljud</span><span class="sxs-lookup"><span data-stu-id="ec498-172">audio</span></span>
* <span data-ttu-id="ec498-173">säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="ec498-173">backup</span></span>
* <span data-ttu-id="ec498-174">bin</span><span class="sxs-lookup"><span data-stu-id="ec498-174">bin</span></span>
* <span data-ttu-id="ec498-175">CDROM</span><span class="sxs-lookup"><span data-stu-id="ec498-175">cdrom</span></span>
* <span data-ttu-id="ec498-176">crontab</span><span class="sxs-lookup"><span data-stu-id="ec498-176">crontab</span></span>
* <span data-ttu-id="ec498-177">Daemon</span><span class="sxs-lookup"><span data-stu-id="ec498-177">daemon</span></span>
* <span data-ttu-id="ec498-178">antingen</span><span class="sxs-lookup"><span data-stu-id="ec498-178">dialout</span></span>
* <span data-ttu-id="ec498-179">DIP</span><span class="sxs-lookup"><span data-stu-id="ec498-179">dip</span></span>
* <span data-ttu-id="ec498-180">disk</span><span class="sxs-lookup"><span data-stu-id="ec498-180">disk</span></span>
* <span data-ttu-id="ec498-181">Fax</span><span class="sxs-lookup"><span data-stu-id="ec498-181">fax</span></span>
* <span data-ttu-id="ec498-182">diskettenheter</span><span class="sxs-lookup"><span data-stu-id="ec498-182">floppy</span></span>
* <span data-ttu-id="ec498-183">säkrad</span><span class="sxs-lookup"><span data-stu-id="ec498-183">fuse</span></span>
* <span data-ttu-id="ec498-184">spel</span><span class="sxs-lookup"><span data-stu-id="ec498-184">games</span></span>
* <span data-ttu-id="ec498-185">gnats</span><span class="sxs-lookup"><span data-stu-id="ec498-185">gnats</span></span>
* <span data-ttu-id="ec498-186">IRC</span><span class="sxs-lookup"><span data-stu-id="ec498-186">irc</span></span>
* <span data-ttu-id="ec498-187">kmem</span><span class="sxs-lookup"><span data-stu-id="ec498-187">kmem</span></span>
* <span data-ttu-id="ec498-188">liggande</span><span class="sxs-lookup"><span data-stu-id="ec498-188">landscape</span></span>
* <span data-ttu-id="ec498-189">libuuid</span><span class="sxs-lookup"><span data-stu-id="ec498-189">libuuid</span></span>
* <span data-ttu-id="ec498-190">lista</span><span class="sxs-lookup"><span data-stu-id="ec498-190">list</span></span>
* <span data-ttu-id="ec498-191">LP</span><span class="sxs-lookup"><span data-stu-id="ec498-191">lp</span></span>
* <span data-ttu-id="ec498-192">E-post</span><span class="sxs-lookup"><span data-stu-id="ec498-192">mail</span></span>
* <span data-ttu-id="ec498-193">man</span><span class="sxs-lookup"><span data-stu-id="ec498-193">man</span></span>
* <span data-ttu-id="ec498-194">MessageBus</span><span class="sxs-lookup"><span data-stu-id="ec498-194">messagebus</span></span>
* <span data-ttu-id="ec498-195">mlocate</span><span class="sxs-lookup"><span data-stu-id="ec498-195">mlocate</span></span>
* <span data-ttu-id="ec498-196">netdev</span><span class="sxs-lookup"><span data-stu-id="ec498-196">netdev</span></span>
* <span data-ttu-id="ec498-197">Nyheter</span><span class="sxs-lookup"><span data-stu-id="ec498-197">news</span></span>
* <span data-ttu-id="ec498-198">Ingen</span><span class="sxs-lookup"><span data-stu-id="ec498-198">nobody</span></span>
* <span data-ttu-id="ec498-199">nogroup</span><span class="sxs-lookup"><span data-stu-id="ec498-199">nogroup</span></span>
* <span data-ttu-id="ec498-200">Operatorn</span><span class="sxs-lookup"><span data-stu-id="ec498-200">operator</span></span>
* <span data-ttu-id="ec498-201">plugdev</span><span class="sxs-lookup"><span data-stu-id="ec498-201">plugdev</span></span>
* <span data-ttu-id="ec498-202">Proxy</span><span class="sxs-lookup"><span data-stu-id="ec498-202">proxy</span></span>
* <span data-ttu-id="ec498-203">rot</span><span class="sxs-lookup"><span data-stu-id="ec498-203">root</span></span>
* <span data-ttu-id="ec498-204">SASL</span><span class="sxs-lookup"><span data-stu-id="ec498-204">sasl</span></span>
* <span data-ttu-id="ec498-205">skugga</span><span class="sxs-lookup"><span data-stu-id="ec498-205">shadow</span></span>
* <span data-ttu-id="ec498-206">src</span><span class="sxs-lookup"><span data-stu-id="ec498-206">src</span></span>
* <span data-ttu-id="ec498-207">SSH</span><span class="sxs-lookup"><span data-stu-id="ec498-207">ssh</span></span>
* <span data-ttu-id="ec498-208">sshd</span><span class="sxs-lookup"><span data-stu-id="ec498-208">sshd</span></span>
* <span data-ttu-id="ec498-209">Personal</span><span class="sxs-lookup"><span data-stu-id="ec498-209">staff</span></span>
* <span data-ttu-id="ec498-210">sudo</span><span class="sxs-lookup"><span data-stu-id="ec498-210">sudo</span></span>
* <span data-ttu-id="ec498-211">Synkronisering</span><span class="sxs-lookup"><span data-stu-id="ec498-211">sync</span></span>
* <span data-ttu-id="ec498-212">sys</span><span class="sxs-lookup"><span data-stu-id="ec498-212">sys</span></span>
* <span data-ttu-id="ec498-213">syslog</span><span class="sxs-lookup"><span data-stu-id="ec498-213">syslog</span></span>
* <span data-ttu-id="ec498-214">band</span><span class="sxs-lookup"><span data-stu-id="ec498-214">tape</span></span>
* <span data-ttu-id="ec498-215">TTY</span><span class="sxs-lookup"><span data-stu-id="ec498-215">tty</span></span>
* <span data-ttu-id="ec498-216">användare</span><span class="sxs-lookup"><span data-stu-id="ec498-216">users</span></span>
* <span data-ttu-id="ec498-217">utmp</span><span class="sxs-lookup"><span data-stu-id="ec498-217">utmp</span></span>
* <span data-ttu-id="ec498-218">UUCP</span><span class="sxs-lookup"><span data-stu-id="ec498-218">uucp</span></span>
* <span data-ttu-id="ec498-219">video</span><span class="sxs-lookup"><span data-stu-id="ec498-219">video</span></span>
* <span data-ttu-id="ec498-220">röst</span><span class="sxs-lookup"><span data-stu-id="ec498-220">voice</span></span>
* <span data-ttu-id="ec498-221">whoopsie</span><span class="sxs-lookup"><span data-stu-id="ec498-221">whoopsie</span></span>
* <span data-ttu-id="ec498-222">www-data</span><span class="sxs-lookup"><span data-stu-id="ec498-222">www-data</span></span>

