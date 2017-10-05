---
title: "Fjärrskrivbord till en virtuell Linux-dator | Microsoft Docs"
description: "Lär dig hur du installerar och konfigurerar Fjärrskrivbord för att ansluta till en Microsoft Azure Linux-VM för den klassiska distributionsmodellen"
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
ms.openlocfilehash: 68031d548bdbeda9a83d1bceaaea7c5bbcab3188
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-remote-desktop-to-connect-to-a-microsoft-azure-linux-vm"></a><span data-ttu-id="4a188-103">Använd Fjärrskrivbord för att ansluta till en virtuell Microsoft Azure Linux-dator</span><span class="sxs-lookup"><span data-stu-id="4a188-103">Using Remote Desktop to connect to a Microsoft Azure Linux VM</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="4a188-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="4a188-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="4a188-105">Den här artikeln täcker den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="4a188-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="4a188-106">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="4a188-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="4a188-107">Den uppdaterade Resource Manager-versionen av den här artikeln finns [här](../use-remote-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="4a188-107">For the updated Resource Manager version of this article, see [here](../use-remote-desktop.md).</span></span>

## <a name="overview"></a><span data-ttu-id="4a188-108">Översikt</span><span class="sxs-lookup"><span data-stu-id="4a188-108">Overview</span></span>
<span data-ttu-id="4a188-109">RDP (Remote Desktop Protocol) är ett protokoll för Windows.</span><span class="sxs-lookup"><span data-stu-id="4a188-109">RDP (Remote Desktop Protocol) is a proprietary protocol used for Windows.</span></span> <span data-ttu-id="4a188-110">Hur kan vi använda RDP för att fjärransluta till en Linux VM (virtuell dator)</span><span class="sxs-lookup"><span data-stu-id="4a188-110">How can we use RDP to connect to a Linux VM (virtual machine) remotely?</span></span>

<span data-ttu-id="4a188-111">Den här vägledningen ger svaret!</span><span class="sxs-lookup"><span data-stu-id="4a188-111">This guidance will give you the answer!</span></span> <span data-ttu-id="4a188-112">Det hjälper dig att installera och config xrdp på din Microsoft Azure Linux VM, som gör att du kan ansluta till den med fjärrskrivbord från en Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="4a188-112">It will help you to install and config xrdp on your Microsoft Azure Linux VM, which lets you connect to it with Remote Desktop from a Windows machine.</span></span> <span data-ttu-id="4a188-113">Vi använder Linux VM körs Ubuntu eller OpenSUSE som exemplet i den här vägledningen.</span><span class="sxs-lookup"><span data-stu-id="4a188-113">We will use Linux VM running Ubuntu or OpenSUSE as the example in this guidance.</span></span>

<span data-ttu-id="4a188-114">Verktyget xrdp är en öppen källkod RDP-server som kan du ansluta din Linux-server med fjärrskrivbord från en Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="4a188-114">The xrdp tool is an open source RDP server that allows you to connect your Linux server with Remote Desktop from a Windows machine.</span></span> <span data-ttu-id="4a188-115">RDP har bättre prestanda än VNC (virtuella nätverk datorer).</span><span class="sxs-lookup"><span data-stu-id="4a188-115">RDP has better performance than VNC (Virtual Network Computing).</span></span> <span data-ttu-id="4a188-116">VNC återger använder JPEG-kvalitet grafik och kan vara långsam, RDP är snabb och crystal Rensa.</span><span class="sxs-lookup"><span data-stu-id="4a188-116">VNC renders using JPEG-quality graphics and can be slow, whereas RDP is fast and crystal clear.</span></span>

> [!NOTE]
> <span data-ttu-id="4a188-117">Du måste redan ha en Microsoft Azure-dator som kör Linux.</span><span class="sxs-lookup"><span data-stu-id="4a188-117">You must already have an Microsoft Azure VM running Linux.</span></span> <span data-ttu-id="4a188-118">Om du vill skapa och konfigurera en Linux-VM finns i [virtuella Azure Linux-datorn kursen](createportal.md).</span><span class="sxs-lookup"><span data-stu-id="4a188-118">To create and set up a Linux VM, see the [Azure Linux VM tutorial](createportal.md).</span></span>
> 
> 

## <a name="create-an-endpoint-for-remote-desktop"></a><span data-ttu-id="4a188-119">Skapa en slutpunkt för fjärrskrivbord</span><span class="sxs-lookup"><span data-stu-id="4a188-119">Create an endpoint for Remote Desktop</span></span>
<span data-ttu-id="4a188-120">Vi använder standardslutpunkten 3389 för fjärrskrivbord i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="4a188-120">We will use the default endpoint 3389 for Remote Desktop in this doc.</span></span> <span data-ttu-id="4a188-121">Ställ in 3389 slutpunkt som `Remote Desktop` till din Linux VM som nedan:</span><span class="sxs-lookup"><span data-stu-id="4a188-121">Set up 3389 endpoint as `Remote Desktop` to your Linux VM like below:</span></span>

![Bild](./media/remote-desktop/endpoint-for-linux-server.png)

<span data-ttu-id="4a188-123">Om du inte vet hur du konfigurerar en slutpunkt för den virtuella datorn, se [vägledningen](setup-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="4a188-123">If you don't know how to set up an endpoint for your VM, see [this guidance](setup-endpoints.md).</span></span>

## <a name="install-gnome-desktop"></a><span data-ttu-id="4a188-124">Installera gör väldigt lätt Desktop</span><span class="sxs-lookup"><span data-stu-id="4a188-124">Install Gnome Desktop</span></span>
<span data-ttu-id="4a188-125">Ansluta till din Linux VM via `putty`, och installera `Gnome Desktop`.</span><span class="sxs-lookup"><span data-stu-id="4a188-125">Connect to your Linux VM through `putty`, and install `Gnome Desktop`.</span></span>

<span data-ttu-id="4a188-126">Ubuntu, Använd:</span><span class="sxs-lookup"><span data-stu-id="4a188-126">For Ubuntu, use:</span></span>

    #sudo apt-get update
    #sudo apt-get install ubuntu-desktop


<span data-ttu-id="4a188-127">För OpenSUSE, använder du:</span><span class="sxs-lookup"><span data-stu-id="4a188-127">For OpenSUSE, use:</span></span>

    #sudo zypper install gnome-session

## <a name="install-xrdp"></a><span data-ttu-id="4a188-128">Installera xrdp</span><span class="sxs-lookup"><span data-stu-id="4a188-128">Install xrdp</span></span>
<span data-ttu-id="4a188-129">Ubuntu, Använd:</span><span class="sxs-lookup"><span data-stu-id="4a188-129">For Ubuntu, use:</span></span>

    #sudo apt-get install xrdp

<span data-ttu-id="4a188-130">För OpenSUSE, använder du:</span><span class="sxs-lookup"><span data-stu-id="4a188-130">For OpenSUSE, use:</span></span>

> [!NOTE]
> <span data-ttu-id="4a188-131">Uppdatera OpenSUSE versionen med den version som du använder i kommandot.</span><span class="sxs-lookup"><span data-stu-id="4a188-131">Update the OpenSUSE version with the version you are using in the following command.</span></span> <span data-ttu-id="4a188-132">I exemplet nedan används för `OpenSUSE 13.2`.</span><span class="sxs-lookup"><span data-stu-id="4a188-132">The example below is for `OpenSUSE 13.2`.</span></span>
> 
> 

    #sudo zypper in http://download.opensuse.org/repositories/X11:/RemoteDesktop/openSUSE_13.2/x86_64/xrdp-0.9.0git.1401423964-2.1.x86_64.rpm
    #sudo zypper install tigervnc xorg-x11-Xvnc xterm remmina-plugin-vnc


## <a name="start-xrdp-and-set-xdrp-service-at-boot-up"></a><span data-ttu-id="4a188-133">Starta xrdp och Ställ in xdrp tjänsten vid uppstart</span><span class="sxs-lookup"><span data-stu-id="4a188-133">Start xrdp and set xdrp service at boot-up</span></span>
<span data-ttu-id="4a188-134">För OpenSUSE, använder du:</span><span class="sxs-lookup"><span data-stu-id="4a188-134">For OpenSUSE, use:</span></span>

    #sudo systemctl start xrdp
    #sudo systemctl enable xrdp

<span data-ttu-id="4a188-135">För Ubuntu, xrdp startas och eanbled vid uppstart automatiskt efter installationen.</span><span class="sxs-lookup"><span data-stu-id="4a188-135">For Ubuntu, xrdp will be started and eanbled at boot-up automatically after installation.</span></span>

## <a name="using-xfce-if-you-are-using-an-ubuntu-version-later-than-ubuntu-1204lts"></a><span data-ttu-id="4a188-136">Med hjälp av xfce om du använder en Ubuntu version senare än Ubuntu 12.04LTS</span><span class="sxs-lookup"><span data-stu-id="4a188-136">Using xfce if you are using an Ubuntu version later than Ubuntu 12.04LTS</span></span>
<span data-ttu-id="4a188-137">Eftersom den aktuella versionen av xrdp inte stöder gör väldigt lätt skrivbordet för Ubuntu versioner senare än Ubuntu 12.04LTS, kommer vi att använda `xfce` skrivbordet i stället.</span><span class="sxs-lookup"><span data-stu-id="4a188-137">Because the current version of xrdp does not support Gnome Desktop for  Ubuntu versions later than Ubuntu 12.04LTS, we will use `xfce` Desktop instead.</span></span>

<span data-ttu-id="4a188-138">Så här installerar du `xfce`, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4a188-138">To install `xfce`, use this command:</span></span>

    #sudo apt-get install xubuntu-desktop

<span data-ttu-id="4a188-139">Aktivera sedan `xfce` med det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="4a188-139">Then enable `xfce` using this command:</span></span>

    #echo xfce4-session >~/.xsession

<span data-ttu-id="4a188-140">Redigera konfigurationsfilen `/etc/xrdp/startwm.sh`:</span><span class="sxs-lookup"><span data-stu-id="4a188-140">Edit the config file `/etc/xrdp/startwm.sh`:</span></span>

    #sudo vi /etc/xrdp/startwm.sh   

<span data-ttu-id="4a188-141">Lägg till rad `xfce4-session` före raden `/etc/X11/Xsession`.</span><span class="sxs-lookup"><span data-stu-id="4a188-141">Add the line `xfce4-session` before the line `/etc/X11/Xsession`.</span></span>

<span data-ttu-id="4a188-142">Använd detta för att starta om tjänsten xrdp:</span><span class="sxs-lookup"><span data-stu-id="4a188-142">To restart the xrdp service, use this:</span></span>

    #sudo service xrdp restart


## <a name="connect-your-linux-vm-from-a-windows-machine"></a><span data-ttu-id="4a188-143">Ansluta din Linux-VM från en Windows-dator</span><span class="sxs-lookup"><span data-stu-id="4a188-143">Connect your Linux VM from a Windows machine</span></span>
<span data-ttu-id="4a188-144">Starta fjärrskrivbordsklienten på en Windows-dator och ange Linux VM DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="4a188-144">In a Windows machine, start the Remote Desktop client and input your Linux VM DNS name.</span></span> <span data-ttu-id="4a188-145">Eller gå till instrumentpanelen på den virtuella datorn i Azure-portalen och klicka på `Connect` att ansluta din Linux VM.</span><span class="sxs-lookup"><span data-stu-id="4a188-145">Or go to the Dashboard of your VM in the Azure portal and click `Connect` to connect your Linux VM.</span></span> <span data-ttu-id="4a188-146">I så fall visas inloggningsfönstret:</span><span class="sxs-lookup"><span data-stu-id="4a188-146">In that case, you see the login window:</span></span>

![Bild](./media/remote-desktop/no2.png)

<span data-ttu-id="4a188-148">Logga in med användarnamn och lösenord för Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="4a188-148">Log in with the user name and password of your Linux VM.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4a188-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4a188-149">Next steps</span></span>
<span data-ttu-id="4a188-150">Läs mer om hur du använder xrdp [http://www.xrdp.org/](http://www.xrdp.org/).</span><span class="sxs-lookup"><span data-stu-id="4a188-150">For more information about using xrdp, see [http://www.xrdp.org/](http://www.xrdp.org/).</span></span>
