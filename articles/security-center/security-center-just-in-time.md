---
title: "aaaJust i tid virtuella datorn åtkomst till i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet vägleder dig igenom hur precis i tid VM åtkomst i Azure Security Center hjälper dig att styra åtkomst till tooyour Azure virtuella datorer."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: terrylan
ms.openlocfilehash: e6b58dd2c686cb227392b294e85914df5a546016
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-virtual-machine-access-using-just-in-time"></a>Hantera virtuella åtkomst med hjälp av precis i tid

Precis i tid virtuell dator (VM) vara åtkomst används toolock ned inkommande trafik tooyour virtuella Azure-datorer att minska exponeringen tooattacks och ger enkel åtkomst tooconnect tooVMs vid behov.

> [!NOTE]
> hello just-in tiden funktionen är i förhandsvisning och finns på hello standardnivån av Security Center.  Se [priser](security-center-pricing.md) toolearn mer om Security Center prisnivåer.
>
>

## <a name="attack-scenario"></a>Angrepp

Brute force-attacker ofta målportar för hantering som ett sätt toogain åtkomst tooa VM. Om det lyckas, kan en angripare ta kontroll över hello VM och upprätta en fot i din miljö.

Enkelriktade tooreduce exponering tooa brute-force attack är toolimit hello tid att öppna en port. Hanteringsportar gör inte behöver toobe öppna hela tiden. De behöver bara toobe öppna medan du är ansluten toohello VM, till exempel tooperform management eller underhållsuppgifter. När precis i tid är aktiverat, Security Center använder [Nätverkssäkerhetsgruppen](../virtual-network/virtual-networks-nsg.md) (NSG) regler som begränsar åtkomst toomanagement portar så att de inte kan riktas av angripare.

![Precis i tid scenario][1]

## <a name="how-does-just-in-time-access-work"></a>Hur just-in-time-åtkomst fungerar?

När precis i tid är aktiverat, låser Security Center ned inkommande trafik tooyour virtuella Azure-datorer genom att skapa en regel för NSG. Du väljer hello portar på hello VM toowhich inkommande trafik ska låsas. De här portarna styrs av hello precis i Tidslösning.

När en användare begär åtkomst tooa VM, Security Center kontrollerar hello användaren har [rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-control-configure.md) behörigheter som ger skrivbehörighet för hello Azure-resurs. Om de har skrivbehörighet, hello begäran har godkänts och Security Center konfigurerar automatiskt hello tooallow Nätverkssäkerhetsgrupper (NSG: er) för inkommande trafik toohello hanteringsportar för hello mängden tid du angett. När hello tid har gått ut, återställer Security Center hello NSG: er tootheir tidigare tillstånd.

> [!NOTE]
> Security Center just-in-time-åtkomst för VM stöder för närvarande endast virtuella datorer som distribueras via Azure Resource Manager. Mer information om hello klassiska och Resource Manager distributionsmodellerna finns toolearn [Azure Resource Manager och klassisk distribution](../azure-resource-manager/resource-manager-deployment-model.md).
>
>

## <a name="using-just-in-time-access"></a>Med hjälp av just-in-time-åtkomst

Hej **precis i tid VM access** panelen på hello **Security Center** bladet visar hello antalet virtuella datorer som konfigurerats för just-in-tid åtkomst och hello antal godkända åtkomstbegäranden som görs i hello föregående vecka.

Välj hello **precis i tid VM access** panelen och hello **precis i tid VM access** blad öppnas.

![Precis i tid VM åtkomst panelen][2]

Hej **precis i tid VM access** bladet innehåller information om hello tillståndet för dina virtuella datorer:

- **Konfigurerad** -virtuella datorer som har konfigurerade toosupport just-in-time VM-åtkomst. hello data presenteras för hello föregående vecka och inkluderar för varje VM hello antal godkända begäranden, datumet och tiden och senast användare.
- **Rekommenderade** -virtuella datorer som stöder just-in-time-åtkomst för VM men inte har konfigurerats att. Vi rekommenderar att du aktiverar precis i tid VM åtkomstkontroll för dessa virtuella datorer. Se [aktivera just-in-time-åtkomst för VM](#enable-just-in-time-vm-access).
- **Ingen rekommendation** -orsaker till att en virtuell dator inte toobe som rekommenderas är:
  - Saknas NSG - hello precis i Tidslösning kräver en NSG toobe på plats.
  - Klassiska VM - Security Center just-in-time-åtkomst för VM stöder för närvarande endast virtuella datorer som distribueras via Azure Resource Manager. Klassisk distribution stöds inte av hello precis i Tidslösning.
  - Andra - en virtuell dator finns i den här kategorin om hello precis i tid lösningen har inaktiverats i hello säkerhetsprincipen för hello prenumeration eller resursgrupp hello eller att hello VM saknar en offentlig IP-adress och inte har en NSG på plats.

## <a name="configuring-a-just-in-time-access-policy"></a>Konfigurera bara i tid åtkomstprincip

tooselect hello virtuella datorer som du vill tooenable:

1. På hello **precis i tid VM access** bladet, Välj hello **rekommenderas** fliken.

  ![Aktivera just-in-time-åtkomst][3]

2. Under **VMs**, Välj hello virtuella datorer som du vill tooenable. Detta placerar en markering nästa tooa VM.
3. Välj **aktivera JIT på virtuella datorer**.
4. Välj **Spara**.

### <a name="default-ports"></a>Standardportar

Du kan se hello standardportarna som Security Center rekommenderar att aktivera precis i tid.

1. På hello **precis i tid VM access** bladet, Välj hello **rekommenderas** fliken.

  ![Visa standardportarna][6]

2. Under **VMs**, Välj en virtuell dator. Detta placerar en markering nästa toohello virtuella datorn och öppnar hello **JIT VM konfiguration** bladet. Det här bladet visar hello standardportarna.

### <a name="add-ports"></a>Lägga till portar

Från hello **JIT VM konfiguration** bladet du kan också lägga till och konfigurera en ny port som du vill använda tooenable hello precis i Tidslösning.

1. På hello **JIT VM konfiguration** bladet väljer **Lägg till**. Då öppnas hello **Lägg till Portkonfiguration** bladet.

  ![Portkonfiguration][7]

2. På **Lägg till Portkonfiguration** bladet du identifiera hello port protokolltyp tillåtna IP-adresser för källa och maximal tid för begäran.

  Tillåtna käll-IP-adresser är hello IP-adressintervall tillåtna tooget åtkomst vid ett godkänt förfrågan.

  Tid för maximal begäran är hello maximala tidsperioden att en specifik port kan öppnas.

3. Välj **OK**.

## <a name="requesting-access-tooa-vm"></a>Begär åtkomst tooa VM

toorequest åtkomst tooa VM:

1. På hello **precis i tid VM access** bladet, Välj hello **konfigurerad** fliken.
2. Under **VMs**, Välj hello virtuella datorer som du vill tooenable åtkomst. Detta placerar en markering nästa tooa VM.
3. Välj **begära åtkomst**. Då öppnas hello **begära åtkomst** bladet.

  ![Begär åtkomst tooa VM][4]

4. På hello **begära åtkomst** bladet som du konfigurerar för varje VM hello portar tooopen tillsammans med hello käll-IP att hello port öppnas tooand hello tidsfönstret för vilka hello porten är öppen. Du kan begära åtkomst endast toohello portar som är konfigurerade i hello precis i tid princip. Varje port har en maximal tillåten tid som härletts från hello precis i tid princip.
5. Välj **öppna portarna**.

## <a name="editing-a-just-in-time-access-policy"></a>Redigera bara i tid åtkomstprincip

Du kan ändra en virtuell dator finns bara i tid princip genom att lägga till och konfigurera en ny port tooopen för den virtuella datorn eller genom att ändra andra parametrar relaterade tooan skyddad redan port.

I ordning tooedit en befintlig bara i en virtuell dator tid princip hello **konfigurerad** används:

1. Under **VMs**, Välj ett VM-tooadd en port tooby att klicka på hello tre punkter inom hello rad för den virtuella datorn. Då öppnas en meny.
2. Välj **redigera** hello-menyn. Då öppnas hello **JIT VM konfiguration** bladet.

  ![Redigera princip][8]

3. På hello **JIT VM konfiguration** bladet du kan antingen redigera hello befintliga inställningar för redan skyddade porten genom att klicka på dess port eller kan du välja **Lägg till**. Då öppnas hello **Lägg till Portkonfiguration** bladet.

  ![Lägga till en port][7]

4. På **Lägg till Portkonfiguration** bladet identifiera hello port, protokolltyp, tillåtna käll-IP-adresser och tid för maximal begäran.
5. Välj **OK**.
6. Välj **Spara**.

## <a name="auditing-just-in-time-access-activity"></a>Granskning precis i tid access-aktivitet

Du kan få insikter om VM aktiviteter loggen sökning. tooview loggar:

1. På hello **precis i tid VM access** bladet, Välj hello **konfigurerad** fliken.
2. Under **VMs**, Välj en VM tooview information om genom att klicka på hello tre punkter inom hello rad för den virtuella datorn. Då öppnas en meny.
3. Välj **aktivitetsloggen** hello-menyn. Då öppnas hello **aktivitetsloggen** bladet.

![Välj aktivitetsloggen][9]

Hej **aktivitetsloggen** bladet innehåller en filtrerad vy av tidigare åtgärder för den virtuella datorn tillsammans med tid och datum prenumeration.

![Visa aktivitetsloggen][5]

Du kan hämta hello logginformation genom att välja **Klicka här toodownload alla hello-objekt som CSV**.

Ändra hello-filter och välj **tillämpa** toocreate Sök- och log.

## <a name="using-just-in-time-vm-access-via-powershell"></a>Med hjälp av precis i tid VM åtkomst via PowerShell

Kontrollera att du har hello i ordning toouse hello precis i Tidslösning via PowerShell, [senaste](/powershell/azure/install-azurerm-ps) Azure PowerShell-version.
När du gör det, måste tooinstall hello [senaste](https://www.powershellgallery.com/packages/Azure-Security-Center/0.0.12) Azure Security Center från hello PowerShell-galleriet.

### <a name="configuring-a-just-in-time-policy-for-a-vm"></a>Konfigurera bara i tid princip för en virtuell dator

tooconfigure bara i tid princip på en specifik VM, behöver du toorun detta kommando i PowerShell-sessionen: Set-ASCJITAccessPolicy.
Följ hello cmdlet-dokumentationen toolearn mer.

### <a name="requesting-access-tooa-vm"></a>Begär åtkomst tooa VM

tooaccess en specifik VM som skyddas med hello precis i tid lösningen måste du toorun detta kommando i PowerShell-sessionen: anropa ASCJITAccess.
Följ hello cmdlet-dokumentationen toolearn mer.

## <a name="next-steps"></a>Nästa steg
I den här artikeln har du lärt dig hur precis i tid VM åtkomst i Security Center hjälper du styr åtkomst tooyour Azure virtuella datorer.

toolearn mer om Security Center finns hello följande:

- [Ställa in säkerhetsprinciper](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.
- [Hantera säkerhetsrekommendationer](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.
- [Övervakning av säkerhetshälsa](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.
- [Hantera och åtgärda toosecurity aviseringar](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.
- [Övervaka partnerlösningar](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.
- [Vanliga frågor om Security Center](security-center-faq.md) – finns vanliga frågor om hur du använder hello-tjänsten.
- [Azures säkerhetsblogg](https://blogs.msdn.microsoft.com/azuresecurity/) – Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure.


<!--Image references-->
[1]: ./media/security-center-just-in-time/just-in-time-scenario.png
[2]: ./media/security-center-just-in-time/just-in-time.png
[3]: ./media/security-center-just-in-time/enable-just-in-time-access.png
[4]: ./media/security-center-just-in-time/request-access-to-a-vm.png
[5]: ./media/security-center-just-in-time/activity-log.png
[6]: ./media/security-center-just-in-time/default-ports.png
[7]: ./media/security-center-just-in-time/add-a-port.png
[8]: ./media/security-center-just-in-time/edit-policy.png
[9]: ./media/security-center-just-in-time/select-activity-log.png
