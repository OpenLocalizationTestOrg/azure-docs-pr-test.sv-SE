---
title: "aaaHow tooreset nätverksgränssnittet för virtuell Azure Windows-dator | Microsoft Docs"
description: "Visar hur tooreset nätverksgränssnittet för virtuell Azure Windows-dator"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: genlin
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: genli
ms.openlocfilehash: 1b653820927ef4c3bb8f384a7e752846a8be3da9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-network-interface-for-azure-windows-vm"></a>Hur tooreset nätverksgränssnittet för virtuell Azure Windows-dator 

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Du kan inte ansluta tooMicrosoft Azure Windows virtuell dator (VM) när du inaktiverar hello standard Network Interface (NIC) eller anger manuellt en statisk IP-adress för hello nätverkskort. Den här artikeln visar hur tooreset hello nätverksgränssnittet för Azure Windows VM som löser hello anslutning till problemet.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]
## <a name="reset-network-interface"></a>Återställ nätverksgränssnittet

### <a name="for-classic-vms"></a>För klassiska virtuella datorer

tooreset nätverk gränssnitt, gör du följande:

1.  Gå toohello [Azure-portalen]( https://ms.portal.azure.com).
2.  Välj **virtuella datorer (klassisk)**.
3.  Välj hello påverkas virtuell dator.
4.  Välj **IP-adresser**.
5.  Om hello **privata IP-tilldelning** är inte **statiska**, ändra också**statiska**.
6.  Ändra hello **IP-adress** tooanother IP-adress som är tillgängliga i hello undernät.
7.  Klicka på Spara.
8.  hello virtuella datorn startar om tooinitialize hello nya NIC toohello system.
9.  Försök tooRDP tooyour datorn. Om det lyckas, kan du ändra hello privat IP-adress tillbaka toohello ursprungliga om du vill ha. Annars kan du behålla den. 

### <a name="for-vms-deployed-in-resource-group-model"></a>För virtuella datorer som distribueras i gruppen modell

1.  Gå toohello [Azure-portalen]( https://ms.portal.azure.com).
2.  Välj hello påverkas virtuell dator.
3.  Välj **nätverksgränssnitt**.
4.  Välj hello nätverksgränssnitt som är kopplade till datorn
5.  Välj **IP-konfigurationer**.
6.  Välj hello IP. 
7.  Om hello **privata IP-tilldelning** är inte **statiska**, ändra också**statiska**.
8.  Ändra hello **IP-adress** tooanother IP-adress som är tillgängliga i hello undernät.
9. hello virtuella datorn startar om tooinitialize hello nya NIC toohello system.
10. Försök tooRDP tooyour datorn. Om det lyckas, kan du ändra hello privat IP-adress tillbaka toohello ursprungliga om du vill ha. Annars kan du behålla den. 

## <a name="delete-hello-unavailable-nics"></a>Ta bort hello tillgänglig nätverkskort
När du kan remote desktop toohello datorn måste du ta bort hello gamla nätverkskort tooavoid hello potentiella problem:

1.  Öppna Enhetshanteraren.
2.  Välj **visa** > **Visa dolda enheter**.
3.  Välj **nätverkskort**. 
4.  Sök efter hello nätverkskort med samma namn som ”Microsoft Hyper-V-nätverkskort”.
5.  Du kan se ett tillgängligt nätverkskort som är nedtonad. Högerklicka på hello-kort och välj sedan avinstallera.

    ![hello bild av hello NIC](media/reset-network-interface/nicpage.png)

    > [!NOTE]
    > Avinstallera endast hello tillgänglig nätverkskort som har hello namn ”Microsoft Hyper-V-nätverkskort”. Om du avinstallerar någon hello andra dolda kort kan orsaka det ytterligare problem.
    >
    >

6.  Nu ska alla tillgängliga nätverkskort vara avmarkerade från datorn.