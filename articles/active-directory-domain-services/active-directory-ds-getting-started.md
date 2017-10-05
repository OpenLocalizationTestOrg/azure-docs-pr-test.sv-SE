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
ms.date: 08/28/2017
ms.author: maheshu
ms.openlocfilehash: 47507096a6245d4f1ba57a652ddf5167b3776ae9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal-preview"></a>Aktivera Azure Active Directory Domain Services med Azure-portalen (förhandsgranskning)
Den här artikeln visar hur du aktiverar Azure Active Directory Domain Services (Azure AD DS) med Azure-portalen.


Att starta den **aktivera Azure AD Domain Services** guiden gör du följande:

1. Gå till [Azure-portalen](https://portal.azure.com).
2. I den vänstra rutan klickar du på **ny**.
3. I den **ny** bladet, skriver **Domain Services** i sökfältet.

    ![Sök efter domäntjänster](./media/getting-started/search-domain-services.png)

4. Välj **Azure AD Domain Services** från listan över sökförslag. På den **Azure AD Domain Services** bladet, klickar du på den **skapa** knappen.

    ![Domain services-bladet](./media/getting-started/domain-services-blade.png)

5. Den **aktivera Azure AD Domain Services** starta guiden.


## <a name="task-1-configure-basic-settings"></a>Uppgift 1: Konfigurera grundläggande inställningar
I den **grunderna** sidan i guiden kan du ange DNS-domännamnet för den hanterade domänen. Du kan också välja resursgruppen och Azure-plats som den hanterade domänen ska distribueras.

![Konfigurera grunderna](./media/getting-started/domain-services-blade-basics.png)

1. Välj den **DNS-domännamn** för din hanterade domän.

   * Standarddomännamnet för katalogen (med en **. onmicrosoft.com** suffix) har angetts som standard.

   * Du kan också skriva i ett anpassat domännamn. I det här exemplet är det anpassade domännamnet *contoso100.com*.

     > [!WARNING]
     > Prefixet för det angivna domännamnet (till exempel *contoso100* i domännamnet *contoso100.com*) kan innehålla upp till 15 tecken. Du kan inte skapa en hanterad domän med ett prefix som är längre än 15 tecken.
     >
     >

2. Kontrollera att DNS-domännamnet som du har valt för den hanterade domänen inte redan finns i det virtuella nätverket. I synnerhet kontrollera om:

   * Du redan har en domän med samma DNS-domännamn i det virtuella nätverket.

   * Det virtuella nätverket där du planerar att aktivera den hanterade domänen har en VPN-anslutning med ditt lokala nätverk. I det här scenariot, se till att du inte har en domän med samma DNS-domännamnet i ditt lokala nätverk.

   * Du har en befintlig molntjänst med det namnet i det virtuella nätverket.

3. Välj den **typen av virtuellt nätverk**. Som standard den **Resource Manager** typ virtuella nätverk som väljs. Vi rekommenderar att du använder den här typen av virtuellt nätverk för alla nyligen skapade hanterade domäner.

4. Välj Azure **prenumeration** i som du vill skapa den hanterade domänen.

5. Välj den **resursgruppen** till den hanterade domänen ska tillhöra. Du kan välja antingen den **Skapa nytt** eller **använda befintliga** alternativ för att välja en resursgrupp.

6. Välj Azure **plats** i som den hanterade domänen ska skapas. På den **nätverk** sidan i guiden visas bara virtuella nätverk som hör till den plats som du har valt.

7. När du är klar klickar du på **OK** att gå vidare till den **nätverk** sidan i guiden.


## <a name="next-step"></a>Nästa steg
[Uppgift 2: Konfigurera nätverksinställningar](active-directory-ds-getting-started-network.md)
