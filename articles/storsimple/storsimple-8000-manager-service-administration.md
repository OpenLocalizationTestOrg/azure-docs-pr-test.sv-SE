---
title: aaaStorSimple administrationen av Enhetshanteraren | Microsoft Docs
description: "Lär dig hur toomanage din StorSimple-enhet med hjälp av hello StorSimple enheten Manager-tjänsten i hello Azure-portalen."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2017
ms.author: alkohli
ms.openlocfilehash: d73dc32bd39b2c832d5c4a5edda32a2e7f55a503
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-tooadminister-your-storsimple-device"></a>Använd hello StorSimple Enhetshanteraren service tooadminister din StorSimple-enhet

## <a name="overview"></a>Översikt

Den här artikeln beskriver hello StorSimple Enhetshanteraren service gränssnitt, inklusive hur tooconnect tooit hello olika alternativ som är tillgängliga och länkar ut toohello specifika arbetsflöden som kan utföras via Användargränssnittet. Den här vägledningen är tillämpliga tooboth; fysiska hello StorSimple-enheten och hello molnet installation.

När du har läst den här artikeln får du lära dig att:

* Ansluta tooStorSimple Enhetshanteraren service
* Administrera din StorSimple-enhet via hello StorSimple enheten Manager-tjänsten

## <a name="connect-toostorsimple-device-manager-service"></a>Ansluta tooStorSimple Enhetshanteraren service

Hej StorSimple enheten Manager-tjänsten körs i Microsoft Azure och ansluter toomultiple StorSimple-enheter. Du kan använda en central Microsoft Azure-portalen som körs i en webbläsare toomanage dessa enheter. tooconnect toohello StorSimple enheten Manager-tjänsten hello följande.

#### <a name="tooconnect-toohello-service"></a>tooconnect toohello service
1. Navigera för[https://portal.azure.com/](https://portal.azure.com/).
2. Med autentiseringsuppgifterna för ditt Microsoft-konto kan logga in toohello Microsoft Azure-portalen (finns i hello längst upp till höger i fönstret hello).
3. Rulla ned hello vänster navigering fönstret tooaccess hello StorSimple enheten Manager-tjänsten.


## <a name="administer-storsimple-device-using-storsimple-device-manager-service"></a>Administrera StorSimple-enhet med hjälp av Enhetshanteraren för StorSimple-tjänsten

hello visas nedan en sammanfattning av alla hello vanliga administrativa uppgifter och komplexa arbetsflöden som kan utföras i hello StorSimple Enhetshanteraren service Användargränssnittet. Aktiviteterna är uppdelade utifrån hello UI blad har initierats.

Mer information om varje arbetsflöde klickar du på hello lämplig procedur i hello tabell.

#### <a name="storsimple-device-manager-workflows"></a>StorSimple Device Manager-arbetsflöden

| Om du vill toodo... | Använd den här proceduren. |
| --- | --- |
| Skapa en tjänst</br>Ta bort en tjänst</br>Hämta nyckel för tjänstregistrering</br>Återskapa nyckeln för tjänstregistrering |[Distribuera en StorSimple Device Manager-tjänst](storsimple-8000-manage-service.md) |
| Visa hello aktivitetsloggar |[Använd hello StorSimple Enhetshanteraren tjänsten sammanfattning](storsimple-8000-service-dashboard.md) |
| Ändra hello krypteringsnyckel för tjänstdata</br>Visa hello åtgärdsloggar |[Använda hello StorSimple Enhetshanteraren service instrumentpanel](storsimple-8000-service-dashboard.md) |
| Inaktivera en enhet</br>Ta bort en enhet |[Inaktivera eller ta bort en enhet](storsimple-8000-deactivate-and-delete-device.md) |
| Lär dig mer om disaster recovery och enheten redundans</br>Redundans tooa fysisk enhet</br>Redundans tooa virtuell enhet</br>Katastrofåterställning för verksamhetskontinuitet (BCDR) |[Redundans och disaster recovery för din StorSimple-enhet](storsimple-8000-device-failover-disaster-recovery.md) |
| Lista säkerhetskopieringar för en volym</br>Välj en säkerhetskopia</br>Ta bort en säkerhetskopia |[Hantera säkerhetskopior](storsimple-8000-manage-backup-catalog.md) |
| Klona en volym |[Klona en volym](storsimple-8000-clone-volume-u2.md) |
| Återställa en säkerhetskopia |[Återställa en säkerhetskopia](storsimple-8000-restore-from-backup-set-u2.md) |
| Om storage-konton</br>Lägg till ett lagringskonto</br>Redigera ett lagringskonto</br>Ta bort ett lagringskonto</br>Viktiga rotation av storage-konton |[Hantera lagringskonton](storsimple-8000-manage-storage-accounts.md) |
| Om bandbredd mallar</br>Lägg till en bandbreddsmall</br>Redigera en bandbreddsmall</br>Ta bort en bandbreddsmall</br>Använda en standardmall för bandbredd</br>Skapa en varar bandbreddsmall som börjar vid en angiven tid |[Hantera bandbreddsmallar](storsimple-8000-manage-bandwidth-templates.md) |
| Om access control-poster</br>Skapa en åtkomstkontrollpost</br>Redigera en åtkomstkontrollpost</br>Ta bort en åtkomstkontrollpost |[Hantera access control-poster](storsimple-8000-manage-acrs.md) |
| Visa jobbinformation</br>Avbryta ett jobb |[Hantera jobb](storsimple-8000-manage-jobs-u2.md) |
| Få varningsmeddelanden</br>Hantera aviseringar</br>Granska aviseringar |[Visa och hantera aviseringar för StorSimple](storsimple-8000-manage-alerts.md) |
| Skapa övervakning diagram |[Övervaka din StorSimple-enhet](storsimple-monitor-device.md) |
| Lägg till volymbehållare</br>Ändra en volymbehållare</br>Ta bort en volymbehållare |[Hantera volymbehållare](storsimple-8000-manage-volume-containers.md) |
| Lägg till en volym</br>Ändra en volym</br>Koppla från en volym</br>Ta bort en volym</br>Övervaka en volym |[Hantera volymer](storsimple-8000-manage-volumes-u2.md) |
| Ändra inställningar för enheter</br>Ändra inställningarna för tid</br>Ändra inställningar för DNS.md</br>Konfigurera nätverksgränssnitt |[Ändra enhetskonfigurationen för din StorSimple-enhet](storsimple-8000-modify-device-config.md) |
| Visa web proxy-inställningar |[Konfigurera webbproxy för din enhet](storsimple-8000-configure-web-proxy.md) |
| Ändra enhetens administratörslösenord</br>Ändra lösenordet för StorSimple Snapshot Manager |[Ändra StorSimple-lösenord](storsimple-8000-change-passwords.md) |
| Konfigurera fjärrhantering |[Fjärransluta tooyour StorSimple-enhet](storsimple-8000-remote-connect.md) |
| Konfigurera aviseringsinställningar |[Visa och hantera aviseringar för StorSimple](storsimple-8000-manage-alerts.md) |
| Konfigurera CHAP för din StorSimple-enhet |[Konfigurera CHAP för din StorSimple-enhet](storsimple-configure-chap.md) |
| Lägga till en säkerhetskopieringspolicy</br>Lägg till eller ändra ett schema</br>Ta bort en princip för säkerhetskopiering</br>Gör en manuell säkerhetskopia</br>Skapa en anpassad princip för säkerhetskopiering med flera volymer och scheman |[Hantera säkerhetskopieringsprinciper](storsimple-8000-manage-backup-policies-u2.md) |
| Stoppa styrenheter</br>Starta om styrenheter</br>Stäng av styrenheter</br>Återställa din enhet toofactory standardvärden</br>(Ovan är för lokala-enhet) |[Hantera styrenhet för StorSimple-enhet](storsimple-8000-manage-device-controller.md) |
| Lär dig mer om StorSimple maskinvarukomponenter</br>Övervakaren maskinvarustatus</br>(Ovan är för lokala-enhet) |[Övervakaren maskinvarukomponenter](storsimple-8000-monitor-hardware-status.md) |
| Skapa ett supportpaket |[Skapa och hantera ett stödpaket](storsimple-8000-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple) |
| Installera programuppdateringar |[Uppdatera din enhet](storsimple-update-device.md) |

## <a name="next-steps"></a>Nästa steg

Om du får problem med hello daglig drift av din StorSimple-enhet eller någon av dess komponenter, se:

* [Felsöka med hello-diagnostik](storsimple-8000-diagnostics.md)
* [Använd StorSimple övervakning indikatorer](storsimple-monitoring-indicators.md)

Om du inte kan lösa problem med hello och du måste toocreate en tjänstbegäran, se för[kontakta Microsoft Support](storsimple-8000-contact-microsoft-support.md).

