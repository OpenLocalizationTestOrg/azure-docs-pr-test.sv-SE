---
title: "Ändra namnet eller logotypen av en enterprise-app i Azure Active Directory | Microsoft Docs"
description: "Så här ändrar du namnet eller logotypen för en anpassad enterprise-app i Azure Active Directory"
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
ms.openlocfilehash: 3e44e876dcbac704a9809ae5b3957bf94be21c48
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-name-or-logo-of-an-enterprise-app-in-azure-active-directory"></a>Ändra namn på eller av en enterprise-app i Azure Active Directory-logotypen
Det är enkelt att ändra namnet eller logotyp för en anpassad företagsprogram i Azure Active Directory (AD Azure). Du måste ha behörighet att göra dessa ändringar, och du måste vara skapare av anpassad app.

## <a name="how-do-i-change-an-enterprise-apps-name-or-logo"></a>Hur ändrar jag enterprise appens namn eller logotyp?
1. Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för katalogen.
2. Välj **fler tjänster**, ange **Azure Active Directory** i textrutan och välj sedan **RETUR**.
3. På den **Azure Active Directory - *directoryname***  bladet (det vill säga Azure AD bladet för den katalog som du hanterar), Välj **företagsprogram**.

    ![Öppna företagsappar](./media/active-directory-coreapps-change-app-logo-azure-portal/open-enterprise-apps.png)
4. På den **företagsprogram** bladet väljer **alla program**. Visas en lista över appar som du kan hantera.
5. På den **företagsprogram - alla program** bladet väljer du en app.
6. På den ***appname*** bladet (det vill säga bladet med namnet på den valda appen i namnet), Välj **egenskaper**.

    ![Att välja kommandot Egenskaper](./media/active-directory-coreapps-change-app-logo-azure-portal/select-app.png)
7. På den ***appname*** **-egenskaper** bladet, bläddra efter en fil som ska användas som en logotyp som är nya eller redigera namnet på appen, eller båda.

    ![Ändra kommandot app logotyp eller nameproperties](./media/active-directory-coreapps-change-app-logo-azure-portal/change-logo.png)
8. Välj den **spara** kommando.

## <a name="next-steps"></a>Nästa steg
* [Visa alla mina grupper](active-directory-groups-view-azure-portal.md)
* [Tilldela en användare eller grupp till en enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)
* [Ta bort en användare eller grupp från en enterprise-app](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Inaktivera användarinloggningar för en enterprise-app](active-directory-coreapps-disable-app-azure-portal.md)
