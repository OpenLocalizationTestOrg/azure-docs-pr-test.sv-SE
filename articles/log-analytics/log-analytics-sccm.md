---
title: aaaConnect Configuration Manager tooLog Analytics | Microsoft Docs
description: "Den här artikeln visar hello steg tooconnect Configuration Manager tooLog Analytics och börja analysera data."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f2298bd7-18d7-4371-b24a-7f9f15f06d66
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: banders
ms.openlocfilehash: dc50ebc46020a806d99d1a3e3d0e91fd09ad2c32
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-configuration-manager-toolog-analytics"></a>Ansluta Configuration Manager tooLog Analytics
Du kan ansluta till System Center Configuration Manager tooLog Analytics i OMS toosync enhetsdata samling. Detta gör data från Configuration Manager-hierarki finns i OMS.

## <a name="prerequisites"></a>Krav

Logganalys stöder System Center Configuration Manager aktuell gren, version 1606 och högre.  

## <a name="configuration-overview"></a>Konfigurationsöversikt
hello följande steg sammanfattar hello processen tooconnect Configuration Manager tooLog Analytics.  

1. Registrera Configuration Manager som en webbprogram och/eller webb-API-app i hello Azure-hanteringsportalen, och kontrollera att du har hello klientens ID och klientens hemliga nyckel från hello registrering från Azure Active Directory. Se [använda portalen toocreate Active Directory-program och tjänstens huvudnamn som kan komma åt resurser](../azure-resource-manager/resource-group-create-service-principal-portal.md) för detaljerad information om hur du utför det här steget.
2. I hello Azure-hanteringsportalen [ge Configuration Manager (hello registrerade webbprogram) behörighet tooaccess OMS](#provide-configuration-manager-with-permissions-to-oms).
3. I Configuration Manager [lägga till en anslutning med hjälp av hello guiden Lägg till OMS](#add-an-oms-connection-to-configuration-manager).
4. I Configuration Manager [uppdatera hello anslutningsegenskaper](#update-oms-connection-properties) om hello lösenord eller klientens hemliga nyckel någonsin upphör att gälla eller går förlorad.
5. Med information från hello OMS-portalen, [ladda ned och installera hello Microsoft Monitoring Agent](#download-and-install-the-agent) på hello-dator som kör hello Configuration Manager-anslutning platssystemsroll. hello agenten skickar tooOMS för Configuration Manager-data.
6. I Log Analytics [importera samlingar från Configuration Manager](#import-collections) som datorgrupper.
7. Visa data från Configuration Manager som i Log Analytics [datorgrupper](log-analytics-computer-groups.md).

Du kan läsa mer om hur du ansluter Configuration Manager tooOMS på [synkronisera data från Configuration Manager toohello Microsoft Operations Management Suite](https://technet.microsoft.com/library/mt757374.aspx).

## <a name="provide-configuration-manager-with-permissions-toooms"></a>Ge behörigheter tooOMS Configuration Manager
hello innehåller följande procedur hello Azure-hanteringsportalen med behörigheter tooaccess OMS. Mer specifikt måste du bevilja hello *deltagarrollen* toousers i hello resursgrupp i ordning tooallow hello Azure-hanteringsportalen tooconnect tooOMS för Configuration Manager.

> [!NOTE]
> Du måste ange behörigheter i OMS för Configuration Manager. Annars får du ett felmeddelande när du använder guiden för konfiguration av hello i Configuration Manager.
>
>

1. Öppna hello [Azure-portalen](https://portal.azure.com/) och på **Bläddra** > **logganalys (OMS)** tooopen hello logganalys (OMS) bladet.  
2. På hello **logganalys (OMS)** bladet, klickar du på **Lägg till** tooopen hello **OMS-arbetsytan** bladet.  
   ![OMS-bladet](./media/log-analytics-sccm/sccm-azure01.png)
3. På hello **OMS-arbetsytan** bladet hello följande information och klickar sedan på **OK**.

   * **OMS-arbetsyta**
   * **Prenumeration**
   * **Resursgrupp**
   * **Plats**
   * **prisnivå**  
     ![OMS-bladet](./media/log-analytics-sccm/sccm-azure02.png)  

     > [!NOTE]
     > hello-exemplet ovan skapar en ny resursgrupp. hello resursgrupp är endast använda tooprovide Configuration Manager med behörigheter toohello OMS-arbetsytan i det här exemplet.
     >
     >
4. Klicka på **Bläddra** > **resursgrupper** tooopen hello **resursgrupper** bladet.
5. I hello **resursgrupper** bladet, klickar du på hello resursgrupp som du skapade ovan tooopen hello &lt;resursgruppens namn&gt; inställningsbladet.  
   ![blad för resursgrupp inställningar](./media/log-analytics-sccm/sccm-azure03.png)
6. I hello &lt;resursgruppnamn&gt; inställningar-bladet klickar du på Access control (IAM) tooopen hello &lt;resursgruppens namn&gt; bladet användare.  
   ![blad för resursgrupp användare](./media/log-analytics-sccm/sccm-azure04.png)  
7. I hello &lt;resursgruppnamn&gt; bladet användare, klickar du på **Lägg till** tooopen hello **Lägg till åtkomst** bladet.
8. I hello **Lägg till åtkomst** bladet, klickar du på **Välj en roll**, och välj sedan hello **deltagare** roll.  
   ![Välj en roll](./media/log-analytics-sccm/sccm-azure05.png)  
9. Klicka på **lägga till användare**, Välj hello Configuration Manager-användare, klicka på **Välj**, och klicka sedan på **OK**.  
   ![Lägg till användare](./media/log-analytics-sccm/sccm-azure06.png)  

## <a name="add-an-oms-connection-tooconfiguration-manager"></a>Lägg till en OMS-anslutning tooConfiguration Manager
I ordning tooadd en OMS-anslutning, Configuration Manager-miljön måste ha en [tjänstanslutningspunkt](https://technet.microsoft.com/library/mt627781.aspx) konfigurerats för online-läge.

1. I hello **Administration** arbetsytan av Configuration Manager, Välj **OMS Connector**. Då öppnas hello **guiden för Lägg till OMS-anslutning**. Välj **nästa**.
2. På hello **allmänna** skärmen, bekräfta att du har gjort hello följande åtgärder och att du har mer detaljer för varje objekt och sedan välja **nästa**.

   1. I hello Azure-hanteringsportalen, du har registrerat Configuration Manager som en webbprogram och/eller webb-API-app och som du har hello [klient-ID från hello registrering](../active-directory/active-directory-integrating-applications.md).
   2. Du har skapat en app hemlig nyckel för hello registrerade app i Azure Active Directory i hello Azure-hanteringsportalen.  
   3. Du har angett hello registrerade webbprogrammet med behörigheten tooaccess OMS i hello Azure-hanteringsportalen.  
      ![Anslutningssidan tooOMS guiden Allmänt](./media/log-analytics-sccm/sccm-console-general01.png)
3. På hello **Azure Active Directory** skärmen, konfigurera din anslutning inställningar tooOMS genom att tillhandahålla ditt **klient** , **klient-ID** , och **klienten hemlig nyckel**  och välj **nästa**.  
   ![Anslutningssidan tooOMS guiden Azure Active Directory](./media/log-analytics-sccm/sccm-wizard-tenant-filled03.png)
4. Om du gör alla hello andra procedurer har, sedan hello information om hello **OMS anslutningskonfiguration** skärm visas automatiskt på den här sidan. Information om inställningar för hello ska visas för dina **Azure-prenumeration** , **Azure-resursgrupp** , och **Operations Management Suite-arbetsyta**.  
   ![Anslutningssidan tooOMS guiden OMS-anslutning](./media/log-analytics-sccm/sccm-wizard-configure04.png)
5. hello guiden ansluter toohello OMS-tjänsten med hjälp av hello information som du har indata. Välj hello enhetssamlingar du vill toosync med OMS och klicka sedan på **Lägg till**.  
   ![Välj samlingar](./media/log-analytics-sccm/sccm-wizard-add-collections05.png)
6. Kontrollera anslutningsinställningarna på hello **sammanfattning** skärmen och väljer sedan **nästa**. Hej **förlopp** skärmen visar hello anslutningsstatus och sedan ska **Slutför**.

> [!NOTE]
> Du måste ansluta OMS toohello översta nivån i hierarkin. Om du ansluter OMS tooa fristående primär plats och sedan lägga till en central administrationsplats plats tooyour miljö kan du ha toodelete och återskapa hello OMS-anslutning i hello ny hierarki.
>
>

När du har länkat tooOMS för Configuration Manager kan du lägga till eller ta bort samlingar och visa hello egenskaper för hello OMS-anslutning.

## <a name="update-oms-connection-properties"></a>Uppdatera OMS anslutningsegenskaper
Om en hemlig nyckel lösenord eller klienten aldrig upphör att gälla eller tappas bort, behöver du toomanually uppdatering hello OMS anslutningsegenskaper.

1. Navigera i Konfigurationshanteraren för**molntjänster** och välj **OMS Connector** tooopen hello **OMS anslutningsegenskaper** sidan.
2. Klicka på den här sidan hello **Azure Active Directory** fliken tooview din **klient**, **klient-ID**, **klientens hemliga nyckel förfallodatum**. **Kontrollera** din **klientens hemliga nyckel** om det har gått ut.

## <a name="download-and-install-hello-agent"></a>Hämta och installera hello-agent
1. I hello OMS-portalen, [Download hello agent installationsfilen från OMS](log-analytics-windows-agents.md#download-the-agent-setup-file-from-oms).
2. Använd någon av följande metoder tooinstall hello och konfigurera hello-agenten på hello-dator som kör hello Configuration Manager service anslutning platssystemroll:
   * [Installera hello-agenten med hjälp av installationsprogrammet](log-analytics-windows-agents.md#install-the-agent-using-setup)
   * [Installera hello agent med kommandoraden hello](log-analytics-windows-agents.md#install-the-agent-using-the-command-line)
   * [Installera hello-agenten i Azure Automation DSC](log-analytics-windows-agents.md#install-the-agent-using-dsc-in-azure-automation)

## <a name="import-collections"></a>Importera samlingar
När du har lagt till en OMS-anslutning tooConfiguration Manager och installerat hello-agenten på hello-dator som kör hello Configuration Manager-anslutning platssystemsroll, hello nästa steg är tooimport samlingar från Configuration Manager i OMS som datorgrupper.

Efter importen har aktiverats, hello samling medlemskapsinformation hämtas alla tre timmar tookeep hello samlingsmedlemskap aktuella. Du kan välja toodisable import när som helst.

1. I hello OMS-portalen klickar du på **inställningar**.
2. Klicka på hello **datorgrupper** och klicka sedan på hello **SCCM** fliken.
3. Välj **Import Configuration Manager samlingsmedlemskap** och klicka sedan på **spara**.  
   ![Datorgrupper - SCCM-fliken](./media/log-analytics-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Visa data från Configuration Manager
När du har lagt till en OMS-anslutning tooConfiguration Manager och installerat hello-agenten på hello-dator som kör hello Configuration Manager service anslutning platssystemroll, skickas data från hello agent tooOMS. Configuration Manager-samlingar visas som i OMS, [datorgrupper](log-analytics-computer-groups.md). Du kan visa hello grupper från hello **Configuration Manager** sidan **datorgrupper** i **inställningar**.

Efter hello samlingar importeras, kan du se hur många datorer med samlingsmedlemskap har identifierats. Du kan också se hello antal samlingar som har importerats.

![Datorgrupper - SCCM-fliken](./media/log-analytics-sccm/sccm-computer-groups02.png)

När du klickar på antingen en sökning öppnas importerade visas antingen alla hello grupper eller alla datorer som tillhör tooeach grupp. Med hjälp av [loggen Sök](log-analytics-log-searches.md), kan du starta djupgående analys av data för Configuration Manager.

## <a name="next-steps"></a>Nästa steg
* Använd [loggen Sök](log-analytics-log-searches.md) tooview detaljerad information om Configuration Manager-data.
