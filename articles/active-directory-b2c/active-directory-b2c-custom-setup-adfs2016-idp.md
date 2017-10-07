---
title: "Azure Active Directory B2C: Lägga till AD FS som en SAML-identitetsprovider anpassade principer"
description: Hur-tooarticle om hur du konfigurerar AD FS 2016 med SAML-protokoll och anpassade principer
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: joroja
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: yoelh
ms.openlocfilehash: 30fb7700e7834e3d91fab1fc1b169b761584b204
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-add-adfs-as-a-saml-identity-provider-using-custom-policies"></a><span data-ttu-id="27939-103">Azure Active Directory B2C: Lägga till AD FS som en SAML-identitetsprovider anpassade principer</span><span class="sxs-lookup"><span data-stu-id="27939-103">Azure Active Directory B2C: Add ADFS as a SAML identity provider using custom policies</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="27939-104">Den här artikeln lär du dig hur tooenable inloggning för användare från AD FS-konto via hello [anpassade principer](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="27939-104">This article shows you how tooenable sign-in for users from ADFS account through hello use of [custom policies](active-directory-b2c-overview-custom.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="27939-105">Krav</span><span class="sxs-lookup"><span data-stu-id="27939-105">Prerequisites</span></span>

<span data-ttu-id="27939-106">Fullständig hello stegen i hello [komma igång med anpassade principer](active-directory-b2c-get-started-custom.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="27939-106">Complete hello steps in hello [Getting started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

<span data-ttu-id="27939-107">De här stegen innefattar:</span><span class="sxs-lookup"><span data-stu-id="27939-107">These steps include:</span></span>

1.  <span data-ttu-id="27939-108">Skapa en AD FS förlitande part.</span><span class="sxs-lookup"><span data-stu-id="27939-108">Creating an ADFS Relying Party Trust.</span></span>
2.  <span data-ttu-id="27939-109">Lägger till hello ADFS förlitande part certifikat tooAzure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="27939-109">Adding hello ADFS Relying Party Trust certificate tooAzure AD B2C.</span></span>
3.  <span data-ttu-id="27939-110">Lägga till anspråk tooa principen.</span><span class="sxs-lookup"><span data-stu-id="27939-110">Adding claims provider tooa policy.</span></span>
4.  <span data-ttu-id="27939-111">Registrera hello ADFS konto anspråk providern tooa användaren resa.</span><span class="sxs-lookup"><span data-stu-id="27939-111">Registering hello ADFS account claims provider tooa user journey.</span></span>
5.  <span data-ttu-id="27939-112">Överför hello princip tooan Azure AD B2C-klient och testa den.</span><span class="sxs-lookup"><span data-stu-id="27939-112">Uploading hello policy tooan Azure AD B2C tenant and test it.</span></span>

## <a name="toocreate-a-claims-aware-relying-party-trust"></a><span data-ttu-id="27939-113">toocreate ett anspråksmedvetet förtroende för förlitande part</span><span class="sxs-lookup"><span data-stu-id="27939-113">toocreate a claims-aware Relying Party Trust</span></span>

<span data-ttu-id="27939-114">toouse ADFS som en identitetsleverantör i Azure Active Directory (AD Azure) B2C du behöver toocreate en AD FS förtroende för förlitande part och lämna hello rätt parametrar.</span><span class="sxs-lookup"><span data-stu-id="27939-114">toouse ADFS as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate an ADFS Relying Party Trust and supply it with hello right parameters.</span></span>

<span data-ttu-id="27939-115">tooadd en ny förlitande part förtroende med hjälp av hello AD FS snapin-modulen Hantering och manuellt konfigurera hello inställningar, utföra hello följa proceduren på en federationsserver.</span><span class="sxs-lookup"><span data-stu-id="27939-115">tooadd a new relying party trust by using hello AD FS Management snap-in and manually configure hello settings, perform hello following procedure on a federation server.</span></span>

<span data-ttu-id="27939-116">Medlemskap i **administratörer**, eller motsvarande, på dator hello hello minsta nödvändiga toocomplete den här proceduren.</span><span class="sxs-lookup"><span data-stu-id="27939-116">Membership in **Administrators**, or equivalent, on hello local computer is hello minimum required toocomplete this procedure.</span></span> <span data-ttu-id="27939-117">Läs mer om hur du använder hello lämpliga konton och gruppmedlemskap i [Local and Domain Default Groups](http://go.microsoft.com/fwlink/?LinkId=83477)</span><span class="sxs-lookup"><span data-stu-id="27939-117">Review details about using hello appropriate accounts and group memberships at [Local and Domain Default Groups](http://go.microsoft.com/fwlink/?LinkId=83477)</span></span>

1.  <span data-ttu-id="27939-118">I Serverhanteraren klickar du på **verktyg**, och välj sedan **AD FS Management**.</span><span class="sxs-lookup"><span data-stu-id="27939-118">In Server Manager, click **Tools**, and then select **ADFS Management**.</span></span>

2.  <span data-ttu-id="27939-119">Klicka på **Lägg till förlitande part**.</span><span class="sxs-lookup"><span data-stu-id="27939-119">Click on **Add Relying Party Trust**.</span></span>
    <span data-ttu-id="27939-120">![Lägg till förlitande part](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-1.png)</span><span class="sxs-lookup"><span data-stu-id="27939-120">![Add Relying Party Trust](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-1.png)</span></span>

3.  <span data-ttu-id="27939-121">På hello **Välkommen** väljer **anspråk medveten** och på **starta**.</span><span class="sxs-lookup"><span data-stu-id="27939-121">On hello **Welcome** page, choose **Claims aware** and click **Start**.</span></span>
    <span data-ttu-id="27939-122">![Välj anspråk medveten på hello välkomstsidan](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-2.png)</span><span class="sxs-lookup"><span data-stu-id="27939-122">![On hello Welcome page, choose Claims aware](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-2.png)</span></span>
4.  <span data-ttu-id="27939-123">På hello **Välj datakälla** klickar du på **ange information om hello förlitande part manuellt**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="27939-123">On hello **Select Data Source** page, click **Enter data about hello relying party manually**, and then click **Next**.</span></span>
    <span data-ttu-id="27939-124">![Ange information om hello förlitande part](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-3.png)</span><span class="sxs-lookup"><span data-stu-id="27939-124">![Enter data about hello relying party](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-3.png)</span></span>

5.  <span data-ttu-id="27939-125">På hello **ange visningsnamn** anger du ett namn i **visningsnamn**under **anteckningar** Skriv en beskrivning för den här förlitande parten och klickar sedan på **nästa** .</span><span class="sxs-lookup"><span data-stu-id="27939-125">On hello **Specify Display Name** page, type a name in **Display name**, under **Notes** type a description for this relying party trust, and then click **Next**.</span></span>
    <span data-ttu-id="27939-126">![Ange namn och anteckningar](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-4.png)</span><span class="sxs-lookup"><span data-stu-id="27939-126">![Specify Display Name and notes](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-4.png)</span></span>
6.  <span data-ttu-id="27939-127">Valfri.</span><span class="sxs-lookup"><span data-stu-id="27939-127">Optional.</span></span> <span data-ttu-id="27939-128">Om du har ett certifikat med valfritt tokenkryptering, sedan på hello **konfigurera certifikat** klickar du på **Bläddra** toolocate din certifikatsfil och klicka sedan på **nästa** .</span><span class="sxs-lookup"><span data-stu-id="27939-128">If you have an optional token encryption certificate, then on hello **Configure Certificate** page, click **Browse** toolocate your certificate file, and then click **Next**.</span></span>
    <span data-ttu-id="27939-129">![Konfigurera certifikat](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-5.png)</span><span class="sxs-lookup"><span data-stu-id="27939-129">![Configure Certificate](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-5.png)</span></span>
7.  <span data-ttu-id="27939-130">På hello **konfigurera URL** sidan, Välj hello **aktivera stöd för hello protokollet SAML 2.0 WebSSO** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="27939-130">On hello **Configure URL** page, select hello **Enable support for hello SAML 2.0 WebSSO protocol** check box.</span></span> <span data-ttu-id="27939-131">Under **URL för förlitande part SAML 2.0 SSO**skriver hello Security Assertion Markup Language (SAML) slutpunkt-URL för den här förlitande parten och klickar sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="27939-131">Under **Relying party SAML 2.0 SSO service URL**, type hello Security Assertion Markup Language (SAML) service endpoint URL for this relying party trust, and then click **Next**.</span></span>  <span data-ttu-id="27939-132">För hello **URL för förlitande part SAML 2.0 SSO**, klistra in hello `https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/{policy}`.</span><span class="sxs-lookup"><span data-stu-id="27939-132">For hello **Relying party SAML 2.0 SSO service URL**, paste hello `https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/{policy}`.</span></span> <span data-ttu-id="27939-133">Ersätt {klient} med namnet för din klient (till exempel contosob2c.onmicrosoft.com) och Ersätt hello {princip} med tillägg-principnamn (till exempel B2C_1A_TrustFrameworkExtensions).</span><span class="sxs-lookup"><span data-stu-id="27939-133">Replace {tenant} with your tenant's name (for example, contosob2c.onmicrosoft.com), and replace hello {policy} with your extensions policy name (for example, B2C_1A_TrustFrameworkExtensions).</span></span>
    > [!IMPORTANT]
    ><span data-ttu-id="27939-134">hello principnamn är hello en signup_or_signin princip ärver från, i det här fallet är det: `B2C_1A_TrustFrameworkExtensions`.</span><span class="sxs-lookup"><span data-stu-id="27939-134">hello policy name  is hello one that signup_or_signin policy inherits from, in this case it is: `B2C_1A_TrustFrameworkExtensions`.</span></span>
    ><span data-ttu-id="27939-135">Hello-URL kan till exempel vara: https://login.microsoftonline.com/te/**contosob2c**.onmicrosoft.com/**B2C_1A_TrustFrameworkBase**.</span><span class="sxs-lookup"><span data-stu-id="27939-135">For example hello URL could be:   https://login.microsoftonline.com/te/**contosob2c**.onmicrosoft.com/**B2C_1A_TrustFrameworkBase**.</span></span>

    ![Förlitande part SAML 2.0 SSO-tjänstens URL](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-6.png)
8. <span data-ttu-id="27939-137">På hello **konfigurera identifierare** anger hello samma Webbadress som hello föregående steg, klicka på **Lägg till** tooadd dem toohello listan och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="27939-137">On hello **Configure Identifiers** page, specify hello same URL as hello previous step, click **Add** tooadd them toohello list, and then click **Next**.</span></span>
    <span data-ttu-id="27939-138">![Förlitande parts förtroende identifierare](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-7.png)</span><span class="sxs-lookup"><span data-stu-id="27939-138">![Relying party trust identifiers](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-7.png)</span></span>
9.  <span data-ttu-id="27939-139">På hello **Välj princip för åtkomstkontroll** Välj en princip och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="27939-139">On hello **Choose Access Control Policy** select a policy and click **Next**.</span></span>
    <span data-ttu-id="27939-140">![Välj princip för åtkomstkontroll](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-8.png)</span><span class="sxs-lookup"><span data-stu-id="27939-140">![Choose Access Control Policy](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-8.png)</span></span>
10.  <span data-ttu-id="27939-141">På hello **klar tooAdd förtroende** sidan Granska hello inställningarna och klicka sedan på **nästa** toosave din förlitande part information.</span><span class="sxs-lookup"><span data-stu-id="27939-141">On hello **Ready tooAdd Trust** page, review hello settings, and then click **Next** toosave your relying party trust information.</span></span>
    <span data-ttu-id="27939-142">![Spara informationen om förlitande part förtroende](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-9.png)</span><span class="sxs-lookup"><span data-stu-id="27939-142">![Save your relying party trust information](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-9.png)</span></span>
11.  <span data-ttu-id="27939-143">På hello **Slutför** klickar du på **Stäng**, den här åtgärden visas automatiskt hello **redigera Anspråksregler** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="27939-143">On hello **Finish** page, click **Close**, this action automatically displays hello **Edit Claim Rules** dialog box.</span></span>
    <span data-ttu-id="27939-144">![Redigera Anspråksregler](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-10.png)</span><span class="sxs-lookup"><span data-stu-id="27939-144">![Edit Claim Rules](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-10.png)</span></span>
12. <span data-ttu-id="27939-145">Klicka på **Lägg till regel**.</span><span class="sxs-lookup"><span data-stu-id="27939-145">Click **Add Rule**.</span></span>  
      <span data-ttu-id="27939-146">![Lägg till ny regel](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-1.png)</span><span class="sxs-lookup"><span data-stu-id="27939-146">![Add new rule](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-1.png)</span></span>
13.  <span data-ttu-id="27939-147">I **anspråksregelmall**väljer **skicka LDAP-attribut som anspråk**.</span><span class="sxs-lookup"><span data-stu-id="27939-147">In **Claim rule template**, select **Send LDAP attributes as claims**.</span></span>
    <span data-ttu-id="27939-148">![Välj Skicka LDAP-attribut som anspråk mall-regel](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-2.png)</span><span class="sxs-lookup"><span data-stu-id="27939-148">![Select Send LDAP attributes as claims template rule](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-2.png)</span></span>
14.  <span data-ttu-id="27939-149">Ange **Regelnamn för anspråk**.</span><span class="sxs-lookup"><span data-stu-id="27939-149">Provide **Claim rule name**.</span></span> <span data-ttu-id="27939-150">För hello **attributarkiv** Välj **Välj Active Directory** Lägg till följande anspråk hello och klicka sedan på **Slutför** och **OK**.</span><span class="sxs-lookup"><span data-stu-id="27939-150">For hello **Attribute store** select **Select Active Directory** Add hello following claims, then click **Finish** and **OK**.</span></span>
    <span data-ttu-id="27939-151">![Ange egenskaper för regeln](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-3.png)</span><span class="sxs-lookup"><span data-stu-id="27939-151">![Set rule properties](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-3.png)</span></span>
15.  <span data-ttu-id="27939-152">I Serverhanteraren väljer **förtroende för förlitande part** Välj hello förlitande partsförtroende som du skapade och klicka sedan **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="27939-152">In Server Manager, select **Relying Party Trusts** then select hello relying party trust you created and click **Properties**.</span></span>
    <span data-ttu-id="27939-153">![Redigera egenskaper för förlitande part](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-1.png)</span><span class="sxs-lookup"><span data-stu-id="27939-153">![Relying party edit properties](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-1.png)</span></span>
16.  <span data-ttu-id="27939-154">En hello förlitande part förtroende (B2C Demo) egenskaper klickar du på **signatur** och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="27939-154">One hello relying party trust (B2C Demo) properties window click **Signature** tab and click **Add**.</span></span>  
    <span data-ttu-id="27939-155">![Ange signatur](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-2.png)</span><span class="sxs-lookup"><span data-stu-id="27939-155">![Set signature](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-2.png)</span></span>
17.  <span data-ttu-id="27939-156">Lägg till din Signaturcertifikat (.cert-fil, utan privat nyckel).</span><span class="sxs-lookup"><span data-stu-id="27939-156">Add your signature certificate (.cert file, without private key).</span></span>  
    ![Lägg till din Signaturcertifikat](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-3.png)
18.  <span data-ttu-id="27939-158">Klicka på hello förlitande part förtroende (B2C Demo) i fönstret **Avancerat** fliken och ändra hello **Secure hash algorithm** för**SHA-1**, klickar du på **Ok**.</span><span class="sxs-lookup"><span data-stu-id="27939-158">On hello relying party trust (B2C Demo) properties window click **Advanced** tab and change hello **Secure hash algorithm** too**SHA-1**, Click **Ok**.</span></span>  
    <span data-ttu-id="27939-159">![Ställ in säkra hash-algoritmen tooSHA-1](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-4.png)</span><span class="sxs-lookup"><span data-stu-id="27939-159">![Set secure hash algorithm tooSHA-1](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-4.png)</span></span>

## <a name="add-hello-adfs-account-application-key-tooazure-ad-b2c"></a><span data-ttu-id="27939-160">Lägg till hello ADFS konto programmet viktiga tooAzure AD B2C</span><span class="sxs-lookup"><span data-stu-id="27939-160">Add hello ADFS account application key tooAzure AD B2C</span></span>
<span data-ttu-id="27939-161">Federation med AD FS-konton kräver en klienthemlighet för ADFS konto tootrust Azure AD B2C uppdrag hello program.</span><span class="sxs-lookup"><span data-stu-id="27939-161">Federation with ADFS accounts requires a client secret for ADFS account tootrust Azure AD B2C on behalf of hello application.</span></span> <span data-ttu-id="27939-162">Du måste toostore AD FS-certifikat i din Azure AD B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="27939-162">You need toostore your ADFS certificate in your Azure AD B2C tenant.</span></span> 

1.  <span data-ttu-id="27939-163">Gå tooyour Azure AD B2C-klient och använda **B2C inställningar** > **identitet upplevelse Framework**</span><span class="sxs-lookup"><span data-stu-id="27939-163">Go tooyour Azure AD B2C tenant, and select **B2C Settings** > **Identity Experience Framework**</span></span>
2.  <span data-ttu-id="27939-164">Välj **princip nycklar** tooview hello-nycklar som är tillgängliga i din klient.</span><span class="sxs-lookup"><span data-stu-id="27939-164">Select **Policy Keys** tooview hello keys available in your tenant.</span></span>
3.  <span data-ttu-id="27939-165">Klicka på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="27939-165">Click **+Add**.</span></span>
4.  <span data-ttu-id="27939-166">För **alternativ**, använda **överför**.</span><span class="sxs-lookup"><span data-stu-id="27939-166">For **Options**, use **Upload**.</span></span>
5.  <span data-ttu-id="27939-167">För **namn**, Använd `ADFSSamlCert`.</span><span class="sxs-lookup"><span data-stu-id="27939-167">For **Name**, use `ADFSSamlCert`.</span></span>  
    <span data-ttu-id="27939-168">hello prefixet `B2C_1A_` kan läggas till automatiskt.</span><span class="sxs-lookup"><span data-stu-id="27939-168">hello prefix `B2C_1A_` might be added automatically.</span></span>
6.  <span data-ttu-id="27939-169">I hello filöverföringen ** väljer din PFX-fil för certifikat med privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="27939-169">In hello File upload,** select your certificate .pfx file with private key.</span></span> <span data-ttu-id="27939-170">Obs: det här certifikatet (med hello privata nyckeln) ska vara hello samma ett som utfärdats och används för hello ADFS förlitande part.</span><span class="sxs-lookup"><span data-stu-id="27939-170">Note: this certificate (with hello private key) should be hello same one that issued and used for hello ADFS relying party.</span></span>
<span data-ttu-id="27939-171">![Ladda upp nyckeln för principen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-cert.png)</span><span class="sxs-lookup"><span data-stu-id="27939-171">![Upload policy key](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-cert.png)</span></span>
7.  <span data-ttu-id="27939-172">Klicka på **Skapa**</span><span class="sxs-lookup"><span data-stu-id="27939-172">Click **Create**</span></span>
8.  <span data-ttu-id="27939-173">Bekräfta att du har skapat hello nyckeln `B2C_1A_ADFSSamlCert`.</span><span class="sxs-lookup"><span data-stu-id="27939-173">Confirm that you've created hello key `B2C_1A_ADFSSamlCert`.</span></span>

## <a name="add-a-claims-provider-in-your-extension-policy"></a><span data-ttu-id="27939-174">Lägg till en anspråksprovider i din princip för tillägg</span><span class="sxs-lookup"><span data-stu-id="27939-174">Add a claims provider in your extension policy</span></span>
<span data-ttu-id="27939-175">Om du vill användare toosign i med hjälp av AD FS-konto behöver du toodefine ADFS-konto som en anspråksprovider.</span><span class="sxs-lookup"><span data-stu-id="27939-175">If you want users toosign in by using ADFS account, you need toodefine ADFS account as a claims provider.</span></span> <span data-ttu-id="27939-176">Du behöver med andra ord toospecify en slutpunkt som Azure AD B2C kommunicerar med.</span><span class="sxs-lookup"><span data-stu-id="27939-176">In other words, you need toospecify an endpoint that Azure AD B2C communicates with.</span></span> <span data-ttu-id="27939-177">hello endpoint innehåller en uppsättning anspråk som används av Azure AD B2C-tooverify som en specifik användare har autentiserats.</span><span class="sxs-lookup"><span data-stu-id="27939-177">hello endpoint provides a set of claims that are used by Azure AD B2C tooverify that a specific user has authenticated.</span></span>

<span data-ttu-id="27939-178">Definiera ADFS som en anspråksprovider genom att lägga till `<ClaimsProvider>` nod i tillägget-principfil:</span><span class="sxs-lookup"><span data-stu-id="27939-178">Define ADFS as a claims provider, by adding `<ClaimsProvider>` node in your extension policy file:</span></span>

1. <span data-ttu-id="27939-179">Öppna hello princip tilläggsfilen (TrustFrameworkExtensions.xml) från arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="27939-179">Open hello extension policy file (TrustFrameworkExtensions.xml) from your working directory.</span></span> <span data-ttu-id="27939-180">Om du behöver en XML-redigerare [försök Visual Studio Code](https://code.visualstudio.com/download), en enkel plattformsoberoende redigerare.</span><span class="sxs-lookup"><span data-stu-id="27939-180">If you need an XML editor, [try Visual Studio Code](https://code.visualstudio.com/download), a lightweight cross-platform editor.</span></span>
2. <span data-ttu-id="27939-181">Hitta hello `<ClaimsProviders>` avsnitt</span><span class="sxs-lookup"><span data-stu-id="27939-181">Find hello `<ClaimsProviders>` section</span></span>
3. <span data-ttu-id="27939-182">Lägg till följande XML-kodstycke under hello hello `ClaimsProviders` element och Ersätt `identityProvider` med din DNS (godtyckligt värde som anger din domän) och sparar hello-filen.</span><span class="sxs-lookup"><span data-stu-id="27939-182">Add hello following XML snippet under hello `ClaimsProviders` element and replace `identityProvider` with your DNS (Arbitrary value that indicates your domain), and save hello file.</span></span> 

```xml
<ClaimsProvider>
    <Domain>contoso.com</Domain>
    <DisplayName>Contoso ADFS</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="Contoso-SAML2">
        <DisplayName>Contoso ADFS</DisplayName>
        <Description>Login with your Contoso account</Description>
        <Protocol Name="SAML2"/>
        <Metadata>
        <Item Key="RequestsSigned">false</Item>
        <Item Key="WantsEncryptedAssertions">false</Item>
        <Item Key="PartnerEntity">https://{your_ADFS_domain}/federationmetadata/2007-06/federationmetadata.xml</Item>
        </Metadata>
        <CryptographicKeys>
        <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
        <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
        </CryptographicKeys>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="userPrincipalName" />
        <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
        <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
        <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name"/>
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="contoso.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication"/>
        </OutputClaims>
        <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
        <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
        <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop"/>
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="register-hello-adfs-account-claims-provider-toosign-up-or-sign-in-user-journey"></a><span data-ttu-id="27939-183">Registrera hello ADFS konto anspråk providern tooSign upp eller logga in användaren resa</span><span class="sxs-lookup"><span data-stu-id="27939-183">Register hello ADFS account claims provider tooSign up or Sign in user journey</span></span>
<span data-ttu-id="27939-184">Nu har hello identitetsleverantören ställts in.</span><span class="sxs-lookup"><span data-stu-id="27939-184">At this point, hello identity provider has been set up.</span></span>  <span data-ttu-id="27939-185">Men finns det inte i någon av hello sign-upp/inloggning skärmar.</span><span class="sxs-lookup"><span data-stu-id="27939-185">However, it is not available in any of hello sign-up/sign-in screens.</span></span> <span data-ttu-id="27939-186">Nu måste tooadd hello ADFS identitet providern tooyour användarkonto `SignUpOrSignIn` användaren resa.</span><span class="sxs-lookup"><span data-stu-id="27939-186">Now you need tooadd hello ADFS account identity provider tooyour user `SignUpOrSignIn` user journey.</span></span> <span data-ttu-id="27939-187">toomake det tillgängliga, skapar vi en dubblett av en befintlig mall användaren resa.</span><span class="sxs-lookup"><span data-stu-id="27939-187">toomake it available, we create a duplicate of an existing template user journey.</span></span>  <span data-ttu-id="27939-188">Sedan ändra vi den så att den omfattar hello ADFS identitetsleverantör:</span><span class="sxs-lookup"><span data-stu-id="27939-188">Then, we modify it so it includes hello ADFS identity provider:</span></span>
    >[!NOTE]
    >If you previously copied hello `<UserJourneys>` element from base file of your policy toohello extension file (TrustFrameworkExtensions.xml) you can skip this section.
1.  <span data-ttu-id="27939-189">Öppna grundläggande hello-filen för principen (till exempel TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="27939-189">Open hello base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
2.  <span data-ttu-id="27939-190">Hitta hello `<UserJourneys>` element och kopiera hela hello-innehållet i `<UserJourneys>` nod.</span><span class="sxs-lookup"><span data-stu-id="27939-190">Find hello `<UserJourneys>` element and copy hello entire content of `<UserJourneys>` node.</span></span>
3.  <span data-ttu-id="27939-191">Öppna filen hello-tillägg (till exempel TrustFrameworkExtensions.xml) och hitta hello `<UserJourneys>` element.</span><span class="sxs-lookup"><span data-stu-id="27939-191">Open hello extension file (for example, TrustFrameworkExtensions.xml) and find hello `<UserJourneys>` element.</span></span> <span data-ttu-id="27939-192">Lägg till ett om hello elementet inte finns.</span><span class="sxs-lookup"><span data-stu-id="27939-192">If hello element doesn't exist, add one.</span></span>
4.  <span data-ttu-id="27939-193">Klistra in hello hela innehållet i `<UserJournesy>` nod som du kopierade som underordnad till hello `<UserJourneys>` element.</span><span class="sxs-lookup"><span data-stu-id="27939-193">Paste hello entire content of `<UserJournesy>` node that you copied as a child of hello `<UserJourneys>` element.</span></span>

### <a name="display-hello-button"></a><span data-ttu-id="27939-194">Visa hello-knappen</span><span class="sxs-lookup"><span data-stu-id="27939-194">Display hello button</span></span>
<span data-ttu-id="27939-195">Hej `<ClaimsProviderSelections>` elementet definierar hello lista med alternativ för val av anspråk providern och deras inbördes ordning.</span><span class="sxs-lookup"><span data-stu-id="27939-195">hello `<ClaimsProviderSelections>` element defines hello list of claims provider selection options and their order.</span></span>  <span data-ttu-id="27939-196">`<ClaimsProviderSelection>`elementet är detsamma tooan identitet provider-knappen en sign-upp/inloggningssidan.</span><span class="sxs-lookup"><span data-stu-id="27939-196">`<ClaimsProviderSelection>` element is analogous tooan identity provider button on a sign-up/sign-in page.</span></span> <span data-ttu-id="27939-197">Om du lägger till en `<ClaimsProviderSelection>` element för AD FS-konto, en ny knapp visas när en användare de hamnar på hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="27939-197">If you add a `<ClaimsProviderSelection>` element for  ADFS account, a new button shows up when a user lands on hello page.</span></span> <span data-ttu-id="27939-198">tooadd det här elementet:</span><span class="sxs-lookup"><span data-stu-id="27939-198">tooadd this element:</span></span>

1.  <span data-ttu-id="27939-199">Hitta hello `<UserJourney>` nod som innehåller `Id="SignUpOrSignIn"` i hello användaren resa som du kopierade.</span><span class="sxs-lookup"><span data-stu-id="27939-199">Find hello `<UserJourney>` node that includes `Id="SignUpOrSignIn"` in hello user journey that you copied.</span></span>
2.  <span data-ttu-id="27939-200">Leta upp hello `<OrchestrationStep>` nod som innehåller`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="27939-200">Locate hello `<OrchestrationStep>` node that includes `Order="1"`</span></span>
3.  <span data-ttu-id="27939-201">Lägg till följande XML-kodstycke under `<ClaimsProviderSelections>` nod:</span><span class="sxs-lookup"><span data-stu-id="27939-201">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```
### <a name="link-hello-button-tooan-action"></a><span data-ttu-id="27939-202">Länken hello tooan åtgärd</span><span class="sxs-lookup"><span data-stu-id="27939-202">Link hello button tooan action</span></span>

<span data-ttu-id="27939-203">Nu när du har en knapp på plats måste toolink den tooan åtgärd.</span><span class="sxs-lookup"><span data-stu-id="27939-203">Now that you have a button in place, you need toolink it tooan action.</span></span> <span data-ttu-id="27939-204">hello åtgärd i det här fallet är för Azure AD B2C toocommunicate med AD FS-konto tooreceive en token.</span><span class="sxs-lookup"><span data-stu-id="27939-204">hello action, in this case, is for Azure AD B2C toocommunicate with ADFS account tooreceive a token.</span></span> <span data-ttu-id="27939-205">Länka hello tooan åtgärd genom att länka tekniska hello-profil för din AD FS-konto anspråksleverantör:</span><span class="sxs-lookup"><span data-stu-id="27939-205">Link hello button tooan action by linking hello technical profile for your ADFS account claims provider:</span></span>

1.  <span data-ttu-id="27939-206">Hitta hello `<OrchestrationStep>` som innehåller `Order="2"` i hello `<UserJourney>` nod.</span><span class="sxs-lookup"><span data-stu-id="27939-206">Find hello `<OrchestrationStep>` that includes `Order="2"` in hello `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="27939-207">Lägg till följande XML-kodstycke under `<ClaimsExchanges>` nod:</span><span class="sxs-lookup"><span data-stu-id="27939-207">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

> [!NOTE]
> * <span data-ttu-id="27939-208">Se till att hello `Id` har samma värde som hello `TargetClaimsExchangeId` i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="27939-208">Ensure hello `Id` has hello same value as that of `TargetClaimsExchangeId` in hello preceding section.</span></span>
> * <span data-ttu-id="27939-209">Se till att `TechnicalProfileReferenceId` anges toohello tekniska profilen du skapade tidigare (Contoso-SAML2).</span><span class="sxs-lookup"><span data-stu-id="27939-209">Ensure `TechnicalProfileReferenceId` is set toohello technical profile you created earlier (Contoso-SAML2).</span></span>

## <a name="upload-hello-policy-tooyour-tenant"></a><span data-ttu-id="27939-210">Överför hello princip tooyour klient</span><span class="sxs-lookup"><span data-stu-id="27939-210">Upload hello policy tooyour tenant</span></span>
1.  <span data-ttu-id="27939-211">I hello [Azure-portalen](https://portal.azure.com), växla till hello [kontext för din Azure AD B2C-klient](active-directory-b2c-navigate-to-b2c-context.md), och öppna hello **Azure AD B2C** bladet.</span><span class="sxs-lookup"><span data-stu-id="27939-211">In hello [Azure portal](https://portal.azure.com), switch into hello [context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and open hello **Azure AD B2C** blade.</span></span>
2.  <span data-ttu-id="27939-212">Välj **identitet upplevelse Framework**.</span><span class="sxs-lookup"><span data-stu-id="27939-212">Select **Identity Experience Framework**.</span></span>
3.  <span data-ttu-id="27939-213">Öppna hello **alla principer** bladet.</span><span class="sxs-lookup"><span data-stu-id="27939-213">Open hello **All Policies** blade.</span></span>
4.  <span data-ttu-id="27939-214">Välj **överföra princip**.</span><span class="sxs-lookup"><span data-stu-id="27939-214">Select **Upload Policy**.</span></span>
5.  <span data-ttu-id="27939-215">Kontrollera **skriva över hello principen om den finns** rutan.</span><span class="sxs-lookup"><span data-stu-id="27939-215">Check **Overwrite hello policy if it exists** box.</span></span>
6.  <span data-ttu-id="27939-216">**Överför** TrustFrameworkExtensions.xml och se till att inte misslyckas verifieringen hello</span><span class="sxs-lookup"><span data-stu-id="27939-216">**Upload** TrustFrameworkExtensions.xml and ensure that it does not fail hello validation</span></span>

## <a name="test-hello-custom-policy-by-using-run-now"></a><span data-ttu-id="27939-217">Testa hello anpassad princip med Kör nu</span><span class="sxs-lookup"><span data-stu-id="27939-217">Test hello custom policy by using Run Now</span></span>
1.  <span data-ttu-id="27939-218">Öppna **Azure AD B2C inställningar** och gå för**identitet upplevelse Framework**.</span><span class="sxs-lookup"><span data-stu-id="27939-218">Open **Azure AD B2C Settings** and go too**Identity Experience Framework**.</span></span>
2.  <span data-ttu-id="27939-219">Öppna **B2C_1A_signup_signin**, hello förlitande part (RP) anpassad princip som du överfört.</span><span class="sxs-lookup"><span data-stu-id="27939-219">Open **B2C_1A_signup_signin**, hello relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="27939-220">Välj **kör nu**.</span><span class="sxs-lookup"><span data-stu-id="27939-220">Select **Run now**.</span></span>
3.  <span data-ttu-id="27939-221">Du ska kunna toosign i med hjälp av AD FS-konto.</span><span class="sxs-lookup"><span data-stu-id="27939-221">You should be able toosign in using ADFS account.</span></span>

## <a name="optional-register-hello-adfs-account-claims-provider-tooprofile-edit-user-journey"></a><span data-ttu-id="27939-222">[Valfritt] Registrera hello ADFS konto anspråk providern tooProfile Redigera användare resa</span><span class="sxs-lookup"><span data-stu-id="27939-222">[Optional] Register hello ADFS account claims provider tooProfile-Edit user journey</span></span>
<span data-ttu-id="27939-223">Vill du kanske tooadd hello ADFS konto identitetsleverantör också tooyour användaren `ProfileEdit` användaren resa.</span><span class="sxs-lookup"><span data-stu-id="27939-223">You may want tooadd hello ADFS account identity provider also tooyour user `ProfileEdit` user journey.</span></span> <span data-ttu-id="27939-224">toomake den är tillgänglig, vi Upprepa hello senaste två steg:</span><span class="sxs-lookup"><span data-stu-id="27939-224">toomake it available, we repeat hello last two steps:</span></span>

### <a name="display-hello-button"></a><span data-ttu-id="27939-225">Visa hello-knappen</span><span class="sxs-lookup"><span data-stu-id="27939-225">Display hello button</span></span>
1.  <span data-ttu-id="27939-226">Öppna filen för hello tillägg av principen (till exempel TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="27939-226">Open hello extension file of your policy (for example, TrustFrameworkExtensions.xml).</span></span>
2.  <span data-ttu-id="27939-227">Hitta hello `<UserJourney>` nod som innehåller `Id="ProfileEdit"` i hello användaren resa som du kopierade.</span><span class="sxs-lookup"><span data-stu-id="27939-227">Find hello `<UserJourney>` node that includes `Id="ProfileEdit"` in hello user journey that you copied.</span></span>
3.  <span data-ttu-id="27939-228">Leta upp hello `<OrchestrationStep>` nod som innehåller`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="27939-228">Locate hello `<OrchestrationStep>` node that includes `Order="1"`</span></span>
4.  <span data-ttu-id="27939-229">Lägg till följande XML-kodstycke under `<ClaimsProviderSelections>` nod:</span><span class="sxs-lookup"><span data-stu-id="27939-229">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```

### <a name="link-hello-button-tooan-action"></a><span data-ttu-id="27939-230">Länken hello tooan åtgärd</span><span class="sxs-lookup"><span data-stu-id="27939-230">Link hello button tooan action</span></span>
1.  <span data-ttu-id="27939-231">Hitta hello `<OrchestrationStep>` som innehåller `Order="2"` i hello `<UserJourney>` nod.</span><span class="sxs-lookup"><span data-stu-id="27939-231">Find hello `<OrchestrationStep>` that includes `Order="2"` in hello `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="27939-232">Lägg till följande XML-kodstycke under `<ClaimsExchanges>` nod:</span><span class="sxs-lookup"><span data-stu-id="27939-232">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

### <a name="test-hello-custom-profile-edit-policy-by-using-run-now"></a><span data-ttu-id="27939-233">Testa hello anpassad profil-Redigera princip genom att använda Kör nu</span><span class="sxs-lookup"><span data-stu-id="27939-233">Test hello custom Profile-Edit policy by using Run Now</span></span>
1.  <span data-ttu-id="27939-234">Öppna **Azure AD B2C inställningar** och gå för**identitet upplevelse Framework**.</span><span class="sxs-lookup"><span data-stu-id="27939-234">Open **Azure AD B2C Settings** and go too**Identity Experience Framework**.</span></span>
2.  <span data-ttu-id="27939-235">Öppna **B2C_1A_ProfileEdit**, hello förlitande part (RP) anpassad princip som du överfört.</span><span class="sxs-lookup"><span data-stu-id="27939-235">Open **B2C_1A_ProfileEdit**, hello relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="27939-236">Välj **kör nu**.</span><span class="sxs-lookup"><span data-stu-id="27939-236">Select **Run now**.</span></span>
3.  <span data-ttu-id="27939-237">Du ska kunna toosign i med hjälp av AD FS-konto.</span><span class="sxs-lookup"><span data-stu-id="27939-237">You should be able toosign in using ADFS account.</span></span>

## <a name="download-hello-complete-policy-files"></a><span data-ttu-id="27939-238">Hämta hello fullständig principfiler</span><span class="sxs-lookup"><span data-stu-id="27939-238">Download hello complete policy files</span></span>
<span data-ttu-id="27939-239">Valfritt: Vi rekommenderar att du skapar ditt scenario med din egen anpassade principfiler när du har slutfört hello komma igång med anpassade principer igenom.</span><span class="sxs-lookup"><span data-stu-id="27939-239">Optional: We recommend you build your scenario using your own Custom policy files after completing hello Getting Started with Custom Policies walk through.</span></span> [<span data-ttu-id="27939-240">Principen exempelfiler endast för referens</span><span class="sxs-lookup"><span data-stu-id="27939-240">Policy sample files for reference only</span></span>](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-adfs2016-app)
