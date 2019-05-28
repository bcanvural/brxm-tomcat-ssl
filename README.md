# Configure Tomcat for SSL

## Generate self-signed certificate to run things locally
(All passwords are "secret")

```bash
keytool -genkey -keysize 2048 -keyalg RSA -alias tomcat -keystore keystore.jks

keytool -importkeystore -srckeystore keystore.jks -destkeystore intermediate.p12 -deststoretype PKCS12

openssl pkcs12 -in intermediate.p12 -nocerts -out  key.pem
openssl pkcs12 -in intermediate.p12 -clcerts -out cert.pem
openssl pkcs12 -in intermediate.p12 -cacerts -out chain.pem
```

* Move generated key,cert and chain pem files to conf/ directory. Modify src/main/docker/assembly/conf-component-docker.xml so that these files are moved inside the docker image
(See said file in this project)

* Update tomcat server.xml connector config (See conf/server.xml)

* Replace keyfile password with the one provided from the environment. (See run command below and also the docker-entrypoint.sh)

# Hst Host config (CMS)

Set "hst:scheme: https" on hst:root node under /hst:platform/hst:hosts/dev-localhost/localhost


#### Run it locally:
```bash
docker run -p 8443:8443 -e profile="h2" -e KEYFILE_PASSWORD="secret" org.example/myproject:0.1.0-SNAPSHOT
```

* Visit https://localhost:8443/cms 
