---
title: 'unRAID Server: Install PostgreSQL'
date: 2025-08-23 14:00:00 +0700
draft: false 
categories: ["Homelab"]
tags: ["unRAID", "Self-Hosted", "Database", "PostgreSQL"]
pin: false
slug: "unraid-install-postgresql"
cover: 'https://cdn.anh.moe/f/sVh5rnX4.png-webp'
---

PostgreSQL is the database management platform on my unRAID Server. It not only ensures stability for personal projects like NPM, Vaultwarden, Umami, etc., but also serves as the backbone for all my data activities.

## Why PostgreSQL? 
Honestly, I don’t fully understand SQL (like PostgreSQL or MariaDB). However, many self-hosted apps require these databases, even though some can run on SQLite. Initially, SQLite seemed convenient since each app creates its own DB file. But eventually, I realized each app having its own SQLite DB was messy. So I consolidated everything into one place for convenience.

And the reason I picked PostgreSQL is simple — because Immich (my Google Photos alternative) only supports PostgreSQL 😅.

## Installing PostgreSQL on unRAID
In this article, I’ll walk you through installing PostgreSQL16 using the available template in unRAID Community Applications (CA).

1. Log in to the unRAID Dashboard
2. Go to the Apps tab
3. Search for `PostgreSQL16`
4. Click Install → Ok
5. Adjust Container Configuration:
    1. Variable: POSTGRES_USER: ""
    2. Variable: POSTGRES_DB: ""

    I left these blank so the app can auto-create a default user/database.

    Later, when apps require specific databases, I’ll create them manually.
    ![docker_postgresql](https://cdn.anh.moe/f/MyXA1wf.jpg-webp)
6. Click Apply → The system will automatically pull data and deploy the app
![docker_postgresql_successfully](https://cdn.anh.moe/f/cQsq8eCr.jpg-webp)

## Login and Create Database
First, access the Docker container’s `console` for PostgreSQL:

1. Open unRAID Dashboard  
2. Go to Docker tab  
3. Right-click on postgresql16 container  
4. Select Console  
5. Log into the database:
```bash
# psql -d postgres -U postgres -W
Password: 
psql (16.9 (Debian 16.9-1.pgdg120+1))
Type "help" for help.

postgres=#
```
6. Create a database:
```bash
postgres=# create database test;
CREATE DATABASE
postgres=# create user testid with encrypted password 'testpd';
CREATE ROLE
postgres=# grant connect on database test to testid;
GRANT
postgres=# alter database test owner to testid;
ALTER DATABASE
```

![postgresql_command](https://cdn.anh.moe/f/19e5Og.jpg-webp)

## Conclusion
Installing PostgreSQL on unRAID is fairly easy thanks to the available template. Just input some basic settings and you’ll have a stable PostgreSQL Docker container up and running.

From this base, I’ve created databases for apps like Vaultwarden (password manager), Nginx Proxy Manager (reverse proxy), Umami Analytics (analytics for this blog), etc. All databases are centralized instead of scattered SQLite files inside individual `/appdata/app_services` folders.

I’ll publish detailed guides for each app later. See you in the next post.

Happy building!