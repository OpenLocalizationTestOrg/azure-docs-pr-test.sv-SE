---
title: aaaCreate en Oracle-databas i en Azure VM | Microsoft Docs
description: "Snabbt en Oracle-databas 12c-databas och körs i Azure-miljön."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/17/2017
ms.author: rclaus
ms.openlocfilehash: 83205154c3275d5f57b46c8acfb0cb4e5c68a412
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-oracle-database-in-an-azure-vm"></a>Skapa en Oracle-databas i en virtuell dator i Azure

Den här guiden information med hjälp av hello Azure CLI toodeploy en virtuell Azure-dator från hello [Oracle marketplace-avbildning i galleriet](https://azuremarketplace.microsoft.com/marketplace/apps/Oracle.OracleDatabase12102EnterpriseEdition?tab=Overview) ordning toocreate en Oracle 12 c-databas. När hello server har distribuerats, ansluter du via SSH i ordning tooconfigure hello Oracle-databas. 

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt denna Snabbstart kräver att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando. En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras. 

hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```
## <a name="create-virtual-machine"></a>Skapa en virtuell dator

toocreate en virtuell dator (VM), Använd hello [az vm skapa](/cli/azure/vm#create) kommando. 

hello följande exempel skapas en virtuell dator med namnet `myVM`. Det skapar också SSH-nycklar, om de inte redan finns på standardplatsen nyckel. toouse för en specifik uppsättning nycklar använder hello `--ssh-key-value` alternativet.  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --admin-username azureuser \
    --generate-ssh-keys
```

När du har skapat hello VM visar Azure CLI information liknande toohello följande exempel. Observera hello värde för `publicIpAddress`. Du kan använda den här adressen tooaccess hello VM.

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/{snip}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-0D-3A-36-2F-56",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.64.104.241",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="connect-toohello-vm"></a>Ansluta toohello VM

toocreate en SSH-session med hello VM, använda hello följande kommando. Ersätt hello IP-adress med hello `publicIpAddress` värde för den virtuella datorn.

```bash 
ssh <publicIpAddress>
```

## <a name="create-hello-database"></a>Skapa hello-databas

hello Oracle-programvara är installerad på hello Marketplace-avbildning. Skapa en exempeldatabas på följande sätt. 

1.  Växla toohello *oracle* superanvändare och sedan initiera hello-lyssnare för loggning:

    ```bash
    $ sudo su - oracle
    $ lsnrctl start
    ```

    hello-utdata är liknande toohello följande:

    ```bash
    Copyright (c) 1991, 2014, Oracle.  All rights reserved.

    Starting /u01/app/oracle/product/12.1.0/dbhome_1/bin/tnslsnr: please wait...

    TNSLSNR for Linux: Version 12.1.0.2.0 - Production
    Log messages written too/u01/app/oracle/diag/tnslsnr/myVM/listener/alert/log.xml
    Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=myVM.twltkue3xvsujaz1bvlrhfuiwf.dx.internal.cloudapp.net)(PORT=1521)))

    Connecting too(ADDRESS=(PROTOCOL=tcp)(HOST=)(PORT=1521))
    STATUS of hello LISTENER
    ------------------------
    Alias                     LISTENER
    Version                   TNSLSNR for Linux: Version 12.1.0.2.0 - Production
    Start Date                23-MAR-2017 15:32:08
    Uptime                    0 days 0 hr. 0 min. 0 sec
    Trace Level               off
    Security                  ON: Local OS Authentication
    SNMP                      OFF
    Listener Log File         /u01/app/oracle/diag/tnslsnr/myVM/listener/alert/log.xml
    Listening Endpoints Summary...
    (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=myVM.twltkue3xvsujaz1bvlrhfuiwf.dx.internal.cloudapp.net)(PORT=1521)))
    hello listener supports no services
    hello command completed successfully
    ```

2.  Skapa databas för hello:

    ```bash
    dbca -silent \
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

    Det tar några minuter toocreate hello-databasen.

3. Ange variabler för Oracle

Innan du ansluter du behöver tooset två miljövariabler: *ORACLE_HOME* och *ORACLE_SID*.

```bash
ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
ORACLE_SID=cdb1; export ORACLE_SID
```
Du kan också lägga till ORACLE_HOME och ORACLE_SID variabler toohello .bashrc fil. Detta skulle spara hello miljövariabler för framtida inloggningar. Bekräfta hello följande instruktioner har lagts till toohello `~/.bashrc` fil med valfri redigerare.

```bash
# Add ORACLE_HOME. 
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1 
# Add ORACLE_SID. 
export ORACLE_SID=cdb1 
```

## <a name="oracle-em-express-connectivity"></a>Oracle EM snabb anslutning

För ett GUI-hanteringsverktyg som du kan använda tooexplore hello databas, ställa in Oracle EM Express. tooconnect tooOracle EM Express, du måste först konfigurera hello port i Oracle. 

1. Anslut tooyour databasen med hjälp av sqlplus:

    ```bash
    sqlplus / as sysdba
    ```

2. När du är ansluten, ange hello port 5502 för EM Express

    ```bash
    exec DBMS_XDB_CONFIG.SETHTTPSPORT(5502);
    ```

3. Öppna hello behållaren PDB1 om inte redan öppnad men första hello statusen:

    ```bash
    select con_id, name, open_mode from v$pdbs;
    ```

    hello-utdata är liknande toohello följande:

    ```bash
      CON_ID NAME                           OPEN_MODE 
      ----------- ------------------------- ---------- 
      2           PDB$SEED                  READ ONLY 
      3           PDB1                      MOUNT
    ```

4. Om hello OPEN_MODE för `PDB1` är inte läsa skriva, köra hello följande kommandon tooopen PDB1:

   ```bash
    alter session set container=pdb1;
    alter database open;
   ```

Du behöver tootype `quit` tooend hello sqlplus session och skriv `exit` toologout av hello oracle-användare.

## <a name="automate-database-startup-and-shutdown"></a>Automatisera databasen start och stopp

hello Oracle-databas som standard startas inte automatiskt när du startar om hello VM. tooset in hello Oracle-databasen toostart automatiskt, först logga in som rot. Sedan skapa och uppdatera vissa systemfiler.

1. Logga in som rot
    ```bash
    sudo su -
    ```

2.  Med redigeringsprogram favorit redigerar hello filen `/etc/oratab` och ändra hello standard `N` för`Y`:

    ```bash
    cdb1:/u01/app/oracle/product/12.1.0/dbhome_1:Y
    ```

3.  Skapa en fil med namnet `/etc/init.d/dbora` och klistra in hello följande innehåll:

    ```
    #!/bin/sh
    # chkconfig: 345 99 10
    # Description: Oracle auto start-stop script.
    #
    # Set ORA_HOME toobe equivalent too$ORACLE_HOME.
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle

    case "$1" in
    'start')
        # Start hello Oracle databases:
        # hello following command assumes that hello Oracle sign-in
        # will not prompt hello user for any values.
        # Remove "&" if you don't want startup as a background process.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" &
        touch /var/lock/subsys/dbora
        ;;

    'stop')
        # Stop hello Oracle databases:
        # hello following command assumes that hello Oracle sign-in
        # will not prompt hello user for any values.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" &
        rm -f /var/lock/subsys/dbora
        ;;
    esac
    ```

4.  Ändra behörigheter för filer med *chmod* på följande sätt:

    ```bash
    chgrp dba /etc/init.d/dbora
    chmod 750 /etc/init.d/dbora
    ```

5.  Skapa symboliska länkar för start och stopp enligt följande:

    ```bash
    ln -s /etc/init.d/dbora /etc/rc.d/rc0.d/K01dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc3.d/S99dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc5.d/S99dbora
    ```

6.  tootest ändringarna, starta om hello VM:

    ```bash
    reboot
    ```

## <a name="open-ports-for-connectivity"></a>Öppna portar för anslutning

hello sista aktivitet är tooconfigure vissa externa slutpunkter. tooset in hello Azure Nätverkssäkerhetsgruppen som skyddar hello VM, avsluta SSH-sessionen i hello VM (bör ha har dessutom out-of-SSH när omstart i föregående steg). 

1.  tooopen hello endpoint att du använder tooaccess hello Oracle-databas, skapar en Nätverkssäkerhetsgrupp regel med [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create) på följande sätt: 

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup\
        --nsg-name myVmNSG \
        --name allow-oracle \
        --protocol tcp \
        --priority 1001 \
        --destination-port-range 1521
    ```

2.  tooopen hello endpoint att du använder tooaccess Oracle EM Express via fjärranslutning, skapar en Nätverkssäkerhetsgrupp regel med [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create) på följande sätt:

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup \
        --nsg-name myVmNSG \
        --name allow-oracle-EM \
        --protocol tcp \
        --priority 1002 \
        --destination-port-range 5502
    ```

3. Om det behövs, hämta hello offentliga IP-adressen för den virtuella datorn igen med [az nätverket offentliga ip-visa](/cli/azure/network/public-ip#show) på följande sätt:

    ```azurecli-interactive
    az network public-ip show \
        --resource-group myResourceGroup \
        --name myVMPublicIP \
        --query [ipAddress] \
        --output tsv
    ```

4.  Ansluta EM Express från din webbläsare. Kontrollera att din webbläsare är kompatibelt med EM Express (Flash installation krävs): 

    ```
    https://<VM ip address or hostname>:5502/em
    ```

Du kan logga in med hjälp av hello **SYS** konto och kontrollera hello **som sysdba** kryssrutan. Använd hello lösenord **OraPasswd1** som du anger under installationen. 

![Skärmbild av inloggningssidan för hello Oracle OEM Express](./media/oracle-quick-start/oracle_oem_express_login.png)

## <a name="clean-up-resources"></a>Rensa resurser

När du är klar med att utforska din första Oracle-databas på Azure och hello VM inte längre behövs, kan du använda hello [ta bort grupp az](/cli/azure/group#delete) kommandot tooremove hello resursgrupp, virtuell dator och alla relaterade resurser.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nästa steg

Lär dig mer om andra [Oracle-lösningar på Azure](oracle-considerations.md). 

Försök hello [installera och konfigurera Oracle Automated lagringshantering](configure-oracle-asm.md) kursen.
