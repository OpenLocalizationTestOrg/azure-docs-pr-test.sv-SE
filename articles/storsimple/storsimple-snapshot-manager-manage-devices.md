---
title: aaaManage enheter med StorSimple Snapshot Manager | Microsoft Docs
description: Beskriver hur toouse hello StorSimple Snapshot Manager MMC snapin-modulen tooconnect och hantera StorSimple-enheter.
services: storsimple
documentationcenter: 
author: SharS
manager: timlt
editor: 
ms.assetid: 966ecbe3-a7fa-4752-825f-6694dd949946
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: 7a2a2ca830e4ea6eb4b01f2542958df3871c1700
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-tooconnect-and-manage-storsimple-devices"></a>Använda StorSimple Snapshot Manager tooconnect och hantera StorSimple-enheter
## <a name="overview"></a>Översikt
Du kan använda noder i hello StorSimple Snapshot Manager **omfång** fönstret tooverify importerat StorSimple enhetsdata och uppdatera anslutna lagringsenheter. Dessutom, när du klickar på hello **enheter** nod, du kan se en lista över anslutna enheter och motsvarande statusinformation i hello **resultat** fönstret.

![Anslutna enheter](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_connect_devices.png)

**Bild 1: StorSimple Snapshot Manager ansluten enhet** 

Beroende på din **visa** val, hello **resultat** visar hello följande information om varje enhet. (Mer information om hur du konfigurerar en vy finns för[Visa-menyn](storsimple-use-snapshot-manager.md#view-menu).

| Resultaten kolumn | Beskrivning |
|:--- |:--- |
| Namn |hello namnet på hello enhet som konfigurerats i hello klassiska Azure-portalen |
| Modellen |hello modellnumret för hello-enhet |
| Version |hello-versionen av hello programvaran på hello enheten |
| Status |Om hello-enhet är tillgänglig |
| Synkroniserades senast |Datum och tid när hello enheten senast synkroniserades |
| Serienr |hello serienumret för hello enheten |

Om du högerklickar på hello **enheter** nod i hello **omfång** fönstret kan du välja från hello följande åtgärder:

* Lägga till eller ersätta en enhet
* Ansluter en enhet och kontrollera import
* Uppdatera anslutna enheter

Om du klickar på hello **enheter** nod och högerklicka sedan på en enhet namn i hello **resultat** fönstret kan du välja från hello följande åtgärder:

* Autentisera en enhet
* Visa information om enhet
* Uppdatera en enhet
* Ta bort en enhetskonfiguration
* Ändra lösenordet för enheten

> [!NOTE]
> Alla dessa åtgärder är också tillgängliga i hello **åtgärder** fönstret.


Den här självstudiekursen beskrivs hur toouse StorSimple Snapshot Manager tooconnect och hantera enheter och utföra hello följande uppgifter:

* Lägga till eller ersätta en enhet
* Ansluter en enhet och kontrollera import
* Uppdatera anslutna enheter
* Autentisera en enhet
* Visa information om enhet
* Uppdatera en enskild enhet
* Ta bort en enhetskonfiguration
* Ändra ett enhetslösenord har upphört att gälla
* Ersätta en misslyckad enhet

> [!NOTE]
> Allmän information om hello StorSimple Snapshot Manager-gränssnittet går för[StorSimple Snapshot Manager användargränssnittet](storsimple-use-snapshot-manager.md).


## <a name="add-or-replace-a-device"></a>Lägga till eller ersätta en enhet
Använd följande procedur tooadd hello eller ersätta en StorSimple-enhet.

#### <a name="tooadd-or-replace-a-device"></a>tooadd eller ersätta en enhet
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
2. I hello **omfång** fönstret, högerklicka på hello **enheter** noden och klicka sedan på **konfigurera en enhet**. Hej **konfigurera en enhet** dialogrutan visas.
   
    ![Konfigurera en StorSimple-enhet](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_config_device.png) 
3. I hello **enhet** listrutan, Välj hello IP-adressen för hello eller virtuella enheten. 
4. I hello **lösenord** textruta, ange hello StorSimple Snapshot Manager ett lösenord som du skapade för hello-enhet i hello klassiska Azure-portalen. Klicka på **OK**. StorSimple Snapshot Manager söker efter hello-enhet som du har identifierats. 
   
   * Om hello-enhet är tillgänglig, StorSimple Snapshot Manager lägger till en anslutning.
   * Om hello enheten inte är tillgänglig av någon anledning, returnerar StorSimple Snapshot Manager ett felmeddelande. Klicka på **OK** tooclose hello felmeddelande och klicka sedan på **Avbryt** tooclose hello **konfigurera en enhet** dialogrutan.

## <a name="connect-a-device-and-verify-imports"></a>Ansluter en enhet och kontrollera import
Använd hello följa proceduren tooconnect StorSimple-enheten och kontrollera att alla befintliga volymen grupper som har associerade säkerhetskopior importeras.

#### <a name="tooconnect-a-device-and-verify-imports"></a>tooconnect en enhet och kontrollera import
1. tooconnect en enhet tooStorSimple Snapshot Manager följer du anvisningarna hello i Lägg till eller ersätta en enhet. När den ansluter tooa enheten svarar StorSimple Snapshot Manager på följande sätt:
   
   * Om hello enheten inte är tillgänglig av någon anledning, returnerar StorSimple Snapshot Manager ett felmeddelande. 
   
   * Om hello-enhet är tillgänglig, StorSimple Snapshot Manager lägger till en anslutning. När du väljer hello enhet visas den i hello **resultat** rutan och hello statusfältet anger hello enheten är **tillgänglig**. StorSimple Snapshot Manager importerar alla volym grupper som konfigurerats för hello-enhet, förutsatt att hello volym grupper har associerade säkerhetskopior. Principer för säkerhetskopiering har inte importerats. Volymen grupper som inte har tillhörande säkerhetskopior har inte importerats.
2. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
3. Högerklicka på hello översta noden i hello **omfång** rutan och klicka sedan på **växla import visa**.
   
    ![Välj Växla import visning](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Toggle_Imports_Display.png) 
4. Hej **växla import visa** dialogrutan visar hello status för hello importeras grupper för volymen och säkerhetskopieringar. Klicka på **OK**.

När hello grupper för volymen och säkerhetskopieringar har importerats kan du använda StorSimple Snapshot Manager toomanage dem, precis som du skulle hantera grupper för volymen och säkerhetskopieringar som du skapat och konfigurerat med StorSimple Snapshot Manager. 

## <a name="refresh-connected-devices"></a>Uppdatera anslutna enheter
Använd följande procedur toosynchronize hello anslutna StorSimple-enheter med StorSimple Snapshot Manager hello.

#### <a name="toorefresh-connected-devices"></a>toorefresh anslutna enheter
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
2. I hello **omfång** fönstret, högerklicka på **enheter**, och klicka sedan på **uppdatera enheter**. Detta synkroniserar hello anslutna enheter med StorSimple Snapshot Manager så att du kan visa hello volym grupper och säkerhetskopior, inklusive eventuella nya tillägg. 
   
    ![Uppdatera hello StorSimple-enheter](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Refresh_devices.png)

Hej **uppdatera enheter** åtgärd hämtar alla nya grupper i volymen och alla tillhörande säkerhetskopior från anslutna enheter. Till skillnad från hello **skanna volymer** åtgärd som är tillgängliga för hello **volymer** noden **uppdatera enheter** återställer inte hello säkerhetskopiering registret.

## <a name="authenticate-a-device"></a>Autentisera en enhet
Använd följande procedur tooauthenticate StorSimple-enhet med StorSimple Snapshot Manager hello.

#### <a name="tooauthenticate-a-device"></a>tooauthenticate en enhet
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
2. I hello **omfång** rutan klickar du på **enheter**.
3. I hello **resultat** fönstret högerklickar du på enheten hello hello namnet och klicka sedan på **autentisera**.
4. Hej **autentisera** dialogrutan visas. Skriv hello enhetens lösenord och klicka sedan på **OK**.
   
    ![Autentisera dialogrutan](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Authenticate.png) 

## <a name="view-device-details"></a>Visa information om enhet
Använd hello följande procedur tooview hello information för en StorSimple-enhet och synkronisera om hello enhet med StorSimple Snapshot Manager om det behövs.

#### <a name="tooview-and-resynchronize-device-details"></a>tooview och synkronisera om enhetsinformation
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
2. I hello **omfång** rutan klickar du på **enheter**.
3. I hello **resultat** fönstret högerklickar du på enheten hello hello namnet och klicka sedan på **information**.

4 hello **enhetsinformation** dialogrutan visas. Den här rutan visar hello namn, modell, version, serienummer, status, iSCSI-målet kvalificerade namn (IQN), och senaste Synkroniseringsdatum och tid.

* Klicka på **omsynkronisering** toosynchronize hello enhet.
* Klicka på **OK** eller **Avbryt** tooclose hello dialogrutan.
  
  ![Information om enhet](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Device_details.png) 

## <a name="refresh-an-individual-device"></a>Uppdatera en enskild enhet
Använd följande procedur tooresynchronize en enskild StorSimple-enhet med StorSimple Snapshot Manager hello.

#### <a name="toorefresh-a-device"></a>toorefresh en enhet
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager. 
2. I hello **omfång** rutan klickar du på **enheter**. 
3. I hello **resultat** fönstret högerklickar du på enheten hello hello namnet och klicka sedan på **uppdatera enheten**. Detta synkroniserar hello enhet med StorSimple Snapshot Manager.

## <a name="delete-a-device-configuration"></a>Ta bort en enhetskonfiguration
Använd följande procedur toodelete en konfiguration för enskilda StorSimple-enheter från StorSimple Snapshot Manager hello.

#### <a name="toodelete-a-device-configuration"></a>toodelete en enhetskonfigurationen
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
2. I hello **omfång** rutan klickar du på **enheter**. 
3. I hello **resultat** fönstret högerklickar du på enheten hello hello namnet och klicka sedan på **ta bort**. 
4. hello följande meddelande visas. Klicka på **Ja** toodelete hello konfigurationen eller klicka på **nr** toocancel hello borttagning.
   
    ![Ta bort konfiguration för enheter](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_DeleteDevice.png)

## <a name="change-an-expired-device-password"></a>Ändra ett enhetslösenord har upphört att gälla
Du måste ange ett lösenord tooauthenticate StorSimple-enhet med StorSimple Snapshot Manager. Du kan konfigurera det här lösenordet när du använder hello Windows PowerShell-gränssnittet tooset in hello enhet. Hello lösenord kan löpa ut. Om det händer kan använda du hello Azure klassiska portal toochange hello lösenord. Eftersom hello enhet har konfigurerats i StorSimple Snapshot Manager innan hello lösenord upphör att gälla, måste du sedan nytt autentisera hello enhet i StorSimple Snapshot Manager.

#### <a name="toochange-hello-expired-password"></a>toochange hello utgånget lösenord
1. Starta hello StorSimple Manager-tjänsten i hello klassiska Azure-portalen.
2. Klicka på **enheter** > **konfigurera** för hello enhet.
3. Rulla ned toohello StorSimple Snapshot Manager avsnitt. Ange ett lösenord som är 14 och 15 tecken. Kontrollera att lösenordet hello innehåller en blandning av versaler, gemener, siffror och särskilda tecken.
4. Ange nytt hello lösenord tooconfirm den.
5. Klicka på **spara** på hello hello sidans nederkant.

#### <a name="toore-authenticate-hello-device"></a>toore-autentisering hello-enhet
1. Starta StorSimple Snapshot Manager.
2. I hello **omfång** rutan klickar du på **enheter**. En lista över konfigurerade enheter visas i hello **resultat** fönstret.
3. Välj hello enhet, högerklicka och klicka sedan på **autentisera**.
4. I hello **autentisera** fönstret Ange hello nytt lösenord.
5. Välj hello enhet, högerklicka och välj **uppdatera enheten**. Detta synkroniserar hello enhet med StorSimple Snapshot Manager.

## <a name="replace-a-failed-device"></a>Ersätta en misslyckad enhet
Om en StorSimple-enhet misslyckas och hello ersättas med en enhet för vänteläge (failover), Använd hello följande steg tooconnect toohello ny enhet och visa tillhörande säkerhetskopior.

#### <a name="tooconnect-tooa-new-device-after-failover"></a>tooconnect tooa ny enhet efter växling vid fel
1. Konfigurera om hello iSCSI-anslutning toohello ny enhet. Anvisningar finns för ”steg 7: montera, initiera och formatera en volym” i [distribuera din lokala StorSimple-enhet](storsimple-8000-deployment-walkthrough-u2.md).

> [!NOTE]
> Om hello nya StorSimple-enhet har samma IP-adress som hello hello gamla, kanske du kan tooconnect hello gammal konfiguration.


1. Stoppa hello Microsoft StorSimple Management-tjänsten:
   
   1. Starta Serverhanteraren.
   2. På hello instrumentpanelen Serverhanteraren på hello **verktyg** väljer du **Services**.
   3. På hello **Services** fönster, Välj hello **Management-tjänsten för Microsoft StorSimple**.
   4. I hello Högerklicka rutan under **Management-tjänsten för Microsoft StorSimple**, klickar du på **stoppa hello tjänsten**.
2. Ta bort hello configuration information relaterad toohello gamla enhet:
   
   1. Bläddra tooC:\ProgramData\Microsoft\StorSimple\BACatalog i Utforskaren.
   2. Ta bort hello filer i hello BACatalog mapp.
3. Starta om hello Microsoft StorSimple Management-tjänsten:
   
   1. På hello instrumentpanelen Serverhanteraren på hello **verktyg** väljer du **Services**.
   2. På hello **Services** fönster, Välj hello **Management-tjänsten för Microsoft StorSimple**.
   3. I hello Högerklicka rutan under **Management-tjänsten för Microsoft StorSimple**, klickar du på **starta om tjänsten hello**.
4. Starta StorSimple Snapshot Manager.
5. tooconfigure hello nya StorSimple-enhet, fullständig hello steg i steg 2: Anslut en virtuell StorSimple-enhet i [distribuera StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).
6. Högerklicka på hello översta noden i hello **omfång** fönstret (StorSimple Snapshot Manager i hello exempel) och klicka sedan på **växla import visa**. 
7. Ett meddelande visas när hello importeras grupper för volymen och säkerhetskopieringar visas i StorSimple Snapshot Manager. Klicka på **OK**.

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[med StorSimple Snapshot Manager tooadminister din StorSimple-lösning](storsimple-snapshot-manager-admin.md).
* Lär dig hur för[använda StorSimple Snapshot Manager tooview och hantera volymer](storsimple-snapshot-manager-manage-volumes.md).

