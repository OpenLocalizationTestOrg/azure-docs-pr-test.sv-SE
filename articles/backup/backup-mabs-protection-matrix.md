---
title: "aaaWhat kan Azure Backup Server säkerhetskopiera | Microsoft Docs"
description: "Den här artikeln innehåller en supportmatrisen som visar en lista över alla arbetsbelastningar datatyper och installationer som skyddar Azure Backup Server v2."
services: backup
documentation center: 
author: markgalioto
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
keywords: 
ms.date: 05/15/2017
ms.topic: article
ms.author: markgal,masaran
manager: carmonm
ms.openlocfilehash: 30303032d2a016c3f3c6a78d274d843b98acfa03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-backup-server-protection-matrix"></a>Skyddsöversikt för Azure Backup Server

Den här artikeln visar hello olika servrar och arbetsbelastningar som kan skyddas med Azure Backup Server. hello visar följande matris vad som kan skyddas med Azure Backup Server v1 och v2.

## <a name="protection-support-matrix"></a>Skydd supportmatrisen

|Arbetsbelastning|Version|Azure Backup Server</br> installation|Azure Backup</br> Servern v2|Azure Backup</br> Servern v1 |Skydd och återställning|
|------------|-----------|--------------------|--------------------------------------------|--------------------------------|---------------------------|
|System Center VMM|VMM 2016<br/>VMM 2012 SP1, R2|Fysisk server<br /><br />Virtuell Hyper-V-dator|Y|Y|Alla distributionsscenarier: databas|
|Klientdatorer (64-bitars och 32-bitars)|Windows 10|Fysisk server<br /><br />Virtuell Hyper-V-dator<br /><br />Virtuell VMware-dator|Y|Y|Filer<br /><br />Skyddade volymer måste vara NTFS. FAT och FAT32 stöds inte.<br /><br />Volymer måste vara minst 1 GB. DPM använder Volume Shadow Copy Service (VSS) tootake hello ögonblicksbild och hello ögonblicksbilden fungerar bara om hello volymen är på minst 1 GB.|
|Klientdatorer (64-bitars och 32-bitars)|Windows 8.1|Fysisk server<br /><br />Virtuell Hyper-V-dator|Y|Y|Filer<br /><br />Skyddade volymer måste vara NTFS. FAT och FAT32 stöds inte.<br /><br />Volymer måste vara minst 1 GB. DPM använder Volume Shadow Copy Service (VSS) tootake hello ögonblicksbild och hello ögonblicksbilden fungerar bara om hello volymen är på minst 1 GB.|
|Klientdatorer (64-bitars och 32-bitars)|Windows 8.1|Windows-dator i VMWare (skyddar arbetsbelastningar som körs på Windows virtuell dator i VMWare)|Y|Y|Filer<br /><br />Skyddade volymer måste vara NTFS och minst 1 GB.|
|Klientdatorer (64-bitars och 32-bitars)|Windows 8|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y|Y|Filer<br /><br />Skyddade volymer måste vara NTFS och minst 1 GB.|
|Klientdatorer (64-bitars och 32-bitars)|Windows 8|Windows-dator i VMWare (skyddar arbetsbelastningar som körs på Windows virtuell dator i VMWare)|Y|Y|Filer<br /><br />Skyddade volymer måste vara NTFS och minst 1 GB.|
|Klientdatorer (64-bitars och 32-bitars)|Windows 7|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y|Y|Filer<br /><br />Skyddade volymer måste vara NTFS och minst 1 GB.|
|Klientdatorer (64-bitars och 32-bitars)|Windows 7|Windows-dator i VMWare (skyddar arbetsbelastningar som körs på Windows virtuell dator i VMWare)|Y|Y |Filer<br /><br />Skyddade volymer måste vara NTFS och minst 1 GB.|
|Klientdatorer (64-bitars och 32-bitars)|Windows Vista med SP2|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y|Y|Filer<br /><br />Skyddade volymer måste vara NTFS och minst 1 GB.|
|Klientdatorer (64-bitars och 32-bitars)|Windows Vista med SP1|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y|Y|Filer<br /><br />Skyddade volymer måste vara NTFS och minst 1 GB.|
|Klientdatorer (64-bitars och 32-bitars)|Windows Vista|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y|Y|Filer<br /><br />Skyddade volymer måste vara NTFS och minst 1 GB.|
|Klientdatorer (64-bitars och 32-bitars)|Windows Vista|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y|Y|Volym, dela, mapp, fil, systemtillstånd/utan operativsystem), deduplicerade volymer|
|Servrar (32-bitars och 64-bitars)|Windows Server 2016|Azure-dator (när arbetsbelastningen körs som virtuell Azure-dator)<br /><br />Windows-dator i VMWare (skyddar arbetsbelastningar som körs på Windows virtuell dator i VMWare)<br /><br />Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y<br /><br />Inte Nano server|N|Volym, dela, mapp, fil, systemtillstånd/utan operativsystem), deduplicerade volymer|
|Servrar (32-bitars och 64-bitars)|Windows Server 2012 R2 – Datacenter och Standard|Azure-dator (när arbetsbelastningen körs som virtuell Azure-dator)|Y|Y |Volym, dela, mapp, fil<br /><br />DPM måste köras på Windows Server 2012 R2 tooprotect Windows Server 2012 deduplicerade volymer.|
|Servrar (32-bitars och 64-bitars)|Windows Server 2012 R2 – Datacenter och Standard|Windows-dator i VMWare (skyddar arbetsbelastningar som körs på Windows virtuell dator i VMWare)|Y|Y|Volym, dela, mapp, fil, systemtillstånd/utan operativsystem)<br /><br />DPM måste köras på Windows Server 2012 eller 2012 R2 tooprotect Windows Server 2012 deduplicerade volymer.|
|Servrar (32-bitars och 64-bitars)|Windows Server 2012/2012 med SP1 – Datacenter och Standard|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y|Y|Volym, dela, mapp, fil, systemtillstånd/utan operativsystem<br /><br />DPM måste köras på Windows Server 2012 R2 tooprotect Windows Server 2012 deduplicerade volymer.|
|Servrar (32-bitars och 64-bitars)|Windows Server 2012/2012 med SP1 – Datacenter och Standard|Azure-dator (när arbetsbelastningen körs som virtuell Azure-dator)|Y|Y|Volym, dela, mapp, fil<br /><br />DPM måste köras på Windows Server 2012 R2 tooprotect Windows Server 2012 deduplicerade volymer.|
|Servrar (32-bitars och 64-bitars)|Windows Server 2012/2012 med SP1 – Datacenter och Standard|Windows-dator i VMWare (skyddar arbetsbelastningar som körs på Windows virtuell dator i VMWare)|Y|Y|Volym, dela, mapp, fil, systemtillstånd/utan operativsystem<br /><br />DPM måste köras på Windows Server 2012 R2 tooprotect Windows Server 2012 deduplicerade volymer.|
|Servrar (32-bitars och 64-bitars)|Windows Server 2008 R2 SP1 - Standard och Enterprise|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y<br /><br />Du behöver toobe med SP1 eller installera [Windows Management ram 4.0](https://www.microsoft.com/en-us/download/details.aspx?id=40855)|Y|Volym, dela, mapp, fil, systemtillstånd/utan operativsystem|
|Servrar (32-bitars och 64-bitars)|Windows Server 2008 R2 SP1 - Standard och Enterprise|Azure-dator (när arbetsbelastningen körs som virtuell Azure-dator)|Y<br /><br />Du behöver toobe med SP1 eller installera [Windows Management ram 4.0](https://www.microsoft.com/en-us/download/details.aspx?id=40855)|Y |Volym, dela, mapp, fil|
|Servrar (32-bitars och 64-bitars)|Windows Server 2008 R2 SP1 - Standard och Enterprise|Windows-dator i VMWare (skyddar arbetsbelastningar som körs på Windows virtuell dator i VMWare)|Y<br /><br />Du behöver toobe med SP1 eller installera [Windows Management ram 4.0](https://www.microsoft.com/en-us/download/details.aspx?id=40855)|Y |Volym, dela, mapp, fil, systemtillstånd/utan operativsystem|
|Servrar (32-bitars och 64-bitars)|Windows Server 2008 R2|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y|Y|Volym, dela, mapp, fil, systemtillstånd/utan operativsystem|
|Servrar (32-bitars och 64-bitars)|Windows Server 2008 R2|Azure-dator (när arbetsbelastningen körs som virtuell Azure-dator)|N|Y|Volym, dela, mapp, fil|
|Servrar (32-bitars och 64-bitars)|Windows Server 2008 R2|Windows-dator i VMWare (skyddar arbetsbelastningar som körs på Windows virtuell dator i VMWare)|N|Y|Volym, dela, mapp, fil, systemtillstånd/utan operativsystem|
|Servrar (32-bitars och 64-bitars)|Windows Server 2008|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|N|Y|Volym, dela, mapp, fil, systemtillstånd/utan operativsystem|
|Servrar (32-bitars och 64-bitars)|Windows Server 2008|Windows-dator i VMWare (skyddar arbetsbelastningar som körs på Windows virtuell dator i VMWare)|Y|Y |Volym, dela, mapp, fil, systemtillstånd/utan operativsystem|
|Servrar (32-bitars och 64-bitars)|Windows Storage Server 2008|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y|Y|Volym, dela, mapp, fil, systemtillstånd/utan operativsystem|
|SQL Server|SQL Server 2016|Fysisk server <br /><br /> Lokal Hyper-V virtuell dator <br /> <br /> Virtuell Azure-dator <br /><br /> Windows-dator i VMWare (skyddar arbetsbelastningar som körs på Windows virtuell dator i VMWare)|Y |N|Alla distributionsscenarier: databas|
|SQL Server|SQL Server 2014|Azure-dator (när arbetsbelastningen körs som virtuell Azure-dator)|Y|Y |Alla distributionsscenarier: databas|
|SQL Server|SQL Server 2014|Windows-dator i VMWare (skyddar arbetsbelastningar som körs på Windows virtuell dator i VMWare)|Y|Y|Alla distributionsscenarier: databas|
|SQL Server|SQLServer 2012 med SP2|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y|Y |Alla distributionsscenarier: databas|
|SQL Server|SQLServer 2012 med SP2|Azure-dator (när arbetsbelastningen körs som virtuell Azure-dator)|Y|Y|Alla distributionsscenarier: databas|
|SQL Server|SQLServer 2012 med SP2|Windows-dator i VMWare (skyddar arbetsbelastningar som körs på Windows virtuell dator i VMWare)|Y|Y|Alla distributionsscenarier: databas|
|SQL Server|SQLServer 2012, SQLServer 2012 med SP1|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y|Y|Alla distributionsscenarier: databas|
|SQL Server|SQLServer 2012, SQLServer 2012 med SP1|Azure-dator (när arbetsbelastningen körs som virtuell Azure-dator)|Y|Y|Alla distributionsscenarier: databas|
|SQL Server|SQLServer 2012, SQLServer 2012 med SP1|Windows-dator i VMWare (skyddar arbetsbelastningar som körs på Windows virtuell dator i VMWare)|Y|Y|Alla distributionsscenarier: databas|
|SQL Server|SQL Server 2008 R2|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y|Y|Alla distributionsscenarier: databas|
|SQL Server|SQL Server 2008 R2|Azure-dator (när arbetsbelastningen körs som virtuell Azure-dator)|Y|Y|Alla distributionsscenarier: databas|
|SQL Server|SQL Server 2008 R2|Windows-dator i VMWare (skyddar arbetsbelastningar som körs på Windows virtuell dator i VMWare)|Y|Y |Alla distributionsscenarier: databas|
|SQL Server|SQLServer 2008|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y|Y|Alla distributionsscenarier: databas|
|SQL Server|SQLServer 2008|Azure-dator (när arbetsbelastningen körs som virtuell Azure-dator)|Y|Y|Alla distributionsscenarier: databas|
|SQL Server|SQLServer 2008|Windows-dator i VMWare (skyddar arbetsbelastningar som körs på Windows virtuell dator i VMWare)|Y|Y|Alla distributionsscenarier: databas|
|Exchange|Exchange 2016|Fysisk server<br/><br/> Lokal Hyper-V virtuell dator|Y|Y|Skydda (alla distributionsscenarion): fristående Exchange-server, databas under en database availability group (DAG)<br /><br />Återställa (alla distributionsscenarion): postlåda, postlådedatabaser under en DAG|
|Exchange|Exchange 2016|Windows-dator i VMWare (skyddar arbetsbelastningar som körs på Windows virtuell dator i VMWare)|Y|Y|Skydda (alla distributionsscenarion): fristående Exchange-server, databas under en database availability group (DAG)<br /><br />Återställa (alla distributionsscenarion): postlåda, postlådedatabaser under en DAG|
|Exchange|Exchange 2013|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y|Y|Skydda (alla distributionsscenarion): fristående Exchange-server, databas under en database availability group (DAG)<br /><br />Återställa (alla distributionsscenarion): postlåda, postlådedatabaser under en DAG|
|Exchange|Exchange 2013|Windows-dator i VMWare (skyddar arbetsbelastningar som körs på Windows virtuell dator i VMWare)|Y|Y |Skydda (alla distributionsscenarion): fristående Exchange-server, databas under en database availability group (DAG)<br /><br />Återställa (alla distributionsscenarion): postlåda, postlådedatabaser under en DAG|
|Exchange|Exchange 2010|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y|Y|Skydda (alla distributionsscenarion): fristående Exchange-server, databas under en database availability group (DAG)<br /><br />Återställa (alla distributionsscenarion): postlåda, postlådedatabaser under en DAG|
|Exchange|Exchange 2010|Windows-dator i VMWare (skyddar arbetsbelastningar som körs på Windows virtuell dator i VMWare)|Y|Y |Skydda (alla distributionsscenarion): fristående Exchange-server, databas under en database availability group (DAG)<br /><br />Återställa (alla distributionsscenarion): postlåda, postlådedatabaser under en DAG|
|Exchange|Exchange 2007|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y|Y|Skydda (alla distributionsscenarion): lagringsgrupp<br /><br />Återställa (alla distributionsscenarion): lagringsgrupp, databas, postlåda|
|Exchange|Exchange 2007|Windows-dator i VMWare (skyddar arbetsbelastningar som körs på Windows virtuell dator i VMWare)|Y|Y |Skydda (alla distributionsscenarion): lagringsgrupp<br /><br />Återställa (alla distributionsscenarion): lagringsgrupp, databas, postlåda|
|SharePoint|SharePoint 2016|Fysisk server<br /><br />Lokal Hyper-V virtuell dator<br /><br />Azure-dator (när arbetsbelastningen körs som virtuell Azure-dator)<br /><br />Windows-dator i VMWare (skyddar arbetsbelastningar som körs på Windows virtuell dator i VMWare)|Y |N|Skydda (alla distributionsscenarion): servergrupp, webbserverinnehåll<br /><br />Återställa (alla distributionsscenarion): servergrupp, databas, web application, fil- eller listobjekt, SharePoint-sökning, webbserver för klientdel<br /><br />Observera att skydda en SharePoint-servergrupp som använder hello SQL Server 2012 AlwaysOn-funktionen för hello innehållsdatabaser inte stöds.|
|SharePoint|SharePoint 2013|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y|Y|Skydda (alla distributionsscenarion): servergrupp, webbserverinnehåll<br /><br />Återställa (alla distributionsscenarion): servergrupp, databas, web application, fil- eller listobjekt, SharePoint-sökning, webbserver för klientdel<br /><br />Observera att skydda en SharePoint-servergrupp som använder hello SQL Server 2012 AlwaysOn-funktionen för hello innehållsdatabaser inte stöds.|
|SharePoint|SharePoint 2013|Azure-dator (när arbetsbelastningen körs som virtuell Azure-dator) – DPM 2012 R2 Samlad uppdatering 3 och senare|Y|Y|Skydda (alla distributionsscenarion): servergrupp, SharePoint-sökning, webbserverinnehåll<br /><br />Återställa (alla distributionsscenarion): servergrupp, databas, web application, fil- eller listobjekt, SharePoint-sökning, webbserver för klientdel<br /><br />Observera att skydda en SharePoint-servergrupp som använder hello SQL Server 2012 AlwaysOn-funktionen för hello innehållsdatabaser inte stöds.|
|SharePoint|SharePoint 2013|Windows-dator i VMWare (skyddar arbetsbelastningar som körs på Windows virtuell dator i VMWare)|Y|Y |Skydda (alla distributionsscenarion): servergrupp, SharePoint-sökning, webbserverinnehåll<br /><br />Återställa (alla distributionsscenarion): servergrupp, databas, web application, fil- eller listobjekt, SharePoint-sökning, webbserver för klientdel<br /><br />Observera att skydda en SharePoint-servergrupp som använder hello SQL Server 2012 AlwaysOn-funktionen för hello innehållsdatabaser inte stöds.|
|SharePoint|SharePoint 2010|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y|Y|Skydda (alla distributionsscenarion): servergrupp, SharePoint-sökning, webbserverinnehåll<br /><br />Återställa (alla distributionsscenarion): servergrupp, databas, web application, fil- eller listobjekt, SharePoint-sökning, webbserver för klientdel|
|SharePoint|SharePoint 2010|Azure-dator (när arbetsbelastningen körs som virtuell Azure-dator)|Y|Y |Skydda (alla distributionsscenarion): servergrupp, SharePoint-sökning, webbserverinnehåll<br /><br />Återställa (alla distributionsscenarion): servergrupp, databas, web application, fil- eller listobjekt, SharePoint-sökning, webbserver för klientdel|
|SharePoint|SharePoint 2010|Windows-dator i VMWare (skyddar arbetsbelastningar som körs på Windows virtuell dator i VMWare)|Y|Y|Skydda (alla distributionsscenarion): servergrupp, SharePoint-sökning, webbserverinnehåll<br /><br />Återställa (alla distributionsscenarion): servergrupp, databas, web application, fil- eller listobjekt, SharePoint-sökning, webbserver för klientdel|
|SharePoint|SharePoint 2007|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y|Y|Skydda (alla distributionsscenarion): servergrupp, SharePoint-sökning, webbserverinnehåll<br /><br />Återställa (alla distributionsscenarion): servergrupp, databas, web application, fil- eller listobjekt, SharePoint-sökning, webbserver för klientdel|
|SharePoint|SharePoint 2007|Windows-dator i VMWare (skyddar arbetsbelastningar som körs på Windows virtuell dator i VMWare)|Y|Y|Skydda (alla distributionsscenarion): servergrupp, SharePoint-sökning, webbserverinnehåll<br /><br />Återställa (alla distributionsscenarion): servergrupp, databas, web application, fil- eller listobjekt, SharePoint-sökning, webbserver för klientdel|
|Hyper-V-värd - DPM-skyddsagenten på Hyper-V-värdservern, kluster eller VM|Windows Server 2016|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y|N|Skydda: Hyper-V-datorer, klusterdelade volymer (CSV)<br /><br />Återställ: virtuell dator, återställning av filer och mappar, volymer, virtuella hårddiskar|
|Hyper-V-värd - DPM-skyddsagenten på Hyper-V-värdservern, kluster eller VM|Windows Server 2012 R2 – Datacenter och Standard|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y|Y|Skydda: Hyper-V-datorer, klusterdelade volymer (CSV)<br /><br />Återställ: virtuell dator, återställning av filer och mappar, volymer, virtuella hårddiskar|
|Hyper-V-värd - DPM-skyddsagenten på Hyper-V-värdservern, kluster eller VM|Windows Server 2012 – Datacenter och Standard|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y|Y|Skydda: Hyper-V-datorer, klusterdelade volymer (CSV)<br /><br />Återställ: virtuell dator, återställning av filer och mappar, volymer, virtuella hårddiskar|
|Hyper-V-värd - DPM-skyddsagenten på Hyper-V-värdservern, kluster eller VM|Windows Server 2008 R2 SP1 - Enterprise och Standard|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|Y|Y|Skydda: Hyper-V-datorer, klusterdelade volymer (CSV)<br /><br />Återställ: virtuell dator, återställning av filer och mappar, volymer, virtuella hårddiskar|
|Hyper-V-värd - DPM-skyddsagenten på Hyper-V-värdservern, kluster eller VM|Windows Server 2008|Fysisk server<br /><br />Lokal Hyper-V virtuell dator|N|N|Skydda: Hyper-V-datorer, klusterdelade volymer (CSV)<br /><br />Återställ: virtuell dator, återställning av filer och mappar, volymer, virtuella hårddiskar|
|VMwares virtuella datorer|VMware server 5.5 eller 6.0 eller 6.5 |Lokal Hyper-V virtuell dator|Y|Y (med UR1)|Virtuella VMware-datorer på klusterdelade volymer (CSV), NFS, och SAN-lagring<br /> Objektnivååterställning av filer och mappar som är endast tillgängligt för Windows<br /> VMware vApps stöds inte|
|Linux|Linux körs som Hyper-V- eller VMware-gäst|Lokal Hyper-V virtuell dator|Y|Y|Hyper-V måste köras på Windows Server 2012 R2 eller Windows Server 2016. Skydda: Hela den virtuella datorn<br /><br />Återställ: Hela den virtuella datorn|

## <a name="cluster-support"></a>Stöd för kluster
Azure Backup-Server kan skydda data i hello följande klustrade program:

-   Filservrar

-   SQL Server

-   Hyper-V - om du skyddar ett Hyper-V-kluster med utskalat DPM-skydd, kan du lägga till sekundärt skydd för hello skyddade Hyper-V-arbetsbelastningar.

    Om du kör Hyper-V på Windows Server 2008 R2, kontrollerar du att tooinstall hello uppdateringen som beskrivs i KB [975354](https://support.microsoft.com/en-us/kb/975354).
    Om du kör Hyper-V på Windows Server 2008 R2 i en klusterkonfiguration, måste du installera SP2 och KB [971394](https://support.microsoft.com/en-us/kb/971394).

-   Exchange Server - Azure Backup-Server kan skydda icke delade diskkluster för Exchange Server versioner som stöds (klustret kontinuerlig replikering) och kan även skydda Exchange Server som konfigurerats för kontinuerlig lokal replikering.

-   SQLServer - Azure Backup-servern stöder inte säkerhetskopiering av SQL Server-databaser som finns på klusterdelade volymer (CSV).

Azure Backup-Server kan skydda arbetsbelastningar i ett kluster som finns i hello samma domän som hello DPM-server och en underordnad eller betrodd domän. Om du vill tooprotect datakällor i obetrodda domäner eller arbetsgrupper måste du använda NTLM eller certifikat för en enskild server, eller certifikatautentisering endast för ett kluster.
