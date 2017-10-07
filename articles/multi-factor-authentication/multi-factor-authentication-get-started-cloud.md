---
title: "aaaGet igång Azure MFA i molnet hello | Microsoft Docs"
description: "Det här är hello Microsoft Azure Multi-Factor authentication sida som beskriver hur tooget igång med Azure MFA i molnet hello."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 6b2e6549-1a26-4666-9c4a-cbe5d64c4e66
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/24/2017
ms.author: kgremban
ms.openlocfilehash: a4aaf44bf08d96f2baad155072fdd6e0e727ce8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-multi-factor-authentication-in-hello-cloud"></a>Komma igång med Azure Multi-Factor Authentication i molnet hello
Den här artikeln beskrivs hur tooget igång med Azure Multi-Factor Authentication i molnet hello.

> [!NOTE]
> hello följande dokumentation innehåller information om hur tooenable användare som använder hello **klassiska Azure-portalen**. Om du letar efter information om hur tooset in Azure Multi-Factor Authentication för O365-användare kan se [ställer in multifaktorautentisering för Office 365.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)

![MFA i hello moln](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## <a name="prerequisite"></a>Krav
[Registrera dig för en Azure-prenumeration](https://azure.microsoft.com/pricing/free-trial/) -om du inte redan har en Azure-prenumeration behöver du toosign upp för en. Om du precis börjat använda Azure MFA kan du använda en utvärderingsprenumeration.

## <a name="enable-azure-multi-factor-authentication"></a>Aktivera Azure Multi-Factor Authentication
Så länge som användarna har licenser som innehåller Azure Multi-Factor Authentication, finns det inget att du behöver toodo tooturn på Azure MFA. Du kan börja med att kräva tvåstegsverifiering för enskilda användare. hello licenser som gör att Azure MFA är:
- Azure Multi-Factor Authentication
- Azure Active Directory Premium
- Enterprise Mobility + Security

Om du inte har någon av dessa tre licenser, eller om du inte har tillräckligt med licenser toocover alla användare, det är ok för. Du precis har toodo ett extra steg och [skapa en leverantör av Multifaktorautent](multi-factor-authentication-get-started-auth-provider.md) i din katalog.

## <a name="turn-on-two-step-verification-for-users"></a>Aktivera tvåstegsverifiering för användare

Använd någon av hello procedurer som anges i [hur toorequire tvåstegsverifiering för en användare eller grupp](multi-factor-authentication-get-started-user-states.md) toostart med hjälp av Azure MFA. Du kan välja tooenforce tvåstegsverifiering för alla inloggningar eller du kan skapa tvåstegsverifiering för villkorlig åtkomst principer toorequire endast när det gäller tooyou.

## <a name="next-steps"></a>Nästa steg
Nu när du har konfigurerat Azure Multi-Factor Authentication i molnet hello, konfigurerar och konfigurera din distribution. Se [Konfigurera Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md) för mer information.

