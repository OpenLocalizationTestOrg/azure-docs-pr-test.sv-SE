---
title: "Slutanvändarens autentisering: Data Lake Store med Azure Active Directory | Microsoft Docs"
description: "Lär dig hur tooachieve slutanvändarens autentisering med Data Lake Store med Azure Active Directory"
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
ms.openlocfilehash: fd58f4f2d8fc915b8bc51d9e5b040d2cee34047e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="end-user-authentication-with-data-lake-store-using-azure-active-directory"></a>Slutanvändarens autentisering med Data Lake Store med Azure Active Directory
> [!div class="op_single_selector"]
> * [Tjänst-till-tjänst-autentisering](data-lake-store-authenticate-using-active-directory.md)
> * [Slutanvändarautentisering](data-lake-store-end-user-authenticate-using-active-directory.md)
> 
> 

Azure Data Lake Store använder Azure Active Directory för autentisering. Innan du skapar ett program som fungerar med Azure Data Lake Store eller Azure Data Lake Analytics kan du först bestämma hur du vill att tooauthenticate ditt program med Azure Active Directory (AD Azure). hello två huvudsakliga tillgängliga alternativ är:

* Slutanvändarens autentisering (den här artikeln)
* Tjänst-till-tjänst-autentisering

Båda dessa alternativ kan resultera i ditt program som tillhandahålls med en OAuth 2.0-token som hämtar bifogade tooeach begäran tooAzure Data Lake Store eller Azure Data Lake Analytics.

Skapa den här artikeln pratar om hur en **interna Azure AD-program för slutanvändare autentisering**. Information om programkonfigurationen för Azure AD för autentisering för tjänst-till-tjänst finns [tjänst-till-tjänst-autentisering med Data Lake Store med Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Krav
* En Azure-prenumeration. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).

* Ditt prenumerations-ID. Du kan hämta den från hello Azure-portalen. Till exempel är den tillgänglig från hello Data Lake Store-kontoblad.
  
    ![Hämta prenumerations-ID](./media/data-lake-store-end-user-authenticate-using-active-directory/get-subscription-id.png)

* Din Azure AD-domännamn. Du kan hämta den med hovra hello musen i hello övre högra hörnet av hello Azure-portalen. Från hello skärmbilden nedan hello domännamnet är **contoso.onmicrosoft.com**, och hello GUID inom hakparenteser är hello klient-ID. 
  
    ![Hämta AAD-domän](./media/data-lake-store-end-user-authenticate-using-active-directory/get-aad-domain.png)

## <a name="end-user-authentication"></a>Slutanvändarautentisering
Detta är hello rekommendationer om du vill att en slutanvändare toolog i tooyour program via Azure AD. Programmet kommer att kunna tooaccess Azure-resurser med hello samma åtkomstnivå som hello slutanvändarens inloggad. Slutanvändaren behöver tooprovide sina autentiseringsuppgifter med jämna mellanrum i ordning för din toomaintain programåtkomst.

hello resultat av att låta hello slutanvändaren loggar in är att tillämpningsprogrammet ges en åtkomst-token och en uppdateringstoken. hello åtkomsttoken hämtar bifogade tooeach begäran tooData Datasjölager eller Data Lake Analytics och är giltig i en timme som standard. Hej uppdateringstoken kan vara används tooobtain som en ny åtkomsttoken, och det är giltigt för in tootwo veckor som standard om användas ofta. Du kan använda två olika metoder för slutanvändaren logga in.

### <a name="using-hello-oauth-20-pop-up"></a>Med hjälp av hello OAuth 2.0 popup-fönster
Programmet kan utlösa en OAuth 2.0 auktorisering popup-fönstret i vilka hello kan slutanvändaren anger sina autentiseringsuppgifter. Det här popup-fönster fungerar även med hello Azure AD tvåfaktorsautentisering (2FA) processen om det behövs. 

> [!NOTE]
> Den här metoden stöds inte ännu i hello Azure AD Authentication Library (ADAL) för Python eller Java.
> 
> 

### <a name="directly-passing-in-user-credentials"></a>Skicka direkt i användarens autentiseringsuppgifter
Programmet kan direkt innehåller användarens autentiseringsuppgifter tooAzure AD. Den här metoden fungerar bara med organisationens ID användarkonton; Det är inte kompatibelt med personal / ”live ID”-användarkonton, inklusive de som slutar på @outlook.com eller @live.com. Dessutom kan är den här metoden inte kompatibel med användarkonton som kräver Azure AD tvåfaktorsautentisering (2FA).

### <a name="what-do-i-need-toouse-this-approach"></a>Vad gör jag behöver toouse den här metoden?
* Azure AD-domännamn. Det finns redan i hello Förutsättningen för den här artikeln.
* Azure AD **programspecifika**
* Program-ID för det ursprungliga hello Azure AD-programmet
* Omdirigerings-URI för hello interna Azure AD-program
* Ange delegerade behörigheter


## <a name="step-1-create-an-active-directory-native-application"></a>Steg 1: Skapa en intern Active Directory-program

Skapa och konfigurera en inbyggd Azure AD-program för slutanvändare autentisering med Azure Data Lake Store med Azure Active Directory. Instruktioner finns i [skapa en Azure AD-program](../azure-resource-manager/resource-group-create-service-principal-portal.md).

Kontrollera att du väljer när följande hello instruktionerna på hello ovanför länken **interna** för programtyp, enligt hello skärmbilden nedan.

![Skapa webbapp](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "skapa inbyggda appen")

## <a name="step-2-get-application-id-and-redirect-uri"></a>Steg 2: Hämta program-id och omdirigerings-URI

Se [hämta program-ID för hello](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) tooretrieve hello program-id (även kallat hello klient-ID i hello klassiska Azure-portalen) för det ursprungliga hello Azure AD-programmet.

tooretrieve hello omdirigerings-URI, följ hello stegen nedan.

1. Hello Azure Portal, Välj **Azure Active Directory**, klickar du på **App registreringar**, söka efter och klickar på det ursprungliga hello Azure AD-programmet som du nyss skapade.

2. Från hello **inställningar** bladet för hello programmet, klickar du på **omdirigerings-URI: er**.

    ![Get omdirigerings-URI](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-redirect-uri.png)

3. Kopiera hello-värde som visas.


## <a name="step-3-set-permissions"></a>Steg 3: Ange behörigheter

1. Hello Azure Portal, Välj **Azure Active Directory**, klickar du på **App registreringar**, söka efter och klickar på det ursprungliga hello Azure AD-programmet som du nyss skapade.

2. Från hello **inställningar** bladet för hello programmet, klickar du på **nödvändiga behörigheter**, och klicka sedan på **Lägg till**.

    ![klient-id](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-1.png)

3. I hello **lägga till API-åtkomst** bladet, klickar du på **väljer en API**, klickar du på **Azure Data Lake**, och klicka sedan på **Välj**.

    ![klient-id](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-2.png)
 
4.  I hello **lägga till API-åtkomst** bladet, klickar du på **Välj behörigheter**, Välj hello kryssrutan toogive **fullständig åtkomst till tooData Datasjölager**, och klicka sedan på **Välj** .

    ![klient-id](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-3.png)

    Klicka på **Klar**.

5. Upprepa hello senaste två stegen toogrant behörigheter för **Windows Azure Service Management API** samt.
   
## <a name="next-steps"></a>Nästa steg
I den här artikeln du skapat ett enhetligt Azure AD-program och samlat hello information du behöver i ditt klientprogram som du skapar med hjälp av SDK för .NET, Java SDK, REST-API och så vidare. Du kan nu fortsätta toohello följande artiklar som talar om hur toouse hello Azure AD web application toofirst autentisera med Data Lake Store och utföra andra åtgärder på hello store.

* [Kom igång med Azure Data Lake Store med hjälp av .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Kom igång med Azure Data Lake Store med hjälp av Java SDK](data-lake-store-get-started-java-sdk.md)
* [Kom igång med Azure Data Lake Store med hjälp av REST API](data-lake-store-get-started-rest-api.md)

