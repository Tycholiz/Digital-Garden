
Cron assumes that the machine is running continuously.
- Therefore, it can't reliably be used on machines that aren't running 24 hours a day.
- If this is an issue, then try [anacron](https://linux.die.net/man/8/anacron) (linux only), which removes this limitation
    - Might be able to get a similar effect with [[launchd|mac.OS.launchd]] on OS X

See all active Crontabs:
`crontab -e`
