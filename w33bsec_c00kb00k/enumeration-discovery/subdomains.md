# Subdomains

## FFuF for Subdomain Discovery

```bash
ffuf -u http://example.com/ -w <WORDLIST> -H "Host: FUZZ.example.com" -ac
```

#### **📝 Explanation**

This FFuF command brute-forces **subdomains** by injecting words into the `Host` header.

| Option | Description                                                  |
| ------ | ------------------------------------------------------------ |
| `-u`   | Target URL (`http://example.com/`)                           |
| `-w`   | **Wordlist** for subdomains                                  |
| `-H`   | Injects **FUZZ** into the `Host` header (`FUZZ.example.com`) |
| `-ac`  | **Auto-calibration**, filters out false positives            |
