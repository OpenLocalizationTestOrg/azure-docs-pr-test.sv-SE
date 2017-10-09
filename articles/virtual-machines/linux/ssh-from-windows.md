---
title: "aaaUse SSH-nycklar för Linux virtuella datorer med Windows | Microsoft Docs"
description: "Lär dig hur toogenerate och Använd SSH-nycklar på en Windows datorn tooconnect tooa Linux virtuella dator på Azure."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 2cacda3b-7949-4036-bd5d-837e8b09a9c8
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: danlep
ms.openlocfilehash: 6c44217332538857cc2ca2e85de4b476aa71251c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-ssh-keys-with-windows-on-azure"></a><span data-ttu-id="c61e9-103">Hur tooUse SSH-nycklar med Windows på Azure</span><span class="sxs-lookup"><span data-stu-id="c61e9-103">How tooUse SSH keys with Windows on Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c61e9-104">Windows</span><span class="sxs-lookup"><span data-stu-id="c61e9-104">Windows</span></span>](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> * [<span data-ttu-id="c61e9-105">Linux/Mac</span><span class="sxs-lookup"><span data-stu-id="c61e9-105">Linux/Mac</span></span>](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
>
>

<span data-ttu-id="c61e9-106">När du ansluter tooLinux virtuella maskiner (VMs) i Azure, bör du använda [offentliga nycklar](https://wikipedia.org/wiki/Public-key_cryptography) tooprovide ett säkrare sätt toolog i tooyour Linux VM.</span><span class="sxs-lookup"><span data-stu-id="c61e9-106">When you connect tooLinux virtual machines (VMs) in Azure, you should use [public-key cryptography](https://wikipedia.org/wiki/Public-key_cryptography) tooprovide a more secure way toolog in tooyour Linux VM.</span></span> <span data-ttu-id="c61e9-107">Den här processen omfattar en offentliga och privata nycklar exchange med hello secure shell (SSH) kommandot tooauthenticate själv i stället för användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="c61e9-107">This process involves a public and private key exchange using hello secure shell (SSH) command tooauthenticate yourself rather than a username and password.</span></span> <span data-ttu-id="c61e9-108">Lösenord är sårbar toobrute force-attacker, särskilt på virtuella datorer mot Internet, till exempel webbservrar.</span><span class="sxs-lookup"><span data-stu-id="c61e9-108">Passwords are vulnerable toobrute-force attacks, especially on Internet-facing VMs such as web servers.</span></span> <span data-ttu-id="c61e9-109">Den här artikeln innehåller en översikt över SSH-nycklar och hur toogenerate hello lämpliga nycklar på en Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="c61e9-109">This article provides an overview of SSH keys and how toogenerate hello appropriate keys on a Windows computer.</span></span>

## <a name="overview-of-ssh-and-keys"></a><span data-ttu-id="c61e9-110">Översikt över SSH och nycklar</span><span class="sxs-lookup"><span data-stu-id="c61e9-110">Overview of SSH and keys</span></span>
<span data-ttu-id="c61e9-111">Du kan på ett säkert sätt logga in tooyour Linux VM med hjälp av offentliga och privata nycklar:</span><span class="sxs-lookup"><span data-stu-id="c61e9-111">You can securely log in tooyour Linux VM by using public and private keys:</span></span>

* <span data-ttu-id="c61e9-112">Hej **offentliga nyckel** är placerad på din Linux VM eller någon annan tjänst som du vill toouse med offentliga nycklar.</span><span class="sxs-lookup"><span data-stu-id="c61e9-112">hello **public key** is placed on your Linux VM, or any other service that you wish toouse with public-key cryptography.</span></span>
* <span data-ttu-id="c61e9-113">Hej **privata nyckel** är vad du finns tooyour Linux VM när du loggar in tooverify din identitet.</span><span class="sxs-lookup"><span data-stu-id="c61e9-113">hello **private key** is what you present tooyour Linux VM when you log in, tooverify your identity.</span></span> <span data-ttu-id="c61e9-114">Skydda den privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="c61e9-114">Protect this private key.</span></span> <span data-ttu-id="c61e9-115">Dela den inte med andra.</span><span class="sxs-lookup"><span data-stu-id="c61e9-115">Do not share it.</span></span>

<span data-ttu-id="c61e9-116">Dessa offentliga och privata nycklar kan användas på flera virtuella datorer och tjänster.</span><span class="sxs-lookup"><span data-stu-id="c61e9-116">These public and private keys can be used on multiple VMs and services.</span></span> <span data-ttu-id="c61e9-117">Du behöver inte ett nyckelpar för varje virtuell dator eller tjänst som du vill tooaccess.</span><span class="sxs-lookup"><span data-stu-id="c61e9-117">You do not need a pair of keys for each VM or service you wish tooaccess.</span></span> <span data-ttu-id="c61e9-118">En detaljerad översikt finns [offentliga nycklar](https://wikipedia.org/wiki/Public-key_cryptography).</span><span class="sxs-lookup"><span data-stu-id="c61e9-118">For a more detailed overview, see [public-key cryptography](https://wikipedia.org/wiki/Public-key_cryptography).</span></span>

<span data-ttu-id="c61e9-119">SSH är en krypterad anslutningsprotokoll som tillåter säker inloggning via oskyddat anslutningar.</span><span class="sxs-lookup"><span data-stu-id="c61e9-119">SSH is an encrypted connection protocol that allows secure logins over unsecured connections.</span></span> <span data-ttu-id="c61e9-120">Det är hello anslutning standardprotokollet för virtuella Linux-datorer finns i Azure.</span><span class="sxs-lookup"><span data-stu-id="c61e9-120">It is hello default connection protocol for Linux VMs hosted in Azure.</span></span> <span data-ttu-id="c61e9-121">Även om SSH själva ger en krypterad anslutning, lämnar med hjälp av lösenord med SSH-anslutningar fortfarande hello VM sårbara toobrute force-attacker eller att gissa lösenord.</span><span class="sxs-lookup"><span data-stu-id="c61e9-121">Although SSH itself provides an encrypted connection, using passwords with SSH connections still leaves hello VM vulnerable toobrute-force attacks or guessing of passwords.</span></span> <span data-ttu-id="c61e9-122">En säkrare och prioriterade metoden för anslutande tooa VM som använder SSH är med hjälp av offentliga och privata nycklarna, även kallat SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="c61e9-122">A more secure and preferred method of connecting tooa VM using SSH is by using these public and private keys, also known as SSH keys.</span></span>

<span data-ttu-id="c61e9-123">Om du inte vill att toouse SSH-nycklar kan du fortfarande logga in tooyour virtuella Linux-datorer med ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="c61e9-123">If you do not wish toouse SSH keys, you can still log in tooyour Linux VMs using a password.</span></span> <span data-ttu-id="c61e9-124">Om den virtuella datorn inte är synliga toohello Internet, kan det vara tillräckligt att använda lösenord.</span><span class="sxs-lookup"><span data-stu-id="c61e9-124">If your VM is not exposed toohello Internet, using passwords may be sufficient.</span></span> <span data-ttu-id="c61e9-125">Men du fortfarande behöver toomanage lösenorden för varje Linux-VM och underhåll felfri lösenordsprinciper och praxis som minsta längd på lösenord och regelbundet uppdaterar dem.</span><span class="sxs-lookup"><span data-stu-id="c61e9-125">However, you still need toomanage your passwords for each Linux VM and maintain healthy password policies and practices, such as minimum password length and regularly updating them.</span></span> <span data-ttu-id="c61e9-126">hello användning av SSH-nycklar minskar hello hantera enskilda autentiseringsuppgifter över flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c61e9-126">hello use of SSH keys reduces hello complexity of managing individual credentials across multiple VMs.</span></span>

## <a name="windows-packages-and-ssh-clients"></a><span data-ttu-id="c61e9-127">Windows-paket och SSH-klienter</span><span class="sxs-lookup"><span data-stu-id="c61e9-127">Windows packages and SSH clients</span></span>
<span data-ttu-id="c61e9-128">Du ansluter tooand hantera virtuella Linux-datorer i Azure med hjälp av en **SSH-klienten**.</span><span class="sxs-lookup"><span data-stu-id="c61e9-128">You connect tooand manage Linux VMs in Azure using an **SSH client**.</span></span> <span data-ttu-id="c61e9-129">Windows-datorer har inte normalt en SSH-klienten installerad.</span><span class="sxs-lookup"><span data-stu-id="c61e9-129">Windows computers do not typically have an SSH client installed.</span></span> <span data-ttu-id="c61e9-130">hello Windows 10 årsdagar Update läggs Bash för Windows och hello senaste skapare av uppdateringen för Windows 10 ger ytterligare uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="c61e9-130">hello Windows 10 Anniversary Update added Bash for Windows, and hello latest Windows 10 Creators Update provides additional updates.</span></span> <span data-ttu-id="c61e9-131">Den här Windows-undersystem för Linux kan verktygen toorun och åtkomst till exempel en SSH-klienten internt i ett Bash-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="c61e9-131">This Windows Subsystem for Linux allows you toorun and access utilities such as an SSH client natively within a Bash shell.</span></span> <span data-ttu-id="c61e9-132">Sedan kan du följa eventuella hello Linux dokumenten som [hur toogenerate SSH-nyckel-par för Linux](mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="c61e9-132">You can then follow any of hello Linux docs, such as [How toogenerate SSH key pairs for Linux](mac-create-ssh-keys.md).</span></span> <span data-ttu-id="c61e9-133">Bash för Windows är fortfarande under utveckling och betraktas som en betaversion.</span><span class="sxs-lookup"><span data-stu-id="c61e9-133">Bash for Windows is still under development, and is considered a beta release.</span></span> <span data-ttu-id="c61e9-134">Mer information om Bash för Windows finns [Bash på Ubuntu på Windows](https://msdn.microsoft.com/commandline/wsl/about).</span><span class="sxs-lookup"><span data-stu-id="c61e9-134">For more information about Bash for Windows, see [Bash on Ubuntu on Windows](https://msdn.microsoft.com/commandline/wsl/about).</span></span>

<span data-ttu-id="c61e9-135">Om du inte vill toouse något annat än Bash för Windows, vanliga Windows SSH klienter kan du installera ingår i hello följande paket:</span><span class="sxs-lookup"><span data-stu-id="c61e9-135">If you wish toouse something other than Bash for Windows, common Windows SSH clients you can install are included in hello following packages:</span></span>

* [<span data-ttu-id="c61e9-136">Git för Windows</span><span class="sxs-lookup"><span data-stu-id="c61e9-136">Git For Windows</span></span>](https://git-for-windows.github.io/)
* [<span data-ttu-id="c61e9-137">puTTY</span><span class="sxs-lookup"><span data-stu-id="c61e9-137">puTTY</span></span>](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
* [<span data-ttu-id="c61e9-138">MobaXterm</span><span class="sxs-lookup"><span data-stu-id="c61e9-138">MobaXterm</span></span>](http://mobaxterm.mobatek.net/)
* [<span data-ttu-id="c61e9-139">Cygwin</span><span class="sxs-lookup"><span data-stu-id="c61e9-139">Cygwin</span></span>](https://cygwin.com/)


## <a name="which-key-files-do-you-need-toocreate"></a><span data-ttu-id="c61e9-140">Som nyckeln filer behöver du toocreate?</span><span class="sxs-lookup"><span data-stu-id="c61e9-140">Which key files do you need toocreate?</span></span>
<span data-ttu-id="c61e9-141">Azure kräver minst 2048-bitars **ssh-rsa** formaterad offentliga och privata nycklar.</span><span class="sxs-lookup"><span data-stu-id="c61e9-141">Azure requires at least 2048-bit, **ssh-rsa** formatted public and private keys.</span></span> <span data-ttu-id="c61e9-142">Om du hanterar Azure-resurser med hjälp av hello klassiska distributionsmodellen, måste du också toogenerate en PEM (`.pem` filen).</span><span class="sxs-lookup"><span data-stu-id="c61e9-142">If you are managing Azure resources using hello Classic deployment model, you also need toogenerate a PEM (`.pem` file).</span></span>

<span data-ttu-id="c61e9-143">Här följer hello distributionsscenarier och hello typer av filer som används i varje:</span><span class="sxs-lookup"><span data-stu-id="c61e9-143">Here are hello deployment scenarios, and hello types of files you use in each:</span></span>

1. <span data-ttu-id="c61e9-144">**SSH-rsa** nycklarna är obligatoriska för en distribution med hello [Azure-portalen](https://portal.azure.com), och Resource Manager distributioner med hjälp av hello [Azure CLI](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="c61e9-144">**ssh-rsa** keys are required for any deployment using hello [Azure portal](https://portal.azure.com), and Resource Manager deployments using hello [Azure CLI](../../cli-install-nodejs.md).</span></span>
   * <span data-ttu-id="c61e9-145">Nyckeln är oftast alla de flesta användare behöver.</span><span class="sxs-lookup"><span data-stu-id="c61e9-145">These keys are usually all most people need.</span></span>
2. <span data-ttu-id="c61e9-146">En `.pem` filen är obligatoriska toocreate virtuella datorer med hello klassisk distribution.</span><span class="sxs-lookup"><span data-stu-id="c61e9-146">A `.pem` file is required toocreate VMs using hello Classic deployment.</span></span> <span data-ttu-id="c61e9-147">De här nycklarna kan användas i klassiska distributioner med hello [Azure-portalen](https://portal.azure.com) eller [Azure CLI](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="c61e9-147">These keys are supported in Classic deployments when using hello [Azure portal](https://portal.azure.com) or [Azure CLI](../../cli-install-nodejs.md).</span></span>
   * <span data-ttu-id="c61e9-148">Du behöver bara toocreate dessa ytterligare nycklar och certifikat om du hanterar resurser som har skapats med hjälp av hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="c61e9-148">You only need toocreate these additional keys and certificates if you are managing resources created using hello Classic deployment model.</span></span>

## <a name="install-git-for-windows"></a><span data-ttu-id="c61e9-149">Installera Git för Windows</span><span class="sxs-lookup"><span data-stu-id="c61e9-149">Install Git for Windows</span></span>
<span data-ttu-id="c61e9-150">hello föregående avsnitt visas flera paket som innehåller hello `openssl` tool för Windows.</span><span class="sxs-lookup"><span data-stu-id="c61e9-150">hello preceding section listed several packages that include hello `openssl` tool for Windows.</span></span> <span data-ttu-id="c61e9-151">Det här verktyget är nödvändiga toocreate offentliga och privata nycklar.</span><span class="sxs-lookup"><span data-stu-id="c61e9-151">This tool is needed toocreate public and private keys.</span></span> <span data-ttu-id="c61e9-152">Hej följande exempel information om hur tooinstall och använda **Git för Windows**, men du kan välja det paket som du föredrar.</span><span class="sxs-lookup"><span data-stu-id="c61e9-152">hello following examples detail how tooinstall and use **Git for Windows**, though you can choose whichever package you prefer.</span></span> <span data-ttu-id="c61e9-153">**Git för Windows** ger dig åtkomst toosome ytterligare med öppen källkod ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) verktyg och hjälpmedel som kan vara användbar när du arbetar med virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="c61e9-153">**Git for Windows** gives you access toosome additional open-source software ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) tools and utilities that may be useful as you work with Linux VMs.</span></span>

1. <span data-ttu-id="c61e9-154">Hämta och installera **Git för Windows** från hello följande plats: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).</span><span class="sxs-lookup"><span data-stu-id="c61e9-154">Download and install **Git for Windows** from hello following location: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).</span></span>
2. <span data-ttu-id="c61e9-155">Acceptera hello standardalternativen under hello installera processen om du inte specifikt behöver toochange dem.</span><span class="sxs-lookup"><span data-stu-id="c61e9-155">Accept hello default options during hello install process unless you specifically need toochange them.</span></span>
3. <span data-ttu-id="c61e9-156">Kör **Git Bash** från hello **Start-menyn** > **Git** > **Git Bash**.</span><span class="sxs-lookup"><span data-stu-id="c61e9-156">Run **Git Bash** from hello **Start Menu** > **Git** > **Git Bash**.</span></span> <span data-ttu-id="c61e9-157">hello konsolen ser ut ungefär toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="c61e9-157">hello console looks similar toohello following example:</span></span>

    ![Git för Windows Bash-gränssnitt](./media/ssh-from-windows/git-bash-window.png)

## <a name="create-a-private-key"></a><span data-ttu-id="c61e9-159">Skapa en privat nyckel</span><span class="sxs-lookup"><span data-stu-id="c61e9-159">Create a private key</span></span>
1. <span data-ttu-id="c61e9-160">I din **Git Bash** fönster, Använd `openssl.exe` toocreate en privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="c61e9-160">In your **Git Bash** window, use `openssl.exe` toocreate a private key.</span></span> <span data-ttu-id="c61e9-161">hello följande exempel skapas en nyckel som heter `myPrivateKey` och certifikatet med namnet `myCert.pem`:</span><span class="sxs-lookup"><span data-stu-id="c61e9-161">hello following example creates a key named `myPrivateKey` and certificate named `myCert.pem`:</span></span>

    ```bash
    openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout myPrivateKey.key -out myCert.pem
    ```

    <span data-ttu-id="c61e9-162">hello utdata ser ut ungefär toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="c61e9-162">hello output looks similar toohello following example:</span></span>

    ```bash
    Generating a 2048 bit RSA private key
    .......................................+++
    .......................+++
    writing new private key too'myPrivateKey.key'
    -----
    You are about toobe asked tooenter information that will be incorporated
    into your certificate request.
    What you are about tooenter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', hello field will be left blank.
    -----
    Country Name (2 letter code) [AU]:
    ```

   <span data-ttu-id="c61e9-163">Om bash rapporterar ett fel, försök att öppna en ny **Git Bash** fönster med förhöjda privilegier.</span><span class="sxs-lookup"><span data-stu-id="c61e9-163">If bash reports an error, try opening a new **Git Bash** window with elevated privileges.</span></span> <span data-ttu-id="c61e9-164">Sedan kör hello `openssl` kommando.</span><span class="sxs-lookup"><span data-stu-id="c61e9-164">Then, rerun hello `openssl` command.</span></span>

2. <span data-ttu-id="c61e9-165">Svaret hello efterfrågar landets namn, plats, organisationsnamn och så vidare.</span><span class="sxs-lookup"><span data-stu-id="c61e9-165">Answer hello prompts for country name, location, organization name, etc.</span></span>
3. <span data-ttu-id="c61e9-166">Din nya privata nyckeln och certifikatet skapas i den aktuella arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="c61e9-166">Your new private key and certificate are created in your current working directory.</span></span> <span data-ttu-id="c61e9-167">Som en säkerhetsåtgärd bör du ange hello behörighet för din privata nyckel så att bara du har åtkomst till den:</span><span class="sxs-lookup"><span data-stu-id="c61e9-167">As a security measure, you should set hello permissions on your private key so that only you can access it:</span></span>

    ```bash
    chmod 0600 myPrivateKey.key
    ```

4. <span data-ttu-id="c61e9-168">Hej [nästa avsnitt](#create-a-private-key-for-putty) information med hjälp av PuTTYgen tooboth visa och använda hello offentliga nyckel och skapa en privat nyckel som är specifika för att använda PuTTY tooSSH tooLinux virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c61e9-168">hello [next section](#create-a-private-key-for-putty) details using PuTTYgen tooboth view and use hello public key, and create a private key specific for using PuTTY tooSSH tooLinux VMs.</span></span> <span data-ttu-id="c61e9-169">hello följande kommando skapar en offentlig nyckelfil med namnet `myPublicKey.key` som du kan använda direkt:</span><span class="sxs-lookup"><span data-stu-id="c61e9-169">hello following command generates a public key file named `myPublicKey.key` that you can use right away:</span></span>

    ```bash
    openssl.exe rsa -pubout -in myPrivateKey.key -out myPublicKey.key
    ```

5. <span data-ttu-id="c61e9-170">Om du behöver också toomanage klassiska resurser kan konvertera hello `myCert.pem` för`myCert.cer` (DER-kodad X509 certifikat).</span><span class="sxs-lookup"><span data-stu-id="c61e9-170">If you also need toomanage Classic resources, convert hello `myCert.pem` too`myCert.cer` (DER encoded X509 certificate).</span></span> <span data-ttu-id="c61e9-171">Utför det här valfria steget endast om du behöver toospecifically hantera äldre klassiska resurser.</span><span class="sxs-lookup"><span data-stu-id="c61e9-171">Perform this optional step only if you need toospecifically manage older Classic resources.</span></span>

    <span data-ttu-id="c61e9-172">Konvertera hello certifikat med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c61e9-172">Convert hello certificate using hello following command:</span></span>

    ```bash
    openssl.exe  x509 -outform der -in myCert.pem -out myCert.cer
    ```

## <a name="create-a-private-key-for-putty"></a><span data-ttu-id="c61e9-173">Skapa en privat nyckel för PuTTY</span><span class="sxs-lookup"><span data-stu-id="c61e9-173">Create a private key for PuTTY</span></span>
<span data-ttu-id="c61e9-174">PuTTY är en gemensam SSH-klient för Windows.</span><span class="sxs-lookup"><span data-stu-id="c61e9-174">PuTTY is a common SSH client for Windows.</span></span> <span data-ttu-id="c61e9-175">Du är ledigt toouse SSH-klienten som du vill.</span><span class="sxs-lookup"><span data-stu-id="c61e9-175">You are free toouse any SSH client that you wish.</span></span> <span data-ttu-id="c61e9-176">toouse PuTTY måste toocreate ytterligare en typ av nyckel - en PuTTY privata nyckel (PPK).</span><span class="sxs-lookup"><span data-stu-id="c61e9-176">toouse PuTTY, you need toocreate an additional type of key - a PuTTY Private Key (PPK).</span></span> <span data-ttu-id="c61e9-177">Hoppa över det här avsnittet om du inte vill att toouse PuTTY.</span><span class="sxs-lookup"><span data-stu-id="c61e9-177">If you do not wish toouse PuTTY, skip this section.</span></span>

<span data-ttu-id="c61e9-178">hello skapas följande exempel den här ytterligare privata nyckeln för PuTTY toouse:</span><span class="sxs-lookup"><span data-stu-id="c61e9-178">hello following example creates this additional private key specifically for PuTTY toouse:</span></span>

1. <span data-ttu-id="c61e9-179">Använd **Git Bash** tooconvert din privata nyckel till en privat RSA-nyckel att PuTTYgen kan förstå.</span><span class="sxs-lookup"><span data-stu-id="c61e9-179">Use **Git Bash** tooconvert your private key into an RSA private key that PuTTYgen can understand.</span></span> <span data-ttu-id="c61e9-180">hello följande exempel skapas en nyckel som heter `myPrivateKey_rsa` från hello befintlig nyckel som heter `myPrivateKey`:</span><span class="sxs-lookup"><span data-stu-id="c61e9-180">hello following example creates a key named `myPrivateKey_rsa` from hello existing key named `myPrivateKey`:</span></span>

    ```bash
    openssl rsa -in ./myPrivateKey.key -out myPrivateKey_rsa
    ```

    <span data-ttu-id="c61e9-181">Som en säkerhetsåtgärd bör du ange hello behörighet för din privata nyckel så att bara du har åtkomst till den:</span><span class="sxs-lookup"><span data-stu-id="c61e9-181">As a security measure, you should set hello permissions on your private key so that only you can access it:</span></span>

    ```bash
    chmod 0600 myPrivateKey_rsa
    ```
2. <span data-ttu-id="c61e9-182">Hämta och köra PuTTYgen från hello följande plats: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span><span class="sxs-lookup"><span data-stu-id="c61e9-182">Download and run PuTTYgen from hello following location: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span></span>
3. <span data-ttu-id="c61e9-183">Klicka på hello: **filen** > **belastningen privat nyckel**</span><span class="sxs-lookup"><span data-stu-id="c61e9-183">Click hello menu: **File** > **Load Private Key**</span></span>
4. <span data-ttu-id="c61e9-184">Leta upp din privata nyckel (`myPrivateKey_rsa` i hello föregående exempel).</span><span class="sxs-lookup"><span data-stu-id="c61e9-184">Locate your private key (`myPrivateKey_rsa` in hello previous example).</span></span> <span data-ttu-id="c61e9-185">hello standardkatalogen när du startar **Git Bash** är `C:\Users\%username%`.</span><span class="sxs-lookup"><span data-stu-id="c61e9-185">hello default directory when you start **Git Bash** is `C:\Users\%username%`.</span></span> <span data-ttu-id="c61e9-186">Ändra hello filen filter tooshow **alla filer (\*.\*)** :</span><span class="sxs-lookup"><span data-stu-id="c61e9-186">Change hello file filter tooshow **All Files (\*.\*)**:</span></span>

    ![Läsa in hello befintlig privat nyckel i PuTTYgen](./media/ssh-from-windows/load-private-key.png)
5. <span data-ttu-id="c61e9-188">Klicka på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="c61e9-188">Click **Open**.</span></span> <span data-ttu-id="c61e9-189">En uppmaning anger hello nyckeln har importerats:</span><span class="sxs-lookup"><span data-stu-id="c61e9-189">A prompt indicates that hello key has been successfully imported:</span></span>

    ![Har importerats viktiga tooPuTTYgen](./media/ssh-from-windows/successfully-imported-key.png)
6. <span data-ttu-id="c61e9-191">Klicka på **OK** tooclose hello prompten.</span><span class="sxs-lookup"><span data-stu-id="c61e9-191">Click **OK** tooclose hello prompt.</span></span>
7. <span data-ttu-id="c61e9-192">hello offentliga nyckeln visas överst hello i hello **PuTTYgen** fönster.</span><span class="sxs-lookup"><span data-stu-id="c61e9-192">hello public key is displayed at hello top of hello **PuTTYgen** window.</span></span> <span data-ttu-id="c61e9-193">Kopiera och klistra in den här offentliga nyckeln i hello Azure-portalen eller Azure Resource Manager-mall när du skapar en Linux VM.</span><span class="sxs-lookup"><span data-stu-id="c61e9-193">You copy and paste this public key into hello Azure portal or Azure Resource Manager template when you create a Linux VM.</span></span> <span data-ttu-id="c61e9-194">Du kan också klicka på **spara offentlig nyckel** toosave en kopia tooyour dator:</span><span class="sxs-lookup"><span data-stu-id="c61e9-194">You can also click **Save public key** toosave a copy tooyour computer:</span></span>

    ![Spara PuTTY fil för offentlig nyckel](./media/ssh-from-windows/save-public-key.png)

    <span data-ttu-id="c61e9-196">hello följande exempel visas hur du kopiera och klistra in den här offentliga nyckeln i hello Azure-portalen när du skapar en Linux VM.</span><span class="sxs-lookup"><span data-stu-id="c61e9-196">hello following example shows how you would copy and paste this public key into hello Azure portal when you create a Linux VM.</span></span> <span data-ttu-id="c61e9-197">hello offentliga nyckel är vanligtvis lagras sedan i `~/.ssh/authorized_keys` på den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c61e9-197">hello public key is typically then stored in `~/.ssh/authorized_keys` on your new VM.</span></span>

    ![Använd en offentlig nyckel när du skapar en virtuell dator i hello Azure-portalen](./media/ssh-from-windows/use-public-key-azure-portal.png)
8. <span data-ttu-id="c61e9-199">Tillbaka i **PuTTYgen**, klickar du på **Spara privat nyckel**:</span><span class="sxs-lookup"><span data-stu-id="c61e9-199">Back in **PuTTYgen**, Click **Save private Key**:</span></span>

    ![Spara filen med PuTTY privata nyckel](./media/ssh-from-windows/save-ppk-file.png)

   > [!WARNING]
   > <span data-ttu-id="c61e9-201">En uppmaning som frågar om du vill toocontinue utan att ange en lösenfras för nyckeln.</span><span class="sxs-lookup"><span data-stu-id="c61e9-201">A prompt asks if you wish toocontinue without entering a passphrase for your key.</span></span> <span data-ttu-id="c61e9-202">Det är en lösenfras som lösenord bifogade tooyour privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="c61e9-202">A passphrase is like a password attached tooyour private key.</span></span> <span data-ttu-id="c61e9-203">Även om någon har tooobtain din privata nyckel de fortfarande inte kan tooauthenticate med bara hello-nyckel.</span><span class="sxs-lookup"><span data-stu-id="c61e9-203">Even if someone were tooobtain your private key, they still would not be able tooauthenticate using just hello key.</span></span> <span data-ttu-id="c61e9-204">De måste också hello lösenfras.</span><span class="sxs-lookup"><span data-stu-id="c61e9-204">They would also need hello passphrase.</span></span> <span data-ttu-id="c61e9-205">Utan en lösenfras om någon erhåller din privata nyckel, kan de logga in tooany VM eller tjänst som använder nyckeln.</span><span class="sxs-lookup"><span data-stu-id="c61e9-205">Without a passphrase, if someone obtains your private key, they can log in tooany VM or service that uses that key.</span></span> <span data-ttu-id="c61e9-206">Vi rekommenderar att du skapar en lösenfras.</span><span class="sxs-lookup"><span data-stu-id="c61e9-206">We recommend you create a passphrase.</span></span> <span data-ttu-id="c61e9-207">Men om du glömmer bort hello lösenfrasen finns inget sätt toorecover den.</span><span class="sxs-lookup"><span data-stu-id="c61e9-207">However, if you forget hello passphrase, there is no way toorecover it.</span></span>
   >
   >

    <span data-ttu-id="c61e9-208">Om du vill tooenter en lösenfras klickar du på **nr**, ange en lösenfras i PuTTYgen hello huvudfönstret och klicka sedan på **Spara privat nyckel** igen.</span><span class="sxs-lookup"><span data-stu-id="c61e9-208">If you wish tooenter a passphrase, click **No**, enter a passphrase in hello main PuTTYgen window, and then click **Save private key** again.</span></span> <span data-ttu-id="c61e9-209">Annars klickar du på **Ja** toocontinue utan att ange hello valfri lösenfras.</span><span class="sxs-lookup"><span data-stu-id="c61e9-209">Otherwise, click **Yes** toocontinue without providing hello optional passphrase.</span></span>
9. <span data-ttu-id="c61e9-210">Ange ett namn och plats toosave PPK-filen.</span><span class="sxs-lookup"><span data-stu-id="c61e9-210">Enter a name and location toosave your PPK file.</span></span>

## <a name="use-putty-toossh-tooa-linux-machine"></a><span data-ttu-id="c61e9-211">Använd Putty tooSSH tooa Linux-dator</span><span class="sxs-lookup"><span data-stu-id="c61e9-211">Use Putty tooSSH tooa Linux Machine</span></span>
<span data-ttu-id="c61e9-212">PuTTY är igen, en vanliga SSH-klient för Windows.</span><span class="sxs-lookup"><span data-stu-id="c61e9-212">Again, PuTTY is a common SSH client for Windows.</span></span> <span data-ttu-id="c61e9-213">Du är ledigt toouse SSH-klienten som du vill.</span><span class="sxs-lookup"><span data-stu-id="c61e9-213">You are free toouse any SSH client that you wish.</span></span> <span data-ttu-id="c61e9-214">Hej följande steg detalj hur toouse din privata nyckel tooauthenticate med din Azure-VM via SSH.</span><span class="sxs-lookup"><span data-stu-id="c61e9-214">hello following steps detail how toouse your private key tooauthenticate with your Azure VM using SSH.</span></span> <span data-ttu-id="c61e9-215">hello stegen är ungefär i andra viktiga SSH-klienter som behöver tooload din privata nyckel tooauthenticate hello SSH-anslutning.</span><span class="sxs-lookup"><span data-stu-id="c61e9-215">hello steps are similar in other SSH key clients in terms of needing tooload your private key tooauthenticate hello SSH connection.</span></span>

1. <span data-ttu-id="c61e9-216">Hämta och kör putty från hello följande plats: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span><span class="sxs-lookup"><span data-stu-id="c61e9-216">Download and run putty from hello following location: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span></span>
2. <span data-ttu-id="c61e9-217">Fyll i hello värdnamn eller IP-adressen för den virtuella datorn från hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="c61e9-217">Fill in hello host name or IP address of your VM from hello Azure portal:</span></span>

    ![Öppna ny PuTTY anslutning](./media/ssh-from-windows/putty-new-connection.png)
3. <span data-ttu-id="c61e9-219">Innan du väljer **öppna**, klickar du på **anslutning** > **SSH** > **Auth** fliken. Bläddra tooand Välj din privata nyckel:</span><span class="sxs-lookup"><span data-stu-id="c61e9-219">Before selecting **Open**, click **Connection** > **SSH** > **Auth** tab. Browse tooand select your private key:</span></span>

    ![Välj din PuTTY privata nyckel för autentisering](./media/ssh-from-windows/putty-auth-dialog.png)
4. <span data-ttu-id="c61e9-221">Klicka på **öppna** tooconnect tooyour virtuell dator</span><span class="sxs-lookup"><span data-stu-id="c61e9-221">Click **Open** tooconnect tooyour virtual machine</span></span>

## <a name="next-steps"></a><span data-ttu-id="c61e9-222">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c61e9-222">Next steps</span></span>
<span data-ttu-id="c61e9-223">Du kan också generera hello offentliga och privata nycklar [använder OS X- och Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c61e9-223">You can also generate hello public and private keys [using OS X and Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="c61e9-224">Mer information om Bash för Windows och hello fördelarna med OSS verktyg är tillgänglig på Windows-dator finns [Bash på Ubuntu på Windows](https://msdn.microsoft.com/commandline/wsl/about).</span><span class="sxs-lookup"><span data-stu-id="c61e9-224">For more information about Bash for Windows and hello benefits of having OSS tools readily available on your Windows computer, see [Bash on Ubuntu on Windows](https://msdn.microsoft.com/commandline/wsl/about).</span></span>

<span data-ttu-id="c61e9-225">Om du har problem med att använda SSH tooconnect tooyour virtuella Linux-datorer, se [felsökning av SSH-anslutningar tooan virtuella Azure Linux-datorn](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c61e9-225">If you have trouble using SSH tooconnect tooyour Linux VMs, see [Troubleshoot SSH connections tooan Azure Linux VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
