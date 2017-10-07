---
title: "aaaManage NSG: er med hjälp av hello Azure-portalen | Microsoft Docs"
description: "Lär dig hur toomanage befintliga NSG: er med hjälp av hello Azure-portalen."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 5d55679d-57da-457c-97dc-1e1973909ee5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2016
ms.author: jdial
ms.openlocfilehash: ad9a4060bd81bae4597ad5a4f59622e10cd214cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-nsgs-using-hello-portal"></a>Hantera NSG: er med hjälp av hello portal

> [!div class="op_single_selector"]
> * [Portal](virtual-network-manage-nsg-arm-portal.md)
> * [PowerShell](virtual-network-manage-nsg-arm-ps.md)
> * [Azure CLI](virtual-network-manage-nsg-arm-cli.md)
>

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md). Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello klassiska distributionsmodellen.
>

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="retrieve-information"></a>Hämta Information
Du kan visa dina befintliga NSG: er, hämta regler för en befintlig NSG och ta reda på vilka resurser en NSG är kopplad till.

### <a name="view-existing-nsgs"></a>Visa befintliga NSG: er

tooview alla befintliga NSG: er i en prenumeration fullständig hello följande steg:

1. Från en webbläsare, navigerar du toohttp://portal.azure.com och, om det behövs, logga in med ditt Azure-konto.

2. Klicka på **Bläddra >** > **Nätverkssäkerhetsgrupper**.

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure1.png)

3. Kontrollera hello listan över NSG: er i hello **Nätverkssäkerhetsgrupper** bladet.

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure2.png)

### <a name="view-nsgs-in-a-resource-group"></a>Visa NSG: er i en resursgrupp

tooview hello listan över NSG: er i hello **RG NSG** resursgrupp, fullständig hello följande steg:

1. Klicka på **resursgrupper >** > **RG NSG** > **...** .

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure3.png)

2. Hello lista över resurser, leta efter hello NSG visas objekt som visas i hello **resurser** bladet nedan.

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure4.png)

### <a name="list-all-rules-for-an-nsg"></a>Visa en lista med alla regler för en NSG

tooview hello regler för en NSG som heter **NSG-klientdel**fullständig hello följande steg:

1. Från hello **Nätverkssäkerhetsgrupper** bladet eller hello **resurser** bladet som visas ovan, klickar du på **NSG-klientdel**.

2. I hello **inställningar** klickar du på **inkommande säkerhetsregler**.

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure5.png)

3. Hej **inkommande säkerhetsregler** bladet visas enligt nedan.

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure6.png)

4. I hello **inställningar** klickar du på **utgående säkerhetsregler** toosee hello utgående regler.

    > [!NOTE]
    > standardregler för tooview, klicka på hello **standard regler** ikonen hello överst i hello blad som visar hello regler.
    >

### <a name="view-nsgs-associations"></a>Visa NSG: er associationer

tooview vilka resurser hello **NSG-klientdel** NSG är associerat med, fullständig hello följande steg:

1. Från hello **Nätverkssäkerhetsgrupper** bladet eller hello **resurser** bladet som visas ovan, klickar du på **NSG-klientdel**.

2. I hello **inställningar** klickar du på **undernät** tooview vilka undernät är associerade toohello NSG.

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure7.png)

3. I hello **inställningar** klickar du på **nätverksgränssnitt** tooview vad nätverkskort associeras toohello NSG.

## <a name="manage-rules"></a>Hantera regler
Du kan lägga till regler tooan befintliga NSG, redigera befintliga regler och ta bort regler.

### <a name="add-a-rule"></a>Lägg till en regel
tooadd en regel som tillåter **inkommande** trafik tooport **443** från alla datorn toohello **NSG-klientdel** NSG fullständig hello följande steg:

1. Från hello **Nätverkssäkerhetsgrupper** bladet eller hello **resurser** bladet som visas ovan, klickar du på **NSG-klientdel**.
2. I hello **inställningar** klickar du på **inkommande säkerhetsregler**.
3. I hello **inkommande säkerhetsregler** bladet, klickar du på **Lägg till**. Sedan hello **Lägg till ingående säkerhetsregel** bladet fyller hello värden som visas nedan och klicka sedan på **OK**.

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure8.png)

    Lägg märke till efter några sekunder hello ny regel i hello **inkommande säkerhetsregler** bladet.

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure9.png)

### <a name="change-a-rule"></a>Ändra en regel
toochange hello regeln som skapade ovan tooallow inkommande trafik från hello **Internet** endast, fullständig hello följande steg:

1. Från hello **Nätverkssäkerhetsgrupper** bladet eller hello **resurser** bladet som visas ovan, klickar du på **NSG-klientdel**.
2. I hello **inställningar** klickar du på hello regeln som skapade ovan.
3. I hello **Tillåt https** bladet, ändra hello **källa** egenskapen enligt nedan och klickar sedan på **spara**.

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure10.png)

### <a name="delete-a-rule"></a>Ta bort en regel

toodelete hello regeln skapade ovan, fullständig hello följande steg:

1. Från hello **Nätverkssäkerhetsgrupper** bladet eller hello **resurser** bladet som visas ovan, klickar du på **NSG-klientdel**.
2. I hello **inställningar** klickar du på hello regeln som skapade ovan.
3. I hello **Tillåt https** bladet, klickar du på **ta bort**, och klicka sedan på **Ja**.

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure11.png)

## <a name="manage-associations"></a>Hantera kopplingar
Du kan koppla en NSG toosubnets och nätverkskort. Du kan också koppla bort en NSG från alla resurser som den är kopplad till.

### <a name="associate-an-nsg-tooa-nic"></a>Koppla en NSG tooa NIC
tooassociate hello **NSG-klientdel** NSG toohello **TestNICWeb1** NIC, fullständig hello följande steg:

1. Från hello **Nätverkssäkerhetsgrupper** bladet eller hello **resurser** bladet som visas ovan, klickar du på **NSG-klientdel**.
2. I hello **inställningar** klickar du på **nätverksgränssnitt** > **associera** > **TestNICWeb1**.

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure12.png)

### <a name="dissociate-an-nsg-from-a-nic"></a>Koppla bort en NSG från ett nätverkskort

toodissociate hello **NSG-klientdel** NSG från hello **TestNICWeb1** NIC, fullständig hello följande steg:

1. Hello Azure-portalen, klicka på **resursgrupper >** > **RG NSG** > **...**   >  **TestNICWeb1**.

2. I hello **TestNICWeb1** bladet, klickar du på **ändra säkerhet...**   >  **Ingen**.

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure13.png)

> [!NOTE]
> Du kan också använda det här bladet tooassociate hello NIC tooany befintliga NSG.
>

### <a name="dissociate-an-nsg-from-a-subnet"></a>Koppla bort en NSG från ett undernät

toodissociate hello **NSG-klientdel** NSG från hello **klientdel** undernät, fullständig hello följande steg:

1. Hello Azure-portalen, klicka på **resursgrupper >** > **RG NSG** > **...**   >  **TestVNet**.

2. I hello **inställningar** bladet, klickar du på **undernät** > **klientdel** > **nätverkssäkerhetsgruppen**  >  **Ingen**.

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure14.png)

3. I hello **klientdel** bladet, klickar du på **spara**.

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure15.png)

### <a name="associate-an-nsg-tooa-subnet"></a>Koppla en NSG tooa undernät

tooassociate hello **NSG-klientdel** NSG toohello **FronEnd** undernät igen, fullständig hello följande steg:

1. Hello Azure-portalen, klicka på **resursgrupper >** > **RG NSG** > **...**   >  **TestVNet**.
2. I hello **inställningar** bladet, klickar du på **undernät** > **klientdel** > **nätverkssäkerhetsgruppen**  >  **NSG-klientdel**.
3. I hello **klientdel** bladet, klickar du på **spara**.

> [!NOTE]
> Du kan även associera en NSG tooa undernät från thh NSG **inställningar** bladet.
>

## <a name="delete-an-nsg"></a>Ta bort en NSG
Du kan bara ta bort en NSG om den inte är kopplad till tooany resurs. toodelete en NSG fullständig hello följande:.

1. Hello Azure-portalen, klicka på **resursgrupper >** > **RG NSG** > **...**   >  **NSG-klientdel**.
2. I hello **inställningar** bladet, klickar du på **nätverksgränssnitt**.
3. Om det finns några nätverkskort i listan, klicka på hello NIC och följ steg 2 i [koppla bort en NSG från ett nätverkskort](#Dissociate-an-NSG-from-a-NIC).
4. Upprepa steg 3 för varje nätverkskort.
5. I hello **inställningar** bladet, klickar du på **undernät**.
6. Om det finns inga undernät i listan, klicka på hello undernät och följ steg 2 och 3 i [koppla bort en NSG från ett undernät](#Dissociate-an-NSG-from-a-subnet).
7. Rullar vänstra toohello **NSG-klientdel** bladet Klicka **ta bort** > **Ja**.

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure16.png)

## <a name="next-steps"></a>Nästa steg
* [Aktivera loggning](virtual-network-nsg-manage-log.md) för NSG: er.
