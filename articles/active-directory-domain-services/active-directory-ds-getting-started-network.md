---
title: "Azure Active Directory Domain Services: Komma igång | Microsoft Docs"
description: "Aktivera Azure Active Directory Domain Services med hello Azure-portalen (förhandsgranskning)"
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
ms.openlocfilehash: 7695dabb11df8d9dcfdac24996edf021af2e7f52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-portal-preview"></a>Aktivera Azure Active Directory Domain Services med hello Azure-portalen (förhandsgranskning)


## <a name="before-you-begin"></a>Innan du börjar
Se för[nätverk överväganden för Azure Active Directory Domain Services](active-directory-ds-networking.md).


## <a name="task-2-configure-network-settings"></a>Uppgift 2: konfigurera nätverksinställningar
hello nästa konfigurationsåtgärd är toocreate virtuellt Azure-nätverk och en dedikerad undernät i den. Du aktiverar Azure Active Directory Domain Services i detta undernät inom ditt virtuella nätverk. Du kan också välja ett befintligt virtuellt nätverk och skapa hello dedikerad undernät i den.

1. Klicka på **för virtuella nätverk** tooselect ett virtuellt nätverk.
2. På hello **Välj virtuellt nätverk** bladet visas alla befintliga virtuella nätverk. Du ser endast virtuella nätverk för hello som tillhör toohello resursgruppen och Azure-plats som du har valt på hello **grunderna** sidan i guiden.

3. Välj hello virtuellt nätverk där Azure AD Domain Services ska aktiveras. Klicka på **Skapa nytt**, om du föredrar toocreate ett nytt virtuellt nätverk. Vi rekommenderar starkt att använda ett dedikerat undernät för Azure AD Domain Services. Om du väljer ett befintligt virtuellt nätverk [skapa ett dedikerat undernät använder hello tillägg för virtuella nätverk](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) och välj sedan det undernätet. 

    ![Välj virtuella nätverk](./media/getting-started/domain-services-blade-network-pick-vnet.png)

4. Klicka på **undernät** toopick hello dedikerad undernät i det här virtuella nätverket, inom vilka tooenable din nya hanterade domän. I hello **skapa undernät** bladet, ange ett namn för hello undernätet och klickar på **OK** när du är klar. Till exempel skapa ett undernät med namnet hello DomainServices, vilket gör det enkelt för andra administratörer toounderstand vad distribueras inom hello undernät.

    ![Välj undernät inom hello virtuellt nätverk](./media/getting-started/domain-services-blade-network-pick-subnet.png)

  > [!NOTE]
  > **Riktlinjer för att välja ett undernät**
  > 1. Använd en dedikerad undernät för Azure AD Domain Services. Distribuera inte alla andra virtuella datorer toothis undernät. Den här konfigurationen kan du tooconfigure nätverkssäkerhetsgrupper (NSG: er) för dina arbetsbelastningar för virtuella datorer utan att avbryta din hanterade domän. Mer information finns i [nätverk överväganden för Azure Active Directory Domain Services](active-directory-ds-networking.md).
  2. Välj inte hello Gateway-undernät för att distribuera Azure AD Domain Services, eftersom den inte är en konfiguration som stöds.
  3. Se till att hello undernät som du har valt har tillräckligt utrymme för tillgängliga adresser - minst 3-5 tillgängliga IP-adresser.
  >

5. När du är klar klickar du på **OK** toomove på toohello **administratörsgruppen** hello guiden.


## <a name="next-step"></a>Nästa steg
[Uppgift 3: Konfigurera administrativ grupp och aktivera Azure AD Domain Services](active-directory-ds-getting-started-admingroup.md)
