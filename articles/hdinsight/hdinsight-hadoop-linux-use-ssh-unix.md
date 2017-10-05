---
title: "Använda SSH med Hadoop - Azure HDInsight | Microsoft Docs"
description: "Du kan komma åt HDInsight med hjälp av SSH (Secure Shell). Det här dokumentet innehåller information om hur du ansluter till HDInsight med hjälp av ssh- och scp-kommandon från Windows, Linux, Unix eller macOS klienter."
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
ms.openlocfilehash: df0feb51469333bac42c779d860192d46f24ac62
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="connect-to-hdinsight-hadoop-using-ssh"></a><span data-ttu-id="6a831-105">Ansluta till HDInsight (Hadoop) med hjälp av SSH</span><span class="sxs-lookup"><span data-stu-id="6a831-105">Connect to HDInsight (Hadoop) using SSH</span></span>

<span data-ttu-id="6a831-106">Lär dig hur du använder [SSH (Secure Shell)](https://en.wikipedia.org/wiki/Secure_Shell) för att ansluta till Hadoop på Azure HD Insight på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="6a831-106">Learn how to use [Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) to securely connect to Hadoop on Azure HDInsight.</span></span> 

<span data-ttu-id="6a831-107">HDInsight kan använda Linux (Ubuntu) som operativsystem för noder i Hadoop-klustret.</span><span class="sxs-lookup"><span data-stu-id="6a831-107">HDInsight can use Linux (Ubuntu) as the operating system for nodes within the Hadoop cluster.</span></span> <span data-ttu-id="6a831-108">Följande tabell innehåller adress- och portinformationen som behövs för att ansluta till Linux-baserad HDInsight med hjälp av en SSH-klient:</span><span class="sxs-lookup"><span data-stu-id="6a831-108">The following table contains the address and port information needed when connecting to Linux-based HDInsight using an SSH client:</span></span>

| <span data-ttu-id="6a831-109">Adress</span><span class="sxs-lookup"><span data-stu-id="6a831-109">Address</span></span> | <span data-ttu-id="6a831-110">Port</span><span class="sxs-lookup"><span data-stu-id="6a831-110">Port</span></span> | <span data-ttu-id="6a831-111">Ansluter till ...</span><span class="sxs-lookup"><span data-stu-id="6a831-111">Connects to...</span></span> |
| ----- | ----- | ----- |
| `<clustername>-ed-ssh.azurehdinsight.net` | <span data-ttu-id="6a831-112">22</span><span class="sxs-lookup"><span data-stu-id="6a831-112">22</span></span> | <span data-ttu-id="6a831-113">Kantnod (R Server på HDInsight)</span><span class="sxs-lookup"><span data-stu-id="6a831-113">Edge node (R Server on HDInsight)</span></span> |
| `<edgenodename>.<clustername>-ssh.azurehdinsight.net` | <span data-ttu-id="6a831-114">22</span><span class="sxs-lookup"><span data-stu-id="6a831-114">22</span></span> | <span data-ttu-id="6a831-115">Kantnod (alla andra klustertyper, om det finns en kantnod)</span><span class="sxs-lookup"><span data-stu-id="6a831-115">Edge node (any other cluster type, if an edge node exists)</span></span> |
| `<clustername>-ssh.azurehdinsight.net` | <span data-ttu-id="6a831-116">22</span><span class="sxs-lookup"><span data-stu-id="6a831-116">22</span></span> | <span data-ttu-id="6a831-117">Den primära huvudnoden</span><span class="sxs-lookup"><span data-stu-id="6a831-117">Primary headnode</span></span> |
| `<clustername>-ssh.azurehdinsight.net` | <span data-ttu-id="6a831-118">23</span><span class="sxs-lookup"><span data-stu-id="6a831-118">23</span></span> | <span data-ttu-id="6a831-119">Den sekundära huvudnoden</span><span class="sxs-lookup"><span data-stu-id="6a831-119">Secondary headnode</span></span> |

> [!NOTE]
> <span data-ttu-id="6a831-120">Ersätt `<edgenodename>` med namnet på kantnoden.</span><span class="sxs-lookup"><span data-stu-id="6a831-120">Replace `<edgenodename>` with the name of the edge node.</span></span>
>
> <span data-ttu-id="6a831-121">Ersätt `<clustername>` med namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="6a831-121">Replace `<clustername>` with the name of your cluster.</span></span>
>
> <span data-ttu-id="6a831-122">Om klustret innehåller en kantnod, rekommenderar vi att du __alltid ansluter till kantnoden__ via SSH.</span><span class="sxs-lookup"><span data-stu-id="6a831-122">If your cluster contains an edge node, we recommend that you __always connect to the edge node__ using SSH.</span></span> <span data-ttu-id="6a831-123">Värdtjänster för huvudnoder är viktiga för Hadoops hälsotillstånd.</span><span class="sxs-lookup"><span data-stu-id="6a831-123">The head nodes host services that are critical to the health of Hadoop.</span></span> <span data-ttu-id="6a831-124">Kantnoden kör bara det som du placerar på den.</span><span class="sxs-lookup"><span data-stu-id="6a831-124">The edge node runs only what you put on it.</span></span>
>
> <span data-ttu-id="6a831-125">Mer information om hur du använder kantnoder finns i [Använda kantnoder i HDInsight](hdinsight-apps-use-edge-node.md#access-an-edge-node).</span><span class="sxs-lookup"><span data-stu-id="6a831-125">For more information on using edge nodes, see [Use edge nodes in HDInsight](hdinsight-apps-use-edge-node.md#access-an-edge-node).</span></span>

## <a name="ssh-clients"></a><span data-ttu-id="6a831-126">SSH-klienter</span><span class="sxs-lookup"><span data-stu-id="6a831-126">SSH clients</span></span>

<span data-ttu-id="6a831-127">Linux, Unix- och macOS system ger kommandon `ssh` och `scp`.</span><span class="sxs-lookup"><span data-stu-id="6a831-127">Linux, Unix, and macOS systems provide the `ssh` and `scp` commands.</span></span> <span data-ttu-id="6a831-128">Klienten `ssh` används ofta för att skapa en fjärrsession med kommandoradsverktyget med Linux eller Unix-baserade system.</span><span class="sxs-lookup"><span data-stu-id="6a831-128">The `ssh` client is commonly used to create a remote command-line session with a Linux or Unix-based system.</span></span> <span data-ttu-id="6a831-129">Klienten `scp` används för att kopiera filer mellan klienten och fjärrdatorn på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="6a831-129">The `scp` client is used to securely copy files between your client and the remote system.</span></span>

<span data-ttu-id="6a831-130">Microsoft Windows tillhandahåller ingen SSH-klient som standard.</span><span class="sxs-lookup"><span data-stu-id="6a831-130">Microsoft Windows does not provide any SSH clients by default.</span></span> <span data-ttu-id="6a831-131">Klienterna `ssh` och `scp` är tillgängliga för Windows via följande paket:</span><span class="sxs-lookup"><span data-stu-id="6a831-131">The `ssh` and `scp` clients are available for Windows through the following packages:</span></span>

* <span data-ttu-id="6a831-132">[Azure Cloud Shell](../cloud-shell/quickstart.md): Cloud Shell tillhandahåller en Bash-miljö i webbläsaren och tillhandahåller `ssh`, `scp`, och andra vanliga Linux-kommandon.</span><span class="sxs-lookup"><span data-stu-id="6a831-132">[Azure Cloud Shell](../cloud-shell/quickstart.md): The Cloud Shell provides a Bash environment in your browser, and provides the `ssh`, `scp`, and other common Linux commands.</span></span>

* <span data-ttu-id="6a831-133">[Bash i Ubuntu för Windows 10](https://msdn.microsoft.com/commandline/wsl/about): `ssh`- och `scp`-kommandot är tillgängligt via Bash för Windows-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="6a831-133">[Bash on Ubuntu on Windows 10](https://msdn.microsoft.com/commandline/wsl/about): The `ssh` and `scp` commands are available through the Bash on Windows command line.</span></span>

* <span data-ttu-id="6a831-134">[Git (https://git-scm.com/)](https://git-scm.com/): `ssh` och `scp`-kommandot är tillgängligt via GitBash-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="6a831-134">[Git (https://git-scm.com/)](https://git-scm.com/): The `ssh` and `scp` commands are available through the GitBash command line.</span></span>

* <span data-ttu-id="6a831-135">[GitHub Desktop (https://desktop.github.com/)](https://desktop.github.com/) `ssh` och `scp`-kommandot är tillgängligt via GitHub Shell-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="6a831-135">[GitHub Desktop (https://desktop.github.com/)](https://desktop.github.com/) The `ssh` and `scp` commands are available through the GitHub Shell command line.</span></span> <span data-ttu-id="6a831-136">GitHub Desktop kan konfigureras att använda Bash, Windows-kommandotolken eller PowerShell som kommandorad för Git Shell.</span><span class="sxs-lookup"><span data-stu-id="6a831-136">GitHub Desktop can be configured to use Bash, the Windows Command Prompt, or PowerShell as the command line for the Git Shell.</span></span>

* <span data-ttu-id="6a831-137">[OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH): PowerShell-teamet porterar OpenSSH till Windows och tillhandahåller testutgåvor.</span><span class="sxs-lookup"><span data-stu-id="6a831-137">[OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH): The PowerShell team is porting OpenSSH to Windows, and provides test releases.</span></span>

    > [!WARNING]
    > <span data-ttu-id="6a831-138">OpenSSH-paketet innehåller SSH-serverkomponenten, `sshd`.</span><span class="sxs-lookup"><span data-stu-id="6a831-138">The OpenSSH package includes the SSH server component, `sshd`.</span></span> <span data-ttu-id="6a831-139">Den här komponenten startar en SSH-server på din dator så att andra kan ansluta till den.</span><span class="sxs-lookup"><span data-stu-id="6a831-139">This component starts an SSH server on your system, allowing others to connect to it.</span></span> <span data-ttu-id="6a831-140">Konfigurera inte den här komponenten och öppna inte port 22 om du inte vill ha en SSH-server på din dator.</span><span class="sxs-lookup"><span data-stu-id="6a831-140">Do not configure this component or open port 22 unless you want to host an SSH server on your system.</span></span> <span data-ttu-id="6a831-141">Det krävs inte för att kommunicera med HDInsight.</span><span class="sxs-lookup"><span data-stu-id="6a831-141">It is not required to communicate with HDInsight.</span></span>

<span data-ttu-id="6a831-142">Det finns också flera grafiska SSH-klienter, till exempel [PuTTY (http://www.chiark.greenend.org.uk/~sgtatham/putty/)](http://www.chiark.greenend.org.uk/~sgtatham/putty/) och [MobaXterm (http://mobaxterm.mobatek.net/)](http://mobaxterm.mobatek.net/).</span><span class="sxs-lookup"><span data-stu-id="6a831-142">There are also several graphical SSH clients, such as [PuTTY (http://www.chiark.greenend.org.uk/~sgtatham/putty/)](http://www.chiark.greenend.org.uk/~sgtatham/putty/) and [MobaXterm (http://mobaxterm.mobatek.net/)](http://mobaxterm.mobatek.net/).</span></span> <span data-ttu-id="6a831-143">Dessa klienter kan användas för att ansluta till HDInsight, men processen för att ansluta skiljer sig från anslutningsprocessen med `ssh`-verktyget.</span><span class="sxs-lookup"><span data-stu-id="6a831-143">While these clients can be used to connect to HDInsight, the process of connecting is different than using the `ssh` utility.</span></span> <span data-ttu-id="6a831-144">Mer information finns i dokumentationen för den grafiska klient som du använder.</span><span class="sxs-lookup"><span data-stu-id="6a831-144">For more information, see the documentation of the graphical client you are using.</span></span>

## <span data-ttu-id="6a831-145"><a id="sshkey"></a>Autentisering: SSH-nycklar</span><span class="sxs-lookup"><span data-stu-id="6a831-145"><a id="sshkey"></a>Authentication: SSH Keys</span></span>

<span data-ttu-id="6a831-146">SSH-nycklar använder [kryptografik med offentliga nycklar](https://en.wikipedia.org/wiki/Public-key_cryptography) för att autentisera SSH-sessioner.</span><span class="sxs-lookup"><span data-stu-id="6a831-146">SSH keys use [Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) to authenticate SSH sessions.</span></span> <span data-ttu-id="6a831-147">SSH-nycklar är säkrare än lösenord och är ett enkelt sätt att skydda Hadoop-klustret.</span><span class="sxs-lookup"><span data-stu-id="6a831-147">SSH keys are more secure than passwords, and provide an easy way to secure access to your Hadoop cluster.</span></span>

<span data-ttu-id="6a831-148">Om ditt SSH-konto skyddas med en nyckel måste klienten tillhandahålla den matchande privata nyckeln när du ansluter:</span><span class="sxs-lookup"><span data-stu-id="6a831-148">If your SSH account is secured using a key, the client must provide the matching private key when you connect:</span></span>

* <span data-ttu-id="6a831-149">De flesta klienter kan konfigureras att använda en __standardnyckel__.</span><span class="sxs-lookup"><span data-stu-id="6a831-149">Most clients can be configured to use a __default key__.</span></span> <span data-ttu-id="6a831-150">Exempelvis söker `ssh`-klienten efter en privat nyckel i `~/.ssh/id_rsa` i Linux- och Unix-miljöer.</span><span class="sxs-lookup"><span data-stu-id="6a831-150">For example, the `ssh` client looks for a private key at `~/.ssh/id_rsa` on Linux and Unix environments.</span></span>

* <span data-ttu-id="6a831-151">Du kan ange __sökvägen till en privat nyckel__.</span><span class="sxs-lookup"><span data-stu-id="6a831-151">You can specify the __path to a private key__.</span></span> <span data-ttu-id="6a831-152">Med `ssh`-klienten används `-i`-parametern för att ange sökvägen till den privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="6a831-152">With the `ssh` client, the `-i` parameter is used to specify the path to private key.</span></span> <span data-ttu-id="6a831-153">Till exempel `ssh -i ~/.ssh/id_rsa sshuser@myedge.mycluster-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="6a831-153">For example, `ssh -i ~/.ssh/id_rsa sshuser@myedge.mycluster-ssh.azurehdinsight.net`.</span></span>

* <span data-ttu-id="6a831-154">Om du har __flera privata nycklar__ för användning med olika servrar kan verktyg som [ssh-agent (https://en.wikipedia.org/wiki/Ssh-agent)](https://en.wikipedia.org/wiki/Ssh-agent) användas.</span><span class="sxs-lookup"><span data-stu-id="6a831-154">If you have __multiple private keys__ for use with different servers, consider using a utility such as [ssh-agent (https://en.wikipedia.org/wiki/Ssh-agent)](https://en.wikipedia.org/wiki/Ssh-agent).</span></span> <span data-ttu-id="6a831-155">Verktyget `ssh-agent` kan användas för att automatiskt välja nyckeln som ska användas när en SSH-session etableras.</span><span class="sxs-lookup"><span data-stu-id="6a831-155">The `ssh-agent` utility can be used to automatically select the key to use when establishing an SSH session.</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="6a831-156">Om du skyddar den privata nyckeln med en lösenfras måste du ange lösenfrasen när du använder nyckeln.</span><span class="sxs-lookup"><span data-stu-id="6a831-156">If you secure your private key with a passphrase, you must enter the passphrase when using the key.</span></span> <span data-ttu-id="6a831-157">Verktyg som `ssh-agent` kan underlätta för dig genom att cachelagra lösenordet.</span><span class="sxs-lookup"><span data-stu-id="6a831-157">Utilities such as `ssh-agent` can cache the password for your convenience.</span></span>

### <a name="create-an-ssh-key-pair"></a><span data-ttu-id="6a831-158">Skapa ett SSH-nyckelpar</span><span class="sxs-lookup"><span data-stu-id="6a831-158">Create an SSH key pair</span></span>

<span data-ttu-id="6a831-159">Använd `ssh-keygen`-kommandot för att skapa filer för offentliga och privata nycklar.</span><span class="sxs-lookup"><span data-stu-id="6a831-159">Use the `ssh-keygen` command to create public and private key files.</span></span> <span data-ttu-id="6a831-160">Följande kommando genererar ett 2048-bitars RSA-nyckelpar som kan användas med HDInsight:</span><span class="sxs-lookup"><span data-stu-id="6a831-160">The following command generates a 2048-bit RSA key pair that can be used with HDInsight:</span></span>

    ssh-keygen -t rsa -b 2048

<span data-ttu-id="6a831-161">Du uppmanas att ange information när nyckeln skapas.</span><span class="sxs-lookup"><span data-stu-id="6a831-161">You are prompted for information during the key creation process.</span></span> <span data-ttu-id="6a831-162">Till exempel var nycklarna lagras eller om du vill använda en lösenfras.</span><span class="sxs-lookup"><span data-stu-id="6a831-162">For example, where the keys are stored or whether to use a passphrase.</span></span> <span data-ttu-id="6a831-163">När processen har slutförts har två filer skapats: en offentlig nyckel och en privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="6a831-163">After the process completes, two files are created; a public key and a private key.</span></span>

* <span data-ttu-id="6a831-164">Den __offentliga nyckeln__ används för att skapa ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="6a831-164">The __public key__ is used to create an HDInsight cluster.</span></span> <span data-ttu-id="6a831-165">Den offentliga nyckeln har filnamnstillägget `.pub`.</span><span class="sxs-lookup"><span data-stu-id="6a831-165">The public key has an extension of `.pub`.</span></span>

* <span data-ttu-id="6a831-166">Den __privata nyckeln__ används för att autentisera din klient mot HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="6a831-166">The __private key__ is used to authenticate your client to the HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6a831-167">Du kan skydda dina nycklar med hjälp av en lösenfras.</span><span class="sxs-lookup"><span data-stu-id="6a831-167">You can secure your keys using a passphrase.</span></span> <span data-ttu-id="6a831-168">Detta är ett lösenord för den privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="6a831-168">A passphrase is effectively a password on your private key.</span></span> <span data-ttu-id="6a831-169">Även om någon får tag på din privata nyckel behöver de lösenfrasen för att kunna använda nyckeln.</span><span class="sxs-lookup"><span data-stu-id="6a831-169">Even if someone obtains your private key, they must have the passphrase to use the key.</span></span>

### <a name="create-hdinsight-using-the-public-key"></a><span data-ttu-id="6a831-170">Skapa HDInsight med hjälp av den offentliga nyckeln</span><span class="sxs-lookup"><span data-stu-id="6a831-170">Create HDInsight using the public key</span></span>

| <span data-ttu-id="6a831-171">Genereringsmetod</span><span class="sxs-lookup"><span data-stu-id="6a831-171">Creation method</span></span> | <span data-ttu-id="6a831-172">Så här använder du den offentliga nyckeln</span><span class="sxs-lookup"><span data-stu-id="6a831-172">How to use the public key</span></span> |
| ------- | ------- |
| <span data-ttu-id="6a831-173">**Azure Portal**</span><span class="sxs-lookup"><span data-stu-id="6a831-173">**Azure portal**</span></span> | <span data-ttu-id="6a831-174">Avmarkera __Använd samma lösenord som klusterinloggning__ och välj sedan __Offentlig nyckel__ som SSH-autentiseringstyp.</span><span class="sxs-lookup"><span data-stu-id="6a831-174">Uncheck __Use same password as cluster login__, and then select __Public Key__ as the SSH authentication type.</span></span> <span data-ttu-id="6a831-175">Välj slutligen filen för den offentliga nyckeln eller klistra in textinnehållet från filen i fältet __Offentlig SSH-nyckel__.</span><span class="sxs-lookup"><span data-stu-id="6a831-175">Finally, select the public key file or paste the text contents of the file in the __SSH public key__ field.</span></span></br><span data-ttu-id="6a831-176">![Dialogrutan Offentlig SSH-nyckel vid generering av HDInsight-kluster](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-public-key.png)</span><span class="sxs-lookup"><span data-stu-id="6a831-176">![SSH public key dialog in HDInsight cluster creation](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-public-key.png)</span></span> |
| <span data-ttu-id="6a831-177">**Azure PowerShell**</span><span class="sxs-lookup"><span data-stu-id="6a831-177">**Azure PowerShell**</span></span> | <span data-ttu-id="6a831-178">Använd `-SshPublicKey`-parametern för cmdleten `New-AzureRmHdinsightCluster` och skicka innehållet i den offentliga nyckeln som en sträng.</span><span class="sxs-lookup"><span data-stu-id="6a831-178">Use the `-SshPublicKey` parameter of the `New-AzureRmHdinsightCluster` cmdlet and pass the contents of the public key as a string.</span></span>|
| <span data-ttu-id="6a831-179">**Azure CLI 1.0**</span><span class="sxs-lookup"><span data-stu-id="6a831-179">**Azure CLI 1.0**</span></span> | <span data-ttu-id="6a831-180">Använd `--sshPublicKey`-parametern för kommandot `azure hdinsight cluster create` och skicka innehållet i den offentliga nyckeln som en sträng.</span><span class="sxs-lookup"><span data-stu-id="6a831-180">Use the `--sshPublicKey` parameter of the `azure hdinsight cluster create` command and pass the contents of the public key as a string.</span></span> |
| <span data-ttu-id="6a831-181">**Resource Manager-mall**</span><span class="sxs-lookup"><span data-stu-id="6a831-181">**Resource Manager Template**</span></span> | <span data-ttu-id="6a831-182">Ett exempel på hur du använder SSH-nycklar med en mall finns i avsnittet [Deploy HDInsight on Linux with SSH key](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-publickey/) (Distribuera HDInsight i Linux med en SSH-nyckel).</span><span class="sxs-lookup"><span data-stu-id="6a831-182">For an example of using SSH keys with a template, see [Deploy HDInsight on Linux with SSH key](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-publickey/).</span></span> <span data-ttu-id="6a831-183">`publicKeys`-elementet i filen [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-publickey/azuredeploy.json) används för att skicka nycklarna till Azure när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="6a831-183">The `publicKeys` element in the [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-publickey/azuredeploy.json) file is used to pass the keys to Azure when creating the cluster.</span></span> |

## <span data-ttu-id="6a831-184"><a id="sshpassword"></a>Autentisering: lösenord</span><span class="sxs-lookup"><span data-stu-id="6a831-184"><a id="sshpassword"></a>Authentication: Password</span></span>

<span data-ttu-id="6a831-185">SSH-konton kan skyddas med ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="6a831-185">SSH accounts can be secured using a password.</span></span> <span data-ttu-id="6a831-186">När du ansluter till HDInsight med hjälp av SSH uppmanas du att ange lösenordet.</span><span class="sxs-lookup"><span data-stu-id="6a831-186">When you connect to HDInsight using SSH, you are prompted to enter the password.</span></span>

> [!WARNING]
> <span data-ttu-id="6a831-187">Vi rekommenderar inte att du använder lösenordsautentisering för SSH.</span><span class="sxs-lookup"><span data-stu-id="6a831-187">We do not recommend using password authentication for SSH.</span></span> <span data-ttu-id="6a831-188">Lösenord kan gissas och är sårbara för råstyrkeattacker.</span><span class="sxs-lookup"><span data-stu-id="6a831-188">Passwords can be guessed and are vulnerable to brute force attacks.</span></span> <span data-ttu-id="6a831-189">I stället rekommenderar vi att du använder [SSH-nycklar för autentisering](#sshkey).</span><span class="sxs-lookup"><span data-stu-id="6a831-189">Instead, we recommend that you use [SSH keys for authentication](#sshkey).</span></span>

### <a name="create-hdinsight-using-a-password"></a><span data-ttu-id="6a831-190">Skapa HDInsight med ett lösenord</span><span class="sxs-lookup"><span data-stu-id="6a831-190">Create HDInsight using a password</span></span>

| <span data-ttu-id="6a831-191">Genereringsmetod</span><span class="sxs-lookup"><span data-stu-id="6a831-191">Creation method</span></span> | <span data-ttu-id="6a831-192">Så här anger du lösenordet</span><span class="sxs-lookup"><span data-stu-id="6a831-192">How to specify the password</span></span> |
| --------------- | ---------------- |
| <span data-ttu-id="6a831-193">**Azure Portal**</span><span class="sxs-lookup"><span data-stu-id="6a831-193">**Azure portal**</span></span> | <span data-ttu-id="6a831-194">SSH-användarkontot har som standard samma lösenord som kontot för klusterinloggning.</span><span class="sxs-lookup"><span data-stu-id="6a831-194">By default, the SSH user account has the same password as the cluster login account.</span></span> <span data-ttu-id="6a831-195">Om du vill använda ett annat lösenord avmarkerar du __Använd samma lösenord som klusterinloggning__ och anger sedan lösenordet i fältet __SSH-lösenord__.</span><span class="sxs-lookup"><span data-stu-id="6a831-195">To use a different password, uncheck __Use same password as cluster login__, and then enter the password in the __SSH password__ field.</span></span></br><span data-ttu-id="6a831-196">![Dialogrutan SSH-lösenord när ett HDInsight-kluster skapas](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-password.png)</span><span class="sxs-lookup"><span data-stu-id="6a831-196">![SSH password dialog in HDInsight cluster creation](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-password.png)</span></span>|
| <span data-ttu-id="6a831-197">**Azure PowerShell**</span><span class="sxs-lookup"><span data-stu-id="6a831-197">**Azure PowerShell**</span></span> | <span data-ttu-id="6a831-198">Använd `--SshCredential`-parametern för cmdleten `New-AzureRmHdinsightCluster` och skicka ett `PSCredential`-objekt som innehåller SSH-användarkontonamnet och SSH-lösenordet.</span><span class="sxs-lookup"><span data-stu-id="6a831-198">Use the `--SshCredential` parameter of the `New-AzureRmHdinsightCluster` cmdlet and pass a `PSCredential` object that contains the SSH user account name and password.</span></span> |
| <span data-ttu-id="6a831-199">**Azure CLI 1.0**</span><span class="sxs-lookup"><span data-stu-id="6a831-199">**Azure CLI 1.0**</span></span> | <span data-ttu-id="6a831-200">Använd `--sshPassword`-parametern för `azure hdinsight cluster create`-kommandot och ange lösenordsvärdet.</span><span class="sxs-lookup"><span data-stu-id="6a831-200">Use the `--sshPassword` parameter of the `azure hdinsight cluster create` command and provide the password value.</span></span> |
| <span data-ttu-id="6a831-201">**Resource Manager-mall**</span><span class="sxs-lookup"><span data-stu-id="6a831-201">**Resource Manager Template**</span></span> | <span data-ttu-id="6a831-202">Ett exempel på hur du använder ett lösenord med en mall finns i [Deploy HDInsight on Linux with SSH password](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/) (Distribuera HDInsight i Linux med SSH-lösenord).</span><span class="sxs-lookup"><span data-stu-id="6a831-202">For an example of using a password with a template, see [Deploy HDInsight on Linux with SSH password](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/).</span></span> <span data-ttu-id="6a831-203">`linuxOperatingSystemProfile`-elementet i filen [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-password/azuredeploy.json) används för att skicka SSH-kontonamnet och SSH-lösenordet till Azure när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="6a831-203">The `linuxOperatingSystemProfile` element in the [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-password/azuredeploy.json) file is used to pass the SSH account name and password to Azure when creating the cluster.</span></span>|

### <a name="change-the-ssh-password"></a><span data-ttu-id="6a831-204">Ändra SSH-lösenordet</span><span class="sxs-lookup"><span data-stu-id="6a831-204">Change the SSH password</span></span>

<span data-ttu-id="6a831-205">Information om hur du ändrar lösenordet för SSH-användarkontot finns i avsnittet __Change passwords__ (Ändra lösenord) i dokumentet [Manage HDInsight](hdinsight-administer-use-portal-linux.md#change-passwords) (Hantera HDInsight).</span><span class="sxs-lookup"><span data-stu-id="6a831-205">For information on changing the SSH user account password, see the __Change passwords__ section of the [Manage HDInsight](hdinsight-administer-use-portal-linux.md#change-passwords) document.</span></span>

## <span data-ttu-id="6a831-206"><a id="domainjoined"></a>Autentisering: Domänanslutet HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="6a831-206"><a id="domainjoined"></a>Authentication: Domain-joined HDInsight</span></span>

<span data-ttu-id="6a831-207">Om du använder ett __domänanslutet HDInsight-kluster__ måste du använda `kinit`-kommandot efter anslutning med SSH.</span><span class="sxs-lookup"><span data-stu-id="6a831-207">If you are using a __domain-joined HDInsight cluster__, you must use the `kinit` command after connecting with SSH.</span></span> <span data-ttu-id="6a831-208">Det här kommandot frågar efter en domänanvändare och ett lösenord och autentiserar din session med Azure Active Directory-domänen som är associerad med klustret.</span><span class="sxs-lookup"><span data-stu-id="6a831-208">This command prompts you for a domain user and password, and authenticates your session with the Azure Active Directory domain associated with the cluster.</span></span>

<span data-ttu-id="6a831-209">Mer information finns i avsnittet [Configure domain-joined HDInsight](hdinsight-domain-joined-configure.md) (Konfigurera domänanslutna HDInsight-kluster).</span><span class="sxs-lookup"><span data-stu-id="6a831-209">For more information, see [Configure domain-joined HDInsight](hdinsight-domain-joined-configure.md).</span></span>

## <a name="connect-to-nodes"></a><span data-ttu-id="6a831-210">Anslut till noder</span><span class="sxs-lookup"><span data-stu-id="6a831-210">Connect to nodes</span></span>

<span data-ttu-id="6a831-211">Huvudnoderna och kantnoden (om sådan finns) kan nås via Internet på port 22 och 23.</span><span class="sxs-lookup"><span data-stu-id="6a831-211">The head nodes and edge node (if there is one) can be accessed over the internet on ports 22 and 23.</span></span>

* <span data-ttu-id="6a831-212">När du ansluter till __huvudnoder__ använder du __22__ för att ansluta till den primära huvudnoden, och port __23__ för att ansluta till den sekundära huvudnoden.</span><span class="sxs-lookup"><span data-stu-id="6a831-212">When connecting to the __head nodes__, use port __22__ to connect to the primary head node and port __23__ to connect to the secondary head node.</span></span> <span data-ttu-id="6a831-213">Det fullständiga domännamnet som ska användas är `clustername-ssh.azurehdinsight.net`, där `clustername` är namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="6a831-213">The fully qualified domain name to use is `clustername-ssh.azurehdinsight.net`, where `clustername` is the name of your cluster.</span></span>

    ```bash
    # Connect to primary head node
    # port not specified since 22 is the default
    ssh sshuser@clustername-ssh.azurehdinsight.net

    # Connect to secondary head node
    ssh -p 23 sshuser@clustername-ssh.azurehdinsight.net
    ```
    
* <span data-ttu-id="6a831-214">När du ansluter till __kantnoden__ använder du port 22.</span><span class="sxs-lookup"><span data-stu-id="6a831-214">When connectiung to the __edge node__, use port 22.</span></span> <span data-ttu-id="6a831-215">Det fullständiga domännamnet är `edgenodename.clustername-ssh.azurehdinsight.net`, där `edgenodename` är det namn du angav när du skapade kantnoden.</span><span class="sxs-lookup"><span data-stu-id="6a831-215">The fully qualified domain name is `edgenodename.clustername-ssh.azurehdinsight.net`, where `edgenodename` is a name you provided when creating the edge node.</span></span> <span data-ttu-id="6a831-216">`clustername` är namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="6a831-216">`clustername` is the name of the cluster.</span></span>

    ```bash
    # Connect to edge node
    ssh sshuser@edgnodename.clustername-ssh.azurehdinsight.net
    ```

> [!IMPORTANT]
> <span data-ttu-id="6a831-217">I föregående exempel förutsätts att du använder lösenordsautentisering eller att certifikatautentisering sker automatiskt.</span><span class="sxs-lookup"><span data-stu-id="6a831-217">The previous examples assume that you are using password authentication, or that certificate authentication is occuring automatically.</span></span> <span data-ttu-id="6a831-218">Om du använder ett SSH-nyckelpar för autentisering och certifikatet inte används automatiskt, anger du den privata nyckeln med parametern `-i`.</span><span class="sxs-lookup"><span data-stu-id="6a831-218">If you use an SSH key-pair for authentication, and the certificate is not used automatically, use the `-i` parameter to specify the private key.</span></span> <span data-ttu-id="6a831-219">Till exempel `ssh -i ~/.ssh/mykey sshuser@clustername-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="6a831-219">For example, `ssh -i ~/.ssh/mykey sshuser@clustername-ssh.azurehdinsight.net`.</span></span>

<span data-ttu-id="6a831-220">När du är ansluten ändras fönstret till att visa SSH-användarnamnet och den nod du är ansluten till.</span><span class="sxs-lookup"><span data-stu-id="6a831-220">Once connected, the prompt changes to indicate the SSH user name and the node you are connected to.</span></span> <span data-ttu-id="6a831-221">När du exempelvis är ansluten till den primära huvudnoden som `sshuser` visar fönstret `sshuser@hn0-clustername:~$`.</span><span class="sxs-lookup"><span data-stu-id="6a831-221">For example, when connected to the primary head node as `sshuser`, the prompt is `sshuser@hn0-clustername:~$`.</span></span>

### <a name="connect-to-worker-and-zookeeper-nodes"></a><span data-ttu-id="6a831-222">Ansluta till arbetarnoder och Zookeeper-noder</span><span class="sxs-lookup"><span data-stu-id="6a831-222">Connect to worker and Zookeeper nodes</span></span>

<span data-ttu-id="6a831-223">Arbetarnoder och Zookeeper-noder är inte tillgängliga direkt från internet.</span><span class="sxs-lookup"><span data-stu-id="6a831-223">The worker nodes and Zookeeper nodes are not directly accessible from the internet.</span></span> <span data-ttu-id="6a831-224">De kan nås från klustrets huvudnoder eller kantnoder.</span><span class="sxs-lookup"><span data-stu-id="6a831-224">They can be accessed from the cluster head nodes or edge nodes.</span></span> <span data-ttu-id="6a831-225">Här är de allmänna steg som du följer för att ansluta till andra noder:</span><span class="sxs-lookup"><span data-stu-id="6a831-225">The following are the general steps to connect to other nodes:</span></span>

1. <span data-ttu-id="6a831-226">Använd SSH för att ansluta till en huvud- eller kantnod:</span><span class="sxs-lookup"><span data-stu-id="6a831-226">Use SSH to connect to a head or edge node:</span></span>

        ssh sshuser@myedge.mycluster-ssh.azurehdinsight.net

2. <span data-ttu-id="6a831-227">Från SSH-anslutningen till huvud- eller kantnoden använder du `ssh`-kommandot för att ansluta till en arbetarnod i klustret:</span><span class="sxs-lookup"><span data-stu-id="6a831-227">From the SSH connection to the head or edge node, use the `ssh` command to connect to a worker node in the cluster:</span></span>

        ssh sshuser@wn0-myhdi

    <span data-ttu-id="6a831-228">Om du vill hämta en lista över domännamnen för noderna i klustret tittar du på [Manage HDInsight by using the Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) (Hantera HDInsight med hjälp av Ambari REST-API:et).</span><span class="sxs-lookup"><span data-stu-id="6a831-228">To retrieve a list of the domain names of the nodes in the cluster, see the [Manage HDInsight by using the Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) document.</span></span>

<span data-ttu-id="6a831-229">Om SSH-kontot är skyddat med ett __lösenord__ anger du lösenordet när du ansluter.</span><span class="sxs-lookup"><span data-stu-id="6a831-229">If the SSH account is secured using a __password__, enter the password when connecting.</span></span>

<span data-ttu-id="6a831-230">Om SSH-kontot är säkrad med __SSH-nycklar__ kontrollerar du att SSH-vidarebefordran är aktiverad på klienten.</span><span class="sxs-lookup"><span data-stu-id="6a831-230">If the SSH account is secured using __SSH keys__, make sure that SSH forwarding is enabled on the client.</span></span>

> [!NOTE]
> <span data-ttu-id="6a831-231">Ett annat sätt att direkt komma åt alla noder i klustret är att installera HDInsight i ett virtuellt Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="6a831-231">Another way to directly access all nodes in the cluster is to install HDInsight into an Azure Virtual Network.</span></span> <span data-ttu-id="6a831-232">Därefter kan du ansluta till din fjärrdatorn i samma virtuella nätverk och direkt komma åt alla noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="6a831-232">Then, you can join your remote machine to the same virtual network and directly access all nodes in the cluster.</span></span>
>
> <span data-ttu-id="6a831-233">Mer information finns i [Use a virtual network with HDInsight](hdinsight-extend-hadoop-virtual-network.md) (Använda ett virtuellt nätverk med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="6a831-233">For more information, see [Use a virtual network with HDInsight](hdinsight-extend-hadoop-virtual-network.md).</span></span>

### <a name="configure-ssh-agent-forwarding"></a><span data-ttu-id="6a831-234">Konfigurera vidarebefordran med SSH-agenten</span><span class="sxs-lookup"><span data-stu-id="6a831-234">Configure SSH agent forwarding</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6a831-235">I följande steg förutsätter vi att du har ett Linux- eller UNIX-baserat system och att du arbetar med Bash i Windows 10.</span><span class="sxs-lookup"><span data-stu-id="6a831-235">The following steps assume a Linux or UNIX-based system, and work with Bash on Windows 10.</span></span> <span data-ttu-id="6a831-236">Om de här stegen inte fungerar på din dator kan du behöva läsa dokumentationen för SSH-klienten.</span><span class="sxs-lookup"><span data-stu-id="6a831-236">If these steps do not work for your system, you may need to consult the documentation for your SSH client.</span></span>

1. <span data-ttu-id="6a831-237">Använd en textredigerare och öppna `~/.ssh/config`.</span><span class="sxs-lookup"><span data-stu-id="6a831-237">Using a text editor, open `~/.ssh/config`.</span></span> <span data-ttu-id="6a831-238">Om den här filen inte finns kan du skapa den genom att ange `touch ~/.ssh/config` på en kommandorad.</span><span class="sxs-lookup"><span data-stu-id="6a831-238">If this file doesn't exist, you can create it by entering `touch ~/.ssh/config` at a command line.</span></span>

2. <span data-ttu-id="6a831-239">Lägg till följande text i `config`-filen.</span><span class="sxs-lookup"><span data-stu-id="6a831-239">Add the following text to the `config` file.</span></span>

        Host <edgenodename>.<clustername>-ssh.azurehdinsight.net
          ForwardAgent yes

    <span data-ttu-id="6a831-240">Ersätt informationen för __Host__ med adressen för den nod som du ansluter till med hjälp av SSH.</span><span class="sxs-lookup"><span data-stu-id="6a831-240">Replace the __Host__ information with the address of the node you connect to using SSH.</span></span> <span data-ttu-id="6a831-241">I föregående exempel används kantnoden.</span><span class="sxs-lookup"><span data-stu-id="6a831-241">The previous example uses the edge node.</span></span> <span data-ttu-id="6a831-242">Den här posten konfigurerar vidarebefordran med SSH-agenten för den angivna noden.</span><span class="sxs-lookup"><span data-stu-id="6a831-242">This entry configures SSH agent forwarding for the specified node.</span></span>

3. <span data-ttu-id="6a831-243">Testa SSH-agentvidarebefordran med hjälp av följande kommando från terminalen:</span><span class="sxs-lookup"><span data-stu-id="6a831-243">Test SSH agent forwarding by using the following command from the terminal:</span></span>

        echo "$SSH_AUTH_SOCK"

    <span data-ttu-id="6a831-244">Det här kommandot returnerar information liknande följande text:</span><span class="sxs-lookup"><span data-stu-id="6a831-244">This command returns information similar to the following text:</span></span>

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    <span data-ttu-id="6a831-245">Om inget returneras så körs inte `ssh-agent`.</span><span class="sxs-lookup"><span data-stu-id="6a831-245">If nothing is returned, then `ssh-agent` is not running.</span></span> <span data-ttu-id="6a831-246">Läs informationen om skripten för agentstart i [Using ssh-agent with ssh (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) (Använda ssh-agent med ssh) eller läs dokumentationen för din SSH-klient för mer information.</span><span class="sxs-lookup"><span data-stu-id="6a831-246">For more information, see the agent startup scripts information at [Using ssh-agent with ssh (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) or consult your SSH client documentation.</span></span>

4. <span data-ttu-id="6a831-247">När du har verifierat att **ssh-agent** körs använder du följande för att lägga till din privata SSH-nyckel till agenten:</span><span class="sxs-lookup"><span data-stu-id="6a831-247">Once you have verified that **ssh-agent** is running, use the following to add your SSH private key to the agent:</span></span>

        ssh-add ~/.ssh/id_rsa

    <span data-ttu-id="6a831-248">Om din privata nyckel finns i en annan fil ersätter du `~/.ssh/id_rsa` med sökvägen till filen.</span><span class="sxs-lookup"><span data-stu-id="6a831-248">If your private key is stored in a different file, replace `~/.ssh/id_rsa` with the path to the file.</span></span>

5. <span data-ttu-id="6a831-249">Anslut till kant- eller huvudnoder i klustret med hjälp av SSH.</span><span class="sxs-lookup"><span data-stu-id="6a831-249">Connect to the cluster edge node or head nodes using SSH.</span></span> <span data-ttu-id="6a831-250">Använd sedan SSH-kommandot för att ansluta till en arbetar- eller Zookeeper-nod.</span><span class="sxs-lookup"><span data-stu-id="6a831-250">Then use the SSH command to connect to a worker or zookeeper node.</span></span> <span data-ttu-id="6a831-251">Anslutningen upprättas med hjälp av den vidarebefordrade nyckeln.</span><span class="sxs-lookup"><span data-stu-id="6a831-251">The connection is established using the forwarded key.</span></span>

## <a name="copy-files"></a><span data-ttu-id="6a831-252">Kopiera filer</span><span class="sxs-lookup"><span data-stu-id="6a831-252">Copy files</span></span>

<span data-ttu-id="6a831-253">Verktyget `scp` kan användas för att kopiera filer till och från enskilda noder i ett kluster.</span><span class="sxs-lookup"><span data-stu-id="6a831-253">The `scp` utility can be used to copy files to and from individual nodes in the cluster.</span></span> <span data-ttu-id="6a831-254">Med följande kommando kopieras till exempel katalogen `test.txt` från det lokala systemet till den primära huvudnoden:</span><span class="sxs-lookup"><span data-stu-id="6a831-254">For example, the following command copies the `test.txt` directory from the local system to the primary head node:</span></span>

```bash
scp test.txt sshuser@clustername-ssh.azurehdinsight.net:
```

<span data-ttu-id="6a831-255">Eftersom ingen sökväg har angetts efter `:` placeras filen i hemkatalogen `sshuser`.</span><span class="sxs-lookup"><span data-stu-id="6a831-255">Since no path is specified after the `:`, the file is placed in the `sshuser` home directory.</span></span>

<span data-ttu-id="6a831-256">I följande exempel kopieras filen `test.txt` från hemkatalogen `sshuser` på den primära huvudnoden till det lokala systemet:</span><span class="sxs-lookup"><span data-stu-id="6a831-256">The following example copies the `test.txt` file from the `sshuser` home directory on the primary head node to the local system:</span></span>

```bash
scp sshuser@clustername-ssh.azurehdinsight.net:test.txt .
```

> [!IMPORTANT]
> <span data-ttu-id="6a831-257">`scp` har bara åtkomst tull filsystemet för enskilda noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="6a831-257">`scp` can only access the file system of individual nodes within the cluster.</span></span> <span data-ttu-id="6a831-258">Det kan inte användas för att få åtkomst till data i det HDFS-kompatibla lagringsutrymmet för klustret.</span><span class="sxs-lookup"><span data-stu-id="6a831-258">It cannot be used to access data in the HDFS-compatible storage for the cluster.</span></span>
>
> <span data-ttu-id="6a831-259">Använd `scp` när du behöver ladda upp en resurs för en SSH-session.</span><span class="sxs-lookup"><span data-stu-id="6a831-259">Use `scp` when you need to upload a resource for use from an SSH session.</span></span> <span data-ttu-id="6a831-260">Ladda exempelvis upp ett Python-skript och kör det sedan från en SSH-session.</span><span class="sxs-lookup"><span data-stu-id="6a831-260">For example, upload a Python script and then run the script from an SSH session.</span></span>
>
> <span data-ttu-id="6a831-261">I följande dokument hittar du information om att läsa in data direkt till det HDFS-kompatibla lagringsutrymmet:</span><span class="sxs-lookup"><span data-stu-id="6a831-261">For information on directly loading data into the HDFS-compatible storage, see the following documents:</span></span>
>
> * <span data-ttu-id="6a831-262">[HDInsight med Azure Storage](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="6a831-262">[HDInsight using Azure Storage](hdinsight-hadoop-use-blob-storage.md).</span></span>
>
> * <span data-ttu-id="6a831-263">[HDInsight med Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md).</span><span class="sxs-lookup"><span data-stu-id="6a831-263">[HDInsight using Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6a831-264">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6a831-264">Next steps</span></span>

* [<span data-ttu-id="6a831-265">Använda SSH-tunnlar med HDInsight</span><span class="sxs-lookup"><span data-stu-id="6a831-265">Use SSH tunneling with HDInsight</span></span>](hdinsight-linux-ambari-ssh-tunnel.md)
* [<span data-ttu-id="6a831-266">Använda ett virtuellt nätverk med HDInsight</span><span class="sxs-lookup"><span data-stu-id="6a831-266">Use a virtual network with HDInsight</span></span>](hdinsight-extend-hadoop-virtual-network.md)
* [<span data-ttu-id="6a831-267">Använda kantnoder i HDInsight</span><span class="sxs-lookup"><span data-stu-id="6a831-267">Use edge nodes in HDInsight</span></span>](hdinsight-apps-use-edge-node.md#access-an-edge-node)