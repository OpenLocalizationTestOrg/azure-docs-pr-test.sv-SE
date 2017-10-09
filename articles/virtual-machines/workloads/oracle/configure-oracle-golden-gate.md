---
title: "aaaImplement Oracle guld Gate på en Azure Linux-dator | Microsoft Docs"
description: "Snabbt en Oracle guld Gate upp och körs i Azure-miljön."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: v-shiuma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: rclaus
ms.openlocfilehash: 320cafd5d23ee472f0af9f92577bc6f432f65778
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="implement-oracle-golden-gate-on-an-azure-linux-vm"></a>Implementera Oracle guld Gate på en Azure Linux-dator 

hello Azure CLI är används toocreate och hantera Azure-resurser från hello kommandoraden eller i skript. Det här guiden beskriver hur toouse hello Azure CLI toodeploy en Oracle 12c-databas från hello Azure Marketplace-galleriet avbildning. 

Det här dokumentet vägleder dig hur toocreate, installera och konfigurera Oracle guld Gate på en virtuell dator i Azure.

Innan du börjar bör du kontrollera om den hello Azure CLI har installerats. Mer information finns i [installationsguiden för Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="prepare-hello-environment"></a>Förbered hello-miljön

tooperform hello Oracle guld Gate installationen måste toocreate två virtuella Azure-datorer på hello samma tillgänglighetsuppsättning. hello Marketplace-avbildning som du använder toocreate hello virtuella datorer är **Oracle: Oracle-databasen-Ee:12.1.0.2:latest**.

Du behöver toobe bekant med Unix editor vi och har en grundläggande förståelse för x11 (Windows X).

hello nedan följer en sammanfattning av konfigurationen för hello-miljö:
> 
> |  | **Primär plats** | **Replikera plats** |
> | --- | --- | --- |
> | **Oracle-versionen** |Oracle 12c version 2 – (12.1.0.2) |Oracle 12c version 2 – (12.1.0.2)|
> | **Namnet på datorn** |myVM1 |myVM2 |
> | **Operativsystem** |Oracle Linux 6.x |Oracle Linux 6.x |
> | **Oracle SID** |CDB1 |CDB1 |
> | **Schemat för replikering** |TEST|TEST |
> | **Guld Gate ägare/replikera** |C ##GGADMIN |REPUSER |
> | **Guld Gate-processen** |EXTORA |REPORA|


### <a name="sign-in-tooazure"></a>Logga in tooAzure 

Logga in tooyour Azure-prenumeration med hello [az inloggningen](/cli/azure/#login) kommando. Följ hello på skärmen anvisningarna.

```azurecli
az login
```

### <a name="create-a-resource-group"></a>Skapa en resursgrupp

Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando. En Azure-resursgrupp är en logisk behållare i vilka Azure-resurser har distribuerats och från vilken de kan hanteras. 

hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `westus` plats.

```azurecli
az group create --name myResourceGroup --location westus
```

### <a name="create-an-availability-set"></a>Skapa en tillgänglighetsuppsättning

hello följande steg är valfritt men rekommenderas. Mer information finns i [Azure tillgänglighetsuppsättningar guiden](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

### <a name="create-a-virtual-machine"></a>Skapa en virtuell dator

Skapa en virtuell dator med hello [az vm skapa](/cli/azure/vm#create) kommando. 

hello följande exempel skapar två virtuella datorer med namnet `myVM1` och `myVM2`. Skapa SSH-nycklar om de inte redan finns på standardplatsen nyckel. toouse för en specifik uppsättning nycklar använder hello `--ssh-key-value` alternativet.

#### <a name="create-myvm1-primary"></a>Skapa myVM1 (primära):
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM1 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --generate-ssh-keys \
```

Efter hello VM har skapats, visar hello Azure CLI information liknande toohello följande exempel. (Anteckna hello `publicIpAddress`. Den här adressen är används tooaccess hello VM.)

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-0D-3A-36-2F-56",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.64.104.241",
  "resourceGroup": "myResourceGroup"
}
```

#### <a name="create-myvm2-replicate"></a>Skapa myVM2 (replikera):
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM2 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --generate-ssh-keys \
```

Anteckna hello `publicIpAddress` samt när den har skapats.

### <a name="open-hello-tcp-port-for-connectivity"></a>Öppna hello TCP-port för anslutning

hello nästa steg är tooconfigure externa slutpunkter, som gör att du tooaccess hello Oracle-databas från en fjärrdator. tooconfigure hello externa slutpunkter genom att köra följande kommandon hello.

#### <a name="open-hello-port-for-myvm1"></a>Öppna hello port för myVM1:

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm1NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

hello resultat bör se ut ungefär toohello efter svar:

```bash
{
  "access": "Allow",
  "description": null,
  "destinationAddressPrefix": "*",
  "destinationPortRange": "1521",
  "direction": "Inbound",
  "etag": "W/\"bd77dcae-e5fd-4bd6-a632-26045b646414\"",
  "id": "/subscriptions/<subscription-id>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myVmNSG/securityRules/allow-oracle",
  "name": "allow-oracle",
  "priority": 999,
  "protocol": "Tcp",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sourceAddressPrefix": "*",
  "sourcePortRange": "*"
}
```

#### <a name="open-hello-port-for-myvm2"></a>Öppna hello port för myVM2:

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm2NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

### <a name="connect-toohello-virtual-machine"></a>Ansluta toohello virtuell dator

Använd hello följande kommando toocreate en SSH-session med hello virtuell dator. Ersätt hello IP-adress med hello `publicIpAddress` i den virtuella datorn.

```bash 
ssh <publicIpAddress>
```

### <a name="create-hello-database-on-myvm1-primary"></a>Skapa hello-databas på myVM1 (primär)

hello Oracle-programvara är installerad på hello Marketplace-avbildning, så hello nästa steg är tooinstall hello-databas. 

Kör hello programvara som hello 'oracle-superanvändare:

```bash
sudo su - oracle
```

Skapa databas för hello:

```bash
$ dbca -silent \
   -createDatabase \
   -templateName General_Purpose.dbc \
   -gdbname cdb1 \
   -sid cdb1 \
   -responseFile NO_VALUE \
   -characterSet AL32UTF8 \
   -sysPassword OraPasswd1 \
   -systemPassword OraPasswd1 \
   -createAsContainerDatabase true \
   -numberOfPDBs 1 \
   -pdbName pdb1 \
   -pdbAdminPassword OraPasswd1 \
   -databaseType MULTIPURPOSE \
   -automaticMemoryManagement false \
   -storageType FS \
   -ignorePreReqs
```
Utdata ska se ut ungefär toohello efter svar:

```bash
Copying database files
1% complete
2% complete
8% complete
13% complete
19% complete
27% complete
Creating and starting Oracle instance
29% complete
32% complete
33% complete
34% complete
38% complete
42% complete
43% complete
45% complete
Completing Database Creation
48% complete
51% complete
53% complete
62% complete
70% complete
72% complete
Creating Pluggable Databases
78% complete
100% complete
Look at hello log file "/u01/app/oracle/cfgtoollogs/dbca/cdb1/cdb1.log" for more details.
```

Ange hello ORACLE_SID och ORACLE_HOME variabler.

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=gg1; export ORACLE_SID
$ LD_LIBRARY_PATH=ORACLE_HOME/lib; export LD_LIBRARY_PATH
```

Alternativt kan du lägga till ORACLE_HOME och ORACLE_SID toohello .bashrc filen, så att de här inställningarna sparas för framtida inloggningar:

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=gg1
# add Oracle library path
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
```

### <a name="start-oracle-listener"></a>Starta Oracle-lyssnare
```bash
$ sudo su - oracle
$ lsnrctl start
```

### <a name="create-hello-database-on-myvm2-replicate"></a>Skapa hello-databas på myVM2 (replikera)

```bash
sudo su - oracle
```
Skapa databas för hello:

```bash
$ dbca -silent \
   -createDatabase \
   -templateName General_Purpose.dbc \
   -gdbname cdb1 \
   -sid cdb1 \
   -responseFile NO_VALUE \
   -characterSet AL32UTF8 \
   -sysPassword OraPasswd1 \
   -systemPassword OraPasswd1 \
   -createAsContainerDatabase true \
   -numberOfPDBs 1 \
   -pdbName pdb1 \
   -pdbAdminPassword OraPasswd1 \
   -databaseType MULTIPURPOSE \
   -automaticMemoryManagement false \
   -storageType FS \
   -ignorePreReqs
```
Ange hello ORACLE_SID och ORACLE_HOME variabler.

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
$ LD_LIBRARY_PATH=ORACLE_HOME/lib; export LD_LIBRARY_PATH
```

Du kan eventuellt ORACLE_HOME och ORACLE_SID toohello .bashrc filen som läggs till, så att de här inställningarna sparas för framtida inloggningar.

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
# add Oracle library path
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
```

### <a name="start-oracle-listener"></a>Starta Oracle-lyssnare
```bash
$ sudo su - oracle
$ lsnrctl start
```

## <a name="configure-golden-gate"></a>Konfigurera guld Gate 
tooconfigure guld Gate ta hello stegen i det här avsnittet.

### <a name="enable-archive-log-mode-on-myvm1-primary"></a>Aktivera archive log-läge på myVM1 (primär)

```bash
$ sqlplus / as sysdba
SQL> SELECT log_mode FROM v$database;

LOG_MODE
------------
NOARCHIVELOG

SQL> SHUTDOWN IMMEDIATE;
SQL> STARTUP MOUNT;
SQL> ALTER DATABASE ARCHIVELOG;
SQL> ALTER DATABASE OPEN;
```
Aktivera Tvingad loggning och kontrollera att det finns minst en loggfil.

```bash
SQL> ALTER DATABASE FORCE LOGGING;
SQL> ALTER SYSTEM SWITCH LOGFILE;
SQL> ALTER SYSTEM set enable_goldengate_replication=true;
SQL> ALTER PLUGGABLE DATABASE PDB1 OPEN;
SQL> ALTER SESSION SET CONTAINER=PDB1;
SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA;
SQL> EXIT;
```

### <a name="download-golden-gate-software"></a>Hämta guld Gate-programvara
toodownload och förbereda hello Oracle guld Gate programvara, fullständig hello följande steg:

1. Hämta hello **fbo_ggs_Linux_x64_shiphome.zip** filen från hello [hämtningssidan för Oracle guld Gate](http://www.oracle.com/technetwork/middleware/goldengate/downloads/index.html). Hämta rubrik under hello **Oracle GoldenGate 12.x.x.x för Oracle Linux x86-64**, bör det finnas en uppsättning toodownload för ZIP-filer.

2. När du har hämtat hello .zip filer tooyour klientdatorn Använd Secure kopiera Protocol (SCP) toocopy hello filer tooyour VM:

  ```bash
  $ scp fbo_ggs_Linux_x64_shiphome.zip <publicIpAddress>:<folder>
  ```

3. Flytta hello .zip filer toohello **/ opt** mapp. Ändra hello ägare av hello filer på följande sätt:

  ```bash
  $ sudo su -
  # mv <folder>/*.zip /opt
  ```

4. Packa upp hello-filer (installera hello Linux packa verktyget om den inte redan är installerat):

  ```bash
  # yum install unzip
  # cd /opt
  # unzip fbo_ggs_Linux_x64_shiphome.zip
  ```

5. Ändra behörigheter:

  ```bash
  # chown -R oracle:oinstall /opt/fbo_ggs_Linux_x64_shiphome
  ```

### <a name="prepare-hello-client-and-vm-toorun-x11-for-windows-clients-only"></a>Förbereda hello klienten och VM-toorun x11 (för Windows-klienter)
Det här är ett valfritt steg. Du kan hoppa över det här steget om du använder en Linux-klient eller redan har x11 installationen.

1. Hämta PuTTY och Xming tooyour Windows-dator:

  * [Hämta PuTTY](http://www.putty.org/)
  * [Hämta Xming](https://xming.en.softonic.com/)

2.  När du har installerat PuTTY i hello PuTTY mappen (till exempel C:\Program Files\PuTTY) genom att köra puttygen.exe (PuTTY nyckel Generator).

3.  I PuTTY Nyckelgenerator:

  - toogenerate en nyckel, Välj hello **generera** knappen.
  - Kopiera hello innehållet på hello nyckel (**Ctrl + C**).
  - Välj hello **Spara privat nyckel** knappen.
  - Ignorera hello varningen som visas och välj sedan **OK**.

    ![Skärmbild av hello PuTTY nyckelgenerator sidan](./media/oracle-golden-gate/puttykeygen.png)

4.  I den virtuella datorn köra följande kommandon:

  ```bash
  # sudo su - oracle
  $ mkdir .ssh (if not already created)
  $ cd .ssh
  ```

5. Skapa en fil med namnet **authorized_keys**. Klistra in hello innehållet i hello nyckel i filen och spara hello-filen.

  > [!NOTE]
  > hello nyckel måste innehålla hello sträng `ssh-rsa`. Hello innehållet i hello nyckel måste också vara en textrad.
  >  

6. Starta PuTTY. I hello **kategori** väljer **anslutning** > **SSH** > **Auth**. I hello **fil för privat nyckel för autentisering** rutan, bläddra toohello nyckel som du skapade tidigare.

  ![Skärmbild av sidan för hello ange privat nyckel](./media/oracle-golden-gate/setprivatekey.png)

7. I hello **kategori** väljer **anslutning** > **SSH** > **X11**. Välj hello **aktivera X11 vidarebefordran** rutan.

  ![Skärmbild av sidan för hello aktivera X11](./media/oracle-golden-gate/enablex11.png)

8. I hello **kategori** rutan Gå för**Session**. Ange information om hello värden och välj sedan **öppna**.

  ![Skärmbild av hello-sida](./media/oracle-golden-gate/puttysession.png)

### <a name="install-golden-gate-software"></a>Installera guld Gate-programvara

tooinstall Oracle guld Gate, fullständig hello följande steg:

1. Logga in som oracle. (Du ska kunna toosign i utan att ange ett lösenord.) Se till att Xming körs innan du påbörjar installationen hello.
 
  ```bash
  $ cd /opt/fbo_ggs_Linux_x64_shiphome/Disk1
  $ ./runInstaller
  ```
2. Välj 'Oracle GoldenGate för Oracle-databas 12c'. Välj sedan **nästa** toocontinue.

  ![Skärmbild av hello installer Välj installationssidan](./media/oracle-golden-gate/golden_gate_install_01.png)

3. Ändra platsen för hello-programvara. Välj hello **Starthanteraren** och anger hello databasplatsen. Välj **nästa** toocontinue.

  ![Skärmbild av hello Välj installationssidan](./media/oracle-golden-gate/golden_gate_install_02.png)

4. Ändra hello inventering katalogen och välj sedan **nästa** toocontinue.

  ![Skärmbild av hello Välj installationssidan](./media/oracle-golden-gate/golden_gate_install_03.png)

5. På hello **sammanfattning** väljer **installera** toocontinue.

  ![Skärmbild av hello installer Välj installationssidan](./media/oracle-golden-gate/golden_gate_install_04.png)

6. Du kanske ange toorun ett skript som 'rot'. I så fall, öppna en egen session ssh toohello VM, sudo tooroot och kör sedan hello skript. Välj **OK** fortsätta.

  ![Skärmbild av hello Välj installationssidan](./media/oracle-golden-gate/golden_gate_install_05.png)

7. När hello installationen är klar väljer du **Stäng** toocomplete hello processen.

  ![Skärmbild av hello Välj installationssidan](./media/oracle-golden-gate/golden_gate_install_06.png)

### <a name="set-up-service-on-myvm1-primary"></a>Ställ in tjänsten på myVM1 (primär)

1. Skapa eller uppdatera hello tnsnames.ora fil:

  ```bash
  $ cd $ORACLE_HOME/network/admin
  $ vi tnsnames.ora

  cdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=cdb1)
      )
    )

  pdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=pdb1)
      )
    )
  ```

2. Skapa hello guld Gate ägar- och konton.

  > [!NOTE]
  > hello ägare kontot måste ha C ## prefix.
  >

    ```bash
    $ sqlplus / as sysdba
    SQL> CREATE USER C##GGADMIN identified by ggadmin;
    SQL> EXEC dbms_goldengate_auth.grant_admin_privilege('C##GGADMIN',container=>'ALL');
    SQL> GRANT DBA tooC##GGADMIN container=all;
    SQL> connect C##GGADMIN/ggadmin
    SQL> ALTER SESSION SET CONTAINER=PDB1;
    SQL> EXIT;
    ```

3. Skapa hello guld Gate testanvändarkonto:

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ sqlplus system/OraPasswd1@pdb1
  SQL> CREATE USER test identified by test DEFAULT TABLESPACE USERS TEMPORARY TABLESPACE TEMP;
  SQL> GRANT connect, resource, dba tootest;
  SQL> ALTER USER test QUOTA 100M on USERS;
  SQL> connect test/test@pdb1
  SQL> @demo_ora_create
  SQL> @demo_ora_insert
  SQL> EXIT;
  ```

4. Konfigurera hello extrahera parameterfil.

 Starta hello gyllene gate-kommandoradsgränssnittet (ggsci):

  ```bash
  $ sudo su - oracle
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> DBLOGIN USERID test@pdb1, PASSWORD test
  Successfully logged into database  pdb1
  GGSCI>  ADD SCHEMATRANDATA pdb1.test
  2017-05-23 15:44:25  INFO    OGG-01788  SCHEMATRANDATA has been added on schema test.
  2017-05-23 15:44:25  INFO    OGG-01976  SCHEMATRANDATA for scheduling columns has been added on schema test.

  GGSCI> EDIT PARAMS EXTORA
  ```
5. Lägg till hello följande toohello EXTRAHERA parametern (med hjälp av vi kommandon). Tryck på Esc-tangenten ': wq!' toosave-fil. 

  ```bash
  EXTRACT EXTORA
  USERID C##GGADMIN, PASSWORD ggadmin
  RMTHOST 10.0.0.5, MGRPORT 7809
  RMTTRAIL ./dirdat/rt  
  DDL INCLUDE MAPPED
  DDLOPTIONS REPORT 
  LOGALLSUPCOLS
  UPDATERECORDFORMAT COMPACT
  TABLE pdb1.test.TCUSTMER;
  TABLE pdb1.test.TCUSTORD;
  ```
6. Registrera extrahera--integrerad extrahera:

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci

  GGSCI> dblogin userid C##GGADMIN, password ggadmin
  Successfully logged into database CDB$ROOT.

  GGSCI> REGISTER EXTRACT EXTORA DATABASE CONTAINER(pdb1)

  2017-05-23 15:58:34  INFO    OGG-02003  Extract EXTORA successfully registered with database at SCN 1821260.

  GGSCI> exit
  ```
7. Konfigurera extrahera kontrollpunkter och börja realtid extrahera:

  ```bash
  $ ./ggsci
  GGSCI>  ADD EXTRACT EXTORA, INTEGRATED TRANLOG, BEGIN NOW
  EXTRACT (Integrated) added.

  GGSCI>  ADD RMTTRAIL ./dirdat/rt, EXTRACT EXTORA, MEGABYTES 10
  RMTTRAIL added.

  GGSCI>  START EXTRACT EXTORA

  Sending START request tooMANAGER ...
  EXTRACT EXTORA starting

  GGSCI > info all

  Program     Status      Group       Lag at Chkpt  Time Since Chkpt

  MANAGER     RUNNING
  EXTRACT     RUNNING     EXTORA      00:00:11      00:00:04
  ```
I det här steget kan hitta du hello startar Tillståndsändringsavisering som kommer att användas senare i ett annat avsnitt:

  ```bash
  $ sqlplus / as sysdba
  SQL> alter session set container = pdb1;
  SQL> SELECT current_scn from v$database;
  CURRENT_SCN
  -----------
      1857887
  SQL> EXIT;
  ```

  ```bash
  $ ./ggsci
  GGSCI> EDIT PARAMS INITEXT
  ```

  ```bash
  EXTRACT INITEXT
  USERID C##GGADMIN, PASSWORD ggadmin
  RMTHOST 10.0.0.5, MGRPORT 7809
  RMTTASK REPLICAT, GROUP INITREP
  TABLE pdb1.test.*, SQLPREDICATE 'AS OF SCN 1857887'; 
  ```

  ```bash
  GGSCI> ADD EXTRACT INITEXT, SOURCEISTABLE
  ```

### <a name="set-up-service-on-myvm2-replicate"></a>Ställ in tjänsten på myVM2 (replikera)


1. Skapa eller uppdatera hello tnsnames.ora fil:

  ```bash
  $ cd $ORACLE_HOME/network/admin
  $ vi tnsnames.ora

  cdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=cdb1)
      )
    )

  pdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=pdb1)
      )
    )
  ```

2. Skapa ett replikera konto:

  ```bash
  $ sqlplus / as sysdba
  SQL> alter session set container = pdb1;
  SQL> create user repuser identified by rep_pass container=current;
  SQL> grant dba toorepuser;
  SQL> exec dbms_goldengate_auth.grant_admin_privilege('REPUSER',container=>'PDB1');
  SQL> connect repuser/rep_pass@pdb1 
  SQL> EXIT;
  ```

3. Skapa ett användarkonto för Guld Gate-test:

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ sqlplus system/OraPasswd1@pdb1
  SQL> CREATE USER test identified by test DEFAULT TABLESPACE USERS TEMPORARY TABLESPACE TEMP;
  SQL> GRANT connect, resource, dba tootest;
  SQL> ALTER USER test QUOTA 100M on USERS;
  SQL> connect test/test@pdb1
  SQL> @demo_ora_create
  SQL> EXIT;
  ```

4. REPLICAT parametern tooreplicate filändringar: 

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> EDIT PARAMS REPORA  
  ```
  Innehållet i filen som REPORA parameter:

  ```bash
  REPLICAT REPORA
  ASSUMETARGETDEFS
  DISCARDFILE ./dirrpt/repora.dsc, PURGE, MEGABYTES 100
  DDL INCLUDE MAPPED
  DDLOPTIONS REPORT
  DBOPTIONS INTEGRATEDPARAMS(parallelism 6)
  USERID repuser@pdb1, PASSWORD rep_pass
  MAP pdb1.test.*, TARGET pdb1.test.*;
  ```

5. Skapa en kontrollpunkt replicat:

  ```bash
  GGSCI> ADD REPLICAT REPORA, INTEGRATED, EXTTRAIL ./dirdat/rt
  GGSCI> EDIT PARAMS INITREP

  ```

  ```bash
  REPLICAT INITREP
  ASSUMETARGETDEFS
  DISCARDFILE ./dirrpt/tcustmer.dsc, APPEND
  USERID repuser@pdb1, PASSWORD rep_pass
  MAP pdb1.test.*, TARGET pdb1.test.*;   
  ```

  ```bash
  GGSCI> ADD REPLICAT INITREP, SPECIALRUN
  ```

### <a name="set-up-hello-replication-myvm1-and-myvm2"></a>Konfigurera hello replikering (myVM1 och myVM2)

#### <a name="1-set-up-hello-replication-on-myvm2-replicate"></a>1. Ställ in hello replikering på myVM2 (replikera)

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> EDIT PARAMS MGR
  ```
Uppdatera hello-filen med hello följande:

  ```bash
  PORT 7809
  ACCESSRULE, PROG *, IPADDR *, ALLOW
  ```
Starta om hello Manager-tjänsten:

  ```bash
  GGSCI> STOP MGR
  GGSCI> START MGR
  GGSCI> EXIT
  ```

#### <a name="2-set-up-hello-replication-on-myvm1-primary"></a>2. Ställ in hello replikering på myVM1 (primär)

Starta hello första last och Sök efter fel:

```bash
$ cd /u01/app/oracle/product/12.1.0/oggcore_1
$ ./ggsci
GGSCI> START EXTRACT INITEXT
GGSCI> VIEW REPORT INITEXT
```
#### <a name="3-set-up-hello-replication-on-myvm2-replicate"></a>3. Ställ in hello replikering på myVM2 (replikera)

Ändra hello Tillståndsändringsavisering tal med hello tal som du erhållit tidigare:

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  START REPLICAT REPORA, AFTERCSN 1857887
  ```
hello replikering har startat och du kan testa den genom att lägga till nya poster tooTEST tabeller.


### <a name="view-job-status-and-troubleshooting"></a>Visa jobbstatus och felsökning

#### <a name="view-reports"></a>Visa rapporter
tooview rapporter om myVM1, kör hello följande kommandon:

  ```bash
  GGSCI> VIEW REPORT EXTORA 
  ```
 
tooview rapporter om myVM2, kör hello följande kommandon:

  ```bash
  GGSCI> VIEW REPORT REPORA
  ```

#### <a name="view-status-and-history"></a>Visa status och historik
tooview status och historik i myVM1, kör hello följande kommandon:

  ```bash
  GGSCI> dblogin userid c##ggadmin, password ggadmin 
  GGSCI> INFO EXTRACT EXTORA, DETAIL
  ```

tooview status och historik i myVM2, kör hello följande kommandon:

  ```bash
  GGSCI> dblogin userid repuser@pdb1 password rep_pass 
  GGSCI> INFO REP REPORA, DETAIL
  ```
Detta avslutar hello installationen och konfigurationen av guld Gate på Oracle linux.


## <a name="delete-hello-virtual-machine"></a>Ta bort hello virtuell dator

När den inte längre behövs, kan det vara hello följande kommando används tooremove hello resursgrupp, virtuell dator och alla relaterade resurser.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nästa steg

[Självstudie: Skapa virtuella datorer med hög tillgänglighet](../../linux/create-cli-complete.md)

[Utforska exempel på distribution av virtuella datorer med CLI](../../linux/cli-samples.md)
