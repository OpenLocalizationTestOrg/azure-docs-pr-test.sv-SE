---
title: "aaaCreate identitet för Azure-app med Azure CLI | Microsoft Docs"
description: "Beskriver hur toouse Azure CLI toocreate ett Azure Active Directory-program och tjänstens huvudnamn och ge det åtkomst till tooresources via rollbaserad åtkomst kontroll. Den visar hur tooauthenticate program med ett lösenord eller ett certifikat."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: c224a189-dd28-4801-b3e3-26991b0eb24d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: 0d693ec801d4f4d6c24ec420580776e73014b325
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-cli-toocreate-a-service-principal-tooaccess-resources"></a><span data-ttu-id="f278f-104">Använda Azure CLI toocreate en service principal tooaccess resurser</span><span class="sxs-lookup"><span data-stu-id="f278f-104">Use Azure CLI toocreate a service principal tooaccess resources</span></span>

<span data-ttu-id="f278f-105">När du har en app eller skript som behöver tooaccess resurser kan du ställa in en identitet för hello app och autentisera hello appen med sina egna autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="f278f-105">When you have an app or script that needs tooaccess resources, you can set up an identity for hello app and authenticate hello app with its own credentials.</span></span> <span data-ttu-id="f278f-106">Den här identiteten kallas ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f278f-106">This identity is known as a service principal.</span></span> <span data-ttu-id="f278f-107">Den här metoden kan du:</span><span class="sxs-lookup"><span data-stu-id="f278f-107">This approach enables you to:</span></span>

* <span data-ttu-id="f278f-108">Tilldela behörigheter toohello app identitet som skiljer sig från din egen behörighet.</span><span class="sxs-lookup"><span data-stu-id="f278f-108">Assign permissions toohello app identity that are different than your own permissions.</span></span> <span data-ttu-id="f278f-109">Dessa behörigheter normalt begränsade tooexactly vilka hello program behöver toodo.</span><span class="sxs-lookup"><span data-stu-id="f278f-109">Typically, these permissions are restricted tooexactly what hello app needs toodo.</span></span>
* <span data-ttu-id="f278f-110">Använda ett certifikat för autentisering när du kör ett oövervakat skript.</span><span class="sxs-lookup"><span data-stu-id="f278f-110">Use a certificate for authentication when executing an unattended script.</span></span>

<span data-ttu-id="f278f-111">Den här artikeln beskrivs hur du toouse [Azure CLI 1.0](../cli-install-nodejs.md) tooset upp ett program toorun under sina egna autentiseringsuppgifter och identitet.</span><span class="sxs-lookup"><span data-stu-id="f278f-111">This article shows you how toouse [Azure CLI 1.0](../cli-install-nodejs.md) tooset up an application toorun under its own credentials and identity.</span></span> <span data-ttu-id="f278f-112">Installera hello senaste versionen av [Azure CLI 1.0](../cli-install-nodejs.md) toomake att din miljö matchar hello exemplen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="f278f-112">Install hello latest version of [Azure CLI 1.0](../cli-install-nodejs.md) toomake sure your environment matches hello examples in this article.</span></span>

## <a name="required-permissions"></a><span data-ttu-id="f278f-113">Behörigheter som krävs</span><span class="sxs-lookup"><span data-stu-id="f278f-113">Required permissions</span></span>
<span data-ttu-id="f278f-114">toocomplete det här avsnittet, du måste ha tillräckliga behörigheter för både i Azure Active Directory och Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="f278f-114">toocomplete this topic, you must have sufficient permissions in both your Azure Active Directory and your Azure subscription.</span></span> <span data-ttu-id="f278f-115">Du måste specifikt vara kan toocreate en app i hello Azure Active Directory och tilldela hello service principal tooa roll.</span><span class="sxs-lookup"><span data-stu-id="f278f-115">Specifically, you must be able toocreate an app in hello Azure Active Directory, and assign hello service principal tooa role.</span></span> 

<span data-ttu-id="f278f-116">hello enklaste sättet toocheck om ditt konto har rätt behörigheter är hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="f278f-116">hello easiest way toocheck whether your account has adequate permissions is through hello portal.</span></span> <span data-ttu-id="f278f-117">Se [Kontrollera behörighet som krävs i portalen](resource-group-create-service-principal-portal.md#required-permissions).</span><span class="sxs-lookup"><span data-stu-id="f278f-117">See [Check required permission in portal](resource-group-create-service-principal-portal.md#required-permissions).</span></span>

<span data-ttu-id="f278f-118">Nu fortsätter du tooa för antingen [lösenord](#create-service-principal-with-password) eller [certifikat](#create-service-principal-with-certificate) autentisering.</span><span class="sxs-lookup"><span data-stu-id="f278f-118">Now, proceed tooa section for either [password](#create-service-principal-with-password) or [certificate](#create-service-principal-with-certificate) authentication.</span></span>

## <a name="create-service-principal-with-password"></a><span data-ttu-id="f278f-119">Skapa tjänstens huvudnamn med lösenord</span><span class="sxs-lookup"><span data-stu-id="f278f-119">Create service principal with password</span></span>
<span data-ttu-id="f278f-120">I det här avsnittet, utföra hello steg toocreate hello AD-program med ett lösenord och tilldela hello Reader rollen toohello tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="f278f-120">In this section, you perform hello steps toocreate hello AD application with a password, and assign hello Reader role toohello service principal.</span></span>

1. <span data-ttu-id="f278f-121">Logga in tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="f278f-121">Sign in tooyour account.</span></span>
   
   ```azurecli
   azure login
   ```
2. <span data-ttu-id="f278f-122">toocreate en app-identitet innehåller hello namnet på hello app och ett lösenord, som visas i hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f278f-122">toocreate an app identity, provide hello name of hello app and a password, as shown in hello following command:</span></span>
     
   ```azurecli
   azure ad sp create -n exampleapp -p {your-password}
   ```
     
   <span data-ttu-id="f278f-123">hello nya tjänstens huvudnamn returneras.</span><span class="sxs-lookup"><span data-stu-id="f278f-123">hello new service principal is returned.</span></span> <span data-ttu-id="f278f-124">hello objekt-Id krävs när tillståndsbeviljande.</span><span class="sxs-lookup"><span data-stu-id="f278f-124">hello Object Id is needed when granting permissions.</span></span> <span data-ttu-id="f278f-125">hello guid som anges med hello Service Principal Names krävs när du loggar in.</span><span class="sxs-lookup"><span data-stu-id="f278f-125">hello guid listed with hello Service Principal Names is needed when logging in.</span></span> <span data-ttu-id="f278f-126">Detta guid är hello samma värde som hello app-id. I hello exempelprogrammen är det här värdet anges tooas hello `Client ID`.</span><span class="sxs-lookup"><span data-stu-id="f278f-126">This guid is hello same value as hello app id. In hello sample applications, this value is referred tooas hello `Client ID`.</span></span> 
     
   ```azurecli
   info:    Executing command ad sp create
     
   Creating application exampleapp
     / Creating service principal for application 7132aca4-1bdb-4238-ad81-996ff91d8db+
     data:    Object Id:               ff863613-e5e2-4a6b-af07-fff6f2de3f4e
     data:    Display Name:            exampleapp
     data:    Service Principal Names:
     data:                             7132aca4-1bdb-4238-ad81-996ff91d8db4
     data:                             https://www.contoso.org/example
     info:    ad sp create command OK
   ```

3. <span data-ttu-id="f278f-127">Bevilja hello service principal behörighet för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f278f-127">Grant hello service principal permissions on your subscription.</span></span> <span data-ttu-id="f278f-128">Lägg till hello service principal toohello rollen Läsare, vilket ger behörighet tooread alla resurser i hello prenumeration i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="f278f-128">In this example, you add hello service principal toohello Reader role, which grants permission tooread all resources in hello subscription.</span></span> <span data-ttu-id="f278f-129">Andra roller, se [RBAC: inbyggda roller](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="f278f-129">For other roles, see [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md).</span></span> <span data-ttu-id="f278f-130">Ange hello objekt-Id som du använde när du skapar hello program för hello objekt-ID-parametern.</span><span class="sxs-lookup"><span data-stu-id="f278f-130">For hello objectid parameter, provide hello Object Id that you used when creating hello application.</span></span> <span data-ttu-id="f278f-131">Innan du kör det här kommandot måste du tillåta en stund innan hello nya service principal toopropagate i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f278f-131">Before running this command, you must allow some time for hello new service principal toopropagate throughout Azure Active Directory.</span></span> <span data-ttu-id="f278f-132">När du kör kommandona manuellt, vanligtvis tillräckligt med tid har gått ut mellan aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="f278f-132">When you run these commands manually, usually enough time has elapsed between tasks.</span></span> <span data-ttu-id="f278f-133">I ett skript, bör du lägga till ett steg toosleep mellan hello-kommandon (t.ex. `sleep 15`).</span><span class="sxs-lookup"><span data-stu-id="f278f-133">In a script, you should add a step toosleep between hello commands (like `sleep 15`).</span></span> <span data-ttu-id="f278f-134">Om du ser ett fel anger hello huvudnamn inte finns i hello directory kör hello kommandot igen.</span><span class="sxs-lookup"><span data-stu-id="f278f-134">If you see an error stating hello principal does not exist in hello directory, rerun hello command.</span></span>
   
   ```azurecli
   azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/
   ```
   
<span data-ttu-id="f278f-135">Klart!</span><span class="sxs-lookup"><span data-stu-id="f278f-135">That's it!</span></span> <span data-ttu-id="f278f-136">Din AD-program och tjänstens huvudnamn ställs in.</span><span class="sxs-lookup"><span data-stu-id="f278f-136">Your AD application and service principal are set up.</span></span> <span data-ttu-id="f278f-137">hello nästa avsnitt visar hur toolog in med hello autentiseringsuppgifter via Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="f278f-137">hello next section shows you how toolog in with hello credential through Azure CLI.</span></span> <span data-ttu-id="f278f-138">Om du vill toouse hello autentiseringsuppgifter i koden programmet behöver inte toocontinue med det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="f278f-138">If you want toouse hello credential in your code application, you do not need toocontinue with this topic.</span></span> <span data-ttu-id="f278f-139">Du kan hoppa toohello [programexempel](#sample-applications) exempel på Logga in med ditt program-id och lösenord.</span><span class="sxs-lookup"><span data-stu-id="f278f-139">You can jump toohello [Sample applications](#sample-applications) for examples of logging in with your application id and password.</span></span> 

### <a name="provide-credentials-through-azure-cli"></a><span data-ttu-id="f278f-140">Ange autentiseringsuppgifter via Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f278f-140">Provide credentials through Azure CLI</span></span>
<span data-ttu-id="f278f-141">Du måste nu toolog i som hello programmet tooperform funktioner.</span><span class="sxs-lookup"><span data-stu-id="f278f-141">Now, you need toolog in as hello application tooperform operations.</span></span>

1. <span data-ttu-id="f278f-142">Varje gång du loggar in som ett huvudnamn för tjänsten, behöver du tooprovide hello klient-id för hello katalog för din AD-app.</span><span class="sxs-lookup"><span data-stu-id="f278f-142">Whenever you sign in as a service principal, you need tooprovide hello tenant id of hello directory for your AD app.</span></span> <span data-ttu-id="f278f-143">En klient är en instans av Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f278f-143">A tenant is an instance of Azure Active Directory.</span></span> <span data-ttu-id="f278f-144">tooretrieve hello klient-id för prenumerationen för tillfället är autentiserat används:</span><span class="sxs-lookup"><span data-stu-id="f278f-144">tooretrieve hello tenant id for your currently authenticated subscription, use:</span></span>
   
   ```azurecli
   azure account show
   ```
   
   <span data-ttu-id="f278f-145">Som returnerar:</span><span class="sxs-lookup"><span data-stu-id="f278f-145">Which returns:</span></span>
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
     <span data-ttu-id="f278f-146">Om du behöver tooget hello klient-id för en annan prenumeration, använder du följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="f278f-146">If you need tooget hello tenant id of another subscription, use hello following command:</span></span>
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. <span data-ttu-id="f278f-147">Om du behöver tooretrieve hello klient-id toouse för att logga in, Använd:</span><span class="sxs-lookup"><span data-stu-id="f278f-147">If you need tooretrieve hello client id toouse for logging in, use:</span></span>
   
   ```azurecli
   azure ad sp show -c exampleapp --json
   ```
   
     <span data-ttu-id="f278f-148">hello värdet toouse för att logga in är hello guid som anges i hello tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="f278f-148">hello value toouse for logging in is hello guid listed in hello service principal names.</span></span>
   
   ```azurecli
   [
     {
       "objectId": "ff863613-e5e2-4a6b-af07-fff6f2de3f4e",
       "objectType": "ServicePrincipal",
       "displayName": "exampleapp",
       "appId": "7132aca4-1bdb-4238-ad81-996ff91d8db4",
       "servicePrincipalNames": [
         "https://www.contoso.org/example",
         "7132aca4-1bdb-4238-ad81-996ff91d8db4"
       ]
     }
   ]
   ```
3. <span data-ttu-id="f278f-149">Logga in som hello tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="f278f-149">Log in as hello service principal.</span></span>
   
   ```azurecli
   azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant {tenant-id}
   ```
   
    <span data-ttu-id="f278f-150">Du ombeds hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="f278f-150">You are prompted for hello password.</span></span> <span data-ttu-id="f278f-151">Ange hello-lösenord som du angav när du skapar hello AD-program.</span><span class="sxs-lookup"><span data-stu-id="f278f-151">Provide hello password you specified when creating hello AD application.</span></span>
   
   ```azurecli
   info:    Executing command login
   Password: ********
   ```

<span data-ttu-id="f278f-152">Du är nu autentiserad som hello tjänstens huvudnamn för hello tjänstens huvudnamn som du skapade.</span><span class="sxs-lookup"><span data-stu-id="f278f-152">You are now authenticated as hello service principal for hello service principal that you created.</span></span>

<span data-ttu-id="f278f-153">Alternativt kan du anropa REST-åtgärder från hello kommandoraden toolog i.</span><span class="sxs-lookup"><span data-stu-id="f278f-153">Alternatively, you can invoke REST operations from hello command line toolog in.</span></span> <span data-ttu-id="f278f-154">Du kan hämta hello åtkomsttoken för användning med andra åtgärder från hello autentiseringssvar.</span><span class="sxs-lookup"><span data-stu-id="f278f-154">From hello authentication response, you can retrieve hello access token for use with other operations.</span></span> <span data-ttu-id="f278f-155">Ett exempel för att hämta hello åtkomst-token genom att anropa REST-åtgärder finns [genererar en åtkomst-Token](resource-manager-rest-api.md#generating-an-access-token).</span><span class="sxs-lookup"><span data-stu-id="f278f-155">For an example of retrieving hello access token by invoking REST operations, see [Generating an Access Token](resource-manager-rest-api.md#generating-an-access-token).</span></span>

## <a name="create-service-principal-with-certificate"></a><span data-ttu-id="f278f-156">Skapa tjänstens huvudnamn med certifikat</span><span class="sxs-lookup"><span data-stu-id="f278f-156">Create service principal with certificate</span></span>
<span data-ttu-id="f278f-157">I det här avsnittet kan du utföra hello steg för att:</span><span class="sxs-lookup"><span data-stu-id="f278f-157">In this section, you perform hello steps to:</span></span>

* <span data-ttu-id="f278f-158">Skapa ett självsignerat certifikat</span><span class="sxs-lookup"><span data-stu-id="f278f-158">create a self-signed certificate</span></span>
* <span data-ttu-id="f278f-159">Skapa hello AD-program med hello certifikat och hello tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="f278f-159">create hello AD application with hello certificate, and hello service principal</span></span>
* <span data-ttu-id="f278f-160">tilldela hello Reader rollen toohello tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="f278f-160">assign hello Reader role toohello service principal</span></span>

<span data-ttu-id="f278f-161">toocomplete dessa steg, måste du ha [OpenSSL](http://www.openssl.org/) installerad.</span><span class="sxs-lookup"><span data-stu-id="f278f-161">toocomplete these steps, you must have [OpenSSL](http://www.openssl.org/) installed.</span></span>

1. <span data-ttu-id="f278f-162">Skapa ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="f278f-162">Create a self-signed certificate.</span></span>
   
   ```
   openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'
   ```

2. <span data-ttu-id="f278f-163">hello före steget Skapa två filer - privkey.pem och cert.pem.</span><span class="sxs-lookup"><span data-stu-id="f278f-163">hello preceding step created two files - privkey.pem and cert.pem.</span></span> <span data-ttu-id="f278f-164">Kombinera hello offentliga och privata nycklar i en enda fil.</span><span class="sxs-lookup"><span data-stu-id="f278f-164">Combine hello public and private keys into a single file.</span></span>

   ```
   cat privkey.pem cert.pem > examplecert.pem
   ```

3. <span data-ttu-id="f278f-165">Öppna hello **examplecert.pem** filen och leta efter hello lång teckenföljd mellan **---BEGIN CERTIFICATE---** och **---END CERTIFICATE---**.</span><span class="sxs-lookup"><span data-stu-id="f278f-165">Open hello **examplecert.pem** file and look for hello long sequence of characters between **-----BEGIN CERTIFICATE-----** and **-----END CERTIFICATE-----**.</span></span> <span data-ttu-id="f278f-166">Kopiera hello certifikatdata.</span><span class="sxs-lookup"><span data-stu-id="f278f-166">Copy hello certificate data.</span></span> <span data-ttu-id="f278f-167">Skicka informationen som en parameter när du skapar hello tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="f278f-167">You pass this data as a parameter when creating hello service principal.</span></span>

4. <span data-ttu-id="f278f-168">Logga in tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="f278f-168">Sign in tooyour account.</span></span>

   ```azurecli
   azure login
   ```
5. <span data-ttu-id="f278f-169">toocreate hello tjänstens huvudnamn, ange hello namn för hello app och hello certifikatdata som visas i hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f278f-169">toocreate hello service principal, provide hello name of hello app and hello certificate data, as shown in hello following command:</span></span>
     
   ```azurecli
   azure ad sp create -n exampleapp --cert-value {certificate data}
   ```
     
   <span data-ttu-id="f278f-170">hello nya tjänstens huvudnamn returneras.</span><span class="sxs-lookup"><span data-stu-id="f278f-170">hello new service principal is returned.</span></span> <span data-ttu-id="f278f-171">hello objekt-Id krävs när tillståndsbeviljande.</span><span class="sxs-lookup"><span data-stu-id="f278f-171">hello Object Id is needed when granting permissions.</span></span> <span data-ttu-id="f278f-172">hello guid som anges med hello Service Principal Names krävs när du loggar in.</span><span class="sxs-lookup"><span data-stu-id="f278f-172">hello guid listed with hello Service Principal Names is needed when logging in.</span></span> <span data-ttu-id="f278f-173">Detta guid är hello samma värde som hello app-id. I hello exempelprogrammen är det här värdet anges tooas hello klient-ID.</span><span class="sxs-lookup"><span data-stu-id="f278f-173">This guid is hello same value as hello app id. In hello sample applications, this value is referred tooas hello Client ID.</span></span> 
     
   ```azurecli
   info:    Executing command ad sp create
     
   Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
     data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
     data:    Display Name:     exampleapp
     data:    Service Principal Names:
     data:                      4fd39843-c338-417d-b549-a545f584a745
     data:                      https://www.contoso.org/example
     info:    ad sp create command OK
   ```
6. <span data-ttu-id="f278f-174">Bevilja hello service principal behörighet för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f278f-174">Grant hello service principal permissions on your subscription.</span></span> <span data-ttu-id="f278f-175">Lägg till hello service principal toohello rollen Läsare, vilket ger behörighet tooread alla resurser i hello prenumeration i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="f278f-175">In this example, you add hello service principal toohello Reader role, which grants permission tooread all resources in hello subscription.</span></span> <span data-ttu-id="f278f-176">Andra roller, se [RBAC: inbyggda roller](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="f278f-176">For other roles, see [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md).</span></span> <span data-ttu-id="f278f-177">Ange hello objekt-Id som du använde när du skapar hello program för hello objekt-ID-parametern.</span><span class="sxs-lookup"><span data-stu-id="f278f-177">For hello objectid parameter, provide hello Object Id that you used when creating hello application.</span></span> <span data-ttu-id="f278f-178">Innan du kör det här kommandot måste du tillåta en stund innan hello nya service principal toopropagate i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f278f-178">Before running this command, you must allow some time for hello new service principal toopropagate throughout Azure Active Directory.</span></span> <span data-ttu-id="f278f-179">När du kör kommandona manuellt, vanligtvis tillräckligt med tid har gått ut mellan aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="f278f-179">When you run these commands manually, usually enough time has elapsed between tasks.</span></span> <span data-ttu-id="f278f-180">I ett skript, bör du lägga till ett steg toosleep mellan hello-kommandon (t.ex. `sleep 15`).</span><span class="sxs-lookup"><span data-stu-id="f278f-180">In a script, you should add a step toosleep between hello commands (like `sleep 15`).</span></span> <span data-ttu-id="f278f-181">Om du ser ett fel anger hello huvudnamn inte finns i hello directory kör hello kommandot igen.</span><span class="sxs-lookup"><span data-stu-id="f278f-181">If you see an error stating hello principal does not exist in hello directory, rerun hello command.</span></span>
   
   ```azurecli
   azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/
   ```
  
### <a name="provide-certificate-through-automated-azure-cli-script"></a><span data-ttu-id="f278f-182">Ange certifikat via automatisk Azure CLI-skript</span><span class="sxs-lookup"><span data-stu-id="f278f-182">Provide certificate through automated Azure CLI script</span></span>
<span data-ttu-id="f278f-183">Du måste nu toolog i som hello programmet tooperform funktioner.</span><span class="sxs-lookup"><span data-stu-id="f278f-183">Now, you need toolog in as hello application tooperform operations.</span></span>

1. <span data-ttu-id="f278f-184">Varje gång du loggar in som ett huvudnamn för tjänsten, behöver du tooprovide hello klient-id för hello katalog för din AD-app.</span><span class="sxs-lookup"><span data-stu-id="f278f-184">Whenever you sign in as a service principal, you need tooprovide hello tenant id of hello directory for your AD app.</span></span> <span data-ttu-id="f278f-185">En klient är en instans av Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f278f-185">A tenant is an instance of Azure Active Directory.</span></span> <span data-ttu-id="f278f-186">tooretrieve hello klient-id för prenumerationen för tillfället är autentiserat används:</span><span class="sxs-lookup"><span data-stu-id="f278f-186">tooretrieve hello tenant id for your currently authenticated subscription, use:</span></span>
   
   ```azurecli
   azure account show
   ```
   
   <span data-ttu-id="f278f-187">Som returnerar:</span><span class="sxs-lookup"><span data-stu-id="f278f-187">Which returns:</span></span>
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
   <span data-ttu-id="f278f-188">Om du behöver tooget hello klient-id för en annan prenumeration, använder du följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="f278f-188">If you need tooget hello tenant id of another subscription, use hello following command:</span></span>
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. <span data-ttu-id="f278f-189">tooretrieve hello tumavtryck för certifikat och ta bort onödiga tecken kan använda:</span><span class="sxs-lookup"><span data-stu-id="f278f-189">tooretrieve hello certificate thumbprint and remove unneeded characters, use:</span></span>
   
   ```
   openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
   ```
   
   <span data-ttu-id="f278f-190">Som returnerar ett tumavtrycksvärde liknar:</span><span class="sxs-lookup"><span data-stu-id="f278f-190">Which returns a thumbprint value similar to:</span></span>
   
   ```
   30996D9CE48A0B6E0CD49DBB9A48059BF9355851
   ```
3. <span data-ttu-id="f278f-191">Om du behöver tooretrieve hello klient-id toouse för att logga in, Använd:</span><span class="sxs-lookup"><span data-stu-id="f278f-191">If you need tooretrieve hello client id toouse for logging in, use:</span></span>
   
   ```azurecli
   azure ad sp show -c exampleapp
   ```
   
   <span data-ttu-id="f278f-192">hello värdet toouse för att logga in är hello guid som anges i hello tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="f278f-192">hello value toouse for logging in is hello guid listed in hello service principal names.</span></span>
     
   ```azurecli
   [
     {
       "objectId": "7dbc8265-51ed-4038-8e13-31948c7f4ce7",
       "objectType": "ServicePrincipal",
       "displayName": "exampleapp",
       "appId": "4fd39843-c338-417d-b549-a545f584a745",
       "servicePrincipalNames": [
         "https://www.contoso.org/example",
         "4fd39843-c338-417d-b549-a545f584a745"
       ]
     }
   ]
   ```
4. <span data-ttu-id="f278f-193">Logga in som hello tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="f278f-193">Log in as hello service principal.</span></span>
   
   ```azurecli
   azure login --service-principal --tenant {tenant-id} -u 4fd39843-c338-417d-b549-a545f584a745 --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}
   ```

<span data-ttu-id="f278f-194">Du är nu autentiserad som hello tjänstens huvudnamn för hello Azure Active Directory-program som du skapade.</span><span class="sxs-lookup"><span data-stu-id="f278f-194">You are now authenticated as hello service principal for hello Azure Active Directory application that you created.</span></span>

## <a name="change-credentials"></a><span data-ttu-id="f278f-195">Ändra autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="f278f-195">Change credentials</span></span>

<span data-ttu-id="f278f-196">toochange hello autentiseringsuppgifter för en AD-app, antingen på grund av ett säkerhetsintrång eller en autentiseringsuppgift förfallodatum, Använd `azure ad app set`.</span><span class="sxs-lookup"><span data-stu-id="f278f-196">toochange hello credentials for an AD app, either because of a security compromise or a credential expiration, use `azure ad app set`.</span></span>

<span data-ttu-id="f278f-197">toochange ett lösenord, Använd:</span><span class="sxs-lookup"><span data-stu-id="f278f-197">toochange a password, use:</span></span>

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --password p@ssword
```

<span data-ttu-id="f278f-198">toochange certifikatvärdet, Använd:</span><span class="sxs-lookup"><span data-stu-id="f278f-198">toochange a certificate value, use:</span></span>

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --cert-value {certificate data}
```

## <a name="debug"></a><span data-ttu-id="f278f-199">Felsökning</span><span class="sxs-lookup"><span data-stu-id="f278f-199">Debug</span></span>

<span data-ttu-id="f278f-200">Du kan stöta på hello följande fel när du skapar ett huvudnamn för tjänsten:</span><span class="sxs-lookup"><span data-stu-id="f278f-200">You may encounter hello following errors when creating a service principal:</span></span>

* <span data-ttu-id="f278f-201">**”Authentication_Unauthorized”** eller **”ingen prenumeration finns i kontexten för hello”.**</span><span class="sxs-lookup"><span data-stu-id="f278f-201">**"Authentication_Unauthorized"** or **"No subscription found in hello context."**</span></span> <span data-ttu-id="f278f-202">– Du ser det här felet när ditt konto inte har hello [nödvändiga behörigheter](#required-permissions) på hello Azure Active Directory tooregister en app.</span><span class="sxs-lookup"><span data-stu-id="f278f-202">- You see this error when your account does not have hello [required permissions](#required-permissions) on hello Azure Active Directory tooregister an app.</span></span> <span data-ttu-id="f278f-203">Normalt ser du det här felet när endast administrativa användare i Azure Active Directory kan du registrera appar och ditt konto är inte en administratör. Be din administratör tooeither tilldela tooan administratörsroll eller tooenable användare tooregister appar.</span><span class="sxs-lookup"><span data-stu-id="f278f-203">Typically, you see this error when only admin users in your Azure Active Directory can register apps, and your account is not an admin. Ask your administrator tooeither assign you tooan administrator role, or tooenable users tooregister apps.</span></span>

* <span data-ttu-id="f278f-204">Ditt konto **”har inte tillstånd tooperform åtgärd 'Microsoft.Authorization/roleAssignments/write' över område '/subscriptions/ {guid}'”.**  -Det här felet visas när ditt konto inte har tillräckliga behörigheter tooassign en roll tooan identitet.</span><span class="sxs-lookup"><span data-stu-id="f278f-204">Your account **"does not have authorization tooperform action 'Microsoft.Authorization/roleAssignments/write' over scope '/subscriptions/{guid}'."** - You see this error when your account does not have sufficient permissions tooassign a role tooan identity.</span></span> <span data-ttu-id="f278f-205">Be din prenumeration administratören tooadd du tooUser åtkomst rollen Administratör.</span><span class="sxs-lookup"><span data-stu-id="f278f-205">Ask your subscription administrator tooadd you tooUser Access Administrator role.</span></span>

## <a name="sample-applications"></a><span data-ttu-id="f278f-206">Exempelprogram</span><span class="sxs-lookup"><span data-stu-id="f278f-206">Sample applications</span></span>
<span data-ttu-id="f278f-207">För information om att logga in som hello program via olika plattformar, se:</span><span class="sxs-lookup"><span data-stu-id="f278f-207">For information about logging in as hello application through different platforms, see:</span></span>

* [<span data-ttu-id="f278f-208">.NET</span><span class="sxs-lookup"><span data-stu-id="f278f-208">.NET</span></span>](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [<span data-ttu-id="f278f-209">Java</span><span class="sxs-lookup"><span data-stu-id="f278f-209">Java</span></span>](/java/azure/java-sdk-azure-authenticate)
* [<span data-ttu-id="f278f-210">Node.js</span><span class="sxs-lookup"><span data-stu-id="f278f-210">Node.js</span></span>](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [<span data-ttu-id="f278f-211">Python</span><span class="sxs-lookup"><span data-stu-id="f278f-211">Python</span></span>](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [<span data-ttu-id="f278f-212">Ruby</span><span class="sxs-lookup"><span data-stu-id="f278f-212">Ruby</span></span>](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a><span data-ttu-id="f278f-213">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f278f-213">Next steps</span></span>
* <span data-ttu-id="f278f-214">Detaljerade anvisningar för att integrera ett program i Azure för att hantera resurser, se [Developer's guide tooauthorization med hello Azure Resource Manager API](resource-manager-api-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="f278f-214">For detailed steps on integrating an application into Azure for managing resources, see [Developer's guide tooauthorization with hello Azure Resource Manager API](resource-manager-api-authentication.md).</span></span>
* <span data-ttu-id="f278f-215">Mer information om hur du använder certifikat och Azure CLI finns i tooget [certifikatbaserad autentisering med Azure-tjänstens huvudnamn från Linux-kommandorad](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx).</span><span class="sxs-lookup"><span data-stu-id="f278f-215">tooget more information about using certificates and Azure CLI, see [Certificate-based authentication with Azure Service Principals from Linux command line](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx).</span></span> 
* <span data-ttu-id="f278f-216">En lista över tillgängliga åtgärder som kan beviljas eller nekas toousers finns [Azure Resource Manager Resource Provider operations](../active-directory/role-based-access-control-resource-provider-operations.md).</span><span class="sxs-lookup"><span data-stu-id="f278f-216">For a list of available actions that can be granted or denied toousers, see [Azure Resource Manager Resource Provider operations](../active-directory/role-based-access-control-resource-provider-operations.md).</span></span>
