---
layout:
  width: wide
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
---

# Troubleshooting

## NTP sync with DC / KRB\_AP\_ERR\_SKEW(Clock skew too great) <a href="#clock-skew-too-great" id="clock-skew-too-great"></a>

```bash
sudo timedatectl set-local-rtc 0
sudo timedatectl set-ntp 1
sudo ntpdate -u IP -b
```

Explanation

This set of commands is used to **synchronize the system time** with an **NTP (Network Time Protocol) server** and ensure the system clock is managed correctly.

| Command                            | Description                                                                                                           |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `sudo timedatectl set-local-rtc 0` | Configures the system to **use UTC** instead of local time, ensuring time consistency across applications.            |
| `sudo timedatectl set-ntp 1`       | Enables **NTP (Network Time Protocol)** to allow automatic time synchronization.                                      |
| `sudo ntpdate -u <IP> -b`          | Manually synchronizes the system time with an **NTP server at `<IP>`**, forcing an update with `-b` (immediate sync). |
