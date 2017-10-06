---
title: "Azure Active Directory Domain Services: Skapa hello Azure AD DC-administratörsgruppen | Microsoft Docs"
description: Aktivera Azure Active Directory Domain Services med hello klassiska Azure-portalen
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
ms.openlocfilehash: d629ab9632ef6a45b549630999ff9a122d60c040
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-classic-portal"></a>Aktivera Azure Active Directory Domain Services med hello klassiska Azure-portalen
Den här artikeln beskriver och går igenom hello konfigurationsåtgärder som krävs för att du tooenable Azure Active Directory Domain Services (Azure AD DS) för din Azure Active Directory (Azure AD)-klient.

> [!NOTE]
> [**Försök i stället hello (förhandsgranskning) Azure portal avanmäla**](active-directory-ds-getting-started.md). 
>

## <a name="task-1-create-hello-azure-ad-dc-administrators-group"></a>Uppgift 1: skapa administratörsgruppen för hello Azure AD-Domänkontrollant
hello första uppgiften är toocreate en administrativ grupp i Azure AD-klienten. Den här särskilda administrativa gruppen kallas *AAD DC administratörer*. Medlemmar i den här gruppen har beviljats administratörsbehörighet på datorer som är ansluten till domänen toohello Azure Active Directory Domain Services-hanterad domän. Den här gruppen har lagts till toohello administratörsgruppen på domänanslutna datorer. Medlemmar i den här gruppen kan dessutom använda Fjärrskrivbord tooconnect via fjärranslutning toodomain anslutna datorer.  

> [!NOTE]
> Du har inte behörighet som domänadministratör eller företagsadministratör på hello-hanterad domän som du skapat med hjälp av Azure Active Directory Domain Services. Dessa behörigheter är reserverade av hello-tjänsten på hanterade domäner och görs inte tillgängliga toousers inom hello-klient. Du kan dock använda hello särskilda administrativa gruppen skapas i den här konfigurationen uppgiften tooperform vissa Privilegierade åtgärder. Dessa åtgärder är ansluta till datorer toohello domän, som tillhör toohello administratörsgrupp på domänanslutna datorer och konfigurera Grupprincip.
>

Uppgiften konfiguration du skapar hello administrativ grupp och Lägg till en eller flera användare i katalogen toohello gruppen. toocreate hello administrativa gruppen för Azure Active Directory Domain Services, hello följande:

1. Gå toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Välj hello hello vänster **Active Directory** knappen.
3. Välj hello Azure AD-klienten (katalogen) som du vill tooenable Azure Active Directory Domain Services. Du kan skapa en enda domän för varje Azure AD-katalog.

    ![Välj Azure AD-katalog](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. På hello **preview directory** klickar du på hello **grupper** fliken.
5. tooadd en grupp tooyour Azure AD-klient hello i åtgärdsfönstret längst ned hello hello-fönstret klickar du på **Lägg till grupp**.

    ![hello-knappen Lägg till grupp](./media/active-directory-domain-services-getting-started/add-group-button.png)
6. I hello **Lägg till grupp** dialogrutan skapar du en grupp med namnet **AAD DC administratörer**, och sedan ange **grupptyp** för**säkerhet**.

   > [!WARNING]
   > tooenable åtkomst i Azure Active Directory Domain Services-hanterade domänen, skapa en grupp med det exakta namnet.
   >
   >

    ![dialogrutan Lägg till grupp för hello](./media/active-directory-domain-services-getting-started/create-admin-group.png)
7. I hello **beskrivning** ange en beskrivning som gör det möjligt för andra toounderstand att den här gruppen ger administrativ behörighet i Azure Active Directory Domain Services.
8. När du har skapat hello grupp, klickar du på hello grupp namnet tooview dess egenskaper.
9. tooadd användare som medlemmar i gruppen hello, längst ned hello hello fönstret klickar du på hello **lägga till medlemmar** knappen.

    ![Lägg till knappen för medlemmar av gruppen](./media/active-directory-domain-services-getting-started/add-group-members-button.png)
10. I hello **lägga till medlemmar** dialogrutan Välj hello-användare som ska vara medlemmar i den här gruppen och klicka sedan på hello bock på hello lägre direkt.

    ![Lägg till användare tooadministrators grupp](./media/active-directory-domain-services-getting-started/add-group-members.png)


## <a name="next-step"></a>Nästa steg
[Uppgift 2: skapa eller välj ett virtuellt Azure-nätverk](active-directory-ds-getting-started-vnet.md)
