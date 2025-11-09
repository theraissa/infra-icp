# Gerar Chave Privada do Servidor
openssl genrsa -out private/web.local.key 2048

# Gerar a CSR do Servidor
openssl req -new \
    -sha256 \
    -nodes \
    -key private/web.local.key \
    -out csr/web.local.csr \
    -config openssl_servidor.cnf

# Assinatura pela AC Intermediária (executar no diretorio do intermediario)
openssl ca \
    -config openssl_inter.cnf \
    -extensions v3_servidor \
    -days 375 \
    -notext \
    -md sha256 \
    -in ../servidor/csr/web.local.csr \
    -out ../servidor/crt/web.local.crt \
    -batch

# Criar o Arquivo da Cadeia (Chain)
cat crt/web.local.crt ../intermediario/certs/intermediaria.crt > web.local.chain.crt