# PHP usado na Intranet

Imagens de contêiner do PHP + Apache usadas na Intranet do Clube Naval. Cada diretório contém o `Dockerfile` de uma versão/combinação de extensões.

## Imagens disponíveis

| Diretório | PHP | Oracle (oci8) | SQL Server | Imagem pública |
|---|---|:---:|:---:|---|
| [`8.5.12-sqlserver`](8.5.12-sqlserver) | 8.5.12 | ❌ | ✅ (ODBC 18) | `ghcr.io/clubenaval/php:8.5.12-sqlserver` |
| [`8.3.2-oci8-sqlserver`](8.3.2-oci8-sqlserver) | 8.3.2 | ✅ | ✅ (ODBC 17) | — |
| [`8.2.8-oci8`](8.2.8-oci8) | 8.2.8 | ✅ | ❌ | — |
| [`7.4.33-oci8-sqlserver`](7.4.33-oci8-sqlserver) | 7.4.33 | ✅ | ✅ (ODBC 17/18) | — |
| [`7.4.33-oci8`](7.4.33-oci8) | 7.4.33 | ✅ | ❌ | — |

Apenas a `8.5.12-sqlserver` é publicada no registro; as demais devem ser construídas localmente.

## Usando a imagem pública

```bash
docker pull ghcr.io/clubenaval/php:8.5.12-sqlserver
docker run -d -p 8080:80 -v "$PWD":/var/www/html ghcr.io/clubenaval/php:8.5.12-sqlserver
```

Exemplo com `docker-compose.yml`:

```yaml
services:
  app:
    image: ghcr.io/clubenaval/php:8.5.12-sqlserver
    ports:
      - "8080:80"
    volumes:
      - ./src:/var/www/html
```

Ou, como base de outra imagem:

```dockerfile
FROM ghcr.io/clubenaval/php:8.5.12-sqlserver
COPY . /var/www/html
```

## Construindo localmente

```bash
docker build -t php:8.5.12-sqlserver ./8.5.12-sqlserver
```

## Detalhes da imagem `8.5.12-sqlserver`

- **Base:** `php:8.5.12-apache` (Debian 13).
- **Bancos de dados:** `sqlsrv` e `pdo_sqlsrv` (Microsoft ODBC Driver 18 for SQL Server), `mysqli` e `pdo_mysql`. Sem Oracle.
- **Extensões adicionais:** `bcmath`, `bz2`, `calendar`, `dba`, `exif`, `ftp`, `gd`, `gettext`, `gmp`, `intl`, `ldap`, `pcntl`, `shmop`, `snmp`, `soap`, `sockets`, `sysvmsg`, `sysvsem`, `sysvshm`, `tidy`, `xsl` e `zip` (mais as que já acompanham a imagem oficial, como `mbstring`, `curl`, `dom`, `opcache`).
- **Apache:** módulos `rewrite` e `remoteip` habilitados; logs em `stdout`/`stderr`.
- **Configuração do PHP (`php.ini`):**

  | Diretiva | Valor |
  |---|---|
  | `date.timezone` | `America/Sao_Paulo` |
  | `max_execution_time` | `300` |
  | `memory_limit` | `2048M` |
  | `upload_max_filesize` | `2048M` |
  | `post_max_size` | `200M` |

- **Fuso do sistema:** `America/Sao_Paulo`.

### Atenção: criptografia no ODBC Driver 18

O driver 18 usa `Encrypt=yes` por padrão. Se o seu SQL Server usar certificado autoassinado, acrescente `TrustServerCertificate=yes` à string de conexão:

```php
$pdo = new PDO('sqlsrv:Server=host;Database=db;TrustServerCertificate=yes', $user, $pass);
```

### Licenças

Esta imagem instala o Microsoft ODBC Driver for SQL Server aceitando o [contrato de licença da Microsoft (veja a página de download do driver)](https://learn.microsoft.com/sql/connect/odbc/download-odbc-driver-for-sql-server) (`ACCEPT_EULA=Y`). Ao usá-la, você concorda com esses termos.
