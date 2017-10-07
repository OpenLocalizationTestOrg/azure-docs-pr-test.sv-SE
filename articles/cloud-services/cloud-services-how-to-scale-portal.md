---
title: "aaaAuto skala en tjänst i molnet i hello portal | Microsoft Docs"
description: "Lär dig hur toouse hello portal tooconfigure automatiskt skala regler för en cloud service webbroll eller en worker-rollen i Azure."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 701d4404-5cc0-454b-999c-feb94c1685c0
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: 265f4c8ec5e1ec2f85585df25f18cd0d0c9946a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-auto-scaling-for-a-cloud-service-in-hello-portal"></a>Hur tooconfigure automatisk skalning för en tjänst i molnet i hello-portalen
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-scale-portal.md)
> * [Klassisk Azure-portal](cloud-services-how-to-scale.md)

Villkor kan anges för en cloud service-arbetsroll som utlöser en skala in eller ut igen. hello villkor för hello roll kan baseras på hello CPU, disk eller nätverksbelastningen hello-rollen. Du kan också ange ett villkor baserat på ett meddelande kö eller hello mått om vissa andra Azure-resurs som är associerade med prenumerationen.

> [!NOTE]
> Den här artikeln fokuserar på Molntjänsten webb- och arbetsroller roller. När du skapar en virtuell dator (klassisk) direkt finns det i en molntjänst. Du kan skala en vanliga virtuell dator genom att associera den med en [tillgänglighetsuppsättning](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) och aktivera dem manuellt eller inaktivera.

## <a name="considerations"></a>Överväganden
Du bör hello följande information innan du konfigurerar skalning för programmet:

* Skalning påverkas av kärnor användning.

    Större rollinstanser använda flera kärnor. Du kan skala ett program bara inom hello kärnor för prenumerationen. Anta till exempel har en begränsning på 20 kärnor för prenumerationen. Om du kör ett program med två medelstora molntjänster (totalt 4 kärnor) kan du bara skala upp andra distributioner av molntjänster i din prenumeration av hello återstående 16 kärnor. Mer information om storlekar finns [Molntjänststorlekar](cloud-services-sizes-specs.md).

* Du kan skala baserat på ett tröskelvärde för meddelandet. Mer information om hur toouse köer, se [hur toouse hello Queue Storage Service](../storage/queues/storage-dotnet-how-to-use-queues.md).

* Du kan även skala andra resurser som är associerade med prenumerationen.

* tooenable hög tillgänglighet för programmet, bör du kontrollera att den har distribuerats med två eller flera rollinstanser. Mer information finns i [serviceavtal](https://azure.microsoft.com/support/legal/sla/).


## <a name="where-scale-is-located"></a>Där skala finns
Du bör ha hello cloud service bladet visas när du har valt Molntjänsten.

1. På hello cloud service bladet på hello **roller och instanser** sida vid sida och välj hello namnet hello-Molntjänsten.   
   **VIKTIGA**: gör att tooclick hello cloud service rollen, inte hello rollinstans som är lägre än hello roll.

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)
2. Välj hello **skala** panelen.

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a>Automatisk skalning
Du kan konfigurera inställningar för en roll med antingen två lägen **manuell** eller **automatisk**. Manuell som förväntat, du anger hello absolut antal instanser. Automatisk kan du dock tooset regler som styr hur och med hur mycket du ska skalas.

Ange hello **skala genom** alternativet för**schema och prestanda regler**.

![Cloud services skalinställningar med profil och regel](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. En befintlig profil.
2. Lägga till en regel för hello överordnade profilen.
3. Lägg till en annan profil.

Välj **lägga till profilen**. hello profil avgör vilket läge du vill använda toouse för hello skala: **alltid**, **återkommande**, **fast datum**.

När du har konfigurerat hello profil och regler, Välj hello **spara** ikonen överst hello.

#### <a name="profile"></a>Profil
hello-profil anger minsta och största instanser för hello skala och även när det här intervallet för skalan är aktiv.

* **Alltid**

    Alltid ha detta antal instanser som är tillgängliga.  

    ![Molntjänst som alltid skala](./media/cloud-services-how-to-scale-portal/select-always.png)
* **Upprepning**

    Välj en uppsättning dagarnas hello vecka tooscale.

    ![Tjänsten molnskala med ett återkommande schema](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
* **Fast datum**

    En fast datum intervallet tooscale hello roll.

    ![Tjänsten molnskala med ett fast datum](./media/cloud-services-how-to-scale-portal/select-fixed.png)

När du har konfigurerat hello profil, Välj hello **OK** knappen längst ned hello hello-profilbladet.

#### <a name="rule"></a>Regel
Regler läggs tooa profil som representerar ett villkor som utlöser hello skala.

hello regeln utlösare är baserad på ett mått för hello Molntjänsten (CPU-användning, diskaktivitet eller nätverksaktivitet) toowhich som du kan lägga till en villkorlig värde. Du kan dessutom ha hello utlösning, baserat på ett meddelande kö eller hello mått om vissa andra Azure-resurs som är associerade med prenumerationen.

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

När du har konfigurerat hello regeln, Välj hello **OK** knappen längst ned hello hello regeln bladet.

## <a name="back-toomanual-scale"></a>Säkerhetskopiera toomanual skala
Navigera toohello [skala inställningar](#where-scale-is-located) och ange hello **skala genom** alternativet för**instansantal som anges manuellt**.

![Cloud services skalinställningar med profil och regel](./media/cloud-services-how-to-scale-portal/manual-basics.png)

Den här inställningen tar bort automatisk skalning från hello-rollen och kan du ange hello instanser direkt.

1. alternativ för hello skala (manuell eller automatisk).
2. En roll instans skjutreglaget tooset hello instanser tooscale till.
3. Instanser av hello roll tooscale till.

När du har konfigurerat hello skalinställningar, Välj hello **spara** ikonen överst hello.
