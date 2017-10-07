---
title: "krav för aaaAzure Data Catalog | Microsoft Docs"
description: "Läs mer om hello krav som du behöver tooget igång med Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: ef497a54-dc4d-4820-b5bf-c361b64b964d
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 0c8e768e5846c61b542b746d7ad80121725a9ec7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-catalog-prerequisites"></a>Förutsättningar för Azure Data Catalog

Du måste tootake vård några saker innan du kan ställa in Azure Data Catalog. Oroa dig inte, den här processen tar inte lång tid.

## <a name="azure-subscription"></a>Azure-prenumeration
tooset upp Data Catalog, måste du vara hello ägare eller Medägare för Azure-prenumeration.

Azure-prenumerationer kan du organisera toocloud-tjänst komma åt resurser, till exempel Data Catalog. Prenumerationer hjälpa dig att styra hur Resursanvändning rapporterade, debiteras och betalat för. Varje prenumeration kan ha separata fakturerings- och inställningar, så du kan ha prenumerationer och scheman som varierar beroende på avdelning, projekt, lokalkontor och så vidare. Varje tjänst i molnet tillhör tooa prenumeration och du behöver toohave en prenumeration innan du konfigurerar Data Catalog. Det finns fler toolearn [hantera konton, prenumerationer och administrativa roller](../active-directory/active-directory-assign-admin-roles.md).

## <a name="azure-active-directory"></a>Azure Active Directory
tooset upp Data Catalog, måste du vara inloggad med ett användarkonto i Azure Active Directory (AD Azure).

Azure AD innehåller ett enkelt sätt för ditt företag toomanage identitets- och, både i hello molnet och lokalt. Användarna kan använda en enda arbets- eller skolkonto för enkel inloggning tooany molnet och lokala webbprogram. Data Catalog använder Azure AD tooauthenticate inloggning. Det finns fler toolearn [vad är Azure Active Directory?](../active-directory/active-directory-whatis.md).

> [!NOTE]
> Med hjälp av hello [Azure-portalen](http://portal.azure.com/), du kan logga in med ett personligt microsoftkonto eller ett Azure Active Directory arbets- eller skolkonto. tooset upp Data Catalog genom att använda antingen hello Azure-portalen eller hello [Data Catalog-portalen](http://www.azuredatacatalog.com), du måste logga in med ett Azure Active Directory-konto, inte ett personligt konto.
>
>

## <a name="active-directory-policy-configuration"></a>Active Directory-principkonfiguration
Du kan hända att du kan logga in toohello Data Catalog-portalen, men när du försöker toosign i toohello registreringsverktyget för datakällor, du får ett felmeddelande som hindrar dig från att logga in. Det här problemet kan inträffa när du är på hello företagets nätverk eller uppstå endast när du kopplar från hello utanför företagets nätverk.

hello registreringsverktyget för datakällor använder formulärautentisering toovalidate användarautentiseringsuppgifter mot Active Directory. toohelp som du loggar in har ett Active Directory-administratören måste aktivera formulärautentisering i hello Global autentiseringsprincip.

I hello Global autentiseringsprincip vara autentiseringsmetoder aktiverade separat för intranät och extranät-anslutningar som visas i följande skärmbild hello. Logga in fel kan inträffa om formulär för autentisering inte har aktiverats för hello-nätverk som du vill ansluta.

 ![Globala autentiseringsprincipen i Active Directory](./media/data-catalog-prerequisites/global-auth-policy.png)

## <a name="next-steps"></a>Nästa steg
Mer information finns i [konfigurera autentiseringsprinciper](https://technet.microsoft.com/library/dn486781.aspx).
