---
title: "aaaHow toouse Azure RemoteApp med Office 365-användarkonton | Microsoft Docs"
description: "Lär dig hur toouse Azure RemoteApp med Office 365-användarkonton"
services: remoteapp
documentationcenter: 
author: piotrci
manager: mbaldwin
ms.assetid: aba70fd3-60b0-4b44-9321-1ab18760ccd5
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: d2dbed2a6838adf9bb0f7508eb7dcecb0a74a62f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-remoteapp-with-office-365-user-accounts"></a>Hur toouse Azure RemoteApp med Office 365-användarkonton
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Om du har en Office 365-prenumeration som du har ett Azure Active Directory som lagrar ditt användarnamn och lösenord används tooaccess Office 365-tjänster. Till exempel när dina användare aktiverar Office 365 ProPlus autentiseras de mot Azure AD toocheck för licenser. De flesta kunder skulle som toouse hello samma katalog med Azure RemoteApp.

Om du distribuerar Azure RemoteApp använder du troligen en Azure-prenumeration som är associerad med en annan Azure AD. I ordning toouse din Office 365-katalog, behöver du toomove hello Azure-prenumeration i katalogen.

Information om hur toodeploy Office 365-klientprogram, se [hur toouse din Office 365-prenumeration med Azure RemoteApp](remoteapp-officesubscription.md).

## <a name="phase-1-register-your-free-office-365-azure-active-directory-subscription"></a>Fas 1: Registrera prenumerationen ledigt Office 365 Azure Active Directory
Om du använder hello klassiska Azure-portalen, använda hello stegen i [registrera prenumerationen gratis Azure Active Directory](https://technet.microsoft.com/library/dn832618.aspx) tooget administrativ åtkomst tooyour Azure AD via hello Azure-hanteringsportalen. Hello följd av den här processen bör du vara kan toolog till hello Azure-portalen och finns i din katalog där – nu visas inte mycket mer eftersom hello fullständig Azure-prenumeration som används med Azure RemoteApp är i en annan katalog.

Kom ihåg hello namn och lösenord för administratörskontot för hello som du skapade i det här steget – de behövs i fas 2.

Om du använder hello Azure-portalen, kolla [hur tooregister och aktivera en gratis Azure Active Directory med hjälp av Office 365-portalen](http://azureblogger.com/2016/01/how-to-register-and-activate-a-free-azure-active-directory-using-office-365-portal/).

## <a name="phase-2-change-hello-azure-ad-associated-with-your-azure-subscription"></a>Fas 2: Ändra hello Azure AD som är associerade med din Azure-prenumeration.
Vi toochange din Azure-prenumeration från den aktuella katalogen i hello Office 365-katalog som vi har arbetat med i fas 1.

Följ instruktionerna i hello [ändra hello Azure Active Directory-klient i Azure RemoteApp](remoteapp-changetenant.md). Betala närmare toohello följande steg:

* Steg #1: Om du har distribuerat Azure RemoteApp (ARA) i den här prenumerationen, kontrollera att du tar bort alla användarkonton i Azure AD från några ARA samlingar först innan du försöker någonting annat. Alternativt kan du ta bort några befintliga samlingar.
* Steg #2: Detta är ett viktigt steg. Du behöver toouse ett Microsoft-konto (t.ex. @outlook.com) som tjänstadministratör för prenumerationen hello; detta beror på att vi inte kan ha några användarkonton från hello befintliga Azure AD kopplade toohello prenumeration – om vi gör vi kommer inte att kunna toomove den tooa olika Azure AD.
* Steg #4: När du lägger till en befintlig katalog, hello system ber dig toosign in med hello administratörskonto för katalogen. Se till att toouse hello-administratörskontot från fas 1.
* Steg #5: Ändra hello överordnade katalogen i hello tooyour Office 365-prenumerationskatalog. hello slutresultatet ska vara att under Inställningar -> prenumerationer prenumerationen visar hello Office 365-katalogen. 
  ![Ändra hello överordnade katalogen i hello prenumeration](./media/remoteapp-o365user/settings.png)

Nu är Azure RemoteApp-prenumeration associerad med din Office 365 Azure AD; Du kan använda hello befintliga Office 365-användarkonton med Azure RemoteApp!

