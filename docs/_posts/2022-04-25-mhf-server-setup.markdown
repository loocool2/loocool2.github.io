---
layout: post
title:  "Setting Up a Monster Hunter Frontier Z Server on Linux"
date:   2022-04-25
categories: documentation
introduction: "Documenting how I set up a MHFZ server on Linux"
read_time: true
---

# Intro

Unfortunately, I was never able to play Monster Hunter Frontier Z since I'm from the West, but now that private server files exist, it's possible to experience what I missed.

I noticed that there aren't any How-Tos on how to set up MHFZ on Linux. The install files from the [/mhg/ pastebin](https://pastebin.com/QqAwZSTC) assume you're using Windows 10, but I didn't want to waste so many resources virtualizing a entire Windows install just for a game server that'd be sitting idle most of the time. After some tinkering and research, I've figured out how to get it up and running. Hopefully this guide will help anyone else that wanted to run the server on a Linux machine.

# Some things to note

## Software requirements

While any distro should work just fine, I set up two Ubuntu 21.10 containers on my Proxmox server, one for running the server, and one for running the postgresql database. I named them `mhpostgres` and `mhfrontier` respectively, and I'll be calling them that throughout this guide.

While this isn't strictly necessary, more separation is always better in case one part gets screwed up, so I can simply nuke it and restore a backup. This guide will be separated into two parts because of this.

Make sure to put the server files somewhere where the containers can access them so you can copy it over. I put them on my NAS and mounted it to the containers. 

## Hardware requirements

So far, I've noticed that neither the database or the game server do not require much performance to run. I'm not sure if this is because it's just me though.

# Part 1: Setting up the Postgres database

## Steps:

You can probably skip steps 2 and 3 if you're using Ubuntu, as Ubuntu includes postgres by default.
1. Update and install all your local packages with `sudo apt update && sudo apt upgrade`.
2. Install the postgresql server with `sudo apt install postgresql`.
3. Make it start at every boot with `sudo systemctl start postgresql.service`.

### Setting up access control

4. Go to `/etc/postgresql/<version>/main/postgresql.conf` and use your preferred text editor to edit it. As of writing the included version in Ubuntu is 12, but you can check with `postgres -V`.
5. Edit the commented out line that says `#listen_addresses = ‘localhost’` to say  `listen_addresses = '*'`.
This is so that connections from anyone will be answered rather than connections only on the local machine.

> If you'd like more security, you can change the listen_addresses whitelist to only the computers you're going to access it from. In other words, `mhfrontier` and whatever desktop you use to administer i.e. `listen_addresses = '192.168.0.2, 192.168.0.4'.

6. Run `sudo -u postgres` to switch to the postgres user.
7. Run `createdb erupe` to create the database.
8. Run `psql` to access the postgres shell.
9. To allow the `mhfrontier` server files to access the database, you need to change the password of the default user, postgres. Run `ALTER USER postgres with encrypted password 'your_password';` with the password you want. This is case sensitive.

### Restoring the database

10. Copy the `Erupe-Backup.sql` file to inside the container using your favorite method of file transfer e.g. cp or scp.
11. Now restore the database with the command `pg_restore -d erupe Erupe-Backup.sql`. Make sure you execute it from the same place the file is located or with the file path location.

11a. If you would like to, you can restore “Road Shop Items.csv” as well. I couldn't figure out how through CLI, so I did it through pgAdmin4.

# Part 2: Setting up the MHFrontier Z Server

## Steps:
1. Copy the the Erupe folder and Quests.7z into your container. 
1a. Move Quests.7z inside Erupe.
2. Update and install all your local packages with `sudo apt update && sudo apt upgrade` again.
3. Install Go by running `sudo apt install golang-go`.
4. Open config.json using your favorite text editor.
5. Change all the 127.0.0.1s to the IP of the container you’re using. i.e. 127.0.0.1 -> 192.168.0.2 if the container you're hosting on has that IP.
6. Change the `localhost` under `database` to the IP of the container you used for the database. i.e. localhost -> 192.168.0.1
6a Change POSTGRES-PASSWORD-HERE to the password you set in step 9 of the database setup.
7. Install 7zip by running `sudo apt install p7zip-full`. You’ll be needing this to extract the quests zip.
8. Switch directories to the Erupe folder using cd.
9. Run `7z x Quests.7z`. This is going to take a while since there’s like 200k files in this folder.
> Make sure not to use `7z e` as that will dump all the files into one folder.

10. Finally, run `go run main.go` from within the Erupe folder and hopefully everything's been set up properly and the server will run.

# Miscellaneous Tips:
* You can use a custom DNS rewrite rule instead of changing your hosts file if you run your own DNS through PiHole, Adguard Home, or something else.