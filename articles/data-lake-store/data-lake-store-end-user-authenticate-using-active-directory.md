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
# <a name="end-user-authentication-with-data-lake-store-using-azure-active-directory"></a><span data-ttu-id="26147-103">Slutanvändarens autentisering med Data Lake Store med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="26147-103">End-user authentication with Data Lake Store using Azure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="26147-104">Tjänst-till-tjänst-autentisering</span><span class="sxs-lookup"><span data-stu-id="26147-104">Service-to-service authentication</span></span>](data-lake-store-authenticate-using-active-directory.md)
> * [<span data-ttu-id="26147-105">Slutanvändarautentisering</span><span class="sxs-lookup"><span data-stu-id="26147-105">End-user authentication</span></span>](data-lake-store-end-user-authenticate-using-active-directory.md)
> 
> 

<span data-ttu-id="26147-106">Azure Data Lake Store använder Azure Active Directory för autentisering.</span><span class="sxs-lookup"><span data-stu-id="26147-106">Azure Data Lake Store uses Azure Active Directory for authentication.</span></span> <span data-ttu-id="26147-107">Innan du skapar ett program som fungerar med Azure Data Lake Store eller Azure Data Lake Analytics kan bestämma du först hur du vill autentisera ditt program med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="26147-107">Before authoring an application that works with Azure Data Lake Store or Azure Data Lake Analytics, you must first decide how you would like to authenticate your application with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="26147-108">De största tillgängliga alternativ är:</span><span class="sxs-lookup"><span data-stu-id="26147-108">The two main options available are:</span></span>

* <span data-ttu-id="26147-109">Slutanvändarens autentisering (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="26147-109">End-user authentication (this article)</span></span>
* <span data-ttu-id="26147-110">Tjänst-till-tjänst-autentisering</span><span class="sxs-lookup"><span data-stu-id="26147-110">Service-to-service authentication</span></span>

<span data-ttu-id="26147-111">Båda dessa alternativ kan resultera i ditt program som tillhandahålls med en OAuth 2.0-token som hämtar kopplade till varje begäran till Azure Data Lake Store eller Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="26147-111">Both these options result in your application being provided with an OAuth 2.0 token, which gets attached to each request made to Azure Data Lake Store or Azure Data Lake Analytics.</span></span>

<span data-ttu-id="26147-112">Skapa den här artikeln pratar om hur en **interna Azure AD-program för slutanvändare autentisering**.</span><span class="sxs-lookup"><span data-stu-id="26147-112">This article talks about how create an **Azure AD native application for end-user authentication**.</span></span> <span data-ttu-id="26147-113">Information om programkonfigurationen för Azure AD för autentisering för tjänst-till-tjänst finns [tjänst-till-tjänst-autentisering med Data Lake Store med Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="26147-113">For instructions on Azure AD application configuration for service-to-service authentication see [Service-to-service authentication with Data Lake Store using Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="26147-114">Krav</span><span class="sxs-lookup"><span data-stu-id="26147-114">Prerequisites</span></span>
* <span data-ttu-id="26147-115">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="26147-115">An Azure subscription.</span></span> <span data-ttu-id="26147-116">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="26147-116">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="26147-117">Ditt prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="26147-117">Your subscription ID.</span></span> <span data-ttu-id="26147-118">Du kan hämta den från Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="26147-118">You can retrieve it from the Azure Portal.</span></span> <span data-ttu-id="26147-119">Till exempel är den tillgänglig från bladet Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="26147-119">For example, it is available from the Data Lake Store account blade.</span></span>
  
    ![Hämta prenumerations-ID](./media/data-lake-store-end-user-authenticate-using-active-directory/get-subscription-id.png)

* <span data-ttu-id="26147-121">Din Azure AD-domännamn.</span><span class="sxs-lookup"><span data-stu-id="26147-121">Your Azure AD domain name.</span></span> <span data-ttu-id="26147-122">Du kan hämta den håller musen i övre högra hörnet i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="26147-122">You can retrieve it by hovering the mouse in the top-right corner of the Azure Portal.</span></span> <span data-ttu-id="26147-123">Från skärmbilden nedan domännamnet är **contoso.onmicrosoft.com**, och GUID inom hakparenteser är klient-ID.</span><span class="sxs-lookup"><span data-stu-id="26147-123">From the screenshot below, the domain name is **contoso.onmicrosoft.com**, and the GUID within brackets is the tenant ID.</span></span> 
  
    ![Hämta AAD-domän](./media/data-lake-store-end-user-authenticate-using-active-directory/get-aad-domain.png)

## <a name="end-user-authentication"></a><span data-ttu-id="26147-125">Slutanvändarautentisering</span><span class="sxs-lookup"><span data-stu-id="26147-125">End-user authentication</span></span>
<span data-ttu-id="26147-126">Det här är den rekommenderade metoden om du vill att en slutanvändare att logga in på ditt program via Azure AD.</span><span class="sxs-lookup"><span data-stu-id="26147-126">This is the recommended approach if you want an end-user to log in to your application via Azure AD.</span></span> <span data-ttu-id="26147-127">Programmet kommer att kunna komma åt Azure-resurser med samma nivå av åtkomst som slutanvändare inloggad.</span><span class="sxs-lookup"><span data-stu-id="26147-127">Your application will be able to access Azure resources with the same level of access as the end-user that logged in.</span></span> <span data-ttu-id="26147-128">Slutanvändaren måste ange sina autentiseringsuppgifter med jämna mellanrum i ordning för programmet att upprätthålla åtkomsten.</span><span class="sxs-lookup"><span data-stu-id="26147-128">Your end-user will need to provide their credentials periodically in order for your application to maintain access.</span></span>

<span data-ttu-id="26147-129">Resultatet av att slutanvändaren loggar in är att tillämpningsprogrammet ges en åtkomst-token och en uppdateringstoken.</span><span class="sxs-lookup"><span data-stu-id="26147-129">The result of having the end-user log in is that your application is given an access token and a refresh token.</span></span> <span data-ttu-id="26147-130">Åtkomsttoken hämtar ansluten till varje begäran till Data Lake Store eller Data Lake Analytics och är giltig i en timme som standard.</span><span class="sxs-lookup"><span data-stu-id="26147-130">The access token gets attached to each request made to Data Lake Store or Data Lake Analytics, and it is valid for one hour by default.</span></span> <span data-ttu-id="26147-131">Uppdateringstoken som kan användas för att hämta en ny åtkomsttoken och det är giltigt för upp till två veckor som standard om användas ofta.</span><span class="sxs-lookup"><span data-stu-id="26147-131">The refresh token can be used to obtain a new access token, and it is valid for up to two weeks by default, if used regularly.</span></span> <span data-ttu-id="26147-132">Du kan använda två olika metoder för slutanvändaren logga in.</span><span class="sxs-lookup"><span data-stu-id="26147-132">You can use two different approaches for end-user log in.</span></span>

### <a name="using-the-oauth-20-pop-up"></a><span data-ttu-id="26147-133">Med hjälp av OAuth 2.0-popup-fönster</span><span class="sxs-lookup"><span data-stu-id="26147-133">Using the OAuth 2.0 pop-up</span></span>
<span data-ttu-id="26147-134">Programmet kan utlösa en OAuth 2.0 auktorisering popup-fönstret som slutanvändaren kan ange sina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="26147-134">Your application can trigger an OAuth 2.0 authorization pop-up, in which the end-user can enter their credentials.</span></span> <span data-ttu-id="26147-135">Det här popup-fönster fungerar även med Azure AD tvåfaktorsautentisering (2FA)-processen om det behövs.</span><span class="sxs-lookup"><span data-stu-id="26147-135">This pop-up also works with the Azure AD Two-factor Authentication (2FA) process, if required.</span></span> 

> [!NOTE]
> <span data-ttu-id="26147-136">Den här metoden stöds inte ännu i Azure AD Authentication Library (ADAL) för Python eller Java.</span><span class="sxs-lookup"><span data-stu-id="26147-136">This method is not yet supported in the Azure AD Authentication Library (ADAL) for Python or Java.</span></span>
> 
> 

### <a name="directly-passing-in-user-credentials"></a><span data-ttu-id="26147-137">Skicka direkt i användarens autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="26147-137">Directly passing in user credentials</span></span>
<span data-ttu-id="26147-138">Programmet kan direkt ange autentiseringsuppgifter för användare till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="26147-138">Your application can directly provide user credentials to Azure AD.</span></span> <span data-ttu-id="26147-139">Den här metoden fungerar bara med organisationens ID användarkonton; Det är inte kompatibelt med personal / ”live ID”-användarkonton, inklusive de som slutar på @outlook.com eller @live.com.</span><span class="sxs-lookup"><span data-stu-id="26147-139">This method only works with organizational ID user accounts; it is not compatible with personal / “live ID” user accounts, including those ending in @outlook.com or @live.com.</span></span> <span data-ttu-id="26147-140">Dessutom kan är den här metoden inte kompatibel med användarkonton som kräver Azure AD tvåfaktorsautentisering (2FA).</span><span class="sxs-lookup"><span data-stu-id="26147-140">Furthermore, this method is not compatible with user accounts that require Azure AD Two-factor Authentication (2FA).</span></span>

### <a name="what-do-i-need-to-use-this-approach"></a><span data-ttu-id="26147-141">Vad behöver jag använda den här metoden?</span><span class="sxs-lookup"><span data-stu-id="26147-141">What do I need to use this approach?</span></span>
* <span data-ttu-id="26147-142">Azure AD-domännamn.</span><span class="sxs-lookup"><span data-stu-id="26147-142">Azure AD domain name.</span></span> <span data-ttu-id="26147-143">Det finns redan i komponenten i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="26147-143">This is already listed in the prerequisite of this article.</span></span>
* <span data-ttu-id="26147-144">Azure AD **programspecifika**</span><span class="sxs-lookup"><span data-stu-id="26147-144">Azure AD **native application**</span></span>
* <span data-ttu-id="26147-145">Program-ID för det interna Azure AD-programmet</span><span class="sxs-lookup"><span data-stu-id="26147-145">Application ID for the Azure AD native application</span></span>
* <span data-ttu-id="26147-146">Omdirigerings-URI för det interna Azure AD-programmet</span><span class="sxs-lookup"><span data-stu-id="26147-146">Redirect URI for the Azure AD native application</span></span>
* <span data-ttu-id="26147-147">Ange delegerade behörigheter</span><span class="sxs-lookup"><span data-stu-id="26147-147">Set delegated permissions</span></span>


## <a name="step-1-create-an-active-directory-native-application"></a><span data-ttu-id="26147-148">Steg 1: Skapa en intern Active Directory-program</span><span class="sxs-lookup"><span data-stu-id="26147-148">Step 1: Create an Active Directory native application</span></span>

<span data-ttu-id="26147-149">Skapa och konfigurera en inbyggd Azure AD-program för slutanvändare autentisering med Azure Data Lake Store med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="26147-149">Create and configure an Azure AD native application for end-user authentication with Azure Data Lake Store using Azure Active Directory.</span></span> <span data-ttu-id="26147-150">Instruktioner finns i [skapa en Azure AD-program](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="26147-150">For instructions, see [Create an Azure AD application](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span>

<span data-ttu-id="26147-151">Kontrollera att du väljer när du följa anvisningarna i länken ovan **interna** för programtyp, som visas i skärmbilden nedan.</span><span class="sxs-lookup"><span data-stu-id="26147-151">While following the instructions at the above link, make sure you select **Native** for application type, as shown in the screenshot below.</span></span>

<span data-ttu-id="26147-152">![Skapa webbapp](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "skapa inbyggda appen")</span><span class="sxs-lookup"><span data-stu-id="26147-152">![Create web app](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "Create native app")</span></span>

## <a name="step-2-get-application-id-and-redirect-uri"></a><span data-ttu-id="26147-153">Steg 2: Hämta program-id och omdirigerings-URI</span><span class="sxs-lookup"><span data-stu-id="26147-153">Step 2: Get application id and redirect URI</span></span>

<span data-ttu-id="26147-154">Se [hämta program-ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) att hämta program-id (även kallat klient-ID i den klassiska Azure-portalen) för den ursprungliga Azure AD-programmet.</span><span class="sxs-lookup"><span data-stu-id="26147-154">See [Get the application ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) to retrieve the application id (also called the client ID in the Azure classic portal) of the Azure AD native application.</span></span>

<span data-ttu-id="26147-155">Följ stegen nedan om du vill hämta omdirigerings-URI.</span><span class="sxs-lookup"><span data-stu-id="26147-155">To retrieve the redirect URI, follow the steps below.</span></span>

1. <span data-ttu-id="26147-156">Välj Azure-portalen **Azure Active Directory**, klickar du på **App registreringar**, söka efter och klickar på den interna Azure AD-program som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="26147-156">From the Azure Portal, select **Azure Active Directory**, click **App registrations**, and then find and click the Azure AD native application that you just created.</span></span>

2. <span data-ttu-id="26147-157">Från den **inställningar** bladet för programmet, klickar du på **omdirigerings-URI: er**.</span><span class="sxs-lookup"><span data-stu-id="26147-157">From the **Settings** blade for the application, click **Redirect URIs**.</span></span>

    ![Get omdirigerings-URI](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-redirect-uri.png)

3. <span data-ttu-id="26147-159">Kopiera värdet som visas.</span><span class="sxs-lookup"><span data-stu-id="26147-159">Copy the value displayed.</span></span>


## <a name="step-3-set-permissions"></a><span data-ttu-id="26147-160">Steg 3: Ange behörigheter</span><span class="sxs-lookup"><span data-stu-id="26147-160">Step 3: Set permissions</span></span>

1. <span data-ttu-id="26147-161">Välj Azure-portalen **Azure Active Directory**, klickar du på **App registreringar**, söka efter och klickar på den interna Azure AD-program som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="26147-161">From the Azure Portal, select **Azure Active Directory**, click **App registrations**, and then find and click the Azure AD native application that you just created.</span></span>

2. <span data-ttu-id="26147-162">Från den **inställningar** bladet för programmet, klickar du på **nödvändiga behörigheter**, och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="26147-162">From the **Settings** blade for the application, click **Required permissions**, and then click **Add**.</span></span>

    ![klient-id](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-1.png)

3. <span data-ttu-id="26147-164">I den **lägga till API-åtkomst** bladet, klickar du på **väljer en API**, klickar du på **Azure Data Lake**, och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="26147-164">In the **Add API Access** blade, click **Select an API**, click **Azure Data Lake**, and then click **Select**.</span></span>

    ![klient-id](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-2.png)
 
4.  <span data-ttu-id="26147-166">I den **lägga till API-åtkomst** bladet, klickar du på **Välj behörigheter**, markera kryssrutan för att ge **fullständig åtkomst till Data Lake Store**, och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="26147-166">In the **Add API Access** blade, click **Select permissions**, select the check box to give **Full access to Data Lake Store**, and then click **Select**.</span></span>

    ![klient-id](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-3.png)

    <span data-ttu-id="26147-168">Klicka på **Klar**.</span><span class="sxs-lookup"><span data-stu-id="26147-168">Click **Done**.</span></span>

5. <span data-ttu-id="26147-169">Upprepa de två sista stegen för att bevilja behörigheter för **Windows Azure Service Management API** samt.</span><span class="sxs-lookup"><span data-stu-id="26147-169">Repeat the last two steps to grant permissions for **Windows Azure Service Management API** as well.</span></span>
   
## <a name="next-steps"></a><span data-ttu-id="26147-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="26147-170">Next steps</span></span>
<span data-ttu-id="26147-171">I den här artikeln du skapat ett enhetligt Azure AD-program och samlat in den information du behöver i ditt klientprogram att du skapar med hjälp av SDK för .NET, Java SDK, REST-API och så vidare. Du kan nu fortsätta i följande artiklar som talar om hur du använder Azure AD-webbprogram att först autentisera med Data Lake Store och sedan utföra andra åtgärder i store.</span><span class="sxs-lookup"><span data-stu-id="26147-171">In this article you created an Azure AD native application and gathered the information you need in your client applications that you author using .NET SDK, Java SDK, REST API, etc. You can now proceed to the following articles that talk about how to use the Azure AD web application to first authenticate with Data Lake Store and then perform other operations on the store.</span></span>

* [<span data-ttu-id="26147-172">Kom igång med Azure Data Lake Store med hjälp av .NET SDK</span><span class="sxs-lookup"><span data-stu-id="26147-172">Get started with Azure Data Lake Store using .NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
* [<span data-ttu-id="26147-173">Kom igång med Azure Data Lake Store med hjälp av Java SDK</span><span class="sxs-lookup"><span data-stu-id="26147-173">Get started with Azure Data Lake Store using Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
* [<span data-ttu-id="26147-174">Kom igång med Azure Data Lake Store med hjälp av REST API</span><span class="sxs-lookup"><span data-stu-id="26147-174">Get started with Azure Data Lake Store using REST API</span></span>](data-lake-store-get-started-rest-api.md)

