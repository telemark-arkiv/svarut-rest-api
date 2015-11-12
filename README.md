# Kladd

## Relevante lenker
* https://kurs.kommit.no/mod/page/view.php?id=780

## Forslag til løsning

Det vil mest sannsynlig bli 3 kall:

return id /forsendelse/metadata {json med feltene i webservicen}

/forsendelse/id/file/navn binærdata i fila. Kan kalles flere ganger for flere filer.

/forsendelse/id/send returnerer ok om det er sendt.

### Metoder i dagens løsning
* sendForsendelse
* retriveForsendelseStatus
* retriveForsendelseHistorikk
* setForsendelseLestAvEksterntSystem

### SendForsendelse SOAP-kall:

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:Body>
      <ns2:sendForsendelse xmlns:ns2="http://www.ks.no/svarut/services">
         <forsendelse>
            <avgivendeSystem>BK-SAK</avgivendeSystem>
            <dokumenter>
               <data>
                  <xop:Include href="cid:example.pdf" xmlns:xop="http://www.w3.org/2004/08/xop/include"/>
               </data>
               <filnavn>small.pdf</filnavn>
               <mimetype>application/pdf</mimetype>
            </dokumenter>
            <krevNiva4Innlogging>false</krevNiva4Innlogging>
            <kryptert>false</kryptert>
            <kunDigitalLevering>false</kunDigitalLevering>
            <mottaker xsi:type="ns2:privatPerson" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
               <adresse1>Adresse1</adresse1>
               <navn>Navn</navn>
               <postnr>5258</postnr>
               <poststed>Poststed</poststed>
               <fodselsnr>04095800000/fodselsnr>
            </mottaker>
            <printkonfigurasjon>
               <brevtype>APOST</brevtype>
               <fargePrint>false</fargePrint>
               <tosidig>false</tosidig>
            </printkonfigurasjon>
            <tittel>tittele8965470-c5a7-4593-9410-54f66d49a96c</tittel>
         </forsendelse>
      </ns2:sendForsendelse>
   </soap:Body>
</soap:Envelope>
```
# REST-forslag
| HTTP METHOD | POST            | GET       |
| ----------- | --------------- | --------- |
| /svarut/v1/forsendelse | Lag ny forsendelse. Send. Returner ID | Error |
| /svarut/v1/forsendelse/{id} | Error | Returnerer forsendelse status |
| /svarut/v1/forsendelse/{id}/historie | Error | Returnerer forsendelse historikk |
| /svarut/v1/forsendelse/{id}/lestEksterntSystem | Setter forsendelse lest | Error |

```js
var forsendelse = {
  tittel: '',
  angivendeSystem: '',
  konteringsKode: '',
  kunDigitalLevering: true,
  kryptert: false,
  krevNiva4Innlogging: false,
  mottaker: {
    type: 'privatPerson',
    navn: '',
    adresse1: '',
    adresse2: '',
    adresse3: '',
    postnr: '',
    postSted: '',
    land: '',
    fodselsNr: '',
    orgNr: ''
  },
  printKonfigurasjon: {
    dobbeltsidig: true,
    fargePrint: true,
    brevType: 'BPOST'
  },
  lenker: [
    {
      ledetekst: '',
      urlLenke: '',
      urlTekst: ''
    }
  ],
  dokumenter: [
    {
      filnavn: '',
      mimetype: '',
      giroarkSider: '',
      data: '' //base64 data
    }
  ]
}
```
