---
title: "Azure Active Directory Domain Services: Skapa Azure AD-DC-administratörsgruppen | Microsoft Docs"
description: "Aktivera Azure Active Directory Domain Services med hjälp av den klassiska Azure-portalen"
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
ms.date: 07/13/2017
ms.author: maheshu
ms.openlocfilehash: 5ed0125e05928cf0f6d9941e099b433ecb46e6e2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-classic-portal"></a>Aktivera Azure Active Directory Domain Services med hjälp av den klassiska Azure-portalen
Den här artikeln beskriver och går igenom konfigurationsuppgifter som krävs om du vill aktivera Azure Active Directory Domain Services (Azure AD DS) för din Azure Active Directory (Azure AD)-klient.

> [!NOTE]
> [**Prova den nya portalen (förhandsgranskning) Azure-upplevelsen i stället**](active-directory-ds-getting-started.md). 
>

## <a name="task-1-create-the-azure-ad-dc-administrators-group"></a>Uppgift 1: skapa administratörsgruppen för Azure AD-Domänkontrollant
Den första uppgiften är att skapa en administrativ grupp i Azure AD-klienten. Den här särskilda administrativa gruppen kallas *AAD DC administratörer*. Medlemmar i den här gruppen har beviljats administratörsbehörighet på datorer som är ansluten till domänen till Azure Active Directory Domain Services-hanterad domän. Den här gruppen har lagts till administratörsgruppen på domänanslutna datorer. Medlemmar i den här gruppen kan dessutom använda Fjärrskrivbord för att fjärransluta till domänanslutna datorer.  

> [!NOTE]
> Du har inte behörighet som domänadministratör eller Företagsadministratörer på den hanterade domänen som du skapat med hjälp av Azure Active Directory Domain Services. Dessa behörigheter är reserverade av tjänsten på hanterade domäner och görs inte tillgängliga för användare i klienten. Du kan dock använda särskilda administrativa gruppen som skapats i konfigurationsåtgärden utföra vissa Privilegierade åtgärder. Dessa åtgärder omfattar ansluta datorer till domänen, som hör till gruppen administration på domänanslutna datorer och konfigurera Grupprincip.
>

Uppgiften konfiguration du skapar den administrativa gruppen och lägger till en eller flera användare i din katalog i gruppen. Skapa den administrativa gruppen för Azure Active Directory Domain Services genom att göra följande:

1. Gå till den [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Välj knappen **Active Directory** i den vänstra rutan.
3. Välj Azure AD-klienten (katalogen) som du vill aktivera Azure Active Directory Domain Services. Du kan skapa en enda domän för varje Azure AD-katalog.

    ![Välj Azure AD-katalog](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. På den **preview directory** klickar du på den **grupper** fliken.
5. Om du vill lägga till en grupp i din Azure AD-klient, i åtgärdsfönstret längst ned i fönstret klickar du på **Lägg till grupp**.

    ![Knappen Lägg till grupp](./media/active-directory-domain-services-getting-started/add-group-button.png)
6. I den **Lägg till grupp** dialogrutan skapar du en grupp med namnet **AAD DC administratörer**, och sedan ange **grupptyp** till **säkerhet**.

   > [!WARNING]
   > Skapa en grupp för att möjliggöra åtkomst i Azure Active Directory Domain Services-hanterade domänen med det exakta namnet.
   >
   >

    ![Dialogrutan Lägg till grupp](./media/active-directory-domain-services-getting-started/create-admin-group.png)
7. I den **beskrivning** ange en beskrivning som gör det möjligt för andra att förstå att den här gruppen ger administrativ behörighet i Azure Active Directory Domain Services.
8. När du har skapat gruppen klickar du på gruppnamnet att visa dess egenskaper.
9. Om du vill lägga till användare som medlemmar i gruppen längst ned i fönstret klickar du på den **lägga till medlemmar** knappen.

    ![Lägg till knappen för medlemmar av gruppen](./media/active-directory-domain-services-getting-started/add-group-members-button.png)
10. I den **lägga till medlemmar** dialogrutan väljer du de användare som ska vara medlemmar i den här gruppen och klicka sedan på bock längst ned till höger.

    ![Lägga till användare i administratörsgruppen](./media/active-directory-domain-services-getting-started/add-group-members.png)


## <a name="next-step"></a>Nästa steg
[Uppgift 2: skapa eller välj ett virtuellt Azure-nätverk](active-directory-ds-getting-started-vnet.md)
