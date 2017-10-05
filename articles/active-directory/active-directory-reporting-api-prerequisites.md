---
title: "Förutsättningar för att få åtkomst till Azure AD reporting API. | Microsoft Docs"
description: "Lär dig mer om förutsättningar för att kunna komma åt Azure AD reporting API"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: 6e409fc56b77f37dac7f37382e664c577666ad4d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a>Förutsättningar för att få åtkomst till Azure AD reporting API
Den [Azure AD reporting API: er](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) ger dig programmatisk åtkomst till data via en uppsättning REST-baserad API: er. Du kan anropa API: erna från en mängd olika programmeringsspråk och verktyg.

Reporting API: N används [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) att bevilja åtkomst till webb-API: er. 

För att förbereda din åtkomst till reporting API, måste du:

1. Skapa ett program i Azure AD-klient 
2. Bevilja programmet behörighet att komma åt Azure AD-data
3. Samla in konfigurationsinställningar från katalogen

För frågor, frågor eller kommentarer, kontakta [AAD Reporting hjälper](mailto:aadreportinghelp@microsoft.com).

## <a name="create-an-azure-ad-application"></a>Skapa en Azure AD-program
Om du vill konfigurera din katalog för att komma åt Azure AD reporting API, måste du logga in på den klassiska Azure-portalen med ett administratörskonto för Azure-prenumeration som även är medlem i rollen Global administratör katalog i Azure AD-klienten.

> [!IMPORTANT]
> Program som körs under autentiseringsuppgifterna med privilegier som ”admin” så här kan vara mycket kraftfulla, så kontrollera att det för att skydda programmets ID-hemlighet autentiseringsuppgifter.
> 
> 

1. I den [klassiska Azure-portalen](https://manage.windowsazure.com), klicka på det vänstra navigeringsfönstret **Active Directory**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Från den **active directory** väljer du din katalog.
3. Klicka på menyn högst upp **program**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/02.png) 
4. Klicka på raden längst **Lägg till**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/03.png) 
5. På den **vad vill du göra?** dialogrutan klickar du på **Lägg till ett program som min organisation utvecklar**. 
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/04.png) 
6. På den **berätta om tillämpningsprogrammet** dialogrutan, utför följande steg: 
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/05.png) 
   
    a. I den **namn** textruta, ange ett namn (t.ex.: Reporting API-program).
   
    b. Välj **webbprogram och/eller webb-API**.
   
    c. Klicka på **Nästa**.
7. På den **appegenskaper** dialogrutan, utför följande steg: 
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/06.png) 
   
    a. I den **inloggnings-URL** textruta typen `https://localhost`.
   
    b. I den **App-ID URI** textruta typen ```https://localhost/ReportingApiApp```.
   
    c. Klicka på **Complete** (Slutför).

## <a name="grant-your-application-permission-to-use-the-api"></a>Ge ditt program behörighet att använda API: et
1. I den [klassiska Azure-portalen](https://manage.windowsazure.com/), klicka på det vänstra navigeringsfönstret **Active Directory**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Från den **active directory** väljer du din katalog.
3. Klicka på menyn högst upp **program**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/02.png)
4. Välj ditt nya program i listan med program.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/07.png)
5. Klicka på menyn högst upp **konfigurera**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/08.png)
6. I den **behörigheter för andra program** avsnittet för den **Azure Active Directory** resurs, klicka på den **programbehörigheter** listrutan och välj sedan **läsa katalogdata**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/09.png)
7. Klicka på raden längst **spara**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/10.png)

## <a name="gather-configuration-settings-from-your-directory"></a>Samla in konfigurationsinställningar från katalogen
Det här avsnittet visar hur du får följande inställningar från din katalog:

* Domännamn
* Klient-ID
* Klienthemlighet

Du måste dessa värden när du konfigurerar anrop reporting-API: et. 

### <a name="get-your-domain-name"></a>Hämta ditt domännamn
1. I den [klassiska Azure-portalen](https://manage.windowsazure.com), klicka på det vänstra navigeringsfönstret **Active Directory**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Från den **active directory** väljer du din katalog.
3. Klicka på menyn högst upp **domäner**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/11.png) 
4. I den **domännamn** kolumn, kopiera ditt domännamn.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/12.png) 

### <a name="get-the-applications-client-id"></a>Hämta programmets klient-ID
1. I den [klassiska Azure-portalen](https://manage.windowsazure.com), klicka på det vänstra navigeringsfönstret **Active Directory**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Från den **active directory** väljer du din katalog.
3. Klicka på menyn högst upp **program**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/02.png) 
4. Välj ditt nya program i listan med program.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/07.png)
5. Klicka på menyn högst upp **konfigurera**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/08.png)
6. Kopiera ditt **klient-ID**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/13.png)

### <a name="get-the-applications-client-secret"></a>Hämta programmets klienthemlighet
För att få ditt program klienthemlighet, måste du skapa en ny nyckel och spara dess värde vid sparas den nya nyckeln eftersom det inte går att hämta det här värdet senare längre.

1. I den [klassiska Azure-portalen](https://manage.windowsazure.com), klicka på det vänstra navigeringsfönstret **Active Directory**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Från den **active directory** väljer du din katalog.
3. Klicka på menyn högst upp **program**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/02.png) 
4. Välj ditt nya program i listan med program.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/07.png)
5. Klicka på menyn högst upp **konfigurera**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/08.png)
6. I den **nycklar** avsnittet, utför följande steg: 
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/14.png)
   
    a. Markera en varaktighet från listan varaktighet
   
    b. Klicka på raden längst **spara**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/10.png)
   
    c. Kopiera värdet för nyckeln.

## <a name="next-steps"></a>Nästa steg
* Vill du komma åt data från Azure AD reporting API på ett programmässiga sätt? Checka ut [komma igång med Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).
* Om du vill veta mer om Azure Active Directory reporting finns i [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).  

