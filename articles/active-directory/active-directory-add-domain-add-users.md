---
title: "Tilldela användare till en anpassad domän i Azure Active Directory | Microsoft Docs"
description: "Hur du fyller i en anpassad domän i Azure Active Directory med användarkonton."
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
ms.openlocfilehash: 39cb54a6637088c35c6aef864a804c24803f48ba
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="assign-users-to-a-custom-domain"></a>Tilldela användare till en anpassad domän
När du har lagt till en anpassad domän till Azure Active Directory, måste du lägga till användarkonton för den här domänen så att du kan börja autentiseras.

> [!IMPORTANT]
> Microsoft rekommenderar att du hanterar Azure AD via [Azure AD administratörscenter](https://aad.portal.azure.com) på Azure Portal istället för via den klassiska Azure-portalen som nämns i den här artikeln. Att hantera dina domännamn i Azure AD-administrationscentret, se [hantera anpassade domännamn i Azure Active Directory](active-directory-domains-manage-azure-portal.md).

## <a name="users-synced-from-a-on-premises-directory"></a>Användare som synkroniseras från en lokal katalog
Om du redan har installerat en anslutning mellan din lokala Active Directory och Azure Active Directory, synkronisering kan fylla i kontona. Mer information om hur du synkroniserar Azure Active Directory med din lokala Active Directory finns [integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).

## <a name="users-added-and-managed-in-the-cloud"></a>Användare läggs till och hanteras i molnet
Ändra domänen för ett befintligt användarkonto:

1. Öppna den klassiska Azure-portalen med ett konto som är en global administratör eller en användaren administratör.
2. Öppna din katalog.
3. Välj fliken **Användare**.
4. Välj användaren i listan.
5. Ändra domänen för användaren och väljer sedan **spara**.

Detta kan även göras med hjälp av [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) eller [Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).

## <a name="select-a-custom-domain-when-creating-a-new-user"></a>Välj en anpassad domän när du skapar en ny användare
1. Öppna den klassiska Azure-portalen med ett konto som är en global administratör eller en användaren administratör.
2. Öppna din katalog.
3. Välj fliken **Användare**.
4. Välj i kommandofältet **Lägg till**.
5. När du lägger till användarnamnet, väljer du den anpassade domänen från listan över domäner.
6. Välj **Spara**.

## <a name="next-steps"></a>Nästa steg
* [Använda anpassade domännamn för att förenkla inloggning för dina användare](active-directory-add-domain.md)
* [Hantera egna domännamn](active-directory-add-manage-domain-names.md)
* [Lär dig mer om domänhanteringsbegrepp i Azure AD](active-directory-add-domain-concepts.md)

