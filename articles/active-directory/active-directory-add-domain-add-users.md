---
title: "aaaAssign användare tooa anpassade domäner i Azure Active Directory | Microsoft Docs"
description: "Hur toopopulate en anpassad domän i Azure Active Directory med användarkonton."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 717b5a7c-7bc3-4ab1-98b5-4740b53338fe
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 23c338a361a90fddf42d4df90db94c9774305886
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="assign-users-tooa-custom-domain"></a>Tilldela användare tooa anpassad domän
När du har lagt till din anpassade domän tooAzure Active Directory, måste du lägga till hello användarkonton för den här domänen så att du kan börja autentiseras.

> [!IMPORTANT]
> Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln. Hur toomanage ditt domännamn i administrationscentret för hello Azure AD, finns i [hantera anpassade domännamn i Azure Active Directory](active-directory-domains-manage-azure-portal.md).

## <a name="users-synced-from-a-on-premises-directory"></a>Användare som synkroniseras från en lokal katalog
Om du redan har installerat en anslutning mellan din lokala Active Directory och Azure Active Directory, fylla synkronisering hello konton. Mer information om hur toosynchronize Azure Active Directory med din lokala Active Directory, se [integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).

## <a name="users-added-and-managed-in-hello-cloud"></a>Användare läggs till och hanteras i hello moln
toochange hello domän för ett befintligt användarkonto:

1. Öppna hello klassiska Azure-portalen med ett konto som är en global administratör eller en användaren administratör.
2. Öppna din katalog.
3. Välj hello **användare** fliken.
4. Välj hello användare hello-listan.
5. Ändra hello domän för hello användaren och väljer sedan **spara**.

Detta kan även göras med hjälp av [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) eller hello [Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).

## <a name="select-a-custom-domain-when-creating-a-new-user"></a>Välj en anpassad domän när du skapar en ny användare
1. Öppna hello klassiska Azure-portalen med ett konto som är en global administratör eller en användaren administratör.
2. Öppna din katalog.
3. Välj hello **användare** fliken.
4. Välj i kommandofältet hello **Lägg till**.
5. När du lägger till hello användarnamn Välj hello domänen hello domänlistan.
6. Välj **Spara**.

## <a name="next-steps"></a>Nästa steg
* [Med hjälp av anpassad domän namn toosimplify hello inloggningen för dina användare](active-directory-add-domain.md)
* [Hantera egna domännamn](active-directory-add-manage-domain-names.md)
* [Lär dig mer om domänhanteringsbegrepp i Azure AD](active-directory-add-domain-concepts.md)

