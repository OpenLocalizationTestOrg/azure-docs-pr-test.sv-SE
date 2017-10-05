---
title: "Självstudier: Azure Active Directory-integrering med Rally programvara | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Rally programvaran och Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ba25fade-e152-42dd-8377-a30bbc48c3ed
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: jeedes
ms.openlocfilehash: 6481c9ef0ca71419ccfa6f7956f4702985743df3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rally-software"></a><span data-ttu-id="fd0db-103">Självstudier: Azure Active Directory-integrering med Rally programvara</span><span class="sxs-lookup"><span data-stu-id="fd0db-103">Tutorial: Azure Active Directory integration with Rally Software</span></span>

<span data-ttu-id="fd0db-104">I kursen får lära du att integrera Rally programvara med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="fd0db-104">In this tutorial, you learn how to integrate Rally Software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fd0db-105">Integrera Rally programvara med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="fd0db-105">Integrating Rally Software with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="fd0db-106">Du kan styra i Azure AD som har åtkomst till Rally programvara.</span><span class="sxs-lookup"><span data-stu-id="fd0db-106">You can control in Azure AD who has access to Rally Software.</span></span>
- <span data-ttu-id="fd0db-107">Du kan aktivera användarna att automatiskt hämta loggat in på Rally programvara (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="fd0db-107">You can enable your users to automatically get signed-on to Rally Software (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="fd0db-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fd0db-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="fd0db-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fd0db-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fd0db-110">Krav</span><span class="sxs-lookup"><span data-stu-id="fd0db-110">Prerequisites</span></span>

<span data-ttu-id="fd0db-111">För att konfigurera Azure AD-integrering med Rally programvara, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="fd0db-111">To configure Azure AD integration with Rally Software, you need the following items:</span></span>

- <span data-ttu-id="fd0db-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="fd0db-112">An Azure AD subscription</span></span>
- <span data-ttu-id="fd0db-113">En Rally programvara enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="fd0db-113">A Rally Software single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fd0db-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="fd0db-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fd0db-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="fd0db-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fd0db-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="fd0db-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fd0db-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fd0db-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fd0db-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="fd0db-118">Scenario description</span></span>
<span data-ttu-id="fd0db-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="fd0db-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fd0db-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="fd0db-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fd0db-121">Lägga till Rally programvara från galleriet</span><span class="sxs-lookup"><span data-stu-id="fd0db-121">Adding Rally Software from the gallery</span></span>
2. <span data-ttu-id="fd0db-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fd0db-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-rally-software-from-the-gallery"></a><span data-ttu-id="fd0db-123">Lägga till Rally programvara från galleriet</span><span class="sxs-lookup"><span data-stu-id="fd0db-123">Adding Rally Software from the gallery</span></span>
<span data-ttu-id="fd0db-124">Du måste lägga till Rally programvara från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Rally programvara i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fd0db-124">To configure the integration of Rally Software into Azure AD, you need to add Rally Software from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="fd0db-125">**Utför följande steg för att lägga till Rally programvara från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="fd0db-125">**To add Rally Software from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="fd0db-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="fd0db-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="fd0db-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="fd0db-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="fd0db-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="fd0db-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="fd0db-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fd0db-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="fd0db-133">I sökrutan skriver **Rally programvara**väljer **Rally programvara** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="fd0db-133">In the search box, type **Rally Software**, select **Rally Software** from result panel then click **Add** button to add the application.</span></span>

    ![Rally programvara i resultatlistan](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="fd0db-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fd0db-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="fd0db-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Rally programvara baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="fd0db-136">In this section, you configure and test Azure AD single sign-on with Rally Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="fd0db-137">Azure AD måste du känna till motsvarande användaren i Rally programvara till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="fd0db-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Rally Software is to a user in Azure AD.</span></span> <span data-ttu-id="fd0db-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Rally programvara upprättas.</span><span class="sxs-lookup"><span data-stu-id="fd0db-138">In other words, a link relationship between an Azure AD user and the related user in Rally Software needs to be established.</span></span>

<span data-ttu-id="fd0db-139">I Rally programvara, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="fd0db-139">In Rally Software, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="fd0db-140">Om du vill konfigurera och testa Azure AD enkel inloggning med Rally programvara, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="fd0db-140">To configure and test Azure AD single sign-on with Rally Software, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="fd0db-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="fd0db-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="fd0db-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fd0db-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fd0db-143">**[Skapa en testanvändare Rally programvara](#create-a-rally-software-test-user)**  – du har en motsvarighet för Britta Simon Rally programvara som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="fd0db-143">**[Create a Rally Software test user](#create-a-rally-software-test-user)** - to have a counterpart of Britta Simon in Rally Software that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="fd0db-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="fd0db-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fd0db-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="fd0db-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="fd0db-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fd0db-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="fd0db-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i Rally programmet.</span><span class="sxs-lookup"><span data-stu-id="fd0db-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Rally Software application.</span></span>

<span data-ttu-id="fd0db-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Rally programvara:**</span><span class="sxs-lookup"><span data-stu-id="fd0db-148">**To configure Azure AD single sign-on with Rally Software, perform the following steps:**</span></span>

1. <span data-ttu-id="fd0db-149">I Azure-portalen på den **Rally programvara** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="fd0db-149">In the Azure portal, on the **Rally Software** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="fd0db-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="fd0db-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_samlbase.png)

3. <span data-ttu-id="fd0db-153">På den **Rally programvara domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="fd0db-153">On the **Rally Software Domain and URLs** section, perform the following steps:</span></span>

    ![Rally programvara domän och URL: er enkel inloggning information](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_url.png)

    <span data-ttu-id="fd0db-155">a.</span><span class="sxs-lookup"><span data-stu-id="fd0db-155">a.</span></span> <span data-ttu-id="fd0db-156">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<tenant-name>.rally.com`</span><span class="sxs-lookup"><span data-stu-id="fd0db-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant-name>.rally.com`</span></span>

    <span data-ttu-id="fd0db-157">b.</span><span class="sxs-lookup"><span data-stu-id="fd0db-157">b.</span></span> <span data-ttu-id="fd0db-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<tenant-name>.rally.com`</span><span class="sxs-lookup"><span data-stu-id="fd0db-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<tenant-name>.rally.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="fd0db-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="fd0db-159">These values are not real.</span></span> <span data-ttu-id="fd0db-160">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="fd0db-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="fd0db-161">Kontakta [Rally Programvaruklienten supportteamet](https://help.rallydev.com/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="fd0db-161">Contact [Rally Software Client support team](https://help.rallydev.com/) to get these values.</span></span> 
 


4. <span data-ttu-id="fd0db-162">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="fd0db-162">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_certificate.png) 

5. <span data-ttu-id="fd0db-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="fd0db-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-rally-software-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="fd0db-166">På den **Rally programvarukonfiguration** klickar du på **konfigurera Rally programvara** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="fd0db-166">On the **Rally Software Configuration** section, click **Configure Rally Software** to open **Configure sign-on** window.</span></span> <span data-ttu-id="fd0db-167">Kopiera den **Sign-Out URL och SAML enhets-ID** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="fd0db-167">Copy the **Sign-Out URL, and SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![Rally konfiguration av programvara](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_configure.png) 

7. <span data-ttu-id="fd0db-169">Logga in på ditt **Rally programvara** klient.</span><span class="sxs-lookup"><span data-stu-id="fd0db-169">Log in to your **Rally Software** tenant.</span></span>

8. <span data-ttu-id="fd0db-170">Klicka på i verktygsfältet högst upp **installationsprogrammet**, och välj sedan **prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="fd0db-170">In the toolbar on the top, click **Setup**, and then select **Subscription**.</span></span>
   
    <span data-ttu-id="fd0db-171">![Prenumerationen](./media/active-directory-saas-rally-software-tutorial/ic769531.png "prenumeration")</span><span class="sxs-lookup"><span data-stu-id="fd0db-171">![Subscription](./media/active-directory-saas-rally-software-tutorial/ic769531.png "Subscription")</span></span>

9. <span data-ttu-id="fd0db-172">Klicka på den **åtgärd** knappen.</span><span class="sxs-lookup"><span data-stu-id="fd0db-172">Click the **Action** button.</span></span> <span data-ttu-id="fd0db-173">Välj **redigera prenumeration** på upp till höger i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="fd0db-173">Select **Edit Subscription** at the top right side of the toolbar.</span></span>

10. <span data-ttu-id="fd0db-174">På den **prenumeration** dialogrutan sida, utför följande steg och klicka sedan på **spara och Stäng**:</span><span class="sxs-lookup"><span data-stu-id="fd0db-174">On the **Subscription** dialog page, perform the following steps, and then click **Save & Close**:</span></span>
   
    <span data-ttu-id="fd0db-175">![Autentisering](./media/active-directory-saas-rally-software-tutorial/ic769542.png "autentisering")</span><span class="sxs-lookup"><span data-stu-id="fd0db-175">![Authentication](./media/active-directory-saas-rally-software-tutorial/ic769542.png "Authentication")</span></span>
   
    <span data-ttu-id="fd0db-176">a.</span><span class="sxs-lookup"><span data-stu-id="fd0db-176">a.</span></span> <span data-ttu-id="fd0db-177">Välj **autentisering Rally eller SSO** autentisering listrutan.</span><span class="sxs-lookup"><span data-stu-id="fd0db-177">Select **Rally or SSO authentication** from Authentication dropdown.</span></span>

    <span data-ttu-id="fd0db-178">b.</span><span class="sxs-lookup"><span data-stu-id="fd0db-178">b.</span></span> <span data-ttu-id="fd0db-179">I den **identitet providern URL** textruta klistra in värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fd0db-179">In the **Identity provider URL** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="fd0db-180">c.</span><span class="sxs-lookup"><span data-stu-id="fd0db-180">c.</span></span> <span data-ttu-id="fd0db-181">I den **SSO logga ut** textruta klistra in värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fd0db-181">In the **SSO Logout** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

> [!TIP]
> <span data-ttu-id="fd0db-182">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="fd0db-182">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="fd0db-183">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="fd0db-183">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="fd0db-184">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fd0db-184">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="fd0db-185">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="fd0db-185">Create an Azure AD test user</span></span>

<span data-ttu-id="fd0db-186">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fd0db-186">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="fd0db-188">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="fd0db-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="fd0db-189">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="fd0db-189">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-rally-software-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="fd0db-191">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="fd0db-191">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-rally-software-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="fd0db-193">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fd0db-193">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-rally-software-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="fd0db-195">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="fd0db-195">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-rally-software-tutorial/create_aaduser_04.png)

    <span data-ttu-id="fd0db-197">a.</span><span class="sxs-lookup"><span data-stu-id="fd0db-197">a.</span></span> <span data-ttu-id="fd0db-198">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fd0db-198">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fd0db-199">b.</span><span class="sxs-lookup"><span data-stu-id="fd0db-199">b.</span></span> <span data-ttu-id="fd0db-200">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="fd0db-200">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="fd0db-201">c.</span><span class="sxs-lookup"><span data-stu-id="fd0db-201">c.</span></span> <span data-ttu-id="fd0db-202">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="fd0db-202">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="fd0db-203">d.</span><span class="sxs-lookup"><span data-stu-id="fd0db-203">d.</span></span> <span data-ttu-id="fd0db-204">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="fd0db-204">Click **Create**.</span></span>
 
### <a name="create-a-rally-software-test-user"></a><span data-ttu-id="fd0db-205">Skapa en testanvändare Rally programvara</span><span class="sxs-lookup"><span data-stu-id="fd0db-205">Create a Rally Software test user</span></span>

<span data-ttu-id="fd0db-206">För Azure AD-användare för att kunna logga in, måste de etableras till Rally-programmet med hjälp av deras användarnamn för Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="fd0db-206">For Azure AD users to be able to sign in, they must be provisioned to the Rally Software application using their Azure Active Directory user names.</span></span>

<span data-ttu-id="fd0db-207">**Utför följande steg för att konfigurera användaretablering:**</span><span class="sxs-lookup"><span data-stu-id="fd0db-207">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="fd0db-208">Logga in på din klient Rally programvara.</span><span class="sxs-lookup"><span data-stu-id="fd0db-208">Log in to your Rally Software tenant.</span></span>

2. <span data-ttu-id="fd0db-209">Gå till **installationsprogrammet \> användare**, och klicka sedan på **+ Lägg till ny**.</span><span class="sxs-lookup"><span data-stu-id="fd0db-209">Go to **Setup \> USERS**, and then click **+ Add New**.</span></span>
   
    <span data-ttu-id="fd0db-210">![Användare](./media/active-directory-saas-rally-software-tutorial/ic781039.png "användare")</span><span class="sxs-lookup"><span data-stu-id="fd0db-210">![Users](./media/active-directory-saas-rally-software-tutorial/ic781039.png "Users")</span></span>

3. <span data-ttu-id="fd0db-211">Ange namnet i textrutan ny användare och klicka sedan på **Lägg till med uppgifter**.</span><span class="sxs-lookup"><span data-stu-id="fd0db-211">Type the name in the New User textbox, and then click **Add with Details**.</span></span>

4. <span data-ttu-id="fd0db-212">I den **skapa användare** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="fd0db-212">In the **Create User** section, perform the following steps:</span></span>
   
    <span data-ttu-id="fd0db-213">![Skapa användare](./media/active-directory-saas-rally-software-tutorial/ic781040.png "skapa användare")</span><span class="sxs-lookup"><span data-stu-id="fd0db-213">![Create User](./media/active-directory-saas-rally-software-tutorial/ic781040.png "Create User")</span></span>

    <span data-ttu-id="fd0db-214">a.</span><span class="sxs-lookup"><span data-stu-id="fd0db-214">a.</span></span> <span data-ttu-id="fd0db-215">I den **användarnamn** textruta, ange namnet på användaren som **Brittsimon**.</span><span class="sxs-lookup"><span data-stu-id="fd0db-215">In the **User Name** textbox, type the name of user like **Brittsimon**.</span></span>
   
    <span data-ttu-id="fd0db-216">b.</span><span class="sxs-lookup"><span data-stu-id="fd0db-216">b.</span></span> <span data-ttu-id="fd0db-217">I **e-postadress** textruta ange e-postadress för användaren som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="fd0db-217">In **E-mail Address** textbox, enter the email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="fd0db-218">c.</span><span class="sxs-lookup"><span data-stu-id="fd0db-218">c.</span></span> <span data-ttu-id="fd0db-219">I **Förnamn** text Ange först namnet på användaren som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="fd0db-219">In **First Name** text box, enter the first name of user like **Britta**.</span></span>

    <span data-ttu-id="fd0db-220">d.</span><span class="sxs-lookup"><span data-stu-id="fd0db-220">d.</span></span> <span data-ttu-id="fd0db-221">I **efternamn** text Ange efternamn för användaren som **Simon**.</span><span class="sxs-lookup"><span data-stu-id="fd0db-221">In **Last Name** text box, enter the last name of user like **Simon**.</span></span>

    <span data-ttu-id="fd0db-222">e.</span><span class="sxs-lookup"><span data-stu-id="fd0db-222">e.</span></span> <span data-ttu-id="fd0db-223">Klicka på **spara och Stäng**.</span><span class="sxs-lookup"><span data-stu-id="fd0db-223">Click **Save & Close**.</span></span>

   >[!NOTE]
   ><span data-ttu-id="fd0db-224">Du kan använda andra Rally användarens konto skapas verktyg eller API: er som tillhandahålls av Rally programvara för att etablera Azure AD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="fd0db-224">You can use any other Rally Software user account creation tools or APIs provided by Rally Software to provision Azure AD user accounts.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="fd0db-225">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="fd0db-225">Assign the Azure AD test user</span></span>

<span data-ttu-id="fd0db-226">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Rally programvara.</span><span class="sxs-lookup"><span data-stu-id="fd0db-226">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Rally Software.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="fd0db-228">**Om du vill tilldela Rally programvara Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="fd0db-228">**To assign Britta Simon to Rally Software, perform the following steps:**</span></span>

1. <span data-ttu-id="fd0db-229">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="fd0db-229">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="fd0db-231">Välj i listan med program **Rally programvara**.</span><span class="sxs-lookup"><span data-stu-id="fd0db-231">In the applications list, select **Rally Software**.</span></span>

    ![Länken Rally programvara i listan med program](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_app.png)  

3. <span data-ttu-id="fd0db-233">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="fd0db-233">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="fd0db-235">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="fd0db-235">Click **Add** button.</span></span> <span data-ttu-id="fd0db-236">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fd0db-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="fd0db-238">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="fd0db-238">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="fd0db-239">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fd0db-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fd0db-240">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fd0db-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="fd0db-241">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fd0db-241">Test single sign-on</span></span>

<span data-ttu-id="fd0db-242">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="fd0db-242">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="fd0db-243">När du klickar på panelen Rally programvara på åtkomstpanelen du bör få automatiskt loggat in på Rally programmet.</span><span class="sxs-lookup"><span data-stu-id="fd0db-243">When you click the Rally Software tile in the Access Panel, you should get automatically signed-on to your Rally Software application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fd0db-244">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="fd0db-244">Additional resources</span></span>

* [<span data-ttu-id="fd0db-245">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fd0db-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fd0db-246">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="fd0db-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_203.png

