---
title: aaaPrerequisites tooaccess hello Azure AD reporting API. | Microsoft Docs
description: "Lär dig mer om hello krav tooaccess hello Azure AD reporting API"
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
ms.openlocfilehash: e9d7ceaedb07d18fbd75b70d68b5cfbebc756c36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="prerequisites-tooaccess-hello-azure-ad-reporting-api"></a>Förutsättningar tooaccess hello Azure AD reporting API
Hej [Azure AD reporting API: er](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) ger dig programmatisk åtkomst toohello data via en uppsättning REST-baserad API: er. Du kan anropa API: erna från en mängd olika programmeringsspråk och verktyg.

hello reporting API: N används [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) tooauthorize åtkomst toohello web API: er. 

tooprepare din åtkomst toohello reporting API, måste du:

1. Skapa ett program i Azure AD-klient 
2. Bevilja hello program behörighet tooaccess hello Azure AD-data
3. Samla in konfigurationsinställningar från katalogen

För frågor, frågor eller kommentarer, kontakta [AAD Reporting hjälper](mailto:aadreportinghelp@microsoft.com).

## <a name="create-an-azure-ad-application"></a>Skapa en Azure AD-program
tooconfigure din katalog tooaccess hello Azure AD reporting API, måste du logga in toohello klassiska Azure-portalen med ett administratörskonto för Azure-prenumeration som även är medlem i rollen hello Global administratör katalog i Azure AD-klienten.

> [!IMPORTANT]
> Program som körs under autentiseringsuppgifterna med privilegier som ”admin” så här kan vara mycket kraftfulla, så du vara säker på att tookeep hello programmets ID-hemlighet autentiseringsuppgifter säker.
> 
> 

1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com), på hello vänstra navigeringsfönstret, klicka på **Active Directory**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Från hello **active directory** väljer du din katalog.
3. Hello-menyn överst hello **program**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/02.png) 
4. Klicka på hello nedre **Lägg till**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/03.png) 
5. På hello **vad vill du vill toodo?** dialogrutan klickar du på **Lägg till ett program som min organisation utvecklar**. 
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/04.png) 
6. På hello **berätta om tillämpningsprogrammet** dialogrutan utföra hello följande steg: 
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/05.png) 
   
    a. I hello **namn** textruta, ange ett namn (t.ex.: Reporting API-program).
   
    b. Välj **webbprogram och/eller webb-API**.
   
    c. Klicka på **Nästa**.
7. På hello **appegenskaper** dialogrutan utföra hello följande steg: 
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/06.png) 
   
    a. I hello **inloggnings-URL** textruta typen `https://localhost`.
   
    b. I hello **App-ID URI** textruta typen ```https://localhost/ReportingApiApp```.
   
    c. Klicka på **Complete** (Slutför).

## <a name="grant-your-application-permission-toouse-hello-api"></a>Ge ditt program behörighet toouse hello API
1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com/), på hello vänstra navigeringsfönstret, klicka på **Active Directory**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Från hello **active directory** väljer du din katalog.
3. Hello-menyn överst hello **program**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/02.png)
4. Välj ditt nya program i listan med program hello.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/07.png)
5. Hello-menyn överst hello **konfigurera**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/08.png)
6. I hello **behörigheter tooother program** för hello **Azure Active Directory** resurs, klicka på hello **programbehörigheter** listrutan, och sedan Välj **läsa katalogdata**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/09.png)
7. Klicka på hello nedre **spara**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/10.png)

## <a name="gather-configuration-settings-from-your-directory"></a>Samla in konfigurationsinställningar från katalogen
Det här avsnittet beskrivs hur du tooget hello följande inställningar från din katalog:

* Domännamn
* Klient-ID
* Klienthemlighet

Du måste dessa värden när du konfigurerar anrop toohello reporting API. 

### <a name="get-your-domain-name"></a>Hämta ditt domännamn
1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com), på hello vänstra navigeringsfönstret, klicka på **Active Directory**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Från hello **active directory** väljer du din katalog.
3. Hello-menyn överst hello **domäner**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/11.png) 
4. I hello **domännamn** kolumn, kopiera ditt domännamn.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/12.png) 

### <a name="get-hello-applications-client-id"></a>Hämta hello programmets klient-ID
1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com), på hello vänstra navigeringsfönstret, klicka på **Active Directory**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Från hello **active directory** väljer du din katalog.
3. Hello-menyn överst hello **program**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/02.png) 
4. Välj ditt nya program i listan med program hello.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/07.png)
5. Hello-menyn överst hello **konfigurera**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/08.png)
6. Kopiera ditt **klient-ID**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/13.png)

### <a name="get-hello-applications-client-secret"></a>Skaffa hello programmet klienthemlighet
tooget programmets klienten hemliga, du behöver toocreate en ny nyckel och spara dess värde vid spara hello ny nyckel eftersom det inte är möjligt tooretrieve det här värdet senare längre.

1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com), på hello vänstra navigeringsfönstret, klicka på **Active Directory**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Från hello **active directory** väljer du din katalog.
3. Hello-menyn överst hello **program**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/02.png) 
4. Välj ditt nya program i listan med program hello.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/07.png)
5. Hello-menyn överst hello **konfigurera**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/08.png)
6. I hello **nycklar** avsnittet, utföra hello följande steg: 
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/14.png)
   
    a. Välj en varaktighet hello varaktighet listan
   
    b. Klicka på hello nedre **spara**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/10.png)
   
    c. Kopiera hello nyckel/värde.

## <a name="next-steps"></a>Nästa steg
* Skulle du som tooaccess hello data från hello Azure AD reporting API på ett programmässiga sätt? Checka ut [komma igång med hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).
* Om du vill toofind mer information om Azure Active Directory reporting finns hello [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).  

