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
# <a name="connect-toohdinsight-hadoop-using-ssh"></a>Ansluta tooHDInsight (Hadoop) via SSH

Lär dig hur toouse [SSH (Secure Shell)](https://en.wikipedia.org/wiki/Secure_Shell) toosecurely ansluta tooHadoop på Azure HDInsight. 

HDInsight kan använda Linux (Ubuntu) som hello operativsystem för noder i hello Hadoop-kluster. hello innehåller följande tabell hello-adressen och porten information som krävs när du ansluter tooLinux-baserade HDInsight med hjälp av en SSH-klient:

| Adress | Port | Ansluter till ... |
| ----- | ----- | ----- |
| `<clustername>-ed-ssh.azurehdinsight.net` | 22 | Kantnod (R Server på HDInsight) |
| `<edgenodename>.<clustername>-ssh.azurehdinsight.net` | 22 | Kantnod (alla andra klustertyper, om det finns en kantnod) |
| `<clustername>-ssh.azurehdinsight.net` | 22 | Den primära huvudnoden |
| `<clustername>-ssh.azurehdinsight.net` | 23 | Den sekundära huvudnoden |

> [!NOTE]
> Ersätt `<edgenodename>` med hello kantnod hello namn.
>
> Ersätt `<clustername>` med hello namnet på klustret.
>
> Om klustret innehåller en kantnod, rekommenderar vi att du __ansluter alltid toohello kantnod__ via SSH. Hej huvudnoderna värd för tjänster som är kritiska toohello hälsotillståndet för Hadoop. hello kantnod körs bara vad som ska visas på den.
>
> Mer information om hur du använder kantnoder finns i [Använda kantnoder i HDInsight](hdinsight-apps-use-edge-node.md#access-an-edge-node).

## <a name="ssh-clients"></a>SSH-klienter

Linux, Unix- och macOS system ger hello `ssh` och `scp` kommandon. Hej `ssh` klienten är vanliga toocreate en kommandorad fjärrsessionen med Linux eller Unix-baserade system. Hej `scp` klienten är används toosecurely kopiera filer mellan klienten och hello fjärrsystemet.

Microsoft Windows tillhandahåller ingen SSH-klient som standard. Hej `ssh` och `scp` klienter som är tillgängliga för Windows via hello följande paket:

* [Azure Cloud Shell](../cloud-shell/quickstart.md): hello molnet Shell innehåller en Bash-miljö i webbläsaren, samt hello `ssh`, `scp`, och andra vanliga Linux-kommandon.

* [Bash på Ubuntu på Windows 10](https://msdn.microsoft.com/commandline/wsl/about): hello `ssh` och `scp` kommandon är tillgängliga via hello Bash på kommandoraden i Windows.

* [Git (https://git-scm.com/)](https://git-scm.com/): hello `ssh` och `scp` kommandon är tillgängliga via hello Bash-kommandoraden.

* [GitHub skrivbordet (https://desktop.github.com/)](https://desktop.github.com/) hello `ssh` och `scp` kommandon är tillgängliga via hello GitHub Shell-kommandoraden. GitHub Desktop kan vara konfigurerade toouse Bash hello Windows kommandotolk eller PowerShell hello kommandoraden för hello Git-gränssnittet.

* [OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH): hello PowerShell team portera OpenSSH tooWindows och ger test-versioner.

    > [!WARNING]
    > Hej OpenSSH paket innehåller hello SSH serverkomponent, `sshd`. Den här komponenten startar en SSH-server i systemet, så att andra tooconnect tooit. Konfigurera den här komponenten eller inte öppna port 22 såvida du inte vill toohost en SSH-server på datorn. Det är inte obligatoriska toocommunicate med HDInsight.

Det finns också flera grafiska SSH-klienter, till exempel [PuTTY (http://www.chiark.greenend.org.uk/~sgtatham/putty/)](http://www.chiark.greenend.org.uk/~sgtatham/putty/) och [MobaXterm (http://mobaxterm.mobatek.net/)](http://mobaxterm.mobatek.net/). Dessa klienter kan vara används tooconnect tooHDInsight, hello anslutningsprocessen skiljer sig med hjälp av hello `ssh` verktyget. Mer information finns i hello dokumentationen för hello grafiska klient du använder.

## <a id="sshkey"></a>Autentisering: SSH-nycklar

SSH-nycklar använda [offentliga nycklar](https://en.wikipedia.org/wiki/Public-key_cryptography) tooauthenticate SSH-sessioner. SSH-nycklar är säkrare än och ger ett enkelt sätt toosecure åtkomst tooyour Hadoop-kluster.

Om SSH-konto är skyddad med en nyckel, måste hello klienten tillhandahålla hello matchar privata nyckel när du ansluter:

* De flesta klienter kan vara konfigurerade toouse en __standardnyckeln__. Till exempel hello `ssh` klienten söker efter en privat nyckel vid `~/.ssh/id_rsa` på Linux och Unix-miljöer.

* Du kan ange hello __sökväg tooa privata nyckeln__. Med hello `ssh` klient, hello `-i` parameter är används toospecify hello sökvägen tooprivate nyckel. Till exempel `ssh -i ~/.ssh/id_rsa sshuser@myedge.mycluster-ssh.azurehdinsight.net`.

* Om du har __flera privata nycklar__ för användning med olika servrar kan verktyg som [ssh-agent (https://en.wikipedia.org/wiki/Ssh-agent)](https://en.wikipedia.org/wiki/Ssh-agent) användas. Hej `ssh-agent` verktyg kan vara används tooautomatically väljer hello viktiga toouse när en SSH-session.

> [!IMPORTANT]
>
> Om du skyddar din privata nyckel med en lösenfras måste du ange hello lösenfrasen när du använder hello nyckel. Verktyg som `ssh-agent` kan cachelagra hello lösenord för din bekvämlighet.

### <a name="create-an-ssh-key-pair"></a>Skapa ett SSH-nyckelpar

Använd hello `ssh-keygen` kommandot toocreate offentliga och privata nyckelfiler. hello skapar följande kommando en 2048-bitars RSA-nyckelpar som kan användas med HDInsight:

    ssh-keygen -t rsa -b 2048

Du uppmanas information under hello viktiga skapandeprocessen. Till exempel där hello nycklar lagras eller om toouse en lösenfras. När hello processen har slutförts, skapas två filer; en offentlig nyckel och en privat nyckel.

* Hej __offentliga nyckel__ är används toocreate ett HDInsight-kluster. hello offentliga nyckel har filnamnstillägget `.pub`.

* Hej __privata nyckel__ är används tooauthenticate klienten toohello HDInsight-kluster.

> [!IMPORTANT]
> Du kan skydda dina nycklar med hjälp av en lösenfras. Detta är ett lösenord för den privata nyckeln. Även om någon hämtar din privata nyckel, måste de ha hello lösenfras toouse hello nyckel.

### <a name="create-hdinsight-using-hello-public-key"></a>Skapa HDInsight med hjälp av hello offentlig nyckel

| Genereringsmetod | Hur toouse hello offentlig nyckel |
| ------- | ------- |
| **Azure Portal** | Avmarkera __använda samma lösenord som klusterinloggning__, och välj sedan __offentliga nyckel__ som hello SSH autentiseringstyp. Slutligen, Välj hello offentlig nyckelfil eller klistra in hello textinnehållet i hello-filen i hello __offentliga SSH-nyckeln__ fältet.</br>![Dialogrutan Offentlig SSH-nyckel vid generering av HDInsight-kluster](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-public-key.png) |
| **Azure PowerShell** | Använd hello `-SshPublicKey` parametern för hello `New-AzureRmHdinsightCluster` cmdlet- och pass hello innehållet i hello offentlig nyckel som en sträng.|
| **Azure CLI 1.0** | Använd hello `--sshPublicKey` parametern för hello `azure hdinsight cluster create` kommandot och skicka hello innehållet i hello offentlig nyckel som en sträng. |
| **Resource Manager-mall** | Ett exempel på hur du använder SSH-nycklar med en mall finns i avsnittet [Deploy HDInsight on Linux with SSH key](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-publickey/) (Distribuera HDInsight i Linux med en SSH-nyckel). Hej `publicKeys` element i hello [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-publickey/azuredeploy.json) filen har använt toopass hello nycklar tooAzure när du skapar hello kluster. |

## <a id="sshpassword"></a>Autentisering: lösenord

SSH-konton kan skyddas med ett lösenord. Ange tooenter hello lösenord anges när du ansluter tooHDInsight via SSH.

> [!WARNING]
> Vi rekommenderar inte att du använder lösenordsautentisering för SSH. Lösenord kan knäckas och är sårbara toobrute force-attacker. I stället rekommenderar vi att du använder [SSH-nycklar för autentisering](#sshkey).

### <a name="create-hdinsight-using-a-password"></a>Skapa HDInsight med ett lösenord

| Genereringsmetod | Hur toospecify hello lösenord |
| --------------- | ---------------- |
| **Azure Portal** | Hello SSH-användarkontot har som standard hello samma lösenord som hello inloggningskonto för klustret. Avmarkera toouse ett annat lösenord __använda samma lösenord som klusterinloggning__, och sedan ange hello lösenord i hello __SSH-lösenordet__ fältet.</br>![Dialogrutan SSH-lösenord när ett HDInsight-kluster skapas](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-password.png)|
| **Azure PowerShell** | Använd hello `--SshCredential` parametern för hello `New-AzureRmHdinsightCluster` cmdlet och skicka en `PSCredential` objekt som innehåller hello SSH användarens kontonamn och lösenord. |
| **Azure CLI 1.0** | Använd hello `--sshPassword` parametern för hello `azure hdinsight cluster create` kommando och ange värdet för hello lösenord. |
| **Resource Manager-mall** | Ett exempel på hur du använder ett lösenord med en mall finns i [Deploy HDInsight on Linux with SSH password](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/) (Distribuera HDInsight i Linux med SSH-lösenord). Hej `linuxOperatingSystemProfile` element i hello [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-password/azuredeploy.json) filen har använt toopass hello SSH-konto och lösenord tooAzure när du skapar hello kluster.|

### <a name="change-hello-ssh-password"></a>Ändra hello SSH-lösenord

Information om hur du ändrar hello SSH lösenord finns hello __ändra lösenord__ avsnitt i hello [hantera HDInsight](hdinsight-administer-use-portal-linux.md#change-passwords) dokumentet.

## <a id="domainjoined"></a>Autentisering: Domänanslutet HDInsight-kluster

Om du använder en __domänanslutna HDInsight-kluster__, måste du använda hello `kinit` kommandot efter anslutning med SSH. Det här kommandot uppmanar dig domänanvändare och lösenord och autentiserar sessionen med hello Azure Active Directory-domänen som är associerade med hello-kluster.

Mer information finns i avsnittet [Configure domain-joined HDInsight](hdinsight-domain-joined-configure.md) (Konfigurera domänanslutna HDInsight-kluster).

## <a name="connect-toonodes"></a>Ansluta toonodes

Hej huvudnoderna och kantnod (om sådan finns) kan nås över hello internet på port 22 och 23.

* När du ansluter toohello __huvudnoder__, använda port __22__ tooconnect toohello primära head nod och port __23__ tooconnect toohello andra huvudnod. hello fullständigt kvalificerade namnet toouse är `clustername-ssh.azurehdinsight.net`, där `clustername` är hello namnet på klustret.

    ```bash
    # Connect tooprimary head node
    # port not specified since 22 is hello default
    ssh sshuser@clustername-ssh.azurehdinsight.net

    # Connect toosecondary head node
    ssh -p 23 sshuser@clustername-ssh.azurehdinsight.net
    ```
    
* När connectiung toohello __kantnoden__, använder port 22. hello fullständigt kvalificerade domännamnet är `edgenodename.clustername-ssh.azurehdinsight.net`, där `edgenodename` är ett namn som du angav när du skapar hello kantnod. `clustername`är hello hello klustrets namn.

    ```bash
    # Connect tooedge node
    ssh sshuser@edgnodename.clustername-ssh.azurehdinsight.net
    ```

> [!IMPORTANT]
> hello föregående exempel förutsätter att du använder en lösenordsautentisering eller att certifikatautentisering är utförs automatiskt. Om du använder en SSH-nyckel för autentisering och används inte hello certifikatet automatiskt, använder hello `-i` parametern toospecify hello privat nyckel. Till exempel `ssh -i ~/.ssh/mykey sshuser@clustername-ssh.azurehdinsight.net`.

När du är ansluten, ändras hello Kommandotolken tooindicate hello SSH användaren namnet hello noden och du är ansluten till. Till exempel när ansluten toohello primära huvudnod som `sshuser`, hello frågan är `sshuser@hn0-clustername:~$`.

### <a name="connect-tooworker-and-zookeeper-nodes"></a>Ansluta tooworker och Zookeeper-noder

Hej arbetarnoder och Zookeeper noder inte är direkt åtkomliga från hello internet. De kan nås från hello-klustrets huvudnoder eller edge-noder. hello följande är hello allmänna steg tooconnect tooother noder:

1. Använd SSH tooconnect tooa head eller edge nod:

        ssh sshuser@myedge.mycluster-ssh.azurehdinsight.net

2. Använd hello från hello SSH-anslutning toohello head eller kantnod `ssh` kommandot tooconnect tooa arbetsnoden hello klustret:

        ssh sshuser@wn0-myhdi

    tooretrieve en lista över hello domännamn för hello noder i klustret hello finns hello [hantera HDInsight med hjälp av hello Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) dokumentet.

Om hello SSH-konto är skyddad med en __lösenord__, ange hello lösenord när du ansluter.

Om hello SSH-konto är säkrad via __SSH-nycklar__, se till att SSH-vidarebefordran är aktiverad på hello-klienten.

> [!NOTE]
> Ett annat sätt toodirectly åtkomst till alla noder i klustret hello är tooinstall HDInsight i ett virtuellt Azure-nätverk. Du kan sedan ansluta till fjärrdatorn-toohello samma virtuella nätverk och få åtkomst till alla noder i klustret hello direkt.
>
> Mer information finns i [Use a virtual network with HDInsight](hdinsight-extend-hadoop-virtual-network.md) (Använda ett virtuellt nätverk med HDInsight).

### <a name="configure-ssh-agent-forwarding"></a>Konfigurera vidarebefordran med SSH-agenten

> [!IMPORTANT]
> hello följande steg förutsätter en Linux eller UNIX-baserat system och arbeta med Bash på Windows 10. Om de här stegen inte fungerar för ditt system, eventuellt tooconsult hello dokumentationen för SSH-klienten.

1. Använd en textredigerare och öppna `~/.ssh/config`. Om den här filen inte finns kan du skapa den genom att ange `touch ~/.ssh/config` på en kommandorad.

2. Lägg till följande text toohello hello `config` fil.

        Host <edgenodename>.<clustername>-ssh.azurehdinsight.net
          ForwardAgent yes

    Ersätt hello __värden__ information med hello adressen hello nod du ansluta toousing SSH. hello föregående exemplet används hello kantnod. Den här posten konfigurerar SSH-agentvidarebefordran för hello angivna noden.

3. Testa SSH-agentvidarebefordran med hjälp av följande kommando från hello terminal hello:

        echo "$SSH_AUTH_SOCK"

    Det här kommandot returnerar informationen liknande toohello följande text:

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    Om inget returneras så körs inte `ssh-agent`. Mer information finns i hello agent startinformation skript på [med hjälp av ssh-agenten med ssh (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) eller Läs dokumentationen för SSH-klienten.

4. När du har verifierat att **ssh-agent** körs, Använd hello följande tooadd SSH privat nyckel toohello agent:

        ssh-add ~/.ssh/id_rsa

    Om din privata nyckel lagras i en annan fil ersätter `~/.ssh/id_rsa` med hello sökväg toohello fil.

5. Anslut toohello klusternod kant eller huvudnoderna via SSH. Använd sedan hello SSH-kommandot tooconnect tooa worker eller zookeeper-nod. hello-anslutning upprättas med hjälp av hello vidarebefordras nyckel.

## <a name="copy-files"></a>Kopiera filer

Hej `scp` verktyg kan vara används toocopy filer tooand från enskilda noder i klustret hello. Till exempel hello efter kommandot kopior hello `test.txt` katalog från hello lokalt system toohello primära huvudnod:

```bash
scp test.txt sshuser@clustername-ssh.azurehdinsight.net:
```

Eftersom ingen sökväg anges efter hello `:`, hello filen placeras i hello `sshuser` -hemkataloger.

följande exempel kopierar hello hello `test.txt` filen från hello `sshuser` arbetskatalog på hello primära huvudnod toohello lokalt system:

```bash
scp sshuser@clustername-ssh.azurehdinsight.net:test.txt .
```

> [!IMPORTANT]
> `scp`bara åt hello filsystemet på enskilda noder i hello kluster. Inte kan använda tooaccess data i hello HDFS-kompatibla lagringen för hello klustret.
>
> Använd `scp` när du behöver tooupload en resurs för användning från en SSH-session. Till exempel överföra en Python-skriptet och kör sedan hello skript från en SSH-session.
>
> Information om att läsa in data direkt till hello HDFS-kompatibla lagring finns i hello följande dokument:
>
> * [HDInsight med Azure Storage](hdinsight-hadoop-use-blob-storage.md).
>
> * [HDInsight med Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md).

## <a name="next-steps"></a>Nästa steg

* [Använda SSH-tunnlar med HDInsight](hdinsight-linux-ambari-ssh-tunnel.md)
* [Använda ett virtuellt nätverk med HDInsight](hdinsight-extend-hadoop-virtual-network.md)
* [Använda kantnoder i HDInsight](hdinsight-apps-use-edge-node.md#access-an-edge-node)