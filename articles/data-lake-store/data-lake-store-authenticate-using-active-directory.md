---
title: "Tjänst-till-tjänst-autentisering: Data Lake Store med Azure Active Directory | Microsoft Docs"
description: "Lär dig hur tooachieve tjänst-till-tjänst autentisering med Data Lake Store med Azure Active Directory"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 820b7c5d-4863-4225-9bd1-df4d8f515537
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: nitinme
ms.openlocfilehash: 2e56237a75f020067b3248a1e1cfaf3c8df1371c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-to-service-authentication-with-data-lake-store-using-azure-active-directory"></a>Tjänst-till-tjänst-autentisering med Data Lake Store med Azure Active Directory
> [!div class="op_single_selector"]
> * [Tjänst-till-tjänst-autentisering](data-lake-store-authenticate-using-active-directory.md)
> * [Slutanvändarautentisering](data-lake-store-end-user-authenticate-using-active-directory.md)
> 
> 

Azure Data Lake Store använder Azure Active Directory för autentisering. Innan du skapar ett program som fungerar med Azure Data Lake Store eller Azure Data Lake Analytics kan du först bestämma hur du vill att tooauthenticate ditt program med Azure Active Directory (AD Azure). hello två huvudsakliga tillgängliga alternativ är:

* Slutanvändarautentisering 
* Tjänst-till-tjänst-autentisering (den här artikeln) 

Båda dessa alternativ kan resultera i ditt program som tillhandahålls med en OAuth 2.0-token som hämtar bifogade tooeach begäran tooAzure Data Lake Store eller Azure Data Lake Analytics.

Skapa den här artikeln pratar om hur en **webbprogram i Azure AD för autentisering av service to service**. Information om programkonfigurationen för Azure AD för autentisering för slutanvändaren finns [slutanvändarens autentisering med Data Lake Store med Azure Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Krav
* En Azure-prenumeration. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="step-1-create-an-active-directory-web-application"></a>Steg 1: Skapa ett Active Directory-webbprogram

Skapa och konfigurera ett webbprogram i Azure AD för autentisering för tjänst-till-tjänst med Azure Data Lake Store med Azure Active Directory. Instruktioner finns i [skapa en Azure AD-program](../azure-resource-manager/resource-group-create-service-principal-portal.md).

Kontrollera att du väljer när följande hello instruktionerna på hello ovanför länken **Web App / API** för programtyp, enligt hello skärmbilden nedan.

![Skapa webbapp](./media/data-lake-store-authenticate-using-active-directory/azure-active-directory-create-web-app.png "skapa webbprogram")

## <a name="step-2-get-application-id-authentication-key-and-tenant-id"></a>Steg 2: Hämta program-id och autentiseringsnyckel klient-id
När programmässigt inloggningen måste hello-id för ditt program. Om hello-programmet köras under egna autentiseringsuppgifter, måste du också en autentiseringsnyckel.

* Anvisningar för hur tooretrieve hello program-ID och autentisering nyckel (även kallade hello klienthemlighet) för ditt program finns [Get-ID och autentisering tangent](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key).

* Anvisningar för hur tooretrieve hello klient-ID finns [hämta klient-ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).

## <a name="step-3-assign-hello-azure-ad-application-toohello-azure-data-lake-store-account-file-or-folder-only-for-service-to-service-authentication"></a>Steg 3: Tilldela hello Azure AD application toohello Azure Data Lake Store-kontofil eller mapp (endast för service to service autentisering)
1. Logga in på nytt toohello [Azure-portalen](https://portal.azure.com) och öppna hello Azure Data Lake Store-konto som du vill tooassociate med hello Azure Active Directory-program som du skapade tidigare.
2. I ditt Data Lake Store-kontoblad klickar du på **Data Explorer**.
   
    ![Skapa kataloger i Data Lake Store-konto](./media/data-lake-store-authenticate-using-active-directory/adl.start.data.explorer.png "skapa kataloger i Data Lake-konto")
3. I hello **Data Explorer** bladet Klicka hello filen eller mappen som du vill tooprovide access toohello Azure AD-program och klicka sedan på **åtkomst**. tooconfigure access tooa-fil som du måste klicka på **åtkomst** från hello **Filförhandsgranskning** bladet.
   
    ![Ange ACL: er i Data Lake filsystemet](./media/data-lake-store-authenticate-using-active-directory/adl.acl.1.png "Ange ACL: er i Data Lake-filsystem")
4. Hej **åtkomst** bladet visar hello standard åtkomst och anpassade åtkomst har redan tilldelats toohello rot. Klicka på hello **Lägg till** ikonen tooadd Anpassad-nivå ACL: er.
   
    ![Visa en lista över standard och anpassad åtkomst](./media/data-lake-store-authenticate-using-active-directory/adl.acl.2.png "listan standard och anpassad åtkomst")
5. Klicka på hello **Lägg till** ikonen tooopen hello **Lägg till anpassad åtkomst** bladet. I det här bladet, klickar du på **Välj användare eller grupp**, och klicka sedan på **Välj användare eller grupp** bladet, leta efter hello Azure Active Directory-program som du skapade tidigare. Om du har en stor mängd grupper toosearch från Använd hello textrutan på hello översta toofilter på hello gruppnamn. Klicka på hello grupp du tooadd och klickar sedan på **Välj**.
   
    ![Lägga till en grupp](./media/data-lake-store-authenticate-using-active-directory/adl.acl.3.png "lägga till en grupp")
6. Klicka på **Välj behörigheter**hello behörigheter och välj om du vill tooassign hello behörigheter som standard ACL åtkomst till ACL eller båda. Klicka på **OK**.
   
    ![Tilldela behörigheter toogroup](./media/data-lake-store-authenticate-using-active-directory/adl.acl.4.png "tilldela behörigheter toogroup")
   
    Mer information om behörigheter i Data Lake Store och standard-/ Access ACL: er finns [åtkomstkontroll i Data Lake Store](data-lake-store-access-control.md).
7. I hello **Lägg till anpassad åtkomst** bladet, klickar du på **OK**. hello nyligen tillagda grupp med hello som är associerade behörigheter visas nu i hello **åtkomst** bladet.
   
    ![Tilldela behörigheter toogroup](./media/data-lake-store-authenticate-using-active-directory/adl.acl.5.png "tilldela behörigheter toogroup")

## <a name="step-4-get-hello-oauth-20-token-endpoint-only-for-java-based-applications"></a>Steg 4: Hämta hello OAuth 2.0-token för slutpunkt (endast för Java-baserade program)

1. Logga in på nytt toohello [Azure-portalen](https://portal.azure.com) och klicka på Active Directory från hello till vänster.

2. Hello till vänster och klicka på **App registreringar**.

3. Hello överkant hello App registreringar bladet, klickar du på **slutpunkter**.

    ![OAuth-token för slutpunkt](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint.png "OAuth-token för slutpunkt")

4. Hello listan över slutpunkter och kopiera hello OAuth 2.0-token för slutpunkt.

    ![OAuth-token för slutpunkt](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint-1.png "OAuth-token för slutpunkt")   

## <a name="next-steps"></a>Nästa steg
I den här artikeln du skapat ett webbprogram i Azure AD och samlat hello information du behöver i ditt klientprogram att du skapar med hjälp av SDK för .NET, Java SDK, osv. Du kan nu fortsätta toohello följande artiklar som talar om hur toouse hello Azure AD web application toofirst autentisera med Data Lake Store och utföra andra åtgärder på hello store.

* [Kom igång med Azure Data Lake Store med hjälp av .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Kom igång med Azure Data Lake Store med hjälp av Java SDK](data-lake-store-get-started-java-sdk.md)

Den här artikeln gått du igenom hello grundläggande steg behövs tooget användaren huvudnamn upp och körs för programmet. Du kan titta på hello följande artiklar tooget ytterligare information:
* [Använd PowerShell toocreate tjänstens huvudnamn](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Använda autentisering med certifikat för service principal autentisering](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal#create-service-principal-with-certificate)
* [Andra metoder tooauthenticate tooAzure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-authentication-scenarios)


