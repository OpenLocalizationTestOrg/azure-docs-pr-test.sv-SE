---
title: aaaRemote skrivbord tooa Linux VM | Microsoft Docs
description: "Lär dig hur tooinstall och konfigurera fjärrskrivbord tooconnect tooa Microsoft Azure Linux VM för hello klassiska distributionsmodellen"
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 34348659-ddb7-41da-82d6-b5885859e7e4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: mingzhan
ms.openlocfilehash: aadd6e87883cf9cacf9d198b680669d594206e61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-remote-desktop-tooconnect-tooa-microsoft-azure-linux-vm"></a><span data-ttu-id="05300-103">Med hjälp av fjärrskrivbord tooconnect tooa Microsoft Azure Linux VM</span><span class="sxs-lookup"><span data-stu-id="05300-103">Using Remote Desktop tooconnect tooa Microsoft Azure Linux VM</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="05300-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="05300-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="05300-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="05300-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="05300-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="05300-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="05300-107">Hello uppdaterades Resource Manager-versionen av den här artikeln finns [här](../use-remote-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="05300-107">For hello updated Resource Manager version of this article, see [here](../use-remote-desktop.md).</span></span>

## <a name="overview"></a><span data-ttu-id="05300-108">Översikt</span><span class="sxs-lookup"><span data-stu-id="05300-108">Overview</span></span>
<span data-ttu-id="05300-109">RDP (Remote Desktop Protocol) är ett protokoll för Windows.</span><span class="sxs-lookup"><span data-stu-id="05300-109">RDP (Remote Desktop Protocol) is a proprietary protocol used for Windows.</span></span> <span data-ttu-id="05300-110">Hur kan vi för att använda RDP tooconnect tooa Linux VM (virtuell dator) från en annan dator?</span><span class="sxs-lookup"><span data-stu-id="05300-110">How can we use RDP tooconnect tooa Linux VM (virtual machine) remotely?</span></span>

<span data-ttu-id="05300-111">Den här vägledningen ger hello svaret!</span><span class="sxs-lookup"><span data-stu-id="05300-111">This guidance will give you hello answer!</span></span> <span data-ttu-id="05300-112">Det hjälper dig tooinstall och config xrdp på din Microsoft Azure Linux VM, som kan du ansluta tooit med fjärrskrivbord från en Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="05300-112">It will help you tooinstall and config xrdp on your Microsoft Azure Linux VM, which lets you connect tooit with Remote Desktop from a Windows machine.</span></span> <span data-ttu-id="05300-113">Vi använder Linux VM kör Ubuntu eller OpenSUSE hello exempel i den här vägledningen.</span><span class="sxs-lookup"><span data-stu-id="05300-113">We will use Linux VM running Ubuntu or OpenSUSE as hello example in this guidance.</span></span>

<span data-ttu-id="05300-114">Hej xrdp verktyget är en öppen källa RDP-server som du kan använda tooconnect Linux-server med fjärrskrivbord från en Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="05300-114">hello xrdp tool is an open source RDP server that allows you tooconnect your Linux server with Remote Desktop from a Windows machine.</span></span> <span data-ttu-id="05300-115">RDP har bättre prestanda än VNC (virtuella nätverk datorer).</span><span class="sxs-lookup"><span data-stu-id="05300-115">RDP has better performance than VNC (Virtual Network Computing).</span></span> <span data-ttu-id="05300-116">VNC återger använder JPEG-kvalitet grafik och kan vara långsam, RDP är snabb och crystal Rensa.</span><span class="sxs-lookup"><span data-stu-id="05300-116">VNC renders using JPEG-quality graphics and can be slow, whereas RDP is fast and crystal clear.</span></span>

> [!NOTE]
> <span data-ttu-id="05300-117">Du måste redan ha en Microsoft Azure-dator som kör Linux.</span><span class="sxs-lookup"><span data-stu-id="05300-117">You must already have an Microsoft Azure VM running Linux.</span></span> <span data-ttu-id="05300-118">toocreate och skapa en Linux VM finns hello [virtuella Azure Linux-datorn kursen](createportal.md).</span><span class="sxs-lookup"><span data-stu-id="05300-118">toocreate and set up a Linux VM, see hello [Azure Linux VM tutorial](createportal.md).</span></span>
> 
> 

## <a name="create-an-endpoint-for-remote-desktop"></a><span data-ttu-id="05300-119">Skapa en slutpunkt för fjärrskrivbord</span><span class="sxs-lookup"><span data-stu-id="05300-119">Create an endpoint for Remote Desktop</span></span>
<span data-ttu-id="05300-120">Vi använder hello standardslutpunkten 3389 för fjärrskrivbord i det här dokumentet. Ställ in 3389 slutpunkt som `Remote Desktop` tooyour Linux VM som nedan:</span><span class="sxs-lookup"><span data-stu-id="05300-120">We will use hello default endpoint 3389 for Remote Desktop in this doc. Set up 3389 endpoint as `Remote Desktop` tooyour Linux VM like below:</span></span>

![Bild](./media/remote-desktop/endpoint-for-linux-server.png)

<span data-ttu-id="05300-122">Om du inte vet hur tooset in en slutpunkt för den virtuella datorn, se [vägledningen](setup-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="05300-122">If you don't know how tooset up an endpoint for your VM, see [this guidance](setup-endpoints.md).</span></span>

## <a name="install-gnome-desktop"></a><span data-ttu-id="05300-123">Installera gör väldigt lätt Desktop</span><span class="sxs-lookup"><span data-stu-id="05300-123">Install Gnome Desktop</span></span>
<span data-ttu-id="05300-124">Ansluta tooyour Linux VM via `putty`, och installera `Gnome Desktop`.</span><span class="sxs-lookup"><span data-stu-id="05300-124">Connect tooyour Linux VM through `putty`, and install `Gnome Desktop`.</span></span>

<span data-ttu-id="05300-125">Ubuntu, Använd:</span><span class="sxs-lookup"><span data-stu-id="05300-125">For Ubuntu, use:</span></span>

    #sudo apt-get update
    #sudo apt-get install ubuntu-desktop


<span data-ttu-id="05300-126">För OpenSUSE, använder du:</span><span class="sxs-lookup"><span data-stu-id="05300-126">For OpenSUSE, use:</span></span>

    #sudo zypper install gnome-session

## <a name="install-xrdp"></a><span data-ttu-id="05300-127">Installera xrdp</span><span class="sxs-lookup"><span data-stu-id="05300-127">Install xrdp</span></span>
<span data-ttu-id="05300-128">Ubuntu, Använd:</span><span class="sxs-lookup"><span data-stu-id="05300-128">For Ubuntu, use:</span></span>

    #sudo apt-get install xrdp

<span data-ttu-id="05300-129">För OpenSUSE, använder du:</span><span class="sxs-lookup"><span data-stu-id="05300-129">For OpenSUSE, use:</span></span>

> [!NOTE]
> <span data-ttu-id="05300-130">Uppdatera hello OpenSUSE version med hello-version som du använder i hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="05300-130">Update hello OpenSUSE version with hello version you are using in hello following command.</span></span> <span data-ttu-id="05300-131">hello exemplet nedan används för `OpenSUSE 13.2`.</span><span class="sxs-lookup"><span data-stu-id="05300-131">hello example below is for `OpenSUSE 13.2`.</span></span>
> 
> 

    #sudo zypper in http://download.opensuse.org/repositories/X11:/RemoteDesktop/openSUSE_13.2/x86_64/xrdp-0.9.0git.1401423964-2.1.x86_64.rpm
    #sudo zypper install tigervnc xorg-x11-Xvnc xterm remmina-plugin-vnc


## <a name="start-xrdp-and-set-xdrp-service-at-boot-up"></a><span data-ttu-id="05300-132">Starta xrdp och Ställ in xdrp tjänsten vid uppstart</span><span class="sxs-lookup"><span data-stu-id="05300-132">Start xrdp and set xdrp service at boot-up</span></span>
<span data-ttu-id="05300-133">För OpenSUSE, använder du:</span><span class="sxs-lookup"><span data-stu-id="05300-133">For OpenSUSE, use:</span></span>

    #sudo systemctl start xrdp
    #sudo systemctl enable xrdp

<span data-ttu-id="05300-134">För Ubuntu, xrdp startas och eanbled vid uppstart automatiskt efter installationen.</span><span class="sxs-lookup"><span data-stu-id="05300-134">For Ubuntu, xrdp will be started and eanbled at boot-up automatically after installation.</span></span>

## <a name="using-xfce-if-you-are-using-an-ubuntu-version-later-than-ubuntu-1204lts"></a><span data-ttu-id="05300-135">Med hjälp av xfce om du använder en Ubuntu version senare än Ubuntu 12.04LTS</span><span class="sxs-lookup"><span data-stu-id="05300-135">Using xfce if you are using an Ubuntu version later than Ubuntu 12.04LTS</span></span>
<span data-ttu-id="05300-136">Eftersom hello aktuella versionen av xrdp inte stöder gör väldigt lätt skrivbordet för Ubuntu versioner senare än Ubuntu 12.04LTS, kommer vi att använda `xfce` skrivbordet i stället.</span><span class="sxs-lookup"><span data-stu-id="05300-136">Because hello current version of xrdp does not support Gnome Desktop for  Ubuntu versions later than Ubuntu 12.04LTS, we will use `xfce` Desktop instead.</span></span>

<span data-ttu-id="05300-137">tooinstall `xfce`, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="05300-137">tooinstall `xfce`, use this command:</span></span>

    #sudo apt-get install xubuntu-desktop

<span data-ttu-id="05300-138">Aktivera sedan `xfce` med det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="05300-138">Then enable `xfce` using this command:</span></span>

    #echo xfce4-session >~/.xsession

<span data-ttu-id="05300-139">Redigera konfigurationsfilen för hello `/etc/xrdp/startwm.sh`:</span><span class="sxs-lookup"><span data-stu-id="05300-139">Edit hello config file `/etc/xrdp/startwm.sh`:</span></span>

    #sudo vi /etc/xrdp/startwm.sh   

<span data-ttu-id="05300-140">Lägg till rad hello `xfce4-session` före hello raden `/etc/X11/Xsession`.</span><span class="sxs-lookup"><span data-stu-id="05300-140">Add hello line `xfce4-session` before hello line `/etc/X11/Xsession`.</span></span>

<span data-ttu-id="05300-141">toorestart Hej xrdp service, Använd den här:</span><span class="sxs-lookup"><span data-stu-id="05300-141">toorestart hello xrdp service, use this:</span></span>

    #sudo service xrdp restart


## <a name="connect-your-linux-vm-from-a-windows-machine"></a><span data-ttu-id="05300-142">Ansluta din Linux-VM från en Windows-dator</span><span class="sxs-lookup"><span data-stu-id="05300-142">Connect your Linux VM from a Windows machine</span></span>
<span data-ttu-id="05300-143">Starta hello fjärrskrivbordsklienten på en Windows-dator och ange Linux VM DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="05300-143">In a Windows machine, start hello Remote Desktop client and input your Linux VM DNS name.</span></span> <span data-ttu-id="05300-144">Eller gå toohello instrumentpanelen på den virtuella datorn i hello Azure-portalen och klicka på `Connect` tooconnect din Linux VM.</span><span class="sxs-lookup"><span data-stu-id="05300-144">Or go toohello Dashboard of your VM in hello Azure portal and click `Connect` tooconnect your Linux VM.</span></span> <span data-ttu-id="05300-145">I så fall kan se du hello inloggningsfönstret:</span><span class="sxs-lookup"><span data-stu-id="05300-145">In that case, you see hello login window:</span></span>

![Bild](./media/remote-desktop/no2.png)

<span data-ttu-id="05300-147">Logga in med hello användarnamn och lösenord för Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="05300-147">Log in with hello user name and password of your Linux VM.</span></span>

## <a name="next-steps"></a><span data-ttu-id="05300-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="05300-148">Next steps</span></span>
<span data-ttu-id="05300-149">Läs mer om hur du använder xrdp [http://www.xrdp.org/](http://www.xrdp.org/).</span><span class="sxs-lookup"><span data-stu-id="05300-149">For more information about using xrdp, see [http://www.xrdp.org/](http://www.xrdp.org/).</span></span>
