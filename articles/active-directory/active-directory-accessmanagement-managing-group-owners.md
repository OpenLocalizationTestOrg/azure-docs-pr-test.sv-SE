---
title: "Nästa steg för hantering med hjälp av grupper | Microsoft Docs"
description: "Avancerade hur-för hantering av säkerhetsgrupper och hur du använder dessa grupper för att hantera åtkomst till en resurs."
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
ms.openlocfilehash: 82fbeb379e90add09f7c569111053f6e9b1bc9c5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="managing-owners-for-a-group"></a>Hantera ägare för en grupp
När en resursägare har tilldelats till en resurs till en Azure AD-grupp, hanteras medlemskap i gruppen av gruppägare. Effektivt resursägaren behörighet att tilldela användare till resursen till ägare av gruppen.

> [!IMPORTANT]
> Microsoft rekommenderar att du hanterar Azure AD via [Azure AD administratörscenter](https://aad.portal.azure.com) på Azure Portal istället för via den klassiska Azure-portalen som nämns i den här artikeln. 

## <a name="assigning-group-ownership"></a>Tilldela ägarskap för grupp
**Att lägga till en ägare till en grupp**

1. I den [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory**, och sedan öppna din organisations katalog.
2. Välj den **grupper** och sedan öppna den grupp som du vill lägga till ägare till.
3. Välj **lägga till ägare**.
4. På den **lägga till ägare** markerar du den användare som du vill lägga till som ägare av den här gruppen och kontrollera att namnet läggs till i **valda** fönstret.

**Ta bort en ägare från en grupp**

1. I den [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory**, och sedan öppna din organisations katalog.
2. Välj den **grupper** och sedan öppna den grupp som du vill ta bort en ägare från.
3. Välj den **ägare** fliken.
4. Välj en ägare som du vill ta bort från den här gruppen och välj sedan **ta bort**.

## <a name="additional-information"></a>Ytterligare information
Dessa artiklar innehåller ytterligare information om Azure Active Directory.

* [Hantera åtkomst till resurser med Azure Active Directory-grupper](active-directory-manage-groups.md)
* [Azure Active Directory-cmdletar för att konfigurera gruppinställningar](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Vad är Azure Active Directory?](active-directory-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
