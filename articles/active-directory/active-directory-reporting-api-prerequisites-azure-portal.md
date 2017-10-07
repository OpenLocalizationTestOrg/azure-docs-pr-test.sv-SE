---
title: aaaPrerequisites tooaccess hello Azure AD reporting API | Microsoft Docs
description: "Lär dig mer om hello krav tooaccess hello Azure AD reporting API"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: ec28a7530f341dda31268a978754b615c727d66f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="prerequisites-tooaccess-hello-azure-ad-reporting-api"></a>Förutsättningar tooaccess hello Azure AD reporting API

Hej [Azure AD reporting API: er](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) ger dig programmatisk åtkomst toohello data via en uppsättning REST-baserad API: er. Du kan anropa API: erna från en mängd olika programmeringsspråk och verktyg.

hello reporting API: N används [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) tooauthorize åtkomst toohello web API: er. 

tooget dataåtkomst toohello reporting via hello API måste du toohave en hello följande roller:

- Säkerhet läsare
- Säkerhet Admin
- Global administratör


tooprepare din åtkomst toohello reporting API, måste du:

1. Registrera ett program 
2. Bevilja behörighet 
3. Samla in konfigurationsinställningar 

Frågor, frågor eller kommentarer finns [filen ett supportärende](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto).

## <a name="register-an-azure-active-directory-application"></a>Registrera ett Azure Active Directory-program

Tooregister en app måste även om du ansluter till hello reporting API: et med ett skript. Detta ger dig en **program-ID**, vilket krävs för ett anrop för auktorisering och den gör att din kod tooreceive token.

tooconfigure din katalog tooaccess hello Azure AD reporting API, måste du logga in toohello Azure-portalen med Azure-administratörskontot som även är medlem i hello **Global administratör** directory roll i Azure AD-klient .

> [!IMPORTANT]
> Program som körs under autentiseringsuppgifterna med privilegier som ”admin” så här kan vara mycket kraftfulla, så du vara säker på att tookeep hello programmets ID-hemlighet autentiseringsuppgifter säker.
> 


**tooregister ett Azure Active Directory-program:**

1. I hello [Azure-portalen](https://portal.azure.com), på hello vänstra navigeringsfönstret, klicka på **Active Directory**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. På hello **Azure Active Directory** bladet, klickar du på **App registreringar**.

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/02.png) 

3. På hello **App registreringar** bladet i hello verktygsfältet hello överst, klickar du på **nya appregistrering**.

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/03.png)

4. På hello **skapa** bladet utföra hello följande steg:

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/04.png)

    a. I hello **namn** textruta typen `Reporting API application`.

    b. Som **programtyp**väljer **webbapp / API**.

    c. I hello **inloggnings-URL** textruta typen `https://localhost`.

    d. Klicka på **Skapa**. 


## <a name="grant-permissions"></a>Bevilja behörighet 

hello syftet med det här steget är toogrant programmet **läsa katalogdata** behörigheter toohello **Windows Azure Active Directory** API.

![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/16.png)
 

**toogrant ditt program behörighet toouse hello API:**

1. På hello **App registreringar** klickar du på bladet i hello applista **Reporting API-program**.

2. På hello **Reporting API-program** bladet i hello verktygsfältet hello överst, klickar du på **inställningar**. 

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

3. På hello **inställningar** bladet, klickar du på **nödvändiga behörigheter**. 

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/06.png)

4. På hello **nödvändiga behörigheter** bladet i hello **API** klickar du på **Windows Azure Active Directory**. 

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/07.png)

5. På hello **Aktivera åtkomst** bladet väljer **läsa katalogdata**. 

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/08.png)

6. Klicka i hello verktygsfältet hello längst upp **spara**.

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/15.png)

## <a name="gather-configuration-settings"></a>Samla in konfigurationsinställningar 
Det här avsnittet beskrivs hur du tooget hello följande inställningar från din katalog:

* Domännamn
* Klient-ID
* Klienthemlighet

Du måste dessa värden när du konfigurerar anrop toohello reporting API. 

### <a name="get-your-domain-name"></a>Hämta ditt domännamn

**tooget ditt domännamn:**

1. I hello [Azure-portalen](https://portal.azure.com), på hello vänstra navigeringsfönstret, klicka på **Active Directory**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. På hello **Azure Active Directory** bladet, klickar du på **domännamn**.

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/09.png) 

3. Kopiera ditt domännamn hello listan över domäner.


### <a name="get-your-applications-client-id"></a>Hämta programmets klient-ID

**tooget programmets klient-ID:**

1. I hello [Azure-portalen](https://portal.azure.com), på hello vänstra navigeringsfönstret, klicka på **Active Directory**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. På hello **App registreringar** klickar du på bladet i hello applista **Reporting API-program**.

3. På hello **Reporting API-program** bladet på hello **program-ID**, klickar du på **klickar du på toocopy**.

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/11.png) 



### <a name="get-your-applications-client-secret"></a>Hämta programmets klienthemlighet
tooget programmets klienten hemliga, du behöver toocreate en ny nyckel och spara dess värde vid spara hello ny nyckel eftersom det inte är möjligt tooretrieve det här värdet senare längre.

**tooget klienthemlighet för ditt program:**

1. I hello [Azure-portalen](https://portal.azure.com), på hello vänstra navigeringsfönstret, klicka på **Active Directory**.
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. På hello **App registreringar** klickar du på bladet i hello applista **Reporting API-program**.


3. På hello **Reporting API-program** bladet i hello verktygsfältet hello överst, klickar du på **inställningar**. 

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

4. På hello **inställningar** bladet i hello **APIR åtkomst** klickar du på **nycklar**. 

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/12.png)


5. På hello **nycklar** bladet utföra hello följande steg:

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/14.png)

    a. I hello **beskrivning** textruta typen `Reporting API`.

    b. Som **Expires**väljer **i två år**.

    c. Klicka på **Spara**.

    d. Kopiera hello nyckel/värde.


## <a name="next-steps"></a>Nästa steg
* Skulle du som tooaccess hello data från hello Azure AD reporting API på ett programmässiga sätt? Checka ut [komma igång med hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).
* Om du vill toofind mer information om Azure Active Directory reporting finns hello [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).  

