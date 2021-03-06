---
title: Meta gegevens schema voor uitvoer Azure Media Services | Microsoft Docs
description: Dit artikel bevat een overzicht van Azure Media Services schema voor uitvoer van meta gegevens.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 1ed84c88-eea5-4a24-9c4f-f2428157d08a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 3f0c6b60e2be625d1f869c3eda4acb9dfd3c6e9e
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74886809"
---
# <a name="output-metadata"></a>Uitvoer meta gegevens
## <a name="overview"></a>Overzicht
Een coderings taak is gekoppeld aan een invoer element (of activa) waarvoor u bepaalde coderings taken wilt uitvoeren. U kunt bijvoorbeeld een MP4-bestand coderen naar H. 264 MP4 Adaptive bitrate sets; Maak een miniatuur. Maak overlays. Wanneer een taak is voltooid, wordt een uitvoer activum geproduceerd.  Het uitvoer element bevat video, audio, miniaturen, enzovoort. Het uitvoer activum bevat ook een bestand met meta gegevens over het uitvoer element. De naam van het XML-bestand met meta gegevens heeft de volgende indeling: &lt;source_file_name&gt;_manifest. XML (bijvoorbeeld BigBuckBunny_manifest. XML).  

Media Services voert geen bekend in voor het genereren van meta gegevens. Invoer meta gegevens worden alleen gegenereerd als een artefact wanneer een invoer element in een taak wordt verwerkt. Dit artefact wordt daarom geschreven naar de uitvoer Asset. Er worden verschillende hulpprogram ma's gebruikt voor het genereren van meta gegevens voor invoer assets en uitvoer assets. Daarom heeft de invoer meta gegevens een iets ander schema dan de uitvoer meta gegevens.

Als u het meta gegevensbestand wilt controleren, kunt u een **SAS** -Locator maken en het bestand downloaden naar uw lokale computer.  

In dit artikel worden de elementen en typen beschreven van het XML-schema waarop de uitvoer-metada (&lt;source_file_name&gt;_manifest. XML) is gebaseerd. Zie invoer van meta gegevens voor informatie over het bestand dat meta gegevens bevat over het invoer element.  

Aan het eind van dit artikel vindt u de volledige schema code en het XML-voor beeld.  

## <a name="AssetFiles"></a>AssetFiles-hoofd element
Verzameling van AssetFile-vermeldingen voor de coderings taak.  

### <a name="child-elements"></a>Onderliggende elementen
| Naam | Beschrijving |
| --- | --- |
| **AssetFile**<br/><br/> minOccurs="0" maxOccurs="1" |Een AssetFile-element dat deel uitmaakt van de AssetFiles-verzameling. |

## <a name="AssetFile"></a>AssetFile-element
U kunt een XML-voor beeld [-XML-voor](#xml)beeld vinden.  

### <a name="attributes"></a>Kenmerken
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Naam**<br/><br/> Verplicht |**XS: teken reeks** |De bestands naam voor het Media-activum. |
| **Grootte**<br/><br/> minInclusive = "0"<br/><br/> Verplicht |**XS: Long** |Grootte van het activa bestand in bytes. |
| **Hebben**<br/><br/> Verplicht |**XS: duration** |Duur van het afspelen van inhoud. |

### <a name="child-elements"></a>Onderliggende elementen
| Naam | Beschrijving |
| --- | --- |
| **Beperken** |Verzameling van invoer/bron media bestanden, die zijn verwerkt om deze AssetFile te maken. Zie bron element voor meer informatie. |
| **VideoTracks**<br/><br/> minOccurs="0" maxOccurs="1" |Elk fysiek AssetFile kan in de waarde nul bevatten of meer Video's worden getraceerd in een geschikte container indeling. Zie het element VideoTracks voor meer informatie. |
| **AudioTracks**<br/><br/> minOccurs="0" maxOccurs="1" |Elk fysiek AssetFile kan in de waarde nul of meer audio tracks Interleaved in een geschikte container indeling bevatten. Dit is de verzameling van al deze audio sporen. Zie het element AudioTracks voor meer informatie. |

## <a name="Sources"></a>Bron element
Verzameling van invoer/bron media bestanden, die zijn verwerkt om deze AssetFile te maken.  

U kunt een XML-voor beeld [-XML-voor](#xml)beeld vinden.  

### <a name="child-elements"></a>Onderliggende elementen
| Naam | Beschrijving |
| --- | --- |
| **Bron**<br/><br/> minOccurs = "1" maxOccurs = "ontgrensd" |Een invoer-of bron bestand dat wordt gebruikt bij het genereren van deze asset. Zie bron element voor meer informatie. |

## <a name="Source"></a>Bron element
Een invoer-of bron bestand dat wordt gebruikt bij het genereren van deze asset.  

U kunt een XML-voor beeld [-XML-voor](#xml)beeld vinden.  

### <a name="attributes"></a>Kenmerken
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Naam**<br/><br/> Verplicht |**XS: teken reeks** |Bestands naam van invoer bron. |

## <a name="VideoTracks"></a>VideoTracks-element
Elk fysiek AssetFile kan in de waarde nul bevatten of meer Video's worden getraceerd in een geschikte container indeling. Het **VideoTracks** -element vertegenwoordigt een verzameling van alle video tracks.  

U kunt een XML-voor beeld [-XML-voor](#xml)beeld vinden.  

### <a name="child-elements"></a>Onderliggende elementen
| Naam | Beschrijving |
| --- | --- |
| **VideoTrack**<br/><br/> minOccurs = "1" maxOccurs = "ontgrensd" |Een specifiek video spoor in het bovenliggende AssetFile. Zie het element VideoTrack voor meer informatie. |

## <a name="VideoTrack"></a>VideoTrack-element
Een specifiek video spoor in het bovenliggende AssetFile.  

U kunt een XML-voor beeld [-XML-voor](#xml)beeld vinden.  

### <a name="attributes"></a>Kenmerken
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Id**<br/><br/> minInclusive = "0"<br/><br/> Verplicht |**XS: int** |Op nul gebaseerde index van deze video track. **Opmerking:**  Deze **id** is niet noodzakelijkerwijs de TrackID die in een MP4-bestand wordt gebruikt. |
| **FourCC**<br/><br/> Verplicht |**XS: teken reeks** |Code van de video-codec FourCC. |
| **Profiel** |**XS: teken reeks** |H264-Profiel (alleen van toepassing op H264-codec). |
| **Niveau** |**XS: teken reeks** |Niveau H264 (alleen van toepassing op H264-codec). |
| **Breedte**<br/><br/> minInclusive = "0"<br/><br/> Verplicht |**XS: int** |Breedte van gecodeerde video in pixels. |
| **Hoogte**<br/><br/> minInclusive = "0"<br/><br/> Verplicht |**XS: int** |Versleutelde video hoogte in pixels. |
| **DisplayAspectRatioNumerator**<br/><br/> minInclusive = "0"<br/><br/> Verplicht |**XS: Double** |Teller voor hoogte-breedte verhouding video weergave. |
| **DisplayAspectRatioDenominator**<br/><br/> minInclusive = "0"<br/><br/> Verplicht |**XS: Double** |Noemer van hoogte-breedte verhouding video weergave. |
| **Framerate**<br/><br/> minInclusive = "0"<br/><br/> Verplicht |**XS: decimaal** |Gemeten video frame frequentie in. 3F-indeling. |
| **TargetFramerate**<br/><br/> minInclusive = "0"<br/><br/> Verplicht |**XS: decimaal** |Vooraf ingestelde doel video frame frequentie in. 3F-indeling. |
| **Bitrate**<br/><br/> minInclusive = "0"<br/><br/> Verplicht |**XS: int** |De gemiddelde bitsnelheid voor video in kilobits per seconde, zoals berekend op basis van de AssetFile. Telt alleen de elementaire stream Payload en bevat niet de verpakkings belasting. |
| **TargetBitrate**<br/><br/> minInclusive = "0"<br/><br/> Verplicht |**XS: int** |De gemiddelde bitrate van het doel voor dit video spoor, zoals aangevraagd via de voor instelling voor versleuteling, in kilobits per seconde. |
| **MaxGOPBitrate**<br/><br/> minInclusive = "0" |**XS: int** |Maximale GOP terug gemiddelde bitrate voor dit video spoor, in kilobits per seconde. |

## <a name="AudioTracks"></a>AudioTracks-element
Elk fysiek AssetFile kan in de waarde nul of meer audio tracks Interleaved in een geschikte container indeling bevatten. Het **AudioTracks** -element vertegenwoordigt een verzameling van al deze audio sporen.  

U kunt een XML-voor beeld [-XML-voor](#xml)beeld vinden.  

### <a name="child-elements"></a>Onderliggende elementen
| Naam | Beschrijving |
| --- | --- |
| **AudioTrack**<br/><br/> minOccurs = "1" maxOccurs = "ontgrensd" |Een specifiek audio spoor in het bovenliggende AssetFile. Zie het element AudioTrack voor meer informatie. |

## <a name="AudioTrack"></a>AudioTrack-element
Een specifiek audio spoor in het bovenliggende AssetFile.  

U kunt een XML-voor beeld [-XML-voor](#xml)beeld vinden.  

### <a name="attributes"></a>Kenmerken
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Id**<br/><br/> minInclusive = "0"<br/><br/> Verplicht |**XS: int** |Op nul gebaseerde index van dit audio spoor. **Opmerking:**  Dit is niet noodzakelijkerwijs de TrackID die in een MP4-bestand wordt gebruikt. |
| **Videocodec** |**XS: teken reeks** |Teken reeks van codec audio track. |
| **EncoderVersion** |**XS: teken reeks** |Optionele Encoder-versie teken reeks, vereist voor EAC3. |
| **Kanalen**<br/><br/> minInclusive = "0"<br/><br/> Verplicht |**XS: int** |Aantal audio kanalen. |
| **SamplingRate**<br/><br/> minInclusive = "0"<br/><br/> Verplicht |**XS: int** |Audio sampling frequentie in samples/sec of Hz. |
| **Bitrate**<br/><br/> minInclusive = "0"<br/><br/> Verplicht |**XS: int** |De gemiddelde bitsnelheid van audio in bits per seconde, zoals berekend op basis van de AssetFile. Telt alleen de elementaire stream Payload en bevat niet de verpakkings belasting. |
| **BitsPerSample**<br/><br/> minInclusive = "0"<br/><br/> Verplicht |**XS: int** |Bits per voor beeld voor het wFormatTag-indelings type. |

### <a name="child-elements"></a>Onderliggende elementen
| Naam | Beschrijving |
| --- | --- |
| **LoudnessMeteringResultParameters**<br/><br/> minOccurs="0" maxOccurs="1" |Para meters voor Loudness-meet resultaten. Zie het element LoudnessMeteringResultParameters voor meer informatie. |

## <a name="LoudnessMeteringResultParameters"></a>LoudnessMeteringResultParameters-element
Para meters voor Loudness-meet resultaten.  

U kunt een XML-voor beeld [-XML-voor](#xml)beeld vinden.  

### <a name="attributes"></a>Kenmerken
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **DPLMVersionInformation** |**XS: teken reeks** |Versie van **Dolby** Professional LOUDNESS meter Development Kit. |
| **DialogNormalization**<br/><br/> minInclusive="-31" maxInclusive="-1"<br/><br/> Verplicht |**XS: int** |DialogNormalization gegenereerd via DPLM, vereist wanneer LoudnessMetering is ingesteld |
| **IntegratedLoudness**<br/><br/> minInclusive="-70" maxInclusive="10"<br/><br/> Verplicht |**xs:float** |Geïntegreerde Loudness |
| **IntegratedLoudnessUnit**<br/><br/> Verplicht |**XS: teken reeks** |Geïntegreerde LOUDNESS-eenheid. |
| **IntegratedLoudnessGatingMethod**<br/><br/> Verplicht |**XS: teken reeks** |Beperking-id |
| **IntegratedLoudnessSpeechPercentage**<br/><br/> minInclusive ="0" maxInclusive="100" |**xs:float** |Spraak inhoud over het programma, uitgedrukt als percentage. |
| **SamplePeak**<br/><br/> Verplicht |**xs:float** |De absolute waarde van een absoluut voor beeld, sinds het opnieuw instellen of sinds de laatste keer dat deze is gewist, per kanaal.  Eenheden zijn dBFS. |
| **SamplePeakUnit**<br/><br/> fixed="dBFS"<br/><br/> Verplicht |**xs:anySimpleType** |Voor beeld piek eenheid. |
| **TruePeak**<br/><br/> Verplicht |**xs:float** |Maximale waarde voor echte piek, conform de ITU-R BS. 1770-2, sinds opnieuw instellen of sinds de laatste keer dat deze is gewist, per kanaal. Eenheden zijn dBTP. |
| **TruePeakUnit**<br/><br/> Fixed = "dBTP"<br/><br/> Verplicht |**xs:anySimpleType** |Echte piek eenheid. |

## <a name="schema-code"></a>Schema code
    <?xml version="1.0" encoding="utf-8"?>  
    <xs:schema xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata" version="1.2"  
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



## <a name="xml"></a>XML-voor beeld

Het volgende XML-bestand is een voor beeld van het meta gegevensbestand voor uitvoer.  

    <AssetFiles xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="https://www.w3.org/2001/XMLSchema"   
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

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
