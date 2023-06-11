# Home Assistant Backup

* Home Assistant creates a backup every night, which is stored in the docker volume
* A systemd timer creates a daily backup of the that file
* An S3 bucket is set up to keep the backup files. It is set without versioning, so that we keep only the last generation for a day of the week or week of the year. Yearly backups will be kept "forever".
* There are seven daily backup files uploaed to an S3 bucket:
  - home-assistant-backup.Monday.tar.gz
  - home-assistant-backup.Tuesday.tar.gz
  - ...
  - home-assistant-backup.Sunday.tar.gz
* Likewise, there is a backup file for each week of the year, copied on Sunday:
  - home-assistant-backup.week-01.tar.gz
  - ...
  - home-assistant-backup.week-53.tar.gz

  Same for each year; the file of Dec 31st of each year is kept as:
  - home-assistant-backup.2022.tar.gz
  - home-assistant-backup.2023.tar.gz
  - ...

# Restore

As per doc, unarchiving the tarball and placing it in `/config/backup` should be enough for the UI to find it. When I tried this, the UI did not offer any means to restore. Instead, I

* unpacked the contained `homeassistant.tar.gz` and
* moved the contents of the `data` directory to where `/config/backup` is mounted.

This worked flawlessly.
