---
title: "Tjänst-till-tjänst-autentisering: Data Lake Store med Azure Active Directory | Microsoft Docs"
description: "Lär dig att uppnå service to service autentisering med Data Lake Store med Azure Active Directory"
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
ms.openlocfilehash: 27ec0a4f48115d44da98dd048868b044aedf173c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="service-to-service-authentication-with-data-lake-store-using-azure-active-directory"></a>Tjänst-till-tjänst-autentisering med Data Lake Store med Azure Active Directory
> [!div class="op_single_selector"]
> * [Tjänst-till-tjänst-autentisering](data-lake-store-authenticate-using-active-directory.md)
> * [Slutanvändarautentisering](data-lake-store-end-user-authenticate-using-active-directory.md)
> 
> 

Azure Data Lake Store använder Azure Active Directory för autentisering. Innan du skapar ett program som fungerar med Azure Data Lake Store eller Azure Data Lake Analytics kan bestämma du först hur du vill autentisera ditt program med Azure Active Directory (AD Azure). De största tillgängliga alternativ är:

* Slutanvändarautentisering 
* Tjänst-till-tjänst-autentisering (den här artikeln) 

Båda dessa alternativ kan resultera i ditt program som tillhandahålls med en OAuth 2.0-token som hämtar kopplade till varje begäran till Azure Data Lake Store eller Azure Data Lake Analytics.

Skapa den här artikeln pratar om hur en **webbprogram i Azure AD för autentisering av service to service**. Information om programkonfigurationen för Azure AD för autentisering för slutanvändaren finns [slutanvändarens autentisering med Data Lake Store med Azure Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Krav
* En Azure-prenumeration. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="step-1-create-an-active-directory-web-application"></a>Steg 1: Skapa ett Active Directory-webbprogram

Skapa och konfigurera ett webbprogram i Azure AD för autentisering för tjänst-till-tjänst med Azure Data Lake Store med Azure Active Directory. Instruktioner finns i [skapa en Azure AD-program](../azure-resource-manager/resource-group-create-service-principal-portal.md).

Kontrollera att du väljer när du följa anvisningarna i länken ovan **Web App / API** för programtyp, som visas i skärmbilden nedan.

![Skapa webbapp](./media/data-lake-store-authenticate-using-active-directory/azure-active-directory-create-web-app.png "skapa webbprogram")

## <a name="step-2-get-application-id-authentication-key-and-tenant-id"></a>Steg 2: Hämta program-id och autentiseringsnyckel klient-id
När programmässigt inloggningen måste ID: t för ditt program. Om programmet körs under egna autentiseringsuppgifter, måste du också en autentiseringsnyckel.

* Instruktioner om hur du hämtar program-ID och autentisering nyckeln (kallas även för klienthemligheten) för ditt program finns [Get-ID och autentisering tangent](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key).

* Instruktioner om hur du hämtar klient-ID finns [hämta klient-ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).

## <a name="step-3-assign-the-azure-ad-application-to-the-azure-data-lake-store-account-file-or-folder-only-for-service-to-service-authentication"></a>Steg 3: Tilldela Azure AD-program till Azure Data Lake Store-konto filen eller mappen (endast för service to service autentisering)
1. Logga in på den nya [Azure-portalen](https://portal.azure.com) och öppna Azure Data Lake Store-konto som du vill associera med Azure Active Directory-program som du skapade tidigare.
2. I ditt Data Lake Store-kontoblad klickar du på **Data Explorer**.
   
    ![Skapa kataloger i Data Lake Store-konto](./media/data-lake-store-authenticate-using-active-directory/adl.start.data.explorer.png "skapa kataloger i Data Lake-konto")
3. I den **Data Explorer** bladet, klickar du på filen eller mappen som du vill ge åtkomst till Azure AD-program och klicka sedan på **åtkomst**. Om du vill konfigurera åtkomst till en fil, måste du klicka på **åtkomst** från den **Filförhandsgranskning** bladet.
   
    ![Ange ACL: er i Data Lake filsystemet](./media/data-lake-store-authenticate-using-active-directory/adl.acl.1.png "Ange ACL: er i Data Lake-filsystem")
4. Den **åtkomst** bladet visar den standard tillgång och anpassade redan tilldelad till roten. Klicka på den **Lägg till** ikon för att lägga till anpassad nivå ACL: er.
   
    ![Visa en lista över standard och anpassad åtkomst](./media/data-lake-store-authenticate-using-active-directory/adl.acl.2.png "listan standard och anpassad åtkomst")
5. Klicka på den **Lägg till** ikon för att öppna den **Lägg till anpassad åtkomst** bladet. I det här bladet, klickar du på **Välj användare eller grupp**, och klicka sedan på **Välj användare eller grupp** bladet, leta efter Azure Active Directory-program som du skapade tidigare. Om du har många grupper att söka i, använder du textrutan längst upp för att filtrera efter gruppnamnet. Klicka på den grupp som du vill lägga till och klicka sedan på **Välj**.
   
    ![Lägga till en grupp](./media/data-lake-store-authenticate-using-active-directory/adl.acl.3.png "lägga till en grupp")
6. Klicka på **Välj behörigheter**behörigheter och välj om du vill tilldela behörigheterna som standard ACL åtkomst till ACL eller båda. Klicka på **OK**.
   
    ![Tilldela behörigheter till gruppen](./media/data-lake-store-authenticate-using-active-directory/adl.acl.4.png "tilldela behörigheter till gruppen")
   
    Mer information om behörigheter i Data Lake Store och standard-/ Access ACL: er finns [åtkomstkontroll i Data Lake Store](data-lake-store-access-control.md).
7. I den **Lägg till anpassad åtkomst** bladet, klickar du på **OK**. Den nyligen tillagda gruppen med associerade behörigheter visas nu i den **åtkomst** bladet.
   
    ![Tilldela behörigheter till gruppen](./media/data-lake-store-authenticate-using-active-directory/adl.acl.5.png "tilldela behörigheter till gruppen")

## <a name="step-4-get-the-oauth-20-token-endpoint-only-for-java-based-applications"></a>Steg 4: Hämta OAuth 2.0-token slutpunkten (endast för Java-baserade program)

1. Logga in på den nya [Azure-portalen](https://portal.azure.com) och klicka på Active Directory i den vänstra rutan.

2. I den vänstra rutan klickar du på **App registreringar**.

3. Högst upp på bladet registreringar klickar du på **slutpunkter**.

    ![OAuth-token för slutpunkt](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint.png "OAuth-token för slutpunkt")

4. Kopiera slutpunkten för OAuth 2.0-token från listan över slutpunkter.

    ![OAuth-token för slutpunkt](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint-1.png "OAuth-token för slutpunkt")   

## <a name="next-steps"></a>Nästa steg
I den här artikeln du skapat ett webbprogram i Azure AD och samlat in information som du behöver i ditt klientprogram som du skapar med hjälp av SDK för .NET, Java SDK, osv. Du kan nu fortsätta i följande artiklar som talar om hur du använder Azure AD-webbprogram att först autentisera med Data Lake Store och sedan utföra andra åtgärder i store.

* [Kom igång med Azure Data Lake Store med hjälp av .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Kom igång med Azure Data Lake Store med hjälp av Java SDK](data-lake-store-get-started-java-sdk.md)

Den här artikeln för att gått igenom de grundläggande steg som behövs för att få en användare huvudnamn upp och körs för programmet. Du kan titta på följande artiklar för att få mer information:
* [Använda PowerShell för att skapa tjänstens huvudnamn](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Använda autentisering med certifikat för service principal autentisering](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal#create-service-principal-with-certificate)
* [Andra metoder för att autentisera till Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-authentication-scenarios)


