1. Kopiera hello installer tooa lokal mapp (till exempel /tmp) på hello-server som du vill tooprotect. Kör följande kommandon hello i en terminal:
  ```
  cd /tmp
  tar -xvzf Microsoft-ASR_UA*release.tar.gz
  ```
2. tooinstall Mobilitetstjänsten, kör hello följande kommando:

  ```
  sudo ./install -d <Install Location> -r MS -v VmWare -q
  ```
3. När installationen är klar måste hello Mobilitetstjänsten tooget registrerade toohello konfigurationsservern. Kör följande kommando tooregister hello Mobilitetstjänsten med konfigurationsservern hello.

  ```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
  ```

#### <a name="mobility-service-installer-command-line"></a>Mobilitetstjänstens installationsprogram kommandoraden

```
Usage:
./install -d <Install Location> -r <MS|MT> -v VmWare -q
```

|Parameter|Typ|Beskrivning|Möjliga värden|
|-|-|-|-|
|-r |Obligatorisk|Anger om Mobility Service (MS) som ska installeras eller MasterTarget(MT) ska installeras|MS </br> HUVUDMÅLSERVERN|
|-d |Valfri|Plats där Mobilitetstjänsten kommer att installeras|/usr/local/ASR|
|-v|Obligatorisk|Anger hello plattform på vilken hello Mobilitetstjänsten komma installeras </br> </br>- **VMware** : Använd det här värdet om du installerar mobilitetstjänsten på en virtuell dator som körs på *vSphere VMware ESXi-värdar*, *Hyper-V-värdar* och *Phsyical servrar* </br> - **Azure** : Använd det här värdet om du installerar agenten på en Azure IaaS-VM| VMware </br> Azure|
|-q|Valfri|Anger toorun installer i tyst läge| Saknas|


#### <a name="mobility-service-configuration-command-line"></a>Mobility tjänstkonfiguration kommandoraden

```
Usage:
cd /usr/local/ASR/Vx/bin
UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
```

|Parameter|Typ|Beskrivning|Möjliga värden|
|-|-|-|-|
|-i |Obligatorisk|Hello konfigurationsservern IP|Vilken giltig IP-adress som helst|
|-P |Obligatorisk|Fullständig sökväg hello fil där hello anslutning lösenfras sparas|En giltig mapp|
