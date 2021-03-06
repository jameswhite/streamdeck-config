# streamdeck-config
  - From the Stream Deck App, choose `Configure Stream Deck`

![Configure Stream Deck](img/README.d/Configure_Stream_Deck.png)

  - Click the Settings Gear to open the Preferences Dialog

![Settings Gear](img/README.d/Settings_Gear.png)

  - Click the Pull-Down inverted carat

![Pull-Down Arrow](img/README.d/Pull-Down_Arrow.png)

  - Navigate to Create Backup

![Create Backup](img/README.d/Create_Backup.png)

  - Place the backup file in the `backups/` directory in this repository, it will be ignored by git, so save the backups elsewhere if you want to use it for a backup
  - The files are `Zip archive data, at least v2.0 to extract` so unzip it into the config directory

```
unzip -o ../backups/Stream\ Deck\ -\ 12-02-2021\ -\ 12-07.streamDeckProfilesBackup
```

  - commit the files to the repository, it's unclear if a wipe should first be used. I'll need to test it.
  - Repeat this process for subsequent backups, checking `git status` to see if the files have changed, commit any changes.


  - Create a backup files in the `restore` directory

```
  (cd config/ ; zip -r -X -Z bzip2 "../restore/Stream Deck - $(date +"%d-%m-%Y") - $(date +"%H-%M").streamDeckProfilesBackup" *)
```

  - Use the Stream Deck Application's `Restore from Backup` to overwrite your Profiles on all Devices.



