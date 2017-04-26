# SaltStack Beacons

These are a collection of SaltStack Beacons that are for use in an environment managed with SaltStack. Each has it's own purpose that should be apparent from it's name, and each requires its own set of configuration data in order to work properly. The configuration data is generally made available to the beacon through Pillar data, but may also be provided from the minion's config file `/etc/salt/minion`.

## Example
As an example, lets use the `salt_minion_heartbeat` beacon. This beacon requires the following data to be available in the minion's Pillar or in the minion's config file:

```
beacons:
  salt_minion_heartbeat:
    enabled: True
    interval: 60
    tag_append: ''
```

This configuration data tells the SaltStack minion that a file called `salt_minion_heartbeat.py` contains a beacon that is enabled and should be executed every 60 seconds. SaltStack implicitly tries to call a function called `beacon` in the file after it executes the `validate` function also in the file.

The `tag_append` value in the configuration data is also made available to the beacon, but as it is empty it doesn't really have an effect.

All beacons should reside in a folder on the master, and if using the default file roots, it will be `/srv/salt/_beacons`.
In order for the minion to receive the beacons, we must execute `salt-call saltutil.sync_beacons && service salt-minion restart`. The minion will then download all the files in the `_beacons` directory and restart. Assuming the beacon config data is available, the minion should start producing beacon events. 
