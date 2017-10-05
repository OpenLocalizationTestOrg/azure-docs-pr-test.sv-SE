---
title: "Använda SSH-nycklar med Windows för virtuella Linux-datorer | Microsoft Docs"
description: "Lär dig hur du skapar och använder SSH-nycklar på en Windows-dator för att ansluta till en virtuell Linux-dator på Azure."
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
ms.openlocfilehash: 66837a3a153cda041f5351c52c8ccb1f8ccfea50
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a><span data-ttu-id="808c5-103">Hur man använda SSH-nycklar med Windows på Azure</span><span class="sxs-lookup"><span data-stu-id="808c5-103">How to Use SSH keys with Windows on Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="808c5-104">Windows</span><span class="sxs-lookup"><span data-stu-id="808c5-104">Windows</span></span>](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> * [<span data-ttu-id="808c5-105">Linux/Mac</span><span class="sxs-lookup"><span data-stu-id="808c5-105">Linux/Mac</span></span>](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
>
>

<span data-ttu-id="808c5-106">När du ansluter till Linux virtuella datorer (VM) i Azure, bör du använda [offentliga nycklar](https://wikipedia.org/wiki/Public-key_cryptography) att tillhandahålla ett säkrare sätt att logga in till Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="808c5-106">When you connect to Linux virtual machines (VMs) in Azure, you should use [public-key cryptography](https://wikipedia.org/wiki/Public-key_cryptography) to provide a more secure way to log in to your Linux VM.</span></span> <span data-ttu-id="808c5-107">Den här processen omfattar ett offentliga och privata nyckelutbyte för kommandot secure shell (SSH) för att autentisera dig själv i stället för användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="808c5-107">This process involves a public and private key exchange using the secure shell (SSH) command to authenticate yourself rather than a username and password.</span></span> <span data-ttu-id="808c5-108">Lösenord är sårbara för brute force-attacker, särskilt på virtuella datorer mot Internet, till exempel webbservrar.</span><span class="sxs-lookup"><span data-stu-id="808c5-108">Passwords are vulnerable to brute-force attacks, especially on Internet-facing VMs such as web servers.</span></span> <span data-ttu-id="808c5-109">Den här artikeln innehåller en översikt över SSH-nycklar och hur du skapar lämpliga nycklar på en Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="808c5-109">This article provides an overview of SSH keys and how to generate the appropriate keys on a Windows computer.</span></span>

## <a name="overview-of-ssh-and-keys"></a><span data-ttu-id="808c5-110">Översikt över SSH och nycklar</span><span class="sxs-lookup"><span data-stu-id="808c5-110">Overview of SSH and keys</span></span>
<span data-ttu-id="808c5-111">Du kan på ett säkert sätt logga in till Linux-VM med hjälp av offentliga och privata nycklar:</span><span class="sxs-lookup"><span data-stu-id="808c5-111">You can securely log in to your Linux VM by using public and private keys:</span></span>

* <span data-ttu-id="808c5-112">Den **offentliga nyckel** är placerad på ditt Linux-VM eller någon annan tjänst som du vill använda med offentliga nycklar.</span><span class="sxs-lookup"><span data-stu-id="808c5-112">The **public key** is placed on your Linux VM, or any other service that you wish to use with public-key cryptography.</span></span>
* <span data-ttu-id="808c5-113">Den **privata nyckel** är vad du presentera för Linux-VM när du loggar in, för att verifiera din identitet.</span><span class="sxs-lookup"><span data-stu-id="808c5-113">The **private key** is what you present to your Linux VM when you log in, to verify your identity.</span></span> <span data-ttu-id="808c5-114">Skydda den privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="808c5-114">Protect this private key.</span></span> <span data-ttu-id="808c5-115">Dela den inte med andra.</span><span class="sxs-lookup"><span data-stu-id="808c5-115">Do not share it.</span></span>

<span data-ttu-id="808c5-116">Dessa offentliga och privata nycklar kan användas på flera virtuella datorer och tjänster.</span><span class="sxs-lookup"><span data-stu-id="808c5-116">These public and private keys can be used on multiple VMs and services.</span></span> <span data-ttu-id="808c5-117">Du behöver inte ett nyckelpar för varje virtuell dator eller tjänst som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="808c5-117">You do not need a pair of keys for each VM or service you wish to access.</span></span> <span data-ttu-id="808c5-118">En detaljerad översikt finns [offentliga nycklar](https://wikipedia.org/wiki/Public-key_cryptography).</span><span class="sxs-lookup"><span data-stu-id="808c5-118">For a more detailed overview, see [public-key cryptography](https://wikipedia.org/wiki/Public-key_cryptography).</span></span>

<span data-ttu-id="808c5-119">SSH är en krypterad anslutningsprotokoll som tillåter säker inloggning via oskyddat anslutningar.</span><span class="sxs-lookup"><span data-stu-id="808c5-119">SSH is an encrypted connection protocol that allows secure logins over unsecured connections.</span></span> <span data-ttu-id="808c5-120">Det är standardprotokollet för anslutning för virtuella Linux-datorer finns i Azure.</span><span class="sxs-lookup"><span data-stu-id="808c5-120">It is the default connection protocol for Linux VMs hosted in Azure.</span></span> <span data-ttu-id="808c5-121">Även om SSH själva ger en krypterad anslutning, lämnar med hjälp av lösenord med SSH-anslutningar fortfarande den virtuella datorn sårbar för brute force-attacker eller att gissa lösenord.</span><span class="sxs-lookup"><span data-stu-id="808c5-121">Although SSH itself provides an encrypted connection, using passwords with SSH connections still leaves the VM vulnerable to brute-force attacks or guessing of passwords.</span></span> <span data-ttu-id="808c5-122">Det är en säkrare och önskade metod för att ansluta till en virtuell dator med hjälp av SSH med hjälp av offentliga och privata nycklarna, även kallat SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="808c5-122">A more secure and preferred method of connecting to a VM using SSH is by using these public and private keys, also known as SSH keys.</span></span>

<span data-ttu-id="808c5-123">Om du inte vill använda SSH-nycklar kan du fortfarande logga in till din virtuella Linux-datorer med ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="808c5-123">If you do not wish to use SSH keys, you can still log in to your Linux VMs using a password.</span></span> <span data-ttu-id="808c5-124">Om den virtuella datorn inte visas för Internet, kan det vara tillräckligt att använda lösenord.</span><span class="sxs-lookup"><span data-stu-id="808c5-124">If your VM is not exposed to the Internet, using passwords may be sufficient.</span></span> <span data-ttu-id="808c5-125">Men behöver du fortfarande hantera lösenord för varje Linux-VM och underhålla felfri lösenordsprinciper och praxis som minsta längd på lösenord och regelbundet uppdaterar dem.</span><span class="sxs-lookup"><span data-stu-id="808c5-125">However, you still need to manage your passwords for each Linux VM and maintain healthy password policies and practices, such as minimum password length and regularly updating them.</span></span> <span data-ttu-id="808c5-126">Användning av SSH-nycklar minskar den för att hantera enskilda autentiseringsuppgifter över flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="808c5-126">The use of SSH keys reduces the complexity of managing individual credentials across multiple VMs.</span></span>

## <a name="windows-packages-and-ssh-clients"></a><span data-ttu-id="808c5-127">Windows-paket och SSH-klienter</span><span class="sxs-lookup"><span data-stu-id="808c5-127">Windows packages and SSH clients</span></span>
<span data-ttu-id="808c5-128">Du ansluter till och hantera virtuella Linux-datorer i Azure med hjälp av en **SSH-klienten**.</span><span class="sxs-lookup"><span data-stu-id="808c5-128">You connect to and manage Linux VMs in Azure using an **SSH client**.</span></span> <span data-ttu-id="808c5-129">Windows-datorer har inte normalt en SSH-klienten installerad.</span><span class="sxs-lookup"><span data-stu-id="808c5-129">Windows computers do not typically have an SSH client installed.</span></span> <span data-ttu-id="808c5-130">Uppdatering av Windows 10 årsdagar läggs Bash för Windows och den senaste uppdateringen av Windows 10-skapare ger ytterligare uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="808c5-130">The Windows 10 Anniversary Update added Bash for Windows, and the latest Windows 10 Creators Update provides additional updates.</span></span> <span data-ttu-id="808c5-131">Den här Windows-undersystem för Linux kan du köra och åtkomst verktyg, till exempel en SSH-klient internt i ett Bash-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="808c5-131">This Windows Subsystem for Linux allows you to run and access utilities such as an SSH client natively within a Bash shell.</span></span> <span data-ttu-id="808c5-132">Sedan kan du följa alla Linux-dokument som [hur du skapar SSH-nyckelpar för Linux](mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="808c5-132">You can then follow any of the Linux docs, such as [How to generate SSH key pairs for Linux](mac-create-ssh-keys.md).</span></span> <span data-ttu-id="808c5-133">Bash för Windows är fortfarande under utveckling och betraktas som en betaversion.</span><span class="sxs-lookup"><span data-stu-id="808c5-133">Bash for Windows is still under development, and is considered a beta release.</span></span> <span data-ttu-id="808c5-134">Mer information om Bash för Windows finns [Bash på Ubuntu på Windows](https://msdn.microsoft.com/commandline/wsl/about).</span><span class="sxs-lookup"><span data-stu-id="808c5-134">For more information about Bash for Windows, see [Bash on Ubuntu on Windows](https://msdn.microsoft.com/commandline/wsl/about).</span></span>

<span data-ttu-id="808c5-135">Om du vill använda något annat än Bash för Windows, vanliga Windows SSH klienter kan du installera ingår i följande paket:</span><span class="sxs-lookup"><span data-stu-id="808c5-135">If you wish to use something other than Bash for Windows, common Windows SSH clients you can install are included in the following packages:</span></span>

* [<span data-ttu-id="808c5-136">Git för Windows</span><span class="sxs-lookup"><span data-stu-id="808c5-136">Git For Windows</span></span>](https://git-for-windows.github.io/)
* [<span data-ttu-id="808c5-137">puTTY</span><span class="sxs-lookup"><span data-stu-id="808c5-137">puTTY</span></span>](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
* [<span data-ttu-id="808c5-138">MobaXterm</span><span class="sxs-lookup"><span data-stu-id="808c5-138">MobaXterm</span></span>](http://mobaxterm.mobatek.net/)
* [<span data-ttu-id="808c5-139">Cygwin</span><span class="sxs-lookup"><span data-stu-id="808c5-139">Cygwin</span></span>](https://cygwin.com/)


## <a name="which-key-files-do-you-need-to-create"></a><span data-ttu-id="808c5-140">Vilka viktiga filer behöver du skapa?</span><span class="sxs-lookup"><span data-stu-id="808c5-140">Which key files do you need to create?</span></span>
<span data-ttu-id="808c5-141">Azure kräver minst 2048-bitars **ssh-rsa** formaterad offentliga och privata nycklar.</span><span class="sxs-lookup"><span data-stu-id="808c5-141">Azure requires at least 2048-bit, **ssh-rsa** formatted public and private keys.</span></span> <span data-ttu-id="808c5-142">Om du hanterar Azure-resurser med hjälp av den klassiska distributionsmodellen kan du också behöva skapa en PEM (`.pem` filen).</span><span class="sxs-lookup"><span data-stu-id="808c5-142">If you are managing Azure resources using the Classic deployment model, you also need to generate a PEM (`.pem` file).</span></span>

<span data-ttu-id="808c5-143">Här följer scenarier för distribution och vilka typer av filer som används i varje:</span><span class="sxs-lookup"><span data-stu-id="808c5-143">Here are the deployment scenarios, and the types of files you use in each:</span></span>

1. <span data-ttu-id="808c5-144">**SSH-rsa** nycklarna är obligatoriska för några med den [Azure-portalen](https://portal.azure.com), och Resource Manager distributioner med hjälp av den [Azure CLI](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="808c5-144">**ssh-rsa** keys are required for any deployment using the [Azure portal](https://portal.azure.com), and Resource Manager deployments using the [Azure CLI](../../cli-install-nodejs.md).</span></span>
   * <span data-ttu-id="808c5-145">Nyckeln är oftast alla de flesta användare behöver.</span><span class="sxs-lookup"><span data-stu-id="808c5-145">These keys are usually all most people need.</span></span>
2. <span data-ttu-id="808c5-146">En `.pem` filen behövs för att skapa virtuella datorer med hjälp av den klassiska distributionen.</span><span class="sxs-lookup"><span data-stu-id="808c5-146">A `.pem` file is required to create VMs using the Classic deployment.</span></span> <span data-ttu-id="808c5-147">De här nycklarna stöds i klassiska distributioner när de [Azure-portalen](https://portal.azure.com) eller [Azure CLI](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="808c5-147">These keys are supported in Classic deployments when using the [Azure portal](https://portal.azure.com) or [Azure CLI](../../cli-install-nodejs.md).</span></span>
   * <span data-ttu-id="808c5-148">Behöver du bara skapa dessa ytterligare nycklar och certifikat om du hanterar resurser som har skapats med hjälp av den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="808c5-148">You only need to create these additional keys and certificates if you are managing resources created using the Classic deployment model.</span></span>

## <a name="install-git-for-windows"></a><span data-ttu-id="808c5-149">Installera Git för Windows</span><span class="sxs-lookup"><span data-stu-id="808c5-149">Install Git for Windows</span></span>
<span data-ttu-id="808c5-150">I föregående avsnitt visas flera paket som innehåller den `openssl` tool för Windows.</span><span class="sxs-lookup"><span data-stu-id="808c5-150">The preceding section listed several packages that include the `openssl` tool for Windows.</span></span> <span data-ttu-id="808c5-151">Det här verktyget behövs för att skapa offentliga och privata nycklar.</span><span class="sxs-lookup"><span data-stu-id="808c5-151">This tool is needed to create public and private keys.</span></span> <span data-ttu-id="808c5-152">I följande exempel innehåller information om hur du installerar och använder **Git för Windows**, men du kan välja det paket som du föredrar.</span><span class="sxs-lookup"><span data-stu-id="808c5-152">The following examples detail how to install and use **Git for Windows**, though you can choose whichever package you prefer.</span></span> <span data-ttu-id="808c5-153">**Git för Windows** ger dig tillgång till vissa ytterligare programvara med öppen källkod ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) verktyg och hjälpmedel som kan vara användbar när du arbetar med virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="808c5-153">**Git for Windows** gives you access to some additional open-source software ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) tools and utilities that may be useful as you work with Linux VMs.</span></span>

1. <span data-ttu-id="808c5-154">Hämta och installera **Git för Windows** från följande plats: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).</span><span class="sxs-lookup"><span data-stu-id="808c5-154">Download and install **Git for Windows** from the following location: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).</span></span>
2. <span data-ttu-id="808c5-155">Acceptera standardalternativen under installationen om du inte specifikt behöver ändra dem.</span><span class="sxs-lookup"><span data-stu-id="808c5-155">Accept the default options during the install process unless you specifically need to change them.</span></span>
3. <span data-ttu-id="808c5-156">Kör **Git Bash** från den **Start-menyn** > **Git** > **Git Bash**.</span><span class="sxs-lookup"><span data-stu-id="808c5-156">Run **Git Bash** from the **Start Menu** > **Git** > **Git Bash**.</span></span> <span data-ttu-id="808c5-157">Konsolen ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="808c5-157">The console looks similar to the following example:</span></span>

    ![Git för Windows Bash-gränssnitt](./media/ssh-from-windows/git-bash-window.png)

## <a name="create-a-private-key"></a><span data-ttu-id="808c5-159">Skapa en privat nyckel</span><span class="sxs-lookup"><span data-stu-id="808c5-159">Create a private key</span></span>
1. <span data-ttu-id="808c5-160">I din **Git Bash** fönster, Använd `openssl.exe` att skapa en privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="808c5-160">In your **Git Bash** window, use `openssl.exe` to create a private key.</span></span> <span data-ttu-id="808c5-161">I följande exempel skapas en nyckel som heter `myPrivateKey` och certifikatet med namnet `myCert.pem`:</span><span class="sxs-lookup"><span data-stu-id="808c5-161">The following example creates a key named `myPrivateKey` and certificate named `myCert.pem`:</span></span>

    ```bash
    openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout myPrivateKey.key -out myCert.pem
    ```

    <span data-ttu-id="808c5-162">Utdata liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="808c5-162">The output looks similar to the following example:</span></span>

    ```bash
    Generating a 2048 bit RSA private key
    .......................................+++
    .......................+++
    writing new private key to 'myPrivateKey.key'
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:
    ```

   <span data-ttu-id="808c5-163">Om bash rapporterar ett fel, försök att öppna en ny **Git Bash** fönster med förhöjda privilegier.</span><span class="sxs-lookup"><span data-stu-id="808c5-163">If bash reports an error, try opening a new **Git Bash** window with elevated privileges.</span></span> <span data-ttu-id="808c5-164">Kör sedan den `openssl` kommando.</span><span class="sxs-lookup"><span data-stu-id="808c5-164">Then, rerun the `openssl` command.</span></span>

2. <span data-ttu-id="808c5-165">Besvara anvisningarna för landets namn, plats, organisationsnamn och så vidare.</span><span class="sxs-lookup"><span data-stu-id="808c5-165">Answer the prompts for country name, location, organization name, etc.</span></span>
3. <span data-ttu-id="808c5-166">Din nya privata nyckeln och certifikatet skapas i den aktuella arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="808c5-166">Your new private key and certificate are created in your current working directory.</span></span> <span data-ttu-id="808c5-167">Som en säkerhetsåtgärd bör du ange behörigheten på din privata nyckel så att bara du har åtkomst till den:</span><span class="sxs-lookup"><span data-stu-id="808c5-167">As a security measure, you should set the permissions on your private key so that only you can access it:</span></span>

    ```bash
    chmod 0600 myPrivateKey.key
    ```

4. <span data-ttu-id="808c5-168">Den [nästa avsnitt](#create-a-private-key-for-putty) information med hjälp av PuTTYgen både visa och använda den offentliga nyckeln och skapa en privat nyckel som är specifika för att använda PuTTY för att SSH till Linux virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="808c5-168">The [next section](#create-a-private-key-for-putty) details using PuTTYgen to both view and use the public key, and create a private key specific for using PuTTY to SSH to Linux VMs.</span></span> <span data-ttu-id="808c5-169">Följande kommando skapar en offentlig nyckelfil med namnet `myPublicKey.key` som du kan använda direkt:</span><span class="sxs-lookup"><span data-stu-id="808c5-169">The following command generates a public key file named `myPublicKey.key` that you can use right away:</span></span>

    ```bash
    openssl.exe rsa -pubout -in myPrivateKey.key -out myPublicKey.key
    ```

5. <span data-ttu-id="808c5-170">Om du behöver hantera klassiska resurser kan du konvertera den `myCert.pem` till `myCert.cer` (DER-kodad X509 certifikat).</span><span class="sxs-lookup"><span data-stu-id="808c5-170">If you also need to manage Classic resources, convert the `myCert.pem` to `myCert.cer` (DER encoded X509 certificate).</span></span> <span data-ttu-id="808c5-171">Det här valfria steget endast utföra om du behöver hantera specifikt äldre klassiska resurser.</span><span class="sxs-lookup"><span data-stu-id="808c5-171">Perform this optional step only if you need to specifically manage older Classic resources.</span></span>

    <span data-ttu-id="808c5-172">Konvertera certifikatet med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="808c5-172">Convert the certificate using the following command:</span></span>

    ```bash
    openssl.exe  x509 -outform der -in myCert.pem -out myCert.cer
    ```

## <a name="create-a-private-key-for-putty"></a><span data-ttu-id="808c5-173">Skapa en privat nyckel för PuTTY</span><span class="sxs-lookup"><span data-stu-id="808c5-173">Create a private key for PuTTY</span></span>
<span data-ttu-id="808c5-174">PuTTY är en gemensam SSH-klient för Windows.</span><span class="sxs-lookup"><span data-stu-id="808c5-174">PuTTY is a common SSH client for Windows.</span></span> <span data-ttu-id="808c5-175">Du kan använda SSH-klienten som du vill.</span><span class="sxs-lookup"><span data-stu-id="808c5-175">You are free to use any SSH client that you wish.</span></span> <span data-ttu-id="808c5-176">Om du vill använda PuTTY måste du skapa ytterligare en typ av nyckel - en PuTTY privata nyckel (PPK).</span><span class="sxs-lookup"><span data-stu-id="808c5-176">To use PuTTY, you need to create an additional type of key - a PuTTY Private Key (PPK).</span></span> <span data-ttu-id="808c5-177">Hoppa över det här avsnittet om du inte vill använda PuTTY.</span><span class="sxs-lookup"><span data-stu-id="808c5-177">If you do not wish to use PuTTY, skip this section.</span></span>

<span data-ttu-id="808c5-178">I följande exempel skapas den här ytterligare privata nyckeln för PuTTY för att använda:</span><span class="sxs-lookup"><span data-stu-id="808c5-178">The following example creates this additional private key specifically for PuTTY to use:</span></span>

1. <span data-ttu-id="808c5-179">Använd **Git Bash** Konvertera din privata nyckel till en privat RSA-nyckel som PuTTYgen kan förstå.</span><span class="sxs-lookup"><span data-stu-id="808c5-179">Use **Git Bash** to convert your private key into an RSA private key that PuTTYgen can understand.</span></span> <span data-ttu-id="808c5-180">I följande exempel skapas en nyckel som heter `myPrivateKey_rsa` från den befintliga nyckeln med namnet `myPrivateKey`:</span><span class="sxs-lookup"><span data-stu-id="808c5-180">The following example creates a key named `myPrivateKey_rsa` from the existing key named `myPrivateKey`:</span></span>

    ```bash
    openssl rsa -in ./myPrivateKey.key -out myPrivateKey_rsa
    ```

    <span data-ttu-id="808c5-181">Som en säkerhetsåtgärd bör du ange behörigheten på din privata nyckel så att bara du har åtkomst till den:</span><span class="sxs-lookup"><span data-stu-id="808c5-181">As a security measure, you should set the permissions on your private key so that only you can access it:</span></span>

    ```bash
    chmod 0600 myPrivateKey_rsa
    ```
2. <span data-ttu-id="808c5-182">Hämta och kör PuTTYgen från följande plats: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span><span class="sxs-lookup"><span data-stu-id="808c5-182">Download and run PuTTYgen from the following location: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span></span>
3. <span data-ttu-id="808c5-183">Klicka på menyn: **filen** > **belastningen privat nyckel**</span><span class="sxs-lookup"><span data-stu-id="808c5-183">Click the menu: **File** > **Load Private Key**</span></span>
4. <span data-ttu-id="808c5-184">Leta upp din privata nyckel (`myPrivateKey_rsa` i föregående exempel).</span><span class="sxs-lookup"><span data-stu-id="808c5-184">Locate your private key (`myPrivateKey_rsa` in the previous example).</span></span> <span data-ttu-id="808c5-185">Standardkatalogen när du startar **Git Bash** är `C:\Users\%username%`.</span><span class="sxs-lookup"><span data-stu-id="808c5-185">The default directory when you start **Git Bash** is `C:\Users\%username%`.</span></span> <span data-ttu-id="808c5-186">Ändra filen filtret för att visa **alla filer (\*.\*)** :</span><span class="sxs-lookup"><span data-stu-id="808c5-186">Change the file filter to show **All Files (\*.\*)**:</span></span>

    ![Läsa in befintlig privat nyckel i PuTTYgen](./media/ssh-from-windows/load-private-key.png)
5. <span data-ttu-id="808c5-188">Klicka på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="808c5-188">Click **Open**.</span></span> <span data-ttu-id="808c5-189">En uppmaning anger att nyckeln har importerats:</span><span class="sxs-lookup"><span data-stu-id="808c5-189">A prompt indicates that the key has been successfully imported:</span></span>

    ![Importerats nyckel till PuTTYgen](./media/ssh-from-windows/successfully-imported-key.png)
6. <span data-ttu-id="808c5-191">Klicka på **OK** att stänga Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="808c5-191">Click **OK** to close the prompt.</span></span>
7. <span data-ttu-id="808c5-192">Den offentliga nyckeln visas överst i den **PuTTYgen** fönster.</span><span class="sxs-lookup"><span data-stu-id="808c5-192">The public key is displayed at the top of the **PuTTYgen** window.</span></span> <span data-ttu-id="808c5-193">Kopiera och klistra in den här offentliga nyckeln i Azure-portalen eller Azure Resource Manager-mall när du skapar en Linux VM.</span><span class="sxs-lookup"><span data-stu-id="808c5-193">You copy and paste this public key into the Azure portal or Azure Resource Manager template when you create a Linux VM.</span></span> <span data-ttu-id="808c5-194">Du kan också klicka på **spara offentlig nyckel** att spara en kopia på datorn:</span><span class="sxs-lookup"><span data-stu-id="808c5-194">You can also click **Save public key** to save a copy to your computer:</span></span>

    ![Spara PuTTY fil för offentlig nyckel](./media/ssh-from-windows/save-public-key.png)

    <span data-ttu-id="808c5-196">I följande exempel visas hur du kan kopiera och klistra in den här offentliga nyckeln i Azure-portalen när du skapar en Linux VM.</span><span class="sxs-lookup"><span data-stu-id="808c5-196">The following example shows how you would copy and paste this public key into the Azure portal when you create a Linux VM.</span></span> <span data-ttu-id="808c5-197">Den offentliga nyckeln är vanligtvis lagras sedan i `~/.ssh/authorized_keys` på den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="808c5-197">The public key is typically then stored in `~/.ssh/authorized_keys` on your new VM.</span></span>

    ![Använda offentliga nyckel när du skapar en virtuell dator i Azure-portalen](./media/ssh-from-windows/use-public-key-azure-portal.png)
8. <span data-ttu-id="808c5-199">Tillbaka i **PuTTYgen**, klickar du på **Spara privat nyckel**:</span><span class="sxs-lookup"><span data-stu-id="808c5-199">Back in **PuTTYgen**, Click **Save private Key**:</span></span>

    ![Spara filen med PuTTY privata nyckel](./media/ssh-from-windows/save-ppk-file.png)

   > [!WARNING]
   > <span data-ttu-id="808c5-201">En uppmaning som frågar om du vill fortsätta utan att ange en lösenfras för nyckeln.</span><span class="sxs-lookup"><span data-stu-id="808c5-201">A prompt asks if you wish to continue without entering a passphrase for your key.</span></span> <span data-ttu-id="808c5-202">Det är en lösenfras som ett lösenord som är ansluten till din privata nyckel.</span><span class="sxs-lookup"><span data-stu-id="808c5-202">A passphrase is like a password attached to your private key.</span></span> <span data-ttu-id="808c5-203">Även om någon försöker hämta din privata nyckel skulle de fortfarande inte kan autentisera med bara nyckeln.</span><span class="sxs-lookup"><span data-stu-id="808c5-203">Even if someone were to obtain your private key, they still would not be able to authenticate using just the key.</span></span> <span data-ttu-id="808c5-204">De måste också lösenfrasen.</span><span class="sxs-lookup"><span data-stu-id="808c5-204">They would also need the passphrase.</span></span> <span data-ttu-id="808c5-205">Utan en lösenfras om någon erhåller din privata nyckel, kan de logga in på en virtuell dator eller tjänst som använder nyckeln.</span><span class="sxs-lookup"><span data-stu-id="808c5-205">Without a passphrase, if someone obtains your private key, they can log in to any VM or service that uses that key.</span></span> <span data-ttu-id="808c5-206">Vi rekommenderar att du skapar en lösenfras.</span><span class="sxs-lookup"><span data-stu-id="808c5-206">We recommend you create a passphrase.</span></span> <span data-ttu-id="808c5-207">Men om du glömmer bort lösenfrasen finns inget sätt att återställa den.</span><span class="sxs-lookup"><span data-stu-id="808c5-207">However, if you forget the passphrase, there is no way to recover it.</span></span>
   >
   >

    <span data-ttu-id="808c5-208">Om du vill ange en lösenfras klickar du på **nr**, ange en lösenfras i PuTTYgen-fönstret och klicka sedan på **Spara privat nyckel** igen.</span><span class="sxs-lookup"><span data-stu-id="808c5-208">If you wish to enter a passphrase, click **No**, enter a passphrase in the main PuTTYgen window, and then click **Save private key** again.</span></span> <span data-ttu-id="808c5-209">Annars klickar du på **Ja** fortsätta utan att ange valfri lösenfras.</span><span class="sxs-lookup"><span data-stu-id="808c5-209">Otherwise, click **Yes** to continue without providing the optional passphrase.</span></span>
9. <span data-ttu-id="808c5-210">Ange ett namn och en plats att spara PPK-filen.</span><span class="sxs-lookup"><span data-stu-id="808c5-210">Enter a name and location to save your PPK file.</span></span>

## <a name="use-putty-to-ssh-to-a-linux-machine"></a><span data-ttu-id="808c5-211">Använd Putty för att SSH till en Linux-dator</span><span class="sxs-lookup"><span data-stu-id="808c5-211">Use Putty to SSH to a Linux Machine</span></span>
<span data-ttu-id="808c5-212">PuTTY är igen, en vanliga SSH-klient för Windows.</span><span class="sxs-lookup"><span data-stu-id="808c5-212">Again, PuTTY is a common SSH client for Windows.</span></span> <span data-ttu-id="808c5-213">Du kan använda SSH-klienten som du vill.</span><span class="sxs-lookup"><span data-stu-id="808c5-213">You are free to use any SSH client that you wish.</span></span> <span data-ttu-id="808c5-214">Följ anvisningarna nedan hur du använder din privata nyckel för att autentisera dina virtuella Azure-datorn via SSH.</span><span class="sxs-lookup"><span data-stu-id="808c5-214">The following steps detail how to use your private key to authenticate with your Azure VM using SSH.</span></span> <span data-ttu-id="808c5-215">Stegen är ungefär som i andra SSH key-klienter som behöver läsa in din privata nyckel att autentisera SSH-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="808c5-215">The steps are similar in other SSH key clients in terms of needing to load your private key to authenticate the SSH connection.</span></span>

1. <span data-ttu-id="808c5-216">Hämta och kör putty från följande plats: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span><span class="sxs-lookup"><span data-stu-id="808c5-216">Download and run putty from the following location: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span></span>
2. <span data-ttu-id="808c5-217">Fyll i värdnamn eller IP-adressen för den virtuella datorn från Azure portal:</span><span class="sxs-lookup"><span data-stu-id="808c5-217">Fill in the host name or IP address of your VM from the Azure portal:</span></span>

    ![Öppna ny PuTTY anslutning](./media/ssh-from-windows/putty-new-connection.png)
3. <span data-ttu-id="808c5-219">Innan du väljer **öppna**, klickar du på **anslutning** > **SSH** > **Auth** fliken.</span><span class="sxs-lookup"><span data-stu-id="808c5-219">Before selecting **Open**, click **Connection** > **SSH** > **Auth** tab.</span></span> <span data-ttu-id="808c5-220">Bläddra till och markera din privata nyckel:</span><span class="sxs-lookup"><span data-stu-id="808c5-220">Browse to and select your private key:</span></span>

    ![Välj din PuTTY privata nyckel för autentisering](./media/ssh-from-windows/putty-auth-dialog.png)
4. <span data-ttu-id="808c5-222">Klicka på **öppna** att ansluta till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="808c5-222">Click **Open** to connect to your virtual machine</span></span>

## <a name="next-steps"></a><span data-ttu-id="808c5-223">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="808c5-223">Next steps</span></span>
<span data-ttu-id="808c5-224">Du kan också generera offentliga och privata nycklar [använder OS X- och Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="808c5-224">You can also generate the public and private keys [using OS X and Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="808c5-225">Mer information om Bash för Windows och fördelarna med OSS verktyg är tillgänglig på Windows-dator finns [Bash på Ubuntu på Windows](https://msdn.microsoft.com/commandline/wsl/about).</span><span class="sxs-lookup"><span data-stu-id="808c5-225">For more information about Bash for Windows and the benefits of having OSS tools readily available on your Windows computer, see [Bash on Ubuntu on Windows](https://msdn.microsoft.com/commandline/wsl/about).</span></span>

<span data-ttu-id="808c5-226">Om du har problem med SSH att ansluta till din virtuella Linux-datorer, se [felsökning av SSH-anslutningar till en Azure Linux VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="808c5-226">If you have trouble using SSH to connect to your Linux VMs, see [Troubleshoot SSH connections to an Azure Linux VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
