# Resolving HTTPS Developer Certificate Trust Issues on Ubuntu

The .NET CLI does not automatically configure certificate trust on Ubuntu, unlike Windows, Even if the development certificate was successfully generated, it is not recognized as trusted by the OS system.  
You will need to manually trust the development certificate by following these steps:

---

## **1. Create the Target Directory**
Ensure the directory for exporting the certificate exists:
```bash
mkdir -p ~/.aspnet/https
```
Set appropriate permissions for security:
```bash
chmod 700 ~/.aspnet/https
```

---

## **2. Export the Certificate**
Run the following command to export the development certificate:
```bash
dotnet dev-certs https -ep ~/.aspnet/https/aspnet-devcert.pfx -p password
```

---

## **3. Verify the Export**
Check that the certificate file was successfully exported:
```bash
ls ~/.aspnet/https/
```
You should see the file `aspnet-devcert.pfx`.

---

## **4. Convert the Certificate to `.crt` Format**
Ubuntu requires the certificate in `.crt` format. Convert it using `openssl`:
```bash
openssl pkcs12 -in ~/.aspnet/https/aspnet-devcert.pfx -out aspnet-devcert.crt -nokeys -passin pass:password
```

---

## **5. Add the Certificate to Trusted Store**
Copy the `.crt` file to the trusted certificates directory:
```bash
sudo cp aspnet-devcert.crt /usr/local/share/ca-certificates/
```
Update the system’s certificate store:
```bash
sudo update-ca-certificates
```

---

## **6. Restart the Browser**
Close and reopen your browser to ensure it picks up the updated certificate store. Access your application again using `https://localhost:<port>`.

---

## **7. Debugging Tips**
If the issue persists:

- Verify that the certificate is in the trusted store:
  ```bash
  ls /usr/local/share/ca-certificates | grep aspnet-devcert.crt
  ```

- Use `curl` to check for SSL errors:
  ```bash
  curl -v https://localhost:5001
  ```

## Author

- website: [Menayer.com](https://www.menayer.com)
- e-mail: <Amr@menayer.com>
