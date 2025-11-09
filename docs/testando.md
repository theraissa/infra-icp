
# Prova de Conceito
docker exec -it client sh -c "curl --cacert /usr/local/share/ca-certificates/raiz.crt https://web.local"

# Testar o Download do Certificado Intermediário (AIA)
docker exec -it client sh -c "curl http://pki.local/crt/intermediaria.crt"

# Testar o Download da CRL (CDP)
 docker exec -it client sh -c "curl http://pki.local/crl/intermediaria.crl"

# Teste Detalhado da Conexão TLS
docker exec -it client sh -c "curl -v --cacert /usr/local/share/ca-certificates/raiz.crt https://web.local"