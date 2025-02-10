# Web: Files & Folders

## 📂 FFuF for Directory & File Discovery

### **📌 Command**

```bash
ffuf -u http://example.com/FUZZ -w <WORDLIST> -ac
```

### **📝 Explanation**

This FFuF command brute-forces directories and files on a target **by injecting FUZZ** into the URL path.

| Option | Description                                   |
| ------ | --------------------------------------------- |
| `-u`   | Target URL with `FUZZ` as the injection point |
| `-w`   | **Wordlist** containing directory/file names  |
| `-ac`  | **Auto-calibration**, filters false positives |
