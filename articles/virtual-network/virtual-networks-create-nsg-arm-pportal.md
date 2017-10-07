---
title: "aaaCreate nätverkssäkerhetsgrupper - Azure-portalen | Microsoft Docs"
description: "Lär dig hur toocreate och distribuera nätverkssäkerhetsgrupper med hello Azure-portalen."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 5bc8fc2e-1e81-40e2-8231-0484cd5605cb
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f74ecc7db06bb69f2041aa64d7b38b63eb379a70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-network-security-groups-using-hello-azure-portal"></a>Skapa nätverk säkerhetsgrupper med hjälp av hello Azure-portalen

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Den här artikeln beskriver hello Resource Manager-modellen. Du kan också [skapa NSG: er i hello klassiska distributionsmodellen](virtual-networks-create-nsg-classic-ps.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

hello exempel PowerShell-kommandona nedan förväntar sig en enkel miljö som redan har skapats utifrån hello scenariot ovan. Om du vill toorun hello kommandon som de visas i det här dokumentet, först skapa hello testmiljö genom att distribuera [mallen](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), klickar du på **distribuera tooAzure**, Ersätt hello Standardparametervärden Om nödvändigt och hello anvisningar i hello portal. hello steg nedan används **RG NSG** som hello hello resurs grupp hello mallens namn har distribuerats till.

## <a name="create-hello-nsg-frontend-nsg"></a>Skapa hello NSG-klientdel NSG
toocreate hello **NSG-klientdel** NSG enligt hello scenariot ovan, följ hello stegen nedan.

1. Från en webbläsare, navigerar du toohttp://portal.azure.com och, om det behövs, logga in med ditt Azure-konto.
2. Klicka på **Bläddra >** > **Nätverkssäkerhetsgrupper**.
   
    ![Azure portal – NSG: er](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)
3. I hello **Nätverkssäkerhetsgrupper** bladet, klickar du på **Lägg till**.
   
    ![Azure portal – NSG: er](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)
4. I hello **skapa nätverkssäkerhetsgruppen** bladet, skapa en NSG som heter *NSG-klientdel* i hello *RG NSG* resursgrupp och klicka sedan på **skapa**.
   
    ![Azure portal – NSG: er](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a>Skapa regler i en befintlig nätverkssäkerhetsgrupp (NSG)
toocreate regler i en befintlig NSG från hello Azure-portalen gör hello nedan.

1. Klicka på **Bläddra >** > **Nätverkssäkerhetsgrupper**.
2. Hello listan med NSG: er, på **NSG-klientdel** > **inkommande säkerhetsregler**
   
    ![Azure portal – NSG-klientdel](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)
3. I hello lista över **inkommande säkerhetsregler**, klickar du på **Lägg till**.
   
    ![Azure portal – Lägg till regel](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)
4. I hello **Lägg till ingående säkerhetsregel** bladet, skapa en regel med namnet *regel för web* med prioriteten för *200* åtkomst via *TCP* tooport *80* tooany VM från någon datakälla och klicka sedan på **OK**. Observera att de flesta av de här inställningarna är standardvärden redan.
   
    ![Azure portal – regelinställningar](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)
5. Efter några sekunder visas hello ny regel i hello NSG.
   
    ![Azure portal – ny regel](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)
6. Upprepa steg too6 toocreate en inkommande regel med namnet *rdp-regel* med prioritet *250* åtkomst via *TCP* tooport *3389* tooany virtuell dator från en källa.

## <a name="associate-hello-nsg-toohello-frontend-subnet"></a>Associera hello NSG toohello klientdel undernät
1. Klicka på **Bläddra >** > **resursgrupper** > **RG NSG**.
2. I hello **RG NSG** bladet, klickar du på **...**   >  **TestVNet**.
   
    ![Azure portal – TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)
3. I hello **inställningar** bladet, klickar du på **undernät** > **klientdel** > **nätverkssäkerhetsgruppen**  >  **NSG-klientdel**.
   
    ![Azure portal – undernätsinställningar](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)
4. I hello **klientdel** bladet, klickar du på **spara**.
   
    ![Azure portal – undernätsinställningar](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-hello-nsg-backend-nsg"></a>Skapa hello NSG BackEnd NSG
toocreate hello **NSG BackEnd** NSG och koppla den toohello **BackEnd** undernät, följ hello stegen nedan.

1. Upprepa hello stegen i [skapa hello NSG-klientdel NSG](#Create-the-NSG-FrontEnd-NSG) toocreate en NSG som heter *NSG-serverdelen*
2. Upprepa hello stegen i [skapa regler i en befintlig NSG](#Create-rules-in-an-existing-NSG) toocreate hello **inkommande** regler i hello nedan.
   
   | Regel för inkommande trafik | Regel för utgående trafik |
   | --- | --- |
   | ![Azure portal - regel för inkommande trafik](./media/virtual-networks-create-nsg-arm-pportal/figure17.png) |![Azure portal - regel för utgående trafik](./media/virtual-networks-create-nsg-arm-pportal/figure18.png) |
3. Upprepa hello stegen i [associera hello NSG toohello klientdel undernät](#Associate-the-NSG-to-the-FrontEnd-subnet) tooassociate hello **NSG Backend** NSG toohello **BackEnd** undernät.

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[hantera befintliga NSG: er](virtual-network-manage-nsg-arm-portal.md)
* [Aktivera loggning](virtual-network-nsg-manage-log.md) för NSG: er.

