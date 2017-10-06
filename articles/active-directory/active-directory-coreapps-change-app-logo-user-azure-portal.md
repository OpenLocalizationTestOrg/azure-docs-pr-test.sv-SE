---
title: aaaChange hello namn eller logotyp av en enterprise-app i Azure Active Directory | Microsoft Docs
description: "Hur toochange hello namn eller logotyp för en anpassad enterprise-app i Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d01303ce-e6cb-4f3b-a4d6-ec29dfd68146
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: b660e1f0f6c7ffd626e17e2e3399e7169f6e8bc8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-name-or-logo-of-an-enterprise-app-in-azure-active-directory"></a>Ändra hello namn eller en enterprise-app i Azure Active Directory-logotyp
Det är enkelt toochange hello namn eller logotyp för en anpassad företagsprogram i Azure Active Directory (AD Azure). Du måste ha hello behörighet toomake ändringarna och du måste vara hello skapare av hello anpassad app.

## <a name="how-do-i-change-an-enterprise-apps-name-or-logo"></a>Hur ändrar jag enterprise appens namn eller logotyp?
1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.
2. Välj **fler tjänster**, ange **Azure Active Directory** i hello textruta och välj sedan **RETUR**.
3. På hello **Azure Active Directory - *directoryname***  bladet (det vill säga hello Azure AD bladet för hello-katalog som du hanterar) Välj **företagsprogram**.

    ![Öppna företagsappar](./media/active-directory-coreapps-change-app-logo-azure-portal/open-enterprise-apps.png)
4. På hello **företagsprogram** bladet väljer **alla program**. Visas en lista över hello-appar som du kan hantera.
5. På hello **företagsprogram - alla program** bladet väljer du en app.
6. På hello ***appname*** bladet (det vill säga hello bladet med hello namn för valda hello-appen i hello rubrik) Välj **egenskaper**.

    ![Kommandot för att välja hello-egenskaper](./media/active-directory-coreapps-change-app-logo-azure-portal/select-app.png)
7. På hello ***appname*** **-egenskaper** bladet Bläddra efter en fil toouse som en ny logotyp eller redigera hello appens namn eller båda.

    ![Ändra hello app logotyp eller nameproperties kommando](./media/active-directory-coreapps-change-app-logo-azure-portal/change-logo.png)
8. Välj hello **spara** kommando.

## <a name="next-steps"></a>Nästa steg
* [Visa alla mina grupper](active-directory-groups-view-azure-portal.md)
* [Tilldela en användare eller grupp tooan enterprise app](active-directory-coreapps-assign-user-azure-portal.md)
* [Ta bort en användare eller grupp från en enterprise-app](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Inaktivera användarinloggningar för en enterprise-app](active-directory-coreapps-disable-app-azure-portal.md)
