# Gerar Chave Privada da Intermediária
openssl genrsa -out intermediate/private/intermediaria.key 2048

# Gerar a CSR da Intermediária
openssl req -new \
    -sha256 \
    -nodes \
    -key intermediate/private/intermediaria.key \
    -out intermediate/intermediaria.csr \
    -config openssl_intermediaria.cnf

# Assinar a CSR da Intermediária usando a Raiz (rodei dentro da raiz)
openssl ca \
    -config openssl_raiz.cnf \
    -extensions v3_intermediaria \
    -days 730 \
    -notext \
    -md sha256 \
    -in ../intermediario/intermediaria.csr \
    -out ../intermediario/certs/intermediaria.crt \
    -batch

# Geração da CRL da AC Intermediária
openssl ca \
    -config openssl_inter.cnf \
    -crldays 30 \
    -md sha256 \
    -keyfile private/intermediaria.key \
    -cert certs/intermediaria.crt \
    -gencrl \
    -out crl/intermediaria.crl

# Resumo dos Artefatos Criados para a AC Intermediária
#     private/intermediaria.key: Chave privada da InterCA.
#     intermediaria.csr: Requisição de assinatura (não é mais necessária, mas está aí).
#     certs/intermediaria.crt: Certificado assinado pela AC Raiz (contém AIA e CDP).
#     crl/intermediaria.crl: CRL da InterCA.
#     openssl_inter.cnf: Arquivo de configuração.