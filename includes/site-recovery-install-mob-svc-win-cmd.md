1. Kopiera hello installer tooa lokal mapp (exempelvis C:\Temp) på hello-server som du vill tooprotect. Kör följande kommandon som administratör i Kommandotolken hello:

  ```
  cd C:\Temp
  ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
  MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
  cd C:\Temp\Extracted.
  ```
2. tooinstall Mobilitetstjänsten, kör hello följande kommando:

  ```
  UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
  ```
3. Hello-agenten måste nu toobe som registrerats med hello konfigurationsservern.

  ```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe”  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```

#### <a name="mobility-service-installer-command-line-arguments"></a>Tjänsten Mobility installer kommandoradsargument

```
Usage :
UnifiedAgent.exe /Role <MS|MT> /InstallLocation <Install Location> /Platform “VmWare” /Silent
```

| Parameter|Typ|Beskrivning|Möjliga värden|
|-|-|-|-|
|/ Roll|Obligatorisk|Anger om Mobility Service (MS) som ska installeras eller MasterTarget(MT) ska installeras|MS </br> HUVUDMÅLSERVERN|
|/InstallLocation|Valfri|Plats där Mobilitetstjänsten är installerad|En mapp på hello-dator|
|/ Platform|Obligatorisk|Anger hello plattform på vilken hello Mobilitetstjänsten komma installeras </br> </br>- **VMware** : Använd det här värdet om du installerar mobilitetstjänsten på en virtuell dator som körs på *vSphere VMware ESXi-värdar*, *Hyper-V-värdar* och *Phsyical servrar* </br> - **Azure** : Använd det här värdet om du installerar agenten på en Azure IaaS-VM| VMware </br> Azure|
|/ Tyst|Valfri|Anger toorun hello installer i tyst läge| Ej tillämpligt|

>[!TIP]
> hello installationsloggarna finns under %ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log

#### <a name="mobility-service-registration-command-line-arguments"></a>Tjänsten Mobility registrering kommandoradsargument

```
Usage :
UnifiedAgentConfigurator.exe”  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
```

  | Parameter|Typ|Beskrivning|Möjliga värden|
  |-|-|-|-|
  |/ CSEndPoint |Obligatorisk|IP-adress för konfigurationsservern hello| En giltig IP-adress|
  |/PassphraseFilePath|Obligatorisk|Platsen för hello lösenfras |En giltig UNC- eller lokal filsökväg|


>[!TIP]
> Hej AgentConfiguration loggar finns under %ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log
