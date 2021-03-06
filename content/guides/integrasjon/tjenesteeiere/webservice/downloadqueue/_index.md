---
title: DownloadQueue
description: Hente ut elementer fra DownloadQueue
weight: 800
---

### DownloadQueue

DownloadQueue inneholder operasjoner for å hente ned en liste over elementer som ligger i tjeneste-eiers DownloadQueue. For å bruke DownloadQueue må tjenesteeier spesifisere dette når man utvikler tjenesten i TUL. Ved å sette tjenesteutgaven til å bruke DownloadQueue vil innleverte skjemaer på denne tjenesten oppdatere DownloadQueue og tjenesteeier vil dermed kunne hente ut disse ved å sende en forespørsel til Altinn.
For å bruke tjenesten må tjenesteeier først spesifisere at tjenesteutgaven bruker DownloadQueue. Deretter vil alle innsendinger sent inn med denne tjenesteutgaven legges til i DownloadQueue.
Når det ligger flere enn 500 elementer i DownloadQueue vil ikke nyere elementer lengre hentes når man bruker GetDownloadQueueItems operasjonen. For å få tilsendt nyere elementer må DownloadQueueItems fjernes fra DownloadQueue ved å bruke PurgeItem operasjonen.
DownloadQueue funksjonaliteten har støtte for MTOM.
Påfølgende kapitler beskriver tjenesteoperasjonenene for denne tjenesten.

##### GetDownloadQueueItems

Denne operasjonen henter en liste over DownloadQueueItems i tjenesteeiers DownloadQueue. Den henter et maksimum av 500 meta-data objekter med arkivreferanse knyttet til den innsendte meldingen. Den henter alltid de eldste objektene først. Man kan velge å filterere hentede objekter basert på ServiceCode.

|**Input**|**Beskrivelse**|
|--------|--------|
|ServiceCode|ServiceCode som man ønsker å filtrere hentede DownloadQueueItems på. Denne inputen er valgfri|
|**Returverdi**|**Beskrivelse**|
|DownloadQueueItemList|En liste med metadata-objekter som beskriver arkiv-referanse, ServiceCode, ServiceEdition, reportee, reporteetype og arkiveringstidspunkt for aktive (non-purged) DownloadQueueItems for tjenesteeier|
|**Property**|**Beskrivelse**|
||**DownloadQueueItemExternalBEList**|
||**DownloadQueueItemExternalBEList.DownloadQueueItemExternalBE**|
|ArchiveReference|Innsendingens arkiv-referanse|
|ServiceCode|Tjenestekode|
|ServiceEditionCode|Tjenesteutgavekode|
|Reportee|Organisasjons eller fødselsnummer for Reportee|
|ReporteeType|Type reportee: Person, Organisasjon, Selvregistrert bruker|
|ArchivedDate|Arkiveringsdato|

##### PurgeItem

Denne operasjonen lar tjenesteeier markere et enkelt DownloadQueueItem som purged. Dette DownloadQueueItem vil deretter ikke lenger bli hentet når GetDownloadQueueItems operasjonen blir kjørt.

|**Input**|**Beskrivelse**|
|--------|--------|
|ArchiveReference|Arkiv-referanse til DownloadQueueItem som skal markeres som purged|

##### GetArchivedFormtaskDQ

Denne operasjonen henter et arkivert FormTask object med tilhørende forms, signatur og attachment.

|**Input**|**Beskrivelse**|
|--------|--------|
|ArchiveReference|Arkiv-referanse til DownloadQueueItem som skal hentes ned|
|**ReturVerdi**|**Beskrivelse**|
|ArchivedFormTaskExternalDQBE|Et ArchivedFormTask element|
||**ArchivedFormTaskExternalDQBE**|
|**Property**|**Beskrivelse**|
|ServiceCode|Tjenestekode|
|ServiceEditionCode|Tjenesteutgavekode|
|CaseId|Unik identifikator på eventuell samhandlingstjeneste skjemasettet er knyttet til|
|SOEncryptedSymmetricKey|Representer informasjon for tjenesteeier kryptering. Se **SOEncryptedSymmetricKeyDQBE**|
|Approvers|Liste med personer som har signert. Se **ApproverDQBE**|
|Forms|Liste med alle forms som følger med i formsettet til formtasken. Se **ArchivedFormExternalDQBE**|
|Attachments|Liste med alle vedlegg som er relatert til formtasken. Se **ArchivedAttachmentExternalDQBE**|
|Reportee|Personnummer, organisasjonsnummer eller brukernavn på avgiver|
|ArchiveReference|Unik id for formtasken slik den er lagret i tjenesteeierarkivet|
|ArchiveTampStamp|Dato og tid for når elementet ble arkivert|
|CorrelationReference|(Ikke i bruk)|
||**SOEncryptedSymmetricKey**|
|**Property**|**Beskrivelse**|
|EnkryptedKey| AES-nøkkel som er RSA-kryptert med tjenesteeiers sertifikat. Denne AES-nøkkelen benyttes for å dekryptere skjemainnhold|
|CertificateThumbPrint|Thumbprint av tjenesteeiers sertifikat som ble benyttet for å kryptere AES-nøkkel. Vil unikt angi sertifikatet i de tilfeller hvor tjenesteeier har flere sertifikater registrert i Altinn|
||**ApproverDQBE**|
|**Property**|**Beskrivelse**|
|ApproverID| Identifikatoren til godkjenneren av skjemaet, d.v.s. fødselsnummer|
|AppprovedTimeStamp|Tidspunktet for godkjennelsen av skjemaet|
|SecurityLevel|Sikkerhetsnivå: notSensitive - Data er godkjent av bruker som er autentisert med sikkerhetsnivå 1, lessSensitive - Data er godkjent av bruker som er autentisert med sikkerhetsnivå 2, sensitive - Data er signert av bruker med sikkerhetsnivå 3, verySensitive - Data er signert av bruker med sikkerhetsnivå 4|
||**ArchiveFormExternalDQBE**|
|**Property**|**Beskrivelse**|
|DataformatID|Id til skjema fra metadata kilde|
|DataformatVersionID|Versjon til skjema fra metadata kilde|
|Reference|Unik id per skjema, satt av altinn|
|ParentReference|Referanse til hovedskjema. Settes kun i underskjema dersom underskjema forekommer|
|FormData|Skjemadata i xml format. ’My’ og ‘altinn’ elementer og deres namespace deklarasjoner i alle elementer er fjernet fra formdata xml|
||**ArchivedAttachmentExternalDQBE**|
|**Property**|**Beskrivelse**|
|ArchiveReference|Unik id for formtask elementet vedlegget er assosiert med|
|FileName|Det orginale navnet til filen|
|AttachmentType|Vedleggets innholds type. (MimeType)|
|IsEncrypted|En verdi som indikerer om vedlegget er kryptert av innsender1
|AttachmentData|Det faktiske inholdet i filen. Tjenesten leverer dette som MTOM og vil inkludere alle filer på opp til 30 MB. Større vedlegg må lastes ned for seg selv ved hjelp av streaming operasjonen|
|AttachmentId|En unik id på vedlegget slik det er lagret i tjenesteeierarkivet|
|AttachmentTypeName|Det tekniske navnet på vedleggsregelen som ble valgt hvis tjenesten har en eller flere slike regler|
|AttachmentTypeNameLanguage|Det oversatte navnet til vedleggsregelen|