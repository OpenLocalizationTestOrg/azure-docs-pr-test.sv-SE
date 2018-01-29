---
title: Azure Media Services metadata utdataschema | Microsoft Docs
description: "Avsnittet ger en översikt över Azure Media Services utdataschema metadata."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 1ed84c88-eea5-4a24-9c4f-f2428157d08a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: juliako
ms.openlocfilehash: c8792535eeeb71e7233c42bd9ea2a446a1c4d43c
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/08/2018
---
# <a name="output-metadata"></a>Metadata för utdata
## <a name="overview"></a>Översikt
Ett kodningsjobb är associerad med en inkommande tillgång (eller tillgångar) om du vill utföra vissa uppgifter som kodning. Koda till exempel en MP4-fil att H.264 MP4 med anpassad bithastighet anger; Skapa en miniatyr; Skapa överlägg. En utdatatillgången skapas vid slutförandet av uppgiften.  Utdatatillgången innehåller video, ljud, miniatyrer, osv. Utdatatillgången innehåller också en fil med metadata om utdatatillgången. Namnet på XML-metadatafilen har följande format: &lt;source_file_name&gt;_manifest.xml (till exempel BigBuckBunny_manifest.xml).  

Om du vill undersöka metadatafilen kan du skapa en **SAS** positionerare och hämta filen till den lokala datorn.  

Den här artikeln beskriver de element och typer av XML-schemat som utdata metada (&lt;source_file_name&gt;_manifest.xml) är baserad. Information om den fil som innehåller metadata om inkommande tillgången, finns [inkommande Metadata](media-services-input-metadata-schema.md).  

> [!NOTE]
> Du kan hitta fullständigt schema koden och XML-exempel i slutet av den här artikeln.  
>
>

## <a name="AssetFiles "></a>Rotelementet för AssetFiles
Samling av AssetFile poster för kodningsjobbet.  

### <a name="child-elements"></a>Underordnade element
| Namn | Beskrivning |
| --- | --- |
| **AssetFile**<br/><br/> minOccurs = ”0” maxOccurs = ”1” |En [AssetFile elementet](media-services-output-metadata-schema.md) som ingår i samlingen AssetFiles. |

## <a name="AssetFile "></a>AssetFile element
Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).  

### <a name="attributes"></a>Attribut
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **Namn**<br/><br/> Krävs |**Xs:String** |Filnamnet för media tillgången. |
| **Storlek**<br/><br/> minInclusive = ”0”<br/><br/> Krävs |**Xs:Long** |Storleken på resursfilen i byte. |
| **Varaktighet**<br/><br/> Krävs |**Duration** |Innehåll play tillbaka varaktighet. |

### <a name="child-elements"></a>Underordnade element
| Namn | Beskrivning |
| --- | --- |
| **Datakällor** |Samling av indatakälla/mediefiler som bearbetades för att skapa den här AssetFile. Mer information finns i [källelementet](media-services-output-metadata-schema.md). |
| **VideoTracks**<br/><br/> minOccurs = ”0” maxOccurs = ”1” |Varje fysisk AssetFile i den kan innehålla noll eller fler videor spårar överlagrat till en lämplig behållare format. Mer information finns i [VideoTracks elementet](media-services-output-metadata-schema.md). |
| **AudioTracks**<br/><br/> minOccurs = ”0” maxOccurs = ”1” |Varje fysisk AssetFile kan innehålla noll eller flera ljud spårar överlagrat till en lämplig behållare format i den. Detta är en samling av alla ljud spår. Mer information finns i [AudioTracks elementet](media-services-output-metadata-schema.md). |

## <a name="Sources "></a>Källor element
Samling av indatakälla/mediefiler som bearbetades för att skapa den här AssetFile.  

Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).  

### <a name="child-elements"></a>Underordnade element
| Namn | Beskrivning |
| --- | --- |
| **Källa**<br/><br/> minOccurs = ”1” maxOccurs = ”unbounded” |En indata/källfilerna som används vid generering av tillgången. Mer information finns i [källelementet](media-services-output-metadata-schema.md). |

## <a name="Source "></a>Källelementet
En indata/källfilerna som används vid generering av tillgången.  

Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).  

### <a name="attributes"></a>Attribut
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **Namn**<br/><br/> Krävs |**Xs:String** |Indatakällan filnamn. |

## <a name="VideoTracks "></a>VideoTracks element
Varje fysisk AssetFile i den kan innehålla noll eller fler videor spårar överlagrat till en lämplig behållare format. Den **VideoTracks** elementet representerar en samling av alla video spår.  

Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).  

### <a name="child-elements"></a>Underordnade element
| Namn | Beskrivning |
| --- | --- |
| **VideoTrack**<br/><br/> minOccurs = ”1” maxOccurs = ”unbounded” |En specifik video spåra i överordnat AssetFile. Mer information finns i [VideoTrack elementet](media-services-output-metadata-schema.md#VideoTrack). |

## <a name="VideoTrack"></a>VideoTrack element
En specifik video spåra i överordnat AssetFile.  

Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).  

### <a name="attributes"></a>Attribut
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **ID**<br/><br/> minInclusive = ”0”<br/><br/> Krävs |**Xs:int** |Nollbaserade indexet för den här videon spåra. **Obs:** detta **Id** är inte nödvändigtvis TrackID som används i en MP4-fil. |
| **FourCC**<br/><br/> Krävs |**Xs:String** |Video-codec FourCC-kod. |
| **Profil** |**Xs:String** |H264-profil (gäller endast för H264 codec). |
| **Nivå** |**Xs:String** |H264 nivå (gäller endast för H264 codec). |
| **Bredd**<br/><br/> minInclusive = ”0”<br/><br/> Krävs |**Xs:int** |Kodad video bredd i bildpunkter. |
| **Höjd**<br/><br/> minInclusive = ”0”<br/><br/> Krävs |**Xs:int** |Kodad video höjd i bildpunkter. |
| **DisplayAspectRatioNumerator**<br/><br/> minInclusive = ”0”<br/><br/> Krävs |**Xs:Double** |Videoenhet proportionerna täljaren. |
| **DisplayAspectRatioDenominator**<br/><br/> minInclusive = ”0”<br/><br/> Krävs |**Xs:Double** |Videoenhet proportionerna nämnare. |
| **Ramhastighet**<br/><br/> minInclusive = ”0”<br/><br/> Krävs |**Xs:decimal** |Mätt video bildfrekvens .3f format. |
| **TargetFramerate**<br/><br/> minInclusive = ”0”<br/><br/> Krävs |**Xs:decimal** |Förinställda mål video bildfrekvens .3f format. |
| **Bithastighet**<br/><br/> minInclusive = ”0”<br/><br/> Krävs |**Xs:int** |Genomsnittlig video bithastighet i kilobit per sekund som beräknas från AssetFile. Räknar elementära dataströmmen nyttolasten och inkluderar inte paketering kostnader. |
| **TargetBitrate**<br/><br/> minInclusive = ”0”<br/><br/> Krävs |**Xs:int** |Rikta genomsnittlig bithastighet för den här videon spår som begärdes via encoding förinställt i kilobit per sekund. |
| **MaxGOPBitrate**<br/><br/> minInclusive = ”0” |**Xs:int** |Max GOP genomsnittlig bithastighet för den här videon spår i kilobit per sekund. |

## <a name="AudioTracks "></a>AudioTracks element
Varje fysisk AssetFile kan innehålla noll eller flera ljud spårar överlagrat till en lämplig behållare format i den. Den **AudioTracks** elementet representerar en samling av alla ljud spår.  

Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).  

### <a name="child-elements"></a>Underordnade element
| Namn | Beskrivning |
| --- | --- |
| **AudioTrack**<br/><br/> minOccurs = ”1” maxOccurs = ”unbounded” |En specifik ljud spåra i överordnat AssetFile. Mer information finns i [AudioTrack elementet](media-services-output-metadata-schema.md). |

## <a name="AudioTrack "></a>AudioTrack element
En specifik ljud spåra i överordnat AssetFile.  

Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).  

### <a name="attributes"></a>Attribut
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **ID**<br/><br/> minInclusive = ”0”<br/><br/> Krävs |**Xs:int** |Nollbaserade indexet för den här ljud spåra. **Obs:** detta är inte nödvändigtvis TrackID som används i en MP4-fil. |
| **Codec** |**Xs:String** |Ljud spåra codec sträng. |
| **EncoderVersion** |**Xs:String** |Versionssträng valfria kodare som krävs för EAC3. |
| **Kanaler**<br/><br/> minInclusive = ”0”<br/><br/> Krävs |**Xs:int** |Antal ljud kanaler. |
| **SamplingRate**<br/><br/> minInclusive = ”0”<br/><br/> Krävs |**Xs:int** |I prover per sekund eller Hz ljud samplingsfrekvensen. |
| **Bithastighet**<br/><br/> minInclusive = ”0”<br/><br/> Krävs |**Xs:int** |Genomsnittlig ljud bithastighet i bitar per sekund som beräknas från AssetFile. Räknar elementära dataströmmen nyttolasten och inkluderar inte paketering kostnader. |
| **BitsPerSample**<br/><br/> minInclusive = ”0”<br/><br/> Krävs |**Xs:int** |Bitar per prov för formatet wFormatTag typ. |

### <a name="child-elements"></a>Underordnade element
| Namn | Beskrivning |
| --- | --- |
| **LoudnessMeteringResultParameters**<br/><br/> minOccurs = ”0” maxOccurs = ”1” |Loudness avläsning resultatet parametrar. Mer information finns i [LoudnessMeteringResultParameters elementet](media-services-output-metadata-schema.md). |

## <a name="LoudnessMeteringResultParameters "></a>LoudnessMeteringResultParameters element
Loudness avläsning resultatet parametrar.  

Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).  

### <a name="attributes"></a>Attribut
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **DPLMVersionInformation** |**Xs:String** |**Dolby** professional loudness avläsning development kit version. |
| **DialogNormalization**<br/><br/> minInclusive = ”-31” maxInclusive = ”-1”<br/><br/> Krävs |**Xs:int** |DialogNormalization genereras genom DPLM, krävs när LoudnessMetering har angetts |
| **IntegratedLoudness**<br/><br/> minInclusive = ”-70” maxInclusive = ”10”<br/><br/> Krävs |**Xs:float** |Integrerad loudness |
| **IntegratedLoudnessUnit**<br/><br/> Krävs |**Xs:String** |Integrerad loudness enhet. |
| **IntegratedLoudnessGatingMethod**<br/><br/> Krävs |**Xs:String** |Gating identifierare |
| **IntegratedLoudnessSpeechPercentage**<br/><br/> minInclusive = ”0” maxInclusive = ”100” |**Xs:float** |Taligenkänning innehåll via programmet, i procent. |
| **SamplePeak**<br/><br/> Krävs |**Xs:float** |Belastning absolut exempelvärde avmarkerad sedan återställning eller sedan den startades senast per kanal.  Enheterna är dBFS. |
| **SamplePeakUnit**<br/><br/> fast = ”dBFS”<br/><br/> Krävs |**Xs:anySimpleType** |Exempel resursbehov. |
| **TruePeak**<br/><br/> Krävs |**Xs:float** |Maximala true högsta värde, enligt ITU-R BS.1770-2, sedan återställning eller sedan den startades senast avmarkerad per kanal. Enheterna är dBTP. |
| **TruePeakUnit**<br/><br/> fast = ”dBTP”<br/><br/> Krävs |**Xs:anySimpleType** |True resursbehov. |

## <a name="schema-code"></a>Schemat kod
    <?xml version="1.0" encoding="utf-8"?>  
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata" version="1.2"  
               xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2013/05/mediaencoder/metadata"  
               targetNamespace="http://schemas.microsoft.com/windowsazure/mediaservices/2013/05/mediaencoder/metadata"  
               elementFormDefault="qualified">  
      <xs:element name="AssetFiles">  
        <xs:annotation>  
          <xs:documentation>Collection of AssetFile entries for the encoding job</xs:documentation>  
        </xs:annotation>  
        <xs:complexType>  
          <xs:sequence>  
            <xs:element name="AssetFile" minOccurs="1" maxOccurs="unbounded">  
              <xs:annotation>  
                <xs:documentation>asset file</xs:documentation>  
              </xs:annotation>  
              <xs:complexType>  
                <xs:sequence>  
                  <xs:element name="Sources">  
                    <xs:annotation>  
                      <xs:documentation>Collection of input/source media files, that was processed in order to produce this AssetFile</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="Source" minOccurs="1" maxOccurs="unbounded">  
                          <xs:annotation>  
                            <xs:documentation>An input/source file used when generating this asset</xs:documentation>  
                          </xs:annotation>  
                          <xs:complexType>  
                            <xs:attribute name="Name" type="xs:string" use="required">  
                              <xs:annotation>  
                                <xs:documentation>input source file name</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                          </xs:complexType>  
                        </xs:element>  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="VideoTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>Each physical AssetFile can contain in it zero or more video tracks interleaved into an appropriate container format. This is the collection of all those video tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="VideoTrack" maxOccurs="unbounded">  
                          <xs:annotation>  
                            <xs:documentation>A specific video track in the parent AssetFile</xs:documentation>  
                          </xs:annotation>  
                          <xs:complexType>  
                            <xs:attribute name="Id" use="required">  
                              <xs:annotation>  
                                <xs:documentation>zero-based index of this video track. Note: this is not necessarily the TrackID as used in an MP4 file</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="FourCC" type="xs:string" use="required">  
                              <xs:annotation>  
                                <xs:documentation>video codec FourCC code</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="Profile" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>H264 profile (only appliable for H264 codec)</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="Level" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>H264 level (only appliable for H264 codec)</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="Width" use="required">  
                              <xs:annotation>  
                                <xs:documentation>encoded video width in pixels</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="Height" use="required">  
                              <xs:annotation>  
                                <xs:documentation>encoded video height in pixels</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="DisplayAspectRatioNumerator" use="required">  
                              <xs:annotation>  
                                <xs:documentation>video display aspect ratio numerator</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:double">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="DisplayAspectRatioDenominator" use="required">  
                              <xs:annotation>  
                                <xs:documentation>video display aspect ratio denominator</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:double">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="Framerate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>measured video frame rate in .3f format</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:decimal">  
                                  <xs:minInclusive value="0"/>  
                                  <xs:fractionDigits value="3"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="TargetFramerate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>preset target video frame rate in .3f format</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:decimal">  
                                  <xs:minInclusive value="0"/>  
                                  <xs:fractionDigits value="3"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="Bitrate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>average video bit rate in kilobits per second, as calculated from the AssetFile. Counts only the elementary stream payload, and does not include the packaging overhead</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="TargetBitrate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>target average bitrate for this video track, as requested via the encoding preset, in kilobits per second</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="MaxGOPBitrate">  
                              <xs:annotation>  
                                <xs:documentation>Max GOP average bitrate for this video track, in kilobits per second</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                          </xs:complexType>  
                        </xs:element>  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="AudioTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>each physical AssetFile can contain in it zero or more audio tracks interleaved into an appropriate container format. This is the collection of all those audio tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="AudioTrack" maxOccurs="unbounded">  
                          <xs:annotation>  
                            <xs:documentation>a specific audio track in the parent AssetFile</xs:documentation>  
                          </xs:annotation>  
                          <xs:complexType>  
                            <xs:sequence>  
                              <xs:element name="LoudnessMeteringResultParameters" minOccurs="0" maxOccurs="1">  
                                <xs:annotation>  
                                  <xs:documentation>Loudness Metering Result Parameters</xs:documentation>  
                                </xs:annotation>  
                                <xs:complexType>  
                                  <xs:attribute name="DPLMVersionInformation" type="xs:string">  
                                    <xs:annotation>  
                                      <xs:documentation>Dolby Professional Loudness Metering Development Kit Version</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="DialogNormalization" use="required">  
                                    <xs:annotation>  
                                      <xs:documentation> DialogNormalization generated through DPLM, required when LoudnessMetering is set</xs:documentation>  
                                    </xs:annotation>  
                                    <xs:simpleType>  
                                      <xs:restriction base="xs:int">  
                                        <xs:minInclusive value="-31"/>  
                                        <xs:maxInclusive value="-1"/>  
                                      </xs:restriction>  
                                    </xs:simpleType>  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudness" use="required">  
                                    <xs:annotation>  
                                      <xs:documentation>Integrated loudness</xs:documentation>  
                                    </xs:annotation>  
                                    <xs:simpleType>  
                                      <xs:restriction base="xs:float">  
                                        <xs:minInclusive value="-70"/>  
                                        <xs:maxInclusive value="10"/>  
                                      </xs:restriction>  
                                    </xs:simpleType>  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudnessUnit" use="required" type="xs:string">  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudnessGatingMethod" use="required" type="xs:string">  
                                    <xs:annotation>  
                                      <xs:documentation>Gating identifier</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudnessSpeechPercentage">  
                                    <xs:annotation>  
                                      <xs:documentation>Speech content over the program, as a percentage.</xs:documentation>  
                                    </xs:annotation>  
                                    <xs:simpleType>  
                                      <xs:restriction base="xs:float">  
                                        <xs:minInclusive value="0"/>  
                                        <xs:maxInclusive value="100"/>  
                                      </xs:restriction>  
                                    </xs:simpleType>  
                                  </xs:attribute>  
                                  <xs:attribute name="SamplePeak" use="required" type="xs:float">  
                                    <xs:annotation>  
                                      <xs:documentation>Peak absolute sample value, since reset or since it was last cleared, per channel.  Units are dBFS.</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="SamplePeakUnit" use="required" fixed="dBFS">  
                                  </xs:attribute>  
                                  <xs:attribute name="TruePeak" use="required" type="xs:float">  
                                    <xs:annotation>  
                                      <xs:documentation>Maximum True Peak value, as per ITU-R BS.1770-2, since reset or since it was last cleared, per channel.  Units are dBTP.</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="TruePeakUnit" use="required" fixed="dBTP">  
                                  </xs:attribute>  
                                </xs:complexType>  
                              </xs:element>  
                            </xs:sequence>  
                            <xs:attribute name="Id" use="required">  
                              <xs:annotation>  
                                <xs:documentation>zero-based index of this audio track. Note: this is not necessarily the TrackID as used in an MP4 file</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="Codec" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>audio track codec string</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="EncoderVersion" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>optional encoder version string, required for EAC3</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="Channels" use="required">  
                              <xs:annotation>  
                                <xs:documentation>number of audio channels</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="SamplingRate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>audio sampling rate in samples/sec or Hz</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="Bitrate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>average audio bit rate in bits per second, as calculated from the AssetFile. Counts only the elementary stream payload, and does not include the packaging overhead</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="BitsPerSample" use="required">  
                              <xs:annotation>  
                                <xs:documentation>Bits per sample for the wFormatTag format type</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                          </xs:complexType>  
                        </xs:element>  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                </xs:sequence>  
                <xs:attribute name="Name" type="xs:string" use="required">  
                  <xs:annotation>  
                    <xs:documentation>the media asset file name</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="Size" use="required">  
                  <xs:annotation>  
                    <xs:documentation>size of file in bytes</xs:documentation>  
                  </xs:annotation>  
                  <xs:simpleType>  
                    <xs:restriction base="xs:long">  
                      <xs:minInclusive value="0"/>  
                    </xs:restriction>  
                  </xs:simpleType>  
                </xs:attribute>  
                <xs:attribute name="Duration" use="required">  
                  <xs:annotation>  
                    <xs:documentation>content play back duration</xs:documentation>  
                  </xs:annotation>  
                  <xs:simpleType>  
                    <xs:restriction base="xs:duration"/>  
                  </xs:simpleType>  
                </xs:attribute>  
              </xs:complexType>  
            </xs:element>  
          </xs:sequence>  
        </xs:complexType>  
      </xs:element>  
    </xs:schema>  



## <a name="xml"></a>XML-exempel

Följande XML är ett exempel på utdata-metadatafil.  

    <AssetFiles xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
                xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2013/05/mediaencoder/metadata">  
      <AssetFile Name="BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4" Size="4646283" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.2" Width="1280" Height="720" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="4250" TargetBitrate="3400" MaxGOPBitrate="5514"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4" Size="3166728" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.1" Width="960" Height="540" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="2846" TargetBitrate="2250" MaxGOPBitrate="3630"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4" Size="2205095" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.1" Width="960" Height="540" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="1932" TargetBitrate="1500" MaxGOPBitrate="2513"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4" Size="1508567" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.0" Width="640" Height="360" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="1271" TargetBitrate="1000" MaxGOPBitrate="1527"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4" Size="1057155" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.0" Width="640" Height="360" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="843" TargetBitrate="650" MaxGOPBitrate="1086"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4" Size="699262" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="1.3" Width="320" Height="180" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="503" TargetBitrate="400" MaxGOPBitrate="661"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_AAC_und_ch2_96kbps.mp4" Size="166780" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_AAC_und_ch2_56kbps.mp4" Size="124576" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="53" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
    </AssetFiles>  

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
