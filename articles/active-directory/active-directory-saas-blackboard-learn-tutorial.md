---
title: "Självstudier: Azure Active Directory-integrering med svart tavla Läs | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och svart tavla lära dig."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0b8ca505-61ea-487c-9a3e-fa50c936df0c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: jeedes
ms.openlocfilehash: c2b7638e99fa46ff41a7f2202bdf0e7a2b017c19
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-blackboard-learn"></a><span data-ttu-id="4c506-103">Självstudier: Azure Active Directory-integrering med svart tavla Läs</span><span class="sxs-lookup"><span data-stu-id="4c506-103">Tutorial: Azure Active Directory integration with Blackboard Learn</span></span>

<span data-ttu-id="4c506-104">Lär dig hur du integrerar svart tavla Läs med Azure Active Directory (AD Azure) i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="4c506-104">In this tutorial, you learn how to integrate Blackboard Learn with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4c506-105">Integrera svart tavla Läs med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="4c506-105">Integrating Blackboard Learn with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="4c506-106">Du kan styra i Azure AD som har åtkomst till svart tavla Läs</span><span class="sxs-lookup"><span data-stu-id="4c506-106">You can control in Azure AD who has access to Blackboard Learn</span></span>
- <span data-ttu-id="4c506-107">Du kan aktivera användarna att automatiskt hämta inloggade svart tavla Läs (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="4c506-107">You can enable your users to automatically get signed-on to Blackboard Learn (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4c506-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="4c506-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="4c506-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4c506-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4c506-110">Krav</span><span class="sxs-lookup"><span data-stu-id="4c506-110">Prerequisites</span></span>

<span data-ttu-id="4c506-111">Om du vill konfigurera Azure AD-integrering med svart tavla Läs behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="4c506-111">To configure Azure AD integration with Blackboard Learn, you need the following items:</span></span>

- <span data-ttu-id="4c506-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="4c506-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4c506-113">En svart tavla Läs enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="4c506-113">A Blackboard Learn single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4c506-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="4c506-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4c506-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="4c506-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4c506-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="4c506-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4c506-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4c506-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4c506-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="4c506-118">Scenario description</span></span>
<span data-ttu-id="4c506-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="4c506-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4c506-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="4c506-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4c506-121">Att lägga till svart tavla Läs från galleriet</span><span class="sxs-lookup"><span data-stu-id="4c506-121">Adding Blackboard Learn from the gallery</span></span>
2. <span data-ttu-id="4c506-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4c506-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-blackboard-learn-from-the-gallery"></a><span data-ttu-id="4c506-123">Att lägga till svart tavla Läs från galleriet</span><span class="sxs-lookup"><span data-stu-id="4c506-123">Adding Blackboard Learn from the gallery</span></span>
<span data-ttu-id="4c506-124">Du måste lägga till svart tavla Läs från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av svart tavla Lär dig i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4c506-124">To configure the integration of Blackboard Learn into Azure AD, you need to add Blackboard Learn from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="4c506-125">**Utför följande steg för att lägga till svart tavla Läs från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="4c506-125">**To add Blackboard Learn from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="4c506-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4c506-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4c506-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="4c506-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="4c506-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4c506-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="4c506-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4c506-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="4c506-133">I sökrutan skriver **svart tavla Läs**.</span><span class="sxs-lookup"><span data-stu-id="4c506-133">In the search box, type **Blackboard Learn**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_search.png)

5. <span data-ttu-id="4c506-135">Välj i resultatpanelen **svart tavla Läs**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="4c506-135">In the results panel, select **Blackboard Learn**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4c506-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4c506-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4c506-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med svart tavla Läs baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="4c506-138">In this section, you configure and test Azure AD single sign-on with Blackboard Learn based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="4c506-139">Azure AD måste du känna till motsvarande användaren i svart tavla Lär dig till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="4c506-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Blackboard Learn is to a user in Azure AD.</span></span> <span data-ttu-id="4c506-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i svart tavla Läs upprättas.</span><span class="sxs-lookup"><span data-stu-id="4c506-140">In other words, a link relationship between an Azure AD user and the related user in Blackboard Learn needs to be established.</span></span>

<span data-ttu-id="4c506-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i svart tavla Läs.</span><span class="sxs-lookup"><span data-stu-id="4c506-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Blackboard Learn.</span></span>

<span data-ttu-id="4c506-142">Om du vill konfigurera och testa Azure AD enkel inloggning med svart tavla Läs, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="4c506-142">To configure and test Azure AD single sign-on with Blackboard Learn, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="4c506-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="4c506-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="4c506-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4c506-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4c506-145">**[Skapa en svart tavla Läs testanvändare](#creating-a-blackboard-learn-test-user)**  – du har en motsvarighet för Britta Simon i svart tavla Läs som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="4c506-145">**[Creating a Blackboard Learn test user](#creating-a-blackboard-learn-test-user)** - to have a counterpart of Britta Simon in Blackboard Learn that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="4c506-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4c506-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4c506-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="4c506-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4c506-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4c506-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4c506-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet svart tavla Läs.</span><span class="sxs-lookup"><span data-stu-id="4c506-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Blackboard Learn application.</span></span>

<span data-ttu-id="4c506-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med svart tavla Läs:**</span><span class="sxs-lookup"><span data-stu-id="4c506-150">**To configure Azure AD single sign-on with Blackboard Learn, perform the following steps:**</span></span>

1. <span data-ttu-id="4c506-151">I Azure-portalen på den **svart tavla Läs** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="4c506-151">In the Azure portal, on the **Blackboard Learn** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="4c506-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4c506-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_samlbase.png)

3. <span data-ttu-id="4c506-155">På den **svart tavla Läs domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="4c506-155">On the **Blackboard Learn Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_url.png)

    <span data-ttu-id="4c506-157">a.</span><span class="sxs-lookup"><span data-stu-id="4c506-157">a.</span></span> <span data-ttu-id="4c506-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.blackboard.com/`</span><span class="sxs-lookup"><span data-stu-id="4c506-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.blackboard.com/`</span></span>

    <span data-ttu-id="4c506-159">b.</span><span class="sxs-lookup"><span data-stu-id="4c506-159">b.</span></span> <span data-ttu-id="4c506-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<subdomain>.blackboard.com/auth-saml/saml/SSO/entity-id/SAML_AD`</span><span class="sxs-lookup"><span data-stu-id="4c506-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.blackboard.com/auth-saml/saml/SSO/entity-id/SAML_AD`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="4c506-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="4c506-161">These values are not real.</span></span> <span data-ttu-id="4c506-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="4c506-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="4c506-163">Kontakta [svart tavla Läs klienten supportteamet](https://www.blackboard.com/support/index.aspx) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="4c506-163">Contact [Blackboard Learn Client support team](https://www.blackboard.com/support/index.aspx) to get these values.</span></span> 

4. <span data-ttu-id="4c506-164">Svart tavla Läs program förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="4c506-164">Blackboard Learn application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="4c506-165">Konfigurera följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="4c506-165">Configure the following claims for this application.</span></span> <span data-ttu-id="4c506-166">Du kan hantera värden för attributen från den **användarattribut** avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="4c506-166">You can manage the values of these attributes from the **User Attributes** section on application integration page.</span></span>
 <span data-ttu-id="4c506-167">Följande skärmbild visar ett exempel på hur den.</span><span class="sxs-lookup"><span data-stu-id="4c506-167">The following screenshot shows an example about it.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_attribute.png)

5. <span data-ttu-id="4c506-169">I den **användarattribut** avsnittet på **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i bilden och utför följande steg.</span><span class="sxs-lookup"><span data-stu-id="4c506-169">In the **User Attributes** section on **Single sign-on** dialog, configure SAML token attributes as shown in the image and perform the following steps.</span></span> <span data-ttu-id="4c506-170">Vi har mappats Userprincipalname som det unika användarattribut här men du kan mappa till lämpligt värde som särskiljer unikt användare i organisationen och som mappar till svart tavla Läs användarnamnfältet.</span><span class="sxs-lookup"><span data-stu-id="4c506-170">We have mapped the Userprincipalname as the unique user attribute here but you can map it to the appropriate value, which uniquely distinguishes the user in the organization and that maps to Blackboard Learn username field.</span></span>
           
    | <span data-ttu-id="4c506-171">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="4c506-171">Attribute Name</span></span> | <span data-ttu-id="4c506-172">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="4c506-172">Attribute Value</span></span> |   
    | ---------------| ----------------|
    | <span data-ttu-id="4c506-173">urn:oid:1.3.6.1.4.1.5923.1.1.1.6</span><span class="sxs-lookup"><span data-stu-id="4c506-173">urn:oid:1.3.6.1.4.1.5923.1.1.1.6</span></span> |<span data-ttu-id="4c506-174">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="4c506-174">user.userprincipalname</span></span> |

    <span data-ttu-id="4c506-175">a.</span><span class="sxs-lookup"><span data-stu-id="4c506-175">a.</span></span> <span data-ttu-id="4c506-176">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4c506-176">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="4c506-179">b.</span><span class="sxs-lookup"><span data-stu-id="4c506-179">b.</span></span> <span data-ttu-id="4c506-180">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="4c506-180">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="4c506-181">c.</span><span class="sxs-lookup"><span data-stu-id="4c506-181">c.</span></span> <span data-ttu-id="4c506-182">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="4c506-182">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="4c506-183">d.</span><span class="sxs-lookup"><span data-stu-id="4c506-183">d.</span></span> <span data-ttu-id="4c506-184">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="4c506-184">Click **Ok**.</span></span>

4. <span data-ttu-id="4c506-185">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="4c506-185">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_certificate.png)

7. <span data-ttu-id="4c506-187">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="4c506-187">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="4c506-189">På den **svart tavla Läs Configuration** klickar du på **konfigurera svart tavla Läs** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="4c506-189">On the **Blackboard Learn Configuration** section, click **Configure Blackboard Learn** to open **Configure sign-on** window.</span></span> <span data-ttu-id="4c506-190">Kopiera den **SAML enhets-ID** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="4c506-190">Copy the **SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_configure.png) 

9. <span data-ttu-id="4c506-192">Konfigurera enkel inloggning på **svart tavla Läs** sida, måste du skicka den hämtade **XML-Metadata för** och **SAML enhets-ID** till [svart tavla Läs support](https://www.blackboard.com/support/index.aspx).</span><span class="sxs-lookup"><span data-stu-id="4c506-192">To configure single sign-on on **Blackboard Learn** side, you need to send the downloaded **Metadata XML** and **SAML Entity ID** to [Blackboard Learn support](https://www.blackboard.com/support/index.aspx).</span></span>

> [!TIP]
> <span data-ttu-id="4c506-193">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="4c506-193">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="4c506-194">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="4c506-194">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="4c506-195">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4c506-195">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4c506-196">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="4c506-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="4c506-197">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4c506-197">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="4c506-199">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="4c506-199">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="4c506-200">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4c506-200">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4c506-202">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="4c506-202">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4c506-204">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4c506-204">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4c506-206">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="4c506-206">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4c506-208">a.</span><span class="sxs-lookup"><span data-stu-id="4c506-208">a.</span></span> <span data-ttu-id="4c506-209">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4c506-209">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4c506-210">b.</span><span class="sxs-lookup"><span data-stu-id="4c506-210">b.</span></span> <span data-ttu-id="4c506-211">I den **användarnamn** textruta typ av **e-postadress** av Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4c506-211">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="4c506-212">c.</span><span class="sxs-lookup"><span data-stu-id="4c506-212">c.</span></span> <span data-ttu-id="4c506-213">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="4c506-213">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="4c506-214">d.</span><span class="sxs-lookup"><span data-stu-id="4c506-214">d.</span></span> <span data-ttu-id="4c506-215">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4c506-215">Click **Create**.</span></span>
 
### <a name="creating-a-blackboard-learn-test-user"></a><span data-ttu-id="4c506-216">Skapa en svart tavla Läs testanvändare</span><span class="sxs-lookup"><span data-stu-id="4c506-216">Creating a Blackboard Learn test user</span></span>
<span data-ttu-id="4c506-217">I det här avsnittet kan du skapa en användare som kallas Britta Simon i svart tavla Läs.</span><span class="sxs-lookup"><span data-stu-id="4c506-217">In this section, you create a user called Britta Simon in Blackboard Learn.</span></span> 

<span data-ttu-id="4c506-218">Svart tavla Läs programmet stöder bara i tid användaretablering.</span><span class="sxs-lookup"><span data-stu-id="4c506-218">Blackboard Learn application support just in time user provisioning.</span></span> <span data-ttu-id="4c506-219">Kontrollera att du har konfigurerat anspråk enligt beskrivningen i avsnittet  **[konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**</span><span class="sxs-lookup"><span data-stu-id="4c506-219">Make sure that you have configured the claims as described in the section **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**</span></span>
### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="4c506-220">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="4c506-220">Assigning the Azure AD test user</span></span>

<span data-ttu-id="4c506-221">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till svart tavla Läs.</span><span class="sxs-lookup"><span data-stu-id="4c506-221">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Blackboard Learn.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="4c506-223">**Om du vill tilldela Britta Simon svart tavla Läs, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4c506-223">**To assign Britta Simon to Blackboard Learn, perform the following steps:**</span></span>

1. <span data-ttu-id="4c506-224">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4c506-224">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="4c506-226">Välj i listan med program **svart tavla Läs**.</span><span class="sxs-lookup"><span data-stu-id="4c506-226">In the applications list, select **Blackboard Learn**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_app.png) 

3. <span data-ttu-id="4c506-228">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="4c506-228">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="4c506-230">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="4c506-230">Click **Add** button.</span></span> <span data-ttu-id="4c506-231">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4c506-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="4c506-233">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="4c506-233">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="4c506-234">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4c506-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4c506-235">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4c506-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4c506-236">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4c506-236">Testing single sign-on</span></span>

<span data-ttu-id="4c506-237">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="4c506-237">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="4c506-238">När du klickar på panelen svart tavla Läs på åtkomstpanelen du ska hämta automatiskt loggat in i tillämpningsprogrammet svart tavla Läs.</span><span class="sxs-lookup"><span data-stu-id="4c506-238">When you click the Blackboard Learn tile in the Access Panel, you should get automatically signed-on to your Blackboard Learn application.</span></span> <span data-ttu-id="4c506-239">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4c506-239">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="4c506-240">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="4c506-240">Additional resources</span></span>

* [<span data-ttu-id="4c506-241">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4c506-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4c506-242">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4c506-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_203.png

