---
title: aaaStorSimple Snapshot Manager och volymer | Microsoft Docs
description: "Beskriver hur toouse hello StorSimple Snapshot Manager MMC snapin-modulen tooview och hantera volymer och tooconfigure säkerhetskopieringar."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 78896323-e57c-431e-bbe2-0cbde1cf43a2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/18/2016
ms.author: v-sharos
ms.openlocfilehash: b8ce102d082b7c773d667a9d3ec747be9e1d3064
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-tooview-and-manage-volumes"></a>Använda StorSimple Snapshot Manager tooview och hantera volymer
## <a name="overview"></a>Översikt
Du kan använda hello StorSimple Snapshot Manager **volymer** nod (på hello **omfång** fönstret) tooselect volymer och visa information om dessa. hello volymer visas som enheter som motsvarar toohello volymer som monterats av hello värden. Hej **volymer** noden visar lokala volymer och volymtyper som stöds av StorSimple, inklusive volymer som identifierats via hello användning av iSCSI- och en enhet. 

Mer information om stöds volymer finns för[stöd för flera volymtyper av](storsimple-what-is-snapshot-manager.md#support-for-multiple-volume-types).

![Lista över volymen i resultatfönstret](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Volume_node.png)

Hej **volymer** nod kan du skanna om eller ta bort volymer när StorSimple Snapshot Manager identifierar dem också. 

Den här självstudiekursen beskriver hur du kan montera, initiera, och formatera volymer och sedan använda StorSimple Snapshot Manager till:

* Visa information om volymer 
* Ta bort volymer
* Skanna volymer 
* Konfigurera en enkel volym och säkerhetskopiera den
* Konfigurera en dynamisk speglad volym och säkerhetskopiera den

> [!NOTE]
> Alla hello **volym** noden åtgärder är också tillgängliga i hello **åtgärder** fönstret.
> 
> 

## <a name="mount-volumes"></a>Montera volymer
Använd hello följande procedur toomount, initiera och formatera StorSimple-volymer. Den här proceduren använder Diskhantering, ett systemverktyg som används för att hantera hårddiskar och hello motsvarande volymer eller partitioner. Mer information om Diskhantering finns för[Diskhantering](https://technet.microsoft.com/library/cc770943.aspx) på hello Microsoft TechNet-webbplatsen.

#### <a name="toomount-volumes"></a>toomount volymer
1. Starta hello Microsoft iSCSI-initieraren på din värddator.
2. Ange hello gränssnittet IP-adresser som hello målportal eller identifiering av IP-adress och ansluta toohello enhet. När hello-enheten är ansluten vara hello volymer tillgängliga tooyour Windows-system. Mer information om hur du använder hello Microsoft iSCSI-initieraren go toohello avsnittet ”ansluta tooan iSCSI target enhet” i [installera och konfigurera Microsoft iSCSI Initiator][1].
3. Använd någon av följande alternativ toostart Diskhantering hello:
   
   * Ange Diskmgmt.msc hello **kör** rutan.
   * Starta Serverhanteraren, expandera hello **lagring** och sedan väljer **Diskhantering**.
   * Starta **Administrationsverktyg**, expandera hello **Datorhantering** och sedan väljer **Diskhantering**. 
     
     > [!NOTE]
     > Du måste använda administratör privilegier toorun Diskhantering.
     > 
     > 
4. Ta hello volymerna online:
   
   1. Högerklicka på den volym som markerats i Diskhantering **Offline**.
   2. Klicka på **reaktivera Disk**. hello markeras **Online** när hello disk återaktiveras.
5. Initiera hello volymerna:
   
   1. Högerklicka på hello identifierade volymer.
   2. Välj på menyn hello **initiera Disk**.
   3. I hello **initiera Disk** dialogrutan, Välj hello diskar du vill tooinitialize och klicka sedan på **OK**.
6. Formatera enkla volymer:
   
   1. Högerklicka på en volym som du vill tooformat.
   2. Välj på menyn hello **ny enkel volym**.
   3. Använd hello ny enkel volym guiden tooformat hello volymen:
      
      * Ange hello volymens storlek.
      * Ange en enhetsbeteckning.
      * Välj hello NTFS-filsystemet.
      * Ange en storlek på allokeringsenheterna på 64 KB.
      * Utför en snabbformatering.
7. Formatera partition flera volymer. Anvisningar finns i avsnittet toohello ”partitioner och volymer” [implementera Diskhantering](https://msdn.microsoft.com/library/dd163556.aspx).

## <a name="view-information-about-your-volumes"></a>Visa information om dina volymer
Använd följande procedur tooview information om lokala och Azure StorSimple-volymer hello.

#### <a name="tooview-volume-information"></a>tooview volyminformation
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager. 
2. I hello **omfång** rutan klickar du på hello **volymer** nod. En lista över lokala och monterade volymer, inklusive alla Azure StorSimple-volymer, som visas i hello **resultat** fönstret. Hej kolumner i hello **resultat** rutan kan konfigureras. (Högerklicka på hello **volymer** väljer **visa**, och välj sedan **Lägg till/ta bort kolumner**.)
   
    ![Konfigurera hello kolumner](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_View_volumes.png)
   
   | Resultaten kolumn | Beskrivning |
   |:--- |:--- |
   |  Namn |Hej **namn** kolumnen innehåller hello enhet enhetsbeteckningen tooeach identifierade volym. |
   |  Enhet |Hej **enhet** kolumnen innehåller hello hello enhet ansluten toohello värden datorns IP-adress. |
   |  Enheten volymnamn |Hej **volym enhetsnamn** kolumnen innehåller hello namnet på hello enheten volym toowhich hello valda volymen tillhör. Detta är hello volymnamn som definierats i hello Azure-portalen för den specifika volymen. |
   |  Sökvägar för åtkomst |Hej **åtkomst sökvägar** hello åtkomst sökvägen toohello volymen visas i kolumnen. Detta är hello enhet enhetsbeteckning eller monteringspunkt punkt vid vilken hello volymen är tillgänglig på hello värddator. |

## <a name="delete-a-volume"></a>Ta bort en volym
Använd följande procedur toodelete en volym från StorSimple Snapshot Manager hello.

> [!NOTE]
> Du kan inte ta bort en volym om det är en del av en volym-grupp. (hello borttagningsalternativet är inte tillgänglig för volymer som är medlemmar i en grupp för volymen.) Du måste ta bort hello hela volymen grupp toodelete hello volym.

#### <a name="toodelete-a-volume"></a>toodelete en volym
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
2. I hello **omfång** rutan klickar du på hello **volymer** nod. 
3. I hello **resultat** fönstret, högerklicka på hello volym som du vill toodelete.
4. På menyn hello **ta bort**. 
   
    ![Ta bort en volym](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Delete_volume.png) 
5. Hej **ta bort volym** dialogrutan visas. Typen **Bekräfta** i hello textrutan och klicka sedan på **OK**.
6. Som standard säkerhetskopierar StorSimple Snapshot Manager en volym innan den tas bort. Den här försiktighetsåtgärden skydda du förlorar data om hello borttagning släpptes avsiktligt. StorSimple Snapshot Manager visar en **automatisk ögonblicksbild** pågående meddelande medan den säkerhetskopierar hello volym. 
   
    ![Automatisk ögonblicksbild meddelande](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Automatic_snap.png) 

## <a name="rescan-volumes"></a>Skanna volymer
Använd följande procedur toorescan hello volymer anslutna tooStorSimple Snapshot Manager hello.

#### <a name="toorescan-hello-volumes"></a>toorescan hello volymer
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
2. I hello **omfång** fönstret, högerklicka på **volymer**, och klicka sedan på **skanna volymer**.
   
    ![Skanna volymer](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Rescan_volumes.png)
   
    Den här proceduren synkroniserar hello volymlistan med StorSimple Snapshot Manager. Alla ändringar, till exempel nya volymer eller borttagna volymer återspeglas i hello resultat.

## <a name="configure-and-back-up-a-basic-volume"></a>Konfigurera och säkerhetskopiera en enkel volym
Använd följande procedur tooconfigure en säkerhetskopia av en enkel volym hello och starta en säkerhetskopiering direkt eller skapa en princip för schemalagda säkerhetskopieringar.

### <a name="prerequisites"></a>Krav
Innan du börjar:

* Kontrollera att hello StorSimple-enhet och värddatorn är korrekt konfigurerade. Mer information finns för[distribuera din lokala StorSimple-enhet](storsimple-deployment-walkthrough-u2.md).
* Installera och konfigurera StorSimple Snapshot Manager. Mer information finns för[distribuera StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).

#### <a name="tooconfigure-backup-of-a-basic-volume"></a>tooconfigure säkerhetskopiering av en enkel volym
1. Skapa en enkel volym på hello StorSimple-enhet.
2. Montera, initiera och formatera hello volym enligt beskrivningen i [montera volymer](#mount-volumes). 
3. Klicka på hello StorSimple Snapshot Manager ikon på skrivbordet. Hej öppnas StorSimple Snapshot Manager. 
4. I hello **omfång** fönstret, högerklicka på hello **volymer** och sedan väljer **skanna volymer**. När hello genomsökningen är klar visas en lista över volymer som ska visas i hello **resultat** fönstret. 
5. I hello **resultat** fönstret, högerklicka på hello volymen och välj sedan **skapa volymen grupp**. 
   
    ![Skapa volymen grupp](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Create_volume_group.png) 
6. I hello **skapa volymen grupp** dialogrutan Skriv ett namn för hello volym gruppen, tilldela volymer tooit och klicka sedan på **OK**.
7. I hello **omfång** rutan Expandera hello **volym grupper** nod. hello ny volym grupp bör visas under hello **volym grupper** nod. 
8. Högerklicka på hello volym gruppnamn.
   
   * toostart en interaktiv (på begäran) säkerhetskopieringsjobb, klickar du på **ta säkerhetskopiering**. 
   * tooschedule en automatisk säkerhetskopiering klickar du på **skapa säkerhetskopieringsprincip**. På hello **allmänna** markerar du en volym grupp hello-listan. På hello **schema** anger hello Schemadetaljer. När du är klar klickar du på **OK**. 
9. tooconfirm som hello säkerhetskopieringsjobbet har startats, expandera hello **jobb** nod i hello **omfång** rutan och klicka sedan på hello **kör** nod. hello lista över jobb som körs för närvarande visas i hello **resultat** fönstret. 

## <a name="configure-and-back-up-a-dynamic-mirrored-volume"></a>Konfigurera och säkerhetskopiera en dynamisk speglad volym
Utför följande steg tooconfigure säkerhetskopiering av en dynamisk speglad volym hello:

* Steg 1: Använd Disk Management toocreate en dynamisk speglad volym. 
* Steg 2: Använd StorSimple Snapshot Manager tooconfigure säkerhetskopiering.

### <a name="prerequisites"></a>Krav
Innan du börjar:

* Kontrollera att hello StorSimple-enhet och värddatorn är korrekt konfigurerade. Mer information finns för[distribuera din lokala StorSimple-enhet](storsimple-8000-deployment-walkthrough-u2.md).
* Installera och konfigurera StorSimple Snapshot Manager. Mer information finns för[distribuera StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).
* Konfigurera två volymer på hello StorSimple-enhet. (I hello exempel hello tillgängliga volymer är **Disk 1** och **Disk 2**.) 

### <a name="step-1-use-disk-management-toocreate-a-dynamic-mirrored-volume"></a>Steg 1: Använd Disk Management toocreate en dynamisk speglad volym
Diskhantering är ett systemverktyg som används för att hantera hårddiskar och hello volymer och partitioner som de innehåller. Mer information om Diskhantering finns för[Diskhantering](https://technet.microsoft.com/library/cc770943.aspx) på hello Microsoft TechNet-webbplatsen.

#### <a name="toocreate-a-dynamic-mirrored-volume"></a>toocreate en dynamisk speglad volym
1. Använd någon av följande alternativ toostart Diskhantering hello: 
   
   * Öppna hello **kör** skriver **Diskmgmt.msc**, och tryck på RETUR.
   * Starta Serverhanteraren, expandera hello **lagring** och sedan väljer **Diskhantering**. 
   * Starta **Administrationsverktyg**, expandera hello **Datorhantering** och sedan väljer **Diskhantering**. 
2. Kontrollera att du har två volymer som är tillgängliga på hello StorSimple-enhet. (I exemplet hello hello tillgängliga volymer är **Disk 1** och **Disk 2**.) 
3. Högerklicka i hello Diskhantering i fönstret hello högra kolumn hello nedre fönstret **Disk 1** och välj **ny speglad volym**. 
   
    ![Ny speglad volym](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_New_mirrored_volume.png) 
4. På hello **ny speglad volym** sidan i guiden klickar du på **nästa**.
5. På hello **Välj diskar** väljer **Disk 2** i hello **valda** rutan klickar du på **Lägg till**, och klicka sedan på **nästa**. 
6. På hello **tilldela enhetsbeteckning eller sökväg** sidan acceptera hello standardvärden och klicka sedan på **nästa**. 
7. På hello **Formatera volym** i hello sidan **storlek på allokeringsenhet** väljer **64 kB**. Välj hello **utför en snabbformatering** kryssrutan och klicka sedan på **nästa**. 
8. På hello **Slutför hello ny speglad volym** sidan, granskar du inställningarna och klicka sedan på **Slutför**. 
9. Ett meddelande visas tooindicate som hello grundläggande disk kommer att konvertera tooa dynamisk disk. Klicka på **Ja**.
   
    ![Meddelandet dynamisk disk](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Disk_management_msg.png) 
10. Se till att Disk 1 och 2 disken visas dynamiska speglade volymer i Diskhantering. (**Dynamiska** ska visas i statuskolumnen för hello och hello kapacitet färg bör ändra toored, som anger en speglad volym.) 
    
    ![Disk Management speglad dynamiska diskar](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Verify_dynamic_disks_2.png) 

### <a name="step-2-use-storsimple-snapshot-manager-tooconfigure-backup"></a>Steg 2: Använd StorSimple Snapshot Manager tooconfigure säkerhetskopiering
Använd hello följa proceduren tooconfigure en dynamisk speglad volym och starta en säkerhetskopiering direkt eller skapa en princip för schemalagda säkerhetskopieringar.

#### <a name="tooconfigure-backup-of-a-dynamic-mirrored-volume"></a>tooconfigure säkerhetskopiering av en dynamisk speglad volym
1. Klicka på hello StorSimple Snapshot Manager ikon på skrivbordet. Hej öppnas StorSimple Snapshot Manager. 
2. I hello **omfång** fönstret, högerklicka på hello **volymer** och välj **skanna volymer**. När hello genomsökningen är klar visas en lista över volymer som ska visas i hello **resultat** fönstret. hello dynamisk speglad volym har listats som en enskild volym. 
3. I hello **resultat** , högerklicka på hello dynamisk speglad volym, och klickar sedan på **skapa volymen grupp**. 
4. I hello **skapa volymen grupp** dialogrutan Skriv ett namn för hello volym gruppen, tilldela hello dynamisk speglad volym toothis grupp och klicka sedan på **OK**. 
5. I hello **omfång** rutan Expandera hello **volym grupper** nod. hello ny volym grupp bör visas under hello **volym grupper** nod. 
6. Högerklicka på hello volym gruppnamn. 
   
   * toostart en interaktiv (på begäran) säkerhetskopieringsjobb, klickar du på **ta säkerhetskopiering**. 
   * tooschedule en automatisk säkerhetskopiering klickar du på **skapa säkerhetskopieringsprincip**. På hello **allmänna** sidan, Välj hello volym grupp hello-listan. På hello **schema** anger hello Schemadetaljer. När du är klar klickar du på **OK**. 
7. Du kan övervaka hello säkerhetskopieringsjobb som körs. I hello **omfång** rutan Expandera hello **jobb** noden och klicka sedan på **kör**, hello jobbinformation visas i hello **resultat** fönstret. När hello säkerhetskopieringsjobbet är slutfört hello information är överförda toohello **senaste 24** timmar jobbet lista. 

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[med StorSimple Snapshot Manager tooadminister din StorSimple-lösning](storsimple-snapshot-manager-admin.md).
* Lär dig hur för[använder StorSimple Snapshot Manager toocreate och hantera volymen grupper](storsimple-snapshot-manager-manage-volume-groups.md).

<!--Reference links-->
[1]: https://msdn.microsoft.com/library/ee338480(v=ws.10).aspx
