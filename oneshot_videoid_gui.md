# What it is

<div style="text-align: justify">
One-Shot represents the solution for electronic signatures based on a single-use certificate issuance. One-Shot API is the solution for Uanataca One-Shot Signature service, a complete option that can be implemented as part of your application or web.
<br></br>
It is designed in such a way that no sensitive data has to be sent away from your premises, as only hashes of the documents to be signed need to be transmitted to the signature service. Documents are signed by means of the creation of single-use digital certificates, which are created at the moment of the signature operation for immediate use to electronically sign all documents included.
<br></br> 
Electronic signatures include a time stamp, providing a document signature with a reliable date and time.

</div>

# How it works

<div style="text-align: justify">
The API is given with a module called One-Shot Optimizer, a server system that exposes our One-Shot HTTP RESTful API through which business applications are able to process their electronic signature requests.
<br></br>
One-Shot Optimizer performs the most computationally expensive workload of the signature process, thus reducing data traffic on the local network and performing most cryptographic hardware acceleration. The documents to be signed are processed in the customer business layer and are not send to Uanataca Services since a hash of the document is sent, created from a hash algorithm.
<br></br>
Uanataca is a Qualified Trusted Service Provider that issues digital certificates by means of its own trusted Certification Authority (CA). Additionally, the role of the Registration Authority Officials (RAO) is vital in the process as they manage every request for new digital certificates that is received from new users. Issued certificates can be used to electronically sign documents.
<br></br>
One-Shot Signature certificates are generated on the spot every time a new set of documents requires a signature. Through One-Shot Signature service you will play RAO's role, providing identification data for each user and requesting the generation of signature certificates. Once user registration data has been provided and the certificate is ready to be issued, the end user triggers the generation of the digital certificate. This action completes the signature procedure.
<br></br>
The electronic signatures are performed on Uanataca Trusted Service Center side, where signature keys are generated and stored in a Qualified Electronic Signature Creation Device (QSCD) system.
</div>
<br></br>

![img](https://github.com/UANATACA/ONESHOT-VIDEOID-REPO/raw/main/img/oneshot-hiw.png)


# Test Environment


For testing purposes, Uanataca provides integrators of a pre-configured test-mode One-Shot Optimizer accessible at the following URL:

</br>

	https://one-shot.developers.uanataca.com

In test environment, requests can be easily created, validated or approved by using a stored default Operator credentials set represented by an alfanumeric string called token. This token can be found when executing the <a href="#tag/Tokens/paths/~1api~1v1~1tokens/get">List Tokens</a> call. 

</br>

List Tokens call output:

	{
		"status": "200 OK",
		"details": {
			"6d1cae4d55be4cdf9cac50ee36f73406": {
				"username": "9001800",
				"password": true,
				"pin": true,
				"NOTE": "On this demo environment, this method will only return the demo token"
			}
		}
	}

</br>

When using our test-mode Optimizer, you must consider:

1) No Billing credentials are required. </br>
2) The Registration Authority id number will be explicitly assigned by our staff. </br>
3) The token ´6d1cae4d55be4cdf9cac50ee36f73406´ is valid for using in <a href="#tag/Requests/paths/~1api~1v1~1request/post">Create Request</a> call. Cannot be deleted. </br>

</br>

Want to configure your own test-mode Optimizer? Find instructions in the <a href='#section/Configuration'>configuration</a> section.


# Classic Workflow


<div style="text-align: justify">
In a common One-Shot Signature service, an OTP (One-Time Password) code is sent via SMS to the end user, directly from Uanataca services. The OTP verification method is used to trigger the issuance of the single-use digital signature certificate, which is inmediately used to sign the request associated documents e.g. the service contract.
<br></br>
One-Shot Signature can use other external authentication methods instead of Uanataca SMS. This alternative methods are subjected to approval.
<br></br>
The following images summarize One-Shot Signature flow involving both authentication methods:
</div>
<br></br>

> Uanataca SMS

![img](https://github.com/UANATACA/ONESHOT-VIDEOID-REPO/raw/main/img/oneshot_flc1.png)

</br>

1. The business application creates a new digital signature request, providing all required user data
2. One-Shot Optimizer returns an identifier for the certificate request
3. The business application provides the documents to be signed by the end user
4. The business application retrieves the service contract
5. The business application shows the documents to be signed and the service contract to its end user
6. After reviewing the documents, the end user agrees to sign them
7. The business application starts the signature process by requesting the generation of an One-Time Password (OTP) token for the signature
8. Uanataca services sends the OTP code directly to the end user through an SMS message
9. By introducing the OTP code, the end user identifies himself as the subject of the signature certificate
10. The business application provides the OTP and the identifier of the signature request to the One-Shot Optimizer
11. One-Shot Optimizer takes care of computing the hash of the documents to be signed in the business layer
12. Hashes are sent with the request identifier and OTP code to Uanataca Services
13. The end user signature certificate is generated and used to sign the hashes
14. The signed hashes and the signature identifier are returned to the One-Shot Optimizer
15. One-Shot Optimizer generates the signed document envelopment, combining the original documents with the signed hashes
16. Finallly, the business application calls One-Shot Optimizer API to obtain the signed documents
<br></br>

> Other authentication methods


![img](https://github.com/UANATACA/ONESHOT-VIDEOID-REPO/raw/main/img/oneshot_flc2.png)

</br>

1. The business application creates a new digital signature request, providing all required user data
2. One-Shot Optimizer returns an identifier for the certificate request
3. The business application provides the documents to be signed by the end user
4. The business application retrieves the service contract
5. The business application shows the documents to be signed and the service contract to its end user
6. After reviewing the documents, the end user agrees to sign them
7. The client application starts the signature process with an authentication method provided by the client
8. One-Shot Optimizer takes care of computing the hash of the documents to be signed in the business layer
9. Hashes are sent together with the request identifier and an id of the business authentication method to Uanataca Services
10. The end user signature certificate is generated and used to sign the hashes
11. The signed hashes and the signature identifier are returned to the One-Shot Optimizer
12. One-Shot Optimizer generates the signed document envelopment, combining the original documents with the signed hashes
13. Finally, the business application calls One-Shot Optimizer API to obtain the signed documents

</br>

The next section presents the workflow for a simple use case of the One-Shot Signature service with a step-by-step description of the API calls required to allow a user to digitally sign a document provided by the client application. 

</br>

*The basic digital signature process involves the following steps:*

**1) RETRIEVE AN EXISTING TOKEN FOR THE RAO**

**2) CREATE A NEW DIGITAL SIGNATURE REQUEST**

**3) UPLOAD A DOCUMENT**

**4) RETRIEVE SERVICE CONTRACT**

**5) GENERATE AN OTP (only for Uanataca SMS)**

**6) SIGN THE DOCUMENT**

**7) RETRIEVE SIGNED DOCUMENT**

**8) DELETE DOCUMENTS FROM OPTIMIZER**

</br>

> **STEP 1: RETRIEVE AN EXISTING TOKEN FOR THE RAO**

</br>

**API Reference:** <a href="#tag/Tokens/paths/~1api~1v1~1tokens/get">List Tokens</a>

The test-mode One-Shot Optimizer is pre-configured with a Registration Authority Officer (RAO) account ready to be used within the test environment. This account has an associated token that can be used to identify the RAO in API calls.

	curl -X GET https://one-shot.developers.uanataca.com/api/v1/tokens

This call should return the following JSON object:

	{
		"status": "200 OK",
		"details": {
			"6d1cae4d55be4cdf9cac50ee36f73406": {
				"username": "9001800",
				"password": true,
				"pin": true
			}
		}
	}

This output tells us that a single token "6d1cae4d55be4cdf9cac50ee36f73406" exists. This token is associated to the RAO account with id "9001800" and can be used instead of the password and pin.

To use tokens in a production environment, you will need to create them first using the corresponding <a href="#tag/Tokens/paths/~1api~1v1~1token/post">Create token</a> API call.


</br>

> **STEP 2: CREATE A NEW DIGITAL SIGNATURE REQUEST**

</br>

**API Reference:** <a href="#tag/Requests/paths/~1api~1v1~1request/post">Create Request</a>

In One-Shot Signature Service, all data corresponding to a given digital signature is collected within a Digital Signature Request. This includes both the identifying information of the signing user -provided when you create the signature request- and the document or documents to be signed, to be uploaded in an upcoming step.

This call must include enough information to identify both the signing user and the RAO approving the request. The full description of the arguments accepted by this endpoint can be found in the API call detailed documentation, but for now it is enough to include at least the following:

	curl --location --request POST 'https://one-shot.developers.uanataca.com/api/v1/request' \
		 --form 'token=6d1cae4d55be4cdf9cac50ee36f73406' \
		 --form 'profile=PFnubeQAFCiudadano' \
		 --form 'given_name=name_of_the_user' \
		 --form 'surname_1=surname_of_the_user' \
		 --form 'email=user-example@domain.com' \
		 --form 'mobile_phone_number=+343391234567' \
		 --form 'document_front=@document_front.png' \
		 --form 'document_rear=@document_rear.png' \
		 --form 'document_owner=@document_owner.png'

where "token" is the token representing the RAO's credentials obtained in the previous step.

If the signature request is completed successfully, we will get the unique identifier assigned to it:

	{
		"status": "201 Created",
		"details": 1464
	}

The code shown above will be used to identify the created signature request in subsequent calls.


</br>

> **STEP 3: UPLOAD A DOCUMENT**

</br>

**API Reference:** <a href="#tag/Documents/paths/~1api~1v1~1document~1{pk}/post">Upload Document</a>


After creating the the signature request, we can associate all pdf documents that must be signed by the user.

	curl -F "file=@doc.pdf" -X POST https://one-shot.developers.uanataca.com/api/v1/document/1464

Note that the number at the end of the call is the request id we obtained in the previous step.

If the upload is successful, the response will contain the identifier assigned to each document:

	{
		"status": "200 OK",
		"details": "712c29ac-a2dc-4530-8c67-d0f227d8294b"
	}

</br>

> **STEP 4: RETRIEVE SERVICE CONTRACT**

</br>

**API Reference:** <a href="#tag/Documents/paths/~1api~1v1~1document~1{pk}~1contract/get">Retrieve Contract</a>

As a Trusted Service Provider, Uanataca must inform certificate requesters about terms and conditions ruling the issuance of certificates. 

A service contract is generated for each digital certificate issue. The user must view the request-associated service contract to be signed in the same step as the documents, using the one-time certificate just issued.

	curl -X GET https://one-shot.developers.uanataca.com/api/v1/document/1464/contract

The response by the server will be the service contract document file in binary format:

	%PDF
	...

</br>

> **STEP 5: GENERATE AN OTP (only for Uanataca SMS)**

</br>

**API Reference:** <a href="#tag/Requests/paths/~1api~1v1~1otp~1{pk}/post">Generate OTP code</a>

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">This step applies only for signatures made using Uanataca SMS method.</blockquote>

Once the documents to be signed are ready, we need to generate a secure One-time password (OTP) that allows the user to sign them. The OTP code is generated by executing the **Generate OTP** call and the resulting OTP is sent as an SMS message directly to the phone number is provided when creating the signature request.

When calling the OTP endpoint you must add the request identifier returned by the **Create Request** call:

	curl -X POST https://one-shot.developers.uanataca.com/api/v1/otp/1464

A successful call will look like this:

	{
		"status": "200 OK",
		"details": "OTP generated"
	}

With this call, an SMS with the secret code is sent to the mobile phone number associated to the signature request.


</br>

> **STEP 6: SIGN THE DOCUMENT**

</br>

**API Reference:** <a href="#tag/Requests/paths/~1api~1v1~1sign~1{pk}/post">Sign</a>

In this step the digital signature certificate is issued, then it signs all documents previously uploaded for the signature request.

Execute the sign call with the request id and json parameters containing the OTP code sent:

	curl -d @params.json -H "Content-Type: application/json -X POST https://one-shot.developers.uanataca.com/api/v1/sign/1464

params.json for Uanataca SMS:

	{
		"secret": "123456"
	}

params.json for other authentication methods:

	{
		"secret": "123456"
		"ext_unlock_type": "biometric"
		"ext_unlock_value": "12345678-12345678"
	}

</br>

A successful call will result in the following response:

	{
		"status": "200 OK",
		"details": "Documents correctly signed"
	}

</br>

> **STEP 7: RETRIEVE SIGNED DOCUMENT**

</br>

API reference: <a href="#tag/Documents/paths/~1api~1v1~1document~1{pk}~1{type}~1{uid}/get">Retrieve Document</a>

Once the signature is done, the next step is to get all signed documents.

To do this, query with an HTTP GET request the endpoint /api/v1/document/{pk}/{type}/{uid}, where {pk} is the Request's unique identifier, {type} is the type of the document (it can be "original" for the uploaded document or "signed" for the digitally-signed version) and {uid} is the document unique identifier.

	curl -X GET https://one-shot.developers.uanataca.com/api/v1/document/1464/signed/712c29ac-a2dc-4530-8c67-d0f227d8294b

The response obtained by the server will be the document in binary format:

	%PDF
	...

</br>

> **STEP 8: DELETE DOCUMENTS FROM OPTIMIZER**

</br>

**API reference:** <a href="#tag/Documents/paths/~1api~1v1~1documents~1{pk}/delete">Delete All Documents</a>

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">⚠ Since the service does not delete uploaded files unless explicitly requested through an API call, it is <strong>strongly recommended</strong> that you backup any file you want to preserve using an alternative system.</blockquote>

Delete all documents associated to a finished digital signature request.

	curl -X DELETE https://one-shot.developers.uanataca.com/api/v1/documents/1464


</br>


# Video ID Workflows


## eIDAS VideoID

This workflow defines the complete process of issuing eIDAS certificates.

</br>

![img](https://raw.githubusercontent.com/UANATACA/ONESHOT-VIDEOID-REPO/raw/main/img/oneshot_1step.png)

</br>

*This process involves the following steps:*


**1) CREATE A NEW VIDEO ID SIGNATURE REQUEST**

**2) REQUEST APPROVAL**

**3) UPLOAD A DOCUMENT**

**4) RETRIEVE SERVICE CONTRACT**

**5) GENERATE AN OTP (only for Uanataca SMS)**

**6) SIGN THE DOCUMENT**

**7) RETRIEVE SIGNED DOCUMENT**

**8) DELETE DOCUMENTS FROM OPTIMIZER**

</br>

> **STEP 1: CREATE A NEW VIDEO ID SIGNATURE REQUEST**

</br>

**API Reference:** <a href="#tag/Video-ID/paths/~1api~1v1~1videoid/post">Create Video ID Request</a>

This call must include preliminary information to identify the signer. 


	curl -i -X POST https://one-shot.developers.uanataca.com/api/v1/videoid \
		-H 'Content-Type: application/json' \
		-d '{
			"mobile_phone_number": "+34699999999",
			"email": "mail@domain",
			"registration_authority": "139",
			"profile": "PFnubeQAFCiudadano",
			"videoid_mode": 1,
			"webhook_url": "my-webhook-url.com"
		}'

If the signature request is completed successfully, both video and request unique identifiers are returned, as well as the corresponding video id link:

	{
		"status": "200 OK",
		"details": {
			"videoid_pk": 150,
			"videoid_link": "https://cms.access.bit4id.org:13035/lcmpl/videoid/ZWxlY3Ryb25pY2lkOkl3YlBNdTktcXpBTU1yd0ROeUR0VWNRNk02bVVmVV9SQnZqYnFOR0Vhc2(...)",
			"request_pk": 45836
		}
	}

The request starts at **VIDEOPENDING** status after creation. The request_pk output parameter will be used to identify this digital signature request in subsequent calls. 

</br>

> At this point, the workflow progress will depend on the video-identification successful completion. This action will change request status from **VIDEOPENDING** to **VIDEOREVIEW**. 

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">⚠ In case the process is not totally completed or has failed for any reason, the request will change to <b>VIDEOINCOMPLETE</b> or <b>VIDEOERROR</b> respectively.</blockquote>

To inform business app and validation RAO about this change at the time it takes place, we recommend the implementation of a **Webhook**. Check our documentation for <a href='#section/Configuration/Webhooks'>Webhook Configuration</a>.  

If request data needs to be modified, use the <a href="#tag/Video-ID/paths/~1api~1v1~1request~1{request_pk}/put">Update Request</a> call. Check API Reference.</br>
If request data needs to be retrieved, use the <a href="#tag/Requests/paths/~1api~1v1~1request~1{request_pk}/get">Get Request</a> call. Check API Reference.

</br>

> **STEP 2: REQUEST APPROVAL**

</br>

**API Reference:** <a href="#tag/Video-ID/paths/~1api~1v1~1videoid~1{request_pk}~1validate/post">Approve Request</a>

This call makes the request ready for signature. Its status changes to **ENROLLREADY** and webhook intervention at this point is important for status update. In 1-step mode, both validation and approval occur when executing this call.

	curl -i -X POST 'https://one-shot.developers.uanataca.com/api/v1/request/45836/approve' \
		-H 'Content-Type: application/json' \
		-d '{
			"username": "1000279",
			"password": "3DPTm:N4",
			"pin": "23bYQq9a",
			"rao": "1400"
		}'

	OR

	curl -i -X POST 'https://one-shot.developers.uanataca.com/api/v1/request/45836/approve' \
		-H 'Content-Type: application/json' \
		-d '{
			"token": "f734066d1ce36f9cae4d55be4cdac50e",
			"rao": "1400"
		}'	

In case of using a token containing RAO's credentials.


The response is a JSON object with added request approval information. 

	{
		"status": "200 OK",
		"details": "Request approved successfully"
	}

In case of not approving a request for any reason, the call <a href="#tag/Requests/paths/~1api~1v1~1requests~1{id}~1cancel/delete">Cancel Request</a> must be executed. Check API Reference.

</br>

> **STEP 3: UPLOAD A DOCUMENT**

</br>

**API Reference:** <a href="#tag/Documents/paths/~1api~1v1~1document~1{pk}/post">Upload Document</a>


After creating the the signature request, we can associate all pdf documents that must be signed by the user.

	curl -F "file=@doc.pdf" -X POST https://one-shot.developers.uanataca.com/api/v1/document/1464

Note that the number at the end of the call is the request id we obtained in the previous step.

If the upload is successful, the response will contain the identifier assigned to each document:

	{
		"status": "200 OK",
		"details": "712c29ac-a2dc-4530-8c67-d0f227d8294b"
	}

</br>

> **STEP 4: RETRIEVE SERVICE CONTRACT**

</br>

**API Reference:** <a href="#tag/Documents/paths/~1api~1v1~1document~1{pk}~1contract/get">Retrieve Contract</a>

As a Trusted Service Provider, Uanataca must inform certificate requesters about terms and conditions ruling the issuance of certificates. 

A service contract is generated for each digital certificate issue. The user must view the request-associated service contract to be signed in the same step as the documents, using the one-time certificate just issued.

	curl -X GET https://one-shot.developers.uanataca.com/api/v1/document/1464/contract

The response by the server will be the service contract document file in binary format:

	%PDF
	...

</br>

> **STEP 5: GENERATE AN OTP (only for Uanataca SMS)**

</br>

**API Reference:** <a href="#tag/Requests/paths/~1api~1v1~1otp~1{pk}/post">Generate OTP code</a>

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">This step applies only for signatures made using Uanataca SMS method.</blockquote>

Once the documents to be signed are ready, we need to generate a secure One-time password (OTP) that allows the user to sign them. The OTP code is generated by executing the **Generate OTP** call and the resulting OTP is sent as an SMS message directly to the phone number is provided when creating the signature request.

When calling the OTP endpoint you must add the request identifier returned by the **Create Request** call:

	curl -X POST https://one-shot.developers.uanataca.com/api/v1/otp/1464

A successful call will look like this:

	{
		"status": "200 OK",
		"details": "OTP generated"
	}

With this call, an SMS with the secret code is sent to the mobile phone number associated to the signature request.


</br>

> **STEP 6: SIGN THE DOCUMENT**

</br>

**API Reference:** <a href="#tag/Requests/paths/~1api~1v1~1sign~1{pk}/post">Sign</a>

In this step the digital signature certificate is issued, then it signs all documents previously uploaded for the signature request.

Execute the sign call with the request id and json parameters containing the OTP code sent:

	curl -d @params.json -H "Content-Type: application/json -X POST https://one-shot.developers.uanataca.com/api/v1/sign/1464

params.json for Uanataca SMS:

	{
		"secret": "123456"
	}

params.json for other authentication methods:

	{
		"secret": "123456"
		"ext_unlock_type": "biometric"
		"ext_unlock_value": "12345678-12345678"
	}

</br>

A successful call will result in the following response:

	{
		"status": "200 OK",
		"details": "Documents correctly signed"
	}

</br>

> **STEP 7: RETRIEVE SIGNED DOCUMENT**

</br>

API reference: <a href="#tag/Documents/paths/~1api~1v1~1document~1{pk}~1{type}~1{uid}/get">Retrieve Document</a>

Once the signature is done, the next step is to get all signed documents.

To do this, query with an HTTP GET request the endpoint /api/v1/document/{pk}/{type}/{uid}, where {pk} is the Request's unique identifier, {type} is the type of the document (it can be "original" for the uploaded document or "signed" for the digitally-signed version) and {uid} is the document unique identifier.

	curl -X GET https://one-shot.developers.uanataca.com/api/v1/document/1464/signed/712c29ac-a2dc-4530-8c67-d0f227d8294b

The response obtained by the server will be the document in binary format:

	%PDF
	...

</br>

> **STEP 8: DELETE DOCUMENTS FROM OPTIMIZER**

</br>

**API reference:** <a href="#tag/Documents/paths/~1api~1v1~1documents~1{pk}/delete">Delete All Documents</a>

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">⚠ Since the service does not delete uploaded files unless explicitly requested through an API call, it is <strong>strongly recommended</strong> that you backup any file you want to preserve using an alternative system.</blockquote>

Delete all documents associated to a finished digital signature request.

	curl -X DELETE https://one-shot.developers.uanataca.com/api/v1/documents/1464


</br>


## External Mode

In External mode Video ID, digital evidences are uploaded to an independent Video ID platform. External mode Video ID can be executed in 1 or 2 steps.

</br>

![img](https://raw.githubusercontent.com/UANATACA/ONESHOT-VIDEOID-REPO/raw/main/img/oneshot_ext.png)

</br>

*This process involves the following steps:*


**1) CREATE A NEW VIDEO ID SIGNATURE REQUEST**

**2) UPLOAD DATA & VIDEO**

**3) REQUEST VALIDATION**

**4) REQUEST APPROVAL**

**5) UPLOAD A DOCUMENT**

**6) RETRIEVE SERVICE CONTRACT**

**7) GENERATE AN OTP (only for Uanataca SMS)**

**8) SIGN THE DOCUMENT**

**9) RETRIEVE SIGNED DOCUMENT**

**10) DELETE DOCUMENTS FROM OPTIMIZER**

</br>

> **STEP 1: CREATE A NEW VIDEO ID SIGNATURE REQUEST**

</br>

**API Reference:** <a href="#tag/Video-ID/paths/~1api~1v1~1videoid/post">Create Video ID Request</a>

This call must include preliminary information to identify the signer. 

	curl -i -X POST https://one-shot.developers.uanataca.com/api/v1/videoid \
		-H 'Content-Type: application/json' \
		-d '{
			"mobile_phone_number": "+34699999999",
			"email": "mail@domain",
			"registration_authority": "139",
			"profile": "PFnubeQAFCiudadano",
			"videoid_mode": 1
		}'

If the signature request is completed successfully, we will get the unique identifier assigned to it:

	{
		"status": "200 OK",
		"details": {
			"videoid_pk": 150,
			"videoid_link": "",
			"request_pk": 45836
		}
	}

The response is the a JSON containing important request parameters, in **VIDEOPENDING** status after creation. The request_pk output parameter will be used to identify this digital signature request in subsequent calls.

If request data needs to be modified, use the <a href="#tag/Video-ID/paths/~1api~1v1~1request~1{request_pk}/put">Update Request</a> call. Check API Reference.

If request data needs to be retrieved, use the <a href="#tag/Requests/paths/~1api~1v1~1request~1{request_pk}/get">Get Request</a> call. Check API Reference.

</br>

> **STEP 2: UPLOAD DATA & VIDEO**

</br>

A previously created Video ID Request needs a set of information defined as evidences. 

- The successful upload of **ALL** information will change the request status to **VIDEOREVIEW**. 
- The partial upload of the information will change the request status to **VIDEOINCOMPLETE**.
- If the upload process fails for any reason, the request status will change to **VIDEOERROR**. 

Data and images are uploaded by using the Upload Data Evidence call.

</br>

**API Reference:** <a href="#tag/Video-ID/paths/~1api~1v1~1videoid~1{request_pk}~1evidences/post">Upload Data Evidence</a>

Data objects in detail:

`acceptance` : Client acceptance parameters (e.g. Terms & Conditions,  Privacy Policy). This is a customizable JSON object. </br>
`videoid_data` : Set of information about the Request. Contains: </br>
- `images`: Pictures associated to the client's ID document plus a face selfie of him/her. </br>
- `ocr_data` : Text information extracted from the client's ID document via Optical Character Recognition (OCR). </br>
- `security_checks` : Set of validation fields associated to the client's identity (underaging, matching info, liveliness, etc) </br>
- `similarity_level` : Similarity level between document picture and face selfie. Ranges within 0 to 100] 

	curl -i -X POST https://one-shot.developers.uanataca.com/api/v1/videoid/45836/evidences \
		-H 'Content-Type: application/json' \
		-d '{
			"acceptance": {
				"description": "User Accepted Terms and Conditions and Privacy Policy",
				"url-doc-privacypolicy": "https://www.uanataca.com/public/pki/privacidad-PSC/",
				"ip": "186.0.91.53",
				"url-web-videoid": "https://cms.access.bit4id.org:13035/lcmpl/videoid/46b92251-4ba8-4930-a5aa-8631ec4666b6",
				"user-agent": "Mozilla/5.0 (Linux; Android 11; AC2003)",
				"date": 1622823879708,
				"url-doc-termsconditions": "https://www.uanataca.com/public/pki/terminos-VID/"
			},
			"videoid_data": {
				"images": {
					"document_front": "/9j/4AAQSkZJRgABAQAAAQABAAD/2wBDAAIBAQEBAQIBAQECAgICAgQDAgICAgUEBAM (...)",
					"document_rear": "/I7ye60+aOKS0mVGVSD9RVfyXukjmnS3cAEbpMVm6M1ncWqS3FszptO1lPRRDJ+orI8b (...)",
					"document_photo": "AkjOOwFfHFrrNlpXxcbU9QuIIIkvR56yddgHpX3GEj1PmanmdS/xV1ySVlv/AIbXLPO (...)",
					"document_owner": "SSVnovgCZ4Lhk+R3lJPUDJr5t/Z/wBV1DWfjRbeI75B5iQytcykc7yMEAV2/iwC0T34 (...)"
				},
				"ocr_data": {
					"given_name": "Name",
					"surname_1": "Surname 1",
					"surname_2": "Surname 2",
					"mobile_phone_number": "+34999999999",
					"email": "mail@domain",
					"serial_number": "A9999999E",
					"id_document_type": "IDC",
					"id_document_country": ES
				},
				"security_checks": {
					"otp_validation": true,
					"documents_match": true,
					"data_integrity": true,
					"document_notcopy": true,
					"document_notexpired": true,
					"document_notunderage": true,
					"liveliness": true
				},
				"similarity_level": "high"
			}
		}'


Successful response status

	{
		"status": "200 OK",
		"details": "Videoid evidences uploaded"
	}

</br>

In the same way, binary multiformat Video is uploaded by using the Upload Video call.

</br>

**API Reference:** <a href="#tag/Video-ID/paths/~1api~1v1~1videoid~1{request_pk}~1evidences~1video/post">Upload Video</a>

	curl -i -X POST https://one-shot.developers.uanataca.com/v1/upload/video/30e57b02819a430d8386fd85be9f499f/ \
		-H 'Content-Type: multipart/form-data' \
		-F video=@sample_folder/sample_video.mp4 

Successful response status

	{
		"status": "200 OK",
		"details": "Videoid video upload completed"
	}

If the uploaded video needs to be retrieved, use <a href="#tag/Video-ID/paths/~1api~1v1~1download~1video~1{video_identifier}/get">Download Video</a>


</br>

> **STEP 4: REQUEST VALIDATION** `2-step mode only`

</br>

**API Reference:** <a href="#tag/Video-ID/paths/~1api~1v1~1videoid~1{request_pk}~1validate/post">Validate Request</a>

A Registration Authority Officer must validate the request data and evidences before approval. This call is used only for 2-step mode.  

	curl -i -X POST https://api.uanataca.com/api/v1/videoid/45836/validate \
		-H 'Content-Type: application/json' \
		-d '{
			"username": "5012345",
			"password": "Gy6F37xK",
			"pin": "belorado74",
			"rao": "1400"
		}'

	OR

	curl -i -X POST 'https://api.uanataca.com/api/v1/request/45836/validate' \
		-H 'Content-Type: application/json' \
		-d '{
			"token": "f734066d1ce36f9cae4d55be4cdac50e"
		}'	

In case of using a token containing RAO's credentials.

The validation successful response status is a JSON object containing request information, as the request status changes to **CREATED**.

	{
		"status": "200 OK",
		"details": "Videoid validated"
	}

For unsuccessful validations leading to a request refusal, the corresponding call is  <a href="#tag/Video-ID/paths/~1api~1v1~1videoid~1{request_pk}~1refuse/post">Refuse Request</a>. Check API Reference.

</br>

> **STEP 5: REQUEST APPROVAL**

</br>

**API Reference:** <a href="#tag/Video-ID/paths/~1api~1v1~1videoid~1{request_pk}~1validate/post">Approve Request</a>

This call makes the request ready for signature. Its status changes to **ENROLLREADY**. In 1-step mode, both validation and approval occur when executing this call.

	curl -i -X POST 'https://api.uanataca.com/api/v1/request/45836/approve' \
		-H 'Content-Type: application/json' \
		-d '{
			"username": "1000279",
			"password": "3DPTm:N4",
			"pin": "23bYQq9a",
			"rao": 972
		}'

The response is a JSON object with added request approval information. 

	{
		"status": "200 OK",
		"details": "Request approved successfully"
	}

In case of not approving a request for any reason, the call <a href="#tag/Requests/paths/~1api~1v1~1requests~1{id}~1cancel/delete">Cancel Request</a> must be executed. Check API Reference.

</br>

> **STEP 6: UPLOAD A DOCUMENT**

</br>

**API Reference:** <a href="#tag/Documents/paths/~1api~1v1~1document~1{pk}/post">Upload Document</a>


After creating the the signature request, we can associate all pdf documents that must be signed by the user.

	curl -F "file=@doc.pdf" -X POST https://one-shot.developers.uanataca.com/api/v1/document/1464

Note that the number at the end of the call is the request id we obtained in the previous step.

If the upload is successful, the response will contain the identifier assigned to each document:

	{
		"status": "200 OK",
		"details": "712c29ac-a2dc-4530-8c67-d0f227d8294b"
	}

</br>

> **STEP 7: RETRIEVE SERVICE CONTRACT**

</br>

**API Reference:** <a href="#tag/Documents/paths/~1api~1v1~1document~1{pk}~1contract/get">Retrieve Contract</a>

As a Trusted Service Provider, Uanataca must inform certificate requesters about terms and conditions ruling the issuance of certificates. 

A service contract is generated for each digital certificate issue. The user must view the request-associated service contract to be signed in the same step as the documents, using the one-time certificate just issued.

	curl -X GET https://one-shot.developers.uanataca.com/api/v1/document/1464/contract

The response by the server will be the service contract document file in binary format:

	%PDF
	...

</br>

> **STEP 8: GENERATE AN OTP (only for Uanataca SMS)**

</br>

**API Reference:** <a href="#tag/Requests/paths/~1api~1v1~1otp~1{pk}/post">Generate OTP code</a>

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">This step applies only for signatures made using Uanataca SMS method.</blockquote>

Once the documents to be signed are ready, we need to generate a secure One-time password (OTP) that allows the user to sign them. The OTP code is generated by executing the **Generate OTP** call and the resulting OTP is sent as an SMS message directly to the phone number is provided when creating the signature request.

When calling the OTP endpoint you must add the request identifier returned by the **Create Request** call:

	curl -X POST https://one-shot.developers.uanataca.com/api/v1/otp/1464

A successful call will look like this:

	{
		"status": "200 OK",
		"details": "OTP generated"
	}

With this call, an SMS with the secret code is sent to the mobile phone number associated to the signature request.


</br>

> **STEP 9: SIGN THE DOCUMENT**

</br>

**API Reference:** <a href="#tag/Requests/paths/~1api~1v1~1sign~1{pk}/post">Sign</a>

In this step the digital signature certificate is issued, then it signs all documents previously uploaded for the signature request.

Execute the sign call with the request id and json parameters containing the OTP code sent:

	curl -d @params.json -H "Content-Type: application/json -X POST https://one-shot.developers.uanataca.com/api/v1/sign/1464

params.json for Uanataca SMS:

	{
		"secret": "123456"
	}

params.json for other authentication methods:

	{
		"secret": "123456"
		"ext_unlock_type": "biometric"
		"ext_unlock_value": "12345678-12345678"
	}

</br>

A successful call will result in the following response:

	{
		"status": "200 OK",
		"details": "Documents correctly signed"
	}

</br>

> **STEP 10: RETRIEVE SIGNED DOCUMENT**

</br>

API reference: <a href="#tag/Documents/paths/~1api~1v1~1document~1{pk}~1{type}~1{uid}/get">Retrieve Document</a>

Once the signature is done, the next step is to get all signed documents.

To do this, query with an HTTP GET request the endpoint /api/v1/document/{pk}/{type}/{uid}, where {pk} is the Request's unique identifier, {type} is the type of the document (it can be "original" for the uploaded document or "signed" for the digitally-signed version) and {uid} is the document unique identifier.

	curl -X GET https://one-shot.developers.uanataca.com/api/v1/document/1464/signed/712c29ac-a2dc-4530-8c67-d0f227d8294b

The response obtained by the server will be the document in binary format:

	%PDF
	...

</br>

> **STEP 11: DELETE DOCUMENTS FROM OPTIMIZER**

</br>

**API reference:** <a href="#tag/Documents/paths/~1api~1v1~1documents~1{pk}/delete">Delete All Documents</a>

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">⚠ Since the service does not delete uploaded files unless explicitly requested through an API call, it is <strong>strongly recommended</strong> that you backup any file you want to preserve using an alternative system.</blockquote>

Delete all documents associated to a finished digital signature request.

	curl -X DELETE https://one-shot.developers.uanataca.com/api/v1/documents/1464


</br>

# Configuration

One-Shot Optimizer can be supplied as a **Docker** or as a **Virtual Machine** image.
See the configuration description in:<br>
<a href="#section/Configuration/One-Shot-Optimizer-on-Docker"> One-Shot Optimizer on Docker</a><br>
<a href="#section/Configuration/One-Shot-Optimizer-on-Virtual-Machine-(OVA)"> One-Shot Optimizer on Virtual Machine</a>


## Hardware requirements


**CPU:** modern multicore (minimum 4 core)

**RAM:** 8GB

**HDD:** 200 GB


## One-Shot Optimizer on Docker


This configuration requires a server with a Linux CentOS operating system.

<a href="#section/Video-tutorials/Docker-Optimizer-Configuration"><img src="https://raw.githubusercontent.com/UANATACA/ONESHOT-VIDEOID-REPO/main/img/img4.png"></a><a href="#section/Video-tutorials/Docker-Optimizer-Configuration"><b>&nbsp;Watch on video!</b></a>


> STEP 1: Install Docker and Docker-Compose.

*Docker*

Run the following commands in this order.

	sudo yum update -y
	yum install -y yum-utils device-mapper-persistent-data lvm2
	yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
	sudo yum install -y docker-ce docker-ce-cli containerd.io
	sudo systemctl start docker



*Docker-Compose*

Run the following commands in this order.


	sudo curl -L "https://github.com/docker/compose/releases/download/1.28.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
	chmod +x /usr/local/bin/docker-compose


Run command **docker-compose version** to check the installation. The outcome should show this information:


![img](https://github.com/UANATACA/ONESHOT-VIDEOID-REPO/raw/main/img/oneshot-docker1.png)

</br>

> STEP 2: Extract and copy One-Shot Optimizer zip content to the server.

Extract all `oneshot_optimizer_docker.zip` content in a local folder.

Move One-Shot Optimizer folder to the path **/opt** in the server.

The outcome should look like this:

![img](https://github.com/UANATACA/ONESHOT-VIDEOID-REPO/raw/main/img/oneshot-docker2.png)


</br>

> STEP 3: Mapping volumes (for environments with a pool of One-Shot Optimizer).

In high performance environments working with a pool of Optimizers, service settings, data and logs must be stored in a shared volume outside Optimizer servers. These volumes must be defined in the `docker-compose.yml` file for each One-Shot Optimizer.

	cd /opt/oneshot_optimizer

Docker-compose.yml settings file:

![img](https://github.com/UANATACA/ONESHOT-VIDEOID-REPO/raw/main/img/oneshot-docker3.png)

</br>

> STEP 4: Load the One-Shot Docker images.

Run the following commands:

	cd /opt/oneshot_optimizer
	docker image load -i oneshot_optimizer.tar
	docker image load -i oneshot_imgconverter.tar

Remove image files:

	rm -rf /opt/oneshot_optimizer/oneshot_optimizer.tar
	rm -rf /opt/oneshot_optimizer/oneshot_imgconverter.tar

</br>

> STEP 5: Launch the service.

Run the following commands:

	cd /opt/oneshot_optimizer
	docker-compose up -d

Check service status:

	docker-compose ps

					Name              			  Command               State                Ports
	--------------------------------------------------------------------------------------------------------------------------------------------------
	oneshot_optimizer_imgconverter_1   /opt/bit4id/imgconverter/b ...   Up      0.0.0.0:49153->5013/tcp,:::49153->5013/tcp
	oneshot_optimizer_nginx_1          /docker-entrypoint.sh ngin ...   Up      0.0.0.0:443->443/tcp,:::443->443/tcp, 0.0.0.0:80->80/tcp,:::80->80/tcp
	oneshot_optimizer_optimizer_1      oneshot_optimizer start          Up

All services must be UP.

</br>

> STEP 6: Service settings.

See <a href="#section/Configuration/Service-settings">service settings</a> configuration section.

</br>


## One-Shot Optimizer on Virtual Machine

<div style="text-align: justify">
The Virtual Machine is supplied in an OVA file. One-Shot Optimizer image is compatible with common virtual environments such as VMWare, AWS, Azure or VirtualBox.
</div>
</br>

> STEP 1: Import One-Shot Optimizer (VM) in the virtual environment.

<div style="text-align: justify">
Adjust the system requirements for optimal usage considering host terminal resources described in <a href="#section/Configuration/Hardware-requirements"> hardware requirements</a>.
</div>

</br>

> STEP 2: Network configuration.

The network settings are configured in the file `ifcfg-ens33`, which can be found in the path **/etc/sysconfig/network-scripts**. Edit the file and insert the correct IP address, network mask, gateway and DNS for your network.

Then restart network services with command **service network restart**.

Example:

![img](https://github.com/UANATACA/ONESHOT-VIDEOID-REPO/raw/main/img/oneshot-docker4.png)

</br>

> STEP 3: Service settings.

See <a href="#section/Configuration/Service-settings">service settings</a> configuration section.

</br>


## Service settings

<div style="text-align: justify">
One-Shot Signature can be configured for usage over test or production environment.
</div>
<br></br>


> TEST environment

**Requirements:**

- Billing credentials for Uanataca test environment.

The `settings.ini` file contains default parameters that can also be adjusted via API using <a href='#tag/Settings/paths/~1api~1v1~1settings/post'>Update Settings</a> call.

Except by `tsa_url`, all parameters shown below are replaced every time the <a href='#tag/Video-ID/paths/~1api~1v1~1videoid/post'>Create Video ID Request</a> call is executed. 

settings.ini file location:

**/opt/oneshot_optimizer/common/etc/settings.ini** or custom mapped volume (Docker)</br>
**/opt/bit4id/oneshot_optimizer/etc/settings.ini** (Virtual Machine)

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">⚠ If billing password is specified, it must be previously converted into Base64 format.</blockquote>

Run the following command to convert your password to base64:

	echo -n <<billing_password>> | base64

</br>

The following is a view of the settings.ini file: 

	1 |	[general]
	2 |	environment = test
	3 |
	4 |	[tsa]
	5 |	tsa_url = https://tsa.access.bit4id.org:13035/tsa/test01
	6 |
	7 |	[billing]
	8 |	billing_username = user@uanataca.com
	9 |	billing_password = ejVxTnFrZkI=
	10|
	11|	[request]
	12|	default_profile = PFnubeQAFCiudadano
	13|	default_ra = 1000

Once you are done editing the file, restart the One-Shot Optimizer service to make changes take effect.

Virtual Machine:

	systemctl restart optimizer

Docker:

	cd /opt/oneshot_optimizer
	docker-compose restart optimizer


<br></br>

> PRODUCTION environment

**Requirements:**

- Billing credentials for Uanataca production environment.
- Certificate (.cer) and key (.key) files for connection to the Uanataca production environment.
- The Id number for the Registration Authority that will issue the certificates.

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">⚠ We recommend starting from a clean copy of One-Shot Optimizer for this step. This prevents test environment leftover files from causing errors in production.</blockquote>

You should have received a certificate (.cer) and a key (.key) file to be used to identify your application in communications with the production signature service. Place both files in the certificates folder.

settings.ini file location:

**/opt/oneshot_optimizer/common/etc/certs/prod** or custom mapped volume (Docker)</br>
**/opt/bit4id/oneshot_optimizer/etc/certs/prod** (Virtual Machine)

![img](https://github.com/UANATACA/ONESHOT-VIDEOID-REPO/raw/main/img/oneshot-docker5.png)

The following is a view of the settings.ini file. **Important: If billing password is specified, it must be previously converted into Base64 format.** Run the following command to convert your password to base64:

	echo -n <<billing_password>> | base64

</br>

settings.ini file:

	1 |	[general]
	2 |	environment = prod
	3 |
	4 |	[tsa]
	5 |	tsa_url = https://tsa.uanataca.com/tsa/tss03
	6 |
	7 |	[billing]
	8 |	billing_username = user@uanataca.com
	9 |	billing_password = ejVxTnFrZkI=
	10|
	11|	[request]
	12|	default_profile = PFnubeQAFCiudadano
	13|	default_ra = 1000


The file `settings.ini` contains default parameters that can also be adjusted via API using the <a href='#tag/Settings/paths/~1api~1v1~1settings/post'>Update Settings</a> call.

Except by `tsa_url`, all parameters shown below are replaced every time the <a href='#tag/Video-ID/paths/~1api~1v1~1videoid/post'>Create Video ID Request</a> call is executed.  

Once you are done editing the file, restart the One-Shot Optimizer service to changes take effect.

Virtual Machine:

	systemctl restart optimizer

Docker:

	cd /opt/oneshot_optimizer
	docker-compose restart optimizer

<br></br>

# Webhook Configuration


One-Shot API requires a Webhook implemented on customer business side to manage our service callbacks. Every request status change will trigger a simple event-notification via HTTP POST, consisting on a JSON object to an URL that must be explicitly included as a **required parameter** in the <a href='#tag/Video-ID/paths/~1api~1v1~1videoid/post'>Create Video ID Request</a> call, when using Uanataca 1-step or 2-step mode. 

The following is a sample view of the JSON object that is sent as a callback at every status change:

	{
		"status": "VIDEOINCOMPLETE", 
		"date": "2021-07-20T08:08:21.132394", 
		"previous_status": "VIDEOPENDING", 
		"request": 46760, 
		"registration_authority": 139
	}

Where:

**status** is the most recent status, this is, the status that triggered the notification.</br>
**date** is the date of the request status change in datetime format.</br>
**previous_status** is the status inmediately previous to last change.</br>
**request** is the request unique id.</br>
**registration_authority** is the Registration Authority id number the request is associated.</br>

</br>

> **Sample code**

In this sample, every JSON object is stored in a file named 'videoid'.

The webhook parameter used in the <a href='#tag/Video-ID/paths/~1api~1v1~1videoid/post'>Create Video ID Request</a> call is defined as:

	{host}/videoid

where {host} is the IP or domain from the server exposing the webhook.

</br>

*Python*

	import web
	import datetime
	
	urls = (
	        '/videoid, 'videoid',
	        )
	
	app = web.application(urls, globals())
	app = app.wsgifunc()
	
	class video:
		def POST(self):
			data = web.data()
			f = open("status.json",'a+')
			f.write(data)
			f.close()
			return ''

	if __name__ == "__main__":
	    app.run()


*PHP*

	<?php
	
	//videoid.json

	$post = file_get_contents('php://input',true);
	$file_handle = fopen('/videoid/status.json', 'w');
	fwrite($file_handle, $post);
	fclose($file_handle);
	
	?>


# Logs

Service logs file `optimizer.log` is stored in a One-Shot Optimizer local folder.

Docker path:

**/opt/oneshot_optimizer/common/logs** or custom mapped volume

Virtual Machine path:

**/opt/bit4id/oneshot_optimizer/logs**



# Postman collection

A postman collection is available as a support for a quick start.<br>
It is only required to edit `host`variable in Postman environment with the IP or domain of One-Shot Optimizer.

<a href="https://cdn.bit4id.com/es/uanataca/public/oneshot/Uanataca_One-Shot_Postman.zip">One-Shot Postman collection download</a>

<div id="APIReference" style="padding-top: 60px;"><h1>API Reference<h1></div>


