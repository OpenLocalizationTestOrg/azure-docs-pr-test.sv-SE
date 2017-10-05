---
title: Skapa en ny enhet med Azure AD under installationen | Microsoft Docs
description: "Ett avsnitt som förklarar hur användare kan konfigurera Azure AD Join under deras välkomstprogrammet."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 4367f453ef7c794653dfa9f305b137a168e0d207
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="set-up-a-new-device-with-azure-ad-during-setup"></a>Skapa en ny enhet med Azure AD under installationen
I Windows 10 ansluta användare sina enheter till Azure Active Directory (AD Azure) i den första körningen (FRX). Detta gör att organisationer kan distribuera form av krymp enheter till sina anställda och studenter eller låta dem välja sina egna enheter (CYOD).
Om Windows 10 Professional eller Windows 10 Enterprise-utgåvor installeras på en enhet, standard upplevelsen installationsprocessen för företagsägda enheter.

## <a name="to-join-a-device-to-azure-ad"></a>Att ansluta till en enhet till Azure AD
1. När du aktiverar den nya enheten och starta installationen, bör du se den **komma redo** meddelande. Följ anvisningarna för att konfigurera din enhet.
2. Starta genom att anpassa din region och språk. Acceptera licensvillkoren för programvara från Microsoft.
   ![Anpassa för din region](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)
3. Markera det nätverk som du vill använda för att ansluta till Internet.
4. Välj om du använder en personlig enhet eller en företagsägd enhet. Om det ägs av företaget, klickar du på **den här enheten tillhör mitt företag**. Detta startar Azure AD Join-upplevelsen. Följande är en skärm som visas om du använder Windows 10 Professional.
   <center>
   ![Vem äger den här datorn skärmen](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)
5. Ange de autentiseringsuppgifter som har fått av din organisation.
   <center>
   ![Inloggningssida](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)
6. När du har angett ditt användarnamn, finns en matchande klient i Azure AD. Om du är i en federerad domän, omdirigeras du till servern lokalt Secure säkerhetstokentjänst (STS) – till exempel Active Directory Federation Services (AD FS).
7. Ange dina autentiseringsuppgifter direkt på Azure AD-värdbaserad sidan om du är en användare i en ofedererad domän. Om företagsanpassning har konfigurerats kommer du också se organisationens logotyp och stöder text.
8. Du ombeds ange en utmaning för multifaktorautentisering. Denna utmaning kan konfigureras av en IT-administratör.
9. Azure AD kontrollerar om den här användarenhet kräver registrering i hantering av mobila enheter.
10. Windows registrerar enheten i organisationens katalog i Azure AD och registrerar den i hantering av mobila enheter.
11. Om du är en användare går du till skrivbordet genom processen för automatisk inloggning i Windows.
12. Om du är en federerad användare dirigeras du till skärmen för Windows att ange dina autentiseringsuppgifter.

> [!NOTE]
> Ansluter till en lokal Windows Server Active Directory-domän i Windows out of box experience stöds inte. Därför, om du planerar att ansluta en dator till en domän, ska du markera länken **konfigurera Windows med ett lokalt konto** i stället. Du kan ansluta till domänen från inställningarna på datorn som du har gjort förut.
> 
> 

## <a name="additional-information"></a>Ytterligare information
* [Windows 10 för företaget: Sätt att använda enheter för arbete](active-directory-azureadjoin-windows10-devices-overview.md)
* [Utöka molnkapaciteten till Windows 10-enheter via Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Autentisera identiteter utan lösenord via Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Läs mer om användningsscenarier för Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ansluta domänanslutna enheter till miljöer med Azure AD och Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurera Azure AD Join](active-directory-azureadjoin-setup.md)

