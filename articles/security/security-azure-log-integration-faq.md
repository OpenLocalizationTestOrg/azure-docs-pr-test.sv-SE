---
title: "aaaAzure loggen Integration vanliga frågor och svar | Microsoft Docs"
description: "Den här artikeln svar på frågor om Azure Log-integrering."
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TerryLanfear
ms.assetid: d06d1ac5-5c3b-49de-800e-4d54b3064c64
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload8: na
ms.date: 08/07/2017
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: e886035c9a180d0cd5fcbe9cc02483782df6dbe4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-log-integration-faq"></a><span data-ttu-id="6a9a9-103">Azure logganalys Integration vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="6a9a9-103">Azure Log Integration FAQ</span></span>
<span data-ttu-id="6a9a9-104">Den här artikeln innehåller svar på vanliga frågor och svar (FAQ) om Azure Log-integrering.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-104">This article answers frequently asked questions (FAQ) about Azure Log Integration.</span></span> 

<span data-ttu-id="6a9a9-105">Azure Log-integrering är en Windows operativsystem som du kan använda toointegrate loggarna från Azure-resurser i dina lokala säkerhet information och händelse (SIEM) hanteringssystem.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-105">Azure Log Integration is a Windows operating system service that you can use toointegrate raw logs from your Azure resources into your on-premises security information and event management (SIEM) systems.</span></span> <span data-ttu-id="6a9a9-106">Den här integreringen ger en enhetlig instrumentpanel för alla dina tillgångar, lokalt eller i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-106">This integration provides a unified dashboard for all your assets, on-premises or in hello cloud.</span></span> <span data-ttu-id="6a9a9-107">Du kan sedan sammanställa, korrelera, analysera och varna för säkerhetshändelser som är kopplade till dina program.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-107">You can then aggregate, correlate, analyze, and alert for security events associated with your applications.</span></span>

## <a name="is-hello-azure-log-integration-software-free"></a><span data-ttu-id="6a9a9-108">Är hello Azure Log-integrering programvara gratis?</span><span class="sxs-lookup"><span data-stu-id="6a9a9-108">Is hello Azure Log Integration software free?</span></span>
<span data-ttu-id="6a9a9-109">Ja.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-109">Yes.</span></span> <span data-ttu-id="6a9a9-110">Det är gratis för hello Azure Log-integrering programvara.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-110">There is no charge for hello Azure Log Integration software.</span></span>

## <a name="where-is-azure-log-integration-available"></a><span data-ttu-id="6a9a9-111">Var finns Azure Log-integrering?</span><span class="sxs-lookup"><span data-stu-id="6a9a9-111">Where is Azure Log Integration available?</span></span>

<span data-ttu-id="6a9a9-112">Det finns för närvarande i kommersiella Azure och Azure Government och är inte tillgänglig i Kina eller Tyskland.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-112">It is currently available in Azure Commercial and Azure Government and is not available in China or Germany.</span></span>

## <a name="how-can-i-see-hello-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs"></a><span data-ttu-id="6a9a9-113">Hur kan jag se hello storage-konton som Azure Log-integrering dra Azure VM loggar?</span><span class="sxs-lookup"><span data-stu-id="6a9a9-113">How can I see hello storage accounts from which Azure Log Integration is pulling Azure VM logs?</span></span>
<span data-ttu-id="6a9a9-114">Kör kommandot hello **azlog källistan**.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-114">Run hello command **azlog source list**.</span></span>

## <a name="how-can-i-tell-which-subscription-hello-azure-log-integration-logs-are-from"></a><span data-ttu-id="6a9a9-115">Hur vet vilken prenumeration hello Azure Log-integrering loggar är från?</span><span class="sxs-lookup"><span data-stu-id="6a9a9-115">How can I tell which subscription hello Azure Log Integration logs are from?</span></span>

<span data-ttu-id="6a9a9-116">Hello gäller granskningsloggar som placeras i hello **AzureResourcemanagerJson** kataloger, hello prenumerations-ID är i hello loggfilens namn.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-116">In hello case of audit logs that are placed in hello **AzureResourcemanagerJson** directories, hello subscription ID is in hello log file name.</span></span> <span data-ttu-id="6a9a9-117">Detta gäller även för loggar i hello **AzureSecurityCenterJson** mapp.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-117">This is also true for logs in hello **AzureSecurityCenterJson** folder.</span></span> <span data-ttu-id="6a9a9-118">Exempel:</span><span class="sxs-lookup"><span data-stu-id="6a9a9-118">For example:</span></span>

<span data-ttu-id="6a9a9-119">20170407T070805_2768037.0000000023. **1111e5ee-1111-111b-a11e-1e111e1111dc**JSON</span><span class="sxs-lookup"><span data-stu-id="6a9a9-119">20170407T070805_2768037.0000000023.**1111e5ee-1111-111b-a11e-1e111e1111dc**.json</span></span>

<span data-ttu-id="6a9a9-120">Azure Active Directory-granskningsloggar innehåller hello klient-ID som en del av hello namn.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-120">Azure Active Directory audit logs include hello tenant ID as part of hello name.</span></span>

<span data-ttu-id="6a9a9-121">Diagnostikloggar som läses från en händelsehubb innehåller inte hello prenumerations-ID som en del av hello namn.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-121">Diagnostic logs that are read from an event hub do not include hello subscription ID as part of hello name.</span></span> <span data-ttu-id="6a9a9-122">I stället inkluderar de hello eget namn som angetts som en del av hello skapandet av hello hubb händelsekälla.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-122">Instead, they include hello friendly name specified as part of hello creation of hello event hub source.</span></span> 

## <a name="how-can-i-update-hello-proxy-configuration"></a><span data-ttu-id="6a9a9-123">Hur kan jag uppdatera hello proxykonfiguration?</span><span class="sxs-lookup"><span data-stu-id="6a9a9-123">How can I update hello proxy configuration?</span></span>
<span data-ttu-id="6a9a9-124">Om proxyserverns inställningar inte tillåter åtkomst till Azure lagring direkt, öppna hello **AZLOG. EXE. CONFIG** filen i **c:\Program Files\Microsoft Azure Log integrering**.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-124">If your proxy setting does not allow Azure storage access directly, open hello **AZLOG.EXE.CONFIG** file in **c:\Program Files\Microsoft Azure Log Integration**.</span></span> <span data-ttu-id="6a9a9-125">Uppdatera hello filen tooinclude hello **defaultProxy** avsnitt med hello proxyadress i din organisation.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-125">Update hello file tooinclude hello **defaultProxy** section with hello proxy address of your organization.</span></span> <span data-ttu-id="6a9a9-126">När hello uppdateringen är klar, stoppa och starta hello tjänsten genom att använda hello kommandon **net stop azlog** och **net start azlog**.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-126">After hello update is done, stop and start hello service by using hello commands **net stop azlog** and **net start azlog**.</span></span>

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.net>
        <connectionManagement>
          <add address="*" maxconnection="400" />
        </connectionManagement>
        <defaultProxy>
          <proxy usesystemdefault="true"
          proxyaddress=http://127.0.0.1:8888
          bypassonlocal="true" />
        </defaultProxy>
      </system.net>
      <system.diagnostics>
        <performanceCounters filemappingsize="20971520" />
      </system.diagnostics>   

## <a name="how-can-i-see-hello-subscription-information-in-windows-events"></a><span data-ttu-id="6a9a9-127">Hur kan jag se hello prenumerationsinformation i Windows-händelser?</span><span class="sxs-lookup"><span data-stu-id="6a9a9-127">How can I see hello subscription information in Windows events?</span></span>
<span data-ttu-id="6a9a9-128">Lägg till hello prenumerations-ID toohello eget namn när du lägger till hello källa:</span><span class="sxs-lookup"><span data-stu-id="6a9a9-128">Append hello subscription ID toohello friendly name while adding hello source:</span></span>

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  
<span data-ttu-id="6a9a9-129">hello händelsen XML har hello efter metadata, inklusive hello prenumerations-ID:</span><span class="sxs-lookup"><span data-stu-id="6a9a9-129">hello event XML has hello following metadata, including hello subscription ID:</span></span>

![Händelsen XML][1]

## <a name="error-messages"></a><span data-ttu-id="6a9a9-131">Felmeddelanden</span><span class="sxs-lookup"><span data-stu-id="6a9a9-131">Error messages</span></span>
### <a name="when-i-run-hello-command-azlog-createazureid-why-do-i-get-hello-following-error"></a><span data-ttu-id="6a9a9-132">När jag kör hello kommandot **azlog createazureid**, varför visas hello följande fel?</span><span class="sxs-lookup"><span data-stu-id="6a9a9-132">When I run hello command **azlog createazureid**, why do I get hello following error?</span></span>
<span data-ttu-id="6a9a9-133">Fel:</span><span class="sxs-lookup"><span data-stu-id="6a9a9-133">Error:</span></span>

  <span data-ttu-id="6a9a9-134">*Kunde inte toocreate AAD-program - klient 72f988bf-86f1-41af-91ab-2d7cd011db37 - orsak = förbjuden - meddelandet = ”privilegier toocomplete hello operation”.*</span><span class="sxs-lookup"><span data-stu-id="6a9a9-134">*Failed toocreate AAD Application - Tenant 72f988bf-86f1-41af-91ab-2d7cd011db37 - Reason = 'Forbidden' - Message = 'Insufficient privileges toocomplete hello operation.'*</span></span>

<span data-ttu-id="6a9a9-135">Hej **azlog createazureid** kommando försöker toocreate ett huvudnamn för tjänsten i alla hello Azure AD-innehavare för hello prenumerationer som hello Azure inloggningen har åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-135">hello **azlog createazureid** command attempts toocreate a service principal in all hello Azure AD tenants for hello subscriptions that hello Azure login has access to.</span></span> <span data-ttu-id="6a9a9-136">Om din Azure-inloggningen är endast gästanvändaren i den Azure AD-klienten, misslyckas hello kommandot med ”privilegier toocomplete hello åtgärden”.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-136">If your Azure login is only a guest user in that Azure AD tenant, hello command fails with "Insufficient privileges toocomplete hello operation."</span></span> <span data-ttu-id="6a9a9-137">Be hello klient admin tooadd ditt konto som en användare i hello-klient.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-137">Ask hello tenant admin tooadd your account as a user in hello tenant.</span></span>

### <a name="when-i-run-hello-command-azlog-authorize-why-do-i-get-hello-following-error"></a><span data-ttu-id="6a9a9-138">När jag kör hello kommandot **azlog auktorisera**, varför visas hello följande fel?</span><span class="sxs-lookup"><span data-stu-id="6a9a9-138">When I run hello command **azlog authorize**, why do I get hello following error?</span></span>
<span data-ttu-id="6a9a9-139">Fel:</span><span class="sxs-lookup"><span data-stu-id="6a9a9-139">Error:</span></span>

  <span data-ttu-id="6a9a9-140">*Varning om att skapa rolltilldelning - AuthorizationFailed: hello klienten janedo@microsoft.com' med objekt id 'fe9e03e4-4dad-4328-910f-fd24a9660bd2' har ingen auktorisering tooperform åtgärd 'Microsoft.Authorization/roleAssignments/write' över område ' / prenumerationer / 70d 95299 d689 4c 97-b971-0d8ff0000000'.*</span><span class="sxs-lookup"><span data-stu-id="6a9a9-140">*Warning creating Role Assignment - AuthorizationFailed: hello client janedo@microsoft.com' with object id 'fe9e03e4-4dad-4328-910f-fd24a9660bd2' does not have authorization tooperform action 'Microsoft.Authorization/roleAssignments/write' over scope '/subscriptions/70d95299-d689-4c97-b971-0d8ff0000000'.*</span></span>

<span data-ttu-id="6a9a9-141">Hej **azlog auktorisera** kommandot tilldelar hello roll för läsare toohello Azure AD tjänstens huvudnamn (skapats med **azlog createazureid**) toohello tillhandahålls prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-141">hello **azlog authorize** command assigns hello role of reader toohello Azure AD service principal (created with **azlog createazureid**) toohello provided subscriptions.</span></span> <span data-ttu-id="6a9a9-142">Om hello Azure-inloggning inte är en medadministratör eller en ägare av hello prenumeration misslyckas med felmeddelandet ”auktorisering misslyckades”.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-142">If hello Azure login is not a co-administrator or an owner of hello subscription, it fails with an "Authorization Failed" error message.</span></span> <span data-ttu-id="6a9a9-143">Azure rollbaserad åtkomstkontroll (RBAC) för en medadministratör eller ägare är nödvändiga toocomplete den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-143">Azure Role-Based Access Control (RBAC) of co-administrator or owner is needed toocomplete this action.</span></span>

## <a name="where-can-i-find-hello-definition-of-hello-properties-in-hello-audit-log"></a><span data-ttu-id="6a9a9-144">Var hittar jag hello definitionen av hello egenskaper i hello granskningsloggen</span><span class="sxs-lookup"><span data-stu-id="6a9a9-144">Where can I find hello definition of hello properties in hello audit log?</span></span>
<span data-ttu-id="6a9a9-145">Se:</span><span class="sxs-lookup"><span data-stu-id="6a9a9-145">See:</span></span>

* [<span data-ttu-id="6a9a9-146">Granskningsåtgärder med Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6a9a9-146">Audit operations with Azure Resource Manager</span></span>](../azure-resource-manager/resource-group-audit.md)
* [<span data-ttu-id="6a9a9-147">Hello management händelser i en prenumeration i hello Azure övervakaren REST API</span><span class="sxs-lookup"><span data-stu-id="6a9a9-147">List hello management events in a subscription in hello Azure Monitor REST API</span></span>](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a><span data-ttu-id="6a9a9-148">Var hittar jag information om Azure Security Center-aviseringar</span><span class="sxs-lookup"><span data-stu-id="6a9a9-148">Where can I find details on Azure Security Center alerts?</span></span>
<span data-ttu-id="6a9a9-149">Se [hantering och svarar toosecurity aviseringar i Azure Security Center](../security-center/security-center-managing-and-responding-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="6a9a9-149">See [Managing and responding toosecurity alerts in Azure Security Center](../security-center/security-center-managing-and-responding-alerts.md).</span></span>

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a><span data-ttu-id="6a9a9-150">Hur kan jag ändra vad samlas in med diagnostik för Virtuella datorer?</span><span class="sxs-lookup"><span data-stu-id="6a9a9-150">How can I modify what is collected with VM diagnostics?</span></span>
<span data-ttu-id="6a9a9-151">Mer information om hur tooget, ändra och ange hello Azure Diagnostics konfiguration finns [Använd PowerShell tooenable Azure-diagnostik i en virtuell dator som kör Windows](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6a9a9-151">For details on how tooget, modify, and set hello Azure Diagnostics configuration, see [Use PowerShell tooenable Azure Diagnostics in a virtual machine running Windows](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="6a9a9-152">hello följande exempel hämtar hello Azure Diagnostics konfiguration:</span><span class="sxs-lookup"><span data-stu-id="6a9a9-152">hello following example gets hello Azure Diagnostics configuration:</span></span>

    -AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

<span data-ttu-id="6a9a9-153">följande exempel hello ändrar hello Azure Diagnostics konfiguration.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-153">hello following example modifies hello Azure Diagnostics configuration.</span></span> <span data-ttu-id="6a9a9-154">I den här konfigurationen endast händelse-ID 4624 och händelse-ID 4625 samlas in från hello säkerhetshändelseloggen.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-154">In this configuration, only event ID 4624 and event ID 4625 are collected from hello security event log.</span></span> <span data-ttu-id="6a9a9-155">Microsoft Antimalware för Azure händelser som samlas in från hello systemets händelselogg.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-155">Microsoft Antimalware for Azure events are collected from hello system event log.</span></span> <span data-ttu-id="6a9a9-156">Mer information om hello användning av XPath-uttryck finns [förbrukar händelser](https://msdn.microsoft.com/library/windows/desktop/dd996910(v=vs.85)).</span><span class="sxs-lookup"><span data-stu-id="6a9a9-156">For details on hello use of XPath expressions, see [Consuming Events](https://msdn.microsoft.com/library/windows/desktop/dd996910(v=vs.85)).</span></span>

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

<span data-ttu-id="6a9a9-157">hello anger följande exempel hello Azure Diagnostics konfiguration:</span><span class="sxs-lookup"><span data-stu-id="6a9a9-157">hello following example sets hello Azure Diagnostics configuration:</span></span>

    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

<span data-ttu-id="6a9a9-158">När du har ändrat Kontrollera hello storage-konto tooensure som hello rätt händelser har samlats in.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-158">After you make changes, check hello storage account tooensure that hello correct events are collected.</span></span>

<span data-ttu-id="6a9a9-159">Om du har problem under hello installation och konfiguration, öppnar du en [supportbegäran](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request).</span><span class="sxs-lookup"><span data-stu-id="6a9a9-159">If you have any issues during hello installation and configuration, please open a [support request](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request).</span></span> <span data-ttu-id="6a9a9-160">Välj **loggen Integration** som hello-tjänst som du begär support.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-160">Select **Log Integration** as hello service for which you are requesting support.</span></span>

## <a name="can-i-use-azure-log-integration-toointegrate-network-watcher-logs-into-my-siem"></a><span data-ttu-id="6a9a9-161">Kan jag använda Azure Log-integrering toointegrate Nätverksbevakaren loggar till min SIEM?</span><span class="sxs-lookup"><span data-stu-id="6a9a9-161">Can I use Azure Log Integration toointegrate Network Watcher logs into my SIEM?</span></span>

<span data-ttu-id="6a9a9-162">Azure Nätverksbevakaren genererar stora mängder loggningsinformation.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-162">Azure Network Watcher generates large quantities of logging information.</span></span> <span data-ttu-id="6a9a9-163">Dessa loggar är inte avsedda toobe skickas tooa SIEM.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-163">These logs are not meant toobe sent tooa SIEM.</span></span> <span data-ttu-id="6a9a9-164">hello stöds bara är för Nätverksbevakaren loggar ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-164">hello only supported destination for Network Watcher logs is a storage account.</span></span> <span data-ttu-id="6a9a9-165">Azure Log-Integration stöder inte läsning av dessa loggar och gör dem tillgängliga tooa SIEM.</span><span class="sxs-lookup"><span data-stu-id="6a9a9-165">Azure Log Integration does not support reading these logs and making them available tooa SIEM.</span></span>

<!--Image references-->
[1]: ./media/security-azure-log-integration-faq/event-xml.png
