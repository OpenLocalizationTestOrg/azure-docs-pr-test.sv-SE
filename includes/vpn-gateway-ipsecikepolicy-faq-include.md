### <a name="is-custom-ipsecike-policy-supported-on-all-azure-vpn-gateway-skus"></a>Stöds anpassade IPsec/IKE-principer på alla Azure VPN Gateway-SKU: er?
Anpassade IPsec/IKE-principer stöds på Azure **VpnGw1, VpnGw2, VpnGw3, Standard** och **HighPerformance** VPN-gatewayer. **Basic** SKU stöds inte.

### <a name="how-many-policies-can-i-specify-on-a-connection"></a>Hur många principer kan jag ställa in för en anslutning?
Du kan bara ange ***en*** principkombination för en viss anslutning.

### <a name="can-i-specify-a-partial-policy-on-a-connection-eg-only-ike-algorithms-but-not-ipsec"></a>Kan jag ange en partiell princip på en anslutning? (T.ex. endast IKE-algoritmer men inte IPsec)
Nej, du måste ange alla algoritmer och parametrar för både IKE (huvudläge) och IPsec (snabbläge). Partiell principspecifikationen tillåts inte.

### <a name="what-are-hello-algorithms-and-key-strengths-supported-in-hello-custom-policy"></a>Vad är hello algoritmer och viktiga fördelar som stöds i hello anpassad princip?
hello tabellen nedan visar hello stöds kryptografiska algoritmer och viktiga styrkor konfigureras av hello kunder. Du måste välja ett alternativ för varje fält.

| **IPsec/IKEv2**  | **Alternativ**                                                                   |
| ---              | ---                                                                           |
| IKEv2-kryptering | AES256, AES192, AES128, DES3, DES                                             |
| IKEv2 Integrity  | SHA384, SHA256, SHA1, MD5                                                     |
| DH-grupp         | DHGroup24, ECP384, ECP256, DHGroup14 (DHGroup2048), DHGroup2, DHGroup1, None |
| IPsec-kryptering | GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, None      |
| IPsec Integrity  | GCMAES256, GCMAES192, GCMAES128, SHA256, SHA1, MD5                            |
| PFS-grupp        | PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, None                              |
| QM SA-livstid   | Sekunder (heltal. **min. 300**/standard 27 000 sekunder)<br>Kilobyte (heltal. **min. 1024**/standard 102400000 kilobyte)           |
| Trafikväljare | UsePolicyBasedTrafficSelectors ($True/$False; default $False)                 |
|                  |                                                                               |

> [!IMPORTANT]
> 1. DHGroup2048 & PFS2048 är hello samma som Diffie-Hellman-grupp **14** i IKE och IPsec PFS. Se [Diffie-Hellman-grupper](#DH) utför mappningar för hello.
> 2. Du måste ange för GCMAES algoritmer hello samma GCMAES algoritm och nyckel längd för både IPSec-kryptering och integritet.
> 3. IKEv2 Main Säkerhetsassociation livslängd vara högst 28 800 sekunder på hello Azure VPN-gatewayer
> 4. QM SA-livslängder är valfria parametrar. Om inget har angetts används standardvärdena på 27 000 sekunder (7,5h) och 102 400 000 kilobyte (102 GB).
> 5. UsePolicyBasedTrafficSelector är en parameter av alternativet på hello-anslutning. Se hello nästa vanliga frågor och svar ”UsePolicyBasedTrafficSelectors”-objekt

### <a name="does-everything-need-toomatch-between-hello-azure-vpn-gateway-policy-and-my-on-premises-vpn-device-configurations"></a>Behöver allt toomatch mellan hello Azure VPN gateway-princip och min lokala VPN-konfigurationer för enheten?
Din lokala VPN-enhetens konfiguration måste stämma överens eller innehålla hello följande algoritmer och parametrar som du anger på hello Azure IPsec/IKE-principen:

* IKE-krypteringsalgoritm
* IKE-integritetsalgoritm
* DH-grupp
* IPsec-krypteringsalgoritm
* IPsec-integritetsalgoritm
* PFS-grupp
* Trafikväljare (*)

hello SA livslängd är lokala specifikationerna, behöver inte toomatch.

Om du aktiverar **UsePolicyBasedTrafficSelectors**, behöver du tooensure din VPN-enhet har hello matchar trafik väljare som definierats med alla kombinationer av lokalt nätverk (lokal nätverksgateway)-prefix till eller från hello Virtuella Azure-nätverksprefix i stället för alla-till-alla. Om din prefix för det lokala nätverket är 10.1.0.0/16 och 10.2.0.0/16 och din prefix för det virtuella nätverket är 192.168.0.0/16 och 172.16.0.0/16, måste till exempel toospecify hello följande trafik väljare:
* 10.1.0.0/16 <====> 192.168.0.0/16
* 10.1.0.0/16 <====> 172.16.0.0/16
* 10.2.0.0/16 <====> 192.168.0.0/16
* 10.2.0.0/16 <====> 172.16.0.0/16

Se för[ansluta flera lokala principbaserad VPN-enheter](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md) för mer information om hur toouse det här alternativet.

### <a name ="DH"></a>Vilka Diffie-Hellman-grupper stöds?
hello tabellen nedan visar hello stöds Diffie-Hellman-grupp för IKE (DHGroup) och IPsec (PFSGroup):

| **Diffie-Hellman-grupp**  | **DHGroup**              | **PFSGroup** | **Nyckellängd** |
| ---                       | ---                      | ---          | ---            |
| 1                         | DHGroup1                 | PFS1         | 768-bitars MODP   |
| 2                         | DHGroup2                 | PFS2         | 1024-bitars MODP  |
| 14                        | DHGroup14<br>DHGroup2048 | PFS2048      | 2048-bitars MODP  |
| 19                        | ECP256                   | ECP256       | 256-bitars ECP    |
| 20                        | ECP384                   | ECP284       | 384-bitars ECP    |
| 24                        | DHGroup24                | PFS24        | 2048-bitars MODP  |
|                           |                          |              |                |

Se för[RFC3526](https://tools.ietf.org/html/rfc3526) och [RFC5114](https://tools.ietf.org/html/rfc5114) för mer information.

### <a name="does-hello-custom-policy-replace-hello-default-ipsecike-policy-sets-for-azure-vpn-gateways"></a>Ersätter hello anpassad princip hello standard IPsec/IKE principen anger för Azure VPN-gatewayer?
Ja, när en anpassad princip har angetts på en anslutning, Azure VPN-gateway kommer endast att använda hello principen hello anslutning, både som IKE initierare och svarare för IKE.

### <a name="if-i-remove-a-custom-ipsecike-policy-does-hello-connection-become-unprotected"></a>Om jag tar bort en anpassad princip för IPsec/IKE hello anslutning blir oskyddade?
Nej, hello anslutningen fortfarande skyddas av IPsec/IKE. När du tar bort hello anpassad princip från en anslutning hello Azure VPN-gatewayen återställs tillbaka toohello [standardlistan med IPsec/IKE förslag](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md) och starta om hello IKE handskakning igen med din lokala VPN-enhet.

### <a name="would-adding-or-updating-an-ipsecike-policy-disrupt-my-vpn-connection"></a>Kan det störa VPN-anslutningen att lägga till eller uppdatera en IPsec/IKE-princip?
Ja, kan detta leda till en liten avbrott (några sekunder) som hello Azure VPN-gateway kommer går sönder ned hello befintlig anslutning och starta hello IKE handskakning toore-upprätta hello IPsec-tunneln med hello nya krypteringsalgoritmer och parametrar. Kontrollera att din lokala VPN-enhet konfigureras också med hello matchande algoritmer och viktiga styrkor toominimize hello avbrott.

### <a name="can-i-use-different-policies-on-different-connections"></a>Kan jag använda olika principer för olika anslutningar?
Ja. Anpassade principer tillämpats på per-anslutningbasis. Du kan skapa och tillämpa olika IPsec/IKE-principer på olika anslutningar. Du kan också välja tooapply anpassade principer för en delmängd av anslutningar. hello återstående använder hello Azure standard IPsec/IKE principen anger.

### <a name="can-i-use-hello-custom-policy-on-vnet-to-vnet-connection-as-well"></a>Kan jag använda hello anpassad princip på VNet-till-VNet-anslutningen?
Ja, du kan använda anpassade principen på både IPSec-anslutningar mellan lokala anslutningar eller VNet-till-VNet-anslutningar.

### <a name="do-i-need-toospecify-hello-same-policy-on-both-vnet-to-vnet-connection-resources"></a>Behöver jag toospecify hello samma princip på båda resurserna för VNet-till-VNet-anslutningen?
Ja. En VNet-till-VNet-tunnel består av två anslutningsresurser i Azure, en för varje riktning. Du behöver tooensure båda anslutningsresurserna har hello samma princip kommer inte att upprätta en othereise hello VNet-till-VNet-anslutningen.

### <a name="does-custom-ipsecike-policy-work-on-expressroute-connection"></a>Fungerar en anpassad IPsec/IKE-princip på ExpressRoute-anslutningen?
Nej. IPSec-/ princip fungerar bara på S2S VPN- och VNet-till-VNet-anslutningar via hello Azure VPN-gatewayer.
