---
title: aaaMicrosoft Azure StorSimple Manager virtuella matris administration | Microsoft Docs
description: "Lär dig hur toomanage din StorSimple lokala virtuella matris med hello StorSimple Enhetshanteraren service i hello Azure-portalen."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 958244a5-f9f5-455e-b7ef-71a65558872e
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/1/2016
ms.author: alkohli
ms.openlocfilehash: 1fabf9ca524b461266346a6cabf49aef772032ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-tooadminister-your-storsimple-virtual-array"></a>Använd hello StorSimple Enhetshanteraren service tooadminister din virtuella StorSimple-matris
![processflöde för installationen](./media/storsimple-virtual-array-manager-service-administration/manage4.png)

## <a name="overview"></a>Översikt
Den här artikeln beskriver hello StorSimple Enhetshanteraren service gränssnitt, inklusive hur tooconnect tooit och hello olika alternativ tillgängliga och innehåller länkar toohello specifika arbetsflöden som kan utföras via Användargränssnittet.

När du har läst den här artikeln kommer du att veta hur du:

* Ansluta toohello StorSimple enheten Manager-tjänsten
* Navigera hello StorSimple Enhetshanteraren UI
* Administrera din virtuella StorSimple-matris via hello StorSimple enheten Manager-tjänsten

> [!NOTE]
> tooview hello hanteringsalternativ som finns tillgängliga för hello StorSimple 8000-serieenhet, gå för[Använd hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).
> 
> 

## <a name="connect-toohello-storsimple-device-manager-service"></a>Ansluta toohello StorSimple enheten Manager-tjänsten
Hej StorSimple enheten Manager-tjänsten körs i Microsoft Azure och ansluter toomultiple StorSimple virtuell matriser. Du kan använda en central Microsoft Azure-portalen som körs i en webbläsare toomanage dessa enheter. tooconnect toohello StorSimple enheten Manager-tjänsten hello följande.

#### <a name="tooconnect-toohello-service"></a>tooconnect toohello service
1. Gå för[https://ms.portal.azure.com](https://ms.portal.azure.com).
2. Med autentiseringsuppgifterna för ditt Microsoft-konto kan logga in toohello Microsoft Azure-portalen (finns i hello längst upp till höger i fönstret hello).
3. Navigera tooBrowse--> ”Filter” på StorSimple-enhetshanterare tooview alla enhetshanterare för en viss prenumeration.

## <a name="use-hello-storsimple-device-manager-service-tooperform-management-tasks"></a>Använd hello StorSimple Enhetshanteraren tooperform Tjänstehantering
hello visas nedan en sammanfattning av alla hello vanliga administrativa uppgifter och komplexa arbetsflöden som kan utföras i hello StorSimple Enhetshanteraren service sammanfattning bladet. Aktiviteterna är uppdelade utifrån hello blad har initierats.

Mer information om varje arbetsflöde klickar du på hello lämplig procedur i hello tabell.

#### <a name="storsimple-device-manager-workflows"></a>StorSimple Device Manager-arbetsflöden
| Om du vill toodo... | Använd den här proceduren |
| --- | --- | --- |
| Skapa en tjänst</br>Ta bort en tjänst</br>Hämta hello nyckel för tjänstregistrering</br>Återskapa hello nyckel för tjänstregistrering |[Distribuera hello StorSimple enheten Manager-tjänsten](storsimple-virtual-array-manage-service.md) |
| Visa hello aktivitetsloggar |[Använd hello StorSimple-tjänsten sammanfattning](storsimple-virtual-array-service-summary.md) |
| Inaktivera en virtuell matris</br>Ta bort en virtuell matris |[Inaktivera eller ta bort en virtuell matris](storsimple-virtual-array-deactivate-and-delete-device.md) |
| Disaster recovery och enheten redundans</br>Krav för redundans</br>Katastrofåterställning för verksamhetskontinuitet (BCDR)</br>Fel vid katastrofåterställning |[Disaster recovery och enheten redundans för din virtuella StorSimple-matris](storsimple-virtual-array-failover-dr.md) |
| Säkerhetskopiera filresurser och volymer</br>Gör en manuell säkerhetskopia</br>Ändra schemat för säkerhetskopiering av hello</br>Visa befintliga säkerhetskopior |[Säkerhetskopiera din virtuella StorSimple-matris](storsimple-virtual-array-backup.md) |
| Klona resurser från en säkerhetskopia</br>Klona volymer från en säkerhetskopia</br>Återställning på objektnivå (endast filserver) |[Klona från en säkerhetskopia av din virtuella StorSimple-matris](storsimple-virtual-array-clone.md) |
| Om storage-konton</br>Lägg till ett lagringskonto</br>Redigera ett lagringskonto</br>Ta bort ett lagringskonto |[Hantera lagringskonton för hello virtuella StorSimple-matris](storsimple-virtual-array-manage-storage-accounts.md) |
| Om access control-poster</br>Lägg till eller ändra en åtkomstkontrollpost </br>Ta bort en åtkomstkontrollpost |[Hantera åtkomstkontroll poster för hello virtuella StorSimple-matris](storsimple-virtual-array-manage-acrs.md) |
| Visa jobbinformation |[Hantera virtuella StorSimple-matris jobb](storsimple-virtual-array-manage-jobs.md) |
| Konfigurera aviseringsinställningar</br>Få varningsmeddelanden</br>Hantera aviseringar</br>Granska aviseringar |[Visa och hantera aviseringar för hello virtuella StorSimple-matris](storsimple-virtual-array-manage-alerts.md) |
| Ändra hello enhetens administratörslösenord |[Ändra hello virtuella StorSimple-matris enhetens administratörslösenord](storsimple-virtual-array-change-device-admin-password.md) |
| Installera programuppdateringar |[Uppdatera din virtuella matris](storsimple-virtual-array-install-update.md) |

> [!NOTE]
> Du måste använda hello [lokala webbgränssnittet](storsimple-ova-web-ui-admin.md) för hello följande uppgifter:
> 
> * [Hämta krypteringsnyckel för tjänstdata hello](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key)
> * [Skapa ett supportpaket](storsimple-ova-web-ui-admin.md#generate-a-log-package)
> * [Stoppa och starta om en virtuell matris](storsimple-ova-web-ui-admin.md#shut-down-and-restart-your-device)
> 
> 

## <a name="next-steps"></a>Nästa steg
Information om hello webbgränssnitt och hur toouse, gå för[Använd hello StorSimple web UI tooadminister din virtuella StorSimple-matris](storsimple-ova-web-ui-admin.md).

