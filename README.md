# Automated Daily Backup System using Bash and Cron

A Bash script that automatically backs up files modified in the last 24 hours into a timestamped `.tar.gz` archive, scheduled to run every day with cron.

Built as the final project for IBM's **Hands-on Introduction to Linux Commands and Shell Scripting** course.

## Scenario

At ABC International Inc., interns had to manually access encrypted password files on core servers every day and back up the ones updated in the last 24 hours. This was slow, error-prone, and weak on security. `backup.sh` automates the whole process.

## Features

- Takes a target directory and a destination directory as command line arguments
- Validates the arguments (exactly two, and both must be valid directories)
- Finds files modified within the last 24 hours using timestamp comparison
- Compresses them into `backup-<timestamp>.tar.gz`
- Moves the archive to the destination directory
- Runs automatically every 24 hours via a cron job

## Usage

```bash
chmod +x backup.sh
./backup.sh <target_directory> <destination_directory>
```

Example:

```bash
./backup.sh important-documents .
```

This creates a file like `backup-1760367480.tar.gz` in the current directory.

## How it works

1. Reads the two arguments into `targetDirectory` and `destinationDirectory`
2. Gets the current timestamp in seconds with `date +%s`
3. Builds the archive name `backup-$currentTS.tar.gz`
4. Saves the original directory and the absolute path of the destination
5. Moves into the target directory
6. Calculates the timestamp from 24 hours ago (`currentTS - 24 * 60 * 60`)
7. Loops over every file with `*`, and if `date -r $file +%s` is newer than that timestamp, adds the file to an array
8. Archives all collected files with `tar -czvf`
9. Moves the archive to the destination with `mv`

## Scheduling with cron

Copy the script to a system location:

```bash
sudo cp backup.sh /usr/local/bin/
```

Open the crontab and add this line to run it every day at midnight:

```bash
crontab -e
```

```
0 0 * * * /usr/local/bin/backup.sh /home/project/important-documents /home/project
```

Start the cron service if it is not already running:

```bash
sudo service cron start
```

## Skills practiced

- Bash scripting: variables, command substitution, arrays, loops, conditionals
- File timestamps and arithmetic
- `tar` compression
- File permissions with `chmod`
- Cron scheduling and syntax

## Author

Vasanth
