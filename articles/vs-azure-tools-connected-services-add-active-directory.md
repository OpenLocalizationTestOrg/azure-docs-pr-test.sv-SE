---
title: "aaaAdding ett Azure Active Directory med anslutna tjänster i Visual Studio | Microsoft Docs"
description: "Lägg till ett Azure Active Directory med hjälp av dialogrutan för hello Visual Studio Lägg till anslutna tjänster"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: f599de6b-e369-436f-9cdc-48a0165684cb
ms.service: active-directory
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/01/2017
ms.author: kraigb
ms.openlocfilehash: 26c8f68edf9ec5f7bf65cbab34e4f9b4085ed18d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Lägga till ett Azure Active Directory med anslutna tjänster i Visual Studio
Med hjälp av Azure Active Directory (Azure AD) kan stöder du enkel inloggning (SSO) för ASP.NET MVC-webbprogram och Active Directory-autentisering i Web API-tjänster. Användarna kan använda sina konton från Azure Active Directory tooconnect tooyour webbprogram med Azure Active Directory-autentisering. hello fördelarna med Azure Active Directory-autentisering med Web API är förbättrad datasäkerhet när exponera en API från ett webbprogram. Med Azure AD har du inte toomanage en separat autentiseringssystemet med sin egen hantering av kontot och användare.

## <a name="prerequisites"></a>Krav
- Azure-konto – om du inte har ett Azure-konto kan du [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) eller [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

### <a name="connect-tooazure-active-directory-using-hello-connected-services-dialog"></a>Ansluta tooAzure Active Directory med hello anslutna tjänster dialogrutan
1. Skapa eller öppna ett ASP.NET MVC-projekt eller ett ASP.NET Web API-projekt i Visual Studio.

1. Från hello Solution Explorer, högerklicka på hello **anslutna tjänster** nod, och hello snabbmenyn, Välj **Lägg till anslutna tjänster**.

1. På hello **anslutna tjänster** väljer **autentisering med Azure Active Directory**.
   
    ![Anslutna Tjänstesida](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)

1. På hello **introduktion** sidan hello **konfigurera Azure AD Authentication** väljer **nästa**.
   
    ![Introduktionssida](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)

1. På hello **enkel inloggning på** sidan hello **konfigurera Azure AD Authentication** guiden, Välj en domän från hello **domän** listrutan. hello listan över domäner innehåller alla domäner som är tillgängliga med hello kontona i hello kontoinställningar dialogrutan. Alternativt kan du ange ett domännamn om du inte hittar hello något du söker efter, t.ex `mydomain.onmicrosoft.com`. Du kan välja hello alternativet toocreate en app i Azure Active Directory eller använda hello inställningar från en befintlig app i Azure Active Directory. Välj **nästa** när du är klar.
   
    ![Enkel inloggning på sidan](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)

1. På hello **katalogåtkomst** sidan hello **konfigurera Azure AD Authentication** guiden, se till att hello **läsa katalogdata** alternativet är markerat. 
   
    ![Åtkomst katalogsidan](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)

1. Välj **Slutför** tooadd hello nödvändig konfiguration kod och referenser tooenable ditt projekt för Azure AD-autentisering. Du kan se hello Active Directory-domän på hello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Visual Studio visas en [vad hände](#how-your-project-is-modified) artikel tooshow du hur projektet har ändrats. Om du vill toocheck som allt arbete, öppna en av hello ändrat konfigurationsfiler och kontrollera att hello-inställningar som anges i hello artikeln är det. 

## <a name="how-your-project-is-modified"></a>Hur projektet har ändrats
När du kör guiden hello Visual Studio lägger till Azure Active Directory och associerade referenser tooyour projekt. Konfigurationsfiler och kodfiler i projektet är också ändrade tooadd stöd för Azure AD. hello specifika ändringar som Visual Studio gör beror på hello-projekt. Detaljerad information om hur ASP.NET MVC-projekt har ändrats, se [vilka happened – MVC projekt](http://go.microsoft.com/fwlink/p/?LinkID=513809). Web API-projekt, se [vad hände – Web API-projekt](http://go.microsoft.com/fwlink/p/?LinkId=513810).

## <a name="next-steps"></a>Nästa steg
* [MSDN-Forum för Azure Active Directory](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)
* [Dokumentationen för Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
* [Blogginlägg: Introduktion tooAzure Active Directory](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)

