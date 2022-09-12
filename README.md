

<div align="center">
  <img src="https://fiverr-res.cloudinary.com/images/t_main1,q_auto,f_auto,q_auto,f_auto/gigs/267479684/original/83f0b98372561b234cb8892340da027eed98e73c/develop-oracle-databases-apex-applications-plsql.png" width="40%" height="40%">  
</div>

# Adicionar Certificados SSL no Oracle APEX 

## Passos

### 1. Acrescentar em SQLNET.ORA:

```bash
WALLET_LOCATION=(SOURCE=(METHOD=file)(METHOD_DATA=(DIRECTORY=$ORACLE_HOME/wallet)))
```

### 2.  Comandos para criar a wallet e adicionar os certificados da url https que o APEX deve alcançar (adicionar a cadeia de certificados). Abaixo, um exemplo:

```bash
orapki wallet create -wallet $ORACLE_HOME/wallet -pwd <My_Password> -auto_login

orapki wallet add -wallet $ORACLE_HOME/wallet -cert /tmp/cert-Digicert.cer -trusted_cert -pwd <My_Password>

orapki wallet add -wallet $ORACLE_HOME/wallet -cert /tmp/cert-ThateRSACA2018.cer -trusted_cert -pwd <My_Password>

orapki wallet display -wallet $ORACLE_HOME/wallet -pwd <My_Password>
```

### 3. Testar a requisição para URL com o script abaixo:

```bash
DECLARE
  req   UTL_HTTP.req;
  resp  UTL_HTTP.resp;
BEGIN
  UTL_HTTP.SET_WALLET('file:<caminho_da_sua_wallet>', <My_Password>;
  req := UTL_HTTP.begin_request('https://<endereco_https_que_quero_alcançar>');
  resp := UTL_HTTP.get_response(req);
  UTL_HTTP.end_response(resp);
END;
/

