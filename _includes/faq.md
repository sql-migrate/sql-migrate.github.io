---
title: FAQ
---

### PostgresCompare is reporting that I have reached my maximum installs, what do I do?

Drop us an [email](mailto://neil@postgrescompare.com), we will reset your license key so you are good to go again.

### PostgresCompare is not responding or just showing a spinner, how do I fix it?

Sometimes (after the app updates for example) the PostgresCompare background process does not restart succesfully which can lead to the UI spinning and failing to load projects. 

Fix the issue by finding the process "PostgresCompareWebApi" and killing it, then restart the PostgresCompare app. If the problem persists try restarting your machine.

### PostgresCompare recently updated and now my projects are not showing up, how do I fix it?

PostgresCompare relies on the operating system to tell it where to store your data. Sometimes this can change leading to a new database file being created.

To recover your data you should:

1. Search your machine for the file "postgrescompare_7.db". There should be 2 instances of a file with that name. Note which file is the older one, it probably also has a larger file size.
2. Copy the old postgrescompare_7.db into the new location, overwriting the db file there.
3. Restart PostgresCompare