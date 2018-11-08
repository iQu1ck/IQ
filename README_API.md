# IQ
Introduction
IQUICK API provide the ability to automatically accept payments in cryptocurrency. Currently three types of cryptocurrency are supported: Bitcoin, Ethereum, Dash. We use cryptographic signatures based on the algorithms of sha256, sha512, hmac-sha256, hmac-sha512. This protection is used over HTTPS connections, which ensures complete safety of your data. To generate a signature, two keys are used, which you can find in your personal account. You need the following algorithm to generate a signature (python3):

    public_key = env['public_key']
    data_for_sign = {
        'currency': 'btc',
        'callback_url': 'https://example.com/callback_page',
    }
    hmac = hmac.new(secret, msg=str(check_dict), digestmod=hashlib.sha256)
    signature = base64.b64encode(hmac.digest()).decode()
API_URL = http://iquick.io/api
API methods
Invoice
This method creates for you a new wallet in the blockchain of the specified cryptocurrency and allows you to accept single payments. It can be provided to the user for payment in your application. All funds transferred to the generated wallets will be redirected to the wallet in your personal account IQUICK After receiving the funds, the request for which you must return the invoice value is received at your callback address. The system sends five GET requests and stops if you application did not respond to the invoice value or the callback responsed an error.

Request
HTTP POST {API_URL}/payment//<callback_url> Example: http://iquick.io/api/payment/eth/abra.com Request with params Header | Value | Description ------ | ------ | ------ api_key | 9b8a0..2f391de1 | Public API Key api_sign | 5b6b4242e....54a271a75c9e | HMAC-SHA256 sign dict (currency, callback_url)

Response
{'success': true, 
'data': {
        'invoice': 63aa620f8e351474cdbbea7f941e08a74e4c48751f56c8d0,
        'address': 1EGBMKqXsmWwPeZS3QKeFkVy4SkGwxQwDk,
    }
}
Ticker
The method returns data on the cryptocurrency used in the system.

Request
HTTP GET {API_URL}/get_ticker/ Example: http://iquick.io/api/get_ticker/

Response
{
    "success": true,
    "data": {
        "btc": {
            "id": 1,
            "name": "Bitcoin",
            "symbol": "BTC",
            "website_slug": "bitcoin",
            "rank": 1,
            "circulating_supply": 17366100.0,
            "total_supply": 17366100.0,
            "max_supply": 21000000.0,
            "quotes": {
                "USD": {
                    "price": 6446.36103044,
                    "volume_24h": 4490503587.36113,
                    "market_cap": 111948150291.0,
                    "percent_change_1h": -0.21,
                    "percent_change_24h": -1.29,
                    "percent_change_7d": 0.97
                }
            },
            "last_updated": 1541697913
        },
        "eth": {
            ...
        },
        "dsh": {
            ...
        }
    }
}
Create product
This method allows you to create an internal product.

Request
HTTP POST {API_URL}/create/[btc]x[eth]x[dsh] Reques with params: Header | Value | Description ------ | ------ | ------ api_key | 9b8a0..2f391de1 | Public API Key api_sign | 5b6b4242e....54a271a75c9e | HMAC-SHA256 sign dict (currency, callback_url) name | Simple Name | Product Name price | 22.5 | Product price paytime | 0 | Subscribe time in day (0 - infinitely)

Response
Product info
Using this method, you can get information about the internal product (created in the merchant tab) and easily integrate it into your application.

Request
HTTP GET {API_URL}/api/product_info//

Example: http://iquick.io/api/product_info/2/*
Response
{
    "id": 1818,
    "name": "Example product",
    "price": "2.00",
    "about": "Example product. Simple create, use, integrate",
    "btc_receive": false,
    "eth_receive": true,
    "dsh_receive": true,
    "subscribe_time": 2,
    "is_deliverable": false
}
