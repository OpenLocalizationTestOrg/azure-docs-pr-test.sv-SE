---
title: "aaaAuto skala en tjänst i molnet i hello klassiska portal | Microsoft Docs"
description: "(klassisk) Lär dig hur toouse hello klassiska portal tooconfigure automatiskt skala regler för en cloud service webbroll eller en worker-rollen i Azure."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: eb167d70-4eba-42a4-b157-d8d0688abf4b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: ddb5816d4d22192c6d2f51d7508e390779742078
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-auto-scaling-for-a-cloud-service-in-hello-classic-portal"></a>Hur tooconfigure automatisk skalning för en tjänst i molnet i hello klassiska portalen
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-scale-portal.md)
> * [Klassisk Azure-portal](cloud-services-how-to-scale.md)

Du kan konfigurera automatiska inställningar för din webbroll eller arbetsrollen hello skala sida av hello klassiska Azure-portalen. Du kan också konfigurera manuell skalning i stället för regelbaserade automatisk skalning.

> [!NOTE]
> Den här artikeln fokuserar på Molntjänsten webb- och arbetsroller roller. När du skapar en virtuell dator (klassisk) direkt finns det i en molntjänst. En del av den här informationen gäller toothese typer av virtuella datorer. Skala en tillgänglighetsuppsättning för virtuella datorer stängs bara dem och inaktivera baserat på hello skala regler som du konfigurerar. Läs mer om virtuella datorer och tillgänglighetsuppsättningar [hantera hello tillgänglighet av virtuella datorer](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

Du bör hello följande information innan du konfigurerar skalning för programmet:

* Skalning påverkas av kärnor användning.

    Större rollinstanser använda flera kärnor. Du kan skala ett program bara inom hello kärnor för prenumerationen. Anta till exempel har en begränsning på 20 kärnor för prenumerationen. Om du kör ett program med två medelstora molntjänster (totalt 4 kärnor) kan du bara skala upp andra distributioner av molntjänster i din prenumeration av hello återstående 16 kärnor. Mer information om storlekar finns [Molntjänststorlekar](cloud-services-sizes-specs.md).

* Du måste skapa en kö och koppla den till en roll innan du kan skala en applikation baserat på ett tröskelvärde för meddelandet. Mer information finns i [hur toouse hello Queue Storage Service](../storage/queues/storage-dotnet-how-to-use-queues.md).

* Du kan skala resurser som är länkade tooyour Molntjänsten. Mer information om hur du länkar resurser finns [så här: länka en molntjänst för resursen tooa](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).

* tooenable hög tillgänglighet för programmet, bör du kontrollera att den har distribuerats med två eller flera rollinstanser. Mer information finns i [serviceavtal](https://azure.microsoft.com/support/legal/sla/).

## <a name="schedule-scaling"></a>Schemalägga skalning
Som standard följer inte alla roller för ett visst schema. Därför gäller alla inställningar som ändrades tooall samt alla dagar över hello året. Om du vill kan konfigurera du manuell eller automatisk skalning av något av följande lägen hello:

* Veckodagar
* Helger
* Vecka kvällar
* Vecka mornings
* Specifika datum
* Specifika datumintervall

hello schema inställning konfigureras i hello [klassiska Azure-portalen](https://manage.windowsazure.com/) på hello  
**Molntjänster** > **\[Molntjänsten\]** > **skala**  >   **\[Produktion eller mellanlagring\]**  sidan.

Klicka på hello **ställer in schema gånger** knappen för varje roll som du vill toochange.

![Molntjänsten automatisk skalning enligt ett schema][scale_schedules]

## <a name="manual-scale"></a>Manuell skala
På hello **skala** kan du manuellt öka eller minska hello antalet instanser som körs i en molntjänst. Den här inställningen har konfigurerats för varje schema som du har skapat eller tooall tid om du inte har skapat ett schema.

1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com/), klickar du på **molntjänster**, och klicka sedan på hello namnet på hello cloud service tooopen hello instrumentpanelen.
   
   > [!TIP]
   > Om du inte hittar din molntjänst kan du behöva toochange från **produktion** för**mellanlagring** eller vice versa.

2. Klicka på **skala**.
3. Välj hello-schema som du vill toochange skalningsalternativ för. *Inte schemalagda tider* är hello standard om du har inga scheman som definierats.
4. Hitta hello **skala efter mått** avsnittet och väljer **NONE**. Den här inställningen är hello standard för alla roller.
5. Varje roll i hello Molntjänsten har en skjutreglaget för att ändra hello antalet instanser toouse.
   
    ![Skala manuellt en rolltjänst för molnet][manual_scale]
   
    Om du behöver fler instanser kan du behöva toochange hello [molnet storleken för den virtuella datorn](cloud-services-sizes-specs.md).
6. Klicka på **Spara**.  
   Rollinstanser läggs till eller tas bort baserat på dina val.

> [!TIP]
> När du ser ![][tip_icon] flytta musen-tooit och du kan få hjälp om en specifik inställning har.

## <a name="automatic-scale---cpu"></a>Automatisk skalning - processor
Det här läget skalas om hello genomsnittliga procentandelen av CPU-användningen går över eller under angivna tröskelvärden. Rollinstanser skapas eller tas bort när detta sker.

1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com/), klickar du på **molntjänster**, och klicka sedan på hello namnet på hello cloud service tooopen hello instrumentpanelen.
   
   > [!TIP]
   > Om du inte hittar din molntjänst kan du behöva toochange från **produktion** för**mellanlagring** eller vice versa.

2. Klicka på **skala**.
3. Välj hello-schema som du vill toochange skalningsalternativ för. *Inte schemalagda tider* är hello standard om du har inga scheman som definierats.
4. Hitta hello **skala efter mått** avsnittet och väljer **CPU**.
5. Du kan nu konfigurera en lägsta och högsta antal roller instanser hello mål CPU-användning (tootrigger en skalas upp) och hur många instanser tooscale uppåt och nedåt av.

![Skala en rolltjänst i molnet genom att cpu-belastning][cpu_scale]

> [!TIP]
> När du ser ![][tip_icon] flytta musen-tooit och du kan få hjälp om en specifik inställning har.

## <a name="automatic-scale---queue"></a>Automatisk skalning - kön
Det här läget skalas automatiskt om hello antal meddelanden i en kö går över eller under ett angivet tröskelvärde. Rollinstanser skapas eller tas bort när detta sker.

1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com/), klickar du på **molntjänster**, och klicka sedan på hello namnet på hello cloud service tooopen hello instrumentpanelen.
   
   > [!TIP]
   > Om du inte hittar din molntjänst kan du behöva toochange från **produktion** för**mellanlagring** eller vice versa.

2. Klicka på **skala**.
3. Hitta hello **skala efter mått** avsnittet och väljer **KÖN**.
4. Du kan nu konfigurera en lägsta och högsta antal roller instanser hello kön och antalet kön meddelanden tooprocess för varje instans och hur många instanser tooscale uppåt och nedåt av.

![Skala en rolltjänst för molntjänster genom en meddelandekö][queue_scale]

> [!TIP]
> När du ser ![][tip_icon] flytta musen-tooit och du kan få hjälp om en specifik inställning har.

## <a name="scale-linked-resources"></a>Skala länkade resurserna
Ofta när du skalar en roll är bra tooscale hello databas som programmet hello använder också. Du kan komma åt hello skalning inställningar för den här resursen genom att klicka på länken för hello om du länkar hello databasen toohello Molntjänsten.

1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com/), klickar du på **molntjänster**, och klicka sedan på hello namnet på hello cloud service tooopen hello instrumentpanelen.
   
   > [!TIP]
   > Om du inte hittar din molntjänst kan du behöva toochange från **produktion** för**mellanlagring** eller vice versa.

2. Klicka på **skala**.
3. Hitta hello **länkade resurser** avsnittet och klicka på **hantera skala för den här databasen**.
   
   > [!NOTE]
   > Om du inte ser en **länkade resurser** avsnittet du förmodligen inte har några länkade resurser.

![][linked_resource]

[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
