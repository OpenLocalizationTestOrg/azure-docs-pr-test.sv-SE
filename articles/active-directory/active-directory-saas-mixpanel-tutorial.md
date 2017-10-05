---
title: "Självstudier: Azure Active Directory-integrering med Mixpanel | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Mixpanel."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a2df26ef-d441-44ac-a9f3-b37bf9709bcb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 3dd11b3477de1329c1c8e45a6dbf212b1635fd95
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mixpanel"></a><span data-ttu-id="74837-103">Självstudier: Azure Active Directory-integrering med Mixpanel</span><span class="sxs-lookup"><span data-stu-id="74837-103">Tutorial: Azure Active Directory integration with Mixpanel</span></span>

<span data-ttu-id="74837-104">I kursen får lära du att integrera Mixpanel med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="74837-104">In this tutorial, you learn how to integrate Mixpanel with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="74837-105">Integrera Mixpanel med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="74837-105">Integrating Mixpanel with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="74837-106">Du kan styra i Azure AD som har åtkomst till Mixpanel</span><span class="sxs-lookup"><span data-stu-id="74837-106">You can control in Azure AD who has access to Mixpanel</span></span>
- <span data-ttu-id="74837-107">Du kan aktivera användarna att automatiskt hämta loggat in på Mixpanel (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="74837-107">You can enable your users to automatically get signed-on to Mixpanel (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="74837-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="74837-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="74837-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="74837-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="74837-110">Krav</span><span class="sxs-lookup"><span data-stu-id="74837-110">Prerequisites</span></span>

<span data-ttu-id="74837-111">För att konfigurera Azure AD-integrering med Mixpanel, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="74837-111">To configure Azure AD integration with Mixpanel, you need the following items:</span></span>

- <span data-ttu-id="74837-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="74837-112">An Azure AD subscription</span></span>
- <span data-ttu-id="74837-113">En Mixpanel enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="74837-113">A Mixpanel single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="74837-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="74837-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="74837-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="74837-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="74837-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="74837-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="74837-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="74837-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="74837-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="74837-118">Scenario description</span></span>
<span data-ttu-id="74837-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="74837-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="74837-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="74837-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="74837-121">Att lägga till Mixpanel från galleriet</span><span class="sxs-lookup"><span data-stu-id="74837-121">Adding Mixpanel from the gallery</span></span>
2. <span data-ttu-id="74837-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="74837-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mixpanel-from-the-gallery"></a><span data-ttu-id="74837-123">Att lägga till Mixpanel från galleriet</span><span class="sxs-lookup"><span data-stu-id="74837-123">Adding Mixpanel from the gallery</span></span>
<span data-ttu-id="74837-124">Du måste lägga till Mixpanel från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Mixpanel i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="74837-124">To configure the integration of Mixpanel into Azure AD, you need to add Mixpanel from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="74837-125">**Utför följande steg för att lägga till Mixpanel från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="74837-125">**To add Mixpanel from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="74837-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="74837-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="74837-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="74837-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="74837-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="74837-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="74837-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="74837-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="74837-133">I sökrutan skriver **Mixpanel**.</span><span class="sxs-lookup"><span data-stu-id="74837-133">In the search box, type **Mixpanel**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_search.png)

5. <span data-ttu-id="74837-135">Välj i resultatpanelen **Mixpanel**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="74837-135">In the results panel, select **Mixpanel**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="74837-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="74837-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="74837-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Mixpanel baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="74837-138">In this section, you configure and test Azure AD single sign-on with Mixpanel based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="74837-139">Azure AD måste du känna till användaren i Mixpanel motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="74837-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Mixpanel is to a user in Azure AD.</span></span> <span data-ttu-id="74837-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Mixpanel upprättas.</span><span class="sxs-lookup"><span data-stu-id="74837-140">In other words, a link relationship between an Azure AD user and the related user in Mixpanel needs to be established.</span></span>

<span data-ttu-id="74837-141">I Mixpanel, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="74837-141">In Mixpanel, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="74837-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Mixpanel, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="74837-142">To configure and test Azure AD single sign-on with Mixpanel, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="74837-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="74837-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="74837-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="74837-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="74837-145">**[Skapa en testanvändare Mixpanel](#creating-a-mixpanel-test-user)**  – du har en motsvarighet för Britta Simon i Mixpanel som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="74837-145">**[Creating a Mixpanel test user](#creating-a-mixpanel-test-user)** - to have a counterpart of Britta Simon in Mixpanel that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="74837-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="74837-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="74837-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="74837-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="74837-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="74837-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="74837-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Mixpanel program.</span><span class="sxs-lookup"><span data-stu-id="74837-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Mixpanel application.</span></span>

<span data-ttu-id="74837-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Mixpanel:**</span><span class="sxs-lookup"><span data-stu-id="74837-150">**To configure Azure AD single sign-on with Mixpanel, perform the following steps:**</span></span>

1. <span data-ttu-id="74837-151">I Azure-portalen på den **Mixpanel** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="74837-151">In the Azure portal, on the **Mixpanel** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="74837-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="74837-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_samlbase.png)

3. <span data-ttu-id="74837-155">På den **Mixpanel domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="74837-155">On the **Mixpanel Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_url.png)

     <span data-ttu-id="74837-157">I den **inloggnings-URL** textruta Skriv en URL som:`https://mixpanel.com/login/`</span><span class="sxs-lookup"><span data-stu-id="74837-157">In the **Sign-on URL** textbox, type a URL as: `https://mixpanel.com/login/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="74837-158">Registrera på [https://mixpanel.com/register/](https://mixpanel.com/register/) att ställa in dina inloggningsuppgifter och kontakta den [Mixpanel supportteamet](mailto:support@mixpanel.com) att aktivera enkel inloggning inställningar för din klient.</span><span class="sxs-lookup"><span data-stu-id="74837-158">Please register at [https://mixpanel.com/register/](https://mixpanel.com/register/) to set up your login credentials and  contact the [Mixpanel support team](mailto:support@mixpanel.com) to enable SSO settings for your tenant.</span></span> <span data-ttu-id="74837-159">Du kan också få tecken i URL-värdet om det behövs från supportteamet Mixpanel.</span><span class="sxs-lookup"><span data-stu-id="74837-159">You can also get your Sign On URL value if necessary from your Mixpanel support team.</span></span> 
 
4. <span data-ttu-id="74837-160">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="74837-160">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_certificate.png) 

5. <span data-ttu-id="74837-162">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="74837-162">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mixpanel-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="74837-164">På den **Mixpanel Configuration** klickar du på **konfigurera Mixpanel** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="74837-164">On the **Mixpanel Configuration** section, click **Configure Mixpanel** to open **Configure sign-on** window.</span></span> <span data-ttu-id="74837-165">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="74837-165">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_configure.png) 

7. <span data-ttu-id="74837-167">I ett annat webbläsarfönster inloggning till Mixpanel programmet som administratör.</span><span class="sxs-lookup"><span data-stu-id="74837-167">In a different browser window, sign-on to your Mixpanel application as an administrator.</span></span>

8. <span data-ttu-id="74837-168">Längst ned på sidan, klicka på den lite **redskap** i det vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="74837-168">On bottom of the page, click the little **gear** icon in the left corner.</span></span> 
   
    ![Mixpanel enkel inloggning](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_06.png) 

9. <span data-ttu-id="74837-170">Klicka på den **säker programåtkomst** fliken och klicka sedan på **ändra inställningar**.</span><span class="sxs-lookup"><span data-stu-id="74837-170">Click the **Access security** tab, and then click **Change settings**.</span></span>
   
    ![Mixpanel inställningar](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_08.png) 

10. <span data-ttu-id="74837-172">På den **ändra ditt certifikat** dialogrutan sidan, klickar du på **Välj fil** att överföra din hämtat certifikat och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="74837-172">On the **Change your certificate** dialog page, click **Choose file** to upload your downloaded certificate, and then click **NEXT**.</span></span>
   
    ![Mixpanel inställningar](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_09.png) 

11.  <span data-ttu-id="74837-174">I textrutan autentisering URL på den **ändra autentiserings-URL** dialogrutan kan du klistra in värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="74837-174">In the authentication URL textbox on the **Change your authentication  URL** dialog page, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal, and then click **NEXT**.</span></span>
   
   ![Mixpanel inställningar](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_10.png) 

12. <span data-ttu-id="74837-176">Klicka på **Klar**.</span><span class="sxs-lookup"><span data-stu-id="74837-176">Click **Done**.</span></span>

> [!TIP]
> <span data-ttu-id="74837-177">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="74837-177">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="74837-178">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="74837-178">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="74837-179">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="74837-179">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="74837-180">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="74837-180">Creating an Azure AD test user</span></span>
<span data-ttu-id="74837-181">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="74837-181">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="74837-183">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="74837-183">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="74837-184">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="74837-184">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="74837-186">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="74837-186">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="74837-188">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="74837-188">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="74837-190">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="74837-190">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="74837-192">a.</span><span class="sxs-lookup"><span data-stu-id="74837-192">a.</span></span> <span data-ttu-id="74837-193">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="74837-193">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="74837-194">b.</span><span class="sxs-lookup"><span data-stu-id="74837-194">b.</span></span> <span data-ttu-id="74837-195">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="74837-195">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="74837-196">c.</span><span class="sxs-lookup"><span data-stu-id="74837-196">c.</span></span> <span data-ttu-id="74837-197">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="74837-197">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="74837-198">d.</span><span class="sxs-lookup"><span data-stu-id="74837-198">d.</span></span> <span data-ttu-id="74837-199">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="74837-199">Click **Create**.</span></span>
 
### <a name="creating-a-mixpanel-test-user"></a><span data-ttu-id="74837-200">Skapa en testanvändare Mixpanel</span><span class="sxs-lookup"><span data-stu-id="74837-200">Creating a Mixpanel test user</span></span>

<span data-ttu-id="74837-201">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Mixpanel.</span><span class="sxs-lookup"><span data-stu-id="74837-201">The objective of this section is to create a user called Britta Simon in Mixpanel.</span></span> 

1. <span data-ttu-id="74837-202">Logga in på webbplatsen Mixpanel företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="74837-202">Sign on to your Mixpanel company site as an administrator.</span></span>

2. <span data-ttu-id="74837-203">Längst ned på sidan, klickar du på knappen lite växel på det vänstra hörnet så öppnas den **inställningar** fönster.</span><span class="sxs-lookup"><span data-stu-id="74837-203">On the bottom of the page, click the little gear button on the left corner to open the **Settings** window.</span></span>

3. <span data-ttu-id="74837-204">Klicka på den **Team** fliken.</span><span class="sxs-lookup"><span data-stu-id="74837-204">Click the **Team** tab.</span></span>

4. <span data-ttu-id="74837-205">I den **teammedlem** textruta Skriv Brittas e-postadress i Azure.</span><span class="sxs-lookup"><span data-stu-id="74837-205">In the **team member** textbox, type Britta's email address in the Azure.</span></span>
   
    ![Mixpanel inställningar](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_11.png) 

5. <span data-ttu-id="74837-207">Klicka på **bjuda in**.</span><span class="sxs-lookup"><span data-stu-id="74837-207">Click **Invite**.</span></span> 

> [!Note]
> <span data-ttu-id="74837-208">Användaren får ett e-postmeddelande för att ställa in profilen.</span><span class="sxs-lookup"><span data-stu-id="74837-208">The user will get an email to set up the profile.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="74837-209">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="74837-209">Assigning the Azure AD test user</span></span>

<span data-ttu-id="74837-210">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Mixpanel.</span><span class="sxs-lookup"><span data-stu-id="74837-210">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Mixpanel.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="74837-212">**Om du vill tilldela Mixpanel Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="74837-212">**To assign Britta Simon to Mixpanel, perform the following steps:**</span></span>

1. <span data-ttu-id="74837-213">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="74837-213">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="74837-215">Välj i listan med program **Mixpanel**.</span><span class="sxs-lookup"><span data-stu-id="74837-215">In the applications list, select **Mixpanel**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_app.png) 

3. <span data-ttu-id="74837-217">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="74837-217">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="74837-219">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="74837-219">Click **Add** button.</span></span> <span data-ttu-id="74837-220">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="74837-220">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="74837-222">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="74837-222">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="74837-223">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="74837-223">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="74837-224">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="74837-224">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="74837-225">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="74837-225">Testing single sign-on</span></span>

<span data-ttu-id="74837-226">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="74837-226">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="74837-227">När du klickar på panelen Mixpanel på åtkomstpanelen du bör få automatiskt loggat in på ditt Mixpanel program.</span><span class="sxs-lookup"><span data-stu-id="74837-227">When you click the Mixpanel tile in the Access Panel, you should get automatically signed-on to your Mixpanel application.</span></span>
<span data-ttu-id="74837-228">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="74837-228">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="74837-229">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="74837-229">Additional resources</span></span>

* [<span data-ttu-id="74837-230">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="74837-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="74837-231">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="74837-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_203.png

