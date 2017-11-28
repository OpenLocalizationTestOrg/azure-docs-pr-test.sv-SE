---
title: "aaaManaging enheter med hjälp av hello Azure portal – preview | Microsoft Docs"
description: "Lär dig hur toouse hello Azure portal toomanage enheter."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: a39d14e4ce8bb79f0223a9de40d5f1259a869927
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-devices-using-hello-azure-portal---preview"></a><span data-ttu-id="05a56-103">Hantera enheter med hjälp av hello Azure-portalen – förhandsgranskning</span><span class="sxs-lookup"><span data-stu-id="05a56-103">Managing devices using hello Azure portal - preview</span></span>

>[!NOTE]
><span data-ttu-id="05a56-104">Den här funktionen är för närvarande i förhandsversion.</span><span class="sxs-lookup"><span data-stu-id="05a56-104">This capability currently is in public preview.</span></span> <span data-ttu-id="05a56-105">Förbereda toorevert eller ta bort alla ändringar.</span><span class="sxs-lookup"><span data-stu-id="05a56-105">Be prepared toorevert or remove any changes.</span></span> <span data-ttu-id="05a56-106">hello-funktionen är tillgänglig i alla Azure Active Directory (Azure AD)-prenumeration under förhandsversion.</span><span class="sxs-lookup"><span data-stu-id="05a56-106">hello feature is available in any Azure Active Directory (Azure AD) subscription during public preview.</span></span> <span data-ttu-id="05a56-107">När hello funktionen blir allmänt tillgänglig, kan vissa aspekter av hello funktionen kräver en Azure Active Directory premium-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="05a56-107">However, when hello feature becomes generally available, some aspects of hello feature might require an Azure Active Directory premium subscription.</span></span>


<span data-ttu-id="05a56-108">Med hantering av enheter i Azure Active Directory (Azure AD), kan du se till att dina användare har åtkomst till dina resurser från enheter som uppfyller dina krav för säkerhet och efterlevnad.</span><span class="sxs-lookup"><span data-stu-id="05a56-108">With device management in Azure Active Directory (Azure AD), you can ensure that your users are accessing your resources from devices that meet your standards for security and compliance.</span></span> 

<span data-ttu-id="05a56-109">Det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="05a56-109">This topic:</span></span>

- <span data-ttu-id="05a56-110">Förutsätter att du är bekant med hello [introduktion toodevice management i Azure Active Directory](device-management-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="05a56-110">Assumes that you are familiar with hello [introduction toodevice management in Azure Active Directory](device-management-introduction.md)</span></span>

- <span data-ttu-id="05a56-111">Ger dig med information om att hantera dina enheter med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="05a56-111">Provides you with information about managing your devices using hello Azure portal</span></span>


<span data-ttu-id="05a56-112">toomanage enheter i hello Azure-portalen, måste tooclick **enheter** i hello **hantera** avsnitt i hello hello **Azure Active Directory** bladet.</span><span class="sxs-lookup"><span data-stu-id="05a56-112">toomanage devices in hello Azure portal, you need tooclick **Devices** in hello **Manage** section of hello hello **Azure Active Directory** blade.</span></span>

![Hantera en enhet i Intune](./media/device-management-azure-portal/11.png)




## <a name="configure-device-settings"></a><span data-ttu-id="05a56-114">Konfigurera inställningar för enheter</span><span class="sxs-lookup"><span data-stu-id="05a56-114">Configure device settings</span></span>

<span data-ttu-id="05a56-115">toomanage dina enheter med hjälp av hello Azure-portalen, de behöver toobe registrerad eller anslutna tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="05a56-115">toomanage your devices using hello Azure portal, they need toobe either registered or joined tooAzure AD.</span></span> <span data-ttu-id="05a56-116">Du kan finjustera hello process för att registrera och ansluta enheter genom att konfigurera hello Enhetsinställningar som en administratör.</span><span class="sxs-lookup"><span data-stu-id="05a56-116">As an administrator, you can fine-tune hello process of registering and joining devices by configuring hello device settings.</span></span>

![Hantera en enhet i Intune](./media/device-management-azure-portal/22.png)


<span data-ttu-id="05a56-118">hello inställningsbladet för enheten kan du tooconfigure:</span><span class="sxs-lookup"><span data-stu-id="05a56-118">hello device settings blade enables you tooconfigure:</span></span>

- <span data-ttu-id="05a56-119">**Användare kan ansluta enheter tooAzure AD** – denna inställningar kan du tooselect hello användare som kan ansluta enheter tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="05a56-119">**Users may join devices tooAzure AD** - This settings enables you tooselect hello users who can join devices tooAzure AD.</span></span> <span data-ttu-id="05a56-120">hello standardvärdet är **alla**.</span><span class="sxs-lookup"><span data-stu-id="05a56-120">hello default is **All**.</span></span>

- <span data-ttu-id="05a56-121">**Ytterligare lokala administratörer på Azure AD-anslutna enheter** -du kan välja hello användare som beviljas lokal administratörsbehörighet på en enhet.</span><span class="sxs-lookup"><span data-stu-id="05a56-121">**Additional local administrators on Azure AD joined devices** - You can select hello users that are granted local administrator rights on a device.</span></span> <span data-ttu-id="05a56-122">Användare som läggs till här läggs toohello *Enhetsadministratörer* roll i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="05a56-122">Users added here are added toohello *Device Administrators* role in Azure AD.</span></span> <span data-ttu-id="05a56-123">Globala administratörer i Azure AD och enhetsägare beviljas lokal administratörsbehörighet som standard.</span><span class="sxs-lookup"><span data-stu-id="05a56-123">Global administrators in Azure AD and device owners are granted local administrator rights by default.</span></span> <span data-ttu-id="05a56-124">Det här alternativet är en premium edition funktioner som är tillgängliga via produkter, till exempel Azure AD Premium eller hello Enterprise Mobility Suite (EMS).</span><span class="sxs-lookup"><span data-stu-id="05a56-124">This option is a premium edition capability available through products such as Azure AD Premium or hello Enterprise Mobility Suite (EMS).</span></span> 

- <span data-ttu-id="05a56-125">**Användarna kan registrera sina enheter med Azure AD** -du behöver tooconfigure toobe den här inställningen tooallow enheter som registrerats med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="05a56-125">**Users may register their devices with Azure AD** - You need tooconfigure this setting tooallow devices toobe registered with Azure AD.</span></span> <span data-ttu-id="05a56-126">Om du väljer **ingen**, enheter är inte tillåtna tooregister när de inte Azure AD ansluten eller hybrid anslutna Azure AD.</span><span class="sxs-lookup"><span data-stu-id="05a56-126">If you select **None**, devices are not allowed tooregister when they are not Azure AD joined or hybrid Azure AD joined.</span></span> <span data-ttu-id="05a56-127">Registrering med Microsoft Intune eller hantering av mobilenheter (MDM) för Office 365 kräver registrering.</span><span class="sxs-lookup"><span data-stu-id="05a56-127">Enrollment with Microsoft Intune or Mobile Device Management (MDM) for Office 365 requires registration.</span></span> <span data-ttu-id="05a56-128">Om du har konfigurerat någon av dessa tjänster **alla** är markerad och **NONE** är inte tillgänglig...</span><span class="sxs-lookup"><span data-stu-id="05a56-128">If you have configured either of these services, **ALL** is selected and **NONE** is not available..</span></span>

- <span data-ttu-id="05a56-129">**Kräv Multi-Factor Authentication toojoin enheter** -du kan välja om användare som är nödvändiga tooprovide en andra autentiseringsmetod factor toojoin deras enhet tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="05a56-129">**Require Multi-Factor Auth toojoin devices** - You can choose whether users are required tooprovide a second authentication factor toojoin their device tooAzure AD.</span></span> <span data-ttu-id="05a56-130">hello standardvärdet är **nr**.</span><span class="sxs-lookup"><span data-stu-id="05a56-130">hello default is **No**.</span></span> <span data-ttu-id="05a56-131">Vi rekommenderar att kräva multifaktorautentisering vid registrering av en enhet.</span><span class="sxs-lookup"><span data-stu-id="05a56-131">We recommend requiring multi-factor authentication when registering a device.</span></span> <span data-ttu-id="05a56-132">Innan du aktiverar multifaktorautentisering för den här tjänsten måste du kontrollera att multifaktorautentisering har konfigurerats för hello användare som registrerar sina enheter.</span><span class="sxs-lookup"><span data-stu-id="05a56-132">Before you enable multi-factor authentication for this service, you must ensure that multi-factor authentication is configured for hello users that register their devices.</span></span> <span data-ttu-id="05a56-133">Mer information om olika Azure Multi-Factor authentication-tjänster finns [komma igång med Azure Multi-Factor authentication](../multi-factor-authentication/multi-factor-authentication-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="05a56-133">For more information on different Azure multi-factor authentication services, see [getting started with Azure multi-factor authentication](../multi-factor-authentication/multi-factor-authentication-get-started.md).</span></span> 

- <span data-ttu-id="05a56-134">**Högsta antal enheter** -den här inställningen aktiverar tooselect hello maximalt antal enheter som en användare kan ha i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="05a56-134">**Maximum number of devices** - This setting enables you tooselect hello maximum number of devices that a user can have in Azure AD.</span></span> <span data-ttu-id="05a56-135">Om användarna når den här kvoten, de är inte kan tooadd fler enheter förrän ett eller flera av hello befintliga enheter har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="05a56-135">If a user reaches this quota, they are not be able tooadd additional devices until one or more of hello existing devices are removed.</span></span> <span data-ttu-id="05a56-136">hello enheten citattecken räknas för alla enheter som är Azure AD ansluten eller Azure AD som registrerade idag.</span><span class="sxs-lookup"><span data-stu-id="05a56-136">hello device quote is counted for all devices that are either Azure AD joined or Azure AD registered today.</span></span> <span data-ttu-id="05a56-137">hello standardvärdet är **20**.</span><span class="sxs-lookup"><span data-stu-id="05a56-137">hello default value is **20**.</span></span>

- <span data-ttu-id="05a56-138">**Användarna kan synkronisera AppData och inställningar mellan enheter** -som standard är inställningen för**NONE**.</span><span class="sxs-lookup"><span data-stu-id="05a56-138">**Users may sync settings and app data across devices** - By default, this setting is set too**NONE**.</span></span> <span data-ttu-id="05a56-139">Välja specifika användare eller grupper eller alla ger hello användarens inställningar och app data toosync över deras Windows 10-enheter.</span><span class="sxs-lookup"><span data-stu-id="05a56-139">Selecting specific users or groups or ALL allows hello user’s settings and app data toosync across their Windows 10 devices.</span></span> <span data-ttu-id="05a56-140">Mer information om hur synkroniseringen fungerar i Windows 10.</span><span class="sxs-lookup"><span data-stu-id="05a56-140">Learn more on how sync works in Windows 10.</span></span>
<span data-ttu-id="05a56-141">Det här alternativet är en premium-funktioner som är tillgängliga via produkter, till exempel Azure AD Premium eller hello Enterprise Mobility Suite (EMS).</span><span class="sxs-lookup"><span data-stu-id="05a56-141">This option is a premium capability available through products such as Azure AD Premium or hello Enterprise Mobility Suite (EMS).</span></span>
 
    ![Hantera en enhet i Intune](./media/device-management-azure-portal/21.png)




## <a name="locate-devices"></a><span data-ttu-id="05a56-143">Hitta enheter</span><span class="sxs-lookup"><span data-stu-id="05a56-143">Locate devices</span></span>

<span data-ttu-id="05a56-144">Som en administratör har i hello Azure-portalen du två alternativ toolocate registrerade och anslutna enheter:</span><span class="sxs-lookup"><span data-stu-id="05a56-144">As an administrator, in hello Azure portal, you have two options toolocate registered and joined devices:</span></span>

- <span data-ttu-id="05a56-145">**Alla enheter** i hello **hantera** avsnitt i hello **enheter** bladet</span><span class="sxs-lookup"><span data-stu-id="05a56-145">**All devices** in hello **Manage** section of hello **Devices** blade</span></span>  

    ![Alla enheter](./media/device-management-azure-portal/41.png)


- <span data-ttu-id="05a56-147">**Enheter** i hello **hantera** avsnitt i en **användaren** bladet</span><span class="sxs-lookup"><span data-stu-id="05a56-147">**Devices** in hello **Manage** section of a **User** blade</span></span>
 
    ![Alla enheter](./media/device-management-azure-portal/43.png)



<span data-ttu-id="05a56-149">Med båda alternativen kan du visa tooa som:</span><span class="sxs-lookup"><span data-stu-id="05a56-149">With both options, you can get tooa view that:</span></span>


- <span data-ttu-id="05a56-150">Låter dig toosearch för enheter med hello visningsnamn som filter.</span><span class="sxs-lookup"><span data-stu-id="05a56-150">Enables you toosearch for devices using hello display name as filter.</span></span>

- <span data-ttu-id="05a56-151">Ger detaljerad översikt över registrerade och domänanslutna enheter</span><span class="sxs-lookup"><span data-stu-id="05a56-151">Provides you with detailed overview of registered and joined devices</span></span>

- <span data-ttu-id="05a56-152">Aktiverar tooperform vanliga hanteringsuppgifter för enhet</span><span class="sxs-lookup"><span data-stu-id="05a56-152">Enables you tooperform common device management tasks</span></span>
   

![Alla enheter](./media/device-management-azure-portal/51.png)


## <a name="device-management-tasks"></a><span data-ttu-id="05a56-154">Enhetens hanteringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="05a56-154">Device management tasks</span></span>

<span data-ttu-id="05a56-155">Som administratör kan du hantera hello registrerade eller anslutna enheter.</span><span class="sxs-lookup"><span data-stu-id="05a56-155">As an administrator, you can manage hello registered or joined devices.</span></span> <span data-ttu-id="05a56-156">Det här avsnittet ger information om vanliga hanteringsuppgifter för enheten.</span><span class="sxs-lookup"><span data-stu-id="05a56-156">This section provides you with information about common device management tasks.</span></span>


<span data-ttu-id="05a56-157">**Hantera en enhet i Intune** -om du är administratör Intune kan du hantera enheter som har markerats som **Microsoft Intune**.</span><span class="sxs-lookup"><span data-stu-id="05a56-157">**Manage an Intune device** - If you are an Intune administrator, you can manage devices marked as **Microsoft Intune**.</span></span> <span data-ttu-id="05a56-158">En administratör kan se ytterligare enheter</span><span class="sxs-lookup"><span data-stu-id="05a56-158">An administrator can see additional device</span></span> 

![Hantera en enhet i Intune](./media/device-management-azure-portal/31.png)


<span data-ttu-id="05a56-160">**Aktivera / inaktivera en enhet i Azure AD**</span><span class="sxs-lookup"><span data-stu-id="05a56-160">**Enable / disable an Azure AD device**</span></span>

<span data-ttu-id="05a56-161">tooenable eller inaktivera en enhet måste toobe en global administratör i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="05a56-161">tooenable or disable a device, you need toobe a global administrator in Azure  AD.</span></span> <span data-ttu-id="05a56-162">Inaktivera en enhet förhindrar att en enhet från att komma åt resurserna i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="05a56-162">Disabling a device prevents a device from accessing your Azure AD resources.</span></span>  <span data-ttu-id="05a56-163">toodisable hello enhet som du kan klicka *...*</span><span class="sxs-lookup"><span data-stu-id="05a56-163">toodisable hello device, you can either click *…*</span></span> <span data-ttu-id="05a56-164">Klicka på hello enhet för ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="05a56-164">click hello device for additional details.</span></span>

 
![Hantera en enhet i Intune](./media/device-management-azure-portal/33.png)

<span data-ttu-id="05a56-166">Inaktivera en enhet ändras hello tillstånd i hello **AKTIVERAD** kolumn för**nr**.</span><span class="sxs-lookup"><span data-stu-id="05a56-166">Disabling a device changes hello state in hello **ENABLED** column too**No**.</span></span>

![Inaktivera en enhet](./media/device-management-azure-portal/32.png)


<span data-ttu-id="05a56-168">**Ta bort en Azure AD-enhet** -toodelete en enhet måste toobe en global administratör i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="05a56-168">**Delete an Azure AD device** - toodelete a device, you need toobe a global administrator in Azure AD.</span></span>  
<span data-ttu-id="05a56-169">Tar bort en enhet:</span><span class="sxs-lookup"><span data-stu-id="05a56-169">Deleting a device:</span></span>
 
- <span data-ttu-id="05a56-170">Förhindrar att en enhet från att komma åt resurserna i Azure AD</span><span class="sxs-lookup"><span data-stu-id="05a56-170">Prevents a device from accessing your Azure AD resources</span></span> 

- <span data-ttu-id="05a56-171">Tar bort all information som är bifogade toohello enhet, till exempel, BitLocker-nycklar för Windows-enheter</span><span class="sxs-lookup"><span data-stu-id="05a56-171">Removes all details that are attached toohello device, for example, BitLocker keys for Windows devices</span></span>  

- <span data-ttu-id="05a56-172">Representerar en oåterkalleligt aktivitet och rekommenderas inte om det inte krävs</span><span class="sxs-lookup"><span data-stu-id="05a56-172">Represents a non-recoverable activity and is not recommended unless it is required</span></span>

<span data-ttu-id="05a56-173">Om en enhet hanteras av en annan hanterare (t.ex. Microsoft Intune), kontrollerar du att hello enheten har rensats / dragits tillbaka innan du tar bort hello enheten i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="05a56-173">If a device is managed by another management authority (e.g. Microsoft Intune), please make sure that hello device has been wiped / retired before deleting hello device in Azure AD.</span></span>

<span data-ttu-id="05a56-174">Du kan antingen välja ”...”</span><span class="sxs-lookup"><span data-stu-id="05a56-174">You can either select “…”</span></span> <span data-ttu-id="05a56-175">toodelete hello enhet eller klicka på hello enhet för ytterligare information</span><span class="sxs-lookup"><span data-stu-id="05a56-175">toodelete hello device or click on hello device for additional details</span></span>
 
![Ta bort en enhet](./media/device-management-azure-portal/34.png)


<span data-ttu-id="05a56-177">**Visa eller kopiera enhets-ID** -du kan använda en enhet ID tooverify hello enhetsinformation ID på hello enhet eller med hjälp av PowerShell vid felsökning.</span><span class="sxs-lookup"><span data-stu-id="05a56-177">**View or copy device ID** - You can use a device ID tooverify hello device ID details on hello device or using PowerShell during troubleshooting.</span></span> <span data-ttu-id="05a56-178">tooaccess hello kopia klickar du på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="05a56-178">tooaccess hello copy option, click hello device.</span></span>

![Visa en enhets-ID](./media/device-management-azure-portal/35.png)
  

<span data-ttu-id="05a56-180">**Visa eller kopiera BitLocker-nycklar** -om du är administratör kan du visa och kopiera hello BitLocker-nycklar toohelp användare toorecover sina krypterade enheten.</span><span class="sxs-lookup"><span data-stu-id="05a56-180">**View or copy BitLocker keys** - If you are an administrator, you can view and copy hello BitLocker keys toohelp users toorecover their encrypted drive.</span></span> <span data-ttu-id="05a56-181">Nycklarna är bara tillgängliga för Windows-enheter som är krypterade och har sina nycklar lagras i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="05a56-181">These keys are only available for Windows devices that are encrypted and have their keys stored in Azure AD.</span></span> <span data-ttu-id="05a56-182">Du kan kopiera dessa nycklar vid åtkomst till information om hello enhet.</span><span class="sxs-lookup"><span data-stu-id="05a56-182">You can copy these keys when accessing details of hello device.</span></span>
 
![Visa BitLocker-nycklar](./media/device-management-azure-portal/36.png)



## <a name="audit-logs"></a><span data-ttu-id="05a56-184">Granskningsloggar</span><span class="sxs-lookup"><span data-stu-id="05a56-184">Audit logs</span></span>


<span data-ttu-id="05a56-185">hello enheten aktiviteter är tillgängliga via hello aktivitetsloggar.</span><span class="sxs-lookup"><span data-stu-id="05a56-185">hello device activities are available through hello activity logs.</span></span> <span data-ttu-id="05a56-186">Detta omfattar aktiviteter som utlöses av hello registreringstjänsten för enheten eller hello användare:</span><span class="sxs-lookup"><span data-stu-id="05a56-186">This includes activities triggered by hello device registration service or by hello user:</span></span>

- <span data-ttu-id="05a56-187">Skapa en enhet och lägga till ägare/användare på hello-enhet</span><span class="sxs-lookup"><span data-stu-id="05a56-187">Device creation and adding owners/users on hello device</span></span>

- <span data-ttu-id="05a56-188">Ändrar toodevice inställningar</span><span class="sxs-lookup"><span data-stu-id="05a56-188">Changes toodevice settings</span></span>

- <span data-ttu-id="05a56-189">Enheten åtgärder, till exempel ta bort eller uppdatera en enhet</span><span class="sxs-lookup"><span data-stu-id="05a56-189">Device operations such as deleting or updating a device</span></span>
 
<span data-ttu-id="05a56-190">Din inmatning punkt toohello granska data är **granskningsloggar** i hello **aktiviteten** avsnitt i hello **enheter* bladet.</span><span class="sxs-lookup"><span data-stu-id="05a56-190">Your entry point toohello auditing data is **Audit logs** in hello **Activity** section of hello **Devices* blade.</span></span>

![Granskningsloggar](./media/device-management-azure-portal/61.png)


<span data-ttu-id="05a56-192">En granskningslogg har en standardlistvy som visar:</span><span class="sxs-lookup"><span data-stu-id="05a56-192">An audit log has a default list view that shows:</span></span>

- <span data-ttu-id="05a56-193">hello datum och tid för hello förekomst</span><span class="sxs-lookup"><span data-stu-id="05a56-193">hello date and time of hello occurrence</span></span>

- <span data-ttu-id="05a56-194">hello mål</span><span class="sxs-lookup"><span data-stu-id="05a56-194">hello targets</span></span>

- <span data-ttu-id="05a56-195">Hej initieraren / aktören (som) för en aktivitet</span><span class="sxs-lookup"><span data-stu-id="05a56-195">hello initiator / actor (who) of an activity</span></span>

- <span data-ttu-id="05a56-196">hello-aktivitet (vad)</span><span class="sxs-lookup"><span data-stu-id="05a56-196">hello activity (what)</span></span>

![Granskningsloggar](./media/device-management-azure-portal/63.png)

<span data-ttu-id="05a56-198">Du kan anpassa hello listvyn genom att klicka på **kolumner** i hello-verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="05a56-198">You can customize hello list view by clicking **Columns** in hello toolbar.</span></span>
 
![Granskningsloggar](./media/device-management-azure-portal/64.png)


<span data-ttu-id="05a56-200">toonarrow ned hello rapporterade data tooa nivå som fungerar för dig, kan du filtrera hello granskningsdata med hello följande fält:</span><span class="sxs-lookup"><span data-stu-id="05a56-200">toonarrow down hello reported data tooa level that works for you, you can filter hello audit data using hello following fields:</span></span>

- <span data-ttu-id="05a56-201">Catergory</span><span class="sxs-lookup"><span data-stu-id="05a56-201">Catergory</span></span>
- <span data-ttu-id="05a56-202">Resurstyp för aktivitet</span><span class="sxs-lookup"><span data-stu-id="05a56-202">Activity resource type</span></span>
- <span data-ttu-id="05a56-203">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="05a56-203">Activity</span></span>
- <span data-ttu-id="05a56-204">Datumintervall</span><span class="sxs-lookup"><span data-stu-id="05a56-204">Date range</span></span>
- <span data-ttu-id="05a56-205">mål</span><span class="sxs-lookup"><span data-stu-id="05a56-205">Target</span></span>
- <span data-ttu-id="05a56-206">Initierad av (aktören)</span><span class="sxs-lookup"><span data-stu-id="05a56-206">Initiated By (Actor)</span></span>

<span data-ttu-id="05a56-207">Dessutom toohello filter, du kan söka efter specifika poster.</span><span class="sxs-lookup"><span data-stu-id="05a56-207">In addition toohello filters, you can search for specific entries.</span></span>

![Granskningsloggar](./media/device-management-azure-portal/65.png)

## <a name="next-steps"></a><span data-ttu-id="05a56-209">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="05a56-209">Next steps</span></span>

* [<span data-ttu-id="05a56-210">Introduktion toodevice management i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="05a56-210">Introduction toodevice management in Azure Active Directory</span></span>](device-management-introduction.md)



