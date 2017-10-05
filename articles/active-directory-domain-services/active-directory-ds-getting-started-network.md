---
title: "Azure Active Directory Domain Services: Komma igång | Microsoft Docs"
description: "Aktivera Azure Active Directory Domain Services med Azure-portalen (förhandsgranskning)"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: maheshu
ms.openlocfilehash: 7f420d60862adf61e4f21e5abac2932a742bd55d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal-preview"></a>Aktivera Azure Active Directory Domain Services med Azure-portalen (förhandsgranskning)


## <a name="before-you-begin"></a>Innan du börjar
Se [Nätverksrelaterade aspekter att tänka på med Azure Active Directory Domain Services](active-directory-ds-networking.md).


## <a name="task-2-configure-network-settings"></a>Uppgift 2: konfigurera nätverksinställningar
Nästa konfigurationsåtgärd är att skapa ett virtuellt Azure-nätverk och en dedikerad undernät i den. Du aktiverar Azure Active Directory Domain Services i detta undernät inom ditt virtuella nätverk. Du kan också välja ett befintligt virtuellt nätverk och skapa dedikerade undernätet i den.

1. Klicka på **för virtuella nätverk** att välja ett virtuellt nätverk.
2. På den **Välj virtuellt nätverk** bladet visas alla befintliga virtuella nätverk. Du ser bara de virtuella nätverk som tillhör resursgruppen och Azure-plats som du har valt på den **grunderna** sidan i guiden.

3. Välj det virtuella nätverket som Azure AD Domain Services ska aktiveras. Klicka på **Skapa nytt**, om du vill skapa ett nytt virtuellt nätverk. Vi rekommenderar starkt att använda ett dedikerat undernät för Azure AD Domain Services. Om du väljer ett befintligt virtuellt nätverk [skapa ett dedikerat undernät med virtuella nätverk](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) och välj sedan det undernätet. 

    ![Välj virtuella nätverk](./media/getting-started/domain-services-blade-network-pick-vnet.png)

4. Klicka på **undernät** att välja dedikerad undernät i det här virtuella nätverket att aktivera din nya hanterade domän. I den **skapa undernät** bladet, anger ett namn för undernätet och klickar på **OK** när du är klar. Till exempel skapa ett undernät med namnet DomainServices, vilket gör det lättare för andra administratörer att förstå vad som har distribuerats i undernätet.

    ![Välj undernät i det virtuella nätverket](./media/getting-started/domain-services-blade-network-pick-subnet.png)

  > [!NOTE]
  > **Riktlinjer för att välja ett undernät**
  > 1. Använd en dedikerad undernät för Azure AD Domain Services. Distribuera inte eventuella andra virtuella datorer till det här undernätet. Den här konfigurationen kan du konfigurera nätverkssäkerhetsgrupper (NSG: er) för dina arbetsbelastningar för virtuella datorer utan att avbryta din hanterade domän. Mer information finns i [nätverk överväganden för Azure Active Directory Domain Services](active-directory-ds-networking.md).
  2. Välj inte Gateway-undernätet för att distribuera Azure AD Domain Services, eftersom den inte är en konfiguration som stöds.
  3. Kontrollera att det undernät som du har valt har tillräckligt utrymme för tillgängliga adresser - minst 3-5 tillgängliga IP-adresser.
  >

5. När du är klar klickar du på **OK** att gå vidare till den **administratörsgruppen** sidan i guiden.


## <a name="next-step"></a>Nästa steg
[Uppgift 3: Konfigurera administrativ grupp och aktivera Azure AD Domain Services](active-directory-ds-getting-started-admingroup.md)
