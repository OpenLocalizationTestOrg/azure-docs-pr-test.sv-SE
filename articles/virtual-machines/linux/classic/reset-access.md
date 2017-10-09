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
# <a name="how-tooreset-a-linux-vm-password-or-ssh-key-fix-hello-ssh-configuration-and-check-disk-consistency-using-hello-vmaccess-extension"></a>Hur tooreset Linux VM lösenord eller SSH-nyckeln åtgärda hello SSH-konfigurationen och kontrollera disk överensstämmelse med hello VMAccess-tillägget
Om du inte kan ansluta tooa Linux-dator i Azure på grund av ett nytt lösenord, en felaktigt SSH (Secure Shell) nyckel eller ett problem med hello SSH-konfigurationen, använda hello VMAccessForLinux-tillägget med hello Azure CLI tooreset hello lösenord eller SSH-nyckel, åtgärda Hej SSH-konfigurationen och kontrollera disk konsekvenskontroll. 

> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Lär dig hur för[utföra dessa steg med hello Resource Manager-modellen](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).

Med hello Azure CLI, använder du hello **azure vm-tillägget set** från dina kommandoradsgränssnittet (Bash, Terminal, Kommandotolken) tooaccess kommandon. Kör **azure vm-tillägget hjälpfunktion** för tillägg för detaljerad användning.

Du kan göra med hello Azure CLI, hello följande uppgifter:

* [Återställ hello lösenord](#pwresetcli)
* [Återställa hello SSH-nyckeln](#sshkeyresetcli)
* [Återställa hello lösenord och hello SSH-nyckeln](#resetbothcli)
* [Skapa ett nytt användarkonto sudo](#createnewsudocli)
* [Återställ hello SSH-konfiguration](#sshconfigresetcli)
* [Tar bort en användare](#deletecli)
* [Visa hello status för hello VMAccess-tillägget](#statuscli)
* [Kontrollera konsekvensen för tillagda diskar](#checkdisk)
* [Reparera tillagda diskar på Linux-VM](#repairdisk)

## <a name="prerequisites"></a>Krav
Du behöver toodo hello följande:

* Du behöver för[installera hello Azure CLI](../../../cli-install-nodejs.md) och [ansluter tooyour prenumeration](../../../xplat-cli-connect.md) toouse Azure resurser som är associerade med ditt konto.
* Ange hello rätt läge för hello klassiska distributionsmodellen genom att skriva följande hello hello Kommandotolken:
    ``` 
        azure config mode asm
    ```
* Har ett nytt lösenord eller SSH-nycklar, om du vill tooreset antingen en. Du behöver inte dessa om du vill tooreset hello SSH-konfigurationen.

## <a name="pwresetcli"></a>Återställ hello lösenord
1. Skapa en fil på den lokala datorn med namnet PrivateConf.json med dessa rader. Ersätt **MittAnvändarnamn** och  **myP@ssW0rd**  med användarnamn och lösenord och Ställ in datum för förfallodatum.

    ```   
        {
        "username":"myUserName",
        "password":"myP@ssW0rd",
        "expiration":"2020-01-01"
        }
    ```
        
2. Kör det här kommandot ersätter hello namnet på den virtuella datorn för **myVM**.

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json
    ```

## <a name="sshkeyresetcli"></a>Återställa hello SSH-nyckeln
1. Skapa en fil som heter PrivateConf.json med innehållet. Ersätt hello **MittAnvändarnamn** och **mySSHKey** värden med din egen information.

    ```   
        {
        "username":"myUserName",
        "ssh_key":"mySSHKey"
        }
    ```
2. Kör det här kommandot ersätter hello namnet på den virtuella datorn för **myVM**.
   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="resetbothcli"></a>Återställa både hello lösenord och hello SSH-nyckel
1. Skapa en fil som heter PrivateConf.json med innehållet. Ersätt hello **MittAnvändarnamn**, **mySSHKey** och  **myP@ssW0rd**  värden med din egen information.

    ``` 
        {
        "username":"myUserName",
        "ssh_key":"mySSHKey",
        "password":"myP@ssW0rd"
        }
    ```

2. Kör det här kommandot ersätter hello namnet på den virtuella datorn för **myVM**.

    ```   
        azure vm extension set MyVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <a name="createnewsudocli"></a>Skapa ett nytt användarkonto sudo

Om du glömmer ditt användarnamn kan använda du VMAccess toocreate en ny med hello sudo utfärdare. I det här fallet hello befintligt användarnamn och lösenord ändras inte.

toocreate en ny användare sudo med lösenordsåtkomst, Använd hello skript i [Återställ hello lösenord](#pwresetcli) och ange hello nytt användarnamn.

toocreate en ny sudo-användare med SSH-nyckel åtkomst, Använd hello skript i [Återställ hello SSH-nyckeln](#sshkeyresetcli) och ange hello nytt användarnamn.

Du kan också använda [återställa hello lösenord och hello SSH-nyckeln](#resetbothcli) toocreate en ny användare med både lösenord och SSH-nyckel åtkomst.

## <a name="sshconfigresetcli"></a>Återställ hello SSH-konfiguration
Om hello SSH-konfigurationen finns i ett oönskat tillstånd, kan du också förlora åtkomst toohello VM. Du kan använda hello VMAccess-tillägget tooreset hello configuration tooits standardläget. toodo så behöver du bara tooset hello ”reset_ssh” nyckel för ”True”. hello-tillägget ska starta om hello SSH-server, öppna hello SSH-port på den virtuella datorn och Återställ hello SSH toodefault konfigurationsvärden. hello användarkonto (namn, lösenord eller SSH-nycklar) ändras inte.

> [!NOTE]
> hello SSH-konfigurationsfil som hämtar återställa finns i /etc/ssh/sshd_config.
> 
> 

1. Skapa en fil som heter PrivateConf.json med det här innehållet.

    ```   
        {
        "reset_ssh":"True"
        }
    ```

2. Kör det här kommandot ersätter hello namnet på den virtuella datorn för **myVM**. 

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <a name="deletecli"></a>Tar bort en användare
Om du vill toodelete ett användarkonto utan att logga in på toohello VM direkt, kan du använda det här skriptet.

1. Skapa en fil med namnet PrivateConf.json med det här innehållet ersätter hello användaren namnet tooremove för **removeUserName**. 

    ```   
        {
        "remove_user":"removeUserName"
        }
    ```

2. Kör det här kommandot ersätter hello namnet på den virtuella datorn för **myVM**. 

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <a name="statuscli"></a>Visa hello status för hello VMAccess-tillägget
toodisplay hello status för hello VMAccess-tillägget, kör kommandot.

```
        azure vm extension get
```

## <a name='checkdisk'></a>Kontrollera konsekvensen för tillagda diskar
toorun fsck på alla diskar i Linux-dator, måste toodo hello följande:

1. Skapa en fil som heter PublicConf.json med det här innehållet. Kontrollera att disken tar ett booleskt värde för om toocheck diskar kopplade tooyour virtuell dator eller inte. 

    ```   
        {   
        "check_disk": "true"
        }
    ```

2. Kör det här kommandot tooexecute, ersätter hello namnet på den virtuella datorn för **myVM**.

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json 
    ```

## <a name='repairdisk'></a>Reparera diskar
toorepair diskar som inte montera eller har montera konfigurationsfel använda hello VMAccess-tillägget tooreset hello montera konfigurationen på Linux-dator. Ersätter hello namnet på disken för **myDisk**.

1. Skapa en fil som heter PublicConf.json med det här innehållet. 

    ```   
        {
        "repair_disk":"true",
        "disk_name":"myDisk"
        }
    ```

2. Kör det här kommandot tooexecute, ersätter hello namnet på den virtuella datorn för **myVM**.

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json
    ```

## <a name="next-steps"></a>Nästa steg
* Om du vill toouse Azure PowerShell-cmdlets eller Azure Resource Manager mallar tooreset hello lösenord eller SSH-nyckel, åtgärda hello SSH-konfigurationen och kontrollera konsekvens disk, se hello [VMAccess-tillägget dokumentation på GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess). 
* Du kan också använda hello [Azure-portalen](https://portal.azure.com) tooreset hello lösenord eller SSH-nyckel för en Linux-VM distribueras i hello klassiska distributionsmodellen. Du kan inte använder hello portal gör toothis för en Linux VM distribueras i hello Resource Manager-distributionsmodellen.
* Se [om virtuella datortillägg och funktioner](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mer information om användning av VM-tillägg för virtuella Azure-datorer.

