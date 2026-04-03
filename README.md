
## OpenWebUI and LiteLLM

A very Local Hosted approach to the home lab and AI

Local Postresql Install and Set up.
```
sudo apt update
sudo apt install -y postgresql-common ca-certificates
sudo /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh
sudo apt install -y postgresql-18 postgresql-18-pgvector

sudo ufw allow from 192.168.0.0/24 to any port 5432 proto tcp comment 'nat access'
sudo ufw allow from 172.16.0.0/12 to any port 5432 proto tcp comment 'docker access'
```

Update Config
/etc/postgresql/18/main/postgresql.conf
```
listen_addresses = '*'
port = 5432
password_encryption = scram-sha-256
```

Update Access
/etc/postgresql/18/main/pg_hba.conf
```
local   all             postgres                                peer

# localhost
host    all             all             127.0.0.1/32            scram-sha-256
host    all             all             ::1/128                 scram-sha-256

# docker
host    all             all             172.16.0.0/12           scram-sha-256

# LAN
host    all             all             192.168.0.0/24         scram-sha-256
```

Complete LiteLLM and RAG db set up.
```
sudo systemctl restart postgresql

sudo -iu postgres
psql
CREATE ROLE litellm WITH LOGIN PASSWORD 'changeMe';
ALTER USER litellm WITH LOGIN PASSWORD 'changeMe';
CREATE DATABASE litellm OWNER litellm;

CREATE ROLE pgvector WITH LOGIN PASSWORD 'changeMe';
ALTER USER pgvector WITH LOGIN PASSWORD 'changeMe';
CREATE DATABASE pgvector OWNER pgvector;
\q

psql -d litellm -c "CREATE EXTENSION IF NOT EXISTS vector;"
psql -d pgvector -c "CREATE EXTENSION IF NOT EXISTS vector;"
```

Validating LLMs are up and running.
```
curl http://localhost:4000/v1/models -H "Content-Type: application/json"   -H "Authorization: Bearer ${LITELLM_API_KEY}"

curl -s ${OLLAMA_SRC1}/api/tags

curl -s $(OLLAMA_SRC2)/api/tags
```
