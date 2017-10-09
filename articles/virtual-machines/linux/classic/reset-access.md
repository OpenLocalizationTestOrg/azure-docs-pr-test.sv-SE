---
title: "aaaReset Linux VM lösenord och SSH-nyckeln från hello CLI | Microsoft Docs"
description: "Hur toouse hello VMAccess-tillägget från hello Azure-kommandoradsgränssnittet (CLI) tooreset Linux VM lösenord eller SSH-nyckeln åtgärda hello SSH-konfigurationen och kontrollera disk konsekvenskontroll"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: d975eb70-5ff1-40d1-a634-8dd2646dcd17
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: cynthn
ms.openlocfilehash: 1650ad64fb982627ae9f90b1a8209bb56bac7004
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-a-linux-vm-password-or-ssh-key-fix-hello-ssh-configuration-and-check-disk-consistency-using-hello-vmaccess-extension"></a><span data-ttu-id="89c98-103">Hur tooreset Linux VM lösenord eller SSH-nyckeln åtgärda hello SSH-konfigurationen och kontrollera disk överensstämmelse med hello VMAccess-tillägget</span><span class="sxs-lookup"><span data-stu-id="89c98-103">How tooreset a Linux VM password or SSH key, fix hello SSH configuration, and check disk consistency using hello VMAccess extension</span></span>
<span data-ttu-id="89c98-104">Om du inte kan ansluta tooa Linux-dator i Azure på grund av ett nytt lösenord, en felaktigt SSH (Secure Shell) nyckel eller ett problem med hello SSH-konfigurationen, använda hello VMAccessForLinux-tillägget med hello Azure CLI tooreset hello lösenord eller SSH-nyckel, åtgärda Hej SSH-konfigurationen och kontrollera disk konsekvenskontroll.</span><span class="sxs-lookup"><span data-stu-id="89c98-104">If you can't connect tooa Linux virtual machine on Azure because of a forgotten password, an incorrect Secure Shell (SSH) key, or a problem with hello SSH configuration, use hello VMAccessForLinux extension with hello Azure CLI tooreset hello password or SSH key, fix hello SSH configuration, and check disk consistency.</span></span> 

> [!IMPORTANT] 
> <span data-ttu-id="89c98-105">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="89c98-105">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="89c98-106">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="89c98-106">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="89c98-107">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="89c98-107">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="89c98-108">Lär dig hur för[utföra dessa steg med hello Resource Manager-modellen](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).</span><span class="sxs-lookup"><span data-stu-id="89c98-108">Learn how too[perform these steps using hello Resource Manager model](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).</span></span>

<span data-ttu-id="89c98-109">Med hello Azure CLI, använder du hello **azure vm-tillägget set** från dina kommandoradsgränssnittet (Bash, Terminal, Kommandotolken) tooaccess kommandon.</span><span class="sxs-lookup"><span data-stu-id="89c98-109">With hello Azure CLI, you use hello **azure vm extension set** command from your command-line interface (Bash, Terminal, Command prompt) tooaccess commands.</span></span> <span data-ttu-id="89c98-110">Kör **azure vm-tillägget hjälpfunktion** för tillägg för detaljerad användning.</span><span class="sxs-lookup"><span data-stu-id="89c98-110">Run **azure help vm extension set** for detailed extension usage.</span></span>

<span data-ttu-id="89c98-111">Du kan göra med hello Azure CLI, hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="89c98-111">With hello Azure CLI, you can do hello following tasks:</span></span>

* [<span data-ttu-id="89c98-112">Återställ hello lösenord</span><span class="sxs-lookup"><span data-stu-id="89c98-112">Reset hello password</span></span>](#pwresetcli)
* [<span data-ttu-id="89c98-113">Återställa hello SSH-nyckeln</span><span class="sxs-lookup"><span data-stu-id="89c98-113">Reset hello SSH key</span></span>](#sshkeyresetcli)
* [<span data-ttu-id="89c98-114">Återställa hello lösenord och hello SSH-nyckeln</span><span class="sxs-lookup"><span data-stu-id="89c98-114">Reset hello password and hello SSH key</span></span>](#resetbothcli)
* [<span data-ttu-id="89c98-115">Skapa ett nytt användarkonto sudo</span><span class="sxs-lookup"><span data-stu-id="89c98-115">Create a new sudo user account</span></span>](#createnewsudocli)
* [<span data-ttu-id="89c98-116">Återställ hello SSH-konfiguration</span><span class="sxs-lookup"><span data-stu-id="89c98-116">Reset hello SSH configuration</span></span>](#sshconfigresetcli)
* [<span data-ttu-id="89c98-117">Tar bort en användare</span><span class="sxs-lookup"><span data-stu-id="89c98-117">Delete a user</span></span>](#deletecli)
* [<span data-ttu-id="89c98-118">Visa hello status för hello VMAccess-tillägget</span><span class="sxs-lookup"><span data-stu-id="89c98-118">Display hello status of hello VMAccess extension</span></span>](#statuscli)
* [<span data-ttu-id="89c98-119">Kontrollera konsekvensen för tillagda diskar</span><span class="sxs-lookup"><span data-stu-id="89c98-119">Check consistency of added disks</span></span>](#checkdisk)
* [<span data-ttu-id="89c98-120">Reparera tillagda diskar på Linux-VM</span><span class="sxs-lookup"><span data-stu-id="89c98-120">Repair added disks on your Linux VM</span></span>](#repairdisk)

## <a name="prerequisites"></a><span data-ttu-id="89c98-121">Krav</span><span class="sxs-lookup"><span data-stu-id="89c98-121">Prerequisites</span></span>
<span data-ttu-id="89c98-122">Du behöver toodo hello följande:</span><span class="sxs-lookup"><span data-stu-id="89c98-122">You will need toodo hello following:</span></span>

* <span data-ttu-id="89c98-123">Du behöver för[installera hello Azure CLI](../../../cli-install-nodejs.md) och [ansluter tooyour prenumeration](../../../xplat-cli-connect.md) toouse Azure resurser som är associerade med ditt konto.</span><span class="sxs-lookup"><span data-stu-id="89c98-123">You will need too[install hello Azure CLI](../../../cli-install-nodejs.md) and [connect tooyour subscription](../../../xplat-cli-connect.md) toouse Azure resources associated with your account.</span></span>
* <span data-ttu-id="89c98-124">Ange hello rätt läge för hello klassiska distributionsmodellen genom att skriva följande hello hello Kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="89c98-124">Set hello correct mode for hello classic deployment model by typing hello following at hello command prompt:</span></span>
    ``` 
        azure config mode asm
    ```
* <span data-ttu-id="89c98-125">Har ett nytt lösenord eller SSH-nycklar, om du vill tooreset antingen en.</span><span class="sxs-lookup"><span data-stu-id="89c98-125">Have a new password or set of SSH keys, if you want tooreset either one.</span></span> <span data-ttu-id="89c98-126">Du behöver inte dessa om du vill tooreset hello SSH-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="89c98-126">You don't need these if you want tooreset hello SSH configuration.</span></span>

## <span data-ttu-id="89c98-127"><a name="pwresetcli"></a>Återställ hello lösenord</span><span class="sxs-lookup"><span data-stu-id="89c98-127"><a name="pwresetcli"></a>Reset hello password</span></span>
1. <span data-ttu-id="89c98-128">Skapa en fil på den lokala datorn med namnet PrivateConf.json med dessa rader.</span><span class="sxs-lookup"><span data-stu-id="89c98-128">Create a file on your local computer named PrivateConf.json with these lines.</span></span> <span data-ttu-id="89c98-129">Ersätt **MittAnvändarnamn** och  **myP@ssW0rd**  med användarnamn och lösenord och Ställ in datum för förfallodatum.</span><span class="sxs-lookup"><span data-stu-id="89c98-129">Replace **myUserName** and **myP@ssW0rd** with your own user name and password and set your own date for expiration.</span></span>

    ```   
        {
        "username":"myUserName",
        "password":"myP@ssW0rd",
        "expiration":"2020-01-01"
        }
    ```
        
2. <span data-ttu-id="89c98-130">Kör det här kommandot ersätter hello namnet på den virtuella datorn för **myVM**.</span><span class="sxs-lookup"><span data-stu-id="89c98-130">Run this command, substituting hello name of your virtual machine for **myVM**.</span></span>

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json
    ```

## <span data-ttu-id="89c98-131"><a name="sshkeyresetcli"></a>Återställa hello SSH-nyckeln</span><span class="sxs-lookup"><span data-stu-id="89c98-131"><a name="sshkeyresetcli"></a>Reset hello SSH key</span></span>
1. <span data-ttu-id="89c98-132">Skapa en fil som heter PrivateConf.json med innehållet.</span><span class="sxs-lookup"><span data-stu-id="89c98-132">Create a file named PrivateConf.json with these contents.</span></span> <span data-ttu-id="89c98-133">Ersätt hello **MittAnvändarnamn** och **mySSHKey** värden med din egen information.</span><span class="sxs-lookup"><span data-stu-id="89c98-133">Replace hello **myUserName** and **mySSHKey** values with your own information.</span></span>

    ```   
        {
        "username":"myUserName",
        "ssh_key":"mySSHKey"
        }
    ```
2. <span data-ttu-id="89c98-134">Kör det här kommandot ersätter hello namnet på den virtuella datorn för **myVM**.</span><span class="sxs-lookup"><span data-stu-id="89c98-134">Run this command, substituting hello name of your virtual machine for **myVM**.</span></span>
   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <span data-ttu-id="89c98-135"><a name="resetbothcli"></a>Återställa både hello lösenord och hello SSH-nyckel</span><span class="sxs-lookup"><span data-stu-id="89c98-135"><a name="resetbothcli"></a>Reset both hello password and hello SSH key</span></span>
1. <span data-ttu-id="89c98-136">Skapa en fil som heter PrivateConf.json med innehållet.</span><span class="sxs-lookup"><span data-stu-id="89c98-136">Create a file named PrivateConf.json with these contents.</span></span> <span data-ttu-id="89c98-137">Ersätt hello **MittAnvändarnamn**, **mySSHKey** och  **myP@ssW0rd**  värden med din egen information.</span><span class="sxs-lookup"><span data-stu-id="89c98-137">Replace hello **myUserName**, **mySSHKey** and **myP@ssW0rd** values with your own information.</span></span>

    ``` 
        {
        "username":"myUserName",
        "ssh_key":"mySSHKey",
        "password":"myP@ssW0rd"
        }
    ```

2. <span data-ttu-id="89c98-138">Kör det här kommandot ersätter hello namnet på den virtuella datorn för **myVM**.</span><span class="sxs-lookup"><span data-stu-id="89c98-138">Run this command, substituting hello name of your virtual machine for **myVM**.</span></span>

    ```   
        azure vm extension set MyVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <span data-ttu-id="89c98-139"><a name="createnewsudocli"></a>Skapa ett nytt användarkonto sudo</span><span class="sxs-lookup"><span data-stu-id="89c98-139"><a name="createnewsudocli"></a>Create a new sudo user account</span></span>

<span data-ttu-id="89c98-140">Om du glömmer ditt användarnamn kan använda du VMAccess toocreate en ny med hello sudo utfärdare.</span><span class="sxs-lookup"><span data-stu-id="89c98-140">If you forget your user name, you can use VMAccess toocreate a new one with hello sudo authority.</span></span> <span data-ttu-id="89c98-141">I det här fallet hello befintligt användarnamn och lösenord ändras inte.</span><span class="sxs-lookup"><span data-stu-id="89c98-141">In this case, hello existing user name and password will not be modified.</span></span>

<span data-ttu-id="89c98-142">toocreate en ny användare sudo med lösenordsåtkomst, Använd hello skript i [Återställ hello lösenord](#pwresetcli) och ange hello nytt användarnamn.</span><span class="sxs-lookup"><span data-stu-id="89c98-142">toocreate a new sudo user with password access, use hello script in [Reset hello password](#pwresetcli) and specify hello new user name.</span></span>

<span data-ttu-id="89c98-143">toocreate en ny sudo-användare med SSH-nyckel åtkomst, Använd hello skript i [Återställ hello SSH-nyckeln](#sshkeyresetcli) och ange hello nytt användarnamn.</span><span class="sxs-lookup"><span data-stu-id="89c98-143">toocreate a new sudo user with SSH key access, use hello script in [Reset hello SSH key](#sshkeyresetcli) and specify hello new user name.</span></span>

<span data-ttu-id="89c98-144">Du kan också använda [återställa hello lösenord och hello SSH-nyckeln](#resetbothcli) toocreate en ny användare med både lösenord och SSH-nyckel åtkomst.</span><span class="sxs-lookup"><span data-stu-id="89c98-144">You can also use [Reset hello password and hello SSH key](#resetbothcli) toocreate a new user with both password and SSH key access.</span></span>

## <span data-ttu-id="89c98-145"><a name="sshconfigresetcli"></a>Återställ hello SSH-konfiguration</span><span class="sxs-lookup"><span data-stu-id="89c98-145"><a name="sshconfigresetcli"></a>Reset hello SSH configuration</span></span>
<span data-ttu-id="89c98-146">Om hello SSH-konfigurationen finns i ett oönskat tillstånd, kan du också förlora åtkomst toohello VM.</span><span class="sxs-lookup"><span data-stu-id="89c98-146">If hello SSH configuration is in an undesired state, you might also lose access toohello VM.</span></span> <span data-ttu-id="89c98-147">Du kan använda hello VMAccess-tillägget tooreset hello configuration tooits standardläget.</span><span class="sxs-lookup"><span data-stu-id="89c98-147">You can use hello VMAccess extension tooreset hello configuration tooits default state.</span></span> <span data-ttu-id="89c98-148">toodo så behöver du bara tooset hello ”reset_ssh” nyckel för ”True”.</span><span class="sxs-lookup"><span data-stu-id="89c98-148">toodo so, you just need tooset hello “reset_ssh” key too“True”.</span></span> <span data-ttu-id="89c98-149">hello-tillägget ska starta om hello SSH-server, öppna hello SSH-port på den virtuella datorn och Återställ hello SSH toodefault konfigurationsvärden.</span><span class="sxs-lookup"><span data-stu-id="89c98-149">hello extension will restart hello SSH server, open hello SSH port on your VM, and reset hello SSH configuration toodefault values.</span></span> <span data-ttu-id="89c98-150">hello användarkonto (namn, lösenord eller SSH-nycklar) ändras inte.</span><span class="sxs-lookup"><span data-stu-id="89c98-150">hello user account (name, password or SSH keys) will not be changed.</span></span>

> [!NOTE]
> <span data-ttu-id="89c98-151">hello SSH-konfigurationsfil som hämtar återställa finns i /etc/ssh/sshd_config.</span><span class="sxs-lookup"><span data-stu-id="89c98-151">hello SSH configuration file that gets reset is located at /etc/ssh/sshd_config.</span></span>
> 
> 

1. <span data-ttu-id="89c98-152">Skapa en fil som heter PrivateConf.json med det här innehållet.</span><span class="sxs-lookup"><span data-stu-id="89c98-152">Create a file named PrivateConf.json with this content.</span></span>

    ```   
        {
        "reset_ssh":"True"
        }
    ```

2. <span data-ttu-id="89c98-153">Kör det här kommandot ersätter hello namnet på den virtuella datorn för **myVM**.</span><span class="sxs-lookup"><span data-stu-id="89c98-153">Run this command, substituting hello name of your virtual machine for **myVM**.</span></span> 

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <span data-ttu-id="89c98-154"><a name="deletecli"></a>Tar bort en användare</span><span class="sxs-lookup"><span data-stu-id="89c98-154"><a name="deletecli"></a>Delete a user</span></span>
<span data-ttu-id="89c98-155">Om du vill toodelete ett användarkonto utan att logga in på toohello VM direkt, kan du använda det här skriptet.</span><span class="sxs-lookup"><span data-stu-id="89c98-155">If you want toodelete a user account without logging into toohello VM directly, you can use this script.</span></span>

1. <span data-ttu-id="89c98-156">Skapa en fil med namnet PrivateConf.json med det här innehållet ersätter hello användaren namnet tooremove för **removeUserName**.</span><span class="sxs-lookup"><span data-stu-id="89c98-156">Create a file named PrivateConf.json with this content, substituting hello user name tooremove for **removeUserName**.</span></span> 

    ```   
        {
        "remove_user":"removeUserName"
        }
    ```

2. <span data-ttu-id="89c98-157">Kör det här kommandot ersätter hello namnet på den virtuella datorn för **myVM**.</span><span class="sxs-lookup"><span data-stu-id="89c98-157">Run this command, substituting hello name of your virtual machine for **myVM**.</span></span> 

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <span data-ttu-id="89c98-158"><a name="statuscli"></a>Visa hello status för hello VMAccess-tillägget</span><span class="sxs-lookup"><span data-stu-id="89c98-158"><a name="statuscli"></a>Display hello status of hello VMAccess extension</span></span>
<span data-ttu-id="89c98-159">toodisplay hello status för hello VMAccess-tillägget, kör kommandot.</span><span class="sxs-lookup"><span data-stu-id="89c98-159">toodisplay hello status of hello VMAccess extension, run this command.</span></span>

```
        azure vm extension get
```

## <span data-ttu-id="89c98-160"><a name='checkdisk'></a>Kontrollera konsekvensen för tillagda diskar</span><span class="sxs-lookup"><span data-stu-id="89c98-160"><a name='checkdisk'></a>Check consistency of added disks</span></span>
<span data-ttu-id="89c98-161">toorun fsck på alla diskar i Linux-dator, måste toodo hello följande:</span><span class="sxs-lookup"><span data-stu-id="89c98-161">toorun fsck on all disks in your Linux virtual machine, you will need toodo hello following:</span></span>

1. <span data-ttu-id="89c98-162">Skapa en fil som heter PublicConf.json med det här innehållet.</span><span class="sxs-lookup"><span data-stu-id="89c98-162">Create a file named PublicConf.json with this content.</span></span> <span data-ttu-id="89c98-163">Kontrollera att disken tar ett booleskt värde för om toocheck diskar kopplade tooyour virtuell dator eller inte.</span><span class="sxs-lookup"><span data-stu-id="89c98-163">Check Disk takes a boolean for whether toocheck disks attached tooyour virtual machine or not.</span></span> 

    ```   
        {   
        "check_disk": "true"
        }
    ```

2. <span data-ttu-id="89c98-164">Kör det här kommandot tooexecute, ersätter hello namnet på den virtuella datorn för **myVM**.</span><span class="sxs-lookup"><span data-stu-id="89c98-164">Run this command tooexecute, substituting hello name of your virtual machine for **myVM**.</span></span>

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json 
    ```

## <span data-ttu-id="89c98-165"><a name='repairdisk'></a>Reparera diskar</span><span class="sxs-lookup"><span data-stu-id="89c98-165"><a name='repairdisk'></a>Repair disks</span></span>
<span data-ttu-id="89c98-166">toorepair diskar som inte montera eller har montera konfigurationsfel använda hello VMAccess-tillägget tooreset hello montera konfigurationen på Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="89c98-166">toorepair disks that are not mounting or have mount configuration errors, use hello VMAccess extension tooreset hello mount configuration on your Linux virtual machine.</span></span> <span data-ttu-id="89c98-167">Ersätter hello namnet på disken för **myDisk**.</span><span class="sxs-lookup"><span data-stu-id="89c98-167">Substituting hello name of your disk for **myDisk**.</span></span>

1. <span data-ttu-id="89c98-168">Skapa en fil som heter PublicConf.json med det här innehållet.</span><span class="sxs-lookup"><span data-stu-id="89c98-168">Create a file named PublicConf.json with this content.</span></span> 

    ```   
        {
        "repair_disk":"true",
        "disk_name":"myDisk"
        }
    ```

2. <span data-ttu-id="89c98-169">Kör det här kommandot tooexecute, ersätter hello namnet på den virtuella datorn för **myVM**.</span><span class="sxs-lookup"><span data-stu-id="89c98-169">Run this command tooexecute, substituting hello name of your virtual machine for **myVM**.</span></span>

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json
    ```

## <a name="next-steps"></a><span data-ttu-id="89c98-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="89c98-170">Next steps</span></span>
* <span data-ttu-id="89c98-171">Om du vill toouse Azure PowerShell-cmdlets eller Azure Resource Manager mallar tooreset hello lösenord eller SSH-nyckel, åtgärda hello SSH-konfigurationen och kontrollera konsekvens disk, se hello [VMAccess-tillägget dokumentation på GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).</span><span class="sxs-lookup"><span data-stu-id="89c98-171">If you want toouse Azure PowerShell cmdlets or Azure Resource Manager templates tooreset hello password or SSH key, fix hello SSH configuration, and check disk consistency, see hello [VMAccess extension documentation on GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).</span></span> 
* <span data-ttu-id="89c98-172">Du kan också använda hello [Azure-portalen](https://portal.azure.com) tooreset hello lösenord eller SSH-nyckel för en Linux-VM distribueras i hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="89c98-172">You can also use hello [Azure portal](https://portal.azure.com) tooreset hello password or SSH key of a Linux VM deployed in hello classic deployment model.</span></span> <span data-ttu-id="89c98-173">Du kan inte använder hello portal gör toothis för en Linux VM distribueras i hello Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="89c98-173">You can't currently use hello portal do toothis for a Linux VM deployed in hello Resource Manager deployment model.</span></span>
* <span data-ttu-id="89c98-174">Se [om virtuella datortillägg och funktioner](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mer information om användning av VM-tillägg för virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="89c98-174">See [About virtual machine extensions and features](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for more about using VM extensions for Azure virtual machines.</span></span>

