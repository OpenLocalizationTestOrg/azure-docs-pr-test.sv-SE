---
title: aaaBackup Azure virtuella Windows-datorer | Microsoft Docs
description: "Skydda Windows-datorer genom att säkerhetskopiera dem med Azure Backup."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 1cd3e1940a83aacd160cba3c8613b63b6f3c11d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-windows-virtual-machines-in-azure"></a>Säkerhetskopiera Windows-datorer i Azure

Du kan skydda dina data genom att säkerhetskopiera med jämna mellanrum. Azure-säkerhetskopiering skapar återställningspunkter som är lagrade i geo-redundant recovery-valv. När du återställer från en återställningspunkt kan du återställa hello hela VM eller bara vissa filer. Den här artikeln förklarar hur toorestore en enda fil tooa VM kör Windows Server och IIS. Om du inte redan har en VM-toouse kan du skapa en med hjälp av hello [Windows quickstart](quick-create-portal.md). I den här självstudiekursen får du lära du dig att:

> [!div class="checklist"]
> * Skapa en säkerhetskopia av en virtuell dator
> * Schemalägga en daglig säkerhetskopiering
> * Återställa en fil från en säkerhetskopia




## <a name="backup-overview"></a>Översikt över Backup

När hello Azure Backup-tjänsten startar ett säkerhetskopieringsjobb, utlöser hello reservanknytning tootake en tidpunkt i ögonblicksbild. hello Azure Backup-tjänsten använder hello _VMSnapshot_ tillägg. hello tillägget installeras under hello första säkerhetskopiering om hello VM körs. Om hello inte körs, tar en ögonblicksbild av hello underliggande lagring (eftersom inga program skrivningar sker när hello VM stoppas) hello Backup-tjänsten.

När en ögonblicksbild av virtuella Windows-datorer, samordnar hello Backup-tjänsten med hello Volume Shadow Copy Service (VSS) tooget en programkonsekvent ögonblicksbild av hello virtuella datorns diskar. När hello Azure Backup-tjänsten tar hello ögonblicksbild, är hello data överförda toohello valvet. toomaximize effektivitet hello-tjänsten identifierar och överför bara hello datablock som har ändrats sedan tidigare hello-säkerhetskopiering.

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

## <a name="recover-a-file"></a>Återställa en fil

Om du tar bort av misstag eller göra ändringar tooa fil, kan du använda filåterställning toorecover hello-fil från din backup-valvet. Filåterställning använder ett skript som körs på hello VM, toomount hello återställningspunkt som lokal enhet. Dessa enheter förblir monterade 12 timmar så att du kan kopiera filer från hello återställningspunkt och återställa dem toohello VM.  

I det här exemplet visar vi hur toorecover hello image-filen som används i hello standardwebbsidan för IIS. 

1. Öppna en webbläsare och Anslut toohello IP-adressen för hello VM tooshow hello IIS standardsida.

    ![Standardwebbsidan för IIS](./media/tutorial-backup-vms/iis-working.png)

2. Ansluta toohello VM.
3. Öppna på hello VM, **Utforskaren** och navigera too\inetpub\wwwroot och ta bort filen hello **iisstart.png**.
4. Uppdatera hello webbläsare toosee som hello bilden på hello standardsida för IIS är borta på den lokala datorn.

    ![Standardwebbsidan för IIS](./media/tutorial-backup-vms/iis-broken.png)

5. På den lokala datorn, öppna en ny flik och gå hello hello [Azure-portalen](https://portal.azure.com).
6. Välj hello menyn hello vänster **virtuella datorer** och välj hello VM formuläret hello lista.
8. Hello VM bladet i hello **inställningar** klickar du på **säkerhetskopiering**. Hej **säkerhetskopiering** blad öppnas. 
9. Välj hello menyn hello överst på bladet hello **filåterställning**. Hej **filåterställning** blad öppnas.
10. I **steg 1: Välj återställningspunkt**, Välj en återställningspunkt hello i listrutan.
11. I **steg 2: hämta skriptet toobrowse och återställa filerna**, klicka på hello **hämta körbara** knappen. Spara hello filen tooyour **hämtar** mapp.
12. Öppna på den lokala datorn **Utforskaren** och navigera tooyour **hämtar** mappen och kopiera hello hämtas .exe-fil. hello-filnamn ska föregås av VM-namn. 
13. Klistra in hello .exe-fil toohello skrivbordet för den virtuella datorn på den virtuella datorn (via hello RDP-anslutning). 
14. Navigera toohello skrivbordet för den virtuella datorn och dubbelklicka på hello .exe. Kommer att starta en kommandotolk och sedan montera hello återställningspunkt som en filresurs som du kan komma åt. När du är klar skapar hello resursen, Skriv **q** tooclose hello-kommandotolk.
15. Öppna på den virtuella datorn **Utforskaren** och navigera toohello enhetsbeteckning som användes för hello filresurs.
16. Navigera too\inetpub\wwwroot och kopiera **iisstart.png** från hello filen dela och klistra in den i \inetpub\wwwroot. Till exempel kopiera F:\inetpub\wwwroot\iisstart.png och klistra in den i c:\inetpub\wwwroot toorecover hello-filen.
17. På den lokala datorn öppnar du hello webbläsarflik när du är ansluten toohello IP-adressen för hello VM som visar hello IIS standardsida. Tryck på CTRL + F5 toorefresh hello Webbläsarsida. Du bör nu se att hello avbildningen har återställts.
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









