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
ms.date: 08/28/2017
ms.author: maheshu
ms.openlocfilehash: 79cbb21c4a50194f5ad8ca1a4a8493ee4a260a9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-portal-preview"></a>Aktivera Azure Active Directory Domain Services med hello Azure-portalen (förhandsgranskning)
Den här artikeln visar hur tooenable Azure Active Directory Domain Services (Azure AD DS) med hjälp av hello Azure-portalen.


toolaunch hello **aktivera Azure AD Domain Services** guiden, fullständig hello följande steg:

1. Gå toohello [Azure-portalen](https://portal.azure.com).
2. Hello vänster, klicka på **ny**.
3. I hello **ny** bladet, skriver **Domain Services** i hello sökfältet.

    ![Sök efter domäntjänster](./media/getting-started/search-domain-services.png)

4. Klicka på tooselect **Azure AD Domain Services** hello listan över sökförslag. På hello **Azure AD Domain Services** bladet, klickar du på hello **skapa** knappen.

    ![Domain services-bladet](./media/getting-started/domain-services-blade.png)

5. Hej **aktivera Azure AD Domain Services** starta guiden.


## <a name="task-1-configure-basic-settings"></a>Uppgift 1: Konfigurera grundläggande inställningar
I hello **grunderna** sidan för hello guiden kan du ange hello DNS-domännamn för hello hanterade domän. Du kan också välja hello resursgruppen och Azure-plats toowhich hello hanterad domän som ska distribueras.

![Konfigurera grunderna](./media/getting-started/domain-services-blade-basics.png)

1. Välj hello **DNS-domännamn** för din hanterade domän.

   * hello standarddomännamnet för hello katalogen (med en **. onmicrosoft.com** suffix) har angetts som standard.

   * Du kan också skriva i ett anpassat domännamn. I det här exemplet hello domännamn är *contoso100.com*.

     > [!WARNING]
     > hello-prefixet för ett angivet domännamn (till exempel *contoso100* i hello *contoso100.com* domännamn) får innehålla högst 15 tecken. Du kan inte skapa en hanterad domän med ett prefix som är längre än 15 tecken.
     >
     >

2. Se till att hello DNS-domännamn som du har valt för hello hanterade domänen inte redan finns i hello virtuella nätverk. I synnerhet kontrollera om:

   * Du redan har en domän med hello samma DNS-domännamn på hello virtuellt nätverk.

   * hello virtuellt nätverk där du planerar att tooenable hello hanterade domänen har en VPN-anslutning med ditt lokala nätverk. I det här scenariot, se till att du inte har en domän med hello samma DNS-namnet på ditt lokala nätverk.

   * Du har en befintlig molntjänst med det namnet på hello virtuellt nätverk.

3. Välj hello **typen av virtuellt nätverk**. Som standard hello **Resource Manager** typ virtuella nätverk som väljs. Vi rekommenderar att du använder den här typen av virtuellt nätverk för alla nyligen skapade hanterade domäner.

4. Välj hello Azure **prenumeration** som du vill att toocreate hello hanterad domän.

5. Välj hello **resursgruppen** toowhich hello hanterad domän ska tillhöra. Du kan välja antingen hello **Skapa nytt** eller **använda befintliga** alternativ tooselect hello resursgruppen.

6. Välj hello Azure **plats** i vilka hello hanterad domän ska skapas. På hello **nätverk** sidan hello guiden kan du se endast virtuella nätverk som tillhör toohello plats som du har valt.

7. När du är klar klickar du på **OK** toomove på toohello **nätverk** hello guiden.


## <a name="next-step"></a>Nästa steg
[Uppgift 2: Konfigurera nätverksinställningar](active-directory-ds-getting-started-network.md)
