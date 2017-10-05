---
title: "Självstudier: Azure Active Directory-integrering med Kantega SSO för Bitbucket | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Kantega SSO för Bitbucket."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c41cdaaf-0441-493c-94c7-569615b7b1ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 6656c9abf8483ee98c0cb1a16c06d078e32240f2
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-bitbucket"></a><span data-ttu-id="cfa64-103">Självstudier: Azure Active Directory-integrering med Kantega SSO för Bitbucket</span><span class="sxs-lookup"><span data-stu-id="cfa64-103">Tutorial: Azure Active Directory integration with Kantega SSO for Bitbucket</span></span>

<span data-ttu-id="cfa64-104">I kursen får lära du att integrera Kantega SSO för Bitbucket med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="cfa64-104">In this tutorial, you learn how to integrate Kantega SSO for Bitbucket with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cfa64-105">Integrera Kantega SSO för Bitbucket med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="cfa64-105">Integrating Kantega SSO for Bitbucket with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="cfa64-106">Du kan styra i Azure AD som har åtkomst till Kantega SSO för Bitbucket</span><span class="sxs-lookup"><span data-stu-id="cfa64-106">You can control in Azure AD who has access to Kantega SSO for Bitbucket</span></span>
- <span data-ttu-id="cfa64-107">Du kan aktivera användarna att automatiskt hämta loggat in på Kantega SSO för Bitbucket (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="cfa64-107">You can enable your users to automatically get signed-on to Kantega SSO for Bitbucket (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cfa64-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="cfa64-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="cfa64-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cfa64-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cfa64-110">Krav</span><span class="sxs-lookup"><span data-stu-id="cfa64-110">Prerequisites</span></span>

<span data-ttu-id="cfa64-111">För att konfigurera Azure AD-integrering med Kantega SSO för Bitbucket, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="cfa64-111">To configure Azure AD integration with Kantega SSO for Bitbucket, you need the following items:</span></span>

- <span data-ttu-id="cfa64-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="cfa64-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cfa64-113">En Kantega SSO för Bitbucket enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="cfa64-113">A Kantega SSO for Bitbucket single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cfa64-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="cfa64-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cfa64-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="cfa64-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cfa64-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="cfa64-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cfa64-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cfa64-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cfa64-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="cfa64-118">Scenario description</span></span>
<span data-ttu-id="cfa64-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="cfa64-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cfa64-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="cfa64-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cfa64-121">Att lägga till Kantega SSO för Bitbucket från galleriet</span><span class="sxs-lookup"><span data-stu-id="cfa64-121">Adding Kantega SSO for Bitbucket from the gallery</span></span>
2. <span data-ttu-id="cfa64-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cfa64-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kantega-sso-for-bitbucket-from-the-gallery"></a><span data-ttu-id="cfa64-123">Att lägga till Kantega SSO för Bitbucket från galleriet</span><span class="sxs-lookup"><span data-stu-id="cfa64-123">Adding Kantega SSO for Bitbucket from the gallery</span></span>
<span data-ttu-id="cfa64-124">Du måste lägga till Kantega SSO för Bitbucket från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Kantega SSO för Bitbucket i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cfa64-124">To configure the integration of Kantega SSO for Bitbucket into Azure AD, you need to add Kantega SSO for Bitbucket from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="cfa64-125">**Utför följande steg för att lägga till Kantega SSO för Bitbucket från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="cfa64-125">**To add Kantega SSO for Bitbucket from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="cfa64-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cfa64-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cfa64-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="cfa64-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="cfa64-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cfa64-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="cfa64-133">I sökrutan skriver **Kantega SSO för Bitbucket**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-133">In the search box, type **Kantega SSO for Bitbucket**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_search.png)

5. <span data-ttu-id="cfa64-135">Välj i resultatpanelen **Kantega SSO för Bitbucket**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="cfa64-135">In the results panel, select **Kantega SSO for Bitbucket**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cfa64-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cfa64-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cfa64-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Kantega SSO för Bitbucket baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="cfa64-138">In this section, you configure and test Azure AD single sign-on with Kantega SSO for Bitbucket based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="cfa64-139">Azure AD måste du känna till motsvarande användaren Kantega SSO för Bitbucket till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="cfa64-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Kantega SSO for Bitbucket is to a user in Azure AD.</span></span> <span data-ttu-id="cfa64-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren Kantega SSO för Bitbucket upprättas.</span><span class="sxs-lookup"><span data-stu-id="cfa64-140">In other words, a link relationship between an Azure AD user and the related user in Kantega SSO for Bitbucket needs to be established.</span></span>

<span data-ttu-id="cfa64-141">Kantega SSO för Bitbucket, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="cfa64-141">In Kantega SSO for Bitbucket, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="cfa64-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Kantega SSO för Bitbucket, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="cfa64-142">To configure and test Azure AD single sign-on with Kantega SSO for Bitbucket, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="cfa64-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="cfa64-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="cfa64-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cfa64-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cfa64-145">**[Skapa en Kantega SSO för Bitbucket testanvändare](#creating-a-kantega-sso-for-bitbucket-test-user)**  – du har en motsvarighet för Britta Simon Kantega SSO för Bitbucket som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="cfa64-145">**[Creating a Kantega SSO for Bitbucket test user](#creating-a-kantega-sso-for-bitbucket-test-user)** - to have a counterpart of Britta Simon in Kantega SSO for Bitbucket that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="cfa64-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cfa64-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cfa64-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="cfa64-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cfa64-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cfa64-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cfa64-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i din Kantega SSO för Bitbucket-program.</span><span class="sxs-lookup"><span data-stu-id="cfa64-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Kantega SSO for Bitbucket application.</span></span>

<span data-ttu-id="cfa64-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Kantega SSO för Bitbucket:**</span><span class="sxs-lookup"><span data-stu-id="cfa64-150">**To configure Azure AD single sign-on with Kantega SSO for Bitbucket, perform the following steps:**</span></span>

1. <span data-ttu-id="cfa64-151">I Azure-portalen på den **Kantega SSO för Bitbucket** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-151">In the Azure portal, on the **Kantega SSO for Bitbucket** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="cfa64-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cfa64-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_samlbase.png)

3. <span data-ttu-id="cfa64-155">I **IDP** initieras läge på den **Kantega SSO Bitbucket domän och URL: er** avsnittet utföra följande steg:</span><span class="sxs-lookup"><span data-stu-id="cfa64-155">In **IDP** initiated mode, on the **Kantega SSO for Bitbucket Domain and URLs** section perform the following step:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_url1.png)

    <span data-ttu-id="cfa64-157">a.</span><span class="sxs-lookup"><span data-stu-id="cfa64-157">a.</span></span> <span data-ttu-id="cfa64-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="cfa64-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

    <span data-ttu-id="cfa64-159">b.</span><span class="sxs-lookup"><span data-stu-id="cfa64-159">b.</span></span> <span data-ttu-id="cfa64-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="cfa64-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

4. <span data-ttu-id="cfa64-161">I **SP** initierade läge, kontrollera **visa avancerade inställningar för URL: en** och utföra följande steg:</span><span class="sxs-lookup"><span data-stu-id="cfa64-161">In **SP** initiated mode, check **Show advanced URL settings** and  perform the following step:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_url2.png)
    
    <span data-ttu-id="cfa64-163">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="cfa64-163">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="cfa64-164">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="cfa64-164">These values are not real.</span></span> <span data-ttu-id="cfa64-165">Uppdatera dessa värden med den faktiska identifierare Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="cfa64-165">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="cfa64-166">Dessa värden tas emot under konfigurationen av Bitbucket plugin-programmet som beskrivs senare i självstudierna.</span><span class="sxs-lookup"><span data-stu-id="cfa64-166">These values are recieved during the configuration of Bitbucket plugin which is explained later in the tutorial.</span></span>

5. <span data-ttu-id="cfa64-167">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="cfa64-167">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_certificate.png) 

6. <span data-ttu-id="cfa64-169">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="cfa64-169">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="cfa64-171">I en annan webbläsarfönster logga du in på administrationsportalen Bitbucket som administratör.</span><span class="sxs-lookup"><span data-stu-id="cfa64-171">In a different web browser window, log in to your Bitbucket admin portal as an administrator.</span></span>

8. <span data-ttu-id="cfa64-172">Klicka på kugghjulet och på den **söka efter nya tillägg**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-172">Click cog and click the **Find new add-ons**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon1.png)

9. <span data-ttu-id="cfa64-174">Sök **Kantega SSO för Bitbucket SAML & Kerberos** och på **installera** för att installera den nya SAML-plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="cfa64-174">Search **Kantega SSO for Bitbucket SAML & Kerberos** and click **Install** button to install the new SAML plugin.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon2.png)

10. <span data-ttu-id="cfa64-176">Plugin-installationen startar.</span><span class="sxs-lookup"><span data-stu-id="cfa64-176">The plugin installation starts.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon31.png)

11. <span data-ttu-id="cfa64-178">När installationen är klar.</span><span class="sxs-lookup"><span data-stu-id="cfa64-178">Once the installation is complete.</span></span> <span data-ttu-id="cfa64-179">Klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-179">Click **Close**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon33.png)

12. <span data-ttu-id="cfa64-181">Klicka på **Hantera**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-181">Click **Manage**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon34.png)
    
13. <span data-ttu-id="cfa64-183">Klicka på **konfigurera** att konfigurera nya plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="cfa64-183">Click **Configure** to configure the new plugin.</span></span>    

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon35.png)

14. <span data-ttu-id="cfa64-185">I den **SAML** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="cfa64-185">In the **SAML** section.</span></span> <span data-ttu-id="cfa64-186">Välj **Azure Active Directory (AD Azure)** från den **Lägg till identitetsleverantör** listrutan.</span><span class="sxs-lookup"><span data-stu-id="cfa64-186">Select **Azure Active Directory (Azure AD)** from the **Add identity provider** dropdown.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon4.png)

15. <span data-ttu-id="cfa64-188">Välj prenumerationsnivån som **grundläggande**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-188">Select subscription level as **Basic**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon5.png)

16. <span data-ttu-id="cfa64-190">På den **appegenskaper** avsnittet, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="cfa64-190">On the **App properties** section, perform following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon6.png)

    <span data-ttu-id="cfa64-192">a.</span><span class="sxs-lookup"><span data-stu-id="cfa64-192">a.</span></span> <span data-ttu-id="cfa64-193">Kopiera den **App-ID URI** värde och använda det som **identifierare, Reply URL och inloggnings-URL** på den **Kantega SSO Bitbucket domän och URL: er** avsnitt i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cfa64-193">Copy the **App ID URI** value and use it as **Identifier, Reply URL, and Sign-On URL** on the **Kantega SSO for Bitbucket Domain and URLs** section in Azure portal.</span></span>

    <span data-ttu-id="cfa64-194">b.</span><span class="sxs-lookup"><span data-stu-id="cfa64-194">b.</span></span> <span data-ttu-id="cfa64-195">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-195">Click **Next**.</span></span>

17. <span data-ttu-id="cfa64-196">På den **Metadata importera** avsnittet, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="cfa64-196">On the **Metadata import** section, perform following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon7.png)

    <span data-ttu-id="cfa64-198">a.</span><span class="sxs-lookup"><span data-stu-id="cfa64-198">a.</span></span> <span data-ttu-id="cfa64-199">Välj **metadatafil på datorn**, och överföra metadata-fil som du har hämtat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cfa64-199">Select **Metadata file on my computer**, and upload metadata file, which you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="cfa64-200">b.</span><span class="sxs-lookup"><span data-stu-id="cfa64-200">b.</span></span> <span data-ttu-id="cfa64-201">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-201">Click **Next**.</span></span>

18. <span data-ttu-id="cfa64-202">På den **och enkel inloggning** avsnittet, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="cfa64-202">On the **Name and SSO location** section, perform following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon8.png)

    <span data-ttu-id="cfa64-204">a.</span><span class="sxs-lookup"><span data-stu-id="cfa64-204">a.</span></span> <span data-ttu-id="cfa64-205">Lägg till namnet på den identitetsleverantör i **identitet providernamn** textruta (t.ex Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cfa64-205">Add Name of the Identity Provider in **Identity provider name** textbox (e.g Azure AD).</span></span>

    <span data-ttu-id="cfa64-206">b.</span><span class="sxs-lookup"><span data-stu-id="cfa64-206">b.</span></span> <span data-ttu-id="cfa64-207">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-207">Click **Next**.</span></span>

19. <span data-ttu-id="cfa64-208">Kontrollera signeringscertifikatet och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-208">Verify the Signing certificate and click **Next**.</span></span>  

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon9.png)

20. <span data-ttu-id="cfa64-210">På den **Bitbucket användarkonton** avsnittet, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="cfa64-210">On the **Bitbucket user accounts** section, perform following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon10.png)

    <span data-ttu-id="cfa64-212">a.</span><span class="sxs-lookup"><span data-stu-id="cfa64-212">a.</span></span> <span data-ttu-id="cfa64-213">Välj **skapa användare i Bitbuckets interna katalogen om det behövs** och ange rätt namn i gruppen för användare (kan vara flera Nej.</span><span class="sxs-lookup"><span data-stu-id="cfa64-213">Select **Create users in Bitbucket's internal Directory if needed** and enter the appropriate name of the group for users (can be multiple no.</span></span> <span data-ttu-id="cfa64-214">av (grupper avgränsade med kommatecken).</span><span class="sxs-lookup"><span data-stu-id="cfa64-214">of groups separated by comma).</span></span>

    <span data-ttu-id="cfa64-215">b.</span><span class="sxs-lookup"><span data-stu-id="cfa64-215">b.</span></span> <span data-ttu-id="cfa64-216">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-216">Click **Next**.</span></span>

21. <span data-ttu-id="cfa64-217">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-217">Click **Finish**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon11.png)

22. <span data-ttu-id="cfa64-219">På den **kända domäner för Azure AD** avsnittet, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="cfa64-219">On the **Known domains for Azure AD** section, perform following steps:</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon12.png)

    <span data-ttu-id="cfa64-221">a.</span><span class="sxs-lookup"><span data-stu-id="cfa64-221">a.</span></span> <span data-ttu-id="cfa64-222">Välj **kända domäner** från den vänstra panelen på sidan.</span><span class="sxs-lookup"><span data-stu-id="cfa64-222">Select **Known domains** from the left panel of the page.</span></span>

    <span data-ttu-id="cfa64-223">b.</span><span class="sxs-lookup"><span data-stu-id="cfa64-223">b.</span></span> <span data-ttu-id="cfa64-224">Ange domännamnet i den **kända domäner** textruta.</span><span class="sxs-lookup"><span data-stu-id="cfa64-224">Enter domain name in the **Known domains** textbox.</span></span>

    <span data-ttu-id="cfa64-225">c.</span><span class="sxs-lookup"><span data-stu-id="cfa64-225">c.</span></span> <span data-ttu-id="cfa64-226">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-226">Click **Save**.</span></span>  

> [!TIP]
> <span data-ttu-id="cfa64-227">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="cfa64-227">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="cfa64-228">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="cfa64-228">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="cfa64-229">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cfa64-229">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cfa64-230">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="cfa64-230">Creating an Azure AD test user</span></span>
<span data-ttu-id="cfa64-231">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cfa64-231">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="cfa64-233">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="cfa64-233">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="cfa64-234">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cfa64-234">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforbitbucket-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cfa64-236">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-236">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforbitbucket-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cfa64-238">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cfa64-238">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforbitbucket-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cfa64-240">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="cfa64-240">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforbitbucket-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cfa64-242">a.</span><span class="sxs-lookup"><span data-stu-id="cfa64-242">a.</span></span> <span data-ttu-id="cfa64-243">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-243">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cfa64-244">b.</span><span class="sxs-lookup"><span data-stu-id="cfa64-244">b.</span></span> <span data-ttu-id="cfa64-245">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cfa64-245">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cfa64-246">c.</span><span class="sxs-lookup"><span data-stu-id="cfa64-246">c.</span></span> <span data-ttu-id="cfa64-247">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-247">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="cfa64-248">d.</span><span class="sxs-lookup"><span data-stu-id="cfa64-248">d.</span></span> <span data-ttu-id="cfa64-249">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-249">Click **Create**.</span></span>
 
### <a name="creating-a-kantega-sso-for-bitbucket-test-user"></a><span data-ttu-id="cfa64-250">Skapa en Kantega SSO för Bitbucket testanvändare</span><span class="sxs-lookup"><span data-stu-id="cfa64-250">Creating a Kantega SSO for Bitbucket test user</span></span>

<span data-ttu-id="cfa64-251">Om du vill aktivera Azure AD-användare kan logga in på Bitbucket, måste de etableras i Bitbucket.</span><span class="sxs-lookup"><span data-stu-id="cfa64-251">To enable Azure AD users to log in to Bitbucket, they must be provisioned into Bitbucket.</span></span> <span data-ttu-id="cfa64-252">Kantega SSO för Bitbucket är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="cfa64-252">In Kantega SSO for Bitbucket, provisioning is a manual task.</span></span>

<span data-ttu-id="cfa64-253">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="cfa64-253">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="cfa64-254">Logga in på webbplatsen Bitbucket företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="cfa64-254">Log in to your Bitbucket company site as an administrator.</span></span>

2. <span data-ttu-id="cfa64-255">Klicka på inställningsikonen.</span><span class="sxs-lookup"><span data-stu-id="cfa64-255">Click on settings icon.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-kantegassoforbitbucket-tutorial/user1.png) 

3. <span data-ttu-id="cfa64-257">Under **Administration** avsnittet klickar du på **användare**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-257">Under **Administration** tab section, click **Users**.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-kantegassoforbitbucket-tutorial/user2.png)

4. <span data-ttu-id="cfa64-259">Klicka på **skapa användare**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-259">Click **Create user**.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-kantegassoforbitbucket-tutorial/user3.png)     

5. <span data-ttu-id="cfa64-261">På den **skapa användare** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="cfa64-261">On the **Create User** dialog page, perform the following steps:</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-kantegassoforbitbucket-tutorial/user4.png) 

    <span data-ttu-id="cfa64-263">a.</span><span class="sxs-lookup"><span data-stu-id="cfa64-263">a.</span></span> <span data-ttu-id="cfa64-264">I den **användarnamn** textruta, ange den e-posten för användare som Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="cfa64-264">In the **Username** textbox, type the email of user like Brittasimon@contoso.com.</span></span>
    
    <span data-ttu-id="cfa64-265">b.</span><span class="sxs-lookup"><span data-stu-id="cfa64-265">b.</span></span> <span data-ttu-id="cfa64-266">I den **fullständiga namn** textruta fullständiga typnamnet för användaren som Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cfa64-266">In the **Full Name** textbox, type full name of the user like Britta Simon.</span></span>
    
    <span data-ttu-id="cfa64-267">c.</span><span class="sxs-lookup"><span data-stu-id="cfa64-267">c.</span></span> <span data-ttu-id="cfa64-268">I den **e-postadress** textruta typen e-postadressen för användaren som Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="cfa64-268">In the **Email address** textbox, type the email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="cfa64-269">d.</span><span class="sxs-lookup"><span data-stu-id="cfa64-269">d.</span></span> <span data-ttu-id="cfa64-270">I den **lösenord** textruta skriver du lösenordet för användaren.</span><span class="sxs-lookup"><span data-stu-id="cfa64-270">In the **Password** textbox, type the password of user.</span></span>  

    <span data-ttu-id="cfa64-271">e.</span><span class="sxs-lookup"><span data-stu-id="cfa64-271">e.</span></span> <span data-ttu-id="cfa64-272">I den **Bekräfta lösenord** textruta ditt lösenord för användaren.</span><span class="sxs-lookup"><span data-stu-id="cfa64-272">In the **Confirm Password** textbox, reenter the password of user.</span></span>

    <span data-ttu-id="cfa64-273">f.</span><span class="sxs-lookup"><span data-stu-id="cfa64-273">f.</span></span> <span data-ttu-id="cfa64-274">Klicka på **skapa användare**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-274">Click **Create user**.</span></span>   

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="cfa64-275">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="cfa64-275">Assigning the Azure AD test user</span></span>

<span data-ttu-id="cfa64-276">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Kantega SSO för Bitbucket.</span><span class="sxs-lookup"><span data-stu-id="cfa64-276">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Kantega SSO for Bitbucket.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="cfa64-278">**Om du vill tilldela Kantega SSO för Bitbucket Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="cfa64-278">**To assign Britta Simon to Kantega SSO for Bitbucket, perform the following steps:**</span></span>

1. <span data-ttu-id="cfa64-279">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-279">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="cfa64-281">Välj i listan med program **Kantega SSO för Bitbucket**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-281">In the applications list, select **Kantega SSO for Bitbucket**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_app.png) 

3. <span data-ttu-id="cfa64-283">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="cfa64-283">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="cfa64-285">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="cfa64-285">Click **Add** button.</span></span> <span data-ttu-id="cfa64-286">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cfa64-286">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="cfa64-288">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="cfa64-288">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="cfa64-289">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cfa64-289">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cfa64-290">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cfa64-290">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cfa64-291">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cfa64-291">Testing single sign-on</span></span>

<span data-ttu-id="cfa64-292">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="cfa64-292">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="cfa64-293">När du klickar på Kantega SSO för Bitbucket-panelen på åtkomstpanelen du bör få automatiskt loggat in på ditt Kantega SSO för Bitbucket-program.</span><span class="sxs-lookup"><span data-stu-id="cfa64-293">When you click the Kantega SSO for Bitbucket tile in the Access Panel, you should get automatically signed-on to your Kantega SSO for Bitbucket application.</span></span>
<span data-ttu-id="cfa64-294">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="cfa64-294">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="cfa64-295">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="cfa64-295">Additional resources</span></span>

* [<span data-ttu-id="cfa64-296">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cfa64-296">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cfa64-297">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cfa64-297">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_203.png

