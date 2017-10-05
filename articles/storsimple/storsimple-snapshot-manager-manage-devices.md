---
title: Hantera enheter med StorSimple Snapshot Manager | Microsoft Docs
description: "Beskriver hur du använder StorSimple Snapshot Manager MMC-snapin-modulen för att ansluta och hantera StorSimple-enheter."
services: storsimple
documentationcenter: 
author: SharS
manager: timlt
editor: 
ms.assetid: 966ecbe3-a7fa-4752-825f-6694dd949946
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: f5e3186a4271e0be781f367fa75ada195c58c960
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-storsimple-snapshot-manager-to-connect-and-manage-storsimple-devices"></a><span data-ttu-id="9f62f-103">Använd StorSimple Snapshot Manager för att ansluta och hantera StorSimple-enheter</span><span class="sxs-lookup"><span data-stu-id="9f62f-103">Use StorSimple Snapshot Manager to connect and manage StorSimple devices</span></span>
## <a name="overview"></a><span data-ttu-id="9f62f-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="9f62f-104">Overview</span></span>
<span data-ttu-id="9f62f-105">Du kan använda noder i StorSimple Snapshot Manager **omfång** fönstret för att verifiera importerade data för StorSimple-enheten och uppdatera anslutna lagringsenheter.</span><span class="sxs-lookup"><span data-stu-id="9f62f-105">You can use nodes in the StorSimple Snapshot Manager **Scope** pane to verify imported StorSimple device data and refresh connected storage devices.</span></span> <span data-ttu-id="9f62f-106">Dessutom, när du klickar på den **enheter** nod, kan du visa en lista över anslutna enheter och motsvarande statusinformation i den **resultat** fönstret.</span><span class="sxs-lookup"><span data-stu-id="9f62f-106">Additionally, when you click the **Devices** node, you can see a list of connected devices and corresponding status information in the **Results** pane.</span></span>

![Anslutna enheter](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_connect_devices.png)

<span data-ttu-id="9f62f-108">**Bild 1: StorSimple Snapshot Manager ansluten enhet**</span><span class="sxs-lookup"><span data-stu-id="9f62f-108">**Figure 1: StorSimple Snapshot Manager connected device**</span></span> 

<span data-ttu-id="9f62f-109">Beroende på din **visa** val av **resultat** visar följande information om varje enhet.</span><span class="sxs-lookup"><span data-stu-id="9f62f-109">Depending on your **View** selections, the **Results** pane shows the following information about each device.</span></span> <span data-ttu-id="9f62f-110">(Mer information om hur du konfigurerar en vy, gå till [Visa-menyn](storsimple-use-snapshot-manager.md#view-menu).</span><span class="sxs-lookup"><span data-stu-id="9f62f-110">(For more information about configuring a view, go to [View menu](storsimple-use-snapshot-manager.md#view-menu).</span></span>

| <span data-ttu-id="9f62f-111">Resultaten kolumn</span><span class="sxs-lookup"><span data-stu-id="9f62f-111">Results column</span></span> | <span data-ttu-id="9f62f-112">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9f62f-112">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="9f62f-113">Namn</span><span class="sxs-lookup"><span data-stu-id="9f62f-113">Name</span></span> |<span data-ttu-id="9f62f-114">Namnet på enheten som konfigurerats i den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9f62f-114">The name of the device as configured in the Azure classic portal</span></span> |
| <span data-ttu-id="9f62f-115">Modellen</span><span class="sxs-lookup"><span data-stu-id="9f62f-115">Model</span></span> |<span data-ttu-id="9f62f-116">Modellnumret för enheten</span><span class="sxs-lookup"><span data-stu-id="9f62f-116">The model number of the device</span></span> |
| <span data-ttu-id="9f62f-117">Version</span><span class="sxs-lookup"><span data-stu-id="9f62f-117">Version</span></span> |<span data-ttu-id="9f62f-118">Versionen av programvaran på enheten</span><span class="sxs-lookup"><span data-stu-id="9f62f-118">The version of the software installed on the device</span></span> |
| <span data-ttu-id="9f62f-119">Status</span><span class="sxs-lookup"><span data-stu-id="9f62f-119">Status</span></span> |<span data-ttu-id="9f62f-120">Om enheten är tillgänglig</span><span class="sxs-lookup"><span data-stu-id="9f62f-120">Whether the device is available</span></span> |
| <span data-ttu-id="9f62f-121">Synkroniserades senast</span><span class="sxs-lookup"><span data-stu-id="9f62f-121">Last Synced</span></span> |<span data-ttu-id="9f62f-122">Datum och tid när enheten senast synkroniserades</span><span class="sxs-lookup"><span data-stu-id="9f62f-122">Date and time when the device was last synchronized</span></span> |
| <span data-ttu-id="9f62f-123">Serienr</span><span class="sxs-lookup"><span data-stu-id="9f62f-123">Serial No.</span></span> |<span data-ttu-id="9f62f-124">Serienumret för enheten</span><span class="sxs-lookup"><span data-stu-id="9f62f-124">The serial number for the device</span></span> |

<span data-ttu-id="9f62f-125">Om du högerklickar på den **enheter** nod i den **omfång** rutan som du kan välja mellan följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="9f62f-125">If you right-click the **Devices** node in the **Scope** pane, you can select from the following actions:</span></span>

* <span data-ttu-id="9f62f-126">Lägga till eller ersätta en enhet</span><span class="sxs-lookup"><span data-stu-id="9f62f-126">Add or replace a device</span></span>
* <span data-ttu-id="9f62f-127">Ansluter en enhet och kontrollera import</span><span class="sxs-lookup"><span data-stu-id="9f62f-127">Connect a device and verify imports</span></span>
* <span data-ttu-id="9f62f-128">Uppdatera anslutna enheter</span><span class="sxs-lookup"><span data-stu-id="9f62f-128">Refresh connected devices</span></span>

<span data-ttu-id="9f62f-129">Om du klickar på den **enheter** nod och högerklicka sedan på en enhet namn i den **resultat** rutan som du kan välja mellan följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="9f62f-129">If you click the **Devices** node and then right-click a device name in the **Results** pane, you can select from the following actions:</span></span>

* <span data-ttu-id="9f62f-130">Autentisera en enhet</span><span class="sxs-lookup"><span data-stu-id="9f62f-130">Authenticate a device</span></span>
* <span data-ttu-id="9f62f-131">Visa information om enhet</span><span class="sxs-lookup"><span data-stu-id="9f62f-131">View device details</span></span>
* <span data-ttu-id="9f62f-132">Uppdatera en enhet</span><span class="sxs-lookup"><span data-stu-id="9f62f-132">Refresh a device</span></span>
* <span data-ttu-id="9f62f-133">Ta bort en enhetskonfiguration</span><span class="sxs-lookup"><span data-stu-id="9f62f-133">Delete a device configuration</span></span>
* <span data-ttu-id="9f62f-134">Ändra lösenordet för enheten</span><span class="sxs-lookup"><span data-stu-id="9f62f-134">Change a device password</span></span>

> [!NOTE]
> <span data-ttu-id="9f62f-135">Alla dessa åtgärder är också tillgängliga i den **åtgärder** fönstret.</span><span class="sxs-lookup"><span data-stu-id="9f62f-135">All of these actions are also available in the **Actions** pane.</span></span>


<span data-ttu-id="9f62f-136">Den här självstudiekursen beskrivs hur du använder StorSimple Snapshot Manager för att ansluta och hantera enheter och utföra följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="9f62f-136">This tutorial explains how to use StorSimple Snapshot Manager to connect and manage devices and perform the following tasks:</span></span>

* <span data-ttu-id="9f62f-137">Lägga till eller ersätta en enhet</span><span class="sxs-lookup"><span data-stu-id="9f62f-137">Add or replace a device</span></span>
* <span data-ttu-id="9f62f-138">Ansluter en enhet och kontrollera import</span><span class="sxs-lookup"><span data-stu-id="9f62f-138">Connect a device and verify imports</span></span>
* <span data-ttu-id="9f62f-139">Uppdatera anslutna enheter</span><span class="sxs-lookup"><span data-stu-id="9f62f-139">Refresh connected devices</span></span>
* <span data-ttu-id="9f62f-140">Autentisera en enhet</span><span class="sxs-lookup"><span data-stu-id="9f62f-140">Authenticate a device</span></span>
* <span data-ttu-id="9f62f-141">Visa information om enhet</span><span class="sxs-lookup"><span data-stu-id="9f62f-141">View device details</span></span>
* <span data-ttu-id="9f62f-142">Uppdatera en enskild enhet</span><span class="sxs-lookup"><span data-stu-id="9f62f-142">Refresh an individual device</span></span>
* <span data-ttu-id="9f62f-143">Ta bort en enhetskonfiguration</span><span class="sxs-lookup"><span data-stu-id="9f62f-143">Delete a device configuration</span></span>
* <span data-ttu-id="9f62f-144">Ändra ett enhetslösenord har upphört att gälla</span><span class="sxs-lookup"><span data-stu-id="9f62f-144">Change an expired device password</span></span>
* <span data-ttu-id="9f62f-145">Ersätta en misslyckad enhet</span><span class="sxs-lookup"><span data-stu-id="9f62f-145">Replace a failed device</span></span>

> [!NOTE]
> <span data-ttu-id="9f62f-146">Allmän information om StorSimple Snapshot Manager-gränssnittet går du till [StorSimple Snapshot Manager användargränssnittet](storsimple-use-snapshot-manager.md).</span><span class="sxs-lookup"><span data-stu-id="9f62f-146">For general information about using the StorSimple Snapshot Manager interface, go to [StorSimple Snapshot Manager user interface](storsimple-use-snapshot-manager.md).</span></span>


## <a name="add-or-replace-a-device"></a><span data-ttu-id="9f62f-147">Lägga till eller ersätta en enhet</span><span class="sxs-lookup"><span data-stu-id="9f62f-147">Add or replace a device</span></span>
<span data-ttu-id="9f62f-148">Använd följande procedur för att lägga till eller ersätta en StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="9f62f-148">Use the following procedure to add or replace a StorSimple device.</span></span>

#### <a name="to-add-or-replace-a-device"></a><span data-ttu-id="9f62f-149">Lägga till eller ersätta en enhet</span><span class="sxs-lookup"><span data-stu-id="9f62f-149">To add or replace a device</span></span>
1. <span data-ttu-id="9f62f-150">Klicka på ikonen skrivbord för att starta StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="9f62f-150">Click the desktop icon to start StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="9f62f-151">I den **omfång** fönstret högerklickar du på den **enheter** noden och klicka sedan på **konfigurera en enhet**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-151">In the **Scope** pane, right-click the **Devices** node, and then click **Configure a device**.</span></span> <span data-ttu-id="9f62f-152">Den **konfigurera en enhet** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="9f62f-152">The **Configure a Device** dialog box appears.</span></span>
   
    ![Konfigurera en StorSimple-enhet](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_config_device.png) 
3. <span data-ttu-id="9f62f-154">I den **enhet** listrutan väljer du IP-adressen till enheten eller virtuella enheten.</span><span class="sxs-lookup"><span data-stu-id="9f62f-154">In the **Device** drop-down box, select the IP address of the device or virtual device.</span></span> 
4. <span data-ttu-id="9f62f-155">I den **lösenord** text Skriv lösenordet för StorSimple Snapshot Manager som du skapade för enheten i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9f62f-155">In the **Password** text box, type the StorSimple Snapshot Manager password that you created for the device in the Azure classic portal.</span></span> <span data-ttu-id="9f62f-156">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-156">Click **OK**.</span></span> <span data-ttu-id="9f62f-157">StorSimple Snapshot Manager söker efter den enhet som du har identifierats.</span><span class="sxs-lookup"><span data-stu-id="9f62f-157">StorSimple Snapshot Manager searches for the device that you identified.</span></span> 
   
   * <span data-ttu-id="9f62f-158">Om enheten inte är tillgänglig, StorSimple Snapshot Manager lägger till en anslutning.</span><span class="sxs-lookup"><span data-stu-id="9f62f-158">If the device is available, StorSimple Snapshot Manager adds a connection.</span></span>
   * <span data-ttu-id="9f62f-159">Om enheten inte är tillgänglig av någon anledning, returnerar StorSimple Snapshot Manager ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="9f62f-159">If the device is unavailable for any reason, StorSimple Snapshot Manager returns an error message.</span></span> <span data-ttu-id="9f62f-160">Klicka på **OK** Stäng felmeddelandet och klicka sedan på **Avbryt** att stänga den **konfigurera en enhet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9f62f-160">Click **OK** to close the error message, and then click **Cancel** to close the **Configure a Device** dialog box.</span></span>

## <a name="connect-a-device-and-verify-imports"></a><span data-ttu-id="9f62f-161">Ansluter en enhet och kontrollera import</span><span class="sxs-lookup"><span data-stu-id="9f62f-161">Connect a device and verify imports</span></span>
<span data-ttu-id="9f62f-162">Använd följande procedur för att ansluta en StorSimple-enhet och kontrollera att alla befintliga volymen grupper som har associerade säkerhetskopior importeras.</span><span class="sxs-lookup"><span data-stu-id="9f62f-162">Use the following procedure to connect a StorSimple device and verify that any existing volume groups that have associated backups are imported.</span></span>

#### <a name="to-connect-a-device-and-verify-imports"></a><span data-ttu-id="9f62f-163">Ansluter en enhet och kontrollera import</span><span class="sxs-lookup"><span data-stu-id="9f62f-163">To connect a device and verify imports</span></span>
1. <span data-ttu-id="9f62f-164">Ansluter en enhet till StorSimple Snapshot Manager, följ instruktionerna i Lägg till eller ersätta en enhet.</span><span class="sxs-lookup"><span data-stu-id="9f62f-164">To connect a device to StorSimple Snapshot Manager, follow the instructions in Add or replace a device.</span></span> <span data-ttu-id="9f62f-165">När den ansluter till en enhet, svarar StorSimple Snapshot Manager på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="9f62f-165">When it connects to a device, StorSimple Snapshot Manager responds as follows:</span></span>
   
   * <span data-ttu-id="9f62f-166">Om enheten inte är tillgänglig av någon anledning, returnerar StorSimple Snapshot Manager ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="9f62f-166">If the device is unavailable for any reason, StorSimple Snapshot Manager returns an error message.</span></span> 
   
   * <span data-ttu-id="9f62f-167">Om enheten inte är tillgänglig, StorSimple Snapshot Manager lägger till en anslutning.</span><span class="sxs-lookup"><span data-stu-id="9f62f-167">If the device is available, StorSimple Snapshot Manager adds a connection.</span></span> <span data-ttu-id="9f62f-168">När du väljer enheten visas den i den **resultat** rutan och statusfältet anger att enheten är **tillgänglig**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-168">When you select the device, it appears in the **Results** pane, and the status field indicates that the device is **Available**.</span></span> <span data-ttu-id="9f62f-169">StorSimple Snapshot Manager importerar alla volym grupper som konfigurerats för enheten, förutsatt att grupperna volymen har associerade säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="9f62f-169">StorSimple Snapshot Manager imports any volume groups configured for the device, provided that the volume groups have associated backups.</span></span> <span data-ttu-id="9f62f-170">Principer för säkerhetskopiering har inte importerats.</span><span class="sxs-lookup"><span data-stu-id="9f62f-170">Backup policies are not imported.</span></span> <span data-ttu-id="9f62f-171">Volymen grupper som inte har tillhörande säkerhetskopior har inte importerats.</span><span class="sxs-lookup"><span data-stu-id="9f62f-171">Volume groups that do not have associated backups are not imported.</span></span>
2. <span data-ttu-id="9f62f-172">Klicka på ikonen skrivbord för att starta StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="9f62f-172">Click the desktop icon to start StorSimple Snapshot Manager.</span></span>
3. <span data-ttu-id="9f62f-173">Högerklicka på den översta noden i den **omfång** rutan och klicka sedan på **växla import visa**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-173">Right-click the top node in the **Scope** pane, and then click **Toggle Imports Display**.</span></span>
   
    ![Välj Växla import visning](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Toggle_Imports_Display.png) 
4. <span data-ttu-id="9f62f-175">Den **växla import visa** dialogruta, visar status för den importerade volymen grupper och säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="9f62f-175">The **Toggle Imports Display** dialog box appears, showing the status of the imported volume groups and backups.</span></span> <span data-ttu-id="9f62f-176">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-176">Click **OK**.</span></span>

<span data-ttu-id="9f62f-177">När grupperna för volymen och säkerhetskopieringar har importerats kan använda du StorSimple Snapshot Manager kan hantera dem, precis som du skulle hantera grupper för volymen och säkerhetskopieringar som du skapat och konfigurerat med StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="9f62f-177">After the volume groups and backups are successfully imported, you can use StorSimple Snapshot Manager to manage them, just as you would manage volume groups and backups that you created and configured with StorSimple Snapshot Manager.</span></span> 

## <a name="refresh-connected-devices"></a><span data-ttu-id="9f62f-178">Uppdatera anslutna enheter</span><span class="sxs-lookup"><span data-stu-id="9f62f-178">Refresh connected devices</span></span>
<span data-ttu-id="9f62f-179">Använd följande procedur om du vill synkronisera anslutna StorSimple-enheter med StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="9f62f-179">Use the following procedure to synchronize the connected StorSimple devices with StorSimple Snapshot Manager.</span></span>

#### <a name="to-refresh-connected-devices"></a><span data-ttu-id="9f62f-180">Uppdatera anslutna enheter</span><span class="sxs-lookup"><span data-stu-id="9f62f-180">To refresh connected devices</span></span>
1. <span data-ttu-id="9f62f-181">Klicka på ikonen skrivbord för att starta StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="9f62f-181">Click the desktop icon to start StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="9f62f-182">I den **Scope** fönstret, högerklicka på **enheter**, och klicka sedan på **uppdatera enheter**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-182">In the **Scope** pane, right-click **Devices**, and then click **Refresh Devices**.</span></span> <span data-ttu-id="9f62f-183">Detta synkroniserar anslutna enheter med StorSimple Snapshot Manager så att du kan visa grupper volymen och säkerhetskopieringar, inklusive eventuella nya tillägg.</span><span class="sxs-lookup"><span data-stu-id="9f62f-183">This synchronizes the connected devices with StorSimple Snapshot Manager so that you can view the volume groups and backups, including any recent additions.</span></span> 
   
    ![Uppdatera StorSimple-enheter](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Refresh_devices.png)

<span data-ttu-id="9f62f-185">Den **uppdatera enheter** åtgärd hämtar alla nya grupper i volymen och alla tillhörande säkerhetskopior från anslutna enheter.</span><span class="sxs-lookup"><span data-stu-id="9f62f-185">The **Refresh Devices** action retrieves any new volume groups and any associated backups from connected devices.</span></span> <span data-ttu-id="9f62f-186">Till skillnad från den **skanna volymer** åtgärd som är tillgängliga för den **volymer** noden **uppdatera enheter** inte återställa registernyckeln för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="9f62f-186">Unlike the **Rescan volumes** action available for the **Volumes** node, **Refresh Devices** does not restore the backup registry.</span></span>

## <a name="authenticate-a-device"></a><span data-ttu-id="9f62f-187">Autentisera en enhet</span><span class="sxs-lookup"><span data-stu-id="9f62f-187">Authenticate a device</span></span>
<span data-ttu-id="9f62f-188">Använd följande procedur för att autentisera en StorSimple-enhet med StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="9f62f-188">Use the following procedure to authenticate a StorSimple device with StorSimple Snapshot Manager.</span></span>

#### <a name="to-authenticate-a-device"></a><span data-ttu-id="9f62f-189">För autentisering av en enhet</span><span class="sxs-lookup"><span data-stu-id="9f62f-189">To authenticate a device</span></span>
1. <span data-ttu-id="9f62f-190">Klicka på ikonen skrivbord för att starta StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="9f62f-190">Click the desktop icon to start StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="9f62f-191">I den **omfång** rutan klickar du på **enheter**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-191">In the **Scope** pane, click **Devices**.</span></span>
3. <span data-ttu-id="9f62f-192">I den **resultat** fönstret, högerklicka på enheten och klicka sedan på **autentisera**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-192">In the **Results** pane, right-click the name of the device, and then click **Authenticate**.</span></span>
4. <span data-ttu-id="9f62f-193">Den **autentisera** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="9f62f-193">The **Authenticate** dialog box appears.</span></span> <span data-ttu-id="9f62f-194">Skriv lösenordet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-194">Type the device password, and then click **OK**.</span></span>
   
    ![Autentisera dialogrutan](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Authenticate.png) 

## <a name="view-device-details"></a><span data-ttu-id="9f62f-196">Visa information om enhet</span><span class="sxs-lookup"><span data-stu-id="9f62f-196">View device details</span></span>
<span data-ttu-id="9f62f-197">Använd följande procedur för att visa information om en StorSimple-enhet och synkronisera enheten med StorSimple Snapshot Manager om det behövs.</span><span class="sxs-lookup"><span data-stu-id="9f62f-197">Use the following procedure to view the details of a StorSimple device and, if necessary, resynchronize the device with StorSimple Snapshot Manager.</span></span>

#### <a name="to-view-and-resynchronize-device-details"></a><span data-ttu-id="9f62f-198">Visa och synkronisera information om enhet</span><span class="sxs-lookup"><span data-stu-id="9f62f-198">To view and resynchronize device details</span></span>
1. <span data-ttu-id="9f62f-199">Klicka på ikonen skrivbord för att starta StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="9f62f-199">Click the desktop icon to start StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="9f62f-200">I den **omfång** rutan klickar du på **enheter**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-200">In the **Scope** pane, click **Devices**.</span></span>
3. <span data-ttu-id="9f62f-201">I den **resultat** fönstret, högerklicka på enheten och klicka sedan på **information**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-201">In the **Results** pane, right-click the name of the device, and then click **Details**.</span></span>

<span data-ttu-id="9f62f-202">4 det **enhetsinformation** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="9f62f-202">4.The **Device Details** dialog box appears.</span></span> <span data-ttu-id="9f62f-203">Den här rutan visas namn, modell, version, serienummer, status, iSCSI-målet kvalificerade namn (IQN), och senaste Synkroniseringsdatum och tid.</span><span class="sxs-lookup"><span data-stu-id="9f62f-203">This box shows the name, model, version, serial number, status, target iSCSI Qualified Name (IQN), and last synchronization date and time.</span></span>

* <span data-ttu-id="9f62f-204">Klicka på **omsynkronisering** att synkronisera enheten.</span><span class="sxs-lookup"><span data-stu-id="9f62f-204">Click **Resync** to synchronize the device.</span></span>
* <span data-ttu-id="9f62f-205">Klicka på **OK** eller **Avbryt** att stänga dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9f62f-205">Click **OK** or **Cancel** to close the dialog box.</span></span>
  
  ![Information om enhet](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Device_details.png) 

## <a name="refresh-an-individual-device"></a><span data-ttu-id="9f62f-207">Uppdatera en enskild enhet</span><span class="sxs-lookup"><span data-stu-id="9f62f-207">Refresh an individual device</span></span>
<span data-ttu-id="9f62f-208">Använd följande procedur för att synkronisera om en enskild StorSimple-enhet med StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="9f62f-208">Use the following procedure to resynchronize an individual StorSimple device with StorSimple Snapshot Manager.</span></span>

#### <a name="to-refresh-a-device"></a><span data-ttu-id="9f62f-209">Så här uppdaterar du en enhet</span><span class="sxs-lookup"><span data-stu-id="9f62f-209">To refresh a device</span></span>
1. <span data-ttu-id="9f62f-210">Klicka på ikonen skrivbord för att starta StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="9f62f-210">Click the desktop icon to start StorSimple Snapshot Manager.</span></span> 
2. <span data-ttu-id="9f62f-211">I den **omfång** rutan klickar du på **enheter**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-211">In the **Scope** pane, click **Devices**.</span></span> 
3. <span data-ttu-id="9f62f-212">I den **resultat** fönstret, högerklicka på enheten och klicka sedan på **uppdatera enheten**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-212">In the **Results** pane, right-click the name of the device, and then click **Refresh Device**.</span></span> <span data-ttu-id="9f62f-213">Detta synkroniserar enheten med StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="9f62f-213">This synchronizes the device with StorSimple Snapshot Manager.</span></span>

## <a name="delete-a-device-configuration"></a><span data-ttu-id="9f62f-214">Ta bort en enhetskonfiguration</span><span class="sxs-lookup"><span data-stu-id="9f62f-214">Delete a device configuration</span></span>
<span data-ttu-id="9f62f-215">Använd följande procedur för att ta bort en konfiguration för enskilda StorSimple-enheter från StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="9f62f-215">Use the following procedure to delete an individual StorSimple device configuration from StorSimple Snapshot Manager.</span></span>

#### <a name="to-delete-a-device-configuration"></a><span data-ttu-id="9f62f-216">Ta bort en enhetskonfiguration</span><span class="sxs-lookup"><span data-stu-id="9f62f-216">To delete a device configuration</span></span>
1. <span data-ttu-id="9f62f-217">Klicka på ikonen skrivbord för att starta StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="9f62f-217">Click the desktop icon to start StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="9f62f-218">I den **omfång** rutan klickar du på **enheter**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-218">In the **Scope** pane, click **Devices**.</span></span> 
3. <span data-ttu-id="9f62f-219">I den **resultat** fönstret, högerklicka på enheten och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-219">In the **Results** pane, right-click the name of the device, and then click **Delete**.</span></span> 
4. <span data-ttu-id="9f62f-220">Följande meddelande visas.</span><span class="sxs-lookup"><span data-stu-id="9f62f-220">The following message appears.</span></span> <span data-ttu-id="9f62f-221">Klicka på **Ja** att ta bort konfigurationen eller klicka på **nr** du avbryta borttagningen.</span><span class="sxs-lookup"><span data-stu-id="9f62f-221">Click **Yes** to delete the configuration or click **No** to cancel the deletion.</span></span>
   
    ![Ta bort konfiguration för enheter](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_DeleteDevice.png)

## <a name="change-an-expired-device-password"></a><span data-ttu-id="9f62f-223">Ändra ett enhetslösenord har upphört att gälla</span><span class="sxs-lookup"><span data-stu-id="9f62f-223">Change an expired device password</span></span>
<span data-ttu-id="9f62f-224">Du måste ange ett lösenord för att autentisera en StorSimple-enhet med StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="9f62f-224">You must enter a password to authenticate a StorSimple device with StorSimple Snapshot Manager.</span></span> <span data-ttu-id="9f62f-225">Du kan konfigurera det här lösenordet när du använder Windows PowerShell-gränssnittet för att ställa in enheten.</span><span class="sxs-lookup"><span data-stu-id="9f62f-225">You configure this password when you use the Windows PowerShell interface to set up the device.</span></span> <span data-ttu-id="9f62f-226">Lösenordet kan löpa ut.</span><span class="sxs-lookup"><span data-stu-id="9f62f-226">However, the password can expire.</span></span> <span data-ttu-id="9f62f-227">Om det händer kan använda du den klassiska Azure-portalen för att ändra lösenordet.</span><span class="sxs-lookup"><span data-stu-id="9f62f-227">If this happens, you can use the Azure classic portal to change the password.</span></span> <span data-ttu-id="9f62f-228">Eftersom enheten har konfigurerats i StorSimple Snapshot Manager innan lösenordet upphör att gälla, måste du sedan nytt autentisera enhet i StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="9f62f-228">Then, because the device was configured in StorSimple Snapshot Manager before the password expired, you must re-authenticate the device in StorSimple Snapshot Manager.</span></span>

#### <a name="to-change-the-expired-password"></a><span data-ttu-id="9f62f-229">Ändra lösenordet har upphört att gälla</span><span class="sxs-lookup"><span data-stu-id="9f62f-229">To change the expired password</span></span>
1. <span data-ttu-id="9f62f-230">Starta StorSimple Manager-tjänsten i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9f62f-230">In the Azure classic portal, start the StorSimple Manager service.</span></span>
2. <span data-ttu-id="9f62f-231">Klicka på **enheter** > **konfigurera** för enheten.</span><span class="sxs-lookup"><span data-stu-id="9f62f-231">Click **Devices** > **Configure** for the device.</span></span>
3. <span data-ttu-id="9f62f-232">Rulla ned till avsnittet StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="9f62f-232">Scroll down to the StorSimple Snapshot Manager section.</span></span> <span data-ttu-id="9f62f-233">Ange ett lösenord som är 14 och 15 tecken.</span><span class="sxs-lookup"><span data-stu-id="9f62f-233">Enter a password that is 14-15 characters.</span></span> <span data-ttu-id="9f62f-234">Kontrollera att lösenordet innehåller en blandning av versaler, gemener, siffror och särskilda tecken.</span><span class="sxs-lookup"><span data-stu-id="9f62f-234">Make sure that the password contains a mix of uppercase, lowercase, numeric, and special characters.</span></span>
4. <span data-ttu-id="9f62f-235">Ange lösenordet för att bekräfta det. igen.</span><span class="sxs-lookup"><span data-stu-id="9f62f-235">Re-enter the password to confirm it.</span></span>
5. <span data-ttu-id="9f62f-236">Klicka på **Spara** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="9f62f-236">Click **Save** at the bottom of the page.</span></span>

#### <a name="to-re-authenticate-the-device"></a><span data-ttu-id="9f62f-237">Återautentisera på enheten</span><span class="sxs-lookup"><span data-stu-id="9f62f-237">To re-authenticate the device</span></span>
1. <span data-ttu-id="9f62f-238">Starta StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="9f62f-238">Start StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="9f62f-239">I den **omfång** rutan klickar du på **enheter**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-239">In the **Scope** pane, click **Devices**.</span></span> <span data-ttu-id="9f62f-240">En lista över konfigurerade enheter visas i den **resultat** fönstret.</span><span class="sxs-lookup"><span data-stu-id="9f62f-240">A list of configured devices appears in the **Results** pane.</span></span>
3. <span data-ttu-id="9f62f-241">Välj enhet, högerklicka och klicka sedan på **autentisera**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-241">Select the device, right-click, and then click **Authenticate**.</span></span>
4. <span data-ttu-id="9f62f-242">I den **autentisera** fönster, ange det nya lösenordet.</span><span class="sxs-lookup"><span data-stu-id="9f62f-242">In the **Authenticate** window, enter the new password.</span></span>
5. <span data-ttu-id="9f62f-243">Välj enhet, högerklicka och välj **uppdatera enheten**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-243">Select the device, right-click, and select **Refresh device**.</span></span> <span data-ttu-id="9f62f-244">Detta synkroniserar enheten med StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="9f62f-244">This synchronizes the device with StorSimple Snapshot Manager.</span></span>

## <a name="replace-a-failed-device"></a><span data-ttu-id="9f62f-245">Ersätta en misslyckad enhet</span><span class="sxs-lookup"><span data-stu-id="9f62f-245">Replace a failed device</span></span>
<span data-ttu-id="9f62f-246">Om en StorSimple-enhet misslyckas och ersätts med en enhet i vänteläge (failover), Använd följande steg för att ansluta till den nya enheten och visa tillhörande säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="9f62f-246">If a StorSimple device fails and is replaced by a standby (failover) device, use the following steps to connect to the new device and view the associated backups.</span></span>

#### <a name="to-connect-to-a-new-device-after-failover"></a><span data-ttu-id="9f62f-247">Ansluta till en ny enhet efter växling vid fel</span><span class="sxs-lookup"><span data-stu-id="9f62f-247">To connect to a new device after failover</span></span>
1. <span data-ttu-id="9f62f-248">Konfigurera iSCSI-anslutning till den nya enheten.</span><span class="sxs-lookup"><span data-stu-id="9f62f-248">Reconfigure the iSCSI connection to the new device.</span></span> <span data-ttu-id="9f62f-249">Anvisningar, finns i ”steg 7: montera, initiera och formatera en volym” i [distribuera din lokala StorSimple-enhet](storsimple-8000-deployment-walkthrough-u2.md).</span><span class="sxs-lookup"><span data-stu-id="9f62f-249">For instructions, go to "Step 7: Mount, initialize, and format a volume" in [Deploy your on-premises StorSimple device](storsimple-8000-deployment-walkthrough-u2.md).</span></span>

> [!NOTE]
> <span data-ttu-id="9f62f-250">Om den nya StorSimple-enheten har samma IP-adress som den gamla servern, kanske du kan ansluta den gamla konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="9f62f-250">If the new StorSimple device has the same IP address as the old one, you might be able to connect the old configuration.</span></span>


1. <span data-ttu-id="9f62f-251">Stoppa tjänsten Microsoft StorSimple Management:</span><span class="sxs-lookup"><span data-stu-id="9f62f-251">Stop the Microsoft StorSimple Management Service:</span></span>
   
   1. <span data-ttu-id="9f62f-252">Starta Serverhanteraren.</span><span class="sxs-lookup"><span data-stu-id="9f62f-252">Start Server Manager.</span></span>
   2. <span data-ttu-id="9f62f-253">På instrumentpanelen i Serverhanteraren på den **verktyg** väljer du **Services**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-253">On the Server Manager Dashboard, on the **Tools** menu, select **Services**.</span></span>
   3. <span data-ttu-id="9f62f-254">På den **Services** väljer den **Management-tjänsten för Microsoft StorSimple**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-254">On the **Services** window, select the **Microsoft StorSimple Management Service**.</span></span>
   4. <span data-ttu-id="9f62f-255">I den högra rutan under **Management-tjänsten för Microsoft StorSimple**, klickar du på **stoppa tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-255">In the right pane, under **Microsoft StorSimple Management Service**, click **Stop the service**.</span></span>
2. <span data-ttu-id="9f62f-256">Ta bort konfigurationsinformation som är relaterade till den gamla enheten:</span><span class="sxs-lookup"><span data-stu-id="9f62f-256">Remove the configuration information related to the old device:</span></span>
   
   1. <span data-ttu-id="9f62f-257">Bläddra till C:\ProgramData\Microsoft\StorSimple\BACatalog i Utforskaren.</span><span class="sxs-lookup"><span data-stu-id="9f62f-257">In File Explorer, browse to C:\ProgramData\Microsoft\StorSimple\BACatalog.</span></span>
   2. <span data-ttu-id="9f62f-258">Ta bort filer i mappen BACatalog.</span><span class="sxs-lookup"><span data-stu-id="9f62f-258">Delete the files in the BACatalog folder.</span></span>
3. <span data-ttu-id="9f62f-259">Starta om tjänsten Microsoft StorSimple Management:</span><span class="sxs-lookup"><span data-stu-id="9f62f-259">Restart the Microsoft StorSimple Management Service:</span></span>
   
   1. <span data-ttu-id="9f62f-260">På instrumentpanelen i Serverhanteraren på den **verktyg** väljer du **Services**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-260">On the Server Manager Dashboard, on the **Tools** menu, select **Services**.</span></span>
   2. <span data-ttu-id="9f62f-261">På den **Services** väljer den **Management-tjänsten för Microsoft StorSimple**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-261">On the **Services** window, select the **Microsoft StorSimple Management Service**.</span></span>
   3. <span data-ttu-id="9f62f-262">I den högra rutan under **Management-tjänsten för Microsoft StorSimple**, klickar du på **starta om tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-262">In the right pane, under **Microsoft StorSimple Management Service**, click **Restart the service**.</span></span>
4. <span data-ttu-id="9f62f-263">Starta StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="9f62f-263">Start StorSimple Snapshot Manager.</span></span>
5. <span data-ttu-id="9f62f-264">Om du vill konfigurera den nya virtuella StorSimple-enheten, följ instruktionerna i steg 2: Anslut en virtuell StorSimple-enhet i [distribuera StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="9f62f-264">To configure the new StorSimple device, complete the steps in Step 2: Connect a StorSimple device in [Deploy StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).</span></span>
6. <span data-ttu-id="9f62f-265">Högerklicka på den översta noden i den **omfång** fönstret (StorSimple Snapshot Manager i exemplet) och klicka sedan på **växla import visa**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-265">Right-click the top-level node in the **Scope** pane (StorSimple Snapshot Manager in the example), and then click **Toggle Imports Display**.</span></span> 
7. <span data-ttu-id="9f62f-266">Ett meddelande visas när den importerade volymen grupper och säkerhetskopieringar visas i StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="9f62f-266">A message appears when the imported volume groups and backups are visible in StorSimple Snapshot Manager.</span></span> <span data-ttu-id="9f62f-267">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9f62f-267">Click **OK**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9f62f-268">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9f62f-268">Next steps</span></span>
* <span data-ttu-id="9f62f-269">Lär dig hur du [använda StorSimple Snapshot Manager för att administrera din StorSimple-lösning](storsimple-snapshot-manager-admin.md).</span><span class="sxs-lookup"><span data-stu-id="9f62f-269">Learn how to [use StorSimple Snapshot Manager to administer your StorSimple solution](storsimple-snapshot-manager-admin.md).</span></span>
* <span data-ttu-id="9f62f-270">Lär dig hur du [använda StorSimple Snapshot Manager för att visa och hantera volymer](storsimple-snapshot-manager-manage-volumes.md).</span><span class="sxs-lookup"><span data-stu-id="9f62f-270">Learn how to [use StorSimple Snapshot Manager to view and manage volumes](storsimple-snapshot-manager-manage-volumes.md).</span></span>

