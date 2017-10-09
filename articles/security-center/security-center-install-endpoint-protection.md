---
title: aaaInstall Endpoint Protection i Azure Security Center | Microsoft Docs
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendation ** installera Endpoint Protection **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 1599ad5f-d810-421d-aafc-892e831b403f
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: fea891810e042e4d4f6e55094c0cd4de013ba68a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-endpoint-protection-in-azure-security-center"></a>Installera Endpoint Protection i Azure Security Center
Azure Security Center rekommenderar att du installerar endpoint protection på din virtuella Azure-datorer (VM) om endpoint protection inte är aktiverad. Den här rekommendationen gäller tooWindows virtuella datorer.

> [!NOTE]
> Den här exempeldistributionen använder Microsoft Antimalware. Se [Partner integrering i Azure Security Center](security-center-partner-integration.md#partners-that-integrate-with-security-center) för en lista med partners som är integrerad med Security Center.  
>
>

## <a name="implement-hello-recommendation"></a>Implementera hello rekommendation

> [!NOTE]
> Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.  Det här dokumentet är inte en stegvis guide.
>
>

1. I hello **rekommendationer** bladet väljer **installera Endpoint Protection**.
   ![Välj installera Endpoint Protection][1]
2. Hej **installera Endpoint Protection** öppnas ett blad med en lista över virtuella datorer utan endpoint protection. Välj från hello listan hello virtuella datorer som du vill att tooinstall endpoint protection på och klicka på **installera på virtuella datorer**.
   ![Välj virtuella datorer tooinstall Endpoint Protection på][2]
3. Hej **Välj Endpoint Protection** blad öppnas tooallow du tooselect hello slutpunktsskydd du vill toouse. I det här exemplet ska vi väljer **Microsoft Antimalware**.
   ![Välj Endpoint Protection][3]
4. Mer information om hello slutpunktsskydd visas. Välj **Skapa**.
   ![Skapa program mot skadlig kod][4]
5. Ange konfigurationsinställningar för hello krävs på hello **lägga till tillägget** bladet och väljer sedan **OK**. toolearn mer om hello konfigurationsinställningar, se [standard och anpassad konfiguration för program mot skadlig kod](../security/azure-security-antimalware.md#default-and-custom-antimalware-configuration).

[Microsoft Antimalware](../security/azure-security-antimalware.md) nu aktiva vid hello valda virtuella datorer.

## <a name="see-also"></a>Se även
Den här artikeln visar hur hello tooimplement Security Center rekommendation ”installera Endpoint Protection”. toolearn mer om hur du aktiverar Microsoft Antimalware i Azure, se hello följande dokument:

* [Microsoft Antimalware för molntjänster och virtuella datorer](../security/azure-security-antimalware.md) – Lär dig hur toodeploy Microsoft Antimalware.

toolearn mer om Security Center finns hello följande dokument:

* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper.
* [Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.
* [Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --hittar du blogginlägg om säkerhet och Azure kompatibilitet.

<!--Image references-->
[1]:./media/security-center-install-endpoint-protection/select-install-endpoint-protection.png
[2]:./media/security-center-install-endpoint-protection/install-endpoint-protection-blade.png
[3]:./media/security-center-install-endpoint-protection/select-endpoint-protection.png
[4]:./media/security-center-install-endpoint-protection/create-antimalware-solution.png
