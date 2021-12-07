source: https://cloud.google.com/sql/docs/postgres/connect-admin-proxy


## Connect using the Cloud SQL Auth proxy

Use the Cloud SQL Auth proxy with private IP
To connect to a Cloud SQL instance using private IP, the Cloud SQL Auth proxy must be on a resource with access to the same VPC network as the instance.

The Cloud SQL Auth proxy uses IP to establish a connection with your Cloud SQL instance. By default, the Cloud SQL Auth proxy attempts to connect using a public IPv4 address. If your Cloud SQL instance has only private IP, the Cloud SQL Auth proxy uses the private IP address to connect.

If the instance has both public and private IP configured, and you want the Cloud SQL Auth proxy to use the private IP address, you must provide the following option when you start the Cloud SQL Auth proxy:

> The Cloud SQL Auth proxy uses 3307 to connect to the Cloud SQL Auth proxy server. Port xxxx is the default port for the PostgreSQL protocol over a direct TCP connection (not using the Cloud SQL Auth proxy).

### 使用public ip to connect cloudSQL (在GCE上執行)。
- GCE 需能連到Internet。
- GCE service account 需要至少關聯以下其中一個 Role
    - Cloud SQL Client (roles/cloudsql.client)
    - Cloud SQL Editor (roles/cloudsql.editor)
    - Cloud SQL Admin (roles/cloudsql.admin)
```bash
./cloud_sql_proxy -instances=INSTANCE_CONNECTION_NAME=tcp:0.0.0.0:1234
```

### 使用private ip to connect cloudSQL (在GCE上執行)。
- 執行地點需要在，與 CloudSQL 相同的 VPC 內。
- GCE service account 需要至少關聯以下其中一個 Role
    - Cloud SQL Client (roles/cloudsql.client)
    - Cloud SQL Editor (roles/cloudsql.editor)
    - Cloud SQL Admin (roles/cloudsql.admin)
```bash
./cloud_sql_proxy -instances=INSTANCE_CONNECTION_NAME=tcp:0.0.0.0:1234 -ip_address_types=PRIVATE
```

### 使用public ip to connect cloudSQL (在 localhost 筆記本上執行)，並透過 service account key 連線。
- localhost 需能連到Internet。
- service account 需要至少關聯以下其中一個 Role
    - Cloud SQL Client (roles/cloudsql.client)
    - Cloud SQL Editor (roles/cloudsql.editor)
    - Cloud SQL Admin (roles/cloudsql.admin)
- 如果您有在地端出站防火牆政策，請確保它允許連接到目標 Cloud SQL 實例上的端口 3307。
```bash
./cloud_sql_proxy -instances=INSTANCE_CONNECTION_NAME=tcp:0.0.0.0:1234 -credential_file=PATH_TO_KEY_FILE
```

### 若想透過localhost SQL client(ex: DBeaver, pgAdmin...)連到，只有private ip 的 Cloud SQL。
- 在與 CloudSQL 相同的 VPC 內的 GCE 上執行以下command。
```bash
./cloud_sql_proxy -instances=INSTANCE_CONNECTION_NAME=tcp:0.0.0.0:1234 -ip_address_types=PRIVATE
```
- Client 再透過 ssh Tunnel 連線
 
#### Example for mysql
- 設定ssh Tunnel（連線到啟用 Cloud SQL Auth Proxy 的GCE）。
![截圖 2021-12-07 下午1 55 12](https://user-images.githubusercontent.com/46012524/144974375-9eda2050-9f06-426a-91fb-93270e7cd776.png)

- 其他設定像是連線地端DB.
![截圖 2021-12-07 下午1 54 38](https://user-images.githubusercontent.com/46012524/144974322-4a875b6b-c623-49b0-afb8-f9505b911d8e.png)
