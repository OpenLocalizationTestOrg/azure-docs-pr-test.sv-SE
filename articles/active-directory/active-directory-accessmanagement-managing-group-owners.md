---
title: "aaaNext steg för hantering med hjälp av grupper | Microsoft Docs"
description: "Avancerade hur-för hantering av säkerhetsgrupper och hur toouse dessa grupper toomanage åtkomst tooa resurs."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 44350a3c-8ea1-4da1-aaac-7fc53933dd21
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 4fd55f893290fac3551a130f29bd12709cf551e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-owners-for-a-group"></a>Hantera ägare för en grupp
När en resursägare har tilldelat åtkomst tooa resurs tooan Azure AD-grupp, hanteras hello medlemskap i gruppen hello av hello gruppägare. hello resursägaren effektivt hello behörighet tooassign användare toohello resurs toohello ägare hello gruppen.

> [!IMPORTANT]
> Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln. 

## <a name="assigning-group-ownership"></a>Tilldela ägarskap för grupp
**tooadd en ägare tooa grupp**

1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory**, och sedan öppna din organisations katalog.
2. Välj hello **grupper** fliken och sedan öppna hello-grupp som du vill tooadd ägare till.
3. Välj **lägga till ägare**.
4. På hello **lägga till ägare** sidan, Välj hello användare du vill tooadd hello ägare av den här gruppen och kontrollera att namnet läggs toohello **valda** fönstret.

**tooremove ägare från en grupp**

1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory**, och sedan öppna din organisations katalog.
2. Välj hello **grupper** fliken och sedan öppna hello grupp som du vill tooremove ägare från.
3. Välj hello **ägare** fliken.
4. Välj hello ägare att du vill tooremove från den här gruppen och välj sedan **ta bort**.

## <a name="additional-information"></a>Ytterligare information
Dessa artiklar innehåller ytterligare information om Azure Active Directory.

* [Hantera åtkomst tooresources med Azure Active Directory-grupper](active-directory-manage-groups.md)
* [Azure Active Directory-cmdletar för att konfigurera gruppinställningar](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Vad är Azure Active Directory?](active-directory-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
