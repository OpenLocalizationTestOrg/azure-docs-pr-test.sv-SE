---
title: "Säkerhetskopiera virtuella Azure Linux-datorer | Microsoft Docs"
description: "Skydda din virtuella Linux-datorer genom att säkerhetskopiera dem med Azure Backup."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 7c00392d5185a2f067f2ee2717529dcbde1e71f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-linux--virtual-machines-in-azure"></a>Säkerhetskopiera virtuella Linux-datorer i Azure

Du kan skydda dina data genom att säkerhetskopiera med jämna mellanrum. Azure-säkerhetskopiering skapar återställningspunkter som är lagrade i geo-redundant recovery-valv. När du återställer från en återställningspunkt kan du återställa hello hela VM eller bara vissa filer. Den här artikeln förklarar hur toorestore en enda fil tooa Linux VM körs nginx. Om du inte redan har en VM-toouse kan du skapa en med hjälp av hello [Linux quickstart](quick-create-cli.md). I den här självstudiekursen får du lära du dig att:

> [!div class="checklist"]
> * Skapa en säkerhetskopia av en virtuell dator
> * Schemalägga en daglig säkerhetskopiering
> * Återställa en fil från en säkerhetskopia



## <a name="backup-overview"></a>Översikt över Backup

När hello Azure Backup service initierar en säkerhetskopia, utlöser hello reservanknytning tootake en tidpunkt i ögonblicksbild. hello Azure Backup-tjänsten använder hello _VMSnapshotLinux_ tillägget Linux. hello tillägget installeras under hello första säkerhetskopiering om hello VM körs. Om hello inte körs, tar en ögonblicksbild av hello underliggande lagring (eftersom inga program skrivningar sker när hello VM stoppas) hello Backup-tjänsten.

Standard Azure Backup tar en konsekvent filsystemsäkerhetskopia för Linux VM men det kan vara konfigurerade tootake [konsekvent säkerhetskopiering av program med skript före och efter skript framework](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent). När hello Azure Backup-tjänsten tar hello ögonblicksbild, är hello data överförda toohello valvet. toomaximize effektivitet hello-tjänsten identifierar och överför bara hello datablock som har ändrats sedan tidigare hello-säkerhetskopiering.

När hello dataöverföring är klar hello ögonblicksbild tas bort och skapa en återställningspunkt.


## <a name="create-a-backup"></a>Skapa en säkerhetskopia
Skapa en enkel schemalagd daglig säkerhetskopiering tooa Recovery Services-valvet. 

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Välj hello menyn hello vänster **virtuella datorer**. 
3. Välj en VM-tooback in hello listan.
4. Hello VM bladet i hello **inställningar** klickar du på **säkerhetskopiering**. Hej **Aktivera säkerhetskopiering** blad öppnas.
5. I **Recovery Services-valvet**, klickar du på **Skapa nytt** och ange hello namn för hello nytt valv. Ett nytt valv skapas i hello samma resursgrupp och plats som hello virtuell dator.
6. Klicka på **säkerhetskopiera princip**. I det här exemplet hålla hello standardvärden och klicka på **OK**.
7. På hello **Aktivera säkerhetskopiering** bladet, klickar du på **Aktivera säkerhetskopiering**. Detta skapar en daglig säkerhetskopiering baserat på hello standardschemat.
10. toocreate en första återställningspunkten på hello **säkerhetskopiering** bladet och klickar på **Säkerhetskopiera nu**.
11. På hello **säkerhetskopiering nu** bladet Klicka hello kalendern använder hello kalender kontrollen tooselect hello sista dagen återställningspunkten behålls Klicka på **säkerhetskopiering**.
12. I hello **säkerhetskopiering** bladet för den virtuella datorn visas hello antal återställningspunkter som är klar.

    ![Återställningspunkter](./media/tutorial-backup-vms/backup-complete.png)

hello första säkerhetskopieringen tar ungefär 20 minuter. När säkerhetskopieringen är klar går du vidare toohello nästa del av den här kursen.

## <a name="restore-a-file"></a>Återställa en fil

Om du tar bort av misstag eller göra ändringar tooa fil, kan du använda filåterställning toorecover hello-fil från din backup-valvet. Filåterställning använder ett skript som körs på hello VM, toomount hello återställningspunkt som lokal enhet. Dessa enheter förblir monterade 12 timmar så att du kan kopiera filer från hello återställningspunkt och återställa dem toohello VM.  

I det här exemplet visar vi hur toorecover hello standard nginx webbsida /var/www/html/index.nginx-debian.html. hello offentliga IP-adressen för våra VM i det här exemplet är *13.69.75.209*. Du kan hitta hello IP-adress för virtuell dator med hjälp av:

 ```bash 
 az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
 ```

 
1. Öppna en webbläsare och Skriv i hello offentliga IP-adressen för din Virtuella toosee hello nginx standardwebbsidan på den lokala datorn.

    ![Standard nginx-webbsida](./media/tutorial-backup-vms/nginx-working.png)

1. SSH till den virtuella datorn.

    ```bash
    ssh 13.69.75.209
    ```
2. Ta bort /var/www/html/index.nginx-debian.html.

    ```bash
    sudo rm /var/www/html/index.nginx-debian.html
    ```
    
4. Uppdatera hello webbläsaren på den lokala datorn genom att träffa CTRL + F5 toosee som standardsida nginx är borta.

    ![Standard nginx-webbsida](./media/tutorial-backup-vms/nginx-broken.png)
    
1. På den lokala datorn loggar du in toohello [Azure-portalen](https://portal.azure.com/).
6. Välj hello menyn hello vänster **virtuella datorer**. 
7. Välj hello VM hello listan.
8. Hello VM bladet i hello **inställningar** klickar du på **säkerhetskopiering**. Hej **säkerhetskopiering** blad öppnas. 
9. Välj hello menyn hello överst på bladet hello **filåterställning**. Hej **filåterställning** blad öppnas.
10. I **steg 1: Välj återställningspunkt**, Välj en återställningspunkt hello i listrutan.
11. I **steg 2: hämta skriptet toobrowse och återställa filerna**, klicka på hello **hämta körbara** knappen. Spara hello nedladdade filen tooyour lokal dator.
7. Klicka på **hämta skriptet** toodownload hello skriptfilen lokalt.
8. Öppna ett Bash-Kommandotolken och Skriv hello efter, ersätter *Linux_myVM_05-05-2017.sh* med hello rätt sökväg och filnamn för hello-skript som du hämtade *azureuser* med hello användarnamn för hello VM och *13.69.75.209* med hello offentliga IP-adressen för den virtuella datorn.
    
    ```bash
    scp Linux_myVM_05-05-2017.sh azureuser@13.69.75.209:
    ```
    
9. Öppna en SSH-anslutning toohello VM på den lokala datorn.

    ```bash
    ssh 13.69.75.209
    ```
    
10. Lägg till på den virtuella datorn köra behörigheter toohello skriptfilen.

    ```bash
    chmod +x Linux_myVM_05-05-2017.sh
    ```
    
11. På den virtuella datorn kör hello skriptet toomount hello återställningspunkt som ett filsystem.

    ```bash
    ./Linux_myVM_05-05-2017.sh
    ```
    
12. hello utdata från hello skriptet ger du hello sökvägen för hello monteringspunkt. hello utdata ser ut ungefär toothis:

    ```bash
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
                          
    Connecting toorecovery point using ISCSI service...
    
    Connection succeeded!
    
    Please wait while we attach volumes of hello recovery point toothis machine...
                         
    ************ Volumes of hello recovery point and their mount paths on this machine ************

    Sr.No.  |  Disk  |  Volume  |  MountPath 

    1)  | /dev/sdc  |  /dev/sdc1  |  /home/azureuser/myVM-20170505191055/Volume1

    ************ Open File Explorer toobrowse for files. ************

    After recovery, tooremove hello disks and close hello connection toohello recovery point, please click 'Unmount Disks' in step 3 of hello portal.

    Please enter 'q/Q' tooexit...
    ```

12. Kopiera hello nginx standardwebbsidan på den virtuella datorn från hello montera punkt tillbaka toowhere hello filen tas bort.

    ```bash
    sudo cp ~/myVM-20170505191055/Volume1/var/www/html/index.nginx-debian.html /var/www/html/
    ```
    
17. På den lokala datorn öppnar du hello webbläsarflik när du är ansluten toohello IP-adressen för hello VM som visar hello nginx standardsida. Tryck på CTRL + F5 toorefresh hello Webbläsarsida. Du bör nu se att hello standardsida fungerar igen.

    ![Standard nginx-webbsida](./media/tutorial-backup-vms/nginx-working.png)

18. På den lokala datorn, gå tillbaka toohello webbläsarflik hello Azure-portalen och på **steg3: demontera hello diskar efter återställningen** klickar du på hello **demontera diskar** knappen. Om du glömmer bort toodo det här steget är hello anslutning toohello monteringspunkt Stäng automatiskt efter 12 timmar. När dessa 12 timmar måste toodownload ett nytt skript toocreate nya monteringspunkt.


## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen lärde du dig att:

> [!div class="checklist"]
> * Skapa en säkerhetskopia av en virtuell dator
> * Schemalägga en daglig säkerhetskopiering
> * Återställa en fil från en säkerhetskopia

Avancera toohello nästa självstudiekurs toolearn om övervakning av virtuella datorer.

> [!div class="nextstepaction"]
> [Övervaka virtuella datorer](tutorial-monitoring.md)

