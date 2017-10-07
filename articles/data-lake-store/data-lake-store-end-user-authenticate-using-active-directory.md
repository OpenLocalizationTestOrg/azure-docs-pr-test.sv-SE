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
# <a name="end-user-authentication-with-data-lake-store-using-azure-active-directory"></a><span data-ttu-id="b545d-103">Slutanvändarens autentisering med Data Lake Store med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b545d-103">End-user authentication with Data Lake Store using Azure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b545d-104">Tjänst-till-tjänst-autentisering</span><span class="sxs-lookup"><span data-stu-id="b545d-104">Service-to-service authentication</span></span>](data-lake-store-authenticate-using-active-directory.md)
> * [<span data-ttu-id="b545d-105">Slutanvändarautentisering</span><span class="sxs-lookup"><span data-stu-id="b545d-105">End-user authentication</span></span>](data-lake-store-end-user-authenticate-using-active-directory.md)
> 
> 

<span data-ttu-id="b545d-106">Azure Data Lake Store använder Azure Active Directory för autentisering.</span><span class="sxs-lookup"><span data-stu-id="b545d-106">Azure Data Lake Store uses Azure Active Directory for authentication.</span></span> <span data-ttu-id="b545d-107">Innan du skapar ett program som fungerar med Azure Data Lake Store eller Azure Data Lake Analytics kan du först bestämma hur du vill att tooauthenticate ditt program med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="b545d-107">Before authoring an application that works with Azure Data Lake Store or Azure Data Lake Analytics, you must first decide how you would like tooauthenticate your application with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="b545d-108">hello två huvudsakliga tillgängliga alternativ är:</span><span class="sxs-lookup"><span data-stu-id="b545d-108">hello two main options available are:</span></span>

* <span data-ttu-id="b545d-109">Slutanvändarens autentisering (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="b545d-109">End-user authentication (this article)</span></span>
* <span data-ttu-id="b545d-110">Tjänst-till-tjänst-autentisering</span><span class="sxs-lookup"><span data-stu-id="b545d-110">Service-to-service authentication</span></span>

<span data-ttu-id="b545d-111">Båda dessa alternativ kan resultera i ditt program som tillhandahålls med en OAuth 2.0-token som hämtar bifogade tooeach begäran tooAzure Data Lake Store eller Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b545d-111">Both these options result in your application being provided with an OAuth 2.0 token, which gets attached tooeach request made tooAzure Data Lake Store or Azure Data Lake Analytics.</span></span>

<span data-ttu-id="b545d-112">Skapa den här artikeln pratar om hur en **interna Azure AD-program för slutanvändare autentisering**.</span><span class="sxs-lookup"><span data-stu-id="b545d-112">This article talks about how create an **Azure AD native application for end-user authentication**.</span></span> <span data-ttu-id="b545d-113">Information om programkonfigurationen för Azure AD för autentisering för tjänst-till-tjänst finns [tjänst-till-tjänst-autentisering med Data Lake Store med Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="b545d-113">For instructions on Azure AD application configuration for service-to-service authentication see [Service-to-service authentication with Data Lake Store using Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b545d-114">Krav</span><span class="sxs-lookup"><span data-stu-id="b545d-114">Prerequisites</span></span>
* <span data-ttu-id="b545d-115">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b545d-115">An Azure subscription.</span></span> <span data-ttu-id="b545d-116">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b545d-116">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="b545d-117">Ditt prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="b545d-117">Your subscription ID.</span></span> <span data-ttu-id="b545d-118">Du kan hämta den från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b545d-118">You can retrieve it from hello Azure Portal.</span></span> <span data-ttu-id="b545d-119">Till exempel är den tillgänglig från hello Data Lake Store-kontoblad.</span><span class="sxs-lookup"><span data-stu-id="b545d-119">For example, it is available from hello Data Lake Store account blade.</span></span>
  
    ![Hämta prenumerations-ID](./media/data-lake-store-end-user-authenticate-using-active-directory/get-subscription-id.png)

* <span data-ttu-id="b545d-121">Din Azure AD-domännamn.</span><span class="sxs-lookup"><span data-stu-id="b545d-121">Your Azure AD domain name.</span></span> <span data-ttu-id="b545d-122">Du kan hämta den med hovra hello musen i hello övre högra hörnet av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b545d-122">You can retrieve it by hovering hello mouse in hello top-right corner of hello Azure Portal.</span></span> <span data-ttu-id="b545d-123">Från hello skärmbilden nedan hello domännamnet är **contoso.onmicrosoft.com**, och hello GUID inom hakparenteser är hello klient-ID.</span><span class="sxs-lookup"><span data-stu-id="b545d-123">From hello screenshot below, hello domain name is **contoso.onmicrosoft.com**, and hello GUID within brackets is hello tenant ID.</span></span> 
  
    ![Hämta AAD-domän](./media/data-lake-store-end-user-authenticate-using-active-directory/get-aad-domain.png)

## <a name="end-user-authentication"></a><span data-ttu-id="b545d-125">Slutanvändarautentisering</span><span class="sxs-lookup"><span data-stu-id="b545d-125">End-user authentication</span></span>
<span data-ttu-id="b545d-126">Detta är hello rekommendationer om du vill att en slutanvändare toolog i tooyour program via Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b545d-126">This is hello recommended approach if you want an end-user toolog in tooyour application via Azure AD.</span></span> <span data-ttu-id="b545d-127">Programmet kommer att kunna tooaccess Azure-resurser med hello samma åtkomstnivå som hello slutanvändarens inloggad.</span><span class="sxs-lookup"><span data-stu-id="b545d-127">Your application will be able tooaccess Azure resources with hello same level of access as hello end-user that logged in.</span></span> <span data-ttu-id="b545d-128">Slutanvändaren behöver tooprovide sina autentiseringsuppgifter med jämna mellanrum i ordning för din toomaintain programåtkomst.</span><span class="sxs-lookup"><span data-stu-id="b545d-128">Your end-user will need tooprovide their credentials periodically in order for your application toomaintain access.</span></span>

<span data-ttu-id="b545d-129">hello resultat av att låta hello slutanvändaren loggar in är att tillämpningsprogrammet ges en åtkomst-token och en uppdateringstoken.</span><span class="sxs-lookup"><span data-stu-id="b545d-129">hello result of having hello end-user log in is that your application is given an access token and a refresh token.</span></span> <span data-ttu-id="b545d-130">hello åtkomsttoken hämtar bifogade tooeach begäran tooData Datasjölager eller Data Lake Analytics och är giltig i en timme som standard.</span><span class="sxs-lookup"><span data-stu-id="b545d-130">hello access token gets attached tooeach request made tooData Lake Store or Data Lake Analytics, and it is valid for one hour by default.</span></span> <span data-ttu-id="b545d-131">Hej uppdateringstoken kan vara används tooobtain som en ny åtkomsttoken, och det är giltigt för in tootwo veckor som standard om användas ofta.</span><span class="sxs-lookup"><span data-stu-id="b545d-131">hello refresh token can be used tooobtain a new access token, and it is valid for up tootwo weeks by default, if used regularly.</span></span> <span data-ttu-id="b545d-132">Du kan använda två olika metoder för slutanvändaren logga in.</span><span class="sxs-lookup"><span data-stu-id="b545d-132">You can use two different approaches for end-user log in.</span></span>

### <a name="using-hello-oauth-20-pop-up"></a><span data-ttu-id="b545d-133">Med hjälp av hello OAuth 2.0 popup-fönster</span><span class="sxs-lookup"><span data-stu-id="b545d-133">Using hello OAuth 2.0 pop-up</span></span>
<span data-ttu-id="b545d-134">Programmet kan utlösa en OAuth 2.0 auktorisering popup-fönstret i vilka hello kan slutanvändaren anger sina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="b545d-134">Your application can trigger an OAuth 2.0 authorization pop-up, in which hello end-user can enter their credentials.</span></span> <span data-ttu-id="b545d-135">Det här popup-fönster fungerar även med hello Azure AD tvåfaktorsautentisering (2FA) processen om det behövs.</span><span class="sxs-lookup"><span data-stu-id="b545d-135">This pop-up also works with hello Azure AD Two-factor Authentication (2FA) process, if required.</span></span> 

> [!NOTE]
> <span data-ttu-id="b545d-136">Den här metoden stöds inte ännu i hello Azure AD Authentication Library (ADAL) för Python eller Java.</span><span class="sxs-lookup"><span data-stu-id="b545d-136">This method is not yet supported in hello Azure AD Authentication Library (ADAL) for Python or Java.</span></span>
> 
> 

### <a name="directly-passing-in-user-credentials"></a><span data-ttu-id="b545d-137">Skicka direkt i användarens autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="b545d-137">Directly passing in user credentials</span></span>
<span data-ttu-id="b545d-138">Programmet kan direkt innehåller användarens autentiseringsuppgifter tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="b545d-138">Your application can directly provide user credentials tooAzure AD.</span></span> <span data-ttu-id="b545d-139">Den här metoden fungerar bara med organisationens ID användarkonton; Det är inte kompatibelt med personal / ”live ID”-användarkonton, inklusive de som slutar på @outlook.com eller @live.com. Dessutom kan är den här metoden inte kompatibel med användarkonton som kräver Azure AD tvåfaktorsautentisering (2FA).</span><span class="sxs-lookup"><span data-stu-id="b545d-139">This method only works with organizational ID user accounts; it is not compatible with personal / “live ID” user accounts, including those ending in @outlook.com or @live.com. Furthermore, this method is not compatible with user accounts that require Azure AD Two-factor Authentication (2FA).</span></span>

### <a name="what-do-i-need-toouse-this-approach"></a><span data-ttu-id="b545d-140">Vad gör jag behöver toouse den här metoden?</span><span class="sxs-lookup"><span data-stu-id="b545d-140">What do I need toouse this approach?</span></span>
* <span data-ttu-id="b545d-141">Azure AD-domännamn.</span><span class="sxs-lookup"><span data-stu-id="b545d-141">Azure AD domain name.</span></span> <span data-ttu-id="b545d-142">Det finns redan i hello Förutsättningen för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="b545d-142">This is already listed in hello prerequisite of this article.</span></span>
* <span data-ttu-id="b545d-143">Azure AD **programspecifika**</span><span class="sxs-lookup"><span data-stu-id="b545d-143">Azure AD **native application**</span></span>
* <span data-ttu-id="b545d-144">Program-ID för det ursprungliga hello Azure AD-programmet</span><span class="sxs-lookup"><span data-stu-id="b545d-144">Application ID for hello Azure AD native application</span></span>
* <span data-ttu-id="b545d-145">Omdirigerings-URI för hello interna Azure AD-program</span><span class="sxs-lookup"><span data-stu-id="b545d-145">Redirect URI for hello Azure AD native application</span></span>
* <span data-ttu-id="b545d-146">Ange delegerade behörigheter</span><span class="sxs-lookup"><span data-stu-id="b545d-146">Set delegated permissions</span></span>


## <a name="step-1-create-an-active-directory-native-application"></a><span data-ttu-id="b545d-147">Steg 1: Skapa en intern Active Directory-program</span><span class="sxs-lookup"><span data-stu-id="b545d-147">Step 1: Create an Active Directory native application</span></span>

<span data-ttu-id="b545d-148">Skapa och konfigurera en inbyggd Azure AD-program för slutanvändare autentisering med Azure Data Lake Store med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b545d-148">Create and configure an Azure AD native application for end-user authentication with Azure Data Lake Store using Azure Active Directory.</span></span> <span data-ttu-id="b545d-149">Instruktioner finns i [skapa en Azure AD-program](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b545d-149">For instructions, see [Create an Azure AD application](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span>

<span data-ttu-id="b545d-150">Kontrollera att du väljer när följande hello instruktionerna på hello ovanför länken **interna** för programtyp, enligt hello skärmbilden nedan.</span><span class="sxs-lookup"><span data-stu-id="b545d-150">While following hello instructions at hello above link, make sure you select **Native** for application type, as shown in hello screenshot below.</span></span>

<span data-ttu-id="b545d-151">![Skapa webbapp](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "skapa inbyggda appen")</span><span class="sxs-lookup"><span data-stu-id="b545d-151">![Create web app](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "Create native app")</span></span>

## <a name="step-2-get-application-id-and-redirect-uri"></a><span data-ttu-id="b545d-152">Steg 2: Hämta program-id och omdirigerings-URI</span><span class="sxs-lookup"><span data-stu-id="b545d-152">Step 2: Get application id and redirect URI</span></span>

<span data-ttu-id="b545d-153">Se [hämta program-ID för hello](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) tooretrieve hello program-id (även kallat hello klient-ID i hello klassiska Azure-portalen) för det ursprungliga hello Azure AD-programmet.</span><span class="sxs-lookup"><span data-stu-id="b545d-153">See [Get hello application ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) tooretrieve hello application id (also called hello client ID in hello Azure classic portal) of hello Azure AD native application.</span></span>

<span data-ttu-id="b545d-154">tooretrieve hello omdirigerings-URI, följ hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="b545d-154">tooretrieve hello redirect URI, follow hello steps below.</span></span>

1. <span data-ttu-id="b545d-155">Hello Azure Portal, Välj **Azure Active Directory**, klickar du på **App registreringar**, söka efter och klickar på det ursprungliga hello Azure AD-programmet som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="b545d-155">From hello Azure Portal, select **Azure Active Directory**, click **App registrations**, and then find and click hello Azure AD native application that you just created.</span></span>

2. <span data-ttu-id="b545d-156">Från hello **inställningar** bladet för hello programmet, klickar du på **omdirigerings-URI: er**.</span><span class="sxs-lookup"><span data-stu-id="b545d-156">From hello **Settings** blade for hello application, click **Redirect URIs**.</span></span>

    ![Get omdirigerings-URI](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-redirect-uri.png)

3. <span data-ttu-id="b545d-158">Kopiera hello-värde som visas.</span><span class="sxs-lookup"><span data-stu-id="b545d-158">Copy hello value displayed.</span></span>


## <a name="step-3-set-permissions"></a><span data-ttu-id="b545d-159">Steg 3: Ange behörigheter</span><span class="sxs-lookup"><span data-stu-id="b545d-159">Step 3: Set permissions</span></span>

1. <span data-ttu-id="b545d-160">Hello Azure Portal, Välj **Azure Active Directory**, klickar du på **App registreringar**, söka efter och klickar på det ursprungliga hello Azure AD-programmet som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="b545d-160">From hello Azure Portal, select **Azure Active Directory**, click **App registrations**, and then find and click hello Azure AD native application that you just created.</span></span>

2. <span data-ttu-id="b545d-161">Från hello **inställningar** bladet för hello programmet, klickar du på **nödvändiga behörigheter**, och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="b545d-161">From hello **Settings** blade for hello application, click **Required permissions**, and then click **Add**.</span></span>

    ![klient-id](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-1.png)

3. <span data-ttu-id="b545d-163">I hello **lägga till API-åtkomst** bladet, klickar du på **väljer en API**, klickar du på **Azure Data Lake**, och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="b545d-163">In hello **Add API Access** blade, click **Select an API**, click **Azure Data Lake**, and then click **Select**.</span></span>

    ![klient-id](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-2.png)
 
4.  <span data-ttu-id="b545d-165">I hello **lägga till API-åtkomst** bladet, klickar du på **Välj behörigheter**, Välj hello kryssrutan toogive **fullständig åtkomst till tooData Datasjölager**, och klicka sedan på **Välj** .</span><span class="sxs-lookup"><span data-stu-id="b545d-165">In hello **Add API Access** blade, click **Select permissions**, select hello check box toogive **Full access tooData Lake Store**, and then click **Select**.</span></span>

    ![klient-id](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-3.png)

    <span data-ttu-id="b545d-167">Klicka på **Klar**.</span><span class="sxs-lookup"><span data-stu-id="b545d-167">Click **Done**.</span></span>

5. <span data-ttu-id="b545d-168">Upprepa hello senaste två stegen toogrant behörigheter för **Windows Azure Service Management API** samt.</span><span class="sxs-lookup"><span data-stu-id="b545d-168">Repeat hello last two steps toogrant permissions for **Windows Azure Service Management API** as well.</span></span>
   
## <a name="next-steps"></a><span data-ttu-id="b545d-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b545d-169">Next steps</span></span>
<span data-ttu-id="b545d-170">I den här artikeln du skapat ett enhetligt Azure AD-program och samlat hello information du behöver i ditt klientprogram som du skapar med hjälp av SDK för .NET, Java SDK, REST-API och så vidare. Du kan nu fortsätta toohello följande artiklar som talar om hur toouse hello Azure AD web application toofirst autentisera med Data Lake Store och utföra andra åtgärder på hello store.</span><span class="sxs-lookup"><span data-stu-id="b545d-170">In this article you created an Azure AD native application and gathered hello information you need in your client applications that you author using .NET SDK, Java SDK, REST API, etc. You can now proceed toohello following articles that talk about how toouse hello Azure AD web application toofirst authenticate with Data Lake Store and then perform other operations on hello store.</span></span>

* [<span data-ttu-id="b545d-171">Kom igång med Azure Data Lake Store med hjälp av .NET SDK</span><span class="sxs-lookup"><span data-stu-id="b545d-171">Get started with Azure Data Lake Store using .NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
* [<span data-ttu-id="b545d-172">Kom igång med Azure Data Lake Store med hjälp av Java SDK</span><span class="sxs-lookup"><span data-stu-id="b545d-172">Get started with Azure Data Lake Store using Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
* [<span data-ttu-id="b545d-173">Kom igång med Azure Data Lake Store med hjälp av REST API</span><span class="sxs-lookup"><span data-stu-id="b545d-173">Get started with Azure Data Lake Store using REST API</span></span>](data-lake-store-get-started-rest-api.md)

