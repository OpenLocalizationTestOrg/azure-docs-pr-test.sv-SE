---
title: "Självstudier: Azure Active Directory-integrering med bra jobbat | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Bra jobbat."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 39c47ce6-4944-47ba-8f53-3afa95398034
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: 353798fcfd4ad7ce017fc2fddf4110715db3ace2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kudos"></a><span data-ttu-id="2e0be-103">Självstudier: Azure Active Directory-integrering med bra jobbat</span><span class="sxs-lookup"><span data-stu-id="2e0be-103">Tutorial: Azure Active Directory integration with Kudos</span></span>

<span data-ttu-id="2e0be-104">I kursen får lära du att integrera Bra jobbat med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="2e0be-104">In this tutorial, you learn how to integrate Kudos with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2e0be-105">Integrera Bra jobbat med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="2e0be-105">Integrating Kudos with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="2e0be-106">Du kan styra i Azure AD som har åtkomst till Bra jobbat</span><span class="sxs-lookup"><span data-stu-id="2e0be-106">You can control in Azure AD who has access to Kudos</span></span>
- <span data-ttu-id="2e0be-107">Du kan aktivera användarna att automatiskt hämta loggat in på Bra jobbat (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="2e0be-107">You can enable your users to automatically get signed-on to Kudos (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2e0be-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2e0be-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="2e0be-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2e0be-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2e0be-110">Krav</span><span class="sxs-lookup"><span data-stu-id="2e0be-110">Prerequisites</span></span>

<span data-ttu-id="2e0be-111">Om du vill konfigurera Azure AD-integrering med bra jobbat behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="2e0be-111">To configure Azure AD integration with Kudos, you need the following items:</span></span>

- <span data-ttu-id="2e0be-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="2e0be-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2e0be-113">En bra jobbat enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="2e0be-113">A Kudos single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2e0be-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="2e0be-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2e0be-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="2e0be-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2e0be-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="2e0be-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2e0be-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2e0be-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2e0be-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="2e0be-118">Scenario description</span></span>
<span data-ttu-id="2e0be-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="2e0be-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2e0be-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="2e0be-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2e0be-121">Att lägga till Bra jobbat från galleriet</span><span class="sxs-lookup"><span data-stu-id="2e0be-121">Adding Kudos from the gallery</span></span>
2. <span data-ttu-id="2e0be-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2e0be-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kudos-from-the-gallery"></a><span data-ttu-id="2e0be-123">Att lägga till Bra jobbat från galleriet</span><span class="sxs-lookup"><span data-stu-id="2e0be-123">Adding Kudos from the gallery</span></span>
<span data-ttu-id="2e0be-124">Du måste lägga till Bra jobbat från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Bra jobbat i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2e0be-124">To configure the integration of Kudos into Azure AD, you need to add Kudos from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="2e0be-125">**Utför följande steg för att lägga till Bra jobbat från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="2e0be-125">**To add Kudos from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="2e0be-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2e0be-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2e0be-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="2e0be-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="2e0be-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="2e0be-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="2e0be-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2e0be-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="2e0be-133">I sökrutan skriver **Bra jobbat**.</span><span class="sxs-lookup"><span data-stu-id="2e0be-133">In the search box, type **Kudos**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_search.png)

5. <span data-ttu-id="2e0be-135">Välj i resultatpanelen **Bra jobbat**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="2e0be-135">In the results panel, select **Kudos**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2e0be-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2e0be-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2e0be-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med bra jobbat baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="2e0be-138">In this section, you configure and test Azure AD single sign-on with Kudos based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2e0be-139">Azure AD måste du känna till användaren i Bra jobbat motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="2e0be-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Kudos is to a user in Azure AD.</span></span> <span data-ttu-id="2e0be-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Bra jobbat upprättas.</span><span class="sxs-lookup"><span data-stu-id="2e0be-140">In other words, a link relationship between an Azure AD user and the related user in Kudos needs to be established.</span></span>

<span data-ttu-id="2e0be-141">I Bra jobbat, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="2e0be-141">In Kudos, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="2e0be-142">Om du vill konfigurera och testa Azure AD enkel inloggning med bra jobbat, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="2e0be-142">To configure and test Azure AD single sign-on with Kudos, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="2e0be-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="2e0be-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="2e0be-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2e0be-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2e0be-145">**[Skapa en bra jobbat testanvändare](#creating-a-kudos-test-user)**  – har en motsvarighet för Britta Simon Bra jobbat som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="2e0be-145">**[Creating a Kudos test user](#creating-a-kudos-test-user)** - to have a counterpart of Britta Simon in Kudos that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="2e0be-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2e0be-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2e0be-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="2e0be-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2e0be-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2e0be-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2e0be-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Bra jobbat.</span><span class="sxs-lookup"><span data-stu-id="2e0be-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Kudos application.</span></span>

<span data-ttu-id="2e0be-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med bra jobbat:**</span><span class="sxs-lookup"><span data-stu-id="2e0be-150">**To configure Azure AD single sign-on with Kudos, perform the following steps:**</span></span>

1. <span data-ttu-id="2e0be-151">I Azure-portalen på den **Bra jobbat** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="2e0be-151">In the Azure portal, on the **Kudos** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="2e0be-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2e0be-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_samlbase.png)

3. <span data-ttu-id="2e0be-155">På den **Bra jobbat domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="2e0be-155">On the **Kudos Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_url.png)

    <span data-ttu-id="2e0be-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<company>.kudosnow.com`</span><span class="sxs-lookup"><span data-stu-id="2e0be-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company>.kudosnow.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="2e0be-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="2e0be-158">This value is not real.</span></span> <span data-ttu-id="2e0be-159">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="2e0be-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="2e0be-160">Kontakta [Bra jobbat klienten supportteamet](http://success.kudosnow.com/home) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="2e0be-160">Contact [Kudos Client support team](http://success.kudosnow.com/home) to get this value.</span></span> 
 
4. <span data-ttu-id="2e0be-161">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="2e0be-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_certificate.png) 

5. <span data-ttu-id="2e0be-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="2e0be-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kudos-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="2e0be-165">På den **Bra jobbat Configuration** klickar du på **konfigurera Bra jobbat** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="2e0be-165">On the **Kudos Configuration** section, click **Configure Kudos** to open **Configure sign-on** window.</span></span> <span data-ttu-id="2e0be-166">Kopiera den **Sign-Out Webbadressen och SAML enkel inloggning Service** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="2e0be-166">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_configure.png) 

7. <span data-ttu-id="2e0be-168">Logga in på webbplatsen Bra jobbat företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="2e0be-168">In a different web browser window, log into your Kudos company site as an administrator.</span></span>

8. <span data-ttu-id="2e0be-169">Klicka på menyn högst upp **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="2e0be-169">In the menu on the top, click **Settings**.</span></span>
   
    <span data-ttu-id="2e0be-170">![Inställningar för](./media/active-directory-saas-kudos-tutorial/ic787806.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="2e0be-170">![Settings](./media/active-directory-saas-kudos-tutorial/ic787806.png "Settings")</span></span>

9. <span data-ttu-id="2e0be-171">Klicka på **integreringar \> SSO**.</span><span class="sxs-lookup"><span data-stu-id="2e0be-171">Click **Integrations \> SSO**.</span></span>

10. <span data-ttu-id="2e0be-172">I den **SSO** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="2e0be-172">In the **SSO** section, perform the following steps:</span></span>
   
    <span data-ttu-id="2e0be-173">![SSO](./media/active-directory-saas-kudos-tutorial/ic787807.png "ENKEL INLOGGNING")</span><span class="sxs-lookup"><span data-stu-id="2e0be-173">![SSO](./media/active-directory-saas-kudos-tutorial/ic787807.png "SSO")</span></span>
   
    <span data-ttu-id="2e0be-174">a.</span><span class="sxs-lookup"><span data-stu-id="2e0be-174">a.</span></span> <span data-ttu-id="2e0be-175">I **inloggning URL** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2e0be-175">In **Sign on URL** textbox, paste the value of  **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="2e0be-176">b.</span><span class="sxs-lookup"><span data-stu-id="2e0be-176">b.</span></span> <span data-ttu-id="2e0be-177">Öppna din Base64-kodade certifikatet i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **X.509-certifikat** textruta</span><span class="sxs-lookup"><span data-stu-id="2e0be-177">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **X.509 certificate** textbox</span></span>
   
    <span data-ttu-id="2e0be-178">c.</span><span class="sxs-lookup"><span data-stu-id="2e0be-178">c.</span></span> <span data-ttu-id="2e0be-179">I **logga ut till URL**, klistra in värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2e0be-179">In **Logout To URL**, paste the value of  **Sign-Out URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="2e0be-180">d.</span><span class="sxs-lookup"><span data-stu-id="2e0be-180">d.</span></span> <span data-ttu-id="2e0be-181">I den **din Bra jobbat URL** textruta Ange företagets namn.</span><span class="sxs-lookup"><span data-stu-id="2e0be-181">In the **Your Kudos URL** textbox, type your company name.</span></span>
   
    <span data-ttu-id="2e0be-182">e.</span><span class="sxs-lookup"><span data-stu-id="2e0be-182">e.</span></span> <span data-ttu-id="2e0be-183">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="2e0be-183">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="2e0be-184">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="2e0be-184">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="2e0be-185">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="2e0be-185">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="2e0be-186">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2e0be-186">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2e0be-187">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="2e0be-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="2e0be-188">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2e0be-188">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="2e0be-190">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="2e0be-190">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="2e0be-191">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2e0be-191">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kudos-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2e0be-193">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="2e0be-193">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kudos-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2e0be-195">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2e0be-195">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kudos-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2e0be-197">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="2e0be-197">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kudos-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2e0be-199">a.</span><span class="sxs-lookup"><span data-stu-id="2e0be-199">a.</span></span> <span data-ttu-id="2e0be-200">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2e0be-200">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2e0be-201">b.</span><span class="sxs-lookup"><span data-stu-id="2e0be-201">b.</span></span> <span data-ttu-id="2e0be-202">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2e0be-202">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2e0be-203">c.</span><span class="sxs-lookup"><span data-stu-id="2e0be-203">c.</span></span> <span data-ttu-id="2e0be-204">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="2e0be-204">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="2e0be-205">d.</span><span class="sxs-lookup"><span data-stu-id="2e0be-205">d.</span></span> <span data-ttu-id="2e0be-206">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="2e0be-206">Click **Create**.</span></span>
 
### <a name="creating-a-kudos-test-user"></a><span data-ttu-id="2e0be-207">Skapa en bra jobbat testanvändare</span><span class="sxs-lookup"><span data-stu-id="2e0be-207">Creating a Kudos test user</span></span>

<span data-ttu-id="2e0be-208">För att aktivera Azure AD-användare att logga in på Bra jobbat etableras de i Bra jobbat.</span><span class="sxs-lookup"><span data-stu-id="2e0be-208">In order to enable Azure AD users to log into Kudos, they must be provisioned into Kudos.</span></span> 

<span data-ttu-id="2e0be-209">Bra jobbat är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="2e0be-209">In the case of Kudos, provisioning is a manual task.</span></span>

<span data-ttu-id="2e0be-210">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="2e0be-210">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="2e0be-211">Logga in på ditt **Bra jobbat** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="2e0be-211">Log in to your **Kudos** company site as administrator.</span></span>

2. <span data-ttu-id="2e0be-212">Klicka på menyn högst upp **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="2e0be-212">In the menu on the top, click **Settings**.</span></span>
   
   <span data-ttu-id="2e0be-213">![Inställningar för](./media/active-directory-saas-kudos-tutorial/ic787806.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="2e0be-213">![Settings](./media/active-directory-saas-kudos-tutorial/ic787806.png "Settings")</span></span>

3. <span data-ttu-id="2e0be-214">Klicka på **Användaradministration**.</span><span class="sxs-lookup"><span data-stu-id="2e0be-214">Click **User Admin**.</span></span>

4. <span data-ttu-id="2e0be-215">Klicka på den **användare** fliken och klicka sedan på **lägga till en användare**.</span><span class="sxs-lookup"><span data-stu-id="2e0be-215">Click the **Users** tab, and then click **Add a User**.</span></span>
   
   <span data-ttu-id="2e0be-216">![Användaradministration](./media/active-directory-saas-kudos-tutorial/ic787809.png "Användaradministration")</span><span class="sxs-lookup"><span data-stu-id="2e0be-216">![User Admin](./media/active-directory-saas-kudos-tutorial/ic787809.png "User Admin")</span></span>

5. <span data-ttu-id="2e0be-217">I den **lägga till en användare** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="2e0be-217">In the **Add a User** section, perform the following steps:</span></span>
   
    <span data-ttu-id="2e0be-218">![Lägga till en användare](./media/active-directory-saas-kudos-tutorial/ic787810.png "lägga till en användare")</span><span class="sxs-lookup"><span data-stu-id="2e0be-218">![Add a User](./media/active-directory-saas-kudos-tutorial/ic787810.png "Add a User")</span></span>
   
    <span data-ttu-id="2e0be-219">a.</span><span class="sxs-lookup"><span data-stu-id="2e0be-219">a.</span></span> <span data-ttu-id="2e0be-220">Typ av **Förnamn**, **efternamn**, **e-post** och annan information om ett giltigt Azure Active Directory-konto som du vill etablera i relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="2e0be-220">Type the **First Name**, **Last Name**, **Email** and other details of a valid Azure Active Directory account you want to provision into the related textboxes.</span></span>
   
    <span data-ttu-id="2e0be-221">b.</span><span class="sxs-lookup"><span data-stu-id="2e0be-221">b.</span></span> <span data-ttu-id="2e0be-222">Klicka på **skapa användare**.</span><span class="sxs-lookup"><span data-stu-id="2e0be-222">Click **Create User**.</span></span>

>[!NOTE]
><span data-ttu-id="2e0be-223">Du kan använda något annat Bra jobbat användarens konto skapas verktyg eller API: er som tillhandahålls av Bra jobbat etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="2e0be-223">You can use any other Kudos user account creation tools or APIs provided by Kudos to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="2e0be-224">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="2e0be-224">Assigning the Azure AD test user</span></span>

<span data-ttu-id="2e0be-225">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Bra jobbat.</span><span class="sxs-lookup"><span data-stu-id="2e0be-225">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Kudos.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="2e0be-227">**Om du vill tilldela Bra jobbat Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2e0be-227">**To assign Britta Simon to Kudos, perform the following steps:**</span></span>

1. <span data-ttu-id="2e0be-228">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="2e0be-228">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="2e0be-230">Välj i listan med program **Bra jobbat**.</span><span class="sxs-lookup"><span data-stu-id="2e0be-230">In the applications list, select **Kudos**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_app.png) 

3. <span data-ttu-id="2e0be-232">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="2e0be-232">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="2e0be-234">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="2e0be-234">Click **Add** button.</span></span> <span data-ttu-id="2e0be-235">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2e0be-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="2e0be-237">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="2e0be-237">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="2e0be-238">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2e0be-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2e0be-239">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2e0be-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2e0be-240">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2e0be-240">Testing single sign-on</span></span>

<span data-ttu-id="2e0be-241">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="2e0be-241">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="2e0be-242">När du klickar på panelen Bra jobbat på åtkomstpanelen du bör få automatiskt loggat in i tillämpningsprogrammet Bra jobbat.</span><span class="sxs-lookup"><span data-stu-id="2e0be-242">When you click the Kudos tile in the Access Panel, you should get automatically signed-on to your Kudos application.</span></span> <span data-ttu-id="2e0be-243">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2e0be-243">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2e0be-244">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="2e0be-244">Additional resources</span></span>

* [<span data-ttu-id="2e0be-245">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2e0be-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2e0be-246">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2e0be-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_203.png

