---
title: "Azure Active Directory Domain Services: Skapa eller välja ett virtuellt nätverk | Microsoft Docs"
description: Aktivera Azure Active Directory Domain Services med hello klassiska Azure-portalen
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 13ab1608-e3d8-40de-9f7b-9b5b42199af4
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/28/2017
ms.author: maheshu
ms.openlocfilehash: 212c741b20e846742d94f70342c4263ce8984153
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-or-select-a-virtual-network-for-azure-active-directory-domain-services"></a>Skapa eller välja ett virtuellt nätverk för Azure Active Directory Domain Services
## <a name="before-you-begin"></a>Innan du börjar
Se för[nätverk överväganden för Azure Active Directory Domain Services](active-directory-ds-networking.md).

## <a name="task-2-create-an-azure-virtual-network"></a>Uppgift 2: Skapa ett virtuellt Azure-nätverk
hello nästa konfigurationsåtgärd är toocreate virtuellt Azure-nätverk och ett undernät i den. Du aktiverar Azure Active Directory Domain Services i detta undernät inom ditt virtuella nätverk. Om du har ett befintligt virtuellt nätverk som du vill använda toouse kan du hoppa över det här steget.

> [!NOTE]
> Se till att hello Azure-nätverket som du skapar eller väljer toouse med Azure Active Directory Domain Services tillhör tooan Azure-region som stöds av Azure Active Directory Domain Services. tooascertain hello Azure-regioner som Azure Active Directory Domain Services är tillgängligt, se [Azure-tjänster efter region](https://azure.microsoft.com/regions/#services/).
>
>Observera hello namnet på hello virtuellt nätverk tooensure att du väljer hello rätt virtuellt nätverk när du aktiverar Azure Active Directory Domain Services i ett efterföljande konfigurationssteg.


toocreate virtuellt Azure-nätverk som du vill tooenable Azure Active Directory Domain Services, följer du dessa konfigurationsinstruktioner:

1. Gå toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. I hello vänster och välj **nätverk**.

    ![Nätverksnod](./media/active-directory-domain-services-getting-started/networks-node.png)  
    Hej **virtuella nätverk** öppnas.
3. Hello i åtgärdsfönstret längst ned hello hello-fönstret klickar du på **ny**.

    ![Fönstret Virtuellt nätverk](./media/active-directory-domain-services-getting-started/virtual-networks.png)
4. Klicka på **Nätverkstjänster** och välj sedan **Virtuellt nätverk**.

    ![Virtuellt nätverk – snabbregistrering](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)
5. toocreate ett virtuellt nätverk, klickar du på **Snabbregistrering**.

6. Ange en **namn** för din virtuella nätverks- och bör du göra hello följande:
    * Du kan välja tooconfigure **adressutrymmet** eller **maximalt antal Virtuella datorer** för det här nätverket.
    * Du kan lämna hello **DNS-server** som **ingen** just nu. Du kan uppdatera hello-inställningen när du har aktiverat Azure Active Directory Domain Services.
7. I hello **plats** listrutan väljer du en Azure-regionen stöds.  
    tooascertain hello Azure-regioner som Azure Active Directory Domain Services är tillgängligt, se [Azure-tjänster efter region](https://azure.microsoft.com/regions/#services/).
8. toocreate ditt virtuella nätverk, klickar du på **skapa ett virtuellt nätverk**.

    ![Skapa ett virtuellt nätverk för Azure Active Directory Domain Services](./media/active-directory-domain-services-getting-started/create-vnet.png)
9. När du har skapat ett virtuellt nätverk, Välj hello hello virtuella nätverk och klicka sedan på hello **konfigurera** fliken.

    ![Skapa ett undernät](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)
10. Under **virtuellt nätverk adressutrymmen**, klickar du på **Lägg till undernät**, och ange ett undernät med namnet hello **AaddsSubnet**.

    ![Skapa ett undernät för Azure Active Directory Domain Services](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)

11. toocreate Hej undernät, klickar du på **spara**.


## <a name="next-step"></a>Nästa steg
[Uppgift 3: Aktivera Azure Active Directory Domain Services](active-directory-ds-getting-started-enableaadds.md)
