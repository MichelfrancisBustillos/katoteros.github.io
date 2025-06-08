---
title: NUT Client Setup
categories: [Guides]
tags: [setup, NUT, UPS]
---

## NUT Client Setup

1. Install the NUT client package:

   ```bash
   sudo apt install nut-client -y
   ```

2. Configure the NUT client by editing the configuration file:

   ```bash
   sudo micro /etc/nut/nut.conf
   ```

   Update the file with the following content:

   ```bash
   MODE=netclient
   ```

   Save and exit the editor.

3. Next, edit the `upsmon.conf` file to set up the monitoring configuration:

   ```bash
   sudo micro /etc/nut/upsmon.conf
   ```

   Update the file with the following content:

   ```bash
    RUN_AS_USER root

    MONITOR <ups_name>@<nut_server_address> 1 <username> <password> slave

    NOTIFYCMD /etc/nut/notify

    MINSUPPLIES 1
    SHUTDOWNCMD "/sbin/shutdown -h"
    NOTIFYCMD /usr/sbin/upssched
    POLLFREQ 2
    POLLFREQALERT 1
    HOSTSYNC 15
    DEADTIME 15
    POWERDOWNFLAG /etc/killpower

    NOTIFYMSG ONLINE    "UPS %s on line power"
    NOTIFYMSG ONBATT    "UPS %s on battery"
    NOTIFYMSG LOWBATT   "UPS %s battery is low"
    NOTIFYMSG FSD       "UPS %s: forced shutdown in progress"
    NOTIFYMSG COMMOK    "Communications with UPS %s established"
    NOTIFYMSG COMMBAD   "Communications with UPS %s lost"
    NOTIFYMSG SHUTDOWN  "Auto logout and shutdown proceeding"
    NOTIFYMSG REPLBATT  "UPS %s battery needs to be replaced"
    NOTIFYMSG NOCOMM    "UPS %s is unavailable"
    NOTIFYMSG NOPARENT  "upsmon parent process died - shutdown impossible"

    NOTIFYFLAG ONLINE   SYSLOG+WALL+EXEC
    NOTIFYFLAG ONBATT   SYSLOG+WALL+EXEC
    NOTIFYFLAG LOWBATT  SYSLOG+WALL
    NOTIFYFLAG FSD      SYSLOG+WALL+EXEC
    NOTIFYFLAG COMMOK   SYSLOG+WALL+EXEC
    NOTIFYFLAG COMMBAD  SYSLOG+WALL+EXEC
    NOTIFYFLAG SHUTDOWN SYSLOG+WALL+EXEC
    NOTIFYFLAG REPLBATT SYSLOG+WALL
    NOTIFYFLAG NOCOMM   SYSLOG+WALL+EXEC
    NOTIFYFLAG NOPARENT SYSLOG+WALL

    RBWARNTIME 43200

    NOCOMMWARNTIME 600

    FINALDELAY 5
   ```

   Save and exit the editor.

4. Edit the `upssched.conf` file to configure the scheduling of UPS commands:

   ```bash
   sudo micro /etc/nut/upssched.conf
   ```

   Update the file with the following content:

   ```bash
    CMDSCRIPT /etc/nut/upssched-cmd
    PIPEFN /etc/nut/upssched.pipe
    LOCKFN /etc/nut/upssched.lock

    AT ONBATT * START-TIMER onbatt 600
    AT ONLINE * CANCEL-TIMER onbatt online
    AT LOWBATT * EXECUTE onbatt
    AT COMMBAD * EXECUTE commbad_message
    AT COMMOK * EXECUTE commok_message
    AT NOCOMM * EXECUTE nocomm_message
    AT SHUTDOWN * EXECUTE shutdown_message
    AT SHUTDOWN * EXECUTE powerdown
   ```

   Save and exit the editor.

5. Create the command script file for `upssched`:

   ```bash
   sudo micro /etc/nut/upssched-cmd
   ```

   Add the following content to the file:

   ```bash
    #!/bin/sh

    case $1 in
        onbatt)
            logger -t upssched-cmd "UPS on Battery Power"
            /usr/sbin/upsmon -c fsd
            ;;
            commbad_message)
            echo "UPS no longer connected" | mailx -s "Proxmox: UPS Warning Message" mxjrc75wg6@pomail.net
            ;;
            online)
            logger -t upssched-cmd "UPS back Online"
            ;;
            commok_message)
            echo "UPS is Reconnected" | mailx -s "Proxmox: UPS Warning Message" mxjrc75wg6@pomail.net
            ;;
            nocomm_message)
            echo "UPS no longer connected" | mailx -s "Proxmox: UPS Warning Message" mxjrc75wg6@pomail.net
            ;;
            shutdowncritical)
            logger -t upssched-cmd "UPS on battery critical, forced shutdown"
            ;;
            upsgone)
            logger -t upssched-cmd "UPS has been gone too long, can't reach"
            ;;
            shutdown_message)
            echo "Proxmox is shutting down" | mailx -s "Promox: UPS Warning Message" mxjrc75wg6@pomail.net
            ;;
        *)
            logger -t upssched-cmd "Unrecognized command: $1"
            ;;
    esac
   ```

   Save and exit the editor.

6. Make the script executable:

   ```bash
   sudo chmod +x /etc/nut/upssched-cmd
   ```
7. Restart the NUT service to apply the changes:

   ```bash
   sudo systemctl restart nut-client
   ```
8. Enable the NUT service to start on boot:

   ```bash
   sudo systemctl enable nut-client
   ```
9. Verify the NUT client status:

   ```bash
   sudo systemctl status nut-client
   ```