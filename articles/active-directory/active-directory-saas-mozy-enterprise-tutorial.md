---
title: "Självstudier: Azure Active Directory-integrering med Mozy Enterprise | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Mozy Enterprise."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 489b5e62-85c2-45c9-8766-326632d48114
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: jeedes
ms.openlocfilehash: ac73aadcb8205f24f9d2dbce5af76f53bbcb9753
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mozy-enterprise"></a><span data-ttu-id="0d039-103">Självstudier: Azure Active Directory-integrering med Mozy Enterprise</span><span class="sxs-lookup"><span data-stu-id="0d039-103">Tutorial: Azure Active Directory integration with Mozy Enterprise</span></span>

<span data-ttu-id="0d039-104">I kursen får lära du att integrera Mozy Enterprise med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="0d039-104">In this tutorial, you learn how to integrate Mozy Enterprise with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0d039-105">Integrera Mozy Enterprise med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="0d039-105">Integrating Mozy Enterprise with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="0d039-106">Du kan styra i Azure AD som har åtkomst till Mozy Enterprise</span><span class="sxs-lookup"><span data-stu-id="0d039-106">You can control in Azure AD who has access to Mozy Enterprise</span></span>
- <span data-ttu-id="0d039-107">Du kan aktivera användarna att automatiskt hämta loggat in på Mozy Enterprise (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="0d039-107">You can enable your users to automatically get signed-on to Mozy Enterprise (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0d039-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0d039-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="0d039-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0d039-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0d039-110">Krav</span><span class="sxs-lookup"><span data-stu-id="0d039-110">Prerequisites</span></span>

<span data-ttu-id="0d039-111">För att konfigurera Azure AD-integrering med Mozy Enterprise, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="0d039-111">To configure Azure AD integration with Mozy Enterprise, you need the following items:</span></span>

- <span data-ttu-id="0d039-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="0d039-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0d039-113">En Mozy Enterprise enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="0d039-113">A Mozy Enterprise single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0d039-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="0d039-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0d039-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="0d039-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0d039-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="0d039-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0d039-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0d039-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0d039-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="0d039-118">Scenario description</span></span>
<span data-ttu-id="0d039-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="0d039-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0d039-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="0d039-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0d039-121">Att lägga till Mozy Enterprise från galleriet</span><span class="sxs-lookup"><span data-stu-id="0d039-121">Adding Mozy Enterprise from the gallery</span></span>
2. <span data-ttu-id="0d039-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0d039-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mozy-enterprise-from-the-gallery"></a><span data-ttu-id="0d039-123">Att lägga till Mozy Enterprise från galleriet</span><span class="sxs-lookup"><span data-stu-id="0d039-123">Adding Mozy Enterprise from the gallery</span></span>
<span data-ttu-id="0d039-124">Du måste lägga till Mozy Enterprise från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Mozy Enterprise i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0d039-124">To configure the integration of Mozy Enterprise into Azure AD, you need to add Mozy Enterprise from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="0d039-125">**Utför följande steg för att lägga till Mozy Enterprise från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="0d039-125">**To add Mozy Enterprise from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="0d039-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="0d039-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0d039-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="0d039-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="0d039-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0d039-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="0d039-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0d039-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="0d039-133">I sökrutan skriver **Mozy Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="0d039-133">In the search box, type **Mozy Enterprise**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_search.png)

5. <span data-ttu-id="0d039-135">Välj i resultatpanelen **Mozy Enterprise**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="0d039-135">In the results panel, select **Mozy Enterprise**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0d039-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0d039-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0d039-138">Du konfigurera och testa Azure AD enkel inloggning med Mozy Enterprise baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="0d039-138">In this section, you configure and test Azure AD single sign-on with Mozy Enterprise based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="0d039-139">Azure AD måste du känna till användaren i Mozy Enterprise motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="0d039-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Mozy Enterprise is to a user in Azure AD.</span></span> <span data-ttu-id="0d039-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Mozy Enterprise upprättas.</span><span class="sxs-lookup"><span data-stu-id="0d039-140">In other words, a link relationship between an Azure AD user and the related user in Mozy Enterprise needs to be established.</span></span>

<span data-ttu-id="0d039-141">I Mozy Enterprise, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="0d039-141">In Mozy Enterprise, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="0d039-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Mozy Enterprise, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="0d039-142">To configure and test Azure AD single sign-on with Mozy Enterprise, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="0d039-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="0d039-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="0d039-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0d039-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0d039-145">**[Skapa en testanvändare Mozy Enterprise](#creating-a-mozy-enterprise-test-user)**  – du har en motsvarighet för Britta Simon i Mozy företag som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="0d039-145">**[Creating a Mozy Enterprise test user](#creating-a-mozy-enterprise-test-user)** - to have a counterpart of Britta Simon in Mozy Enterprise that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="0d039-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0d039-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0d039-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="0d039-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0d039-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0d039-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0d039-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i Mozy Enterprise-programmet.</span><span class="sxs-lookup"><span data-stu-id="0d039-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Mozy Enterprise application.</span></span>

<span data-ttu-id="0d039-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Mozy Enterprise:**</span><span class="sxs-lookup"><span data-stu-id="0d039-150">**To configure Azure AD single sign-on with Mozy Enterprise, perform the following steps:**</span></span>

1. <span data-ttu-id="0d039-151">I Azure-portalen på den **Mozy Enterprise** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="0d039-151">In the Azure portal, on the **Mozy Enterprise** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="0d039-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0d039-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_samlbase.png)

3. <span data-ttu-id="0d039-155">På den **Mozy företagsdomänen och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="0d039-155">On the **Mozy Enterprise Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_url.png)

    <span data-ttu-id="0d039-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<tenantname>.Mozyenterprise.com`</span><span class="sxs-lookup"><span data-stu-id="0d039-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenantname>.Mozyenterprise.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0d039-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="0d039-158">This value is not real.</span></span> <span data-ttu-id="0d039-159">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="0d039-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="0d039-160">Kontakta [Mozy Enterprise-klienten supportteamet](http://support.mozy.com/) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="0d039-160">Contact [Mozy Enterprise Client support team](http://support.mozy.com/) to get this value.</span></span>

4. <span data-ttu-id="0d039-161">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="0d039-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_certificate.png) 

5. <span data-ttu-id="0d039-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="0d039-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0d039-165">På den **Mozy företagskonfiguration** klickar du på **konfigurera Mozy Enterprise** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="0d039-165">On the **Mozy Enterprise Configuration** section, click **Configure Mozy Enterprise** to open **Configure sign-on** window.</span></span> <span data-ttu-id="0d039-166">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="0d039-166">Copy the **SAML Entity ID and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_configure.png) 

7. <span data-ttu-id="0d039-168">Logga in på webbplatsen Mozy Enterprise företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="0d039-168">In a different web browser window, log into your Mozy Enterprise company site as an administrator.</span></span>

8. <span data-ttu-id="0d039-169">I den **Configuration** klickar du på **autentiseringsprincip**.</span><span class="sxs-lookup"><span data-stu-id="0d039-169">In the **Configuration** section, click **Authentication Policy**.</span></span>
   
   <span data-ttu-id="0d039-170">![Autentiseringsprincip](./media/active-directory-saas-mozy-enterprise-tutorial/ic777314.png "autentiseringsprincip")</span><span class="sxs-lookup"><span data-stu-id="0d039-170">![Authentication policy](./media/active-directory-saas-mozy-enterprise-tutorial/ic777314.png "Authentication policy")</span></span>

9. <span data-ttu-id="0d039-171">På den **autentiseringsprincip** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="0d039-171">On the **Authentication Policy** section, perform the following steps:</span></span>
   
   <span data-ttu-id="0d039-172">![Autentiseringsprincip](./media/active-directory-saas-mozy-enterprise-tutorial/ic777315.png "autentiseringsprincip")</span><span class="sxs-lookup"><span data-stu-id="0d039-172">![Authentication policy](./media/active-directory-saas-mozy-enterprise-tutorial/ic777315.png "Authentication policy")</span></span>
   
   <span data-ttu-id="0d039-173">a.</span><span class="sxs-lookup"><span data-stu-id="0d039-173">a.</span></span> <span data-ttu-id="0d039-174">Välj **katalogtjänsten** som **Provider**.</span><span class="sxs-lookup"><span data-stu-id="0d039-174">Select **Directory Service** as **Provider**.</span></span>
   
   <span data-ttu-id="0d039-175">b.</span><span class="sxs-lookup"><span data-stu-id="0d039-175">b.</span></span> <span data-ttu-id="0d039-176">Välj **använder LDAP Push**.</span><span class="sxs-lookup"><span data-stu-id="0d039-176">Select **Use LDAP Push**.</span></span>
   
   <span data-ttu-id="0d039-177">c.</span><span class="sxs-lookup"><span data-stu-id="0d039-177">c.</span></span> <span data-ttu-id="0d039-178">Klicka på den **SAML-autentisering** fliken.</span><span class="sxs-lookup"><span data-stu-id="0d039-178">Click the **SAML Authentication** tab.</span></span>
   
   <span data-ttu-id="0d039-179">d.</span><span class="sxs-lookup"><span data-stu-id="0d039-179">d.</span></span> <span data-ttu-id="0d039-180">Klistra in **SAML enkel inloggning Tjänstwebbadress**, som du har kopierat från Azure-portalen i den **autentiserings-URL för** textruta.</span><span class="sxs-lookup"><span data-stu-id="0d039-180">Paste **SAML Single Sign-On Service URL**, which you have copied from the Azure portal into the **Authentication URL** textbox.</span></span>
   
   <span data-ttu-id="0d039-181">e.</span><span class="sxs-lookup"><span data-stu-id="0d039-181">e.</span></span> <span data-ttu-id="0d039-182">Klistra in **SAML enhets-ID**, som du har kopierat från Azure-portalen i den **SAML Endpoint** textruta.</span><span class="sxs-lookup"><span data-stu-id="0d039-182">Paste **SAML Entity ID**, which you have copied from the Azure portal into the **SAML Endpoint** textbox.</span></span>
   
   <span data-ttu-id="0d039-183">f.</span><span class="sxs-lookup"><span data-stu-id="0d039-183">f.</span></span> <span data-ttu-id="0d039-184">Öppna din hämtade Base64-kodade certifikatet i anteckningar, kopiera innehållet i den till Urklipp och klistra in hela certifikatet till **SAML certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="0d039-184">Open your downloaded base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste the entire Certificate into **SAML Certificate** textbox.</span></span>
   
   <span data-ttu-id="0d039-185">g.</span><span class="sxs-lookup"><span data-stu-id="0d039-185">g.</span></span> <span data-ttu-id="0d039-186">Välj **aktivera enkel inloggning för administratörer att logga in med sina nätverksinloggningsuppgifter**.</span><span class="sxs-lookup"><span data-stu-id="0d039-186">Select **Enable SSO for Admins to log in with their network credentials**.</span></span>
   
   <span data-ttu-id="0d039-187">h.</span><span class="sxs-lookup"><span data-stu-id="0d039-187">h.</span></span> <span data-ttu-id="0d039-188">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="0d039-188">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="0d039-189">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="0d039-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="0d039-190">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="0d039-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="0d039-191">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0d039-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0d039-192">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="0d039-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="0d039-193">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0d039-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="0d039-195">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="0d039-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="0d039-196">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="0d039-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0d039-198">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="0d039-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0d039-200">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0d039-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0d039-202">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="0d039-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0d039-204">a.</span><span class="sxs-lookup"><span data-stu-id="0d039-204">a.</span></span> <span data-ttu-id="0d039-205">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0d039-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0d039-206">b.</span><span class="sxs-lookup"><span data-stu-id="0d039-206">b.</span></span> <span data-ttu-id="0d039-207">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0d039-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0d039-208">c.</span><span class="sxs-lookup"><span data-stu-id="0d039-208">c.</span></span> <span data-ttu-id="0d039-209">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="0d039-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="0d039-210">d.</span><span class="sxs-lookup"><span data-stu-id="0d039-210">d.</span></span> <span data-ttu-id="0d039-211">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0d039-211">Click **Create**.</span></span>
 
### <a name="creating-a-mozy-enterprise-test-user"></a><span data-ttu-id="0d039-212">Skapa en testanvändare Mozy Enterprise</span><span class="sxs-lookup"><span data-stu-id="0d039-212">Creating a Mozy Enterprise test user</span></span>

<span data-ttu-id="0d039-213">För att aktivera Azure AD-användare att logga in på Mozy Enterprise, måste de etableras i Mozy Enterprise.</span><span class="sxs-lookup"><span data-stu-id="0d039-213">In order to enable Azure AD users to log into Mozy Enterprise, they must be provisioned into Mozy Enterprise.</span></span> <span data-ttu-id="0d039-214">När det gäller Mozy Enterprise är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="0d039-214">In the case of Mozy Enterprise, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="0d039-215">Du kan använda något annat Mozy Enterprise användarens konto skapas verktyg eller API: er som tillhandahålls av Mozy Enterprise etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="0d039-215">You can use any other Mozy Enterprise user account creation tools or APIs provided by Mozy Enterprise to provision AAD user accounts.</span></span>

<span data-ttu-id="0d039-216">**Utför följande steg för att etablera en användarkonton:**</span><span class="sxs-lookup"><span data-stu-id="0d039-216">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="0d039-217">Logga in på ditt **Mozy Enterprise** klient.</span><span class="sxs-lookup"><span data-stu-id="0d039-217">Log in to your **Mozy Enterprise** tenant.</span></span>

2. <span data-ttu-id="0d039-218">Klicka på **användare**, och klicka sedan på **Lägg till nya användare**.</span><span class="sxs-lookup"><span data-stu-id="0d039-218">Click **Users**, and then click **Add New User**.</span></span>
   
   <span data-ttu-id="0d039-219">![Användare](./media/active-directory-saas-mozy-enterprise-tutorial/ic777317.png "användare")</span><span class="sxs-lookup"><span data-stu-id="0d039-219">![Users](./media/active-directory-saas-mozy-enterprise-tutorial/ic777317.png "Users")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="0d039-220">Den **Lägg till nya användare** alternativet visas bara om **Mozy** är markerad som providern under **autentiseringsprincip**.</span><span class="sxs-lookup"><span data-stu-id="0d039-220">The **Add New User** option is only displayed only if **Mozy** is selected as the provider under **Authentication policy**.</span></span> <span data-ttu-id="0d039-221">Om SAML-autentisering är konfigurerad, sedan användare läggs till automatiskt på sin första inloggning via enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="0d039-221">If SAML Authentication is configured, then the users are added automatically on their first login through Single sign on.</span></span>
    
3. <span data-ttu-id="0d039-222">I användardialogrutan ny utför du följande steg:</span><span class="sxs-lookup"><span data-stu-id="0d039-222">On the new user dialog, perform the following steps:</span></span>
   
   <span data-ttu-id="0d039-223">![Lägg till användare](./media/active-directory-saas-mozy-enterprise-tutorial/ic777318.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="0d039-223">![Add Users](./media/active-directory-saas-mozy-enterprise-tutorial/ic777318.png "Add Users")</span></span>
   
   <span data-ttu-id="0d039-224">a.</span><span class="sxs-lookup"><span data-stu-id="0d039-224">a.</span></span> <span data-ttu-id="0d039-225">Från den **väljer du en grupp** väljer du en grupp.</span><span class="sxs-lookup"><span data-stu-id="0d039-225">From the **Choose a Group** list, select a group.</span></span>
   
   <span data-ttu-id="0d039-226">b.</span><span class="sxs-lookup"><span data-stu-id="0d039-226">b.</span></span> <span data-ttu-id="0d039-227">Från den **vilken typ av användare** väljer du en typ.</span><span class="sxs-lookup"><span data-stu-id="0d039-227">From the **What type of user** list, select a type.</span></span>
   
   <span data-ttu-id="0d039-228">c.</span><span class="sxs-lookup"><span data-stu-id="0d039-228">c.</span></span> <span data-ttu-id="0d039-229">I den **användarnamn** textruta skriver du namnet på Azure AD-användare.</span><span class="sxs-lookup"><span data-stu-id="0d039-229">In the **Username** textbox, type the name of the Azure AD user.</span></span>
   
   <span data-ttu-id="0d039-230">d.</span><span class="sxs-lookup"><span data-stu-id="0d039-230">d.</span></span> <span data-ttu-id="0d039-231">I den **e-post** textruta Skriv e-postadress för Azure AD-användare.</span><span class="sxs-lookup"><span data-stu-id="0d039-231">In the **Email** textbox, type the email address of the Azure AD user.</span></span>
   
   <span data-ttu-id="0d039-232">e.</span><span class="sxs-lookup"><span data-stu-id="0d039-232">e.</span></span> <span data-ttu-id="0d039-233">Välj **skicka e-post för användaren instruktion**.</span><span class="sxs-lookup"><span data-stu-id="0d039-233">Select **Send user instruction email**.</span></span>
   
   <span data-ttu-id="0d039-234">f.</span><span class="sxs-lookup"><span data-stu-id="0d039-234">f.</span></span> <span data-ttu-id="0d039-235">Klicka på **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="0d039-235">Click **Add User(s)**.</span></span>

     >[!NOTE]
     > <span data-ttu-id="0d039-236">Efter att användaren kan skickas ett e-postmeddelande till Azure AD-användare som innehåller en länk för att bekräfta kontot innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="0d039-236">After creating the user, an email will be sent to the Azure AD user that includes a link to confirm the account before it becomes active.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="0d039-237">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="0d039-237">Assigning the Azure AD test user</span></span>

<span data-ttu-id="0d039-238">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Mozy Enterprise.</span><span class="sxs-lookup"><span data-stu-id="0d039-238">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Mozy Enterprise.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="0d039-240">**Om du vill tilldela Mozy Enterprise Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="0d039-240">**To assign Britta Simon to Mozy Enterprise, perform the following steps:**</span></span>

1. <span data-ttu-id="0d039-241">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0d039-241">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="0d039-243">Välj i listan med program **Mozy Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="0d039-243">In the applications list, select **Mozy Enterprise**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_app.png) 

3. <span data-ttu-id="0d039-245">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="0d039-245">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="0d039-247">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="0d039-247">Click **Add** button.</span></span> <span data-ttu-id="0d039-248">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0d039-248">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="0d039-250">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="0d039-250">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="0d039-251">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0d039-251">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0d039-252">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0d039-252">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0d039-253">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0d039-253">Testing single sign-on</span></span>

<span data-ttu-id="0d039-254">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="0d039-254">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="0d039-255">När du klickar på panelen Mozy Enterprise på åtkomstpanelen bör du hämta inloggningssidan Mozy affärsprogram.</span><span class="sxs-lookup"><span data-stu-id="0d039-255">When you click the Mozy Enterprise tile in the Access Panel, you should get login page of Mozy Enterprise application.</span></span>
<span data-ttu-id="0d039-256">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0d039-256">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0d039-257">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="0d039-257">Additional resources</span></span>

* [<span data-ttu-id="0d039-258">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0d039-258">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0d039-259">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0d039-259">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_203.png

