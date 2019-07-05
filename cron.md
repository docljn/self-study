## Understanding Cron

arguments in a cron job

cron day/date/time/etc
user
command

* * * * * root check_if_master_for_account 2852 && /usr/local/sbin/run_script_with_locking -n 'Andigestion Upload' -d '/home/andigestion' -r '/usr/bin/php bin/do_upload.php' -e 'upload-digest@podfather.com' -t 120 2>&1 | mail -Es "Andigestion Upload Integration Cron Error" "upload-digest@podfather.com"



PARSED:
Run the following command every minute
As root user

https://github.com/beveradb/scripts/blob/master/run_script_with_locking

First:
`symfather/batch/check_if_master_for_account.php` with the argument `2852`
- i.e. is the server able to run commands in this context

Then:
`/usr/local/sbin/run_script_with_locking -n 'Andigestion Upload' -d '/home/andigestion' -r '/usr/bin/php bin/do_upload.php' -e 'upload-digest@podfather.com' -t 120`
- i.e.
  - make sure that only one instance of this script is run at any one time, suing a lock directory
  - `-n` using the human-friendly name `Andigestion Upload`
  - `-d` from the directory (absolute path) `/home/andigestion`
  - `-r` run `/usr/bin/php bin/do_upload.php`
  - `-e` if there is an error, email the address `upload-digest@podfather.com` if there are any critical errors
  - `-t` allow a maximum of two hours lock time when running

Then:
`2>&1`
- redirect `stderr` (file descriptor 2) to `stdout` (file descriptor 1)

Then
`| mail -Es "Andigestion Upload Integration Cron Error" "upload-digest@podfather.com"`
- pipe the output of the redirect to the unix 'mail' utility
- `-E` skip sending if the body of the mail is empty i.e. if there are no errors, don't send
- `s` set the email subject to be `'Andigestion Upload Integration Cron Error'`
- address the email to `upload-digest@podfather.com`

CONFUSED:
- why do I have -e and pipe to mail?
