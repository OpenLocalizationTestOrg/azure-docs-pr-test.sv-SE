---
title: "aaaRemove en tilldelning av användare eller grupp från en enterprise-app i Azure Active Directory | Microsoft Docs"
description: "Hur tooremove hello åt tilldelning av en användare eller grupp från en enterprise-app i Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 7b2d365b-ae92-477f-9702-353cc6acc5ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: c067ecf59b4dedfe8f848357ca8bd545bdc610eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a>Ta bort en användare eller grupp från en enterprise-app i Azure Active Directory
Det är enkelt tooremove en användare eller en grupp som tilldelas åtkomst tooone enterprise program i Azure Active Directory (AD Azure). Du måste ha en hello behörighet toomanage hello och du måste vara global administratör för hello-katalogen.

## <a name="how-do-i-remove-a-user-or-group-assignment"></a>Hur tar jag bort en användare eller grupptilldelning?
1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.
2. Välj **fler tjänster**, ange **Azure Active Directory** i hello textruta och välj sedan **RETUR**.
3. På hello **Azure Active Directory - *directoryname***  bladet (det vill säga hello Azure AD bladet för hello-katalog som du hanterar) Välj **företagsprogram**.

    ![Öppna företagsappar](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)
4. På hello **företagsprogram** bladet väljer **alla program**. Visas en lista över hello-appar som du kan hantera.
5. På hello **företagsprogram - alla program** bladet väljer du en app.
6. På hello ***appname*** bladet (det vill säga hello bladet med hello namn för valda hello-appen i hello rubrik) Välj **användare och grupper**.

    ![Att välja användare eller grupper](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)
7. På hello ***appname*** **-användaren & grupptilldelning** bladet Välj en av flera användare eller grupper och välj sedan hello **ta bort** kommando. Bekräfta beslutet hello i Kommandotolken.

    ![Välja hello ta borttagningskommando](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a>Nästa steg
* [Visa alla mina grupper](active-directory-groups-view-azure-portal.md)
* [Tilldela en användare eller grupp tooan enterprise app](active-directory-coreapps-assign-user-azure-portal.md)
* [Inaktivera användarinloggningar för en enterprise-app](active-directory-coreapps-disable-app-azure-portal.md)
* [Ändra hello namn eller logotyp av en enterprise-app](active-directory-coreapps-change-app-logo-user-azure-portal.md)
