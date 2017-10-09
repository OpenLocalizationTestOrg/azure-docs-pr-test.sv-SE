---
title: "Felsöka: ”Active Directory-objekt är saknas eller är inte tillgänglig | Microsoft Docs"
description: "Vilka toodo när menyalternativet för Active Directory inte visas i hello Azure-hanteringsportalen."
services: active-directory
documentationcenter: na
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 3383020d-6397-43ea-b7aa-c6a9d6a1e3df
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: bryanla
ms.openlocfilehash: d7355a4e39141f0b09272dc5615c309b23c8c70f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-active-directory-item-is-missing-or-not-available"></a>Felsöka: ”Active Directory-objekt är saknas eller är inte tillgänglig
Många av hello anvisningar för användning av Azure Active Directory-funktioner och tjänster som börjar med ”gå toohello Azure-hanteringsportalen och klicka på **Active Directory**”. Men vad gör du om hello Active Directory-tillägget eller menyobjektet inte visas eller om den är markerad **saknas**? Det här avsnittet är utformad toohelp. Beskriver hello villkor under vilka **Active Directory** visas inte eller är inte tillgängligt och förklarar hur tooproceed.

## <a name="active-directory-is-missing"></a>Active Directory saknas
Normalt en **Active Directory** objektet visas i hello vänstra navigeringsmenyn. hello-instruktioner i Azure Active Directory procedurer förutsätter att det här objektet är i vyn.

![Skärmbild som visar: Active Directory i Azure](./media/active-directory-troubleshooting/typical-view.png)

hello Active Directory-objektet visas i hello vänstra navigeringsmenyn när någon av hello följande villkor är sant. Annars visas inte hello-objektet.

* hello aktuell användare har loggat in med ett Microsoft-konto (tidigare känt som Windows Live ID).
  
    ELLER
* hello Azure-klient har en katalog och hello aktuella kontot är en directory-administratör.
  
    ELLER
* hello Azure-klient har minst en Azure AD-åtkomstkontroll (ACS) namnområde. Mer information finns i [Access Control Namespace](https://msdn.microsoft.com/library/azure/gg185908.aspx).
  
    ELLER
* hello Azure-klient har minst en provider för Azure Multi-Factor Authentication. Mer information finns i [administrera Azure Multi-Factor Authentication-Providers](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

toocreate ett namnområde för åtkomstkontroll eller en Multi-Factor Authentication-leverantör, klickar du på **+ ny** > **Apptjänster** > **Active Directory**.

tooget administratörsbehörighet tooa directory har en administratör tilldela administratör rollen tooyour konto. Mer information finns i [Tilldela administratörsroller](active-directory-assign-admin-roles.md).

## <a name="active-directory-is-not-available"></a>Active Directory är inte tillgängligt
När du klickar på **+ ny** > **Apptjänster**, en **Active Directory** objektet visas. Mer specifikt visas hello Active Directory-objekt när någon av hello Active Directory-funktioner, till exempel katalog, Access Control eller leverantör av Multifaktorautent, är tillgängliga toohello aktuella användaren.

Dock medan hello sidan läses hello-objektet är nedtonad och markeras **inte tillgänglig**. Detta är ett tillfälligt tillstånd. Om du vänta några sekunder blir hello objektet tillgänglig. Om hello fördröjning förlängs löser uppdaterar hello webbsida ofta hello problem.

![Skärmbild som visar: Active Directory är inte tillgängligt](./media/active-directory-troubleshooting/not-available.png)

