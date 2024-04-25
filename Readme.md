## Definition

- SSRF is a web application security vulnerability that allows the attacker to force the server to make unauthorised reqests to any local or external source on behalf of the web server.

- SSRF allows an attacker to interact with internal systems, potentially leading to data leaks, service disruption, or even remote code execution.

## Risk of SSRF

    1. Data Exposure
        As explained earlier, cybercriminals can gain unauthorised access by tampering with requests on behalf of the vulnerable web application to gain access to sensitive data hosted in the internal network.
    
    2. Reconnaissance
        An attacker can carry out port scanning of internal networks by running malicious scripts on vulnerable servers or redirecting to scripts hosted on some external server.
    
    3. Denial of Service
        It is a common scenario that internal networks or servers do not expect many requests; therefore, they are configured to handle low bandwidth. Attackers can flood the servers with multiple illegitimate requests, causing them to remain unavailable to handle genuine requests.


## Types of SSRF

1. Basic SSRF
    Basic SSRF is a web attack technique where an attacker tricks a server into making requests on their behalf, often targeting internal systems or third-party services. By exploiting vulnerabilities in input validation, the attacker can gain unauthorised access to sensitive information or control over remote resources, posing a significant security risk to the targeted application and its underlying infrastructure.

2. Blind SSRF
     Blind SSRF refers to a scenario where the attacker can send requests to a target server, but they do not receive direct responses or feedback about the outcome of their requests. In other words, the attacker is blind to the server's responses. 

     1. Out-of-band SSRF - Blind
        Out-of-band SSRF is a technique where the attacker leverages a separate, out-of-band communication channel instead of directly receiving responses from the target server to receive information or control the exploited server. This approach is practical when the server's responses are not directly accessible to the attacker.

        For instance, the attacker might manipulate the vulnerable server to make a DNS request to a domain he owns or to initiate a connection to an external server with specific data. This external interaction provides the attacker with evidence that the SSRF vulnerability exists and potentially allows him to gather additional information, such as internal IP addresses or the internal network's structure.  


     2. Semi-Blind SSRF (Time-based)
        Time-based SSRF is a variation of SSRF where the attacker leverages timing-related clues or delays to infer the success or failure of their malicious requests. By observing how long it takes for the application to respond, the attacker can make educated guesses about whether their SSRF attack was successful. 

        The attacker sends a series of requests, each targeting a different resource or URL. The attacker measures the response times for each request. If a response takes significantly longer, it may indicate that the server successfully accessed the targeted resource, implying a successful SSRF attack.

    
## Remediation

    1. Implement strict input validation and sanitise all user-provided input, especially any URLs or input parameters the application uses to make external requests.
    
    2. Instead of trying to blocklist or filter out disallowed URLs, maintain allowlists of trusted URLs or domains. Only allow requests to these trusted sources.
    
    3. Implement network segmentation to isolate sensitive internal resources from external access.
    
    4. Implement security headers, such as Content-Security-Policy, that restricts the application's load of external resources.
    
    5. Implement strong access controls for internal resources, so even if an attacker succeeds in making a request, they can't access sensitive data without proper authorisation.

    6. Implement comprehensive logging and monitoring to track and analyse incoming requests. Look for unusual or unauthorised requests and set up alerts for suspicious activity.

## Testing methodology

    1. Check application fuctionality where one applications needs to request data from another application.
    2. Check whether any URL is being passed in request which can be tampered.
    3. Check whether blacklisting or whitelisting is done using different urls.
    4. Try to bypass blacklist
        - By changing url to 
            http://127.1/admin
            http://127.1./%2523dmin --> "a" is double url encoded

    5. Fuzz with wordlist to check if application is vulnerable.
            

