---
title: aaaUse SSH med Hadoop - Azure HDInsight | Microsoft Docs
description: "Du kan komma åt HDInsight med hjälp av SSH (Secure Shell). Det här dokumentet innehåller information om hur du ansluter tooHDInsight med hello ssh och scp-kommandon från Windows, Linux, Unix eller macOS klienter."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: hadoop-kommandon i linux, hadoop linux-kommandon, hadoop macos, ssh hadoop, ssh hadoop-kluster
ms.assetid: a6a16405-a4a7-4151-9bbf-ab26972216c5
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/03/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: ac9e70ce3c70693c1b81c9514ba4fd47686070ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toohdinsight-hadoop-using-ssh"></a><span data-ttu-id="f62ac-105">Ansluta tooHDInsight (Hadoop) via SSH</span><span class="sxs-lookup"><span data-stu-id="f62ac-105">Connect tooHDInsight (Hadoop) using SSH</span></span>

<span data-ttu-id="f62ac-106">Lär dig hur toouse [SSH (Secure Shell)](https://en.wikipedia.org/wiki/Secure_Shell) toosecurely ansluta tooHadoop på Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f62ac-106">Learn how toouse [Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) toosecurely connect tooHadoop on Azure HDInsight.</span></span> 

<span data-ttu-id="f62ac-107">HDInsight kan använda Linux (Ubuntu) som hello operativsystem för noder i hello Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="f62ac-107">HDInsight can use Linux (Ubuntu) as hello operating system for nodes within hello Hadoop cluster.</span></span> <span data-ttu-id="f62ac-108">hello innehåller följande tabell hello-adressen och porten information som krävs när du ansluter tooLinux-baserade HDInsight med hjälp av en SSH-klient:</span><span class="sxs-lookup"><span data-stu-id="f62ac-108">hello following table contains hello address and port information needed when connecting tooLinux-based HDInsight using an SSH client:</span></span>

| <span data-ttu-id="f62ac-109">Adress</span><span class="sxs-lookup"><span data-stu-id="f62ac-109">Address</span></span> | <span data-ttu-id="f62ac-110">Port</span><span class="sxs-lookup"><span data-stu-id="f62ac-110">Port</span></span> | <span data-ttu-id="f62ac-111">Ansluter till ...</span><span class="sxs-lookup"><span data-stu-id="f62ac-111">Connects to...</span></span> |
| ----- | ----- | ----- |
| `<clustername>-ed-ssh.azurehdinsight.net` | <span data-ttu-id="f62ac-112">22</span><span class="sxs-lookup"><span data-stu-id="f62ac-112">22</span></span> | <span data-ttu-id="f62ac-113">Kantnod (R Server på HDInsight)</span><span class="sxs-lookup"><span data-stu-id="f62ac-113">Edge node (R Server on HDInsight)</span></span> |
| `<edgenodename>.<clustername>-ssh.azurehdinsight.net` | <span data-ttu-id="f62ac-114">22</span><span class="sxs-lookup"><span data-stu-id="f62ac-114">22</span></span> | <span data-ttu-id="f62ac-115">Kantnod (alla andra klustertyper, om det finns en kantnod)</span><span class="sxs-lookup"><span data-stu-id="f62ac-115">Edge node (any other cluster type, if an edge node exists)</span></span> |
| `<clustername>-ssh.azurehdinsight.net` | <span data-ttu-id="f62ac-116">22</span><span class="sxs-lookup"><span data-stu-id="f62ac-116">22</span></span> | <span data-ttu-id="f62ac-117">Den primära huvudnoden</span><span class="sxs-lookup"><span data-stu-id="f62ac-117">Primary headnode</span></span> |
| `<clustername>-ssh.azurehdinsight.net` | <span data-ttu-id="f62ac-118">23</span><span class="sxs-lookup"><span data-stu-id="f62ac-118">23</span></span> | <span data-ttu-id="f62ac-119">Den sekundära huvudnoden</span><span class="sxs-lookup"><span data-stu-id="f62ac-119">Secondary headnode</span></span> |

> [!NOTE]
> <span data-ttu-id="f62ac-120">Ersätt `<edgenodename>` med hello kantnod hello namn.</span><span class="sxs-lookup"><span data-stu-id="f62ac-120">Replace `<edgenodename>` with hello name of hello edge node.</span></span>
>
> <span data-ttu-id="f62ac-121">Ersätt `<clustername>` med hello namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="f62ac-121">Replace `<clustername>` with hello name of your cluster.</span></span>
>
> <span data-ttu-id="f62ac-122">Om klustret innehåller en kantnod, rekommenderar vi att du __ansluter alltid toohello kantnod__ via SSH.</span><span class="sxs-lookup"><span data-stu-id="f62ac-122">If your cluster contains an edge node, we recommend that you __always connect toohello edge node__ using SSH.</span></span> <span data-ttu-id="f62ac-123">Hej huvudnoderna värd för tjänster som är kritiska toohello hälsotillståndet för Hadoop.</span><span class="sxs-lookup"><span data-stu-id="f62ac-123">hello head nodes host services that are critical toohello health of Hadoop.</span></span> <span data-ttu-id="f62ac-124">hello kantnod körs bara vad som ska visas på den.</span><span class="sxs-lookup"><span data-stu-id="f62ac-124">hello edge node runs only what you put on it.</span></span>
>
> <span data-ttu-id="f62ac-125">Mer information om hur du använder kantnoder finns i [Använda kantnoder i HDInsight](hdinsight-apps-use-edge-node.md#access-an-edge-node).</span><span class="sxs-lookup"><span data-stu-id="f62ac-125">For more information on using edge nodes, see [Use edge nodes in HDInsight](hdinsight-apps-use-edge-node.md#access-an-edge-node).</span></span>

## <a name="ssh-clients"></a><span data-ttu-id="f62ac-126">SSH-klienter</span><span class="sxs-lookup"><span data-stu-id="f62ac-126">SSH clients</span></span>

<span data-ttu-id="f62ac-127">Linux, Unix- och macOS system ger hello `ssh` och `scp` kommandon.</span><span class="sxs-lookup"><span data-stu-id="f62ac-127">Linux, Unix, and macOS systems provide hello `ssh` and `scp` commands.</span></span> <span data-ttu-id="f62ac-128">Hej `ssh` klienten är vanliga toocreate en kommandorad fjärrsessionen med Linux eller Unix-baserade system.</span><span class="sxs-lookup"><span data-stu-id="f62ac-128">hello `ssh` client is commonly used toocreate a remote command-line session with a Linux or Unix-based system.</span></span> <span data-ttu-id="f62ac-129">Hej `scp` klienten är används toosecurely kopiera filer mellan klienten och hello fjärrsystemet.</span><span class="sxs-lookup"><span data-stu-id="f62ac-129">hello `scp` client is used toosecurely copy files between your client and hello remote system.</span></span>

<span data-ttu-id="f62ac-130">Microsoft Windows tillhandahåller ingen SSH-klient som standard.</span><span class="sxs-lookup"><span data-stu-id="f62ac-130">Microsoft Windows does not provide any SSH clients by default.</span></span> <span data-ttu-id="f62ac-131">Hej `ssh` och `scp` klienter som är tillgängliga för Windows via hello följande paket:</span><span class="sxs-lookup"><span data-stu-id="f62ac-131">hello `ssh` and `scp` clients are available for Windows through hello following packages:</span></span>

* <span data-ttu-id="f62ac-132">[Azure Cloud Shell](../cloud-shell/quickstart.md): hello molnet Shell innehåller en Bash-miljö i webbläsaren, samt hello `ssh`, `scp`, och andra vanliga Linux-kommandon.</span><span class="sxs-lookup"><span data-stu-id="f62ac-132">[Azure Cloud Shell](../cloud-shell/quickstart.md): hello Cloud Shell provides a Bash environment in your browser, and provides hello `ssh`, `scp`, and other common Linux commands.</span></span>

* <span data-ttu-id="f62ac-133">[Bash på Ubuntu på Windows 10](https://msdn.microsoft.com/commandline/wsl/about): hello `ssh` och `scp` kommandon är tillgängliga via hello Bash på kommandoraden i Windows.</span><span class="sxs-lookup"><span data-stu-id="f62ac-133">[Bash on Ubuntu on Windows 10](https://msdn.microsoft.com/commandline/wsl/about): hello `ssh` and `scp` commands are available through hello Bash on Windows command line.</span></span>

* <span data-ttu-id="f62ac-134">[Git (https://git-scm.com/)](https://git-scm.com/): hello `ssh` och `scp` kommandon är tillgängliga via hello Bash-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="f62ac-134">[Git (https://git-scm.com/)](https://git-scm.com/): hello `ssh` and `scp` commands are available through hello GitBash command line.</span></span>

* <span data-ttu-id="f62ac-135">[GitHub skrivbordet (https://desktop.github.com/)](https://desktop.github.com/) hello `ssh` och `scp` kommandon är tillgängliga via hello GitHub Shell-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="f62ac-135">[GitHub Desktop (https://desktop.github.com/)](https://desktop.github.com/) hello `ssh` and `scp` commands are available through hello GitHub Shell command line.</span></span> <span data-ttu-id="f62ac-136">GitHub Desktop kan vara konfigurerade toouse Bash hello Windows kommandotolk eller PowerShell hello kommandoraden för hello Git-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="f62ac-136">GitHub Desktop can be configured toouse Bash, hello Windows Command Prompt, or PowerShell as hello command line for hello Git Shell.</span></span>

* <span data-ttu-id="f62ac-137">[OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH): hello PowerShell team portera OpenSSH tooWindows och ger test-versioner.</span><span class="sxs-lookup"><span data-stu-id="f62ac-137">[OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH): hello PowerShell team is porting OpenSSH tooWindows, and provides test releases.</span></span>

    > [!WARNING]
    > <span data-ttu-id="f62ac-138">Hej OpenSSH paket innehåller hello SSH serverkomponent, `sshd`.</span><span class="sxs-lookup"><span data-stu-id="f62ac-138">hello OpenSSH package includes hello SSH server component, `sshd`.</span></span> <span data-ttu-id="f62ac-139">Den här komponenten startar en SSH-server i systemet, så att andra tooconnect tooit.</span><span class="sxs-lookup"><span data-stu-id="f62ac-139">This component starts an SSH server on your system, allowing others tooconnect tooit.</span></span> <span data-ttu-id="f62ac-140">Konfigurera den här komponenten eller inte öppna port 22 såvida du inte vill toohost en SSH-server på datorn.</span><span class="sxs-lookup"><span data-stu-id="f62ac-140">Do not configure this component or open port 22 unless you want toohost an SSH server on your system.</span></span> <span data-ttu-id="f62ac-141">Det är inte obligatoriska toocommunicate med HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f62ac-141">It is not required toocommunicate with HDInsight.</span></span>

<span data-ttu-id="f62ac-142">Det finns också flera grafiska SSH-klienter, till exempel [PuTTY (http://www.chiark.greenend.org.uk/~sgtatham/putty/)](http://www.chiark.greenend.org.uk/~sgtatham/putty/) och [MobaXterm (http://mobaxterm.mobatek.net/)](http://mobaxterm.mobatek.net/).</span><span class="sxs-lookup"><span data-stu-id="f62ac-142">There are also several graphical SSH clients, such as [PuTTY (http://www.chiark.greenend.org.uk/~sgtatham/putty/)](http://www.chiark.greenend.org.uk/~sgtatham/putty/) and [MobaXterm (http://mobaxterm.mobatek.net/)](http://mobaxterm.mobatek.net/).</span></span> <span data-ttu-id="f62ac-143">Dessa klienter kan vara används tooconnect tooHDInsight, hello anslutningsprocessen skiljer sig med hjälp av hello `ssh` verktyget.</span><span class="sxs-lookup"><span data-stu-id="f62ac-143">While these clients can be used tooconnect tooHDInsight, hello process of connecting is different than using hello `ssh` utility.</span></span> <span data-ttu-id="f62ac-144">Mer information finns i hello dokumentationen för hello grafiska klient du använder.</span><span class="sxs-lookup"><span data-stu-id="f62ac-144">For more information, see hello documentation of hello graphical client you are using.</span></span>

## <span data-ttu-id="f62ac-145"><a id="sshkey"></a>Autentisering: SSH-nycklar</span><span class="sxs-lookup"><span data-stu-id="f62ac-145"><a id="sshkey"></a>Authentication: SSH Keys</span></span>

<span data-ttu-id="f62ac-146">SSH-nycklar använda [offentliga nycklar](https://en.wikipedia.org/wiki/Public-key_cryptography) tooauthenticate SSH-sessioner.</span><span class="sxs-lookup"><span data-stu-id="f62ac-146">SSH keys use [Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) tooauthenticate SSH sessions.</span></span> <span data-ttu-id="f62ac-147">SSH-nycklar är säkrare än och ger ett enkelt sätt toosecure åtkomst tooyour Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="f62ac-147">SSH keys are more secure than passwords, and provide an easy way toosecure access tooyour Hadoop cluster.</span></span>

<span data-ttu-id="f62ac-148">Om SSH-konto är skyddad med en nyckel, måste hello klienten tillhandahålla hello matchar privata nyckel när du ansluter:</span><span class="sxs-lookup"><span data-stu-id="f62ac-148">If your SSH account is secured using a key, hello client must provide hello matching private key when you connect:</span></span>

* <span data-ttu-id="f62ac-149">De flesta klienter kan vara konfigurerade toouse en __standardnyckeln__.</span><span class="sxs-lookup"><span data-stu-id="f62ac-149">Most clients can be configured toouse a __default key__.</span></span> <span data-ttu-id="f62ac-150">Till exempel hello `ssh` klienten söker efter en privat nyckel vid `~/.ssh/id_rsa` på Linux och Unix-miljöer.</span><span class="sxs-lookup"><span data-stu-id="f62ac-150">For example, hello `ssh` client looks for a private key at `~/.ssh/id_rsa` on Linux and Unix environments.</span></span>

* <span data-ttu-id="f62ac-151">Du kan ange hello __sökväg tooa privata nyckeln__.</span><span class="sxs-lookup"><span data-stu-id="f62ac-151">You can specify hello __path tooa private key__.</span></span> <span data-ttu-id="f62ac-152">Med hello `ssh` klient, hello `-i` parameter är används toospecify hello sökvägen tooprivate nyckel.</span><span class="sxs-lookup"><span data-stu-id="f62ac-152">With hello `ssh` client, hello `-i` parameter is used toospecify hello path tooprivate key.</span></span> <span data-ttu-id="f62ac-153">Till exempel `ssh -i ~/.ssh/id_rsa sshuser@myedge.mycluster-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="f62ac-153">For example, `ssh -i ~/.ssh/id_rsa sshuser@myedge.mycluster-ssh.azurehdinsight.net`.</span></span>

* <span data-ttu-id="f62ac-154">Om du har __flera privata nycklar__ för användning med olika servrar kan verktyg som [ssh-agent (https://en.wikipedia.org/wiki/Ssh-agent)](https://en.wikipedia.org/wiki/Ssh-agent) användas.</span><span class="sxs-lookup"><span data-stu-id="f62ac-154">If you have __multiple private keys__ for use with different servers, consider using a utility such as [ssh-agent (https://en.wikipedia.org/wiki/Ssh-agent)](https://en.wikipedia.org/wiki/Ssh-agent).</span></span> <span data-ttu-id="f62ac-155">Hej `ssh-agent` verktyg kan vara används tooautomatically väljer hello viktiga toouse när en SSH-session.</span><span class="sxs-lookup"><span data-stu-id="f62ac-155">hello `ssh-agent` utility can be used tooautomatically select hello key toouse when establishing an SSH session.</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="f62ac-156">Om du skyddar din privata nyckel med en lösenfras måste du ange hello lösenfrasen när du använder hello nyckel.</span><span class="sxs-lookup"><span data-stu-id="f62ac-156">If you secure your private key with a passphrase, you must enter hello passphrase when using hello key.</span></span> <span data-ttu-id="f62ac-157">Verktyg som `ssh-agent` kan cachelagra hello lösenord för din bekvämlighet.</span><span class="sxs-lookup"><span data-stu-id="f62ac-157">Utilities such as `ssh-agent` can cache hello password for your convenience.</span></span>

### <a name="create-an-ssh-key-pair"></a><span data-ttu-id="f62ac-158">Skapa ett SSH-nyckelpar</span><span class="sxs-lookup"><span data-stu-id="f62ac-158">Create an SSH key pair</span></span>

<span data-ttu-id="f62ac-159">Använd hello `ssh-keygen` kommandot toocreate offentliga och privata nyckelfiler.</span><span class="sxs-lookup"><span data-stu-id="f62ac-159">Use hello `ssh-keygen` command toocreate public and private key files.</span></span> <span data-ttu-id="f62ac-160">hello skapar följande kommando en 2048-bitars RSA-nyckelpar som kan användas med HDInsight:</span><span class="sxs-lookup"><span data-stu-id="f62ac-160">hello following command generates a 2048-bit RSA key pair that can be used with HDInsight:</span></span>

    ssh-keygen -t rsa -b 2048

<span data-ttu-id="f62ac-161">Du uppmanas information under hello viktiga skapandeprocessen.</span><span class="sxs-lookup"><span data-stu-id="f62ac-161">You are prompted for information during hello key creation process.</span></span> <span data-ttu-id="f62ac-162">Till exempel där hello nycklar lagras eller om toouse en lösenfras.</span><span class="sxs-lookup"><span data-stu-id="f62ac-162">For example, where hello keys are stored or whether toouse a passphrase.</span></span> <span data-ttu-id="f62ac-163">När hello processen har slutförts, skapas två filer; en offentlig nyckel och en privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="f62ac-163">After hello process completes, two files are created; a public key and a private key.</span></span>

* <span data-ttu-id="f62ac-164">Hej __offentliga nyckel__ är används toocreate ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="f62ac-164">hello __public key__ is used toocreate an HDInsight cluster.</span></span> <span data-ttu-id="f62ac-165">hello offentliga nyckel har filnamnstillägget `.pub`.</span><span class="sxs-lookup"><span data-stu-id="f62ac-165">hello public key has an extension of `.pub`.</span></span>

* <span data-ttu-id="f62ac-166">Hej __privata nyckel__ är används tooauthenticate klienten toohello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="f62ac-166">hello __private key__ is used tooauthenticate your client toohello HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f62ac-167">Du kan skydda dina nycklar med hjälp av en lösenfras.</span><span class="sxs-lookup"><span data-stu-id="f62ac-167">You can secure your keys using a passphrase.</span></span> <span data-ttu-id="f62ac-168">Detta är ett lösenord för den privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="f62ac-168">A passphrase is effectively a password on your private key.</span></span> <span data-ttu-id="f62ac-169">Även om någon hämtar din privata nyckel, måste de ha hello lösenfras toouse hello nyckel.</span><span class="sxs-lookup"><span data-stu-id="f62ac-169">Even if someone obtains your private key, they must have hello passphrase toouse hello key.</span></span>

### <a name="create-hdinsight-using-hello-public-key"></a><span data-ttu-id="f62ac-170">Skapa HDInsight med hjälp av hello offentlig nyckel</span><span class="sxs-lookup"><span data-stu-id="f62ac-170">Create HDInsight using hello public key</span></span>

| <span data-ttu-id="f62ac-171">Genereringsmetod</span><span class="sxs-lookup"><span data-stu-id="f62ac-171">Creation method</span></span> | <span data-ttu-id="f62ac-172">Hur toouse hello offentlig nyckel</span><span class="sxs-lookup"><span data-stu-id="f62ac-172">How toouse hello public key</span></span> |
| ------- | ------- |
| <span data-ttu-id="f62ac-173">**Azure Portal**</span><span class="sxs-lookup"><span data-stu-id="f62ac-173">**Azure portal**</span></span> | <span data-ttu-id="f62ac-174">Avmarkera __använda samma lösenord som klusterinloggning__, och välj sedan __offentliga nyckel__ som hello SSH autentiseringstyp.</span><span class="sxs-lookup"><span data-stu-id="f62ac-174">Uncheck __Use same password as cluster login__, and then select __Public Key__ as hello SSH authentication type.</span></span> <span data-ttu-id="f62ac-175">Slutligen, Välj hello offentlig nyckelfil eller klistra in hello textinnehållet i hello-filen i hello __offentliga SSH-nyckeln__ fältet.</span><span class="sxs-lookup"><span data-stu-id="f62ac-175">Finally, select hello public key file or paste hello text contents of hello file in hello __SSH public key__ field.</span></span></br><span data-ttu-id="f62ac-176">![Dialogrutan Offentlig SSH-nyckel vid generering av HDInsight-kluster](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-public-key.png)</span><span class="sxs-lookup"><span data-stu-id="f62ac-176">![SSH public key dialog in HDInsight cluster creation](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-public-key.png)</span></span> |
| <span data-ttu-id="f62ac-177">**Azure PowerShell**</span><span class="sxs-lookup"><span data-stu-id="f62ac-177">**Azure PowerShell**</span></span> | <span data-ttu-id="f62ac-178">Använd hello `-SshPublicKey` parametern för hello `New-AzureRmHdinsightCluster` cmdlet- och pass hello innehållet i hello offentlig nyckel som en sträng.</span><span class="sxs-lookup"><span data-stu-id="f62ac-178">Use hello `-SshPublicKey` parameter of hello `New-AzureRmHdinsightCluster` cmdlet and pass hello contents of hello public key as a string.</span></span>|
| <span data-ttu-id="f62ac-179">**Azure CLI 1.0**</span><span class="sxs-lookup"><span data-stu-id="f62ac-179">**Azure CLI 1.0**</span></span> | <span data-ttu-id="f62ac-180">Använd hello `--sshPublicKey` parametern för hello `azure hdinsight cluster create` kommandot och skicka hello innehållet i hello offentlig nyckel som en sträng.</span><span class="sxs-lookup"><span data-stu-id="f62ac-180">Use hello `--sshPublicKey` parameter of hello `azure hdinsight cluster create` command and pass hello contents of hello public key as a string.</span></span> |
| <span data-ttu-id="f62ac-181">**Resource Manager-mall**</span><span class="sxs-lookup"><span data-stu-id="f62ac-181">**Resource Manager Template**</span></span> | <span data-ttu-id="f62ac-182">Ett exempel på hur du använder SSH-nycklar med en mall finns i avsnittet [Deploy HDInsight on Linux with SSH key](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-publickey/) (Distribuera HDInsight i Linux med en SSH-nyckel).</span><span class="sxs-lookup"><span data-stu-id="f62ac-182">For an example of using SSH keys with a template, see [Deploy HDInsight on Linux with SSH key](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-publickey/).</span></span> <span data-ttu-id="f62ac-183">Hej `publicKeys` element i hello [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-publickey/azuredeploy.json) filen har använt toopass hello nycklar tooAzure när du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="f62ac-183">hello `publicKeys` element in hello [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-publickey/azuredeploy.json) file is used toopass hello keys tooAzure when creating hello cluster.</span></span> |

## <span data-ttu-id="f62ac-184"><a id="sshpassword"></a>Autentisering: lösenord</span><span class="sxs-lookup"><span data-stu-id="f62ac-184"><a id="sshpassword"></a>Authentication: Password</span></span>

<span data-ttu-id="f62ac-185">SSH-konton kan skyddas med ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="f62ac-185">SSH accounts can be secured using a password.</span></span> <span data-ttu-id="f62ac-186">Ange tooenter hello lösenord anges när du ansluter tooHDInsight via SSH.</span><span class="sxs-lookup"><span data-stu-id="f62ac-186">When you connect tooHDInsight using SSH, you are prompted tooenter hello password.</span></span>

> [!WARNING]
> <span data-ttu-id="f62ac-187">Vi rekommenderar inte att du använder lösenordsautentisering för SSH.</span><span class="sxs-lookup"><span data-stu-id="f62ac-187">We do not recommend using password authentication for SSH.</span></span> <span data-ttu-id="f62ac-188">Lösenord kan knäckas och är sårbara toobrute force-attacker.</span><span class="sxs-lookup"><span data-stu-id="f62ac-188">Passwords can be guessed and are vulnerable toobrute force attacks.</span></span> <span data-ttu-id="f62ac-189">I stället rekommenderar vi att du använder [SSH-nycklar för autentisering](#sshkey).</span><span class="sxs-lookup"><span data-stu-id="f62ac-189">Instead, we recommend that you use [SSH keys for authentication](#sshkey).</span></span>

### <a name="create-hdinsight-using-a-password"></a><span data-ttu-id="f62ac-190">Skapa HDInsight med ett lösenord</span><span class="sxs-lookup"><span data-stu-id="f62ac-190">Create HDInsight using a password</span></span>

| <span data-ttu-id="f62ac-191">Genereringsmetod</span><span class="sxs-lookup"><span data-stu-id="f62ac-191">Creation method</span></span> | <span data-ttu-id="f62ac-192">Hur toospecify hello lösenord</span><span class="sxs-lookup"><span data-stu-id="f62ac-192">How toospecify hello password</span></span> |
| --------------- | ---------------- |
| <span data-ttu-id="f62ac-193">**Azure Portal**</span><span class="sxs-lookup"><span data-stu-id="f62ac-193">**Azure portal**</span></span> | <span data-ttu-id="f62ac-194">Hello SSH-användarkontot har som standard hello samma lösenord som hello inloggningskonto för klustret.</span><span class="sxs-lookup"><span data-stu-id="f62ac-194">By default, hello SSH user account has hello same password as hello cluster login account.</span></span> <span data-ttu-id="f62ac-195">Avmarkera toouse ett annat lösenord __använda samma lösenord som klusterinloggning__, och sedan ange hello lösenord i hello __SSH-lösenordet__ fältet.</span><span class="sxs-lookup"><span data-stu-id="f62ac-195">toouse a different password, uncheck __Use same password as cluster login__, and then enter hello password in hello __SSH password__ field.</span></span></br><span data-ttu-id="f62ac-196">![Dialogrutan SSH-lösenord när ett HDInsight-kluster skapas](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-password.png)</span><span class="sxs-lookup"><span data-stu-id="f62ac-196">![SSH password dialog in HDInsight cluster creation](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-password.png)</span></span>|
| <span data-ttu-id="f62ac-197">**Azure PowerShell**</span><span class="sxs-lookup"><span data-stu-id="f62ac-197">**Azure PowerShell**</span></span> | <span data-ttu-id="f62ac-198">Använd hello `--SshCredential` parametern för hello `New-AzureRmHdinsightCluster` cmdlet och skicka en `PSCredential` objekt som innehåller hello SSH användarens kontonamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="f62ac-198">Use hello `--SshCredential` parameter of hello `New-AzureRmHdinsightCluster` cmdlet and pass a `PSCredential` object that contains hello SSH user account name and password.</span></span> |
| <span data-ttu-id="f62ac-199">**Azure CLI 1.0**</span><span class="sxs-lookup"><span data-stu-id="f62ac-199">**Azure CLI 1.0**</span></span> | <span data-ttu-id="f62ac-200">Använd hello `--sshPassword` parametern för hello `azure hdinsight cluster create` kommando och ange värdet för hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="f62ac-200">Use hello `--sshPassword` parameter of hello `azure hdinsight cluster create` command and provide hello password value.</span></span> |
| <span data-ttu-id="f62ac-201">**Resource Manager-mall**</span><span class="sxs-lookup"><span data-stu-id="f62ac-201">**Resource Manager Template**</span></span> | <span data-ttu-id="f62ac-202">Ett exempel på hur du använder ett lösenord med en mall finns i [Deploy HDInsight on Linux with SSH password](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/) (Distribuera HDInsight i Linux med SSH-lösenord).</span><span class="sxs-lookup"><span data-stu-id="f62ac-202">For an example of using a password with a template, see [Deploy HDInsight on Linux with SSH password](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/).</span></span> <span data-ttu-id="f62ac-203">Hej `linuxOperatingSystemProfile` element i hello [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-password/azuredeploy.json) filen har använt toopass hello SSH-konto och lösenord tooAzure när du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="f62ac-203">hello `linuxOperatingSystemProfile` element in hello [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-password/azuredeploy.json) file is used toopass hello SSH account name and password tooAzure when creating hello cluster.</span></span>|

### <a name="change-hello-ssh-password"></a><span data-ttu-id="f62ac-204">Ändra hello SSH-lösenord</span><span class="sxs-lookup"><span data-stu-id="f62ac-204">Change hello SSH password</span></span>

<span data-ttu-id="f62ac-205">Information om hur du ändrar hello SSH lösenord finns hello __ändra lösenord__ avsnitt i hello [hantera HDInsight](hdinsight-administer-use-portal-linux.md#change-passwords) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f62ac-205">For information on changing hello SSH user account password, see hello __Change passwords__ section of hello [Manage HDInsight](hdinsight-administer-use-portal-linux.md#change-passwords) document.</span></span>

## <span data-ttu-id="f62ac-206"><a id="domainjoined"></a>Autentisering: Domänanslutet HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="f62ac-206"><a id="domainjoined"></a>Authentication: Domain-joined HDInsight</span></span>

<span data-ttu-id="f62ac-207">Om du använder en __domänanslutna HDInsight-kluster__, måste du använda hello `kinit` kommandot efter anslutning med SSH.</span><span class="sxs-lookup"><span data-stu-id="f62ac-207">If you are using a __domain-joined HDInsight cluster__, you must use hello `kinit` command after connecting with SSH.</span></span> <span data-ttu-id="f62ac-208">Det här kommandot uppmanar dig domänanvändare och lösenord och autentiserar sessionen med hello Azure Active Directory-domänen som är associerade med hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="f62ac-208">This command prompts you for a domain user and password, and authenticates your session with hello Azure Active Directory domain associated with hello cluster.</span></span>

<span data-ttu-id="f62ac-209">Mer information finns i avsnittet [Configure domain-joined HDInsight](hdinsight-domain-joined-configure.md) (Konfigurera domänanslutna HDInsight-kluster).</span><span class="sxs-lookup"><span data-stu-id="f62ac-209">For more information, see [Configure domain-joined HDInsight](hdinsight-domain-joined-configure.md).</span></span>

## <a name="connect-toonodes"></a><span data-ttu-id="f62ac-210">Ansluta toonodes</span><span class="sxs-lookup"><span data-stu-id="f62ac-210">Connect toonodes</span></span>

<span data-ttu-id="f62ac-211">Hej huvudnoderna och kantnod (om sådan finns) kan nås över hello internet på port 22 och 23.</span><span class="sxs-lookup"><span data-stu-id="f62ac-211">hello head nodes and edge node (if there is one) can be accessed over hello internet on ports 22 and 23.</span></span>

* <span data-ttu-id="f62ac-212">När du ansluter toohello __huvudnoder__, använda port __22__ tooconnect toohello primära head nod och port __23__ tooconnect toohello andra huvudnod.</span><span class="sxs-lookup"><span data-stu-id="f62ac-212">When connecting toohello __head nodes__, use port __22__ tooconnect toohello primary head node and port __23__ tooconnect toohello secondary head node.</span></span> <span data-ttu-id="f62ac-213">hello fullständigt kvalificerade namnet toouse är `clustername-ssh.azurehdinsight.net`, där `clustername` är hello namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="f62ac-213">hello fully qualified domain name toouse is `clustername-ssh.azurehdinsight.net`, where `clustername` is hello name of your cluster.</span></span>

    ```bash
    # Connect tooprimary head node
    # port not specified since 22 is hello default
    ssh sshuser@clustername-ssh.azurehdinsight.net

    # Connect toosecondary head node
    ssh -p 23 sshuser@clustername-ssh.azurehdinsight.net
    ```
    
* <span data-ttu-id="f62ac-214">När connectiung toohello __kantnoden__, använder port 22.</span><span class="sxs-lookup"><span data-stu-id="f62ac-214">When connectiung toohello __edge node__, use port 22.</span></span> <span data-ttu-id="f62ac-215">hello fullständigt kvalificerade domännamnet är `edgenodename.clustername-ssh.azurehdinsight.net`, där `edgenodename` är ett namn som du angav när du skapar hello kantnod.</span><span class="sxs-lookup"><span data-stu-id="f62ac-215">hello fully qualified domain name is `edgenodename.clustername-ssh.azurehdinsight.net`, where `edgenodename` is a name you provided when creating hello edge node.</span></span> <span data-ttu-id="f62ac-216">`clustername`är hello hello klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="f62ac-216">`clustername` is hello name of hello cluster.</span></span>

    ```bash
    # Connect tooedge node
    ssh sshuser@edgnodename.clustername-ssh.azurehdinsight.net
    ```

> [!IMPORTANT]
> <span data-ttu-id="f62ac-217">hello föregående exempel förutsätter att du använder en lösenordsautentisering eller att certifikatautentisering är utförs automatiskt.</span><span class="sxs-lookup"><span data-stu-id="f62ac-217">hello previous examples assume that you are using password authentication, or that certificate authentication is occuring automatically.</span></span> <span data-ttu-id="f62ac-218">Om du använder en SSH-nyckel för autentisering och används inte hello certifikatet automatiskt, använder hello `-i` parametern toospecify hello privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="f62ac-218">If you use an SSH key-pair for authentication, and hello certificate is not used automatically, use hello `-i` parameter toospecify hello private key.</span></span> <span data-ttu-id="f62ac-219">Till exempel `ssh -i ~/.ssh/mykey sshuser@clustername-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="f62ac-219">For example, `ssh -i ~/.ssh/mykey sshuser@clustername-ssh.azurehdinsight.net`.</span></span>

<span data-ttu-id="f62ac-220">När du är ansluten, ändras hello Kommandotolken tooindicate hello SSH användaren namnet hello noden och du är ansluten till.</span><span class="sxs-lookup"><span data-stu-id="f62ac-220">Once connected, hello prompt changes tooindicate hello SSH user name and hello node you are connected to.</span></span> <span data-ttu-id="f62ac-221">Till exempel när ansluten toohello primära huvudnod som `sshuser`, hello frågan är `sshuser@hn0-clustername:~$`.</span><span class="sxs-lookup"><span data-stu-id="f62ac-221">For example, when connected toohello primary head node as `sshuser`, hello prompt is `sshuser@hn0-clustername:~$`.</span></span>

### <a name="connect-tooworker-and-zookeeper-nodes"></a><span data-ttu-id="f62ac-222">Ansluta tooworker och Zookeeper-noder</span><span class="sxs-lookup"><span data-stu-id="f62ac-222">Connect tooworker and Zookeeper nodes</span></span>

<span data-ttu-id="f62ac-223">Hej arbetarnoder och Zookeeper noder inte är direkt åtkomliga från hello internet.</span><span class="sxs-lookup"><span data-stu-id="f62ac-223">hello worker nodes and Zookeeper nodes are not directly accessible from hello internet.</span></span> <span data-ttu-id="f62ac-224">De kan nås från hello-klustrets huvudnoder eller edge-noder.</span><span class="sxs-lookup"><span data-stu-id="f62ac-224">They can be accessed from hello cluster head nodes or edge nodes.</span></span> <span data-ttu-id="f62ac-225">hello följande är hello allmänna steg tooconnect tooother noder:</span><span class="sxs-lookup"><span data-stu-id="f62ac-225">hello following are hello general steps tooconnect tooother nodes:</span></span>

1. <span data-ttu-id="f62ac-226">Använd SSH tooconnect tooa head eller edge nod:</span><span class="sxs-lookup"><span data-stu-id="f62ac-226">Use SSH tooconnect tooa head or edge node:</span></span>

        ssh sshuser@myedge.mycluster-ssh.azurehdinsight.net

2. <span data-ttu-id="f62ac-227">Använd hello från hello SSH-anslutning toohello head eller kantnod `ssh` kommandot tooconnect tooa arbetsnoden hello klustret:</span><span class="sxs-lookup"><span data-stu-id="f62ac-227">From hello SSH connection toohello head or edge node, use hello `ssh` command tooconnect tooa worker node in hello cluster:</span></span>

        ssh sshuser@wn0-myhdi

    <span data-ttu-id="f62ac-228">tooretrieve en lista över hello domännamn för hello noder i klustret hello finns hello [hantera HDInsight med hjälp av hello Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f62ac-228">tooretrieve a list of hello domain names of hello nodes in hello cluster, see hello [Manage HDInsight by using hello Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) document.</span></span>

<span data-ttu-id="f62ac-229">Om hello SSH-konto är skyddad med en __lösenord__, ange hello lösenord när du ansluter.</span><span class="sxs-lookup"><span data-stu-id="f62ac-229">If hello SSH account is secured using a __password__, enter hello password when connecting.</span></span>

<span data-ttu-id="f62ac-230">Om hello SSH-konto är säkrad via __SSH-nycklar__, se till att SSH-vidarebefordran är aktiverad på hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="f62ac-230">If hello SSH account is secured using __SSH keys__, make sure that SSH forwarding is enabled on hello client.</span></span>

> [!NOTE]
> <span data-ttu-id="f62ac-231">Ett annat sätt toodirectly åtkomst till alla noder i klustret hello är tooinstall HDInsight i ett virtuellt Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="f62ac-231">Another way toodirectly access all nodes in hello cluster is tooinstall HDInsight into an Azure Virtual Network.</span></span> <span data-ttu-id="f62ac-232">Du kan sedan ansluta till fjärrdatorn-toohello samma virtuella nätverk och få åtkomst till alla noder i klustret hello direkt.</span><span class="sxs-lookup"><span data-stu-id="f62ac-232">Then, you can join your remote machine toohello same virtual network and directly access all nodes in hello cluster.</span></span>
>
> <span data-ttu-id="f62ac-233">Mer information finns i [Use a virtual network with HDInsight](hdinsight-extend-hadoop-virtual-network.md) (Använda ett virtuellt nätverk med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="f62ac-233">For more information, see [Use a virtual network with HDInsight](hdinsight-extend-hadoop-virtual-network.md).</span></span>

### <a name="configure-ssh-agent-forwarding"></a><span data-ttu-id="f62ac-234">Konfigurera vidarebefordran med SSH-agenten</span><span class="sxs-lookup"><span data-stu-id="f62ac-234">Configure SSH agent forwarding</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f62ac-235">hello följande steg förutsätter en Linux eller UNIX-baserat system och arbeta med Bash på Windows 10.</span><span class="sxs-lookup"><span data-stu-id="f62ac-235">hello following steps assume a Linux or UNIX-based system, and work with Bash on Windows 10.</span></span> <span data-ttu-id="f62ac-236">Om de här stegen inte fungerar för ditt system, eventuellt tooconsult hello dokumentationen för SSH-klienten.</span><span class="sxs-lookup"><span data-stu-id="f62ac-236">If these steps do not work for your system, you may need tooconsult hello documentation for your SSH client.</span></span>

1. <span data-ttu-id="f62ac-237">Använd en textredigerare och öppna `~/.ssh/config`.</span><span class="sxs-lookup"><span data-stu-id="f62ac-237">Using a text editor, open `~/.ssh/config`.</span></span> <span data-ttu-id="f62ac-238">Om den här filen inte finns kan du skapa den genom att ange `touch ~/.ssh/config` på en kommandorad.</span><span class="sxs-lookup"><span data-stu-id="f62ac-238">If this file doesn't exist, you can create it by entering `touch ~/.ssh/config` at a command line.</span></span>

2. <span data-ttu-id="f62ac-239">Lägg till följande text toohello hello `config` fil.</span><span class="sxs-lookup"><span data-stu-id="f62ac-239">Add hello following text toohello `config` file.</span></span>

        Host <edgenodename>.<clustername>-ssh.azurehdinsight.net
          ForwardAgent yes

    <span data-ttu-id="f62ac-240">Ersätt hello __värden__ information med hello adressen hello nod du ansluta toousing SSH.</span><span class="sxs-lookup"><span data-stu-id="f62ac-240">Replace hello __Host__ information with hello address of hello node you connect toousing SSH.</span></span> <span data-ttu-id="f62ac-241">hello föregående exemplet används hello kantnod.</span><span class="sxs-lookup"><span data-stu-id="f62ac-241">hello previous example uses hello edge node.</span></span> <span data-ttu-id="f62ac-242">Den här posten konfigurerar SSH-agentvidarebefordran för hello angivna noden.</span><span class="sxs-lookup"><span data-stu-id="f62ac-242">This entry configures SSH agent forwarding for hello specified node.</span></span>

3. <span data-ttu-id="f62ac-243">Testa SSH-agentvidarebefordran med hjälp av följande kommando från hello terminal hello:</span><span class="sxs-lookup"><span data-stu-id="f62ac-243">Test SSH agent forwarding by using hello following command from hello terminal:</span></span>

        echo "$SSH_AUTH_SOCK"

    <span data-ttu-id="f62ac-244">Det här kommandot returnerar informationen liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="f62ac-244">This command returns information similar toohello following text:</span></span>

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    <span data-ttu-id="f62ac-245">Om inget returneras så körs inte `ssh-agent`.</span><span class="sxs-lookup"><span data-stu-id="f62ac-245">If nothing is returned, then `ssh-agent` is not running.</span></span> <span data-ttu-id="f62ac-246">Mer information finns i hello agent startinformation skript på [med hjälp av ssh-agenten med ssh (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) eller Läs dokumentationen för SSH-klienten.</span><span class="sxs-lookup"><span data-stu-id="f62ac-246">For more information, see hello agent startup scripts information at [Using ssh-agent with ssh (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) or consult your SSH client documentation.</span></span>

4. <span data-ttu-id="f62ac-247">När du har verifierat att **ssh-agent** körs, Använd hello följande tooadd SSH privat nyckel toohello agent:</span><span class="sxs-lookup"><span data-stu-id="f62ac-247">Once you have verified that **ssh-agent** is running, use hello following tooadd your SSH private key toohello agent:</span></span>

        ssh-add ~/.ssh/id_rsa

    <span data-ttu-id="f62ac-248">Om din privata nyckel lagras i en annan fil ersätter `~/.ssh/id_rsa` med hello sökväg toohello fil.</span><span class="sxs-lookup"><span data-stu-id="f62ac-248">If your private key is stored in a different file, replace `~/.ssh/id_rsa` with hello path toohello file.</span></span>

5. <span data-ttu-id="f62ac-249">Anslut toohello klusternod kant eller huvudnoderna via SSH.</span><span class="sxs-lookup"><span data-stu-id="f62ac-249">Connect toohello cluster edge node or head nodes using SSH.</span></span> <span data-ttu-id="f62ac-250">Använd sedan hello SSH-kommandot tooconnect tooa worker eller zookeeper-nod.</span><span class="sxs-lookup"><span data-stu-id="f62ac-250">Then use hello SSH command tooconnect tooa worker or zookeeper node.</span></span> <span data-ttu-id="f62ac-251">hello-anslutning upprättas med hjälp av hello vidarebefordras nyckel.</span><span class="sxs-lookup"><span data-stu-id="f62ac-251">hello connection is established using hello forwarded key.</span></span>

## <a name="copy-files"></a><span data-ttu-id="f62ac-252">Kopiera filer</span><span class="sxs-lookup"><span data-stu-id="f62ac-252">Copy files</span></span>

<span data-ttu-id="f62ac-253">Hej `scp` verktyg kan vara används toocopy filer tooand från enskilda noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="f62ac-253">hello `scp` utility can be used toocopy files tooand from individual nodes in hello cluster.</span></span> <span data-ttu-id="f62ac-254">Till exempel hello efter kommandot kopior hello `test.txt` katalog från hello lokalt system toohello primära huvudnod:</span><span class="sxs-lookup"><span data-stu-id="f62ac-254">For example, hello following command copies hello `test.txt` directory from hello local system toohello primary head node:</span></span>

```bash
scp test.txt sshuser@clustername-ssh.azurehdinsight.net:
```

<span data-ttu-id="f62ac-255">Eftersom ingen sökväg anges efter hello `:`, hello filen placeras i hello `sshuser` -hemkataloger.</span><span class="sxs-lookup"><span data-stu-id="f62ac-255">Since no path is specified after hello `:`, hello file is placed in hello `sshuser` home directory.</span></span>

<span data-ttu-id="f62ac-256">följande exempel kopierar hello hello `test.txt` filen från hello `sshuser` arbetskatalog på hello primära huvudnod toohello lokalt system:</span><span class="sxs-lookup"><span data-stu-id="f62ac-256">hello following example copies hello `test.txt` file from hello `sshuser` home directory on hello primary head node toohello local system:</span></span>

```bash
scp sshuser@clustername-ssh.azurehdinsight.net:test.txt .
```

> [!IMPORTANT]
> <span data-ttu-id="f62ac-257">`scp`bara åt hello filsystemet på enskilda noder i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="f62ac-257">`scp` can only access hello file system of individual nodes within hello cluster.</span></span> <span data-ttu-id="f62ac-258">Inte kan använda tooaccess data i hello HDFS-kompatibla lagringen för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="f62ac-258">It cannot be used tooaccess data in hello HDFS-compatible storage for hello cluster.</span></span>
>
> <span data-ttu-id="f62ac-259">Använd `scp` när du behöver tooupload en resurs för användning från en SSH-session.</span><span class="sxs-lookup"><span data-stu-id="f62ac-259">Use `scp` when you need tooupload a resource for use from an SSH session.</span></span> <span data-ttu-id="f62ac-260">Till exempel överföra en Python-skriptet och kör sedan hello skript från en SSH-session.</span><span class="sxs-lookup"><span data-stu-id="f62ac-260">For example, upload a Python script and then run hello script from an SSH session.</span></span>
>
> <span data-ttu-id="f62ac-261">Information om att läsa in data direkt till hello HDFS-kompatibla lagring finns i hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="f62ac-261">For information on directly loading data into hello HDFS-compatible storage, see hello following documents:</span></span>
>
> * <span data-ttu-id="f62ac-262">[HDInsight med Azure Storage](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="f62ac-262">[HDInsight using Azure Storage](hdinsight-hadoop-use-blob-storage.md).</span></span>
>
> * <span data-ttu-id="f62ac-263">[HDInsight med Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md).</span><span class="sxs-lookup"><span data-stu-id="f62ac-263">[HDInsight using Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f62ac-264">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f62ac-264">Next steps</span></span>

* [<span data-ttu-id="f62ac-265">Använda SSH-tunnlar med HDInsight</span><span class="sxs-lookup"><span data-stu-id="f62ac-265">Use SSH tunneling with HDInsight</span></span>](hdinsight-linux-ambari-ssh-tunnel.md)
* [<span data-ttu-id="f62ac-266">Använda ett virtuellt nätverk med HDInsight</span><span class="sxs-lookup"><span data-stu-id="f62ac-266">Use a virtual network with HDInsight</span></span>](hdinsight-extend-hadoop-virtual-network.md)
* [<span data-ttu-id="f62ac-267">Använda kantnoder i HDInsight</span><span class="sxs-lookup"><span data-stu-id="f62ac-267">Use edge nodes in HDInsight</span></span>](hdinsight-apps-use-edge-node.md#access-an-edge-node)