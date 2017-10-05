---
title: "Självstudier: Azure Active Directory-integrering med Evidence.com | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Evidence.com."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f9a7cb7c-ff67-40dc-872c-1fa35f9dd03b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: a9c474cfc648fc8a306d736c89a4813d82c133ea
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-evidencecom"></a><span data-ttu-id="00b4e-103">Självstudier: Azure Active Directory-integrering med Evidence.com</span><span class="sxs-lookup"><span data-stu-id="00b4e-103">Tutorial: Azure Active Directory integration with Evidence.com</span></span>

<span data-ttu-id="00b4e-104">I kursen får lära du att integrera Evidence.com med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="00b4e-104">In this tutorial, you learn how to integrate Evidence.com with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="00b4e-105">Integrera Evidence.com med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="00b4e-105">Integrating Evidence.com with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="00b4e-106">Du kan styra i Azure AD som har åtkomst till Evidence.com.</span><span class="sxs-lookup"><span data-stu-id="00b4e-106">You can control in Azure AD who has access to Evidence.com.</span></span>
- <span data-ttu-id="00b4e-107">Du kan aktivera användarna att automatiskt hämta loggat in på Evidence.com (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="00b4e-107">You can enable your users to automatically get signed-on to Evidence.com (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="00b4e-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="00b4e-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="00b4e-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="00b4e-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="00b4e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="00b4e-110">Prerequisites</span></span>

<span data-ttu-id="00b4e-111">För att konfigurera Azure AD-integrering med Evidence.com, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="00b4e-111">To configure Azure AD integration with Evidence.com, you need the following items:</span></span>

- <span data-ttu-id="00b4e-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="00b4e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="00b4e-113">En Evidence.com enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="00b4e-113">A Evidence.com single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="00b4e-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="00b4e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="00b4e-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="00b4e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="00b4e-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="00b4e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="00b4e-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="00b4e-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="00b4e-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="00b4e-118">Scenario description</span></span>
<span data-ttu-id="00b4e-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="00b4e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="00b4e-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="00b4e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="00b4e-121">Att lägga till Evidence.com från galleriet</span><span class="sxs-lookup"><span data-stu-id="00b4e-121">Adding Evidence.com from the gallery</span></span>
2. <span data-ttu-id="00b4e-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="00b4e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-evidencecom-from-the-gallery"></a><span data-ttu-id="00b4e-123">Att lägga till Evidence.com från galleriet</span><span class="sxs-lookup"><span data-stu-id="00b4e-123">Adding Evidence.com from the gallery</span></span>
<span data-ttu-id="00b4e-124">Du måste lägga till Evidence.com från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Evidence.com i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="00b4e-124">To configure the integration of Evidence.com into Azure AD, you need to add Evidence.com from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="00b4e-125">**Utför följande steg för att lägga till Evidence.com från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="00b4e-125">**To add Evidence.com from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="00b4e-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="00b4e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="00b4e-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="00b4e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="00b4e-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="00b4e-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="00b4e-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="00b4e-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="00b4e-133">I sökrutan skriver **Evidence.com**väljer **Evidence.com** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="00b4e-133">In the search box, type **Evidence.com**, select **Evidence.com** from result panel then click **Add** button to add the application.</span></span>

    ![Evidence.com i resultatlistan](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="00b4e-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="00b4e-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="00b4e-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Evidence.com baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="00b4e-136">In this section, you configure and test Azure AD single sign-on with Evidence.com based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="00b4e-137">Azure AD måste du känna till användaren i Evidence.com motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="00b4e-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Evidence.com is to a user in Azure AD.</span></span> <span data-ttu-id="00b4e-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Evidence.com upprättas.</span><span class="sxs-lookup"><span data-stu-id="00b4e-138">In other words, a link relationship between an Azure AD user and the related user in Evidence.com needs to be established.</span></span>

<span data-ttu-id="00b4e-139">I Evidence.com, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="00b4e-139">In Evidence.com, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="00b4e-140">Om du vill konfigurera och testa Azure AD enkel inloggning med Evidence.com, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="00b4e-140">To configure and test Azure AD single sign-on with Evidence.com, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="00b4e-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="00b4e-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="00b4e-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="00b4e-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="00b4e-143">**[Skapa en testanvändare Evidence.com](#create-a-evidencecom-test-user)**  – du har en motsvarighet för Britta Simon i Evidence.com som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="00b4e-143">**[Create a Evidence.com test user](#create-a-evidencecom-test-user)** - to have a counterpart of Britta Simon in Evidence.com that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="00b4e-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="00b4e-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="00b4e-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="00b4e-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="00b4e-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="00b4e-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="00b4e-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Evidence.com program.</span><span class="sxs-lookup"><span data-stu-id="00b4e-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Evidence.com application.</span></span>

<span data-ttu-id="00b4e-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Evidence.com:**</span><span class="sxs-lookup"><span data-stu-id="00b4e-148">**To configure Azure AD single sign-on with Evidence.com, perform the following steps:**</span></span>

1. <span data-ttu-id="00b4e-149">I Azure-portalen på den **Evidence.com** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="00b4e-149">In the Azure portal, on the **Evidence.com** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="00b4e-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="00b4e-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_samlbase.png)

3. <span data-ttu-id="00b4e-153">På den **Evidence.com domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="00b4e-153">On the **Evidence.com Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och evidence.com domän med enkel inloggning information](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_url.png)

    <span data-ttu-id="00b4e-155">a.</span><span class="sxs-lookup"><span data-stu-id="00b4e-155">a.</span></span> <span data-ttu-id="00b4e-156">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<yourtenant>.evidence.com`</span><span class="sxs-lookup"><span data-stu-id="00b4e-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<yourtenant>.evidence.com`</span></span>

    <span data-ttu-id="00b4e-157">b.</span><span class="sxs-lookup"><span data-stu-id="00b4e-157">b.</span></span> <span data-ttu-id="00b4e-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<yourtenant>.evidence.com`</span><span class="sxs-lookup"><span data-stu-id="00b4e-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<yourtenant>.evidence.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="00b4e-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="00b4e-159">These values are not real.</span></span> <span data-ttu-id="00b4e-160">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="00b4e-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="00b4e-161">Kontakta [Evidence.com klienten supportteamet](https://communities.taser.com/support/SupportContactUs?typ=LE) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="00b4e-161">Contact [Evidence.com Client support team](https://communities.taser.com/support/SupportContactUs?typ=LE) to get these values.</span></span> 

4. <span data-ttu-id="00b4e-162">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="00b4e-162">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_certificate.png) 

5. <span data-ttu-id="00b4e-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="00b4e-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-evidence-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="00b4e-166">På den **Evidence.com Configuration** klickar du på **konfigurera Evidence.com** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="00b4e-166">On the **Evidence.com Configuration** section, click **Configure Evidence.com** to open **Configure sign-on** window.</span></span> <span data-ttu-id="00b4e-167">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="00b4e-167">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Evidence.com konfiguration](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_configure.png) 

7. <span data-ttu-id="00b4e-169">I ett separat webbläsarfönster logga in på ditt Evidence.com klient som administratör och gå till **Admin** fliken</span><span class="sxs-lookup"><span data-stu-id="00b4e-169">In a separate web browser window, login to your Evidence.com tenant as an administrator and navigate to **Admin** Tab</span></span>

8. <span data-ttu-id="00b4e-170">Klicka på **Agency för enkel inloggning**</span><span class="sxs-lookup"><span data-stu-id="00b4e-170">Click on **Agency Single Sign On**</span></span>

9. <span data-ttu-id="00b4e-171">Välj **SAML utifrån för enkel inloggning**</span><span class="sxs-lookup"><span data-stu-id="00b4e-171">Select **SAML Based Single Sign On**</span></span>

10. <span data-ttu-id="00b4e-172">Kopiera den **SAML enhets-ID**, **SAML enkel inloggning Tjänstwebbadress** och **Sign-Out URL** värden som visas i Azure-portalen och motsvarande fält i Evidence.com.</span><span class="sxs-lookup"><span data-stu-id="00b4e-172">Copy the **SAML Entity ID**, **SAML Single Sign-On Service URL** and **Sign-Out URL** values shown in the Azure portal and to the corresponding fields in Evidence.com.</span></span>

11. <span data-ttu-id="00b4e-173">Öppna filen hämtade Certificate(Base64) i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **säkerhetscertifikat** rutan.</span><span class="sxs-lookup"><span data-stu-id="00b4e-173">Open your downloaded Certificate(Base64) file in notepad, copy the content of it into your clipboard, and then paste it to the **Security Certificate** box.</span></span> 

12. <span data-ttu-id="00b4e-174">Spara konfigurationen i Evidence.com.</span><span class="sxs-lookup"><span data-stu-id="00b4e-174">Save the configuration in Evidence.com.</span></span>

> [!TIP]
> <span data-ttu-id="00b4e-175">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="00b4e-175">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="00b4e-176">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="00b4e-176">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="00b4e-177">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="00b4e-177">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="00b4e-178">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="00b4e-178">Create an Azure AD test user</span></span>

<span data-ttu-id="00b4e-179">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="00b4e-179">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="00b4e-181">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="00b4e-181">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="00b4e-182">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="00b4e-182">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-evidence-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="00b4e-184">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="00b4e-184">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-evidence-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="00b4e-186">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="00b4e-186">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-evidence-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="00b4e-188">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="00b4e-188">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-evidence-tutorial/create_aaduser_04.png)

    <span data-ttu-id="00b4e-190">a.</span><span class="sxs-lookup"><span data-stu-id="00b4e-190">a.</span></span> <span data-ttu-id="00b4e-191">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="00b4e-191">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="00b4e-192">b.</span><span class="sxs-lookup"><span data-stu-id="00b4e-192">b.</span></span> <span data-ttu-id="00b4e-193">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="00b4e-193">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="00b4e-194">c.</span><span class="sxs-lookup"><span data-stu-id="00b4e-194">c.</span></span> <span data-ttu-id="00b4e-195">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="00b4e-195">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="00b4e-196">d.</span><span class="sxs-lookup"><span data-stu-id="00b4e-196">d.</span></span> <span data-ttu-id="00b4e-197">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="00b4e-197">Click **Create**.</span></span>
 
### <a name="create-a-evidencecom-test-user"></a><span data-ttu-id="00b4e-198">Skapa en testanvändare Evidence.com</span><span class="sxs-lookup"><span data-stu-id="00b4e-198">Create a Evidence.com test user</span></span>

<span data-ttu-id="00b4e-199">För Azure AD-användare för att kunna logga in, måste de etableras för åtkomst i Evidence.com-programmet.</span><span class="sxs-lookup"><span data-stu-id="00b4e-199">For Azure AD users to be able to sign in, they must be provisioned for access inside the Evidence.com application.</span></span> <span data-ttu-id="00b4e-200">Det här avsnittet beskriver hur du skapar användarkonton i Azure AD i Evidence.com</span><span class="sxs-lookup"><span data-stu-id="00b4e-200">This section describes how to create Azure AD user accounts inside Evidence.com</span></span>

<span data-ttu-id="00b4e-201">**Att tillhandahålla ett användarkonto i Evidence.com:**</span><span class="sxs-lookup"><span data-stu-id="00b4e-201">**To provision a user account in Evidence.com:**</span></span>

1. <span data-ttu-id="00b4e-202">Logga in på webbplatsen Evidence.com företag som en administratör i ett webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="00b4e-202">In a web browser window, log into your Evidence.com company site as an administrator.</span></span>

2. <span data-ttu-id="00b4e-203">Gå till **Admin** fliken.</span><span class="sxs-lookup"><span data-stu-id="00b4e-203">Navigate to **Admin** tab.</span></span>

3. <span data-ttu-id="00b4e-204">Klicka på **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="00b4e-204">Click on **Add User**.</span></span>

4. <span data-ttu-id="00b4e-205">Klicka på den **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="00b4e-205">Click the **Add** button.</span></span>

5. <span data-ttu-id="00b4e-206">Den **e-postadress** för den tillagda användaren måste matcha användarnamnet för användare i Azure AD som du vill ge åtkomst.</span><span class="sxs-lookup"><span data-stu-id="00b4e-206">The **Email Address** of the added user must match the username of the users in Azure AD who you wish to give access.</span></span> <span data-ttu-id="00b4e-207">Om användarnamnet och e-postadress inte är samma värde i din organisation, kan du använda den **Evidence.com > attribut > enkel inloggning** avsnitt i Azure portal för att ändra nameidenitifer skickas till Evidence.com för att vara e-postadress.</span><span class="sxs-lookup"><span data-stu-id="00b4e-207">If the username and email address are not the same value in your organization, you can use the **Evidence.com > Attributes > Single Sign-On** section of the Azure portal to change the nameidenitifer sent to Evidence.com to be the email address.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="00b4e-208">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="00b4e-208">Assign the Azure AD test user</span></span>

<span data-ttu-id="00b4e-209">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Evidence.com.</span><span class="sxs-lookup"><span data-stu-id="00b4e-209">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Evidence.com.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="00b4e-211">**Om du vill tilldela Evidence.com Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="00b4e-211">**To assign Britta Simon to Evidence.com, perform the following steps:**</span></span>

1. <span data-ttu-id="00b4e-212">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="00b4e-212">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="00b4e-214">Välj i listan med program **Evidence.com**.</span><span class="sxs-lookup"><span data-stu-id="00b4e-214">In the applications list, select **Evidence.com**.</span></span>

    ![Länken Evidence.com i listan med program](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_app.png)  

3. <span data-ttu-id="00b4e-216">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="00b4e-216">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="00b4e-218">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="00b4e-218">Click **Add** button.</span></span> <span data-ttu-id="00b4e-219">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="00b4e-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="00b4e-221">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="00b4e-221">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="00b4e-222">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="00b4e-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="00b4e-223">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="00b4e-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="00b4e-224">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="00b4e-224">Test single sign-on</span></span>

<span data-ttu-id="00b4e-225">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="00b4e-225">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="00b4e-226">När du klickar på panelen Evidence.com på åtkomstpanelen du bör få automatiskt loggat in på ditt Evidence.com program.</span><span class="sxs-lookup"><span data-stu-id="00b4e-226">When you click the Evidence.com tile in the Access Panel, you should get automatically signed-on to your Evidence.com application.</span></span>
<span data-ttu-id="00b4e-227">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="00b4e-227">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="00b4e-228">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="00b4e-228">Additional resources</span></span>

* [<span data-ttu-id="00b4e-229">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="00b4e-229">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="00b4e-230">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="00b4e-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_203.png

