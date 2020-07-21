# KYC_DOC
This repository contains the documentation of KernelSnap‘s KYC system

## Communication with the API
## Data Input body 
```
- front : Front image of ID card
- back : Back image of ID card
- selfie : selfie of the person
- front_tolerance : integer between 1 and 3 (1 is recommended)
- back_tolerance : integer between 1 and 3 (1 is recommended)
- face_match_tolerance : integer between 1 and 3 (3 is recommended)
- ocr_tolerance : integer between 1 and 3 (1 is recommended)
- barcode tolerance : integer between 1 and 3 (1 is recommended)
- public_key : API public key
- private_key : API private key
- document_type : CIN or DL
- country_code : TN
```


### NodeJs Axios

```javascript
var axios = require('axios');
var FormData = require('form-data');
var fs = require('fs');
var data = new FormData();
data.append('front', fs.createReadStream('Path/to/Image'));
data.append('back', fs.createReadStream('Path/to/Image'));
data.append('selfie', fs.createReadStream('Path/to/Image'));
data.append('public_key', 'pubkey');
data.append('private_key', 'privatekey');
data.append('country_code', 'TN');
data.append('document_type', 'CIN');
data.append('front_tolerance', '3');
data.append('back_tolerance', '3');
data.append('face_match_tolerance', '3');
data.append('barcode_tolerance', '3');
data.append('ocr_tolerance', '3');

var config = {
		method: 'post',
		url: 'https://api.kernelsnap.com/api/verify',
		headers: { 
				...data.getHeaders()
		},
		data : data
};

axios(config)
.then(function (response) {
		console.log(JSON.stringify(response.data));
})
.catch(function (error) {
		console.log(error);
});
```
### NodeJs Native
```javascript
var https = require('follow-redirects').https;
var fs = require('fs');

var options = {
  'method': 'POST',
  'hostname': 'api.kernelsnap.com',
  'path': '/api/verify',
  'headers': {
  },
  'maxRedirects': 20
};

var req = https.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function (chunk) {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });

  res.on("error", function (error) {
    console.error(error);
  });
});

var postData = "------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"front\"; filename=\"IMG_0676.jpeg\"\r\nContent-Type: \"{Insert_File_Content_Type}\"\r\n\r\n" + fs.readFileSync('Path/to/Image') + "\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"back\"; filename=\"IMG_0694.JPG\"\r\nContent-Type: \"{Insert_File_Content_Type}\"\r\n\r\n" + fs.readFileSync('/Path/to/Image') + "\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"selfie\"; filename=\"selfiee.JPG\"\r\nContent-Type: \"{Insert_File_Content_Type}\"\r\n\r\n" + fs.readFileSync('Path/to/Image') + "\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"public_key\"\r\n\r\npubkey\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"private_key\"\r\n\r\nprivatekey\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"country_code\"\r\n\r\nTN\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"document_type\"\r\n\r\nCIN\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"front_tolerance\"\r\n\r\n3\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"back_tolerance\"\r\n\r\n3\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"face_match_tolerance\"\r\n\r\n3\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"barcode_tolerance\"\r\n\r\n3\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"ocr_tolerance\"\r\n\r\n3\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW--";

req.setHeader('content-type', 'multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW');

req.write(postData);

req.end();
```

### NodeJs Request
```javascript
var request = require('request');
var fs = require('fs');
var options = {
  'method': 'POST',
  'url': 'https://api.kernelsnap.com/api/verify',
  'headers': {
  },
  formData: {
    'front': {
      'value': fs.createReadStream('Path/to/Image'),
      'options': {
        'filename': 'IMG_0676.jpeg',
        'contentType': null
      }
    },
    'back': {
      'value': fs.createReadStream('Path/to/Image'),
      'options': {
        'filename': 'IMG_0694.JPG',
        'contentType': null
      }
    },
    'selfie': {
      'value': fs.createReadStream('/Path/to/Image'),
      'options': {
        'filename': 'selfiee.JPG',
        'contentType': null
      }
    },
    'public_key': 'pubkey',
    'private_key': 'privatekey',
    'country_code': 'TN',
    'document_type': 'CIN',
    'front_tolerance': '3',
    'back_tolerance': '3',
    'face_match_tolerance': '3',
    'barcode_tolerance': '3',
    'ocr_tolerance': '3'
  }
};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});

```

### Case 1 : All data is valid
```javascript
{
    "cin_id_barcode": "09634285",
    "cin_id_ocr": "09634285",
    "cin_nb_barcode": "096342850230140619",
    "description": "Success!",
    "face_match": true,
    "issuance_day": "14",
    "issuance_month": "06",
    "issuance_year": "19",
    "kyc_recommendation": "accepted",
    "metadata": {
        "make": "Apple",
        "model": "iPhone 6s",
        "software": "12.4.1",
        "time": "2020:07:01 23:36:41"
    },
    "score": 0,
    "two_factor_verif": true,
    "verif_back": true,
    "verif_front": true
}
```


### Case 2 : Wrong person in selfie
```javascript
{
    "description": "Faces are not identical",
    "face_match": false,
    "kyc_recommendation": "rejected",
    "score": 0,
    "two_factor_verif": false,
    "verif_back": true,
    "verif_front": true
}
```


### Case 3 : Invalid back
```javascript
{
    "description": "Back is not valid",
    "face_match": false,
    "kyc_recommendation": "rejected",
    "score": 0,
    "two_factor_verif": false,
    "verif_back": false,
    "verif_front": true
}
```


### Case 4 : Invalid back and selfie
```javascript
{
    "description": "Back is not valid",
    "face_match": false,
    "kyc_recommendation": "rejected",
    "score": 0,
    "two_factor_verif": false,
    "verif_back": false,
    "verif_front": true
}
```


### Case 5 : Invalid Front
```javascript
{
    "description": "Front is not valid",
    "face_match": false,
    "kyc_recommendation": "rejected",
    "score": 0,
    "two_factor_verif": false,
    "verif_back": false,
    "verif_front": false
}

```


### Case 6 : Invalid front and selfie
```javascript
{
    "description": "Front is not valid",
    "face_match": false,
    "kyc_recommendation": "rejected",
    "score": 0,
    "two_factor_verif": false,
    "verif_back": false,
    "verif_front": false
}

```


### Case 7 : Invalid front and back
```javascript
{
    "description": "Front is not valid",
    "face_match": false,
    "kyc_recommendation": "rejected",
    "score": 0,
    "two_factor_verif": false,
    "verif_back": false,
    "verif_front": false
}
```


### Case 8 : Invalid front,back and selfie
```javascript
{
    "description": "Front is not valid",
    "face_match": false,
    "kyc_recommendation": "rejected",
    "score": 0,
    "two_factor_verif": false,
    "verif_back": false,
    "verif_front": false
}
```


### Case 9 : Front from other CIN
```javascript
{
    "description": "Faces are not identical",
    "face_match": false,
    "kyc_recommendation": "rejected",
    "score": 0,
    "two_factor_verif": false,
    "verif_back": true,
    "verif_front": true
}

```


### Case 10 : Back from other CIN
```javascript
{
    "cin_id_barcode": "07403519",
    "cin_id_ocr": "09634285",
    "cin_nb_barcode": "074035190106141204",
    "description": "Front and Back are not compatible",
    "face_match": true,
    "kyc_recommendation": "rejected",
    "score": 0,
    "two_factor_verif": false,
    "verif_back": true,
    "verif_front": true
}
```


### Case 11 : Invalid Public Key
```javascript
{
    "type": "NotFoundError",
    "message": "NotFoundError",
    "stack": [],
    "details": "User not found. Please register"
}
```


### Case 12 : Invalid private key
```javascript
{
    "type": "ValidationError",
    "message": "ValidationError",
    "stack": [],
    "details": "Wrong Private key"
}
```



### Case 13 : Invalid barcode tolerance
```javascript
{
    "type": "ValidationError",
    "message": "ValidationError",
    "stack": [],
    "details": "unvalid barcode tolerance"
}
```

### Case 14 : Invalid ocr tolerance
```javascript
{
    "type": "ValidationError",
    "message": "ValidationError",
    "stack": [],
    "details": "unvalid ocr tolerance"
}
```

### Case 15 : Invalid front tolerance
```javascript
{
    "type": "ValidationError",
    "message": "ValidationError",
    "stack": [],
    "details": "unvalid Front tolerance"
}
```

### Case 16 : Invalid back tolerance
```javascript
{
    "type": "ValidationError",
    "message": "ValidationError",
    "stack": [],
    "details": "unvalid Back tolerance"
}
```


### Case 17 : Invalid face_match tolerance
```javascript
{
    "type": "ValidationError",
    "message": "ValidationError",
    "stack": [],
    "details": "unvalid Face match tolerance"
}
```
