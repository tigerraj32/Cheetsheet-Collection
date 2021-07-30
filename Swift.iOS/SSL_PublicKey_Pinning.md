# SSL Pinning Or Public Key Pinning

Most of the client application (mobile apps, or a web client) get its data from server  over `https`.  When the ap exchange information, they typically use the `Transport Layer Security (TLS)` protocol to provide secure connection. Even if `TLS` protect the transmitted data from temparing, attackers can setup  `man-in-the-middle` attacks using self-signed certificates. With these certificate they can capture data moving to and from your apps. Detail tutorial on how we can use `man-in-the-middle` attack  here [Using Charles Proxy to Debug HTTPS connection](https://github.com/tigerraj32/Cheetsheet-Collection/blob/master/CharlesProxy.md)

To protect this `man-in-the-middle` attack it is required we imppement `SSL Certificate Pinning`.

**So what is `Certificate Pinning` ?**

`Certificate Pinning ` force your client application to validate the server's certificate agains a known copy of server certificaet while making secure communication with server. 


# How does SSL Works ?

There are multiple events occuring when making a single `https` connection.

![](../resources/tls_connection.jpg)

- Client machine sends a connection request to server, server listens the request.
- Server gives response including public key and certificate.
- Client checks the certificate and sends a encrypted key to server.
- Server decrypt the key and sends encrypted data back to the client machine.
- Client receives and decrypt the encrypted data.


# Types  of SSL Pinning

- **Pin the certificate:**

   You can download the server’s certificate and bundle it into your app. At runtime, the app compares the server’s certificate to the one you’ve embedded. 
   
  - Disadvantage:
  
    This is not the preffred method for mobile application because typically every certificate have a validity life span. Once expired we need to renew or generate new one. This will require us to bundle this certificate in app again and release new build. If not done on time then app will stop working

- **Pin the public key:**
  
   You can retrieve the certificate’s public key and include it in your code as a string. At runtime, the app compares the certificate’s public key to the one hard-coded in your code.

  - Advantage

    This is the preffered method of `SSL Pinning` for mobile appilcation. Because the `public key hash` is never going to change.



# How to get SSL Certificate

Here I am going to demonstrate SSL Pinning for getting github repoitoty list using `github public api`.  There are multiple ways we can extract `Public SLL Certificate`

**Method 1:** From Browser

- Open the browser and hit `github.com`
- In the address bar you will see the lock icon indicating that this is a secure site.
- Click on this `lock icon`, this will open a window that shows the detail of certificate. 
- Click the `root certificate` and drag it to `Desktop`
- Now you have a `ssl certificate` which you can bundle into the application to implement `ssl pinning`

![](../resources/extract_ssl_crt.gif)

**Method 2:** From Terminal

We can use `openssl` and `s_client` to generate the certificate.   First of all copy and paste below command in terminal

```
openssl s_client -connect github.com:443 </dev/null  
```

Once it completes, you'll receive a lot of data including a list of certificates. Each certificate in the chain has a Common Name (CN).

```
Certificate chain
 0 s:/C=US/ST=California/L=San Francisco/O=GitHub, Inc./CN=github.com
   i:/C=US/O=DigiCert, Inc./CN=DigiCert High Assurance TLS Hybrid ECC SHA256 2020 CA1
 1 s:/C=US/O=DigiCert, Inc./CN=DigiCert High Assurance TLS Hybrid ECC SHA256 2020 CA1
   i:/C=US/O=DigiCert Inc/OU=www.digicert.com/CN=DigiCert High Assurance EV Root CA
```
Below that, you can see the actual certificate you're interested in, which is the one where CN is githubcom.

```
Server certificate
-----BEGIN CERTIFICATE-----
MIIFBjCCBK2gAwIBAgIQDovzdw2S0Zbwu2H5PEFmvjAKBggqhkjOPQQDAjBnMQsw
CQYDVQQGEwJVUzEXMBUGA1UEChMORGlnaUNlcnQsIEluYy4xPzA9BgNVBAMTNkRp
Z2lDZXJ0IEhpZ2ggQXNzdXJhbmNlIFRMUyBIeWJyaWQgRUNDIFNIQTI1NiAyMDIw
IENBMTAeFw0yMTAzMjUwMDAwMDBaFw0yMjAzMzAyMzU5NTlaMGYxCzAJBgNVBAYT
AlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1TYW4gRnJhbmNpc2Nv
MRUwEwYDVQQKEwxHaXRIdWIsIEluYy4xEzARBgNVBAMTCmdpdGh1Yi5jb20wWTAT
BgcqhkjOPQIBBggqhkjOPQMBBwNCAASt9vd1sdNJVApdEHG93CUGSyIcoiNOn6H+
udCMvTm8DCPHz5GmkFrYRasDE77BI3q5xMidR/aW4Ll2a1A2ZvcNo4IDOjCCAzYw
HwYDVR0jBBgwFoAUUGGmoNI1xBEqII0fD6xC8M0pz0swHQYDVR0OBBYEFCexfp+7
JplQ2PPDU1v+MRawux5yMCUGA1UdEQQeMByCCmdpdGh1Yi5jb22CDnd3dy5naXRo
dWIuY29tMA4GA1UdDwEB/wQEAwIHgDAdBgNVHSUEFjAUBggrBgEFBQcDAQYIKwYB
BQUHAwIwgbEGA1UdHwSBqTCBpjBRoE+gTYZLaHR0cDovL2NybDMuZGlnaWNlcnQu
Y29tL0RpZ2lDZXJ0SGlnaEFzc3VyYW5jZVRMU0h5YnJpZEVDQ1NIQTI1NjIwMjBD
QTEuY3JsMFGgT6BNhktodHRwOi8vY3JsNC5kaWdpY2VydC5jb20vRGlnaUNlcnRI
aWdoQXNzdXJhbmNlVExTSHlicmlkRUNDU0hBMjU2MjAyMENBMS5jcmwwPgYDVR0g
BDcwNTAzBgZngQwBAgIwKTAnBggrBgEFBQcCARYbaHR0cDovL3d3dy5kaWdpY2Vy
dC5jb20vQ1BTMIGSBggrBgEFBQcBAQSBhTCBgjAkBggrBgEFBQcwAYYYaHR0cDov
L29jc3AuZGlnaWNlcnQuY29tMFoGCCsGAQUFBzAChk5odHRwOi8vY2FjZXJ0cy5k
aWdpY2VydC5jb20vRGlnaUNlcnRIaWdoQXNzdXJhbmNlVExTSHlicmlkRUNDU0hB
MjU2MjAyMENBMS5jcnQwDAYDVR0TAQH/BAIwADCCAQUGCisGAQQB1nkCBAIEgfYE
gfMA8QB2ACl5vvCeOTkh8FZzn2Old+W+V32cYAr4+U1dJlwlXceEAAABeGq/vRoA
AAQDAEcwRQIhAJ7miER//DRFnDJNn6uUhgau3WMt4vVfY5dGigulOdjXAiBIVCfR
xjK1v4F31+sVaKzyyO7JAa0fzDQM7skQckSYWQB3ACJFRQdZVSRWlj+hL/H3bYbg
IyZjrcBLf13Gg1xu4g8CAAABeGq/vTkAAAQDAEgwRgIhAJgAEkoJQRivBlwo7x67
3oVsf1ip096WshZqmRCuL/JpAiEA3cX4rb3waLDLq4C48NSoUmcw56PwO/m2uwnQ
prb+yh0wCgYIKoZIzj0EAwIDRwAwRAIgK+Kv7G+/KkWkNZg3PcQFp866Z7G6soxo
a4etSZ+SRlYCIBSiXS20Wc+yjD111nPzvQUCfsP4+DKZ3K+2GKsERD6d
-----END CERTIFICATE-----
subject=/C=US/ST=California/L=San Francisco/O=GitHub, Inc./CN=github.com
issuer=/C=US/O=DigiCert, Inc./CN=DigiCert High Assurance TLS Hybrid ECC SHA256 2020 CA1
```

To copy the certificate into a file, use openssl again. Repeat the previous command and pass its output to openssl x509, specify DER encoding and output it to a new file named github.com.der:

```
openssl s_client -connect api.github.com:443 </dev/null  | openssl x509 -outform DER -out api.github.com.der
```


# How Do I Pin Certs in My Mobile App?

### For iOS

First, add your certificate into your ios project.  

For this example I am going to make a call to `github api` to list the my repository. Here github uses SSL and now I want to pin github's ssl certificate when making https request. To get the certificate follow the above steps. Before we dive into coding,  let's first create github personal token to pass as authorization  token when making api calls. [Get Personal Token](https://github.com/tigerraj32/Cheetsheet-Collection/blob/master/git/git-token.md)

Now let's create a swift class `NetworkManager` as following

```swift 
enum PinningOption {
    case certificate
    case publicKey
}


class NetworkManager: NSObject,URLSessionDelegate{
    
    let pinningOption: PinningOption
    
    init(option: PinningOption) {
        self.pinningOption = option
    }
    
    func apiCall() {
        
        let urlString = "https://api.github.com/user/repos"
        guard let url = URL(string: urlString) else {
            print("Invalid URL")
            return
        }
        
        var request = URLRequest(url: url,timeoutInterval: Double.infinity)
        request.addValue("Bearer <github token>", forHTTPHeaderField: "Authorization")
        request.httpMethod = "GET"
        request.setValue("application/json", forHTTPHeaderField: "Content-Type")
        
        let session = URLSession(configuration: .ephemeral, delegate: self, delegateQueue: nil)
        let task = session.dataTask(with: request) { data, response, error in
            
            if error != nil {
                print(error!.localizedDescription)
            }else{
                guard let data = data else {
                    print(String(describing: error))
                    
                    return
                }
                
                print(data.prettyPrintedJSONString!)
                
            }
        }
        
        task.resume()
    }
}
```

In the above code there is nothing fancy going on. We have a class properties `pinningOption` to indicate whether we want `certificate pinning` or `public key pinning`. And we have a method `apiCall` that connect to github's api server and get my repository list. Here user info is included in github personal token that we use in Authorization header.  For `ssl pinning` the most important part here is `URLSession delegate` we set in 

> URLSession(configuration: .ephemeral, delegate: self, delegateQueue: nil)

This means NetworkManager class need to conform `URLSessionDelegate`  and implement the below protocol. This is where we verify ssl certificate of public key 

```swift
func urlSession(_ session: URLSession, didReceive challenge: URLAuthenticationChallenge, completionHandler: @escaping (URLSession.AuthChallengeDisposition, URLCredential?) -> Void) {

}
```

Now here's the complete code 

```swift
static let publicKeyHash = "Ues/xSDzybWMEgs2jgMiz7GJUgX1dYZ6gKFXJso8nXU="

func urlSession(_ session: URLSession, didReceive challenge: URLAuthenticationChallenge, completionHandler: @escaping (URLSession.AuthChallengeDisposition, URLCredential?) -> Void) {
        print(">>>>>> didReceive: ", challenge)
        guard let serverTrust = challenge.protectionSpace.serverTrust else {
            completionHandler(.cancelAuthenticationChallenge, nil)
            return
        }
        
        guard let serverCertificate = SecTrustGetCertificateAtIndex(serverTrust, 0) else {
            completionHandler(.cancelAuthenticationChallenge, nil)
            return
        }
        
        switch self.pinningOption {
        //certificate pinning
        case .certificate:
            
            // SSL Policies for domain name check
            let policy = NSMutableArray()
            policy.add(SecPolicyCreateSSL(true, challenge.protectionSpace.host as CFString))
            
            //evaluate server certifiacte
            let isServerTrusted = SecTrustEvaluateWithError(serverTrust, nil)
            
            //Remote certificate Data
            let remoteCertificateData:NSData =  SecCertificateCopyData(serverCertificate)
            
            //let pathToCertificate = Bundle.main.path(forResource: "api.github.com", ofType: "cer")
            let pathToCertificate = Bundle.main.path(forResource: "github.com", ofType: "der")
            let localCertificateData:NSData = NSData(contentsOfFile: pathToCertificate!)!
            //Compare certificates
            if(isServerTrusted && remoteCertificateData.isEqual(to: localCertificateData as Data)){
                let credential:URLCredential =  URLCredential(trust:serverTrust)
                print("Certificate pinning is successfully completed")
                completionHandler(.useCredential, credential)
                
            }else{
                print("Certificate pinning is failed")
                completionHandler(.cancelAuthenticationChallenge, nil)
            }
          
        //public key pinning
        case .publicKey:
            // Server public key
            let serverPublicKey = SecCertificateCopyKey(serverCertificate)
            let serverPublicKeyData = SecKeyCopyExternalRepresentation(serverPublicKey!, nil )!
            let data:Data = serverPublicKeyData as Data
            
            // Server Hash key
            //let serverHashKey = sha256(data: data)
            let serverHashKey = SHA256.hash(data: data).data.base64EncodedString()
            
            
            // Local Hash Key
            let publickKeyLocal = type(of: self).publicKeyHash
            if (serverHashKey == publickKeyLocal) {
                // Success! This is our server
                print("Public key pinning is successfully completed")
                let credential:URLCredential =  URLCredential(trust:serverTrust)
                
                completionHandler(.useCredential, credential)
                
            }else{
                print("Public key pinning is failed ")
                completionHandler(.cancelAuthenticationChallenge, nil)
            }
            
            
            
        }
    }

```

There are multiple ways to get public key  for the second case. 

- First that works for swift code. print the output below code and use it as string constant  public key comparasion.
>  let serverHashKey = SHA256.hash(data: data).data.base64EncodedString()


- Second via terminal

> openssl s_client -connect api.github.com:443 | openssl x509 -pubkey -noout

which produces below output

```
-----BEGIN PUBLIC KEY-----
MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAElL9/+0TidTIALPfU/tiS6pI8zAIk
rU4pohUldVc0bb6O3FARl3cnqIDK9SoF65z3xiR6XsnFS8F0Oy/chXR/kQ==
-----END PUBLIC KEY-----

```

In this case, we will not store the actual certificates in the application. we will create sha256 hashes and store them instead. This has several advantages. its easier to manage due to size, and it allows shipping an application with a hash of a future (or a backup) certificate or key without exposing them ahead of time


**Generate HashJey**

> openssl s_client -servername githu.com -connect api.github.com:443 | openssl x509 -pubkey -noout | openssl pkey -pubin -outform der | openssl dgst -sha256 -binary | openssl enc -base64

which produces following output

```
azE5Ew0LGsMgkYqiDpYay0olLAS8cxxNGUZ8OJU756k=
```

**Special note**
For now the public key we got from  swift code and terminal are different. I need to research further on that. One main reason is described [here](https://stackoverflow.com/questions/64113417/swift-urlsession-returns-a-different-public-key-than-openssl)





Refrences
- https://www.raywenderlich.com/1484288-preventing-man-in-the-middle-attacks-in-ios-with-ssl-pinning