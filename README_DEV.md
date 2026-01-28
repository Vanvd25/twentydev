# 🚀 Twenty CRM - Local Development Setup

This guide provides step-by-step instructions to set up the **Twenty CRM** development environment on your local machine (Linux, MacOS, or Windows WSL).

## 📋 Prerequisites

### 🐧 Linux & 🍎 MacOS

* **Git**: [Install Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* **Node.js**: Version **v24.5.0** [Download](https://nodejs.org/en/download)
* **Yarn**: Version **v4** [Getting Started](https://yarnpkg.com/getting-started/install)
* **NVM**: [Node Version Manager](https://github.com/nvm-sh/nvm)

### 🪟 Windows (WSL)

Open PowerShell as Administrator and run:

```powershell
wsl --install

```

Then, inside your WSL terminal, install and configure the necessary tools:

```bash
# Install Git
sudo apt-get update && sudo apt-get install git -y

# Configure Git
git config --global user.name "Your Name"
git config --global user.email "youremail@domain.com"

# Install NVM, Node.js and Yarn
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash
# Restart your terminal, then run:
nvm install 24.5.0
nvm use 24.5.0
corepack enable # Enables Yarn 4

```

## 🛠 Installation Steps

### Step 1: Clone the Repository

```bash
git clone https://github.com/twentyhq/twenty.git
cd twenty
# If you need version v1.15.0 specifically:
git checkout v1.15.0

```

### Step 2: Set up Databases (Docker)

Twenty requires PostgreSQL and Redis. Use the provided `make` commands to spin them up quickly:

```bash
# Start PostgreSQL
make -C packages/twenty-docker postgres-on-docker

# Start Redis (Cache)
make -C packages/twenty-docker redis-on-docker

```

### Step 3: Setup Environment Variables

```bash
cp ./packages/twenty-front/.env.example ./packages/twenty-front/.env
cp ./packages/twenty-server/.env.example ./packages/twenty-server/.env

```

### Step 4: Install Dependencies

```bash
yarn

```

### Step 5: Initialize & Run the Project

Reset and prepare your database:

```bash
npx nx database:reset twenty-server

```

Start all services (Frontend, Server, and Worker) at once:

```bash
npx nx start

```

---

## 💾 Step 6: Restore Backup Data (Optional)

If you have a `backup.sql` file and wish to restore it into your local Docker container, use the following command:

```bash
cat backup.sql | docker exec -i twenty-docker-postgres-1 psql -U twenty

```

*(Note: Use `docker ps` to verify the exact name of your postgres container if the command above fails).*

---

## 🌐 Accessing Twenty

| Service | URL | Default Credentials |
| --- | --- | --- |
| **Frontend** | `http://localhost:3001` | User: `tim@apple.dev` / Pass: `tim@apple.dev` |
| **Backend API** | `http://localhost:3000` | - |
| **GraphQL API** | `http://localhost:3000/graphql` | - |
| **REST API** | `http://localhost:3000/rest` | - |

---

## 💡 Useful Commands

* **Run Server only:** `npx nx start twenty-server`
* **Run Frontend only:** `npx nx start twenty-front`
* **Run Worker only:** `npx nx worker twenty-server`

---

