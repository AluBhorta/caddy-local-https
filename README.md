# caddy local https

use caddy's automatic tls to access sites locally with https.

## prerequisites

- docker
- docker compose

## quickstart

- start the containers:

  ```sh
  docker compose up -d
  ```

- copy caddy's internal cert from container to local machine:

  ```sh
  docker compose cp caddy:/data/caddy/pki/authorities/local/root.crt ./caddy_root.crt
  ```

- <details>
      <summary>
        install caddy's cert (<code>caddy_root.crt</code>) on your local machine
      </summary>

  **Mac:**

  a. Double-click the `caddy_root.crt` file.

  b. The Keychain Access application will open. Select the "System" keychain and click "Add".

  c. Authenticate with your password when prompted.

  d. Find the certificate in the "System" keychain, double-click on it, expand the "Trust" section, and set "Secure Sockets Layer (SSL)" to "Always Trust".

  e. Close the certificate window and authenticate with your password again when prompted.

  **Windows:**

  a. Right-click the `caddy_root.crt` file and select "Install Certificate".

  b. Select "Local Machine" and click "Next".

  c. Choose "Place all certificates in the following store" and click "Browse".

  d. Select "Trusted Root Certification Authorities" and click "OK".

  e. Click "Next" and then click "Finish" to complete the installation.

  **Linux:**

  The process may vary depending on the distribution. Here's an example for Ubuntu:

  a. Copy the `caddy_root.crt` to the trusted certificate directory:

  ```
  sudo cp caddy_root.crt /usr/local/share/ca-certificates/caddy_root.crt
  ```

  b. Update the certificate trust store:

  ```
  sudo update-ca-certificates
  ```

  After installing the root CA certificate, you should restart your browser and any other applications that use TLS certificates to ensure they recognize the new certificate.

  </details>

- update your hosts file (`/etc/hosts`) 
  
  add the custom domain names that you want to resolve to local address `127.0.0.1`. in this case, we're adding 2 new lines (for `whoami.myapp.local` & `nginx.myapp.local`):
  ```conf
  127.0.0.1       whoami.myapp.local
  127.0.0.1       nginx.myapp.local
  ```

## clean up

- tear down the containers:

  ```sh
  docker compose down
  ```

  adding `-v` will also remove the volumes, in this case, caddy's data folder which contains the generated certs.

- remove the values previously added to your hosts file (`/etc/hosts`)
- uninstall/remove caddy's cert from your local machine
