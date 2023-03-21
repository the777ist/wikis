# MISC

### Disable CORS on Chrome:
```bash
$ google-chrome-stable --disable-web-security --user-data-dir="<directory>"
```

### CA cert validation failed while npm install (CAfile: none CRLfile: none):
```bash
$ git config --global http.sslverify false
```