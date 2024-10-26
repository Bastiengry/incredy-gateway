### SSL certificates
- Generate a certificate (do better in production for password):
keytool -genkeypair -alias incredy-gateway -keyalg RSA -keysize 4096 -storetype PKCS12 -keystore incredy-gateway-keystore.p12 -validity 3600 -keypass password -storepass password

- Export certificate in PEM
openssl pkcs12 -in incredy-gateway-keystore.p12 -out incredy-gateway.crt.pem -clcerts -nokeys -nodes -passin 'pass:password'

- Export private key in PEM
openssl pkcs12 -in incredy-gateway-keystore.p12 -out incredy-gateway.key.pem -nocerts -nodes -passin 'pass:password'

- Import gateway certificate in truststore (if necessary)
keytool -import -alias incredy-gateway -storetype PKCS12 -keystore curl-incredy-truststore.p12 -file incredy-gateway.crt.pem -keypass password -storepass password

- Call with curl
curl -v -k -i -E ./incredy-gateway.crt.pem --key ./incredy-gateway.key.pem https://172.20.0.1:8443/incredy/api/v1/topic