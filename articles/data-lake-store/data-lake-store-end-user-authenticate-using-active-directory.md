---
title: "Slutanvändarens autentisering: Data Lake Store med Azure Active Directory | Microsoft Docs"
description: "Lär dig att uppnå slutanvändarens autentisering med Data Lake Store med Azure Active Directory"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ec586ecd-1b42-459e-b600-fadbb7b80a9b
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: nitinme
ms.openlocfilehash: c20f5c39b00992d801909c8e5de292f3c2f12673
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="end-user-authentication-with-data-lake-store-using-azure-active-directory"></a>Slutanvändarens autentisering med Data Lake Store med Azure Active Directory
> [!div class="op_single_selector"]
> * [Tjänst-till-tjänst-autentisering](data-lake-store-authenticate-using-active-directory.md)
> * [Slutanvändarautentisering](data-lake-store-end-user-authenticate-using-active-directory.md)
> 
> 

Azure Data Lake Store använder Azure Active Directory för autentisering. Innan du skapar ett program som fungerar med Azure Data Lake Store eller Azure Data Lake Analytics kan bestämma du först hur du vill autentisera ditt program med Azure Active Directory (AD Azure). De största tillgängliga alternativ är:

* Slutanvändarens autentisering (den här artikeln)
* Tjänst-till-tjänst-autentisering

Båda dessa alternativ kan resultera i ditt program som tillhandahålls med en OAuth 2.0-token som hämtar kopplade till varje begäran till Azure Data Lake Store eller Azure Data Lake Analytics.

Skapa den här artikeln pratar om hur en **interna Azure AD-program för slutanvändare autentisering**. Information om programkonfigurationen för Azure AD för autentisering för tjänst-till-tjänst finns [tjänst-till-tjänst-autentisering med Data Lake Store med Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Krav
* En Azure-prenumeration. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).

* Ditt prenumerations-ID. Du kan hämta den från Azure Portal. Till exempel är den tillgänglig från bladet Data Lake Store-konto.
  
    ![Hämta prenumerations-ID](./media/data-lake-store-end-user-authenticate-using-active-directory/get-subscription-id.png)

* Din Azure AD-domännamn. Du kan hämta den håller musen i övre högra hörnet i Azure Portal. Från skärmbilden nedan domännamnet är **contoso.onmicrosoft.com**, och GUID inom hakparenteser är klient-ID. 
  
    ![Hämta AAD-domän](./media/data-lake-store-end-user-authenticate-using-active-directory/get-aad-domain.png)

## <a name="end-user-authentication"></a>Slutanvändarautentisering
Det här är den rekommenderade metoden om du vill att en slutanvändare att logga in på ditt program via Azure AD. Programmet kommer att kunna komma åt Azure-resurser med samma nivå av åtkomst som slutanvändare inloggad. Slutanvändaren måste ange sina autentiseringsuppgifter med jämna mellanrum i ordning för programmet att upprätthålla åtkomsten.

Resultatet av att slutanvändaren loggar in är att tillämpningsprogrammet ges en åtkomst-token och en uppdateringstoken. Åtkomsttoken hämtar ansluten till varje begäran till Data Lake Store eller Data Lake Analytics och är giltig i en timme som standard. Uppdateringstoken som kan användas för att hämta en ny åtkomsttoken och det är giltigt för upp till två veckor som standard om användas ofta. Du kan använda två olika metoder för slutanvändaren logga in.

### <a name="using-the-oauth-20-pop-up"></a>Med hjälp av OAuth 2.0-popup-fönster
Programmet kan utlösa en OAuth 2.0 auktorisering popup-fönstret som slutanvändaren kan ange sina autentiseringsuppgifter. Det här popup-fönster fungerar även med Azure AD tvåfaktorsautentisering (2FA)-processen om det behövs. 

> [!NOTE]
> Den här metoden stöds inte ännu i Azure AD Authentication Library (ADAL) för Python eller Java.
> 
> 

### <a name="directly-passing-in-user-credentials"></a>Skicka direkt i användarens autentiseringsuppgifter
Programmet kan direkt ange autentiseringsuppgifter för användare till Azure AD. Den här metoden fungerar bara med organisationens ID användarkonton; Det är inte kompatibelt med personal / ”live ID”-användarkonton, inklusive de som slutar på @outlook.com eller @live.com. Dessutom kan är den här metoden inte kompatibel med användarkonton som kräver Azure AD tvåfaktorsautentisering (2FA).

### <a name="what-do-i-need-to-use-this-approach"></a>Vad behöver jag använda den här metoden?
* Azure AD-domännamn. Det finns redan i komponenten i den här artikeln.
* Azure AD **programspecifika**
* Program-ID för det interna Azure AD-programmet
* Omdirigerings-URI för det interna Azure AD-programmet
* Ange delegerade behörigheter


## <a name="step-1-create-an-active-directory-native-application"></a>Steg 1: Skapa en intern Active Directory-program

Skapa och konfigurera en inbyggd Azure AD-program för slutanvändare autentisering med Azure Data Lake Store med Azure Active Directory. Instruktioner finns i [skapa en Azure AD-program](../azure-resource-manager/resource-group-create-service-principal-portal.md).

Kontrollera att du väljer när du följa anvisningarna i länken ovan **interna** för programtyp, som visas i skärmbilden nedan.

![Skapa webbapp](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "skapa inbyggda appen")

## <a name="step-2-get-application-id-and-redirect-uri"></a>Steg 2: Hämta program-id och omdirigerings-URI

Se [hämta program-ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) att hämta program-id (även kallat klient-ID i den klassiska Azure-portalen) för den ursprungliga Azure AD-programmet.

Följ stegen nedan om du vill hämta omdirigerings-URI.

1. Välj Azure-portalen **Azure Active Directory**, klickar du på **App registreringar**, söka efter och klickar på den interna Azure AD-program som du nyss skapade.

2. Från den **inställningar** bladet för programmet, klickar du på **omdirigerings-URI: er**.

    ![Get omdirigerings-URI](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-redirect-uri.png)

3. Kopiera värdet som visas.


## <a name="step-3-set-permissions"></a>Steg 3: Ange behörigheter

1. Välj Azure-portalen **Azure Active Directory**, klickar du på **App registreringar**, söka efter och klickar på den interna Azure AD-program som du nyss skapade.

2. Från den **inställningar** bladet för programmet, klickar du på **nödvändiga behörigheter**, och klicka sedan på **Lägg till**.

    ![klient-id](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-1.png)

3. I den **lägga till API-åtkomst** bladet, klickar du på **väljer en API**, klickar du på **Azure Data Lake**, och klicka sedan på **Välj**.

    ![klient-id](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-2.png)
 
4.  I den **lägga till API-åtkomst** bladet, klickar du på **Välj behörigheter**, markera kryssrutan för att ge **fullständig åtkomst till Data Lake Store**, och klicka sedan på **Välj**.

    ![klient-id](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-3.png)

    Klicka på **Klar**.

5. Upprepa de två sista stegen för att bevilja behörigheter för **Windows Azure Service Management API** samt.
   
## <a name="next-steps"></a>Nästa steg
I den här artikeln du skapat ett enhetligt Azure AD-program och samlat in den information du behöver i ditt klientprogram att du skapar med hjälp av SDK för .NET, Java SDK, REST-API och så vidare. Du kan nu fortsätta i följande artiklar som talar om hur du använder Azure AD-webbprogram att först autentisera med Data Lake Store och sedan utföra andra åtgärder i store.

* [Kom igång med Azure Data Lake Store med hjälp av .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Kom igång med Azure Data Lake Store med hjälp av Java SDK](data-lake-store-get-started-java-sdk.md)
* [Kom igång med Azure Data Lake Store med hjälp av REST API](data-lake-store-get-started-rest-api.md)

