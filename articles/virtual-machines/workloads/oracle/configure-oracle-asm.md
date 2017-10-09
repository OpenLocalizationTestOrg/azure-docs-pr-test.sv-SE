---
title: "aaaSet in Oracle ASM på en virtuell Azure Linux-dator | Microsoft Docs"
description: "Snabbt Oracle ASM upp och körs i Azure-miljön."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: RicksterCDN
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/19/2017
ms.author: rclaus
ms.openlocfilehash: d6a7046638e919876477d46943faabcb1872acac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-oracle-asm-on-an-azure-linux-virtual-machine"></a>Ställa in Oracle ASM på en virtuell Azure Linux-dator  

Virtuella datorer i Azure ger en fullständigt konfigurerbara och flexibel datormiljö. Den här självstudiekursen beskriver distributionen av grundläggande Azure virtuella datorer i kombination med hello installation och konfiguration av Oracle Automated Storage Management (ASM).  Lär dig att:

> [!div class="checklist"]
> * Skapa och ansluta tooan VM för Oracle-databas
> * Installera och konfigurera automatisk lagringshantering för Oracle
> * Installera och konfigurera infrastrukturen för Oracle rutnätet
> * Initiera en Oracle ASM-installation
> * Skapa en Oracle-databas som hanteras av ASM


[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="prepare-hello-environment"></a>Förbered hello-miljön

### <a name="create-a-resource-group"></a>Skapa en resursgrupp

toocreate en resursgrupp, använda hello [az gruppen skapa](/cli/azure/group#create) kommando. En Azure-resursgrupp är en logisk behållare i vilka Azure resurser distribueras och hanteras. I det här exemplet en resursgrupp med namnet *myResourceGroup* i hello *eastus* region.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

### <a name="create-a-vm"></a>Skapa en virtuell dator

toocreate en virtuell dator baserat på hello Oracle-databas avbildningen och konfigurera den toouse Oracle ASM använder hello [az vm skapa](/cli/azure/vm#create) kommando. 

hello skapas följande exempel en virtuell dator med namnet myVM som har en Standard_DS2_v2 storlek med fyra bifogade datadiskar på 50 GB. Om de inte redan finns i hello standardplatsen för nyckeln, skapas också SSH-nycklar.  toouse för en specifik uppsättning nycklar använder hello `--ssh-key-value` alternativet.  

   ```azurecli-interactive
   az vm create --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --generate-ssh-keys \
    --data-disk-sizes-gb 50 50 50 50
   ```

När du har skapat hello VM visar Azure CLI information liknande toohello följande exempel. Observera hello värde för `publicIpAddress`. Du kan använda den här adressen tooaccess hello VM.

   ```azurecli
   {
     "fqdns": "",
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
     "location": "eastus",
     "macAddress": "00-0D-3A-36-2F-56",
     "powerState": "VM running",
     "privateIpAddress": "10.0.0.4",
     "publicIpAddress": "13.64.104.241",
     "resourceGroup": "myResourceGroup"
   }
   ```

### <a name="connect-toohello-vm"></a>Ansluta toohello VM

toocreate en SSH-session med hello VM och konfigurera ytterligare inställningar kan använda följande kommando hello. Ersätt hello IP-adress med hello `publicIpAddress` värde för den virtuella datorn.

```bash 
ssh <publicIpAddress>
```

## <a name="install-oracle-asm"></a>Installera Oracle ASM

tooinstall Oracle ASM, fullständig hello följande steg. 

Mer information om hur du installerar Oracle ASM finns [Oracle ASMLib laddar ned för Oracle Linux 6](http://www.oracle.com/technetwork/server-storage/linux/asmlib/ol6-1709075.html).  

1. Du behöver toologin som rot i ordning toocontinue med ASM installation:

   ```bash
   sudo su -
   ```
   
2. Kör kommandona ytterligare tooinstall Oracle ASM komponenter:

   ```bash
    yum list | grep oracleasm 
    yum -y install kmod-oracleasm.x86_64 
    yum -y install oracleasm-support.x86_64 
    wget http://download.oracle.com/otn_software/asmlib/oracleasmlib-2.0.12-1.el6.x86_64.rpm 
    yum -y install oracleasmlib-2.0.12-1.el6.x86_64.rpm 
    rm -f oracleasmlib-2.0.12-1.el6.x86_64.rpm
   ```

3. Kontrollera att Oracle ASM är installerad:

   ```bash
   rpm -qa |grep oracleasm
   ```

    hello utdata från kommandot ska visa en lista över hello följande komponenter:

    ```bash
   oracleasm-support-2.1.10-4.el6.x86_64
   kmod-oracleasm-2.0.8-15.el6_9.x86_64
   oracleasmlib-2.0.12-1.el6.x86_64
    ```

4. ASM kräver särskilda användare och roller i ordning toofunction korrekt. hello följande kommandon skapar hello krävs användarkonton och grupper: 

   ```bash
    groupadd -g 54345 asmadmin 
    groupadd -g 54346 asmdba 
    groupadd -g 54347 asmoper 
    useradd -u 3000 -g oinstall -G dba,asmadmin,asmdba,asmoper grid 
    usermod -g oinstall -G dba,asmdba,asmadmin oracle
   ```

5. Kontrollera att användare och grupper har skapats på rätt sätt:

   ```bash
   id grid
   ```

    hello utdata från kommandot ska visa en lista över hello följande användare och grupper:

    ```bash
    uid=3000(grid) gid=54321(oinstall) groups=54321(oinstall),54322(dba),54345(asmadmin),54346(asmdba),54347(asmoper)
    ```
 
6. Skapa en mapp för användaren *rutnätet* och ändra hello ägare:

   ```bash
   mkdir /u01/app/grid 
   chown grid:oinstall /u01/app/grid
   ```

## <a name="set-up-oracle-asm"></a>Ställ in Oracle ASM

I den här självstudien är hello standardanvändaren *rutnätet* och hello standardgruppen är *asmadmin*. Se till att hello *oracle* användaren är en del av hello asmadmin grupp. tooset in Oracle ASM-installation, fullständig hello följande steg:

1. Konfigurera hello Oracle ASM biblioteket drivrutinen innebär att definiera hello standardanvändaren (rutnätet) och standardgruppen (asmadmin) samt konfigurera hello enhet toostart på Start (Välj y) och tooscan diskar på Start (Välj y). Du behöver tooanswer hello anvisningarna från hello följande kommando:

   ```bash
   /usr/sbin/oracleasm configure -i
   ```

   hello utdata från kommandot ska se ut liknande toohello efter, stoppar med prompter toobe besvaras.

    ```bash
   Configuring hello Oracle ASM library driver.

   This will configure hello on-boot properties of hello Oracle ASM library
   driver. hello following questions will determine whether hello driver is
   loaded on boot and what permissions it will have. hello current values
   will be shown in brackets ('[]'). Hitting <ENTER> without typing an
   answer will keep that current value. Ctrl-C will abort.

   Default user tooown hello driver interface []: grid
   Default group tooown hello driver interface []: asmadmin
   Start Oracle ASM library driver on boot (y/n) [n]: y
   Scan for Oracle ASM disks on boot (y/n) [y]: y
   Writing Oracle ASM library driver configuration: done
   ```

2. Visa hello diskkonfiguration:
   ```bash
   cat /proc/partitions
   ```

   hello utdata från kommandot ska se ut ungefär toohello följande lista över tillgängliga diskar

   ```bash
   8       16   14680064 sdb
   8       17   14678976 sdb1
   8        0   52428800 sda
   8        1     512000 sda1
   8        2   51915776 sda2
   8       48   52428800 sdd
   8       64   52428800 sde
   8       80   52428800 sdf
   8       32   52428800 sdc
   11       0       1152 sr0
   ```

3. Formatera disk */dev/sdc* genom att köra hello följande kommando och svara på hello prompter med:
   - *n*för nya partitionen
   - *p* för primär partition
   - *1* tooselect hello första partitionen
   - Tryck på `enter` för hello standard första cylinder
   - Tryck på `enter` för hello standard senaste cylinder
   - Tryck på *w* toowrite hello ändringar toohello partitionstabellen  

   ```bash
   fdisk /dev/sdc
   ```
   
   Använder hello-svar som anges ovan, ska hello utdata för hello fdisk kommandot se ut hello följande:

   ```bash
   Device contains not a valid DOS partition table, or Sun, SGI or OSF disklabel
   Building a new DOS disklabel with disk identifier 0xf865c6ca.
   Changes will remain in memory only, until you decide toowrite them.
   After that, of course, hello previous content won't be recoverable.

   Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

   hello device presents a logical sector size that is smaller than
   hello physical sector size. Aligning tooa physical sector (or optimal
   I/O) size boundary is recommended, or performance may be impacted.

   WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
           switch off hello mode (command 'c') and change display units to
           sectors (command 'u').

   Command (m for help): n
   Command action
     e   extended
     p   primary partition (1-4)
   p
   Partition number (1-4): 1
   First cylinder (1-6527, default 1):
   Using default value 1
   Last cylinder, +cylinders or +size{K,M,G} (1-6527, default 6527):
   Using default value 6527

   Command (m for help): w
   hello partition table has been altered!

   Calling ioctl() toore-read partition table.
   Syncing disks.
   ```

4. Upprepa hello föregående fdisk kommandot för `/dev/sdd`, `/dev/sde`, och `/dev/sdf`.

5. Kontrollera diskkonfigurationen hello:

   ```bash
   cat /proc/partitions
   ```

   hello hello kommandots utdata ska se ut hello följande:

   ```bash
   major minor  #blocks  name

     8       16   14680064 sdb
     8       17   14678976 sdb1
     8       32   52428800 sdc
     8       33   52428096 sdc1
     8       48   52428800 sdd
     8       49   52428096 sdd1
     8       64   52428800 sde
     8       65   52428096 sde1
     8       80   52428800 sdf
     8       81   52428096 sdf1
     8        0   52428800 sda
     8        1     512000 sda1
     8        2   51915776 sda2
     11       0    1048575 sr0
   ```

6. Kontrollera status för hello Oracle ASM-tjänsten och starta hello Oracle ASM-tjänsten:

   ```bash
   service oracleasm status 
   service oracleasm start
   ```

   hello hello kommandots utdata ska se ut hello följande:
   
   ```bash
   Checking if ASM is loaded: no
   Checking if /dev/oracleasm is mounted: no
   Initializing hello Oracle ASMLib driver:                     [  OK  ]
   Scanning hello system for Oracle ASMLib disks:               [  OK  ]
   ```

7. Skapa Oracle ASM diskar:

   ```bash
   service oracleasm createdisk ASMSP /dev/sdc1 
   service oracleasm createdisk DATA /dev/sdd1 
   service oracleasm createdisk DATA1 /dev/sde1 
   service oracleasm createdisk FRA /dev/sdf1
   ```    

   hello hello kommandots utdata ska se ut hello följande:

   ```bash
   Marking disk "ASMSP" as an ASM disk:                       [  OK  ]
   Marking disk "DATA" as an ASM disk:                        [  OK  ]
   Marking disk "DATA1" as an ASM disk:                       [  OK  ]
   Marking disk "FRA" as an ASM disk:                         [  OK  ]
   ```

8. Visa en lista med Oracle ASM diskar:

   ```bash
   service oracleasm listdisks
   ```   

   hello utdatan hello kommandot lista av hello följande Oracle ASM diskar:

   ```bash
    ASMSP
    DATA
    DATA1
    FRA
   ```

9. Ändra hello lösenord för hello rot, oracle och rutnätet användare. **Anteckna dessa nya lösenord** som du använder dem senare under hello-installationen.

   ```bash
   passwd oracle 
   passwd grid 
   passwd root
   ```

10. Ändra behörigheten för hello-mapp:

   ```bash
   chmod -R 775 /opt 
   chown grid:oinstall /opt 
   chown oracle:oinstall /dev/sdc1 
   chown oracle:oinstall /dev/sdd1 
   chown oracle:oinstall /dev/sde1 
   chown oracle:oinstall /dev/sdf1 
   chmod 600 /dev/sdc1 
   chmod 600 /dev/sdd1 
   chmod 600 /dev/sde1 
   chmod 600 /dev/sdf1
   ```

## <a name="download-and-prepare-oracle-grid-infrastructure"></a>Hämta och förbereda infrastrukturen för Oracle-rutnätet

toodownload och förbereda hello Oracle rutnätet infrastruktur programvara, fullständig hello följande steg:

1. Hämta Oracle rutnätet infrastruktur från hello [hämtningssidan för Oracle ASM](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-linux-download-2240591.html). 

   Under hello hämta med titeln **Oracle-databas 12c Versionsinfrastruktur 1 rutnätet (12.1.0.2.0) för Linux x86-64**, hämta hello två ZIP-filer.

2. Du kan använda säkra kopia Protocol (SCP) toocopy hello filer tooyour VM när du har hämtat hello .zip filer tooyour klientdatorn:

   ```bash
   scp *.zip <publicIpAddress>:.
   ```

3. SSH tillbaka till din Oracle VM i Azure i ordning toomove hello ZIP-filer till hello eller välja mappen. Ändra hello ägare av hello filer:

   ```bash
   ssh <publicIPAddress>
   sudo mv ./*.zip /opt
   cd /opt
   sudo chown grid:oinstall linuxamd64_12102_grid_1of2.zip
   sudo chown grid:oinstall linuxamd64_12102_grid_2of2.zip
   ```

4. Packa upp hello-filer. (Installera hello Linux packa upp verktyget om den inte redan är installerad.)
   
   ```bash
   sudo yum install unzip
   sudo unzip linuxamd64_12102_grid_1of2.zip
   sudo unzip linuxamd64_12102_grid_2of2.zip
   ```

5. Ändra behörigheter:
   
   ```bash
   sudo chown -R grid:oinstall /opt/grid
   ```

6. Uppdatera konfigurerats växlingsutrymme. Oracle rutnätet komponenter måste minst 6,8 GB växlingen utrymme tooinstall rutnätet. standard hello växlingen filstorleken för Oracle Linux-avbildningar i Azure är endast 2 048 MB. Du behöver tooincrease `ResourceDisk.SwapSizeMB` i hello `/etc/waagent.conf` filen och starta om tjänsten för hello WALinuxAgent för hello uppdaterade inställningar tootake effekt. Eftersom det är en skrivskyddad fil, måste toochange filen behörigheter tooenable skrivåtkomst.

   ```bash
   sudo chmod 777 /etc/waagent.conf  
   vi /etc/waagent.conf
   ```
   
   Sök efter `ResourceDisk.SwapSizeMB` och ändra hello värdet för**8192**. Du behöver toopress `insert` typ i hello värde i infogningsläge tooenter **8192** och tryck sedan på `esc` tooreturn toocommand läge. toowrite hello ändringar och avsluta hello filtyp `:wq` och tryck på `enter`.
   
   > [!NOTE]
   > Vi rekommenderar starkt att du alltid använder `WALinuxAgent` tooconfigure växlingsutrymme så att den alltid har skapats på hello tillfälliga hårddisk (tillfällig disk) för bästa prestanda. Mer information om finns [hur tooadd en växling filen i virtuella Linux Azure-datorer](https://support.microsoft.com/en-us/help/4010058/how-to-add-a-swap-file-in-linux-azure-virtual-machines).

## <a name="prepare-your-local-client-and-vm-toorun-x11"></a>Förbereda din lokala klient och VM-toorun x11
Konfigurera Oracle ASM kräver ett grafiskt gränssnitt toocomplete hello installation och konfiguration. Vi använder hello x11 protokollet toofacilitate installationen. Om du använder ett klientsystem (Mac eller Linux) som redan har X11 funktioner aktiverat och konfigurerat – du kan hoppa över den här konfigurationen och konfigurera exklusiv tooWindows datorer. 

1. [Hämta PuTTY](http://www.putty.org/) och [hämta Xming](https://xming.en.softonic.com/) tooyour Windows-dator. Du behöver toocomplete hello installation av båda dessa program med hello standardvärden innan du fortsätter.

2. När du har installerat PuTTY öppna Kommandotolken, ändra till hello PuTTY mappen (till exempel C:\Program Files\PuTTY) och kör `puttygen.exe` ordning toogenerate en nyckel.

3. I PuTTY Nyckelgenerator:
   
   1. Generera en nyckel genom att välja hello `Generate` knappen.
   2. Kopiera hello innehållet på hello nyckel (Ctrl + C).
   3. Välj hello `Save private key` knappen.
   4. Ignorera hello varningen om att skydda hello nyckel med en lösenfras och välj sedan `OK`.

   ![Skärmbild av PuTTY Nyckelgenerator](./media/oracle-asm/puttykeygen.png)

4. I den virtuella datorn köra följande kommandon:

   ```bash
   sudo su - grid
   mkdir .ssh 
   cd .ssh
   ```

5. Skapa en fil med namnet `authorized_keys`. Klistra in hello innehållet i hello nyckel i filen och spara hello-filen.

   > [!NOTE]
   > hello nyckel måste innehålla hello sträng `ssh-rsa`. Hello innehållet i hello nyckel måste också vara en textrad.
   >  

6. Starta PuTTY på din klientsystemet. I hello **kategori** rutan Gå för**anslutning** > **SSH** > **Auth**. I hello **fil för privat nyckel för autentisering** rutan, bläddra toohello nyckel som du skapade tidigare.

   ![Skärmbild av alternativ för hello SSH-autentisering](./media/oracle-asm/setprivatekey.png)

7. I hello **kategori** rutan Gå för**anslutning** > **SSH** > **X11**. Välj hello **aktivera X11 vidarebefordran** kryssrutan.

   ![Skärmbild av hello SSH X11 vidarebefordran alternativ](./media/oracle-asm/enablex11.png)

8. I hello **kategori** rutan Gå för**Session**. Ange den virtuella datorn ASM Oracle `<publicIPaddress>` i dialogrutan hello värd namn, fyller du i en ny `Saved Session` namn och klicka sedan på `Save`.  När du sparat klickar du på `open` tooconnect tooyour Oracle ASM virtuell dator.  hello första gången du ansluter du varnas hello fjärrsystemet inte cachelagras i registret. Klicka på `yes` tooadd det och fortsätta.

   ![Skärmbild av hello PuTTY-sessionsalternativ](./media/oracle-asm/puttysession.png)

## <a name="install-oracle-grid-infrastructure"></a>Installera infrastrukturen för Oracle-rutnätet

tooinstall Oracle rutnätet infrastruktur, fullständig hello följande steg:

1. Logga in som **rutnätet**. (Du ska kunna toosign i utan att ange ett lösenord.) 

   > [!NOTE]
   > Om du kör Windows Kontrollera att du har startat Xming innan du påbörjar installationen hello.

   ```bash
   cd /opt/grid
   ./runInstaller
   ```

   Oracle rutnätet infrastruktur 12c version 1 Installer öppnas. (Det kan ta några minuter för hello installer toostart.)

2. På hello **väljer installationsalternativet** väljer **installera och konfigurera infrastrukturen för Oracle rutnät för en fristående Server**.

   ![Skärmbild av hello installer väljer installationsalternativet sida](./media/oracle-asm/install01.png)

3. På hello **Välj språk för produkten** Kontrollera **engelska** eller hello språk har valts.  Klicka på `next`.

4. På hello **skapa ASM diskgruppen** sidan:
   - Ange ett namn för hello diskgruppen.
   - Under **redundans**väljer **externa**.
   - Under **storlek på allokeringsenhet**väljer **4**.
   - Under **Lägg till diskar**väljer **ORCLASMSP**.
   - Klicka på `next`.

5. På hello **ange ASM lösenord** sidan, Välj hello **använda samma lösenord för dessa konton** alternativet och ange ett lösenord.

   ![Skärmbild av lösenordssidan för hello installer ange ASM](./media/oracle-asm/install04.png)

6. På hello **ange alternativ för hantering av** kan du ha hello alternativet tooconfigure EM molnet kontroll. Vi vill hoppa över det här alternativet – Klicka på `next` toocontinue. 

7. På hello **Privilegierade Operativsystemgrupper** använder hello standardinställningarna. Klicka på `next` toocontinue.

8. På hello **ange installationsplatsen** använder hello standardinställningarna. Klicka på `next` toocontinue.

9. På hello **skapa** ändrar hello inventering Directory för`/u01/app/grid/oraInventory`. Klicka på `next` toocontinue.

   ![Skärmbild av hello installer Skapa sida](./media/oracle-asm/install08.png)

10. På hello **rot skript körning configuration** sidan, Välj hello **kör automatiskt konfigurationsskript** kryssrutan. Markera hello **använder ”rot” användarens autentiseringsuppgifter** alternativet och ange hello rot användarens lösenord.

    ![Skärmbild av konfigurationssidan för hello installer rot skript för körning](./media/oracle-asm/install09.png)

11. På hello **utföra nödvändiga kontrollerar** sidan hello nuvarande konfiguration kommer att misslyckas med fel. Det här är ett förväntat beteende. Välj `Fix & Check Again`.

12. I hello **korrigering skriptet** dialogrutan klickar du på `OK`.

13. På hello **sammanfattning** sidan Granska de angivna inställningarna och klicka sedan på `Install`.

    ![Skärmbild av hello installer sammanfattningssida](./media/oracle-asm/install12.png)

14. Ett varningsmeddelande visas om du konfigurationsskript måste toobe körs som en Privilegierade användare. Klicka på `Yes` toocontinue.

15. På hello **Slutför** klickar du på `Close` toofinish hello installation.

## <a name="set-up-your-oracle-asm-installation"></a>Ställ in Oracle ASM-installation

tooset in Oracle ASM-installation, fullständig hello följande steg:

1. Se till att du fortfarande är inloggad som **rutnätet**, från din X11 session. Du kan behöva toohit `enter` toorevive hello terminal. Starta hello Oracle Automated Storage Management Configuration Assistant:

   ```bash
   cd /u01/app/grid/product/12.1.0/grid/bin
   ./asmca
   ```

   Oracle ASM Configuration Assistant öppnas.

2. I hello **konfigurera ASM: diskgrupper** dialogrutan klickar du på hello `Create` knappen och klicka sedan på `Show Advanced Options`.

3. I hello **skapa diskgruppen** dialogrutan:

   - Ange gruppnamn för hello disk **DATA**.
   - Under **Välj medlem diskar**väljer **ORCL_DATA** och **ORCL_DATA1**.
   - Under **storlek på allokeringsenhet**väljer **4**.
   - Klicka på `ok` toocreate hello diskgruppen.
   - Klicka på `ok` tooclose hello bekräftelsefönstret.

   ![Skärmbild av dialogrutan för hello skapa diskgruppen](./media/oracle-asm/asm02.png)

4. I hello **konfigurera ASM: diskgrupper** dialogrutan klickar du på hello `Create` knappen och klicka sedan på `Show Advanced Options`.

5. I hello **skapa diskgruppen** dialogrutan:

   - Ange gruppnamn för hello disk **FRA**.
   - Under **redundans**väljer **externt (ingen)**.
   - Under **Välj medlem diskar**väljer **ORCL_FRA**.
   - Under **storlek på allokeringsenhet**väljer **4**.
   - Klicka på `ok` toocreate hello diskgruppen.
   - Klicka på `ok` tooclose hello bekräftelsefönstret.

   ![Skärmbild av dialogrutan för hello skapa diskgruppen](./media/oracle-asm/asm04.png)

6. Välj **avsluta** tooclose ASM Configuration Installationsassistenten.

   ![Skärmbild av hello konfigurera ASM: Disk-grupper i dialogrutan Avsluta](./media/oracle-asm/asm05.png)

## <a name="create-hello-database"></a>Skapa hello-databas

hello Oracle-databas som är redan installerad på hello Azure Marketplace-avbildning. toocreate en databas, fullständig hello följande steg:

1. Växla användare toohello Oracle superanvändare och sedan initiera hello-lyssnaren för loggning:

   ```bash
   su - oracle
   cd /u01/app/oracle/product/12.1.0/dbhome_1/bin
   ./dbca
   ```
   Databasen Configuration Assistant öppnas.

2. På hello **Databasåtgärden** klickar du på `Create Database`.

3. På hello **läget Skapa** sidan:

   - Ange ett namn för hello-databasen.
   - För **lagringstyp**, se till att **automatisk hantering av lagring (ASM)** är markerad.
   - För **filer databasplatsen**, Använd hello standard ASM förslag på plats.
   - För **snabb återställning området**, Använd hello standard ASM förslag på plats.
   - Ange en **administratörslösenord** och **Bekräfta lösenord**.
   - Se till att `create as container database` är markerad.
   - Ange en `pluggable database name` värde.

4. På hello **sammanfattning** sidan Granska de angivna inställningarna och klicka sedan på `Finish` toocreate hello-databasen.

   ![Skärmbild av hello sammanfattningssida](./media/oracle-asm/createdb03.png)

5. hello databas har skapats. På hello **Slutför** , har hello alternativet toounlock ytterligare konton toouse databasen och ändra hello lösenord. Om du vill toodo så kan du välja **lösenordshantering** -Annars klickar du på `close`.

## <a name="delete-hello-vm"></a>Ta bort hello VM

Du har konfigurerat Oracle Automated lagringshantering på hello Oracle DB-avbildning från hello Azure Marketplace.  När du inte längre behöver den här virtuella datorn kan du använda hello efter kommandot tooremove hello resursgrupp, VM och alla relaterade resurser:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nästa steg

[Självstudier: Konfigurera Oracle DataGuard](configure-oracle-dataguard.md)

[Självstudier: Konfigurera Oracle GoldenGate](Configure-oracle-golden-gate.md)

Granska [skapa en Oracle-databas](oracle-design.md)
