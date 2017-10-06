---
title: aaaHow tooLink en Azure-prenumeration tooAzure AD B2C | Microsoft Docs
description: "Stegvisa instruktioner för tooenable fakturering för Azure AD B2C-klient till en Azure-prenumeration."
services: active-directory-b2c
documentationcenter: dev-center-name
author: rojasja
manager: mbaldwin
ms.service: active-directory-b2c
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/05/2016
ms.author: joroja
ms.openlocfilehash: 07b2ed5f7f5f543c9cbb8e9a35107462448e9440
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="linking-an-azure-subscription-tooan-azure-b2c-tenant-toopay-for-usage-charges"></a>Länka ett Azure-prenumeration tooan Azure B2C-klient toopay för avgifter för användning

Pågående kostnader för Azure Active Directory B2C (eller Azure AD B2C) är fakturerade tooan Azure-prenumeration. Det är nödvändigt för hello klient administratören tooexplicitly länk hello Azure AD B2C-klient tooan Azure-prenumeration när du har skapat hello B2C-klient sig själv.  Den här länken uppnås genom att skapa en Azure AD ”B2C-klient” resurs i mål-hello Azure-prenumeration. Många B2C-klienter kan vara länkad tooa enda Azure-prenumeration tillsammans med andra Azure-resurser (till exempel virtuella datorer, lagring av Data, LogicApps)


> [!IMPORTANT]
> Hej senaste informationen om användning fakturering och priser för B2C är på hello följande sida: [Azure AD B2C-priser](
https://azure.microsoft.com/pricing/details/active-directory-b2c/)

## <a name="step-1---create-an-azure-ad-b2c-tenant"></a>Steg 1 – skapa en Azure AD B2C-klient
Skapa en B2C-klient måste slutföras först. Hoppa över det här steget om du redan har skapat ditt mål B2C-klient. [Kom igång med Azure AD B2C](active-directory-b2c-get-started.md)

## <a name="step-2---open-azure-portal-in-hello-azure-ad-tenant-that-shows-your-azure-subscription"></a>Steg 2 – öppna Azure-portalen i hello Azure AD-klient som visar din Azure-prenumeration
Navigera toohello [Azure-portalen](https://portal.azure.com). Växla toohello Azure AD-klient som visar hello Azure-prenumeration som toouse. Azure AD-klient skiljer sig från hello B2C-klient. Klicka på hello kontonamn på hello övre högra hörnet av hello instrumentpanelen tooselect hello Azure AD-klient i hello Azure-portalen. En Azure-prenumeration är nödvändiga tooproceed. [Hämta en Azure-prenumeration](https://account.windowsazure.com/signup?showCatalog=True)

![Växla tooyour Azure AD-klient](./media/active-directory-b2c-how-to-enable-billing/SelectAzureADTenant.png)

## <a name="step-3---create-a-b2c-tenant-resource-in-azure-marketplace"></a>Steg 3 – skapa en B2C-klient-resurs i Azure Marketplace
Öppna Marketplace genom hello Marketplace ikonen eller välja hello grön ”+” i hello övre vänstra hörnet av hello instrumentpanelen.  Sök efter och välj Azure Active Directory B2C. Välj Skapa.

![Välj Marketplace](./media/active-directory-b2c-how-to-enable-billing/marketplace.png)

![Sök AD B2C](./media/active-directory-b2c-how-to-enable-billing/searchb2c.png)

hello Azure AD B2C resurs skapa dialogrutan omfattar hello följande parametrar:

1. Azure AD B2C-klient – Välj en Azure AD B2C-klient hello listrutan.  Visa endast berättigade Azure AD B2C-klienter.  Berättigad B2C hyresgäster uppfyller dessa villkor: du är hello global administratör för hello B2C-klienten och hello B2C-klienten inte är för närvarande associerad tooan Azure-prenumeration

2. Azure AD B2C resursnamnet - är förvalda toomatch hello domännamn hello B2C-klient

3. Prenumeration – en aktiv Azure-prenumeration som du är administratör eller en medadministratör.  Flera innehavare av Azure AD B2C kan läggas till tooone Azure-prenumeration

4. Resursgrupp och resursgruppen plats - med hjälp av den här artefakten kan du ordna flera Azure-resurser.  Det här alternativet har ingen inverkan på din B2C-klient plats, prestanda eller fakturering status

5. Fäst toodashboard för enklaste åtkomst tooyour B2C-klient fakturering information och hello B2C-klient inställningar ![skapa B2C-resurs](./media/active-directory-b2c-how-to-enable-billing/createresourceb2c.png)

## <a name="step-4---manage-your-b2c-tenant-resources-optional"></a>Steg 4 – hantera B2C-klient-resurser (valfritt)
När installationen är klar får en ny resurs ”B2C-klient” skapas i hello målresursgruppen och relaterade Azure-prenumeration.  Du bör se en ny resurs av typen ”B2C-klient” lagt till tillsammans med andra Azure-resurser.

![Skapa resurs för B2C](./media/active-directory-b2c-how-to-enable-billing/b2cresourcedashboard.png)

Genom att klicka på hello B2C-klient resurs, kan du
- Klicka på namnet prenumeration tooreview faktureringsinformation. Se fakturerings- och användning.
- Klicka på Azure AD B2C inställningar tooopen en ny webbläsarflik direkt i tooyour B2C-klient inställningsbladet
- Skicka en supportförfrågan
- Flytta din B2C-klient resurs tooanother Azure-prenumeration eller tooanother resursgruppen.  Det här valet ändringar som Azure-prenumeration tar emot avgifter för användning.

![B2C resursinställningar](./media/active-directory-b2c-how-to-enable-billing/b2cresourcesettings.png)

## <a name="known-issues"></a>Kända problem
- Borttagning av B2C-klient. Om en B2C-klient skapas, tas bort och återskapas med hello samma domännamn, du också ta bort och återskapa hello ”länka” resursen med hello samma domännamn.  Du hittar detta ”länka” resurs under ”alla resurser” i hello prenumeration klient via hello Azure-portalen.
- Automatisk införas begränsningar för regional resource-plats.  I sällsynta fall kan har en användare upprättat en regional begränsning för att skapa en Azure-resurs.  Den här begränsningen kan hindra hello skapandet av hello länken mellan en Azure-prenumeration och en B2C-klient. toomitigate, du slappna av den här begränsningen.

## <a name="next-steps"></a>Nästa steg
När de här stegen är klar för varje B2C-klienter kan debiteras din Azure-prenumeration i enlighet med dina uppgifter för Azure direkt eller Enterprise-avtal.
- Granska användnings- och fakturering inom din valda Azure-prenumeration
- Granska detaljerad dag för dag användningsrapporter med hello [användning Reporting API](active-directory-b2c-reference-usage-reporting-api.md)
