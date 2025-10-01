Nice — let’s get you a compact, **all-purpose SkySQL guide** you can follow in a real project: from launching a service to connecting (CLI / Python / JDBC / ODBC), plus the major SQL commands and practical tips (security, pooling, troubleshooting). I’ll keep it actionable — copy/paste-ready examples and a short checklist.

> **Sources:** official SkySQL docs (Quickstart, Connect guides, Encryption) and MariaDB connector docs. ([SkySQL Documentation][1])

---

# 1 — Quick checklist (high level)

1. Create SkySQL account & login. ([SkySQL Documentation][1])
2. Launch a service (choose MariaDB or MySQL-compatible service type). ([SkySQL Documentation][1])
3. Note **Service Details**: FQDN (host), port (3306 or 3307), username, generated password, database name. ([SkySQL Documentation][2])
4. **Allowlist** your app/server IP(s) in the SkySQL console so clients can reach the DB. ([SkySQL][3])
5. Download the service SSL certificate / .pem (if provided) and use TLS for client connections. SkySQL encrypts client connections by default (TLS 1.2/1.3). ([SkySQL Documentation][4])
6. Test connection with the `mariadb` client, then integrate into your app (Python/JDBC/ODBC). ([SkySQL Documentation][2])

---

# 2 — Connect from the command line (quick example)

Replace placeholders with the values from your SkySQL **Service Details** page.

```bash
# interactive prompt for password (recommended)
mariadb --host your-service-name.sysp0000.db1.skysql.com \
       --port 3306 \
       --user your_user_name -p \
       --ssl-verify-server-cert
```

Notes:

* Use port **3306** for read-write, **3307** is often the read-only replica port. ([SkySQL Documentation][2])
* If the console gave you a .pem CA cert, add `--ssl-ca=/path/to/cert.pem` if needed. ([SkySQL Documentation][2])

---

# 3 — Connect from a Python app (example)

Use **MariaDB Connector/Python** (recommended by MariaDB and SkySQL).

```python
import mariadb
import os

conn = mariadb.connect(
    user=os.getenv("DB_USER"),
    password=os.getenv("DB_PASS"),
    host=os.getenv("DB_HOST"),                # FQDN from SkySQL
    port=int(os.getenv("DB_PORT", 3306)),
    database=os.getenv("DB_NAME"),
    ssl_verify_cert=True,                     # enforce TLS cert verification
    ssl_ca="/path/to/skysql-service-cert.pem" # if SkySQL supplies a CA file
)

cur = conn.cursor()
cur.execute("SELECT NOW()")
print(cur.fetchone())
cur.close()
conn.close()
```

Tips:

* Put credentials in **environment variables** or a secrets manager, not source code.
* Use a connection pool (see `mariadb` pooling docs) for web apps; avoid opening/closing per request. ([SkySQL Documentation][5])

---

# 4 — Other client options

* **JDBC (Java)**: use the MariaDB JDBC driver and the host/port/user/password from the service page. (SkySQL is MySQL-compatible.) ([MariaDB][6])
* **ODBC**: configure MariaDB Connector/ODBC and create a DSN; use SkySQL host/port + TLS cert settings. ([SkySQL Documentation][7])

---

# 5 — Major SQL commands you’ll use in every project

Use these from any client (mariadb/mysql, Python, JDBC):

Schema & data:

```sql
-- create database and use it
CREATE DATABASE myapp;
USE myapp;

-- create table
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  username VARCHAR(100) NOT NULL UNIQUE,
  email VARCHAR(255),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- insert
INSERT INTO users (username, email) VALUES ('alice','a@example.com');

-- select
SELECT id, username, email FROM users WHERE username = 'alice';

-- update
UPDATE users SET email = 'alice@new.com' WHERE id = 1;

-- delete
DELETE FROM users WHERE id = 1;
```

Transactions:

```sql
START TRANSACTION;
-- multiple DML statements...
COMMIT;       -- or ROLLBACK;
```

Indexing & safety:

```sql
ALTER TABLE users ADD INDEX idx_username (username);
```

User & privileges (admin tasks — be careful):

```sql
CREATE USER 'appuser'@'%' IDENTIFIED BY 'strong_password';
GRANT SELECT, INSERT, UPDATE, DELETE ON myapp.* TO 'appuser'@'%';
FLUSH PRIVILEGES;
```

---

# 6 — Project-level best practices

* **TLS by default**: SkySQL enforces/encourages TLS. Verify certs and avoid disabling TLS unless you have VPC-peering / private connectivity. ([SkySQL Documentation][4])
* **Allowlist IPs**: Add only the IPs you need (app servers, CI runners). ([SkySQL][3])
* **Secrets management**: store DB credentials in environment variables or vaults (AWS Secrets Manager / HashiCorp Vault).
* **Connection pooling**: use pooling libraries in your language/framework (e.g., SQLAlchemy pool for Python, HikariCP for Java).
* **Backups & monitoring**: follow SkySQL console options for automated backups and alerts (check the Quickstart/Service details). ([SkySQL Documentation][1])
* **Read replicas**: route read-heavy traffic to read-only ports/endpoints (SkySQL exposes replica/read ports). ([SkySQL Documentation][2])

---

# 7 — Troubleshooting common errors

* **Can’t connect** → check allowlist, correct FQDN, port, and that you used the generated username/password. Test from the app server directly with `mariadb` client. ([SkySQL Documentation][2])
* **SSL/TLS errors** → ensure `ssl-verify-server-cert` or `ssl_ca` is set if the server requires cert verification; use the SkySQL-provided CA if available. ([SkySQL Documentation][4])
* **Auth failed** → ensure you’re using the correct user (service details often show the default user) and that the user has privileges on the DB.
* **Slow queries** → enable slow query log via SkySQL console (or use EXPLAIN and add indexes).

---

# 8 — Quick example: simple app flow

1. Launch SkySQL service → copy host/port/credentials. ([SkySQL Documentation][1])
2. Allowlist app IP. ([SkySQL][3])
3. Download CA cert (if provided) and store it on app server. ([SkySQL Documentation][4])
4. Use a pooled DB client in your app (example above) pointing to host/port. ([SkySQL Documentation][5])
5. Migrate schema (run `CREATE TABLE` scripts) and start serving traffic.

---

# 9 — Useful docs to bookmark (official)

* SkySQL Quickstart / Service lifecycle. ([SkySQL Documentation][1])
* Connect using MariaDB CLI (command examples & port notes). ([SkySQL Documentation][2])
* Connect from Python App (Connector/Python docs & examples). ([SkySQL Documentation][5])
* SkySQL Encryption & TLS details. ([SkySQL Documentation][4])
* ODBC/JDBC connectivity notes. ([SkySQL Documentation][7])

---

If you want, I can now:

* produce a ready-to-run **README** for your project that includes exact `mariadb` CLI commands, a Python module example (with pooling), and a `.env.example` file; **or**
* generate the exact **migration SQL** (tables/indices) for your app schema if you paste a short spec (models/fields).

Tell me which one you want and I’ll generate it right away (no waiting).

[1]: https://docs.skysql.com/Quickstart/?utm_source=chatgpt.com "Quickstart - SkySQL Documentation"
[2]: https://docs.skysql.com/Connecting%20to%20SkySQL%20DBs/Connect%20using%20MariaDB%20CLI/?utm_source=chatgpt.com "Connect using MariaDB CLI - SkySQL Documentation"
[3]: https://skysql.com/skysql?utm_source=chatgpt.com "AI-Driven Serverless MySQL & MariaDB Cloud Database - SkySQL"
[4]: https://docs.skysql.com/Security/Encryption/?utm_source=chatgpt.com "Encryption - SkySQL Documentation"
[5]: https://docs.skysql.com/Connecting%20to%20SkySQL%20DBs/Connect%20from%20Python%20App/?utm_source=chatgpt.com "Connect from Python App - SkySQL Documentation"
[6]: https://mariadb.com/docs/skysql-previous-release/quickstart/launch/?utm_source=chatgpt.com "Quickstart Launch Walkthrough — MariaDB Documentation"
[7]: https://docs.skysql.com/Connecting%20to%20SkySQL%20DBs/Connect%20using%20ODBC/?utm_source=chatgpt.com "Connect using ODBC - SkySQL Documentation"
