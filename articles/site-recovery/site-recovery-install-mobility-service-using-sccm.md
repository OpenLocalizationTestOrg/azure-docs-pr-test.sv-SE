---
title: "aaaAutomate installation av Mobilitetstjänsten för Azure Site Recovery med hjälp av programdistribueringsverktyg | Microsoft Docs"
description: "Den här artikeln hjälper dig att automatisera installation av Mobilitetstjänsten med hjälp av verktyg för programvarudistribution som System Center Configuration Manager."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: 6c883c6d5308dcec6e0628b0c2196b3a12e08ebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automate-mobility-service-installation-by-using-software-deployment-tools"></a>Automatisera installation av Mobilitetstjänsten med hjälp av verktyg för programvarudistribution

>[!IMPORTANT]
Det här dokumentet förutsätts att du använder version **9.9.4510.1** eller högre.

Den här artikeln innehåller ett exempel på hur du kan använda System Center Configuration Manager toodeploy hello Azure Site Recovery Mobility-tjänsten i ditt datacenter. Använda ett distributionsverktyg för programvara som Configuration Manager har hello följande fördelar:
* Schemalägga distributionen av nya installationer och uppgraderingar under planerat underhållsfönstret för programuppdateringar
* Skalning toohundreds för distribution av servrar samtidigt


> [!NOTE]
> Den här artikeln använder System Center Configuration Manager 2012 R2 toodemonstrate hello aktiviteter. Du kan också automatisera installation av Mobilitetstjänsten med hjälp av [Azure Automation och Desired State Configuration](site-recovery-automate-mobility-service-install.md).

## <a name="prerequisites"></a>Krav
1. Ett program distributionsverktyg, som Configuration Manager som redan har distribuerats i din miljö.
  Skapa två [enhetssamlingar](https://technet.microsoft.com/library/gg682169.aspx), en för alla **Windows-servrar**, och en annan för alla **Linux-servrar**, som du vill tooprotect genom att använda Site Recovery.
3. Konfigurationsservern som redan har registrerats med Site Recovery.
4. En säker nätverksfilresurs (Server Message Block-resurs) som kan användas av hello Configuration Manager-servern.

## <a name="deploy-mobility-service-on-computers-running-windows"></a>Distribuera Mobilitetstjänsten på datorer som kör Windows
> [!NOTE]
> Den här artikeln förutsätter att hello hello configuration serverns IP-adress är 192.168.3.121 och att hello säkert nätverk filresursen \\\ContosoSecureFS\MobilityServiceInstallers.

### <a name="step-1-prepare-for-deployment"></a>Steg 1: Förbereda för distribution
1. Skapa en mapp på hello nätverksresurs och ge den namnet **MobSvcWindows**.
2. Logga in tooyour konfigurationsservern och öppna en administrativ kommandotolk.
3. Kör följande kommandon toogenerate en lösenfras fil hello:

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. Kopiera hello **MobSvc.passphrase** filen till hello **MobSvcWindows** mapp på nätverksresursen.
5. Bläddra toohello installer databasen på hello konfigurationsservern genom att köra följande kommando hello:

   `cd %ProgramData%\ASR\home\svsystems\puhsinstallsvc\repository`

6. Kopiera hello  **Microsoft ASR\_UA\_*version*\_Windows\_GA\_*datum* \_ Release.exe** toohello **MobSvcWindows** mapp på nätverksresursen.
7. Kopiera följande kod hello och spara den som **install.bat** till hello **MobSvcWindows** mapp.

   > [!NOTE]
   > Ersätt hello [CSIP] platshållare i det här skriptet med hello faktiska värden för hello configuration-serverns IP-adress.

```DOS
Time /t >> C:\Temp\logfile.log
REM ==================================================
REM ==== Clean up hello folders ========================
RMDIR /S /q %temp%\MobSvc
MKDIR %Temp%\MobSvc
MKDIR C:\Temp
REM ==================================================

REM ==== Copy new files ==============================
COPY M*.* %Temp%\MobSvc
CD %Temp%\MobSvc
REN Micro*.exe MobSvcInstaller.exe
REM ==================================================

REM ==== Extract hello installer =======================
MobSvcInstaller.exe /q /x:%Temp%\MobSvc\Extracted
REM ==== Wait 10s for extraction toocomplete =========
TIMEOUT /t 10
REM =================================================

REM ==== Perform installation =======================
REM =================================================

CD %Temp%\MobSvc\Extracted
whoami >> C:\Temp\logfile.log
SET PRODKEY=HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall
REG QUERY %PRODKEY%\{275197FC-14FD-4560-A5EB-38217F80CBD1}
IF NOT %ERRORLEVEL% EQU 0 (
    echo "Product is not installed. Goto INSTALL." >> C:\Temp\logfile.log
    GOTO :INSTALL
) ELSE (
    echo "Product is installed." >> C:\Temp\logfile.log

    echo "Checking for Post-install action status." >> C:\Temp\logfile.log
    GOTO :POSTINSTALLCHECK
)

:POSTINSTALLCHECK
    REG QUERY "HKLM\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\5" /v "PostInstallActions" | Find "Succeeded"
    If %ERRORLEVEL% EQU 0 (
        echo "Post-install actions succeeded. Checking for Configuration status." >> C:\Temp\logfile.log
        GOTO :CONFIGURATIONCHECK
    ) ELSE (
        echo "Post-install actions didn't succeed. Goto INSTALL." >> C:\Temp\logfile.log
        GOTO :INSTALL
    )

:CONFIGURATIONCHECK
    REG QUERY "HKLM\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\5" /v "AgentConfigurationStatus" | Find "Succeeded"
    If %ERRORLEVEL% EQU 0 (
        echo "Configuration has succeeded. Goto UPGRADE." >> C:\Temp\logfile.log
        GOTO :UPGRADE
    ) ELSE (
        echo "Configuration didn't succeed. Goto CONFIGURE." >> C:\Temp\logfile.log
        GOTO :CONFIGURE
    )


:INSTALL
    echo "Perform installation." >> C:\Temp\logfile.log
    UnifiedAgent.exe /Role MS /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
    IF %ERRORLEVEL% EQU 0 (
        echo "Installation has succeeded." >> C:\Temp\logfile.log
        (GOTO :CONFIGURE)
    ) ELSE (
        echo "Installation has failed." >> C:\Temp\logfile.log
        GOTO :ENDSCRIPT
    )

:CONFIGURE
    echo "Perform configuration." >> C:\Temp\logfile.log
    cd "C:\Program Files (x86)\Microsoft Azure Site Recovery\agent"
    UnifiedAgentConfigurator.exe  /CSEndPoint "[CSIP]" /PassphraseFilePath %Temp%\MobSvc\MobSvc.passphrase
    IF %ERRORLEVEL% EQU 0 (
        echo "Configuration has succeeded." >> C:\Temp\logfile.log
    ) ELSE (
        echo "Configuration has failed." >> C:\Temp\logfile.log
    )
    GOTO :ENDSCRIPT

:UPGRADE
    echo "Perform upgrade." >> C:\Temp\logfile.log
    UnifiedAgent.exe /Platform "VmWare" /Silent
    IF %ERRORLEVEL% EQU 0 (
        echo "Upgrade has succeeded." >> C:\Temp\logfile.log
    ) ELSE (
        echo "Upgrade has failed." >> C:\Temp\logfile.log
    )
    GOTO :ENDSCRIPT

:ENDSCRIPT
    echo "End of script." >> C:\Temp\logfile.log


```

### <a name="step-2-create-a-package"></a>Steg 2: Skapa ett paket

1. Logga in tooyour Configuration Manager-konsolen.
2. Bläddra för**programbibliotek** > **programhantering** > **paket**.
3. Högerklicka på **paket**, och välj **skapa paket**.
4. Ange värden för hello namn, beskrivning, tillverkare, språk och version.
5. Välj hello **det här paketet innehåller källfiler** kryssrutan.
6. Klicka på **Bläddra**, och välj hello nätverksresurs där hello installer lagras (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcWindows).

  ![Skärmbild av skapa paket och Program Guiden](./media/site-recovery-install-mobility-service-using-sccm/create_sccm_package.png)

7. På hello **Välj hello typ av program som du vill toocreate** väljer **standardprogram**, och klicka på **nästa**.

  ![Skärmbild av skapa paket och Program Guiden](./media/site-recovery-install-mobility-service-using-sccm/sccm-standard-program.png)

8. På hello **ange information om det här standardprogrammet** anger hello följande indata, och klicka på **nästa**. (hello andra indata kan använda sina standardvärden.)

  | **Parameternamn** | **Värde** |
  |--|--|
  | Namn | Installera Microsoft Azure-Mobilitetstjänsten (Windows) |
  | Kommandorad | Install.bat |
  | Programmet kan köras | Om en användare är inloggad |

  ![Skärmbild av skapa paket och Program Guiden](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties.png)

9. Välj hello operativsystem på hello nästa sida. Mobilitetstjänsten kan installeras endast på Windows Server 2012 R2, Windows Server 2012 och Windows Server 2008 R2.

  ![Skärmbild av skapa paket och Program Guiden](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2.png)

10. toocomplete hello guiden klickar du på **nästa** två gånger.


> [!NOTE]
> hello skript stöder både nya installationer av Mobilitetstjänsten agenter och uppdaterar tooagents som redan är installerade.

### <a name="step-3-deploy-hello-package"></a>Steg 3: Distribuera hello-paket
1. Högerklicka på paketet i hello Configuration Manager-konsolen och välj **distribuera innehåll**.
  ![Skärmbild av Configuration Manager-konsolen](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)
2. Välj hello  **[distributionsplatser](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)**  på toowhich hello paket ska kopieras.
3. Fullständig hello guiden. hello-paketet och sedan startar replikerar toohello angivna distributionsplatser.
4. När hello paketdistribution görs, högerklicka på hello paketet och väljer **distribuera**.
  ![Skärmbild av Configuration Manager-konsolen](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)
5. Välj hello Windows Server enhetssamling som du skapade i avsnittet förutsättningar för hello som hello målsamlingen för distributionen.

  ![Guiden Distribuera programvara skärmbild](./media/site-recovery-install-mobility-service-using-sccm/sccm-select-target-collection.png)

6. På hello **ange hello innehållsmål** väljer din **distributionsplatser**.
7. På hello **ange inställningar toocontrol hur programvaran distribueras** , se till att hello syfte är **krävs**.

  ![Guiden Distribuera programvara skärmbild](./media/site-recovery-install-mobility-service-using-sccm/sccm-deploy-select-purpose.png)

8. På hello **ange hello schemat för distributionen** anger du ett schema. Mer information finns i [schemaläggning paket](https://technet.microsoft.com/library/gg682178.aspx).
9. På hello **distributionsplatser** konfigurerar hello egenskaper enligt toohello behoven i ditt datacenter. Slutför guiden hello.

> [!TIP]
> tooavoid onödiga startas om, schema hello installationen under din månatligt underhåll eller programvara uppdateringar fönster.

Du kan övervaka hello förlopp med hjälp av hello Configuration Manager-konsolen. Gå för**övervakning** > **distributioner** > *[package name]*.

  ![Skärmbild av Configuration Manager alternativet toomonitor distributioner](./media/site-recovery-install-mobility-service-using-sccm/report.PNG)

## <a name="deploy-mobility-service-on-computers-running-linux"></a>Distribuera Mobilitetstjänsten på datorer som kör Linux
> [!NOTE]
> Den här artikeln förutsätter att hello hello configuration serverns IP-adress är 192.168.3.121 och att hello säkert nätverk filresursen \\\ContosoSecureFS\MobilityServiceInstallers.

### <a name="step-1-prepare-for-deployment"></a>Steg 1: Förbereda för distribution
1. Skapa en mapp på hello nätverksresurs, och ger den namnet som **MobSvcLinux**.
2. Logga in tooyour konfigurationsservern och öppna en administrativ kommandotolk.
3. Kör följande kommandon toogenerate en lösenfras fil hello:

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. Kopiera hello **MobSvc.passphrase** filen till hello **MobSvcLinux** mapp på nätverksresursen.
5. Bläddra toohello installer databasen på hello konfigurationsservern genom att köra hello-kommando:

   `cd %ProgramData%\ASR\home\svsystems\puhsinstallsvc\repository`

6. Kopiera hello följande filer toohello **MobSvcLinux** mapp på nätverksresursen:
   * Microsoft ASR\_UA\*RHEL6 64*release.tar.gz
   * Microsoft ASR\_UA\*RHEL7 64\*release.tar.gz
   * Microsoft ASR\_UA\*SLES11-SP3-64\*release.tar.gz
   * Microsoft ASR\_UA\*SLES11-SP4-64\*release.tar.gz
   * Microsoft ASR\_UA\*OL6 64\*release.tar.gz
   * Microsoft ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz


7. Kopiera följande kod hello och spara den som **install_linux.sh** till hello **MobSvcLinux** mapp.
   > [!NOTE]
   > Ersätt hello [CSIP] platshållare i det här skriptet med hello faktiska värden för hello configuration-serverns IP-adress.

```Bash
#!/usr/bin/env bash

rm -rf /tmp/MobSvc
mkdir -p /tmp/MobSvc
INSTALL_DIR='/usr/local/ASR'
VX_VERSION_FILE='/usr/local/.vx_version'

echo "=============================" >> /tmp/MobSvc/sccm.log
echo `date` >> /tmp/MobSvc/sccm.log
echo "=============================" >> /tmp/MobSvc/sccm.log

if [ -f /etc/oracle-release ] && [ -f /etc/redhat-release ]; then
    if grep -q 'Oracle Linux Server release 6.*' /etc/oracle-release; then
        if uname -a | grep -q x86_64; then
            OS="OL6-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *OL6*.tar.gz /tmp/MobSvc
        fi
    fi
elif [ -f /etc/redhat-release ]; then
    if grep -q 'Red Hat Enterprise Linux Server release 6.* (Santiago)' /etc/redhat-release || \
        grep -q 'CentOS Linux release 6.* (Final)' /etc/redhat-release || \
        grep -q 'CentOS release 6.* (Final)' /etc/redhat-release; then
        if uname -a | grep -q x86_64; then
            OS="RHEL6-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *RHEL6*.tar.gz /tmp/MobSvc
        fi
    elif grep -q 'Red Hat Enterprise Linux Server release 7.* (Maipo)' /etc/redhat-release || \
        grep -q 'CentOS Linux release 7.* (Core)' /etc/redhat-release; then
        if uname -a | grep -q x86_64; then
            OS="RHEL7-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *RHEL7*.tar.gz /tmp/MobSvc
                fi
    fi
elif [ -f /etc/SuSE-release ] && grep -q 'VERSION = 11' /etc/SuSE-release; then
    if grep -q "SUSE Linux Enterprise Server 11" /etc/SuSE-release && grep -q 'PATCHLEVEL = 3' /etc/SuSE-release; then
        if uname -a | grep -q x86_64; then
            OS="SLES11-SP3-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *SLES11-SP3*.tar.gz /tmp/MobSvc
        fi
    elif grep -q "SUSE Linux Enterprise Server 11" /etc/SuSE-release && grep -q 'PATCHLEVEL = 4' /etc/SuSE-release; then
        if uname -a | grep -q x86_64; then
            OS="SLES11-SP4-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *SLES11-SP4*.tar.gz /tmp/MobSvc
        fi
    fi
elif [ -f /etc/lsb-release ] ; then
    if grep -q 'DISTRIB_RELEASE=14.04' /etc/lsb-release ; then
       if uname -a | grep -q x86_64; then
           OS="UBUNTU-14.04-64"
           echo $OS >> /tmp/MobSvc/sccm.log
           cp *UBUNTU-14*.tar.gz /tmp/MobSvc
       fi
    fi
else
    exit 1
fi

if [ -z "$OS" ]; then
    exit 1
fi

Install()
{
    echo "Perform Installation." >> /tmp/MobSvc/sccm.log
    ./install -q -d ${INSTALL_DIR} -r MS -v VmWare
    RET_VAL=$?
    echo "Installation Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Installation has succeeded. Proceed tooconfiguration." >> /tmp/MobSvc/sccm.log
        Configure
    else
        echo "Installation has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

Configure()
{
    echo "Perform configuration." >> /tmp/MobSvc/sccm.log
    ${INSTALL_DIR}/Vx/bin/UnifiedAgentConfigurator.sh -i [CSIP] -P MobSvc.passphrase
    RET_VAL=$?
    echo "Configuration Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Configuration has succeeded." >> /tmp/MobSvc/sccm.log
    else
        echo "Configuration has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

Upgrade()
{
    echo "Perform Upgrade." >> /tmp/MobSvc/sccm.log
    ./install -q -v VmWare
    RET_VAL=$?
    echo "Upgrade Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Upgrade has succeeded." >> /tmp/MobSvc/sccm.log
    else
        echo "Upgrade has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

cp MobSvc.passphrase /tmp/MobSvc
cd /tmp/MobSvc

tar -zxvf *.tar.gz

if [ -e ${VX_VERSION_FILE} ]; then
    echo "${VX_VERSION_FILE} exists. Checking for configuration status." >> /tmp/MobSvc/sccm.log
    agent_configuration=$(grep ^AGENT_CONFIGURATION_STATUS "${VX_VERSION_FILE}" | cut -d"=" -f2 | tr -d " ")
    echo "agent_configuration=$agent_configuration" >> /tmp/MobSvc/sccm.log
     if [ "$agent_configuration" == "Succeeded" ]; then
        echo "Agent is already configured. Proceed tooUpgrade." >> /tmp/MobSvc/sccm.log
        Upgrade
    else
        echo "Agent is not configured. Proceed tooConfigure." >> /tmp/MobSvc/sccm.log
        Configure
    fi
else
    Install
fi

cd /tmp

```

### <a name="step-2-create-a-package"></a>Steg 2: Skapa ett paket

1. Logga in tooyour Configuration Manager-konsolen.
2. Bläddra för**programbibliotek** > **programhantering** > **paket**.
3. Högerklicka på **paket**, och välj **skapa paket**.
4. Ange värden för hello namn, beskrivning, tillverkare, språk och version.
5. Välj hello **det här paketet innehåller källfiler** kryssrutan.
6. Klicka på **Bläddra**, och välj hello nätverksresurs där hello installer lagras (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcLinux).

  ![Skärmbild av skapa paket och Program Guiden](./media/site-recovery-install-mobility-service-using-sccm/create_sccm_package-linux.png)

7. På hello **Välj hello typ av program som du vill toocreate** väljer **standardprogram**, och klicka på **nästa**.

  ![Skärmbild av skapa paket och Program Guiden](./media/site-recovery-install-mobility-service-using-sccm/sccm-standard-program.png)

8. På hello **ange information om det här standardprogrammet** anger hello följande indata, och klicka på **nästa**. (hello andra indata kan använda sina standardvärden.)

    | **Parameternamn** | **Värde** |
  |--|--|
  | Namn | Installera Microsoft Azure-Mobilitetstjänsten (Linux) |
  | Kommandorad | ./install_linux.SH |
  | Programmet kan köras | Om en användare är inloggad |

  ![Skärmbild av skapa paket och Program Guiden](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-linux.png)

9. På nästa sida i hello väljer **det här programmet kan köras på alla plattformar**.
  ![Skärmbild av skapa paket och Program Guiden](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2-linux.png)

10. toocomplete hello guiden klickar du på **nästa** två gånger.

> [!NOTE]
> hello skript stöder både nya installationer av Mobilitetstjänsten agenter och uppdaterar tooagents som redan är installerade.

### <a name="step-3-deploy-hello-package"></a>Steg 3: Distribuera hello-paket
1. Högerklicka på paketet i hello Configuration Manager-konsolen och välj **distribuera innehåll**.
  ![Skärmbild av Configuration Manager-konsolen](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)
2. Välj hello  **[distributionsplatser](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)**  på toowhich hello paket ska kopieras.
3. Fullständig hello guiden. hello-paketet och sedan startar replikerar toohello angivna distributionsplatser.
4. När hello paketdistribution görs, högerklicka på hello paketet och väljer **distribuera**.
  ![Skärmbild av Configuration Manager-konsolen](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)
5. Välj hello Linux Server enhetssamling som du skapade i avsnittet förutsättningar för hello som hello målsamlingen för distributionen.

  ![Guiden Distribuera programvara skärmbild](./media/site-recovery-install-mobility-service-using-sccm/sccm-select-target-collection-linux.png)

6. På hello **ange hello innehållsmål** väljer din **distributionsplatser**.
7. På hello **ange inställningar toocontrol hur programvaran distribueras** , se till att hello syfte är **krävs**.

  ![Guiden Distribuera programvara skärmbild](./media/site-recovery-install-mobility-service-using-sccm/sccm-deploy-select-purpose.png)

8. På hello **ange hello schemat för distributionen** anger du ett schema. Mer information finns i [schemaläggning paket](https://technet.microsoft.com/library/gg682178.aspx).
9. På hello **distributionsplatser** konfigurerar hello egenskaper enligt toohello behoven i ditt datacenter. Slutför guiden hello.

Mobilitetstjänsten installeras på hello Enhetssamling för Linux-servern, enligt toohello schema som du har konfigurerat.

## <a name="other-methods-tooinstall-mobility-service"></a>Andra metoder tooinstall Mobilitetstjänsten
Här följer några andra alternativ för att installera Mobilitetstjänsten:
* [Manuell Installation med hjälp av Användargränssnittet](http://aka.ms/mobsvcmanualinstall)
* [Manuell Installation med hjälp av kommandoraden](http://aka.ms/mobsvcmanualinstallcli)
* [Push-Installation med konfigurationsservern](http://aka.ms/pushinstall)
* [Automatisk Installation med hjälp av Azure Automation & Desired State Configuration](http://aka.ms/mobsvcdscinstall)

## <a name="uninstall-mobility-service"></a>Avinstallera Mobilitetstjänsten
Du kan skapa Configuration Manager paket toouninstall Mobilitetstjänsten. Använd följande skript toodo så hello:

```
Time /t >> C:\logfile.log
REM ==================================================
REM ==== Check if Mob Svc is already installed =======
REM ==== If not installed no operation required ========
REM ==== Else run uninstall command =====================
REM ==== {275197FC-14FD-4560-A5EB-38217F80CBD1} is ====
REM ==== guid for Mob Svc Installer ====================
whoami >> C:\logfile.log
NET START | FIND "InMage Scout Application Service"
IF  %ERRORLEVEL% EQU 1 (GOTO :INSTALL) ELSE GOTO :UNINSTALL
:NOOPERATION
                echo "No Operation Required." >> c:\logfile.log
                GOTO :ENDSCRIPT
:UNINSTALL
                echo "Uninstall" >> C:\logfile.log
                MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
:ENDSCRIPT

```

## <a name="next-steps"></a>Nästa steg
Du är nu redo för[Aktivera skydd](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-vmware-to-azure#step-6-replicate-applications) för dina virtuella datorer.
