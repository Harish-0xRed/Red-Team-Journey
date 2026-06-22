# Bandit Level 0 Notes

## Objective

Learn how to connect to a remote Linux server using SSH.

## Website

OverTheWire Bandit

## Login Information

Host:
bandit.labs.overthewire.org

Port:
2220

Username:
bandit0

Password:
bandit0

## Command Used

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

## Command Breakdown

# ssh

Secure Shell (remote login protocol)

# bandit0

Username

# @

Connect as this user

# bandit.labs.overthewire.org

Remote server hostname

# -p 2220

Use port 2220 instead of default port 22

## What Happens?

1. SSH client contacts the remote server.
2. Server asks for password.
3. Enter password:
   bandit0
4. Successful authentication.
5. User receives a Linux shell on the remote machine.

## Skills Learned

* What SSH is
* Remote Linux access
* Username and password authentication
* Connecting to a non-default port
* Reading terminal login banners

## Real-World Relevance

SOC Analysts, System Administrators, Cloud Engineers, and Red Teamers frequently use SSH to manage Linux servers remotely.

## Level Completion

Level 0 → Successfully connected to the Bandit server.

Next Goal:
Bandit Level 0 → Level 1
Find the password stored in the file named:
readme
