The guide for contributors (or curious developers) who want to run Twenty locally.

Prerequisites
Before you can install and use Twenty, make sure you install the following on your computer:


Linux and MacOS :

Git https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
Node v24.5.0 https://nodejs.org/en/download
yarn v4 https://yarnpkg.com/getting-started/install
nvm https://github.com/nvm-sh/nvm/blob/master/README.md



Windows (WSL)

Install WSL Open PowerShell as Administrator and run:

wsl --install

Install and configure git
sudo apt-get install git

git config --global user.name "Your Name"

git config --global user.email "youremail@domain.com"

Install nvm, node.js and yarn
sudo apt-get install curl

curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash

Close and reopen your terminal to use nvm. Then run the following commands.
nvm install # installs recommended node version

nvm use # use recommended node version

corepack enable

Step 1: Git Clone
git clone https://github.com/twentyhq/twenty.git

Step 2: Position yourself at the root
cd twenty


Step 3: Set up a PostgreSQL Database

make -C packages/twenty-docker postgres-on-docker

Step 4: Set up a Redis Database (cache)
make -C packages/twenty-docker redis-on-docker


Step 5: Setup environment variables

cp ./packages/twenty-front/.env.example ./packages/twenty-front/.env
cp ./packages/twenty-server/.env.example ./packages/twenty-server/.env

Step 6: Installing dependencies
yarn


Step 7: Running the project
Set up your database with the following command:
npx nx database:reset twenty-server

Start the server, the worker and the frontend services:

npx nx start twenty-server
npx nx worker twenty-server
npx nx start twenty-front

Alternatively, you can start all services at once:
npx nx start

Step 8: Use Twenty

Frontend

Twenty’s frontend will be running at http://localhost:3001. You can log in using the default demo account: tim@apple.dev (password: tim@apple.dev)

Backend
Twenty’s server will be up and running at http://localhost:3000
The GraphQL API can be accessed at http://localhost:3000/graphql
The REST API can be reached at http://localhost:3000/rest

To restore backup data, run
cat backup.sql | docker exec -i {db_container_name_or_id} psql -U {postgres_user}
