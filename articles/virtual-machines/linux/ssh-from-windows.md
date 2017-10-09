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
# <a name="how-toouse-ssh-keys-with-windows-on-azure"></a>Hur tooUse SSH-nycklar med Windows på Azure
> [!div class="op_single_selector"]
> * [Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> * [Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
>
>

När du ansluter tooLinux virtuella maskiner (VMs) i Azure, bör du använda [offentliga nycklar](https://wikipedia.org/wiki/Public-key_cryptography) tooprovide ett säkrare sätt toolog i tooyour Linux VM. Den här processen omfattar en offentliga och privata nycklar exchange med hello secure shell (SSH) kommandot tooauthenticate själv i stället för användarnamn och lösenord. Lösenord är sårbar toobrute force-attacker, särskilt på virtuella datorer mot Internet, till exempel webbservrar. Den här artikeln innehåller en översikt över SSH-nycklar och hur toogenerate hello lämpliga nycklar på en Windows-dator.

## <a name="overview-of-ssh-and-keys"></a>Översikt över SSH och nycklar
Du kan på ett säkert sätt logga in tooyour Linux VM med hjälp av offentliga och privata nycklar:

* Hej **offentliga nyckel** är placerad på din Linux VM eller någon annan tjänst som du vill toouse med offentliga nycklar.
* Hej **privata nyckel** är vad du finns tooyour Linux VM när du loggar in tooverify din identitet. Skydda den privata nyckeln. Dela den inte med andra.

Dessa offentliga och privata nycklar kan användas på flera virtuella datorer och tjänster. Du behöver inte ett nyckelpar för varje virtuell dator eller tjänst som du vill tooaccess. En detaljerad översikt finns [offentliga nycklar](https://wikipedia.org/wiki/Public-key_cryptography).

SSH är en krypterad anslutningsprotokoll som tillåter säker inloggning via oskyddat anslutningar. Det är hello anslutning standardprotokollet för virtuella Linux-datorer finns i Azure. Även om SSH själva ger en krypterad anslutning, lämnar med hjälp av lösenord med SSH-anslutningar fortfarande hello VM sårbara toobrute force-attacker eller att gissa lösenord. En säkrare och prioriterade metoden för anslutande tooa VM som använder SSH är med hjälp av offentliga och privata nycklarna, även kallat SSH-nycklar.

Om du inte vill att toouse SSH-nycklar kan du fortfarande logga in tooyour virtuella Linux-datorer med ett lösenord. Om den virtuella datorn inte är synliga toohello Internet, kan det vara tillräckligt att använda lösenord. Men du fortfarande behöver toomanage lösenorden för varje Linux-VM och underhåll felfri lösenordsprinciper och praxis som minsta längd på lösenord och regelbundet uppdaterar dem. hello användning av SSH-nycklar minskar hello hantera enskilda autentiseringsuppgifter över flera virtuella datorer.

## <a name="windows-packages-and-ssh-clients"></a>Windows-paket och SSH-klienter
Du ansluter tooand hantera virtuella Linux-datorer i Azure med hjälp av en **SSH-klienten**. Windows-datorer har inte normalt en SSH-klienten installerad. hello Windows 10 årsdagar Update läggs Bash för Windows och hello senaste skapare av uppdateringen för Windows 10 ger ytterligare uppdateringar. Den här Windows-undersystem för Linux kan verktygen toorun och åtkomst till exempel en SSH-klienten internt i ett Bash-gränssnitt. Sedan kan du följa eventuella hello Linux dokumenten som [hur toogenerate SSH-nyckel-par för Linux](mac-create-ssh-keys.md). Bash för Windows är fortfarande under utveckling och betraktas som en betaversion. Mer information om Bash för Windows finns [Bash på Ubuntu på Windows](https://msdn.microsoft.com/commandline/wsl/about).

Om du inte vill toouse något annat än Bash för Windows, vanliga Windows SSH klienter kan du installera ingår i hello följande paket:

* [Git för Windows](https://git-for-windows.github.io/)
* [puTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
* [MobaXterm](http://mobaxterm.mobatek.net/)
* [Cygwin](https://cygwin.com/)


## <a name="which-key-files-do-you-need-toocreate"></a>Som nyckeln filer behöver du toocreate?
Azure kräver minst 2048-bitars **ssh-rsa** formaterad offentliga och privata nycklar. Om du hanterar Azure-resurser med hjälp av hello klassiska distributionsmodellen, måste du också toogenerate en PEM (`.pem` filen).

Här följer hello distributionsscenarier och hello typer av filer som används i varje:

1. **SSH-rsa** nycklarna är obligatoriska för en distribution med hello [Azure-portalen](https://portal.azure.com), och Resource Manager distributioner med hjälp av hello [Azure CLI](../../cli-install-nodejs.md).
   * Nyckeln är oftast alla de flesta användare behöver.
2. En `.pem` filen är obligatoriska toocreate virtuella datorer med hello klassisk distribution. De här nycklarna kan användas i klassiska distributioner med hello [Azure-portalen](https://portal.azure.com) eller [Azure CLI](../../cli-install-nodejs.md).
   * Du behöver bara toocreate dessa ytterligare nycklar och certifikat om du hanterar resurser som har skapats med hjälp av hello klassiska distributionsmodellen.

## <a name="install-git-for-windows"></a>Installera Git för Windows
hello föregående avsnitt visas flera paket som innehåller hello `openssl` tool för Windows. Det här verktyget är nödvändiga toocreate offentliga och privata nycklar. Hej följande exempel information om hur tooinstall och använda **Git för Windows**, men du kan välja det paket som du föredrar. **Git för Windows** ger dig åtkomst toosome ytterligare med öppen källkod ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) verktyg och hjälpmedel som kan vara användbar när du arbetar med virtuella Linux-datorer.

1. Hämta och installera **Git för Windows** från hello följande plats: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).
2. Acceptera hello standardalternativen under hello installera processen om du inte specifikt behöver toochange dem.
3. Kör **Git Bash** från hello **Start-menyn** > **Git** > **Git Bash**. hello konsolen ser ut ungefär toohello följande exempel:

    ![Git för Windows Bash-gränssnitt](./media/ssh-from-windows/git-bash-window.png)

## <a name="create-a-private-key"></a>Skapa en privat nyckel
1. I din **Git Bash** fönster, Använd `openssl.exe` toocreate en privat nyckel. hello följande exempel skapas en nyckel som heter `myPrivateKey` och certifikatet med namnet `myCert.pem`:

    ```bash
    openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout myPrivateKey.key -out myCert.pem
    ```

    hello utdata ser ut ungefär toohello följande exempel:

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

   Om bash rapporterar ett fel, försök att öppna en ny **Git Bash** fönster med förhöjda privilegier. Sedan kör hello `openssl` kommando.

2. Svaret hello efterfrågar landets namn, plats, organisationsnamn och så vidare.
3. Din nya privata nyckeln och certifikatet skapas i den aktuella arbetskatalogen. Som en säkerhetsåtgärd bör du ange hello behörighet för din privata nyckel så att bara du har åtkomst till den:

    ```bash
    chmod 0600 myPrivateKey.key
    ```

4. Hej [nästa avsnitt](#create-a-private-key-for-putty) information med hjälp av PuTTYgen tooboth visa och använda hello offentliga nyckel och skapa en privat nyckel som är specifika för att använda PuTTY tooSSH tooLinux virtuella datorer. hello följande kommando skapar en offentlig nyckelfil med namnet `myPublicKey.key` som du kan använda direkt:

    ```bash
    openssl.exe rsa -pubout -in myPrivateKey.key -out myPublicKey.key
    ```

5. Om du behöver också toomanage klassiska resurser kan konvertera hello `myCert.pem` för`myCert.cer` (DER-kodad X509 certifikat). Utför det här valfria steget endast om du behöver toospecifically hantera äldre klassiska resurser.

    Konvertera hello certifikat med hjälp av hello följande kommando:

    ```bash
    openssl.exe  x509 -outform der -in myCert.pem -out myCert.cer
    ```

## <a name="create-a-private-key-for-putty"></a>Skapa en privat nyckel för PuTTY
PuTTY är en gemensam SSH-klient för Windows. Du är ledigt toouse SSH-klienten som du vill. toouse PuTTY måste toocreate ytterligare en typ av nyckel - en PuTTY privata nyckel (PPK). Hoppa över det här avsnittet om du inte vill att toouse PuTTY.

hello skapas följande exempel den här ytterligare privata nyckeln för PuTTY toouse:

1. Använd **Git Bash** tooconvert din privata nyckel till en privat RSA-nyckel att PuTTYgen kan förstå. hello följande exempel skapas en nyckel som heter `myPrivateKey_rsa` från hello befintlig nyckel som heter `myPrivateKey`:

    ```bash
    openssl rsa -in ./myPrivateKey.key -out myPrivateKey_rsa
    ```

    Som en säkerhetsåtgärd bör du ange hello behörighet för din privata nyckel så att bara du har åtkomst till den:

    ```bash
    chmod 0600 myPrivateKey_rsa
    ```
2. Hämta och köra PuTTYgen från hello följande plats: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
3. Klicka på hello: **filen** > **belastningen privat nyckel**
4. Leta upp din privata nyckel (`myPrivateKey_rsa` i hello föregående exempel). hello standardkatalogen när du startar **Git Bash** är `C:\Users\%username%`. Ändra hello filen filter tooshow **alla filer (\*.\*)** :

    ![Läsa in hello befintlig privat nyckel i PuTTYgen](./media/ssh-from-windows/load-private-key.png)
5. Klicka på **öppna**. En uppmaning anger hello nyckeln har importerats:

    ![Har importerats viktiga tooPuTTYgen](./media/ssh-from-windows/successfully-imported-key.png)
6. Klicka på **OK** tooclose hello prompten.
7. hello offentliga nyckeln visas överst hello i hello **PuTTYgen** fönster. Kopiera och klistra in den här offentliga nyckeln i hello Azure-portalen eller Azure Resource Manager-mall när du skapar en Linux VM. Du kan också klicka på **spara offentlig nyckel** toosave en kopia tooyour dator:

    ![Spara PuTTY fil för offentlig nyckel](./media/ssh-from-windows/save-public-key.png)

    hello följande exempel visas hur du kopiera och klistra in den här offentliga nyckeln i hello Azure-portalen när du skapar en Linux VM. hello offentliga nyckel är vanligtvis lagras sedan i `~/.ssh/authorized_keys` på den nya virtuella datorn.

    ![Använd en offentlig nyckel när du skapar en virtuell dator i hello Azure-portalen](./media/ssh-from-windows/use-public-key-azure-portal.png)
8. Tillbaka i **PuTTYgen**, klickar du på **Spara privat nyckel**:

    ![Spara filen med PuTTY privata nyckel](./media/ssh-from-windows/save-ppk-file.png)

   > [!WARNING]
   > En uppmaning som frågar om du vill toocontinue utan att ange en lösenfras för nyckeln. Det är en lösenfras som lösenord bifogade tooyour privat nyckel. Även om någon har tooobtain din privata nyckel de fortfarande inte kan tooauthenticate med bara hello-nyckel. De måste också hello lösenfras. Utan en lösenfras om någon erhåller din privata nyckel, kan de logga in tooany VM eller tjänst som använder nyckeln. Vi rekommenderar att du skapar en lösenfras. Men om du glömmer bort hello lösenfrasen finns inget sätt toorecover den.
   >
   >

    Om du vill tooenter en lösenfras klickar du på **nr**, ange en lösenfras i PuTTYgen hello huvudfönstret och klicka sedan på **Spara privat nyckel** igen. Annars klickar du på **Ja** toocontinue utan att ange hello valfri lösenfras.
9. Ange ett namn och plats toosave PPK-filen.

## <a name="use-putty-toossh-tooa-linux-machine"></a>Använd Putty tooSSH tooa Linux-dator
PuTTY är igen, en vanliga SSH-klient för Windows. Du är ledigt toouse SSH-klienten som du vill. Hej följande steg detalj hur toouse din privata nyckel tooauthenticate med din Azure-VM via SSH. hello stegen är ungefär i andra viktiga SSH-klienter som behöver tooload din privata nyckel tooauthenticate hello SSH-anslutning.

1. Hämta och kör putty från hello följande plats: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2. Fyll i hello värdnamn eller IP-adressen för den virtuella datorn från hello Azure-portalen:

    ![Öppna ny PuTTY anslutning](./media/ssh-from-windows/putty-new-connection.png)
3. Innan du väljer **öppna**, klickar du på **anslutning** > **SSH** > **Auth** fliken. Bläddra tooand Välj din privata nyckel:

    ![Välj din PuTTY privata nyckel för autentisering](./media/ssh-from-windows/putty-auth-dialog.png)
4. Klicka på **öppna** tooconnect tooyour virtuell dator

## <a name="next-steps"></a>Nästa steg
Du kan också generera hello offentliga och privata nycklar [använder OS X- och Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Mer information om Bash för Windows och hello fördelarna med OSS verktyg är tillgänglig på Windows-dator finns [Bash på Ubuntu på Windows](https://msdn.microsoft.com/commandline/wsl/about).

Om du har problem med att använda SSH tooconnect tooyour virtuella Linux-datorer, se [felsökning av SSH-anslutningar tooan virtuella Azure Linux-datorn](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
