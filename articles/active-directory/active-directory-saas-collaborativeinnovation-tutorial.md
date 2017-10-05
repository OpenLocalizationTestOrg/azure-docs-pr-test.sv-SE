---
title: "Självstudier: Azure Active Directory-integrering med samarbetsfunktioner Innovation | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och samarbetsfunktioner Innovation."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bba95df3-75a4-4a93-8805-b3a8aa3d4861
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: 5706ba9f4e7c92de77a0edc5146aa150de379c9f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-collaborative-innovation"></a><span data-ttu-id="e80ed-103">Självstudier: Azure Active Directory-integrering med samarbetsfunktioner Innovation</span><span class="sxs-lookup"><span data-stu-id="e80ed-103">Tutorial: Azure Active Directory integration with Collaborative Innovation</span></span>

<span data-ttu-id="e80ed-104">I kursen får lära du att integrera samarbetsfunktioner Innovation med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e80ed-104">In this tutorial, you learn how to integrate Collaborative Innovation with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e80ed-105">Integrera samarbetsfunktioner Innovation med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e80ed-105">Integrating Collaborative Innovation with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e80ed-106">Du kan styra i Azure AD som har åtkomst till gemensamma Innovation</span><span class="sxs-lookup"><span data-stu-id="e80ed-106">You can control in Azure AD who has access to Collaborative Innovation</span></span>
- <span data-ttu-id="e80ed-107">Du kan aktivera användarna att automatiskt hämta inloggade samarbetsfunktioner innovation (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="e80ed-107">You can enable your users to automatically get signed-on to Collaborative Innovation (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e80ed-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e80ed-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e80ed-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e80ed-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e80ed-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e80ed-110">Prerequisites</span></span>

<span data-ttu-id="e80ed-111">För att konfigurera Azure AD-integrering med samarbetsfunktioner Innovation, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="e80ed-111">To configure Azure AD integration with Collaborative Innovation, you need the following items:</span></span>

- <span data-ttu-id="e80ed-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e80ed-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e80ed-113">En gemensam Innovation enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="e80ed-113">A Collaborative Innovation single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e80ed-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e80ed-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e80ed-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="e80ed-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e80ed-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="e80ed-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e80ed-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e80ed-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e80ed-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="e80ed-118">Scenario description</span></span>
<span data-ttu-id="e80ed-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="e80ed-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e80ed-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="e80ed-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e80ed-121">Att lägga till samarbetsfunktioner Innovation från galleriet</span><span class="sxs-lookup"><span data-stu-id="e80ed-121">Adding Collaborative Innovation from the gallery</span></span>
2. <span data-ttu-id="e80ed-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e80ed-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-collaborative-innovation-from-the-gallery"></a><span data-ttu-id="e80ed-123">Att lägga till samarbetsfunktioner Innovation från galleriet</span><span class="sxs-lookup"><span data-stu-id="e80ed-123">Adding Collaborative Innovation from the gallery</span></span>
<span data-ttu-id="e80ed-124">Du måste lägga till samarbetsfunktioner Innovation från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av samarbetsfunktioner Innovation i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e80ed-124">To configure the integration of Collaborative Innovation into Azure AD, you need to add Collaborative Innovation from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e80ed-125">**Utför följande steg för att lägga till samarbetsfunktioner Innovation från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="e80ed-125">**To add Collaborative Innovation from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e80ed-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e80ed-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e80ed-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="e80ed-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e80ed-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e80ed-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="e80ed-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e80ed-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="e80ed-133">I sökrutan skriver **samarbetsfunktioner Innovation**.</span><span class="sxs-lookup"><span data-stu-id="e80ed-133">In the search box, type **Collaborative Innovation**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_search.png)

5. <span data-ttu-id="e80ed-135">Välj i resultatpanelen **samarbetsfunktioner Innovation**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="e80ed-135">In the results panel, select **Collaborative Innovation**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e80ed-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e80ed-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e80ed-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med samarbetsfunktioner Innovation baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="e80ed-138">In this section, you configure and test Azure AD single sign-on with Collaborative Innovation based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e80ed-139">Azure AD måste du känna till motsvarande användaren i samarbetsfunktioner Innovation till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="e80ed-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Collaborative Innovation is to a user in Azure AD.</span></span> <span data-ttu-id="e80ed-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i samarbetsfunktioner Innovation upprättas.</span><span class="sxs-lookup"><span data-stu-id="e80ed-140">In other words, a link relationship between an Azure AD user and the related user in Collaborative Innovation needs to be established.</span></span>

<span data-ttu-id="e80ed-141">I samarbetsfunktioner Innovation, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="e80ed-141">In Collaborative Innovation, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e80ed-142">Om du vill konfigurera och testa Azure AD enkel inloggning med samarbetsfunktioner Innovation, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="e80ed-142">To configure and test Azure AD single sign-on with Collaborative Innovation, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e80ed-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e80ed-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e80ed-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e80ed-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e80ed-145">**[Skapa en testanvändare samarbetsfunktioner Innovation](#creating-a-collaborative-innovation-test-user)**  – du har en motsvarighet för Britta Simon samarbetsfunktioner innovation som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="e80ed-145">**[Creating a Collaborative Innovation test user](#creating-a-collaborative-innovation-test-user)** - to have a counterpart of Britta Simon in Collaborative Innovation that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e80ed-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e80ed-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e80ed-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="e80ed-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e80ed-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e80ed-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e80ed-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet samarbetsfunktioner Innovation.</span><span class="sxs-lookup"><span data-stu-id="e80ed-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Collaborative Innovation application.</span></span>

<span data-ttu-id="e80ed-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med samarbetsfunktioner Innovation:**</span><span class="sxs-lookup"><span data-stu-id="e80ed-150">**To configure Azure AD single sign-on with Collaborative Innovation, perform the following steps:**</span></span>

1. <span data-ttu-id="e80ed-151">I Azure-portalen på den **samarbetsfunktioner Innovation** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e80ed-151">In the Azure portal, on the **Collaborative Innovation** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="e80ed-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e80ed-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_samlbase.png)

3. <span data-ttu-id="e80ed-155">På den **samarbetsfunktioner Innovation domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e80ed-155">On the **Collaborative Innovation Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_url.png)

    <span data-ttu-id="e80ed-157">a.</span><span class="sxs-lookup"><span data-stu-id="e80ed-157">a.</span></span> <span data-ttu-id="e80ed-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<instancename>.foundry.<companyname>.com/`</span><span class="sxs-lookup"><span data-stu-id="e80ed-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<instancename>.foundry.<companyname>.com/`</span></span>

    <span data-ttu-id="e80ed-159">b.</span><span class="sxs-lookup"><span data-stu-id="e80ed-159">b.</span></span> <span data-ttu-id="e80ed-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<instancename>.foundry.<companyname>.com`</span><span class="sxs-lookup"><span data-stu-id="e80ed-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<instancename>.foundry.<companyname>.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="e80ed-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="e80ed-161">These values are not real.</span></span> <span data-ttu-id="e80ed-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="e80ed-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="e80ed-163">Kontakta [samarbetsfunktioner Innovation klienten supportteamet](https://www.unilever.com/contact/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="e80ed-163">Contact [Collaborative Innovation Client support team](https://www.unilever.com/contact/) to get these values.</span></span>  

4. <span data-ttu-id="e80ed-164">Samarbetsfunktioner Innovation program förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="e80ed-164">Collaborative Innovation application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="e80ed-165">Konfigurera följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="e80ed-165">Please configure the following claims for this application.</span></span> <span data-ttu-id="e80ed-166">Du kan hantera värden för attributen från den ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="e80ed-166">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="e80ed-167">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="e80ed-167">The following screenshot shows an example for this.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-collaborativeinnovation-tutorial/attribute.png)
    
5. <span data-ttu-id="e80ed-169">Klicka på **visa och redigera andra användarattribut** kryssrutan i den **användarattribut** avsnittet för att expandera attribut.</span><span class="sxs-lookup"><span data-stu-id="e80ed-169">Click **View and edit all other user attributes** checkbox in the **User Attributes** section to expand the attributes.</span></span> <span data-ttu-id="e80ed-170">Utför följande steg på varje visas attribut-</span><span class="sxs-lookup"><span data-stu-id="e80ed-170">Perform the following steps on each of the displayed attributes-</span></span>

    | <span data-ttu-id="e80ed-171">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="e80ed-171">Attribute Name</span></span> | <span data-ttu-id="e80ed-172">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="e80ed-172">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="e80ed-173">givenName</span><span class="sxs-lookup"><span data-stu-id="e80ed-173">givenname</span></span> | <span data-ttu-id="e80ed-174">User.givenName</span><span class="sxs-lookup"><span data-stu-id="e80ed-174">user.givenname</span></span> |
    | <span data-ttu-id="e80ed-175">Efternamn</span><span class="sxs-lookup"><span data-stu-id="e80ed-175">surname</span></span> | <span data-ttu-id="e80ed-176">User.surname</span><span class="sxs-lookup"><span data-stu-id="e80ed-176">user.surname</span></span> |
    | <span data-ttu-id="e80ed-177">e-postadress</span><span class="sxs-lookup"><span data-stu-id="e80ed-177">emailaddress</span></span> | <span data-ttu-id="e80ed-178">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="e80ed-178">user.userprincipalname</span></span> |
    | <span data-ttu-id="e80ed-179">namn</span><span class="sxs-lookup"><span data-stu-id="e80ed-179">name</span></span> | <span data-ttu-id="e80ed-180">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="e80ed-180">user.userprincipalname</span></span> |

    <span data-ttu-id="e80ed-181">a.</span><span class="sxs-lookup"><span data-stu-id="e80ed-181">a.</span></span> <span data-ttu-id="e80ed-182">Klicka på ett attribut för att öppna den **Redigera attribut** fönster.</span><span class="sxs-lookup"><span data-stu-id="e80ed-182">Click the attribute to open the **Edit Attribute** window.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-collaborativeinnovation-tutorial/url_update.png)

    <span data-ttu-id="e80ed-184">b.</span><span class="sxs-lookup"><span data-stu-id="e80ed-184">b.</span></span> <span data-ttu-id="e80ed-185">Ta bort URL-värdet från den **Namespace**.</span><span class="sxs-lookup"><span data-stu-id="e80ed-185">Delete the URL value from the **Namespace**.</span></span>
    
    <span data-ttu-id="e80ed-186">c.</span><span class="sxs-lookup"><span data-stu-id="e80ed-186">c.</span></span> <span data-ttu-id="e80ed-187">Klicka på **Ok** spara inställningen.</span><span class="sxs-lookup"><span data-stu-id="e80ed-187">Click **Ok** to save the setting.</span></span>

6. <span data-ttu-id="e80ed-188">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="e80ed-188">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_certificate.png) 

7. <span data-ttu-id="e80ed-190">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e80ed-190">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="e80ed-192">Konfigurera enkel inloggning på **samarbetsfunktioner Innovation** sida, måste du skicka den hämtade **XML-Metadata för** till [samarbetsfunktioner Innovation supportteamet](https://www.unilever.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="e80ed-192">To configure single sign-on on **Collaborative Innovation** side, you need to send the downloaded **Metadata XML** to [Collaborative Innovation support team](https://www.unilever.com/contact/).</span></span> <span data-ttu-id="e80ed-193">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="e80ed-193">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="e80ed-194">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="e80ed-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e80ed-195">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="e80ed-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e80ed-196">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e80ed-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e80ed-197">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e80ed-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="e80ed-198">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e80ed-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="e80ed-200">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="e80ed-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e80ed-201">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e80ed-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e80ed-203">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="e80ed-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e80ed-205">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e80ed-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e80ed-207">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e80ed-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e80ed-209">a.</span><span class="sxs-lookup"><span data-stu-id="e80ed-209">a.</span></span> <span data-ttu-id="e80ed-210">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e80ed-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e80ed-211">b.</span><span class="sxs-lookup"><span data-stu-id="e80ed-211">b.</span></span> <span data-ttu-id="e80ed-212">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e80ed-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e80ed-213">c.</span><span class="sxs-lookup"><span data-stu-id="e80ed-213">c.</span></span> <span data-ttu-id="e80ed-214">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="e80ed-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e80ed-215">d.</span><span class="sxs-lookup"><span data-stu-id="e80ed-215">d.</span></span> <span data-ttu-id="e80ed-216">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e80ed-216">Click **Create**.</span></span>
 
### <a name="creating-a-collaborative-innovation-test-user"></a><span data-ttu-id="e80ed-217">Skapa en testanvändare samarbetsfunktioner Innovation</span><span class="sxs-lookup"><span data-stu-id="e80ed-217">Creating a Collaborative Innovation test user</span></span>

<span data-ttu-id="e80ed-218">Om du vill aktivera Azure AD-användare kan logga in på samarbetsfunktioner Innovation, måste de etableras i samarbetsfunktioner Innovation.</span><span class="sxs-lookup"><span data-stu-id="e80ed-218">To enable Azure AD users to log in to Collaborative Innovation, they must be provisioned into Collaborative Innovation.</span></span>  

<span data-ttu-id="e80ed-219">Vid det här programmet är etablera automatisk eftersom programmet stöder bara i tid användaretablering.</span><span class="sxs-lookup"><span data-stu-id="e80ed-219">In case of this application provisioning is automatic as the application supports just in time user provisioning.</span></span> <span data-ttu-id="e80ed-220">Det finns så behöver du inte utföra några steg här.</span><span class="sxs-lookup"><span data-stu-id="e80ed-220">So there is no need to perform any steps here.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e80ed-221">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="e80ed-221">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e80ed-222">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till gemensamma Innovation.</span><span class="sxs-lookup"><span data-stu-id="e80ed-222">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Collaborative Innovation.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="e80ed-224">**Om du vill tilldela samarbetsfunktioner Innovation Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e80ed-224">**To assign Britta Simon to Collaborative Innovation, perform the following steps:**</span></span>

1. <span data-ttu-id="e80ed-225">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e80ed-225">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="e80ed-227">Välj i listan med program **samarbetsfunktioner Innovation**.</span><span class="sxs-lookup"><span data-stu-id="e80ed-227">In the applications list, select **Collaborative Innovation**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_app.png) 

3. <span data-ttu-id="e80ed-229">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="e80ed-229">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="e80ed-231">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e80ed-231">Click **Add** button.</span></span> <span data-ttu-id="e80ed-232">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e80ed-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="e80ed-234">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="e80ed-234">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e80ed-235">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e80ed-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e80ed-236">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e80ed-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e80ed-237">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e80ed-237">Testing single sign-on</span></span>

<span data-ttu-id="e80ed-238">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="e80ed-238">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="e80ed-239">Du bör få inloggningssidan för samarbetsfunktioner Innovation program när du klickar på panelen samarbetsfunktioner Innovation på åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="e80ed-239">When you click the Collaborative Innovation tile in the Access Panel, you should get login page of Collaborative Innovation application.</span></span>
<span data-ttu-id="e80ed-240">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e80ed-240">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="e80ed-241">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e80ed-241">Additional resources</span></span>

* [<span data-ttu-id="e80ed-242">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e80ed-242">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e80ed-243">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e80ed-243">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_203.png

