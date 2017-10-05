---
title: "Självstudier: Azure Active Directory-integrering med Citrix GoToMeeting | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Citrix GoToMeeting."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7d7897f6-b88e-4dd5-8f3a-e612337b1413
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: c1ac144c4fa43312ec26fce03cd0ee1bfcf73d4b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-gotomeeting"></a><span data-ttu-id="635e5-103">Självstudier: Azure Active Directory-integrering med Citrix GoToMeeting</span><span class="sxs-lookup"><span data-stu-id="635e5-103">Tutorial: Azure Active Directory integration with Citrix GoToMeeting</span></span>

<span data-ttu-id="635e5-104">I kursen får lära du att integrera Citrix GoToMeeting med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="635e5-104">In this tutorial, you learn how to integrate Citrix GoToMeeting with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="635e5-105">Integrera Citrix GoToMeeting med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="635e5-105">Integrating Citrix GoToMeeting with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="635e5-106">Du kan styra i Azure AD som har åtkomst till Citrix GoToMeeting</span><span class="sxs-lookup"><span data-stu-id="635e5-106">You can control in Azure AD who has access to Citrix GoToMeeting</span></span>
- <span data-ttu-id="635e5-107">Du kan aktivera användarna att automatiskt hämta loggat in på Citrix GoToMeeting (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="635e5-107">You can enable your users to automatically get signed-on to Citrix GoToMeeting (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="635e5-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="635e5-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="635e5-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="635e5-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="635e5-110">Krav</span><span class="sxs-lookup"><span data-stu-id="635e5-110">Prerequisites</span></span>

<span data-ttu-id="635e5-111">Om du vill konfigurera Azure AD-integrering med Citrix GoToMeeting behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="635e5-111">To configure Azure AD integration with Citrix GoToMeeting, you need the following items:</span></span>

- <span data-ttu-id="635e5-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="635e5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="635e5-113">En Citrix GoToMeeting enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="635e5-113">A Citrix GoToMeeting single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="635e5-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="635e5-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="635e5-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="635e5-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="635e5-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="635e5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="635e5-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="635e5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="635e5-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="635e5-118">Scenario description</span></span>
<span data-ttu-id="635e5-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="635e5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="635e5-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="635e5-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="635e5-121">Att lägga till Citrix GoToMeeting från galleriet</span><span class="sxs-lookup"><span data-stu-id="635e5-121">Adding Citrix GoToMeeting from the gallery</span></span>
2. <span data-ttu-id="635e5-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="635e5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-citrix-gotomeeting-from-the-gallery"></a><span data-ttu-id="635e5-123">Att lägga till Citrix GoToMeeting från galleriet</span><span class="sxs-lookup"><span data-stu-id="635e5-123">Adding Citrix GoToMeeting from the gallery</span></span>
<span data-ttu-id="635e5-124">Du måste lägga till Citrix GoToMeeting från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Citrix GoToMeeting i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="635e5-124">To configure the integration of Citrix GoToMeeting into Azure AD, you need to add Citrix GoToMeeting from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="635e5-125">**Utför följande steg för att lägga till Citrix GoToMeeting från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="635e5-125">**To add Citrix GoToMeeting from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="635e5-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="635e5-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="635e5-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="635e5-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="635e5-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="635e5-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="635e5-131">Klicka på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="635e5-131">Click **New application** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="635e5-133">I sökrutan skriver **Citrix GoToMeeting**.</span><span class="sxs-lookup"><span data-stu-id="635e5-133">In the search box, type **Citrix GoToMeeting**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_search.png)

5. <span data-ttu-id="635e5-135">Välj i resultatpanelen **Citrix GoToMeeting**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="635e5-135">In the results panel, select **Citrix GoToMeeting**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="635e5-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="635e5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="635e5-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Citrix GoToMeeting baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="635e5-138">In this section, you configure and test Azure AD single sign-on with Citrix GoToMeeting based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="635e5-139">Azure AD måste du känna till användaren i Citrix GoToMeeting motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="635e5-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Citrix GoToMeeting is to a user in Azure AD.</span></span> <span data-ttu-id="635e5-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Citrix GoToMeeting upprättas.</span><span class="sxs-lookup"><span data-stu-id="635e5-140">In other words, a link relationship between an Azure AD user and the related user in Citrix GoToMeeting needs to be established.</span></span>

<span data-ttu-id="635e5-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="635e5-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Citrix GoToMeeting.</span></span>

<span data-ttu-id="635e5-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Citrix GoToMeeting, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="635e5-142">To configure and test Azure AD single sign-on with Citrix GoToMeeting, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="635e5-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="635e5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="635e5-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="635e5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="635e5-145">**[Skapa en testanvändare Citrix GoToMeeting](#creating-a-citrix-gotomeeting-test-user)**  – du har en motsvarighet för Britta Simon i Citrix GoToMeeting som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="635e5-145">**[Creating a Citrix GoToMeeting test user](#creating-a-citrix-gotomeeting-test-user)** - to have a counterpart of Britta Simon in Citrix GoToMeeting that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="635e5-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="635e5-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="635e5-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="635e5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="635e5-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="635e5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="635e5-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i Citrix GoToMeeting-program.</span><span class="sxs-lookup"><span data-stu-id="635e5-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Citrix GoToMeeting application.</span></span>

<span data-ttu-id="635e5-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Citrix GoToMeeting:**</span><span class="sxs-lookup"><span data-stu-id="635e5-150">**To configure Azure AD single sign-on with Citrix GoToMeeting, perform the following steps:**</span></span>

1. <span data-ttu-id="635e5-151">I Azure-portalen på den **Citrix GoToMeeting** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="635e5-151">In the Azure portal, on the **Citrix GoToMeeting** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="635e5-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="635e5-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_samlbase.png)

3. <span data-ttu-id="635e5-155">På den **Citrix GoToMeeting domän och URL: er** avsnitt, behöver du inte utföra några steg.</span><span class="sxs-lookup"><span data-stu-id="635e5-155">On the **Citrix GoToMeeting Domain and URLs** section, no need to perform any steps.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_url.png)


3. <span data-ttu-id="635e5-157">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="635e5-157">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_certificate.png) 

4. <span data-ttu-id="635e5-159">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="635e5-159">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_400.png)
    
5. <span data-ttu-id="635e5-161">Klicka på Konfigurera Citrix GoToMeeting SAML så öppnas konfigurera inloggning på Citrix GoToMeeting SAML konfigurationsavsnittet.</span><span class="sxs-lookup"><span data-stu-id="635e5-161">On the Citrix GoToMeeting SAML Configuration section, click Configure Citrix GoToMeeting SAML to open Configure sign-on window.</span></span> <span data-ttu-id="635e5-162">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="635e5-162">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

6. <span data-ttu-id="635e5-163">I en annan webbläsare och logga in på ditt [Citrix organisation Center](https://account.citrixonline.com/organization/administration/).</span><span class="sxs-lookup"><span data-stu-id="635e5-163">In a different browser window, log in to your [Citrix Organization Center](https://account.citrixonline.com/organization/administration/).</span></span>

7. <span data-ttu-id="635e5-164">Klicka på den **identitetsleverantör** fliken och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="635e5-164">Click the **Identity Provider** tab, and then perform the following steps:</span></span>  
   
    <span data-ttu-id="635e5-165">![SAML-installationsprogrammet](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "SAML-installationen")</span><span class="sxs-lookup"><span data-stu-id="635e5-165">![SAML setup](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "SAML setup")</span></span>
   
    <span data-ttu-id="635e5-166">a.</span><span class="sxs-lookup"><span data-stu-id="635e5-166">a.</span></span> <span data-ttu-id="635e5-167">Välj **manuell**</span><span class="sxs-lookup"><span data-stu-id="635e5-167">Select **Manual**</span></span>

    <span data-ttu-id="635e5-168">b.</span><span class="sxs-lookup"><span data-stu-id="635e5-168">b.</span></span> <span data-ttu-id="635e5-169">I Azure-portalen på den **Konfigurera enkel inloggning på Citrix GoToMeeting** dialogrutan sidan, kopiera den **SAML enkel inloggning Tjänstwebbadress** värdet och klistrar in det i den **inloggning Sidadress** textruta.</span><span class="sxs-lookup"><span data-stu-id="635e5-169">In the Azure portal, on the **Configure single sign-on at Citrix GoToMeeting** dialog page, copy the **SAML Single Sign-On Service URL** value, and then paste it into the **Sign-in page URL** textbox.</span></span> 

    <span data-ttu-id="635e5-170">c.</span><span class="sxs-lookup"><span data-stu-id="635e5-170">c.</span></span> <span data-ttu-id="635e5-171">I Azure-portalen på den **Konfigurera enkel inloggning på Citrix GoToMeeting** dialogrutan sidan, kopiera den **Sign-Out URL** värdet och klistrar in det i den **URL för utloggning** textruta.</span><span class="sxs-lookup"><span data-stu-id="635e5-171">In the Azure portal, on the **Configure single sign-on at Citrix GoToMeeting** dialog page, copy the **Sign-Out URL** value, and then paste it into the **Sign-out page URL** textbox.</span></span>

    <span data-ttu-id="635e5-172">d.</span><span class="sxs-lookup"><span data-stu-id="635e5-172">d.</span></span> <span data-ttu-id="635e5-173">I Azure-portalen på den **Konfigurera enkel inloggning på Citrix GoToMeeting** dialogrutan sidan, kopiera den **SAML enhets-ID** värdet och klistrar in det i den **identitet providern enhets-ID**textruta.</span><span class="sxs-lookup"><span data-stu-id="635e5-173">In the Azure portal, on the **Configure single sign-on at Citrix GoToMeeting** dialog page, copy the **SAML Entity ID** value, and then paste it into the **Identity Provider Entity ID** textbox.</span></span>

    <span data-ttu-id="635e5-174">e.</span><span class="sxs-lookup"><span data-stu-id="635e5-174">e.</span></span> <span data-ttu-id="635e5-175">Om du vill överföra din hämtat certifikat klickar du på **överför certifikat**.</span><span class="sxs-lookup"><span data-stu-id="635e5-175">To upload your downloaded certificate, click **Upload Certificate**.</span></span>

    <span data-ttu-id="635e5-176">f.</span><span class="sxs-lookup"><span data-stu-id="635e5-176">f.</span></span> <span data-ttu-id="635e5-177">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="635e5-177">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="635e5-178">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="635e5-178">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="635e5-179">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="635e5-179">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="635e5-180">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="635e5-180">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="635e5-181">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="635e5-181">Creating an Azure AD test user</span></span>
<span data-ttu-id="635e5-182">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="635e5-182">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="635e5-184">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="635e5-184">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="635e5-185">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="635e5-185">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="635e5-187">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="635e5-187">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="635e5-189">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="635e5-189">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="635e5-191">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="635e5-191">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="635e5-193">a.</span><span class="sxs-lookup"><span data-stu-id="635e5-193">a.</span></span> <span data-ttu-id="635e5-194">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="635e5-194">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="635e5-195">b.</span><span class="sxs-lookup"><span data-stu-id="635e5-195">b.</span></span> <span data-ttu-id="635e5-196">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="635e5-196">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="635e5-197">c.</span><span class="sxs-lookup"><span data-stu-id="635e5-197">c.</span></span> <span data-ttu-id="635e5-198">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="635e5-198">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="635e5-199">d.</span><span class="sxs-lookup"><span data-stu-id="635e5-199">d.</span></span> <span data-ttu-id="635e5-200">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="635e5-200">Click **Create**.</span></span>
 
### <a name="creating-a-citrix-gotomeeting-test-user"></a><span data-ttu-id="635e5-201">Skapa en testanvändare Citrix GoToMeeting</span><span class="sxs-lookup"><span data-stu-id="635e5-201">Creating a Citrix GoToMeeting test user</span></span>

<span data-ttu-id="635e5-202">I det här avsnittet skapas en användare som kallas Britta Simon i Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="635e5-202">In this section, a user called Britta Simon is created in Citrix GoToMeeting.</span></span> <span data-ttu-id="635e5-203">Citrix GoToMeeting stöder just-in-time-allokering som är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="635e5-203">Citrix GoToMeeting supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="635e5-204">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="635e5-204">There is no action item for you in this section.</span></span> <span data-ttu-id="635e5-205">Om en användare inte redan finns i Citrix GoToMeeting, skapas en ny när du försöker komma åt Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="635e5-205">If a user doesn't already exist in Citrix GoToMeeting, a new one is created when you attempt to access Citrix GoToMeeting.</span></span>

>[!Note]
><span data-ttu-id="635e5-206">Om du behöver skapa en användare manuellt, kontakta [Citrix GoToMeeting supportteamet](https://care.citrixonline.com/gotomeeting)</span><span class="sxs-lookup"><span data-stu-id="635e5-206">If you need to create a user manually, Contact [Citrix GoToMeeting support team](https://care.citrixonline.com/gotomeeting)</span></span> 


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="635e5-207">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="635e5-207">Assigning the Azure AD test user</span></span>

<span data-ttu-id="635e5-208">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="635e5-208">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Citrix GoToMeeting.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="635e5-210">**Om du vill tilldela Citrix GoToMeeting Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="635e5-210">**To assign Britta Simon to Citrix GoToMeeting, perform the following steps:**</span></span>

1. <span data-ttu-id="635e5-211">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="635e5-211">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="635e5-213">Välj i listan med program **Citrix GoToMeeting**.</span><span class="sxs-lookup"><span data-stu-id="635e5-213">In the applications list, select **Citrix GoToMeeting**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_app.png) 

3. <span data-ttu-id="635e5-215">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="635e5-215">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="635e5-217">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="635e5-217">Click **Add** button.</span></span> <span data-ttu-id="635e5-218">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="635e5-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="635e5-220">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="635e5-220">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="635e5-221">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="635e5-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="635e5-222">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="635e5-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="635e5-223">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="635e5-223">Testing single sign-on</span></span>

<span data-ttu-id="635e5-224">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="635e5-224">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="635e5-225">Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="635e5-225">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="635e5-226">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="635e5-226">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="635e5-227">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="635e5-227">Additional resources</span></span>

* [<span data-ttu-id="635e5-228">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="635e5-228">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="635e5-229">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="635e5-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="635e5-230">Konfigurera Användaretablering</span><span class="sxs-lookup"><span data-stu-id="635e5-230">Configure User Provisioning</span></span>](active-directory-saas-citrixgotomeeting-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_203.png

