# Cron

>  Cron is useful for automatically running commands.

Basic commands:

To list / edit existing / new tasks:

```bash
crontab -l # list the existing tasks
crontab -e # edit the existing tasks
```

Task format:

```
minute hour day-of-month month day-of-week command
```

e.g.

```bash
0 0 * * * cd ~
```

Will change to the home directory on the 0th minute of the 0th hour every day.

**Example: automatically backing up a folder every day**

Create a cron task to automatically add, commit, and push a repo to github once per day.

1. Start by creating a shell script that cron will run. Create a file called myCronScript.sh with the following:

   ```sh
   #!/bin/bash
   currentTime=$(date +%Y%m%d%I%M%S)
   cd /Users/Peter/Documents/notes
   git add .
   git commit -m "notes auto backup $currentTime"
   git push
   ```

   This script will get the current date, change directories to the notes directory, add everything to git, commit it with the current time and push to git. You can test it by running `sh myCronScript.sh`.

2. Next, create the cron task to run it every day. Open up crontab with:

   ```bash
   >> crontab -e
   ```

   and add the following line:

   ```bash
   0 0 * * * cd /Users/Peter/Documents/chron && sh myCronScript.sh
   ```

3. You can check that it worked with the following:

   ```bash
   >> crontab -l
   0 0 * * * cd /Users/Peter/Documents/chron && sh backupScpt.sh
   ```

4. The notes folder will now be automatically backed up to github every day!