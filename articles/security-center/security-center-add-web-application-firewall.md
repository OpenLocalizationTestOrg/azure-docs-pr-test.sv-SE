---
title: "aaaAdd en brandvägg för webbaserade program i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendationer ** lägga till en web application firewall ** och ** slutför programmet skydd **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 8f56139a-4466-48ac-90fb-86d002cf8242
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2017
ms.author: terrylan
ms.openlocfilehash: bff0aa2a5c6e0dde23396f93de52abe295053581
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-web-application-firewall-in-azure-security-center"></a>Lägg till en brandvägg för webbaserade program i Azure Security Center
Azure Security Center kan rekommenderar att du lägger till en brandvägg för webbaserade program (Brandvägg) från en Microsoft-partner toosecure webbaserade program. Det här dokumentet vägleder dig genom ett exempel på hur tooapply den här rekommendationen.

En Brandvägg rekommendation visas för alla offentliga Internetriktade IP-adresser (instans nivå IP eller Load belastningsutjämnade IP) som har en nätverkssäkerhetsgrupp med öppna webbplats för inkommande portar (80,443).

Security Center rekommenderar att etablera en Brandvägg toohelp skydd mot angrepp målobjekt för webbaserade program på virtuella datorer samt på Apptjänst-miljö. En App Service miljö (ASE) är en [Premium](https://azure.microsoft.com/pricing/details/app-service/) service plan alternativet för Azure App Service som tillhandahåller en helt isolerad och dedikerad miljö för Azure App Service-program som körs på ett säkert sätt. toolearn mer om ASE, se hello [dokumentationen till App Service-miljö](../app-service/app-service-app-service-environments-readme.md).

> [!NOTE]
> Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.  Det här dokumentet är inte en stegvis guide.
>
>

## <a name="implement-hello-recommendation"></a>Implementera hello rekommendation
1. I hello **rekommendationer** bladet väljer **säkra webbprogram med hjälp av Brandvägg för webbaserade program**.
   ![Skydda webb-program][1]
2. I hello **skydda ditt webbprogram med hjälp av Brandvägg för webbaserade program** bladet Välj ett webbprogram. Hej **lägga till en brandvägg för webbaserade program** blad öppnas.
   ![Lägga till en brandvägg för webbappar][2]
3. Du kan välja toouse en befintlig Brandvägg för webbaserade program om det är tillgängligt eller du kan skapa en ny. I det här exemplet finns det inga befintliga WAFs så skapar vi en Brandvägg.
4. toocreate en Brandvägg, markera en lösning hello listan med integrerade partnerleverantörer. I det här exemplet väljer vi **Barracuda Brandvägg för webbaserade program**.
5. Hej **Barracuda Brandvägg för webbaserade program** öppnas ett blad med information om hello partnerlösning. Välj **skapa** hello information-bladet.

   ![Brandväggen information bladet][3]

6. Hej **ny Brandvägg för webbaserade program** blad öppnas, där du kan utföra **VM-konfiguration** steg och ange **Brandvägg Information**. Välj **VM-konfiguration**.
7. I hello **VM-konfiguration** bladet anger du information som krävs toospin in hello virtuell dator som kör hello Brandvägg.
   ![VM-konfiguration][4]
8. Returnera toohello **ny Brandvägg för webbaserade program** och välj **Brandvägg Information**. I hello **Brandvägg Information** bladet du konfigurerar hello Brandvägg sig själv. Steg 7 kan tooconfigure hello virtuell dator på vilken hello Brandvägg körs och steg 8 kan du tooprovision hello Brandvägg sig själv.

## <a name="finalize-application-protection"></a>Slutför programskydd
1. Returnera toohello **rekommendationer** bladet. En ny post skapades när du har skapat hello Brandvägg, kallas **Slutför programskydd**. Den här posten kan du vet att du måste toocomplete hello processen för att koppla samman hello Brandvägg inom hello Azure Virtual Network så att den kan skydda hello program.

   ![Slutför programskydd][5]

2. Välj **Slutför programskydd**. Ett nytt blad öppnas. Du kan se att det finns ett program som behöver toohave dess trafik dirigeras om.
3. Välj hello webbprogram. Då öppnas ett blad som ger steg för att slutföra installationen av brandväggen hello. Utför hello steg och välj sedan **begränsa trafik**. Security Center sedan hello-kablar upp för dig.

   ![Begränsa trafik][6]

> [!NOTE]
> Du kan skydda flera webbprogram i Security Center genom att lägga till dessa program tooyour befintliga Brandvägg-distributioner.
>
>

hello loggar från den Brandvägg är nu fullständigt integrerat. Security Center kan starta automatiskt samla in och analysera hello-loggarna så att den kan ansluta viktig säkerhetsuppgift aviseringar tooyou.

## <a name="next-steps"></a>Nästa steg
Det här dokumentet visar dig hur hello tooimplement Security Center rekommendation ”Lägg till ett webbprogram”. toolearn mer om hur du konfigurerar en brandvägg för webbaserade program, finns följande hello:

* [Konfigurera en brandvägg för webbaserade program (Brandvägg) för Apptjänst-miljö](../app-service-web/app-service-app-service-environment-web-application-firewall.md)

toolearn mer om Security Center finns hello följande:

* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.
* [Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --hittar du blogginlägg om säkerhet och Azure kompatibilitet.

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png
