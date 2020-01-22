## General Explanation

To know if the request is success or failed, you can see the `HTTP Status Code` on each response. [#](http://www.restapitutorial.com/httpstatuscodes.html)

Every request should be sent as `application/x-www-form-urlencoded` unless told differently. The body of the request must be sent as post data (e.g `attribute=value&attribute2=value2&attribute3=value3`).

**422** status code is used to indicate the mistake from your side. The common response for this status code is like on the right side:

```json
{
    "code": [outer_code],
    "errors": [
        {
            "attribute": "[attribute]",
            "code": [inner_code],
            "message": "[message]"
        }
    ]
}
```

Possible value for `outer_code` is:

* `BALANCE_INSUFFICIENT`, happen when your Flip account balance are insufficient for the current transaction (balance < (transfer amount + transfer fee)).
*  `VALIDATION_ERROR`, error related to the validation of your payload data.

Possible value for `inner_code` is all the code listed in the [Error Code section](#error-code).

## Supported Banks

Currently, we only support disbursement to these banks:

Bank Code | Bank Name
----------|-----------
`mandiri`|Bank Mandiri
`bri`|Bank Rakyat Indonesia
`bni`|BNI (Bank Negara Indonesia) & BNI Syariah
`bca`|Bank Central Asia
`bsm`|Bank Syariah Mandiri
`cimb`|CIMB Niaga & CIMB Niaga Syariah
`muamalat`|Muamalat
`danamon`|Bank Danamon
`permata`|Bank Permata
`bii`|Maybank Indonesia
`panin`|Panin Bank
`uob`|UOB Indonesia
`ocbc`|OCBC NISP
`citibank`|Citibank
`artha`|Bank Artha Graha Internasional
`tokyo`|Bank of Tokyo Mitsubishi UFJ
`dbs`|DBS Indonesia
`standard_chartered`|Standard Chartered Bank
`capital`|Bank Capital Indonesia
`anz`|ANZ Indonesia
`boc`|Bank of China (Hong Kong) Limited
`bumi_arta`|Bank Bumi Arta
`hsbc`|HSBC Indonesia
`rabobank`|Rabobank International Indonesia
`mayapada`|Bank Mayapada
`bjb`|BJB
`dki`|Bank DKI Jakarta
`daerah_istimewa`|BPD DIY
`jawa_tengah`|Bank Jateng
`jawa_timur`|Bank Jatim
`jambi`|Bank Jambi
`sumut`|Bank Sumut
`sumatera_barat`|Bank Sumbar (Bank Nagari)
`riau_dan_kepri`|Bank Riau Kepri
`sumsel_dan_babel`|Bank Sumsel Babel
`lampung`|Bank Lampung
`kalimantan_selatan`|Bank Kalsel
`kalimantan_barat`|Bank Kalbar
`kalimantan_timur`|Bank Kaltim
`kalimantan_tengah`|Bank Kalteng
`sulselbar`|Bank Sulselbar
`sulut`|Bank SulutGo
`nusa_tenggara_barat`|Bank NTB
`bali`|BPD Bali
`nusa_tenggara_timur`|Bank NTT
`maluku`|Bank Maluku
`papua`|Bank Papua
`bengkulu`|Bank Bengkulu
`sulawesi`|Bank Sulteng
`sulawesi_tenggara`|Bank Sultra
`nusantara_parahyangan`|Bank Nusantara Parahyangan
`india`|Bank of India Indonesia
`mestika_dharma`|Bank Mestika Dharma
`sinarmas`|Bank Sinarmas
`maspion`|Bank Maspion Indonesia
`ganesha`|Bank Ganesha
`icbc`|ICBC Indonesia
`qnb_kesawan`|QNB Indonesia
`btn`|BTN (Bank Tabungan Negara)
`woori`|Bank Woori Saudara
`tabungan_pensiunan_nasional`|BTPN
`bri_syr`|BRI (Bank Rakyat Indonesia) Syariah
`bjb_syr`|BJB Syariah
`mega`|Bank Mega
`bukopin`|Bukopin
`jasa_jakarta`|Bank Jasa Jakarta
`hana`|KEB Hana Bank Indonesia
`mnc_internasional`|Bank MNC Internasional
`agroniaga`|BRI Agroniaga
`sbi_indonesia`|SBI Indonesia
`royal`|Bank Royal Indonesia
`nationalnobu`|Nobu (Nationalnobu) Bank
`mega_syr`|Bank Mega Syariah
`ina_perdana`|Bank Ina Perdana
`sahabat_sampoerna`|Bank Sahabat Sampoerna
`kesejahteraan_ekonomi`|Bank Kesejahteraan Ekonomi
`bca_syr`|BCA (Bank Central Asia) Syariah
`artos`|Bank Artos Indonesia
`mayora`|Bank Mayora Indonesia
`index_selindo`|Bank Index Selindo
`victoria_internasional`|Bank Victoria International
`agris`|Bank Agris
`chinatrust`|CTBC (Chinatrust) Indonesia
`commonwealth`|Commonwealth Bank
`ovo`|OVO
`dana`|Dana
`doku`|Doku