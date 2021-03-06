
# 1. JSON Examples
**URL** : https://main.metakocka.si/rest/eshop/v1/json/product_list

**Description** : return list of all products with addition states :
* warehause amount
* prices on pricelists
* compound or norm structure
* partner info on product
* searching product with LIKE parameter

## 1.1 With amount
Request :
```javascript
{
    "secret_key":"my_secret_key",
    "company_id":"16",
    "sales":"true",
    "return_warehause_stock":"true"
}
```

Attribute                 | Type | Notes| MK SLO |
--------------------------|------|------|--------|
| count_code | char,30 | | Id artikla |
| mk_id | char,30 | | / |
| code | char,50 | | Šifra artikla |
| sales | bool | Product must be flag as sales | Prodajni |
| purchase | bool | Product must be flag as purchase | Nabavni |
| service | bool | Product must be flag as service | Storitev |
| work | bool | Product must be flag as work | Delo |
| active | bool | Product must be flag as active | Aktiven |
| eshop_sync | bool | Product must be flag for e-commerce sync | Izmenjava - Spletna trg. |
| return\_warehause\_stock | bool | If attribute is set, every product in respond will have "amount" attribute. In case the product was newer use on stock, attribute "amount" will still have value 0. | / |
| return\_warehouse\_reservation | bool | Product will have additional attribute "reservation_detail" with list of reservation amount per warehouse. | / |
| return\_product\_partner\_info | bool | Product will have additional attribute "product_partner_info" with list of custom partner info on product. | / |
| offset | int | see notes. | / |
| limit | int | see notes. | / |

Notes :
* call will always return max 1000 records (limit = 1000). To get next window of results, you must set offset on value 1000. 

Reapond (with return\_warehause\_stock = 'true'):
```javascript
{
   "opr_code":"0",
   "opr_time_ms":"11762",
   "sales":"true",
   "return_warehause_stock":"true",
   "limit":"2",
   "offset":"0",
   "product_list_count":"2",
   "product_list":[
      {
         "company_id":"0",
         "mk_id":"1600000027",
         "code":"prod sifra 1",
         "name":"naziv sifra 1",
         "unit":"kos",
         "service":"false",
         "sales":"true",
         "purchasing":"true",
         "height":"1",
         "width":"2",
         "depth":"3",
         "weight":"4",
         "amount":"10",
         "amount_detail": [
         {
            "warehouse_mark": "oznaka1",
            "warehouse_name": "moje skladisce",
            "serial_number": "ser1",
            "exp_date": "2013-06-10+02:00",
            "amount": "4"
         },
         {
            "warehouse_mark": "oznaka1",
            "warehouse_name": "moje skladisce",
            "serial_number": "ser2",
            "exp_date": "2013-06-10+02:00",
            "amount": "6"
         }
         ]
      },
      {
         "company_id":"0",
         "mk_id":"1600000028",
         "code":"pod sifra 2",
         "name":"pod naziv 2",
         "unit":"dan",
         "service":"false",
         "sales":"true",
         "purchasing":"false"
      }
   ]
}
```

Notes :
* head of respond contains attributes (sales, limit, etc) from request.
* product\_list\_count is number of records in respond. Not the number of all records. If product\_list\_count = limit, you should read next window of results.
* amount\_detail will give you detail view for amount on different warehouses and separate by serial number, exp date, microloc or lot.

## 1.2 With pricelist
Request :
```javascript
{
    "secret_key":"my_password",
    "company_id":"16",
    "count_code":"PA_115_PA",
    "return_pricelist" : "true"
}
```

Respond :
```javascript
{  
   "opr_code":"0",
   "opr_time_ms":"144",
   "limit":"1000",
   "offset":"0",
   "product_list_count":"1",
   "product_list":{  
      "count_code":"PA_115_PA",
      "mk_id":"1600000392",
      "code":"art1",
      "name":"art1",
      "unit":"kos",
      "service":"false",
      "sales":"true",
      "purchasing":"true",
      "pricelist":[  
         {  
            "count_code":"PC_200_PC",
            "price_def":{  
               "amount_to":"10",
               "price":"2"
            }
         },
         {  
            "count_code":"PC_200_PC",
            "price_def":{  
               "amount_from":"10",
               "price":"3"
            }
         },
         {  
            "count_code":"c1",
            "price_def":{  
               "price":"20"
            }
         },
         {  
            "count_code":"PC_115",
            "price_def":{  
               "tax":"EX4",
               "tax_desc":"22",
               "price":"2"
            }
         },
         {  
            "count_code":"PC-2016",
            "price_def":{  
               "discount":"15",
               "tax":"EX4",
               "tax_desc":"22",
               "price_with_tax":"20"
            }
         }
      ]
   }
}
```
Notes :
* pricelist 'PC\_200\_PC' has price variation (2 EUR to amount 10, 3 EUR for more then 10),
* pricelist 'c1' has only only price (without tax)
* pricelist 'PC_115' has price (without tax)
* pricelist 'PC-2016' has price with tax and 
* all pricelists are includes - sales and purchase

## 1.3 With compound
Request :
```javascript
{
    "secret_key":"my_password",
    "company_id":"16",
    "count_code":"PA_115_PA",
    "return_product_compound" : "true"
}
```

Respond :
```javascript
{
    "opr_code": "0",
    "opr_time_ms": "191",
    "count_code": "PA-4171",
    "limit": "1000",
    "offset": "0",
    "product_list_count": "1",
    "product_list": {
        "count_code": "PA-4171",
        "mk_id": "1600129636",
        "code": "k1",
        "name": "kitchen",
        "unit": "kos",
        "service": "true",
        "sales": "true",
        "purchasing": "true",
        "compound_type": "compound",
        "compounds": [
            {
                "product_mk_id": "1600129635",
                "product_count_code": "PA-4170",
                "product_code": "t1",
                "product_title": "table",
                "amount": "1",
                "purchase_unit_factor": "0,5"
            },
            {
                "product_mk_id": "1600129634",
                "product_count_code": "PA-4169",
                "product_code": "c1",
                "product_title": "chair",
                "amount": "5",
                "purchase_unit_factor": "0,1"
            }
        ]
    }
}
```
Notes :
* compound_type - can have values 'norm' (MK SLO : 'Normativ') or 'compound' (MK SLO : 'Kosovnica')

## 1.4 Unit2 and unit factor
Respond :
```javascript
{
    "opr_code": "0",
    "opr_time_ms": "57",
    "count_code": "26276",
    "limit": "1000",
    "offset": "0",
    "product_list_count": "1",
    "product_list": {
        "count_code": "26276",
        "mk_id": "1600180258",
        "code": "uni2",
        "name": "unit2",
        
        "unit": "kg",
        "unit2": "cm3",
        "unit_factor": "10,52",
        
        "service": "false",
        "sales": "true",
        "purchasing": "false"
    }
}
```

## 1.5 Extend paramethers
Respond :
```javascript
{
    "opr_code": "0",
    "opr_time_ms": "57",
    "count_code": "26276",
    "limit": "1000",
    "offset": "0",
    "product_list_count": "1",
    "product_list": {
        "count_code": "26276",
        "mk_id": "1600180258",
        "code": "uni2",
        "name": "unit2",
        
        "unit": "kg",
        "unit2": "cm3",
        "unit_factor": "10,52",
        
        "service": "false",
        "sales": "true",
        "purchasing": "false",
        "extra_column": [
          {
            "name": "ime_artikla_nas_sistem",
            "value": "T-SHIRT 4"
          },
          {
            "name": "ime_pakiranja",
            "value": "KOS 4"
          },
          {
            "name": "marketing",
            "value": "false"
          }
        ]
    }
}
```
## 1.6 with return\_warehause\_reservation
```javascript
{
   "opr_code":"0",
   "opr_time_ms":"11762",
   "sales":"true",
   "return_warehause_stock":"true",
   "limit":"2",
   "offset":"0",
   "product_list_count":"2",
   "product_list":[
      {
         "company_id":"0",
         "mk_id":"1600000027",
         "code":"prod sifra 1",
         "name":"naziv sifra 1",
         "unit":"kos",
         "service":"false",
         "sales":"true",
         "purchasing":"true",
         "height":"1",
         "width":"2",
         "depth":"3",
         
         "weight":"4",
         "amount":"10",
         "amount_detail": [
            {
               "warehouse_mark": "oznaka1",
               "warehouse_name": "moje skladisce",
               "serial_number": "ser1",
               "exp_date": "2013-06-10+02:00",
               "amount": "4"
            },
            {
               "warehouse_mark": "oznaka1",
               "warehouse_name": "moje skladisce",
               "serial_number": "ser2",
               "exp_date": "2013-06-10+02:00",
               "amount": "6"
            }
         ],
         "reservation_detail": [
         {
            "warehouse_mark": "oznaka1",
            "warehouse_name": "moje skladisce",
            "amount": "1"
         }
      ],
      }
   ]
}
```
## 1.7 With partner info
Request :
```javascript
{
    "secret_key":"my_secret_key",
    "company_id":"16",
    "sales":"true",
    "return_product_partner_info" : "true",
    "count_code":"PA_102_PA"
}
```
Respond :
```javascript
{
  "opr_code": "0",
  "opr_time_ms": "146",
  "count_code": "PA_102_PA",
  "sales": "true",
  "limit": "1000",
  "offset": "0",
  "product_list_count": "1",
  "product_list": [
    {
      "count_code": "PA_102_PA",
      "mk_id": "1600000066",
      "code": "sktest1",
      "barcode": "987654321",
      "name": "sktest1",
      "name_desc": "To je opis sktest1 artikla. D<font color=\"FF0000\">odatni test <br><br><br><font color=\"FF0000\">za testiranje HTM<font color=\"FF0000\">L opis<font color=\"FF0000\">ov <br><br><a href=\"http://\">www.google.com</a><br><br></font></font></font></font><ul><li><font color=\"FF0000\">dfs</font></li><li><font color=\"FF0000\"><font color=\"FF0000\">ghfh</font></font></li><li><font color=\"FF0000\"><font color=\"FF0000\"><font color=\"FF0000\">a,kdask</font></font></font></li></ul><p><br></p>",
      "unit": "kom",
      "service": "false",
      "sales": "true",
      "purchasing": "true",
      "eshop_sync": "false",
      "height": "60",
      "width": "80",
      "depth": "110",
      "weight": "40",
      "customs_fee": "54321",
      "koli_package_amount": "24",
      "asset": "false",
      "compound": "false",
      "expiration_dates": "false",
      "gross_weight": "50",
      "lot_numbers": "false",
      "min_stock": "10",
      "norm": "false",
      "serial_numbers": "false",
      "work": "false",
      "product_partner_info": [
        {
          "mk_id": "1600444908",
          "product_id": "1600000066",
          "product_code": "t12345",
          "product_name": "sifra_partner",
          "partner_id": "1600113924",
          "partner_name": "3D ART d.o.o."
        },
        {
          "mk_id": "1600444912",
          "product_id": "1600000066",
          "product_code": "t998",
          "product_name": "ps_test",
          "partner_id": "1600106702",
          "partner_name": "A. Kronenberg1"
        }
      ]
    }
  ]
}
```

## 1.8 With LIKE operator
Searching products with LIKE parameter. Possible to search by following fields:
* count_code
* code
* title

Request :
```javascript
{
    "secret_key":"my_secret_key",
    "company_id":"16",
    "search_with_like" : true,
    "return_product_compound" : true,
	"count_code":"PA_10"
}
```
Respond :
```javascript
{
    "opr_code": "0",
    "opr_time_ms": "741",
    "count_code": "PA_10",
    "limit": "1000",
    "offset": "0",
    "product_list_count": "10",
    "product_list": [
        {
            "count_code": "PA_100_PA",
            "mk_id": "1600000027",
            "code": "prod sifra 1",
            "barcode": "BAR_pa_100_pa",
            "name": "naziv sifra 1",
            "name_desc": "Dodatni opis 1",
            "unit": "kpl",
            "service": "true",
            "sales": "true",
            "activated": "true",
            "purchasing": "true",
            "eshop_sync": "true",
            "weight": "5",
            "customs_fee": "2710 19 27",
            "extra_column": [
                {
                    "name": "add_col_varchar1",
                    "value": "Test2"
                },
                {
                    "name": "add_col_varchar0",
                    "value": "Me2"
                }
            ],
            "localization": {
                "language": "en",
                "name": "12\" nekajfds",
                "name_desc": "<br>"
            },
            "compound_type": "compound",
            "compounds": {
                "product_mk_id": "1600000550",
                "product_count_code": "PA_120_PA",
                "product_code": "s120",
                "product_title": "n120",
                "amount": "5",
                "purchase_unit_factor": "0,2"
            },
            "asset": "false",
            "compound": "true",
            "country": "VE",
            "expiration_dates": "false",
            "lot_numbers": "false",
            "min_stock": "44",
            "norm": "false",
            "serial_numbers": "true",
            "work": "true",
            "supplier_info": {
                "partner_id": "1600113924",
                "partner_name": "3D ART d.o.o."
            }
        },
        {
            "count_code": "PA_101_PA",
            "mk_id": "1600000028",
            "code": "pod sifra 23",
            "name": "pod naziv 2a",
            "unit": "dan",
            "service": "true",
            "sales": "true",
            "activated": "true",
            "purchasing": "true",
            "eshop_sync": "false",
            "weight": "121",
            "compound_type": "compound",
            "compounds": {
                "product_mk_id": "1600000027",
                "product_count_code": "PA_100_PA",
                "product_code": "prod sifra 1",
                "product_title": "naziv sifra 1",
                "amount": "200",
                "purchase_unit_factor": "0,005"
            },
            "asset": "false",
            "compound": "true",
            "expiration_dates": "false",
            "gross_weight": "4",
            "lot_numbers": "false",
            "norm": "false",
            "serial_numbers": "false",
            "work": "true"
        },
        {
            "count_code": "PA_102_PA",
            "mk_id": "1600000066",
            "code": "sktest1",
            "barcode": "aergaerga",
            "name": "sktest1",
            "name_desc": "HTML<div><br></div><div><font color=\"#ff0000\">opis artikla</font></div><div><font color=\"#ff0000\"><br></font></div><div><font color=\"#ff0000\"><a href=\"www.google.com\">www.google.com</a></font></div>",
            "unit": "kg",
            "unit2": "g",
            "unit_factor": "1000",
            "service": "false",
            "sales": "true",
            "activated": "true",
            "purchasing": "true",
            "eshop_sync": "false",
            "weight": "45",
            "customs_fee": "2710 19 27",
            "asset": "false",
            "compound": "false",
            "expiration_dates": "false",
            "gross_weight": "50",
            "lot_numbers": "false",
            "min_stock": "10",
            "norm": "false",
            "serial_numbers": "false",
            "work": "false"
        },
        {
            "count_code": "PA_103_PA",
            "mk_id": "1600000087",
            "code": "crke_test1",
            "name": "crke_test1",
            "name_desc": "304490",
            "unit": "dan",
            "service": "false",
            "sales": "true",
            "activated": "true",
            "purchasing": "true",
            "eshop_sync": "true",
            "height": "50",
            "width": "60",
            "depth": "30",
            "asset": "false",
            "compound": "false",
            "expiration_dates": "false",
            "lot_numbers": "false",
            "norm": "false",
            "serial_numbers": "false",
            "work": "false"
        },
        {
            "count_code": "PA_105_PA",
            "mk_id": "1600000160",
            "code": "UVOZ_1",
            "name": "Uvoz 1",
            "name_desc": "Dodatni uvoz 2a",
            "unit": "cm",
            "service": "false",
            "sales": "true",
            "activated": "true",
            "purchasing": "true",
            "eshop_sync": "false",
            "height": "12",
            "width": "10",
            "depth": "12",
            "weight": "13",
            "asset": "false",
            "compound": "false",
            "expiration_dates": "false",
            "lot_numbers": "false",
            "norm": "false",
            "serial_numbers": "false",
            "work": "false"
        },
        {
            "count_code": "PA_106_PA",
            "mk_id": "1600000161",
            "code": "UVOZ_2",
            "name": "Uvoz 2",
            "name_desc": "Dodatni uvoz 2",
            "unit": "kg",
            "service": "true",
            "sales": "true",
            "activated": "true",
            "purchasing": "false",
            "eshop_sync": "false",
            "height": "12,123",
            "width": "10",
            "depth": "12",
            "weight": "2",
            "asset": "false",
            "compound": "false",
            "expiration_dates": "false",
            "lot_numbers": "false",
            "norm": "false",
            "serial_numbers": "false",
            "work": "false"
        },
        {
            "count_code": "PA_107_PA",
            "mk_id": "1600000167",
            "code": "sestav_del_1",
            "barcode": "8892342374",
            "name": "sestav_del_1",
            "unit": "m3",
            "service": "false",
            "sales": "true",
            "activated": "true",
            "purchasing": "true",
            "eshop_sync": "false",
            "asset": "false",
            "compound": "false",
            "expiration_dates": "false",
            "lot_numbers": "false",
            "norm": "false",
            "serial_numbers": "false",
            "work": "false"
        },
        {
            "count_code": "PA_108_PA",
            "mk_id": "1600000168",
            "code": "sestav_del_2",
            "barcode": "3492342374",
            "name": "sestav_del_2",
            "unit": "cm",
            "service": "false",
            "sales": "true",
            "activated": "true",
            "purchasing": "true",
            "eshop_sync": "false",
            "asset": "false",
            "compound": "false",
            "expiration_dates": "false",
            "lot_numbers": "false",
            "norm": "false",
            "serial_numbers": "false",
            "work": "false"
        },
        {
            "count_code": "PA_109_PA",
            "mk_id": "1600000169",
            "code": "sestav test 2",
            "name": "sestav test 2",
            "unit": "kos",
            "service": "true",
            "sales": "true",
            "activated": "true",
            "purchasing": "true",
            "eshop_sync": "false",
            "compound_type": "compound",
            "compounds": [
                {
                    "product_mk_id": "1600000167",
                    "product_count_code": "PA_107_PA",
                    "product_code": "sestav_del_1",
                    "product_title": "sestav_del_1",
                    "amount": "2",
                    "purchase_unit_factor": "0,2824859"
                },
                {
                    "product_mk_id": "1600000168",
                    "product_count_code": "PA_108_PA",
                    "product_code": "sestav_del_2",
                    "product_title": "sestav_del_2",
                    "amount": "1",
                    "purchase_unit_factor": "0,4350282"
                }
            ],
            "asset": "false",
            "compound": "true",
            "expiration_dates": "false",
            "lot_numbers": "false",
            "norm": "false",
            "serial_numbers": "false",
            "work": "false"
        },
        {
            "count_code": "PA_10_PA",
            "mk_id": "1600051891",
            "code": "skupni_popust_test",
            "name": "skupni_popust_test",
            "unit": "kos",
            "service": "false",
            "sales": "true",
            "activated": "true",
            "purchasing": "true",
            "eshop_sync": "false",
            "asset": "false",
            "compound": "false",
            "expiration_dates": "false",
            "lot_numbers": "false",
            "norm": "false",
            "serial_numbers": "false",
            "work": "false"
        }
    ]
}
```
