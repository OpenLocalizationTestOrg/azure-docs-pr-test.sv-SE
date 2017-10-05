---
title: "Självstudier: Azure Active Directory-integrering med Cerner Central | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Cerner Central."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2bc549d-d286-4679-854e-bb67c62b0475
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 77b5fb94cdfa5722081198aabc59fbf86229c2b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cerner-central"></a><span data-ttu-id="502ba-103">Självstudier: Azure Active Directory-integrering med Cerner Central</span><span class="sxs-lookup"><span data-stu-id="502ba-103">Tutorial: Azure Active Directory integration with Cerner Central</span></span>

<span data-ttu-id="502ba-104">I kursen får lära du att integrera Cerner Central med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="502ba-104">In this tutorial, you learn how to integrate Cerner Central with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="502ba-105">Integrera Cerner Central med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="502ba-105">Integrating Cerner Central with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="502ba-106">Du kan styra i Azure AD som har åtkomst till Cerner Central</span><span class="sxs-lookup"><span data-stu-id="502ba-106">You can control in Azure AD who has access to Cerner Central</span></span>
- <span data-ttu-id="502ba-107">Du kan aktivera användarna att automatiskt hämta loggat in på Cerner Central (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="502ba-107">You can enable your users to automatically get signed-on to Cerner Central (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="502ba-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="502ba-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="502ba-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="502ba-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="502ba-110">Krav</span><span class="sxs-lookup"><span data-stu-id="502ba-110">Prerequisites</span></span>

<span data-ttu-id="502ba-111">Om du vill konfigurera Azure AD-integrering med Cerner Central behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="502ba-111">To configure Azure AD integration with Cerner Central, you need the following items:</span></span>

- <span data-ttu-id="502ba-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="502ba-112">An Azure AD subscription</span></span>
- <span data-ttu-id="502ba-113">En godkänd Cerner centrala System-kontot</span><span class="sxs-lookup"><span data-stu-id="502ba-113">An approved Cerner Central System Account</span></span>

> [!NOTE]
> <span data-ttu-id="502ba-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="502ba-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="502ba-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="502ba-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="502ba-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="502ba-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="502ba-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="502ba-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="502ba-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="502ba-118">Scenario description</span></span>
<span data-ttu-id="502ba-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="502ba-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="502ba-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="502ba-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="502ba-121">Att lägga till Cerner Central från galleriet</span><span class="sxs-lookup"><span data-stu-id="502ba-121">Adding Cerner Central from the gallery</span></span>
2. <span data-ttu-id="502ba-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="502ba-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cerner-central-from-the-gallery"></a><span data-ttu-id="502ba-123">Att lägga till Cerner Central från galleriet</span><span class="sxs-lookup"><span data-stu-id="502ba-123">Adding Cerner Central from the gallery</span></span>
<span data-ttu-id="502ba-124">Du måste lägga till Cerner Central från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Cerner Central i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="502ba-124">To configure the integration of Cerner Central into Azure AD, you need to add Cerner Central from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="502ba-125">**Utför följande steg för att lägga till Cerner Central från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="502ba-125">**To add Cerner Central from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="502ba-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="502ba-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="502ba-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="502ba-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="502ba-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="502ba-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="502ba-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen ovanpå dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="502ba-131">To add new application, click **New application** button on top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="502ba-133">I sökrutan skriver **Cerner Central**.</span><span class="sxs-lookup"><span data-stu-id="502ba-133">In the search box, type **Cerner Central**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_search.png)

5. <span data-ttu-id="502ba-135">Välj i resultatpanelen **Cerner Central**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="502ba-135">In the results panel, select **Cerner Central**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="502ba-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="502ba-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="502ba-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Cerner Central baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="502ba-138">In this section, you configure and test Azure AD single sign-on with Cerner Central based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="502ba-139">Azure AD måste du känna till användaren i Cerner Central motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="502ba-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Cerner Central is to a user in Azure AD.</span></span> <span data-ttu-id="502ba-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Cerner Central upprättas.</span><span class="sxs-lookup"><span data-stu-id="502ba-140">In other words, a link relationship between an Azure AD user and the related user in Cerner Central needs to be established.</span></span>

<span data-ttu-id="502ba-141">Om du vill konfigurera och testa Azure AD enkel inloggning med Cerner Central, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="502ba-141">To configure and test Azure AD single sign-on with Cerner Central, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="502ba-142">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="502ba-142">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="502ba-143">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="502ba-143">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="502ba-144">**[Skapa en testanvändare Cerner Central](#creating-a-cerner-central-test-user)**  – har en motsvarighet för Britta Simon Cerner Central som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="502ba-144">**[Creating a Cerner Central test user](#creating-a-cerner-central-test-user)** - to have a counterpart of Britta Simon in Cerner Central that is linked to the Azure AD representation of the user.</span></span>
4. <span data-ttu-id="502ba-145">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="502ba-145">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="502ba-146">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="502ba-146">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="502ba-147">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="502ba-147">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="502ba-148">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Cerner Central.</span><span class="sxs-lookup"><span data-stu-id="502ba-148">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Cerner Central application.</span></span>

<span data-ttu-id="502ba-149">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Cerner Central:**</span><span class="sxs-lookup"><span data-stu-id="502ba-149">**To configure Azure AD single sign-on with Cerner Central, perform the following steps:**</span></span>

1. <span data-ttu-id="502ba-150">I Azure-portalen på den **Cerner Central** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="502ba-150">In the Azure portal, on the **Cerner Central** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="502ba-152">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="502ba-152">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_samlbase.png)

3. <span data-ttu-id="502ba-154">På den **Cerner centrala domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="502ba-154">On the **Cerner Central Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_url.png)

    <span data-ttu-id="502ba-156">a.</span><span class="sxs-lookup"><span data-stu-id="502ba-156">a.</span></span> <span data-ttu-id="502ba-157">I den **identifierare** textruta Skriv det värde som använder följande mönster:</span><span class="sxs-lookup"><span data-stu-id="502ba-157">In the **Identifier** textbox, type the value using the following patterns:</span></span>
    
    | |
    |--|
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/metadata` |
    | `https://<instancename>.sandboxcerner.com/session-api/protocol/saml2/metadata` |
    | `https://<instancename>.sandboxcernercentral.com/session-api/protocol/saml2/metadata` |
    | `https://sandboxcernercentral.com/session-api/protocol/saml2/metadata` |
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/metadata` |

    <span data-ttu-id="502ba-158">b.</span><span class="sxs-lookup"><span data-stu-id="502ba-158">b.</span></span> <span data-ttu-id="502ba-159">I den **Reply URL** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="502ba-159">In the **Reply URL** textbox, type a URL using the following patterns:</span></span> 
    | |
    |--|
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/sso` |
    | `https://cernercentral.com/<instasncename>` |
    | `https://sandboxcernercentral.com/<instancename>` |
    | `https://sandboxcernercentral.com/<instancename>` |
    | `https://<subdomain>.sandboxcernercentral.com/<instancename>` |

    > [!NOTE] 
    > <span data-ttu-id="502ba-160">Dessa värden är inte verkligt.</span><span class="sxs-lookup"><span data-stu-id="502ba-160">These values are not the real.</span></span> <span data-ttu-id="502ba-161">Uppdatera dessa värden med den faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="502ba-161">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="502ba-162">Kontakta [Cerner Central supportteamet](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="502ba-162">Contact [Cerner Central support team](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations) to get these values.</span></span>
 
4. <span data-ttu-id="502ba-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="502ba-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cernercentral-tutorial/tutorial_general_400.png)

5. <span data-ttu-id="502ba-165">Att generera den **Metadata** url, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="502ba-165">To generate the **Metadata** url, perform the following steps:</span></span>

    <span data-ttu-id="502ba-166">a.</span><span class="sxs-lookup"><span data-stu-id="502ba-166">a.</span></span> <span data-ttu-id="502ba-167">Klicka på **App registreringar**.</span><span class="sxs-lookup"><span data-stu-id="502ba-167">Click **App registrations**.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_appregistrations.png)
   
    <span data-ttu-id="502ba-169">b.</span><span class="sxs-lookup"><span data-stu-id="502ba-169">b.</span></span> <span data-ttu-id="502ba-170">Klicka på **slutpunkter** att öppna **slutpunkter** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="502ba-170">Click **Endpoints** to open **Endpoints** dialog box.</span></span>  
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_endpointicon.png)

    <span data-ttu-id="502ba-172">c.</span><span class="sxs-lookup"><span data-stu-id="502ba-172">c.</span></span> <span data-ttu-id="502ba-173">Klicka på kopieringsknappen för att kopiera **FEDERATION METADATADOKUMENTET** url och klistra in den i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="502ba-173">Click the copy button to copy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_endpoint.png)
     
    <span data-ttu-id="502ba-175">d.</span><span class="sxs-lookup"><span data-stu-id="502ba-175">d.</span></span> <span data-ttu-id="502ba-176">Gå till egenskapssidan för **Cerner Central** och kopiera den **program-Id** med **kopiera** knappen och klistra in den i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="502ba-176">Now go to the property page of **Cerner Central** and copy the **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_appid.png)

    <span data-ttu-id="502ba-178">e.</span><span class="sxs-lookup"><span data-stu-id="502ba-178">e.</span></span> <span data-ttu-id="502ba-179">Generera den **URL för tjänstmetadata** med hjälp av följande mönster:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span><span class="sxs-lookup"><span data-stu-id="502ba-179">Generate the **Metadata URL** using the following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span></span>

6. <span data-ttu-id="502ba-180">Konfigurera enkel inloggning på **Cerner Central** sida, måste du skicka den **URL för tjänstmetadata** till [Cerner Central support](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations).</span><span class="sxs-lookup"><span data-stu-id="502ba-180">To configure single sign-on on **Cerner Central** side, you need to send the **Metadata URL** to [Cerner Central support](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations).</span></span> <span data-ttu-id="502ba-181">De Konfigurera SSO längs program för att slutföra integrationen.</span><span class="sxs-lookup"><span data-stu-id="502ba-181">They configure the SSO on application side to complete the integration.</span></span>

> [!TIP]
> <span data-ttu-id="502ba-182">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="502ba-182">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="502ba-183">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="502ba-183">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="502ba-184">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="502ba-184">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="502ba-185">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="502ba-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="502ba-186">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="502ba-186">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span> 

![Skapa Azure AD-användare][100]

<span data-ttu-id="502ba-188">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="502ba-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="502ba-189">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="502ba-189">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="502ba-191">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="502ba-191">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="502ba-193">Öppna den **användare** dialogrutan klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="502ba-193">To open the **User** dialog, click **Add**.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="502ba-195">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="502ba-195">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="502ba-197">a.</span><span class="sxs-lookup"><span data-stu-id="502ba-197">a.</span></span> <span data-ttu-id="502ba-198">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="502ba-198">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="502ba-199">b.</span><span class="sxs-lookup"><span data-stu-id="502ba-199">b.</span></span> <span data-ttu-id="502ba-200">I den **användarnamn** textruta typ av **e-postadress** av Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="502ba-200">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="502ba-201">c.</span><span class="sxs-lookup"><span data-stu-id="502ba-201">c.</span></span> <span data-ttu-id="502ba-202">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="502ba-202">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="502ba-203">d.</span><span class="sxs-lookup"><span data-stu-id="502ba-203">d.</span></span> <span data-ttu-id="502ba-204">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="502ba-204">Click **Create**.</span></span>
 
### <a name="creating-a-cerner-central-test-user"></a><span data-ttu-id="502ba-205">Skapa en testanvändare Cerner Central</span><span class="sxs-lookup"><span data-stu-id="502ba-205">Creating a Cerner Central test user</span></span>

<span data-ttu-id="502ba-206">**Cerner Central** programmet tillåter autentisering från valfri provider som federerad identitet.</span><span class="sxs-lookup"><span data-stu-id="502ba-206">**Cerner Central** application allows authentication from any federated identity provider.</span></span> <span data-ttu-id="502ba-207">Om en användare kan logga in på programmets startsida, de har federerad och har inte behov av manuell etablering.</span><span class="sxs-lookup"><span data-stu-id="502ba-207">If a user is able to log in to the application home page, they are federated and have no need for any manual provisioning.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="502ba-208">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="502ba-208">Assigning the Azure AD test user</span></span>

<span data-ttu-id="502ba-209">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Cerner Central.</span><span class="sxs-lookup"><span data-stu-id="502ba-209">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Cerner Central.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="502ba-211">**Om du vill tilldela Cerner Central Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="502ba-211">**To assign Britta Simon to Cerner Central, perform the following steps:**</span></span>

1. <span data-ttu-id="502ba-212">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="502ba-212">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="502ba-214">Välj i listan med program **Cerner Central**.</span><span class="sxs-lookup"><span data-stu-id="502ba-214">In the applications list, select **Cerner Central**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_app.png) 

3. <span data-ttu-id="502ba-216">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="502ba-216">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="502ba-218">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="502ba-218">Click **Add** button.</span></span> <span data-ttu-id="502ba-219">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="502ba-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="502ba-221">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="502ba-221">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="502ba-222">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="502ba-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="502ba-223">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="502ba-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="502ba-224">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="502ba-224">Testing single sign-on</span></span>

<span data-ttu-id="502ba-225">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="502ba-225">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="502ba-226">När du klickar på panelen Cerner Central på åtkomstpanelen du ska hämta automatiskt loggat in på ditt Cerner Central program.</span><span class="sxs-lookup"><span data-stu-id="502ba-226">When you click the Cerner Central tile in the Access Panel, you should get automatically signed-on to your Cerner Central application.</span></span> <span data-ttu-id="502ba-227">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="502ba-227">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="502ba-228">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="502ba-228">Additional resources</span></span>

* [<span data-ttu-id="502ba-229">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="502ba-229">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="502ba-230">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="502ba-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_203.png

