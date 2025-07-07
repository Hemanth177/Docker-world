# 📄 Docker Compose: Explained with Example

---

## 🚀 What is Docker Compose?

Docker Compose is a **tool for defining and running multi-container Docker applications**.

Instead of manually running `docker run …` commands for each container, Compose lets you:

✅ Define all services (containers), their networks, and volumes in **one YAML file**.
✅ Start the entire application stack with a single command:

```bash
docker-compose up
```

---

## 🔷 Why use Docker Compose?

* Manage **multiple containers** easily.
* Define networks and volumes declaratively.
* Portable configuration (`docker-compose.yml`).
* Great for local development & testing.

---

## 📄 Components of a Compose file

A typical `docker-compose.yml` includes:

| Section      | Purpose                                       |
| ------------ | --------------------------------------------- |
| **version**  | Compose file syntax version.                  |
| **services** | Define containers (name, image, ports, etc.). |
| **volumes**  | Named volumes for persistent data.            |
| **networks** | Custom networks to connect services.          |

---

## 📝 Example: 2-Tier App

We’ll build a **2-tier application**:

* 🖥 Web server → **nginx**
* 🗄 Database → **MySQL**
* Connected via a custom network.

---

### 📄 `docker-compose.yml`

```yaml
version: '3.8'

services:
  db:
    image: mysql:8
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: testdb
    ports:
      - "3306:3306"
    volumes:
      - db_data:/var/lib/mysql
    networks:
      - app-network

  web:
    image: nginx:latest
    ports:
      - "8080:80"
    depends_on:
      - db
    networks:
      - app-network

volumes:
  db_data:

networks:
  app-network:
    driver: bridge
```

---

## 🔗 What happens here?

### 📦 **Services**

#### `db`

* Runs MySQL 8.
* Sets root password and creates `testdb` database.
* Mounts a volume `db_data` for persisting data.
* Exposes MySQL on host port 3306.

#### `web`

* Runs nginx.
* Maps host port 8080 → container port 80.
* Depends on `db` service.

---

### 📡 **Networks**

We define a custom bridge network `app-network` so that:

* `web` and `db` can communicate via their service names (`db`, `web`).
* Isolated from other containers outside this project.

---

### 📂 **Volumes**

`db_data` is a named volume:

* Persists MySQL data on your host.
* So data is not lost when the container restarts.

---

## 🔷 Commands

### Start the stack

```bash
docker-compose up -d
```

### Stop & remove everything

```bash
docker-compose down
```

### View logs

```bash
docker-compose logs -f
```

### Scale services (if applicable)

```bash
docker-compose up --scale web=3
```

---

## 🌟 Benefits for DevOps & Developers

✅ Declarative configuration.
✅ Simplifies multi-container workflows.
✅ Easy to version control.
✅ Great for CI/CD pipelines.
✅ Supports environment overrides.

