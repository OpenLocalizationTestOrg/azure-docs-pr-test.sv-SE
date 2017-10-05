---
title: "Självstudier: Azure Active Directory-integrering med Pantheon | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Pantheon."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2c965d1-666f-44c2-b08f-b73163096374
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 3f4ac1db2ee83d9f9fcb375d0fb7c40ad21c4688
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pantheon"></a><span data-ttu-id="fd5f8-103">Självstudier: Azure Active Directory-integrering med Pantheon</span><span class="sxs-lookup"><span data-stu-id="fd5f8-103">Tutorial: Azure Active Directory integration with Pantheon</span></span>

<span data-ttu-id="fd5f8-104">I kursen får lära du att integrera Pantheon med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="fd5f8-104">In this tutorial, you learn how to integrate Pantheon with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fd5f8-105">Integrera Pantheon med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="fd5f8-105">Integrating Pantheon with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="fd5f8-106">Du kan styra i Azure AD som har åtkomst till Pantheon</span><span class="sxs-lookup"><span data-stu-id="fd5f8-106">You can control in Azure AD who has access to Pantheon</span></span>
- <span data-ttu-id="fd5f8-107">Du kan aktivera användarna att automatiskt hämta loggat in på Pantheon (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="fd5f8-107">You can enable your users to automatically get signed-on to Pantheon (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="fd5f8-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fd5f8-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="fd5f8-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fd5f8-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fd5f8-110">Krav</span><span class="sxs-lookup"><span data-stu-id="fd5f8-110">Prerequisites</span></span>

<span data-ttu-id="fd5f8-111">För att konfigurera Azure AD-integrering med Pantheon, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="fd5f8-111">To configure Azure AD integration with Pantheon, you need the following items:</span></span>

- <span data-ttu-id="fd5f8-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="fd5f8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="fd5f8-113">En Pantheon enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="fd5f8-113">A Pantheon single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fd5f8-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fd5f8-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="fd5f8-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fd5f8-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fd5f8-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fd5f8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fd5f8-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="fd5f8-118">Scenario description</span></span>
<span data-ttu-id="fd5f8-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fd5f8-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="fd5f8-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fd5f8-121">Att lägga till Pantheon från galleriet</span><span class="sxs-lookup"><span data-stu-id="fd5f8-121">Adding Pantheon from the gallery</span></span>
2. <span data-ttu-id="fd5f8-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fd5f8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pantheon-from-the-gallery"></a><span data-ttu-id="fd5f8-123">Att lägga till Pantheon från galleriet</span><span class="sxs-lookup"><span data-stu-id="fd5f8-123">Adding Pantheon from the gallery</span></span>
<span data-ttu-id="fd5f8-124">Du måste lägga till Pantheon från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Pantheon i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-124">To configure the integration of Pantheon into Azure AD, you need to add Pantheon from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="fd5f8-125">**Utför följande steg för att lägga till Pantheon från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="fd5f8-125">**To add Pantheon from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="fd5f8-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="fd5f8-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="fd5f8-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="fd5f8-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="fd5f8-133">I sökrutan skriver **Pantheon**.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-133">In the search box, type **Pantheon**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_search.png)

5. <span data-ttu-id="fd5f8-135">Välj i resultatpanelen **Pantheon**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-135">In the results panel, select **Pantheon**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="fd5f8-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fd5f8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="fd5f8-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Pantheon baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-138">In this section, you configure and test Azure AD single sign-on with Pantheon based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="fd5f8-139">Azure AD måste du känna till användaren i Pantheon motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Pantheon is to a user in Azure AD.</span></span> <span data-ttu-id="fd5f8-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Pantheon upprättas.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-140">In other words, a link relationship between an Azure AD user and the related user in Pantheon needs to be established.</span></span>

<span data-ttu-id="fd5f8-141">I Pantheon, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-141">In Pantheon, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="fd5f8-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Pantheon, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="fd5f8-142">To configure and test Azure AD single sign-on with Pantheon, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="fd5f8-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="fd5f8-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fd5f8-145">**[Skapa en testanvändare Pantheon](#creating-a-pantheon-test-user)**  – du har en motsvarighet för Britta Simon i Pantheon som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-145">**[Creating a Pantheon test user](#creating-a-pantheon-test-user)** - to have a counterpart of Britta Simon in Pantheon that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="fd5f8-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fd5f8-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="fd5f8-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fd5f8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="fd5f8-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Pantheon program.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Pantheon application.</span></span>

<span data-ttu-id="fd5f8-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Pantheon:**</span><span class="sxs-lookup"><span data-stu-id="fd5f8-150">**To configure Azure AD single sign-on with Pantheon, perform the following steps:**</span></span>

1. <span data-ttu-id="fd5f8-151">I Azure-portalen på den **Pantheon** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-151">In the Azure portal, on the **Pantheon** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="fd5f8-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_samlbase.png)

3. <span data-ttu-id="fd5f8-155">På den **Pantheon domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="fd5f8-155">On the **Pantheon Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_url.png)

    <span data-ttu-id="fd5f8-157">a.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-157">a.</span></span> <span data-ttu-id="fd5f8-158">I den **identifierare** textruta Skriv en URL med följande mönster:`urn:auth0:pantheon:<orgname>-SSO`</span><span class="sxs-lookup"><span data-stu-id="fd5f8-158">In the **Identifier** textbox, type a URL using the following pattern: `urn:auth0:pantheon:<orgname>-SSO`</span></span>

    <span data-ttu-id="fd5f8-159">b.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-159">b.</span></span> <span data-ttu-id="fd5f8-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://pantheon.auth0.com/login/callback?connection=<orgname>-SSO`</span><span class="sxs-lookup"><span data-stu-id="fd5f8-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://pantheon.auth0.com/login/callback?connection=<orgname>-SSO`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="fd5f8-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-161">These values are not real.</span></span> <span data-ttu-id="fd5f8-162">Uppdatera dessa värden med den faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="fd5f8-163">Kontakta [Pantheon supportteamet](https://pantheon.io/docs/getting-support/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-163">Contact [Pantheon support team](https://pantheon.io/docs/getting-support/) to get these values.</span></span>

4. <span data-ttu-id="fd5f8-164">Pantheon program förväntar SAML-kontroll i specifika format, vilket kräver att du anger värdet på attributet UserIdentifier med användarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-164">Pantheon application expects the SAML assertion in specific format, which requires you to set the UserIdentifier attribute value with the user’s email address.</span></span> <span data-ttu-id="fd5f8-165">Som standard använder Azure AD UserPrincipalName för UserIdentifier attribut.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-165">By default Azure AD uses the UserPrincipalName for UserIdentifier attribute.</span></span> <span data-ttu-id="fd5f8-166">Men för lyckad integration måste du justera det här värdet som matchar användarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-166">But for successful integration you need to adjust this value to match with user’s email address.</span></span> <span data-ttu-id="fd5f8-167">Integrationen fungerar endast när du har gjort korrekt mappning.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-167">The integration will only work after doing the correct mapping.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pantheon-tutorial/tutorial_attribute.png)  


5. <span data-ttu-id="fd5f8-169">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-169">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_certificate.png)

6. <span data-ttu-id="fd5f8-171">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-171">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pantheon-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="fd5f8-173">På den **Pantheon Configuration** klickar du på **konfigurera Pantheon** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-173">On the **Pantheon Configuration** section, click **Configure Pantheon** to open **Configure sign-on** window.</span></span> <span data-ttu-id="fd5f8-174">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="fd5f8-174">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_configure.png) 

8. <span data-ttu-id="fd5f8-176">Konfigurera enkel inloggning på **Pantheon** sida, måste du skicka den hämtade **certifikat** och **SAML enkel inloggning Tjänstwebbadress** till [Pantheon supportteam](https://pantheon.io/docs/getting-support/).</span><span class="sxs-lookup"><span data-stu-id="fd5f8-176">To configure single sign-on on **Pantheon** side, you need to send the downloaded **Certificate** and **SAML Single Sign-On Service URL** to [Pantheon support team](https://pantheon.io/docs/getting-support/).</span></span>

     > [!Note]
     > <span data-ttu-id="fd5f8-177">Du måste ange information om e-domäner och datum och tid när du vill aktivera den här anslutningen.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-177">You also need to provide the Email Domain(s) information and Date Time when you want to enable this connection.</span></span> <span data-ttu-id="fd5f8-178">Du hittar mer information om den från [här](https://pantheon.io/docs/sso-organizations/)</span><span class="sxs-lookup"><span data-stu-id="fd5f8-178">You can find more details about it from [here](https://pantheon.io/docs/sso-organizations/)</span></span>

> [!TIP]
> <span data-ttu-id="fd5f8-179">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="fd5f8-179">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="fd5f8-180">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-180">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="fd5f8-181">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fd5f8-181">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="fd5f8-182">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="fd5f8-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="fd5f8-183">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-183">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="fd5f8-185">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="fd5f8-185">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="fd5f8-186">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-186">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pantheon-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="fd5f8-188">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-188">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pantheon-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="fd5f8-190">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-190">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pantheon-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="fd5f8-192">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="fd5f8-192">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pantheon-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="fd5f8-194">a.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-194">a.</span></span> <span data-ttu-id="fd5f8-195">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-195">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fd5f8-196">b.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-196">b.</span></span> <span data-ttu-id="fd5f8-197">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-197">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="fd5f8-198">c.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-198">c.</span></span> <span data-ttu-id="fd5f8-199">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-199">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="fd5f8-200">d.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-200">d.</span></span> <span data-ttu-id="fd5f8-201">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-201">Click **Create**.</span></span>
 
### <a name="creating-a-pantheon-test-user"></a><span data-ttu-id="fd5f8-202">Skapa en testanvändare Pantheon</span><span class="sxs-lookup"><span data-stu-id="fd5f8-202">Creating a Pantheon test user</span></span>

<span data-ttu-id="fd5f8-203">I det här avsnittet skapar du en användare som kallas Britta Simon i Pantheon.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-203">In this section, you create a user called Britta Simon in Pantheon.</span></span> <span data-ttu-id="fd5f8-204">Följ de nedanstående steg för att lägga till användaren i Pantheon.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-204">Please follow the below steps to add the user in Pantheon.</span></span> 

>[!NOTE] 
><span data-ttu-id="fd5f8-205">För enkel inloggning ska fungera användare behöver skapas först i Pantheon.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-205">For SSO to work user needs to be created first in Pantheon.</span></span>

1. <span data-ttu-id="fd5f8-206">Logga in på Pantheon med autentiseringsuppgifter som administratör.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-206">Login to Pantheon with admin credentials.</span></span>

2. <span data-ttu-id="fd5f8-207">Gå till **organisation** instrumentpanelens sida.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-207">Navigate to **Organization** dashboard page.</span></span>
 
3. <span data-ttu-id="fd5f8-208">Klicka på **personer**.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-208">Click **People**.</span></span>

4. <span data-ttu-id="fd5f8-209">Klicka på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-209">Click **Add user**.</span></span>

5. <span data-ttu-id="fd5f8-210">Ange användarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-210">Enter the user's email address.</span></span>

6. <span data-ttu-id="fd5f8-211">Välj användarens roll.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-211">Choose the user's role.</span></span>

7. <span data-ttu-id="fd5f8-212">Klicka på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-212">Click **Add user**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="fd5f8-213">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="fd5f8-213">Assigning the Azure AD test user</span></span>

<span data-ttu-id="fd5f8-214">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Pantheon.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-214">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Pantheon.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="fd5f8-216">**Om du vill tilldela Pantheon Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="fd5f8-216">**To assign Britta Simon to Pantheon, perform the following steps:**</span></span>

1. <span data-ttu-id="fd5f8-217">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-217">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="fd5f8-219">Välj i listan med program **Pantheon**.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-219">In the applications list, select **Pantheon**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_app.png) 

3. <span data-ttu-id="fd5f8-221">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-221">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="fd5f8-223">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-223">Click **Add** button.</span></span> <span data-ttu-id="fd5f8-224">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="fd5f8-226">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-226">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="fd5f8-227">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fd5f8-228">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="fd5f8-229">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fd5f8-229">Testing single sign-on</span></span>

<span data-ttu-id="fd5f8-230">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-230">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="fd5f8-231">När du klickar på panelen Pantheon på åtkomstpanelen du bör få automatiskt loggat in på ditt Pantheon program.</span><span class="sxs-lookup"><span data-stu-id="fd5f8-231">When you click the Pantheon tile in the Access Panel, you should get automatically signed-on to your Pantheon application.</span></span>
<span data-ttu-id="fd5f8-232">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="fd5f8-232">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="fd5f8-233">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="fd5f8-233">Additional resources</span></span>

* [<span data-ttu-id="fd5f8-234">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fd5f8-234">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fd5f8-235">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="fd5f8-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_203.png

