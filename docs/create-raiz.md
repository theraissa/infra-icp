

#Chave privada para a Autoridade Certificadora Raiz.
openssl genrsa -out private/raiz.key 2048

#Certificado Autoassinado da AC Raiz
openssl req -x509 \
    -new \
    -sha256 \
    -nodes \
    -key private/raiz.key \
    -days 3650 \
    -out certs/raiz.crt \
    -config openssl_raiz.cnf \
    -extensions v3_ca_raiz

# Lista de Revogação de Certificados (CRL) da Raiz
openssl ca \
    -config openssl_raiz.cnf \
    -crldays 30 \
    -md sha256 \
    -keyfile private/raiz.key \
    -cert certs/raiz.crt \
    -gencrl \
    -out crl/raiz.crl