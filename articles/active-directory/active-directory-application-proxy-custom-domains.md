---
title: "aaaCustom domäner i Azure AD Application Proxy | Microsoft Docs"
description: "Hantera anpassade domäner i Azure AD Application Proxy hello URL: en för hello app är hello samma oavsett där användarna komma åt den."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 2fe9f895-f641-4362-8b27-7a5d08f8600f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 7a433c411976077210a2435c3c087991c7430755
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a><span data-ttu-id="f291f-103">Arbeta med anpassade domäner i Azure AD Application Proxy</span><span class="sxs-lookup"><span data-stu-id="f291f-103">Working with custom domains in Azure AD Application Proxy</span></span>

<span data-ttu-id="f291f-104">När du publicerar ett program via Azure Active Directory Application Proxy kan skapa du en extern URL för din användare toogo toowhen som de arbetar via fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="f291f-104">When you publish an application through Azure Active Directory Application Proxy, you create an external URL for your users toogo toowhen they're working remotely.</span></span> <span data-ttu-id="f291f-105">Denna URL hämtar hello standarddomän *yourtenant.msappproxy.net*.</span><span class="sxs-lookup"><span data-stu-id="f291f-105">This URL gets hello default domain *yourtenant.msappproxy.net*.</span></span> <span data-ttu-id="f291f-106">Till exempel om du har publicerat en app med namnet kostnader och din klient namnet Contoso och hello externa URL: en skulle vara https://expenses-contoso.msappproxy.net.</span><span class="sxs-lookup"><span data-stu-id="f291f-106">For example, if you published an app named Expenses and your tenant is named Contoso, then hello external URL would be https://expenses-contoso.msappproxy.net.</span></span> <span data-ttu-id="f291f-107">Om du vill toouse ditt eget domännamn kan du konfigurera en anpassad domän för ditt program.</span><span class="sxs-lookup"><span data-stu-id="f291f-107">If you want toouse your own domain name, configure a custom domain for your application.</span></span> 

<span data-ttu-id="f291f-108">Vi rekommenderar att du konfigurerar anpassade domäner för dina program när det är möjligt.</span><span class="sxs-lookup"><span data-stu-id="f291f-108">We recommend that you set up custom domains for your applications whenever possible.</span></span> <span data-ttu-id="f291f-109">Några av hello fördelarna med anpassade domäner är:</span><span class="sxs-lookup"><span data-stu-id="f291f-109">Some of hello benefits of custom domains include:</span></span>

- <span data-ttu-id="f291f-110">Användarna kan få toohello program med hello samma URL, oavsett om de arbetar i eller utanför nätverket.</span><span class="sxs-lookup"><span data-stu-id="f291f-110">Your users can get toohello application with hello same URL, whether they are working inside or outside of your network.</span></span>
- <span data-ttu-id="f291f-111">Om alla program har hello samma interna och externa URL: er, fortsätter länkar i ett program som pekar tooanother toowork även utanför hello företagsnätverket.</span><span class="sxs-lookup"><span data-stu-id="f291f-111">If all of your applications have hello same internal and external URLs, then links in one application that point tooanother continue toowork even outside hello corporate network.</span></span> 
- <span data-ttu-id="f291f-112">Du styr din företagsanpassning och skapa hello webbadresser som du vill.</span><span class="sxs-lookup"><span data-stu-id="f291f-112">You control your branding, and create hello URLs you want.</span></span> 


## <a name="configure-a-custom-domain"></a><span data-ttu-id="f291f-113">Konfigurera en anpassad domän</span><span class="sxs-lookup"><span data-stu-id="f291f-113">Configure a custom domain</span></span>

### <a name="prerequisites"></a><span data-ttu-id="f291f-114">Krav</span><span class="sxs-lookup"><span data-stu-id="f291f-114">Prerequisites</span></span>

<span data-ttu-id="f291f-115">Innan du konfigurerar en anpassad domän måste du kontrollera att du har hello följande krav förberedd:</span><span class="sxs-lookup"><span data-stu-id="f291f-115">Before you configure a custom domain, make sure that you have hello following requirements prepared:</span></span> 
- <span data-ttu-id="f291f-116">En [verifierad domän läggs tooAzure Active Directory](active-directory-domains-add-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f291f-116">A [verified domain added tooAzure Active Directory](active-directory-domains-add-azure-portal.md).</span></span>
- <span data-ttu-id="f291f-117">Ett anpassat certifikat hello domän i hello form av en PFX-fil.</span><span class="sxs-lookup"><span data-stu-id="f291f-117">A custom certificate for hello domain, in hello form of a PFX file.</span></span> 
- <span data-ttu-id="f291f-118">En lokal app [som publicerats via Application Proxy](application-proxy-publish-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f291f-118">An on-premises app [published through Application Proxy](application-proxy-publish-azure-portal.md).</span></span>

### <a name="configure-your-custom-domain"></a><span data-ttu-id="f291f-119">Konfigurera den anpassade domänen</span><span class="sxs-lookup"><span data-stu-id="f291f-119">Configure your custom domain</span></span>

<span data-ttu-id="f291f-120">Följ dessa steg tooset in den anpassade domänen när du har dessa tre krav som är redo:</span><span class="sxs-lookup"><span data-stu-id="f291f-120">When you have those three requirements ready, follow these steps tooset up your custom domain:</span></span>

1. <span data-ttu-id="f291f-121">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f291f-121">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f291f-122">Navigera för**Azure Active Directory** > **företagsprogram** > **alla program** och välj hello app du vill toomanage.</span><span class="sxs-lookup"><span data-stu-id="f291f-122">Navigate too**Azure Active Directory** > **Enterprise applications** > **All applications** and choose hello app you want toomanage.</span></span>
3. <span data-ttu-id="f291f-123">Välj **programproxy**.</span><span class="sxs-lookup"><span data-stu-id="f291f-123">Select **Application Proxy**.</span></span> 
4. <span data-ttu-id="f291f-124">Använd hello nedrullningsbara listan tooselect domänen i hello extern URL-fältet.</span><span class="sxs-lookup"><span data-stu-id="f291f-124">In hello External URL field, use hello dropdown list tooselect your custom domain.</span></span> <span data-ttu-id="f291f-125">Om du inte ser din domän i listan hello har inte sedan den verifierats ännu.</span><span class="sxs-lookup"><span data-stu-id="f291f-125">If you don't see your domain in hello list, then it hasn't been verified yet.</span></span> 
5. <span data-ttu-id="f291f-126">Välj **spara**</span><span class="sxs-lookup"><span data-stu-id="f291f-126">Select **Save**</span></span>
5. <span data-ttu-id="f291f-127">Hej **certifikat** fält som har inaktiverats aktiveras.</span><span class="sxs-lookup"><span data-stu-id="f291f-127">hello **Certificate** field that was disabled becomes enabled.</span></span> <span data-ttu-id="f291f-128">Välj det här fältet.</span><span class="sxs-lookup"><span data-stu-id="f291f-128">Select this field.</span></span> 

   ![Klicka på tooupload ett certifikat](./media/active-directory-application-proxy-custom-domains/certificate.png)

   <span data-ttu-id="f291f-130">Om du redan har överfört ett certifikat för den här domänen visar hello certifikatfältet hello certifikatinformationen.</span><span class="sxs-lookup"><span data-stu-id="f291f-130">If you already uploaded a certificate for this domain, hello Certificate field displays hello certificate information.</span></span> 

6. <span data-ttu-id="f291f-131">Överför hello PFX-certifikat och ange hello lösenord för hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="f291f-131">Upload hello PFX certificate and enter hello password for hello certificate.</span></span> 
7. <span data-ttu-id="f291f-132">Välj **spara** toosave ändringarna.</span><span class="sxs-lookup"><span data-stu-id="f291f-132">Select **Save** toosave your changes.</span></span> 
8. <span data-ttu-id="f291f-133">Lägg till en [DNS-post](../dns/dns-operations-recordsets-portal.md) att omdirigeringar hello ny externa URL: en toohello msappproxy.net domän.</span><span class="sxs-lookup"><span data-stu-id="f291f-133">Add a [DNS record](../dns/dns-operations-recordsets-portal.md) that redirects hello new external URL toohello msappproxy.net domain.</span></span> 

>[!TIP] 
><span data-ttu-id="f291f-134">Du behöver bara ett certifikat för tooupload per domän.</span><span class="sxs-lookup"><span data-stu-id="f291f-134">You only need tooupload one certificate per custom domain.</span></span> <span data-ttu-id="f291f-135">När du överföra ett certifikat kan du välja hello domänen när du publicerar en ny app och inte har toodo ytterligare konfiguration förutom hello DNS-post.</span><span class="sxs-lookup"><span data-stu-id="f291f-135">Once you upload a certificate, you can choose hello custom domain when you publish a new app and not have toodo additional configuration except for hello DNS record.</span></span> 

## <a name="manage-certificates"></a><span data-ttu-id="f291f-136">Hantera certifikat</span><span class="sxs-lookup"><span data-stu-id="f291f-136">Manage certificates</span></span>

### <a name="certificate-format"></a><span data-ttu-id="f291f-137">Certifikatets format</span><span class="sxs-lookup"><span data-stu-id="f291f-137">Certificate format</span></span>
<span data-ttu-id="f291f-138">Det finns ingen begränsning för hello certifikat signatur metoder.</span><span class="sxs-lookup"><span data-stu-id="f291f-138">There is no restriction on hello certificate signature methods.</span></span> <span data-ttu-id="f291f-139">Elliptic Curve Cryptography (ECC), alternativt namn på CERTIFIKATMOTTAGARE och andra vanliga typer av certifikat kan användas.</span><span class="sxs-lookup"><span data-stu-id="f291f-139">Elliptic Curve Cryptography (ECC), Subject Alternative Name (SAN), and other common certificate types are all supported.</span></span> 

<span data-ttu-id="f291f-140">Du kan använda ett jokerteckencertifikat så länge som hello jokertecken matchar hello önskad externa URL: en.</span><span class="sxs-lookup"><span data-stu-id="f291f-140">You can use a wildcard certificate as long as hello wildcard matches hello desired external URL.</span></span> 

<span data-ttu-id="f291f-141">Du kan använda självsignerade certifikat, samt.</span><span class="sxs-lookup"><span data-stu-id="f291f-141">You can use self-signed certificates, as well.</span></span> <span data-ttu-id="f291f-142">Om du använder en privat certifikatsutfärdare ska hello CDP (certifikat punkt distributionsplats återkallade) för hello certifikatet vara offentlig.</span><span class="sxs-lookup"><span data-stu-id="f291f-142">If you’re using a private certificate authority, hello CDP (certificate revocation point distribution point) for hello certificate should be public.</span></span>

### <a name="changing-hello-domain"></a><span data-ttu-id="f291f-143">Ändra hello domän</span><span class="sxs-lookup"><span data-stu-id="f291f-143">Changing hello domain</span></span>
<span data-ttu-id="f291f-144">Alla verifierade domäner som visas i listrutan för hello externa URL: en för ditt program.</span><span class="sxs-lookup"><span data-stu-id="f291f-144">All verified domains appear in hello External URL dropdown list for your application.</span></span> <span data-ttu-id="f291f-145">toochange hello domänen bara uppdatering som fältet för hello program.</span><span class="sxs-lookup"><span data-stu-id="f291f-145">toochange hello domain, just update that field for hello application.</span></span> <span data-ttu-id="f291f-146">Om det inte finns i listan hello hello-domän som du vill [Lägg till den som en verifierad domän](active-directory-domains-add-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f291f-146">If hello domain you want isn't in hello list, [add it as a verified domain](active-directory-domains-add-azure-portal.md).</span></span> <span data-ttu-id="f291f-147">Följ steg 5 – 7 tooadd hello certifikatet om du väljer en domän som inte redan har ett associerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="f291f-147">If you select a domain that doesn't have an associated certificate yet, follow steps 5-7 tooadd hello certificate.</span></span> <span data-ttu-id="f291f-148">Kontrollera sedan att du uppdaterar hello DNS-poster tooredirect från hello nya externa URL: en.</span><span class="sxs-lookup"><span data-stu-id="f291f-148">Then, make sure you update hello DNS record tooredirect from hello new external URL.</span></span> 

### <a name="certificate-management"></a><span data-ttu-id="f291f-149">Certifikathantering</span><span class="sxs-lookup"><span data-stu-id="f291f-149">Certificate management</span></span>
<span data-ttu-id="f291f-150">Du kan använda samma certifikat för flera program, om inte hello program dela en extern värd hello.</span><span class="sxs-lookup"><span data-stu-id="f291f-150">You can use hello same certificate for multiple applications unless hello applications share an external host.</span></span> 

<span data-ttu-id="f291f-151">Du får en varning när certifikatet upphör att gälla, uppmanar tooupload ett annat certifikat hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="f291f-151">You get a warning when a certificate expires, telling you tooupload another certificate through hello portal.</span></span> <span data-ttu-id="f291f-152">Hello-certifikatet har återkallats användarna ser en säkerhetsvarning vid åtkomst till programmet hello.</span><span class="sxs-lookup"><span data-stu-id="f291f-152">If hello certificate is revoked, your users may see a security warning when accessing hello application.</span></span> <span data-ttu-id="f291f-153">Vi utföra inte återkallelsekontroller för certifikat.</span><span class="sxs-lookup"><span data-stu-id="f291f-153">We don’t perform revocation checks for certificates.</span></span>  <span data-ttu-id="f291f-154">tooupdate hello certifikat för ett visst program, navigera toohello program och följ steg 5 – 7 för att konfigurera anpassade domäner på publicerade program tooupload ett nytt certifikat.</span><span class="sxs-lookup"><span data-stu-id="f291f-154">tooupdate hello certificate for a given application, navigate toohello application and follow steps 5-7 for configuring custom domains on published applications tooupload a new certificate.</span></span> <span data-ttu-id="f291f-155">Om hello gammalt certifikat inte används av andra program, raderas det automatiskt.</span><span class="sxs-lookup"><span data-stu-id="f291f-155">If hello old certificate is not being used by other applications, it is deleted automatically.</span></span> 

<span data-ttu-id="f291f-156">Alla certifikathantering är för närvarande via enskilda programsidor så du måste toomanage certifikat i hello ramen för hello relevant program.</span><span class="sxs-lookup"><span data-stu-id="f291f-156">Currently all certificate management is through individual application pages so you need toomanage certificates in hello context of hello relevant applications.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="f291f-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f291f-157">Next steps</span></span>
* <span data-ttu-id="f291f-158">[Aktivera enkel inloggning](active-directory-application-proxy-sso-using-kcd.md) tooyour publicerade appar med Azure AD-autentisering.</span><span class="sxs-lookup"><span data-stu-id="f291f-158">[Enable single sign-on](active-directory-application-proxy-sso-using-kcd.md) tooyour published apps with Azure AD authentication.</span></span>
* <span data-ttu-id="f291f-159">[Aktivera villkorlig åtkomst](active-directory-application-proxy-conditional-access.md) tooyour publicerade appar.</span><span class="sxs-lookup"><span data-stu-id="f291f-159">[Enable conditional access](active-directory-application-proxy-conditional-access.md) tooyour published apps.</span></span>
* [<span data-ttu-id="f291f-160">Lägg till din anpassade domän namn tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="f291f-160">Add your custom domain name tooAzure AD</span></span>](active-directory-domains-add-azure-portal.md)


