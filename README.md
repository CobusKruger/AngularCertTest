# SSL with NG Serve

This is a test app to show how to serve Angular over SSL when using `ng serve`.

The best reference is: https://medium.com/@richardr39/using-angular-cli-to-serve-over-https-locally-70dab07417c8

# Summary

Summing up the linked article, follow these steps to the letter. There is no point deviating, because it will waste hours of your life.

1. Create a file named `certificate.cnf`, with the following content (taken directly from the article, except for renaming `CN`):

```ini
[req]
default_bits = 2048
prompt = no
default_md = sha256
x509_extensions = v3_req
distinguished_name = dn
[dn]
C = GB
ST = London
L = London
O = My Organisation
OU = My Organisational Unit
emailAddress = email@domain.com
CN = Angular Local Dev
[v3_req]
subjectAltName = @alt_names
[alt_names]
DNS.1 = localhost
```
2. Open Git Bash. If you don't have it, just install Git for Windows. `git-bash.exe` is in the same folder as `git.exe`.
3. In Git Bash, navigate to your chosen folder and execute the following:
```bash
openssl req -new -x509 -newkey rsa:2048 -sha256 -nodes -keyout localhost.key -days 3560 -out localhost.crt -config certificate.cnf
```
4. You will now have two new files in the folder, namely `localhost.key` and `localhost.crt`. Double-click `localhost.crt` and install it in the user's `Trusted Root Certificate Authorities` store.
5. Copy `localhost.key` and `localhost.crt` to your Angular project, under a folder named `certs`. This is a top-level folder, on the same level as `src`.
6. To run `ng serve` with SSL, execute the command like this:
```bash
ng serve --ssl --sslKey ./certs/localhost.key --sslCert ./certs/localhost.crt
```
7. Don't bother trying to specify SSL settings in the `angular.json` file. It doesn't work at all at the time of writing.