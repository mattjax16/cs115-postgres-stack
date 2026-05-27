# CS115 PostgreSQL + pgAdmin Setup (Docker)

This repository provides a Docker-based alternative to installing PostgreSQL and pgAdmin directly on your machine. Instead of a native install, everything runs inside containers, so setup is fast, clean, and works the same on any OS. Be sure to complete **all steps below** to get up and running.

---

## Prerequisites: Install Docker

Before anything else, you need Docker Desktop installed on your computer.

1. Go to [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/) and download the installer for your operating system (Windows, macOS, or Linux).
2. Run the installer and follow the default steps.
3. Once installed, open Docker Desktop and let it finish starting up. You should see a green "Engine running" indicator in the bottom left.

> **Note:** On Windows, Docker Desktop may ask you to enable WSL 2 (Windows Subsystem for Linux). Follow the on-screen prompts to do so, as it's required for Docker to work.

---

## Step 1: Configure Your Credentials (Optional)

By default the containers will start with the credentials defined in `docker-compose.yml`. If you would like to use your own, copy the provided example file to create a `.env` file:

```
cp .env.example .env
```

Open `.env` and fill in your own values. **Write down your credentials somewhere safe.** You will need them to log in to pgAdmin and to connect to the database.

The `.env` file is excluded from version control and will never be committed to the repository.

---

## Step 2: Start PostgreSQL

1. Download or clone this repository to your computer.

2. Open a terminal and `cd` into the repository folder:
   - **Windows:** Open PowerShell or Command Prompt.
   - **macOS/Linux:** Open the Terminal application.

3. Run the following command to start both PostgreSQL and pgAdmin:
   ```
   docker compose up -d
   ```
   The first time you run this, Docker will download the necessary images. This may take a minute or two. Subsequent starts will be much faster.

4. To verify PostgreSQL is running, run the command below, replacing `yourusername` and `yourdbname` with the values you set in Step 1:
   ```
   docker exec -it postgres psql -U yourusername -d yourdbname
   ```
   You should see a prompt that looks like this:
   ```
   yourdbname=#
   ```
   If so, that's it! You've successfully set up Postgres. Continue to Step 3 below.

   Type `\q` and hit Enter to exit the psql shell.

---

## Step 3: Open pgAdmin

pgAdmin is the graphical interface you will use to interact with your PostgreSQL database. Because it's running inside Docker, you access it through your web browser rather than a desktop app.

1. Open any web browser and go to:
   ```
   http://localhost
   ```

2. You will be prompted to set a master password for pgAdmin. Choose something you will remember and **write it down** for future reference.

3. Log in with the email and password you set in your `.env` file.

4. Once pgAdmin opens, in the left-hand navigation bar you should see **Servers**. Click the arrow next to it to open the tree of servers. It may prompt you to enter the password for the PostgreSQL server you set up in Step 2.

   After opening the tree, you should see **Servers (1)** with your PostgreSQL instance listed beneath it.

   If you don't see a server listed, you can manually add the connection by doing the following:
   1. Right-click on **Servers** and select **Register > Server**.
   2. In the **General** tab, give the connection a name (suggested: `dbserver`).
   3. In the **Connection** tab, enter the following:
      - **Host name/address:** `postgres` *(this is the Docker service name, not `localhost`)*
      - **Username:** the value you set for `POSTGRES_USER` in your `.env` file
      - **Password:** the value you set for `POSTGRES_PASSWORD` in your `.env` file
   4. Click **Save**.

---

## Stopping and Starting the Containers

When you're done working, you can stop the containers without losing any data:
```
docker compose down
```

To start them again next time:
```
docker compose up -d
```

Your database and any data you've saved will still be there.
