---
title: "Självstudier: Azure Active Directory-integrering med Predictix pris Reporting | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Predictix pris Reporting."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 691d0c43-3aa1-4220-9e46-e7a88db234ad
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: 62280b205f2fd691e8c75134585172b995377311
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-predictix-price-reporting"></a><span data-ttu-id="80334-103">Självstudier: Azure Active Directory-integrering med Predictix pris rapportering</span><span class="sxs-lookup"><span data-stu-id="80334-103">Tutorial: Azure Active Directory integration with Predictix Price Reporting</span></span>

<span data-ttu-id="80334-104">I kursen får lära du att integrera Predictix pris rapportering med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="80334-104">In this tutorial, you learn how to integrate Predictix Price Reporting with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="80334-105">Integrera Predictix pris rapportering med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="80334-105">Integrating Predictix Price Reporting with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="80334-106">Du kan styra i Azure AD som har åtkomst till Predictix pris Reporting.</span><span class="sxs-lookup"><span data-stu-id="80334-106">You can control in Azure AD who has access to Predictix Price Reporting.</span></span>
- <span data-ttu-id="80334-107">Du kan aktivera användarna att automatiskt hämta loggat in på Predictix pris Reporting (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="80334-107">You can enable your users to automatically get signed-on to Predictix Price Reporting (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="80334-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="80334-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="80334-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="80334-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="80334-110">Krav</span><span class="sxs-lookup"><span data-stu-id="80334-110">Prerequisites</span></span>

<span data-ttu-id="80334-111">Om du vill konfigurera Azure AD-integrering med Predictix pris rapportering behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="80334-111">To configure Azure AD integration with Predictix Price Reporting, you need the following items:</span></span>

- <span data-ttu-id="80334-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="80334-112">An Azure AD subscription</span></span>
- <span data-ttu-id="80334-113">En Predictix pris Reporting enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="80334-113">A Predictix Price Reporting single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="80334-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="80334-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="80334-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="80334-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="80334-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="80334-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="80334-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="80334-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="80334-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="80334-118">Scenario description</span></span>
<span data-ttu-id="80334-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="80334-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="80334-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="80334-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="80334-121">Att lägga till Predictix pris Reporting från galleriet</span><span class="sxs-lookup"><span data-stu-id="80334-121">Adding Predictix Price Reporting from the gallery</span></span>
2. <span data-ttu-id="80334-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="80334-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-predictix-price-reporting-from-the-gallery"></a><span data-ttu-id="80334-123">Att lägga till Predictix pris Reporting från galleriet</span><span class="sxs-lookup"><span data-stu-id="80334-123">Adding Predictix Price Reporting from the gallery</span></span>
<span data-ttu-id="80334-124">Du måste lägga till Predictix pris Reporting från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Predictix pris Reporting i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="80334-124">To configure the integration of Predictix Price Reporting into Azure AD, you need to add Predictix Price Reporting from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="80334-125">**Utför följande steg för att lägga till Predictix pris Reporting från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="80334-125">**To add Predictix Price Reporting from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="80334-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="80334-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="80334-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="80334-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="80334-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="80334-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="80334-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="80334-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="80334-133">I sökrutan skriver **Predictix pris Reporting**väljer **Predictix pris Reporting** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="80334-133">In the search box, type **Predictix Price Reporting**, select **Predictix Price Reporting** from result panel then click **Add** button to add the application.</span></span>

    ![Predictix pris rapportering i resultatlistan](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="80334-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="80334-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="80334-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Predictix pris Reporting baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="80334-136">In this section, you configure and test Azure AD single sign-on with Predictix Price Reporting based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="80334-137">Azure AD måste du känna till motsvarande användaren i Predictix pris Reporting till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="80334-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Predictix Price Reporting is to a user in Azure AD.</span></span> <span data-ttu-id="80334-138">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i Predictix pris Reporting upprättas.</span><span class="sxs-lookup"><span data-stu-id="80334-138">In other words, a link relationship between an Azure AD user and the related user in Predictix Price Reporting needs to be established.</span></span>

<span data-ttu-id="80334-139">I Predictix pris Reporting, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="80334-139">In Predictix Price Reporting, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="80334-140">Om du vill konfigurera och testa Azure AD enkel inloggning med Predictix pris rapportering, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="80334-140">To configure and test Azure AD single sign-on with Predictix Price Reporting, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="80334-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="80334-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="80334-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="80334-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="80334-143">**[Skapa en testanvändare Predictix pris Reporting](#create-a-predictix-price-reporting-test-user)**  – du har en motsvarighet för Britta Simon i Predictix pris rapportering som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="80334-143">**[Create a Predictix Price Reporting test user](#create-a-predictix-price-reporting-test-user)** - to have a counterpart of Britta Simon in Predictix Price Reporting that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="80334-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="80334-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="80334-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="80334-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="80334-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="80334-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="80334-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Predictix pris Reporting.</span><span class="sxs-lookup"><span data-stu-id="80334-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Predictix Price Reporting application.</span></span>

<span data-ttu-id="80334-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Predictix pris Reporting:**</span><span class="sxs-lookup"><span data-stu-id="80334-148">**To configure Azure AD single sign-on with Predictix Price Reporting, perform the following steps:**</span></span>

1. <span data-ttu-id="80334-149">I Azure-portalen på den **Predictix pris Reporting** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="80334-149">In the Azure portal, on the **Predictix Price Reporting** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="80334-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="80334-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_samlbase.png)

3. <span data-ttu-id="80334-153">På den **Predictix pris Reporting domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="80334-153">On the **Predictix Price Reporting Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och Predictix pris Reporting domän med enkel inloggning information](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_url.png)

    <span data-ttu-id="80334-155">a.</span><span class="sxs-lookup"><span data-stu-id="80334-155">a.</span></span> <span data-ttu-id="80334-156">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname-pricing>.predictix.com/sso/request`</span><span class="sxs-lookup"><span data-stu-id="80334-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname-pricing>.predictix.com/sso/request`</span></span>

    <span data-ttu-id="80334-157">b.</span><span class="sxs-lookup"><span data-stu-id="80334-157">b.</span></span> <span data-ttu-id="80334-158">I den **identifierare** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="80334-158">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname-pricing>.predictix.com` |
    | `https://<companyname-pricing>.dev.predictix.com` |

    > [!NOTE] 
    > <span data-ttu-id="80334-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="80334-159">These values are not real.</span></span> <span data-ttu-id="80334-160">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="80334-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="80334-161">Kontakta [Predictix pris Reporting klienten supportteamet](http://www.infor.com/company/customer-center/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="80334-161">Contact [Predictix Price Reporting Client support team](http://www.infor.com/company/customer-center/) to get these values.</span></span> 
 
4. <span data-ttu-id="80334-162">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="80334-162">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_certificate.png) 

5. <span data-ttu-id="80334-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="80334-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="80334-166">På den **Predictix pris Reporting Configuration** klickar du på **konfigurera Predictix pris Reporting** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="80334-166">On the **Predictix Price Reporting Configuration** section, click **Configure Predictix Price Reporting** to open **Configure sign-on** window.</span></span> <span data-ttu-id="80334-167">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="80334-167">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfiguration av Felrapportering Predictix pris](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_configure.png) 

7. <span data-ttu-id="80334-169">Konfigurera enkel inloggning på **Predictix pris Reporting** sida, måste du skicka den hämtade **certifikat (Base64)**, **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning tjänst-URL**  till [Predictix pris Reporting supportteamet](http://www.infor.com/company/customer-center/).</span><span class="sxs-lookup"><span data-stu-id="80334-169">To configure single sign-on on **Predictix Price Reporting** side, you need to send the downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Predictix Price Reporting support team](http://www.infor.com/company/customer-center/).</span></span> <span data-ttu-id="80334-170">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="80334-170">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="80334-171">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="80334-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="80334-172">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="80334-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="80334-173">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="80334-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="80334-174">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="80334-174">Create an Azure AD test user</span></span>

<span data-ttu-id="80334-175">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="80334-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="80334-177">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="80334-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="80334-178">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="80334-178">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-predictixpricereporting-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="80334-180">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="80334-180">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-predictixpricereporting-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="80334-182">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="80334-182">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-predictixpricereporting-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="80334-184">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="80334-184">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-predictixpricereporting-tutorial/create_aaduser_04.png)

    <span data-ttu-id="80334-186">a.</span><span class="sxs-lookup"><span data-stu-id="80334-186">a.</span></span> <span data-ttu-id="80334-187">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="80334-187">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="80334-188">b.</span><span class="sxs-lookup"><span data-stu-id="80334-188">b.</span></span> <span data-ttu-id="80334-189">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="80334-189">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="80334-190">c.</span><span class="sxs-lookup"><span data-stu-id="80334-190">c.</span></span> <span data-ttu-id="80334-191">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="80334-191">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="80334-192">d.</span><span class="sxs-lookup"><span data-stu-id="80334-192">d.</span></span> <span data-ttu-id="80334-193">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="80334-193">Click **Create**.</span></span>
 
### <a name="create-a-predictix-price-reporting-test-user"></a><span data-ttu-id="80334-194">Skapa en testanvändare Predictix pris Reporting</span><span class="sxs-lookup"><span data-stu-id="80334-194">Create a Predictix Price Reporting test user</span></span>

<span data-ttu-id="80334-195">I det här avsnittet kan du skapa en användare som kallas Britta Simon i Predictix pris Reporting.</span><span class="sxs-lookup"><span data-stu-id="80334-195">In this section, you create a user called Britta Simon in Predictix Price Reporting.</span></span> <span data-ttu-id="80334-196">Arbeta med [Predictix pris Reporting supportteamet](http://www.infor.com/company/customer-center/) att lägga till användare i Predictix pris Reporting-plattformen.</span><span class="sxs-lookup"><span data-stu-id="80334-196">Work with [Predictix Price Reporting support team](http://www.infor.com/company/customer-center/) to add the users in the Predictix Price Reporting platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="80334-197">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="80334-197">Assign the Azure AD test user</span></span>

<span data-ttu-id="80334-198">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Predictix pris Reporting.</span><span class="sxs-lookup"><span data-stu-id="80334-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Predictix Price Reporting.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="80334-200">**Om du vill tilldela Britta Simon Predictix pris Reporting, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="80334-200">**To assign Britta Simon to Predictix Price Reporting, perform the following steps:**</span></span>

1. <span data-ttu-id="80334-201">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="80334-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="80334-203">Välj i listan med program **Predictix pris Reporting**.</span><span class="sxs-lookup"><span data-stu-id="80334-203">In the applications list, select **Predictix Price Reporting**.</span></span>

    ![Länken Predictix pris rapportering i listan med program](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_app.png)  

3. <span data-ttu-id="80334-205">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="80334-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="80334-207">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="80334-207">Click **Add** button.</span></span> <span data-ttu-id="80334-208">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="80334-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="80334-210">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="80334-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="80334-211">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="80334-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="80334-212">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="80334-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="80334-213">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="80334-213">Test single sign-on</span></span>

<span data-ttu-id="80334-214">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="80334-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="80334-215">När du klickar på panelen Predictix pris Reporting på åtkomstpanelen du bör få automatiskt loggat in på programmet Predictix pris Reporting.</span><span class="sxs-lookup"><span data-stu-id="80334-215">When you click the Predictix Price Reporting tile in the Access Panel, you should get automatically signed-on to your Predictix Price Reporting application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="80334-216">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="80334-216">Additional resources</span></span>

* [<span data-ttu-id="80334-217">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="80334-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="80334-218">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="80334-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_203.png

