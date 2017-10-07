---
title: "aaaDisable användarinloggningar för en enterprise-app i Azure Active Directory | Microsoft Docs"
description: "Hur toodisable ett företagsprogram så att inga användare kan logga in tooit i Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a27562f9-18dc-42e8-9fee-5419566f8fd7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 4c560b59359d433b0852a7606cc2cc0204866234
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a>Inaktivera användarinloggningar för en enterprise-app i Azure Active Directory
Det är enkelt toodisable ett företagsprogram så att inga användare kan logga in tooit i Azure Active Directory (AD Azure). Du måste ha en hello behörighet toomanage hello och du måste vara global administratör för hello-katalogen.

## <a name="how-do-i-disable-user-sign-ins"></a>Hur inaktiverar användarinloggningar?
1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.
2. Välj **fler tjänster**, ange **Azure Active Directory** i hello textruta och välj sedan **RETUR**.
3. På hello **Azure Active Directory** -  ***directoryname*** bladet (det vill säga hello Azure AD bladet för hello-katalog som du hanterar) Välj **företagsprogram**.

    ![Öppna företagsappar](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)
4. På hello **företagsprogram** bladet väljer **alla program**. Du kan se en lista över hello-appar som du kan hantera.
5. På hello **företagsprogram - alla program** bladet väljer du en app.
6. På hello ***appname*** bladet (det vill säga hello bladet med hello namn för valda hello-appen i hello rubrik) Välj **egenskaper**.

    ![Att välja hello kommandot för alla program](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)
7. På hello ***appname*** - **egenskaper** bladet väljer **nr** för **aktiverats för användare i toosign?**.
8. Välj hello **spara** kommando.

## <a name="next-steps"></a>Nästa steg
* [Se alla grupper](active-directory-groups-view-azure-portal.md)
* [Tilldela en användare eller grupp tooan enterprise app](active-directory-coreapps-assign-user-azure-portal.md)
* [Ta bort en användare eller grupp från en enterprise-app](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Ändra hello namn eller logotyp av en enterprise-app](active-directory-coreapps-change-app-logo-user-azure-portal.md)
