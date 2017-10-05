---
title: "Självstudier: Azure Active Directory-integrering med YouEarnedIt | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och YouEarnedIt."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3011d44d-dfcf-4061-888f-cff90fbc8150
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeedes
ms.openlocfilehash: c29d218dbca581f102caf8070fa40894e7006e71
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-youearnedit"></a><span data-ttu-id="437a7-103">Självstudier: Azure Active Directory-integrering med YouEarnedIt</span><span class="sxs-lookup"><span data-stu-id="437a7-103">Tutorial: Azure Active Directory integration with YouEarnedIt</span></span>

<span data-ttu-id="437a7-104">I kursen får lära du att integrera YouEarnedIt med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="437a7-104">In this tutorial, you learn how to integrate YouEarnedIt with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="437a7-105">Integrera YouEarnedIt med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="437a7-105">Integrating YouEarnedIt with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="437a7-106">Du kan styra i Azure AD som har åtkomst till YouEarnedIt.</span><span class="sxs-lookup"><span data-stu-id="437a7-106">You can control in Azure AD who has access to YouEarnedIt.</span></span>
- <span data-ttu-id="437a7-107">Du kan aktivera användarna att automatiskt hämta loggat in på YouEarnedIt (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="437a7-107">You can enable your users to automatically get signed-on to YouEarnedIt (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="437a7-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="437a7-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="437a7-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="437a7-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="437a7-110">Krav</span><span class="sxs-lookup"><span data-stu-id="437a7-110">Prerequisites</span></span>

<span data-ttu-id="437a7-111">För att konfigurera Azure AD-integrering med YouEarnedIt, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="437a7-111">To configure Azure AD integration with YouEarnedIt, you need the following items:</span></span>

- <span data-ttu-id="437a7-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="437a7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="437a7-113">En YouEarnedIt enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="437a7-113">A YouEarnedIt single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="437a7-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="437a7-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="437a7-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="437a7-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="437a7-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="437a7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="437a7-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="437a7-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="437a7-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="437a7-118">Scenario description</span></span>
<span data-ttu-id="437a7-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="437a7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="437a7-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="437a7-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="437a7-121">Att lägga till YouEarnedIt från galleriet</span><span class="sxs-lookup"><span data-stu-id="437a7-121">Adding YouEarnedIt from the gallery</span></span>
2. <span data-ttu-id="437a7-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="437a7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-youearnedit-from-the-gallery"></a><span data-ttu-id="437a7-123">Att lägga till YouEarnedIt från galleriet</span><span class="sxs-lookup"><span data-stu-id="437a7-123">Adding YouEarnedIt from the gallery</span></span>
<span data-ttu-id="437a7-124">Du måste lägga till YouEarnedIt från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av YouEarnedIt i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="437a7-124">To configure the integration of YouEarnedIt into Azure AD, you need to add YouEarnedIt from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="437a7-125">**Utför följande steg för att lägga till YouEarnedIt från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="437a7-125">**To add YouEarnedIt from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="437a7-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="437a7-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="437a7-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="437a7-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="437a7-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="437a7-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="437a7-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="437a7-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="437a7-133">I sökrutan skriver **YouEarnedt**väljer **YouEarnedt** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="437a7-133">In the search box, type **YouEarnedt**, select  **YouEarnedt**  from result panel then click **Add** button to add the application.</span></span>

    ![YouEarnedIt i resultatlistan](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="437a7-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="437a7-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="437a7-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med YouEarnedIt baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="437a7-136">In this section, you configure and test Azure AD single sign-on with YouEarnedIt based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="437a7-137">Azure AD måste du känna till användaren i YouEarnedIt motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="437a7-137">For single sign-on to work, Azure AD needs to know what the counterpart user in YouEarnedIt is to a user in Azure AD.</span></span> <span data-ttu-id="437a7-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i YouEarnedIt upprättas.</span><span class="sxs-lookup"><span data-stu-id="437a7-138">In other words, a link relationship between an Azure AD user and the related user in YouEarnedIt needs to be established.</span></span>

<span data-ttu-id="437a7-139">I YouEarnedIt, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="437a7-139">In YouEarnedIt, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="437a7-140">Om du vill konfigurera och testa Azure AD enkel inloggning med YouEarnedIt, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="437a7-140">To configure and test Azure AD single sign-on with YouEarnedIt, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="437a7-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="437a7-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="437a7-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="437a7-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="437a7-143">**[Skapa en testanvändare YouEarnedIt](#create-a-youearnedit-test-user)**  – du har en motsvarighet för Britta Simon i YouEarnedIt som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="437a7-143">**[Create a YouEarnedIt test user](#create-a-youearnedit-test-user)** - to have a counterpart of Britta Simon in YouEarnedIt that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="437a7-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="437a7-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="437a7-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="437a7-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="437a7-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="437a7-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="437a7-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt YouEarnedIt program.</span><span class="sxs-lookup"><span data-stu-id="437a7-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your YouEarnedIt application.</span></span>

<span data-ttu-id="437a7-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med YouEarnedIt:**</span><span class="sxs-lookup"><span data-stu-id="437a7-148">**To configure Azure AD single sign-on with YouEarnedIt, perform the following steps:**</span></span>

1. <span data-ttu-id="437a7-149">I Azure-portalen på den **YouEarnedIt** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="437a7-149">In the Azure portal, on the **YouEarnedIt** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="437a7-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="437a7-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_samlbase.png)

3. <span data-ttu-id="437a7-153">På den **YouEarnedIt domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="437a7-153">On the **YouEarnedIt Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och YouEarnedIt domän med enkel inloggning information](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_url.png)

    <span data-ttu-id="437a7-155">a.</span><span class="sxs-lookup"><span data-stu-id="437a7-155">a.</span></span> <span data-ttu-id="437a7-156">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="437a7-156">In the **Sign-on URL** textbox, type a URL using the following patterns:</span></span> 
    | <span data-ttu-id="437a7-157">Miljö</span><span class="sxs-lookup"><span data-stu-id="437a7-157">Environment</span></span>  | <span data-ttu-id="437a7-158">Mönstret</span><span class="sxs-lookup"><span data-stu-id="437a7-158">Pattern</span></span>  |
    |:--- |:--- |
    | <span data-ttu-id="437a7-159">Produktion</span><span class="sxs-lookup"><span data-stu-id="437a7-159">Production</span></span> | `https://<company name>.youearnedit.com/users/sign_in` |
    | <span data-ttu-id="437a7-160">Sandbox</span><span class="sxs-lookup"><span data-stu-id="437a7-160">Sandbox</span></span>  |`https://<company name>.sandbox.youearnedit.com/users/sign_in` |

    <span data-ttu-id="437a7-161">b.</span><span class="sxs-lookup"><span data-stu-id="437a7-161">b.</span></span> <span data-ttu-id="437a7-162">I den **identifierare** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="437a7-162">In the **Identifier** textbox, type a URL using the following patterns:</span></span>
    | <span data-ttu-id="437a7-163">Miljö</span><span class="sxs-lookup"><span data-stu-id="437a7-163">Environment</span></span>  | <span data-ttu-id="437a7-164">Mönstret</span><span class="sxs-lookup"><span data-stu-id="437a7-164">Pattern</span></span>  |
    |:--- |:--- |
    | <span data-ttu-id="437a7-165">Produktion</span><span class="sxs-lookup"><span data-stu-id="437a7-165">Production</span></span> | `https://<company name>.youearnedit.com` |
    | <span data-ttu-id="437a7-166">Sandbox</span><span class="sxs-lookup"><span data-stu-id="437a7-166">Sandbox</span></span>  |`https://<company name>.sandbox.youearnedit.com` |

    > [!NOTE] 
    > <span data-ttu-id="437a7-167">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="437a7-167">These values are not real.</span></span> <span data-ttu-id="437a7-168">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="437a7-168">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="437a7-169">Kontakta [YouEarnedIt klienten supportteamet](https://youearnedit.freshdesk.com/support/tickets/new) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="437a7-169">Contact [YouEarnedIt Client support team](https://youearnedit.freshdesk.com/support/tickets/new) to get these values.</span></span> 
 
4. <span data-ttu-id="437a7-170">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="437a7-170">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_certificate.png) 

5. <span data-ttu-id="437a7-172">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="437a7-172">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-youearnedit-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="437a7-174">På den **YouEarnedIt Configuration** klickar du på **konfigurera YouEarnedIt** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="437a7-174">On the **YouEarnedIt Configuration** section, click **Configure YouEarnedIt** to open **Configure sign-on** window.</span></span> <span data-ttu-id="437a7-175">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="437a7-175">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![YouEarnedIt konfiguration](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_configure.png) 

7. <span data-ttu-id="437a7-177">Konfigurera enkel inloggning på **YouEarnedIt** sida, måste du skicka den hämtade **Certificate(Base64)** och **SAML enkel inloggning Tjänstwebbadress** till [YouEarnedIt supportteamet](https://youearnedit.freshdesk.com/support/tickets/new).</span><span class="sxs-lookup"><span data-stu-id="437a7-177">To configure single sign-on on **YouEarnedIt** side, you need to send the downloaded **Certificate(Base64)** and **SAML Single Sign-On Service URL** to [YouEarnedIt support team](https://youearnedit.freshdesk.com/support/tickets/new).</span></span> <span data-ttu-id="437a7-178">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="437a7-178">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="437a7-179">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="437a7-179">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="437a7-180">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="437a7-180">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="437a7-181">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="437a7-181">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="437a7-182">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="437a7-182">Create an Azure AD test user</span></span>

<span data-ttu-id="437a7-183">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="437a7-183">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="437a7-185">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="437a7-185">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="437a7-186">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="437a7-186">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="437a7-188">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="437a7-188">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="437a7-190">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="437a7-190">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="437a7-192">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="437a7-192">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_04.png)

    <span data-ttu-id="437a7-194">a.</span><span class="sxs-lookup"><span data-stu-id="437a7-194">a.</span></span> <span data-ttu-id="437a7-195">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="437a7-195">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="437a7-196">b.</span><span class="sxs-lookup"><span data-stu-id="437a7-196">b.</span></span> <span data-ttu-id="437a7-197">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="437a7-197">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="437a7-198">c.</span><span class="sxs-lookup"><span data-stu-id="437a7-198">c.</span></span> <span data-ttu-id="437a7-199">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="437a7-199">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="437a7-200">d.</span><span class="sxs-lookup"><span data-stu-id="437a7-200">d.</span></span> <span data-ttu-id="437a7-201">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="437a7-201">Click **Create**.</span></span>
 
### <a name="create-a-youearnedit-test-user"></a><span data-ttu-id="437a7-202">Skapa en testanvändare YouEarnedIt</span><span class="sxs-lookup"><span data-stu-id="437a7-202">Create a YouEarnedIt test user</span></span>

<span data-ttu-id="437a7-203">I det här avsnittet skapar du en användare som kallas Britta Simon i YouEarnedIt.</span><span class="sxs-lookup"><span data-stu-id="437a7-203">In this section, you create a user called Britta Simon in YouEarnedIt.</span></span> <span data-ttu-id="437a7-204">Se tillsammans med YouEarnedIt supportteamet att lägga till användare i YouEarnedIt-plattformen.</span><span class="sxs-lookup"><span data-stu-id="437a7-204">Please work with YouEarnedIt support team to add the users in the YouEarnedIt platform.</span></span>

>[!NOTE]
><span data-ttu-id="437a7-205">YouEarnedIt räknar identitetsleverantören ange en e-postadress eller ett användarnamn i attributet NameID.</span><span class="sxs-lookup"><span data-stu-id="437a7-205">YouEarnedIt expect the Identity Provider to supply an EmailAddress  or UserName in the NameID attribute.</span></span> <span data-ttu-id="437a7-206">Autentiseringen misslyckas om en motsvarande användarnamn eller e-postadress finns inte i databasen eller inte matchar varandra exakt.</span><span class="sxs-lookup"><span data-stu-id="437a7-206">Authentication will fail if a corresponding UserName or EmailAddress is not found within the database or does not match exactly.</span></span> <span data-ttu-id="437a7-207">Detta kräver att konton importeras till YouEarnedIt-systemet innan SSO-integration (vanligtvis antingen via API: et eller CSV-import).</span><span class="sxs-lookup"><span data-stu-id="437a7-207">This will require that accounts be imported into the YouEarnedIt system before the SSO integration (Typically either via API or CSV import).</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="437a7-208">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="437a7-208">Assign the Azure AD test user</span></span>

<span data-ttu-id="437a7-209">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till YouEarnedIt.</span><span class="sxs-lookup"><span data-stu-id="437a7-209">In this section, you enable Britta Simon to use Azure single sign-on by granting access to YouEarnedIt.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="437a7-211">**Om du vill tilldela YouEarnedIt Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="437a7-211">**To assign Britta Simon to YouEarnedIt, perform the following steps:**</span></span>

1. <span data-ttu-id="437a7-212">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="437a7-212">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="437a7-214">Välj i listan med program **YouEarnedIt**.</span><span class="sxs-lookup"><span data-stu-id="437a7-214">In the applications list, select **YouEarnedIt**.</span></span>

    ![Länken YouEarnedIt i listan med program](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_app.png)  

3. <span data-ttu-id="437a7-216">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="437a7-216">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="437a7-218">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="437a7-218">Click **Add** button.</span></span> <span data-ttu-id="437a7-219">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="437a7-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="437a7-221">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="437a7-221">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="437a7-222">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="437a7-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="437a7-223">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="437a7-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="437a7-224">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="437a7-224">Test single sign-on</span></span>

<span data-ttu-id="437a7-225">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="437a7-225">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="437a7-226">När du klickar på panelen YouEarnedIt på åtkomstpanelen du bör få automatiskt loggat in på ditt YouEarnedIt program.</span><span class="sxs-lookup"><span data-stu-id="437a7-226">When you click the YouEarnedIt tile in the Access Panel, you should get automatically signed-on to your YouEarnedIt application.</span></span>
<span data-ttu-id="437a7-227">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="437a7-227">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="437a7-228">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="437a7-228">Additional resources</span></span>

* [<span data-ttu-id="437a7-229">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="437a7-229">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="437a7-230">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="437a7-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_203.png

