download data file with:
```
wget
"http://hdpdn703.hio.athenahealth.com:50075/webhdfs/v1/log/common_apache/access/20150907/hdpnn711.hio.athenahealth.com_15.log.gz?op=OPEN&namenoderpcaddress=hdphio&offset=0"
-O /var/tmp/test.json.gz
gunzip /var/tmp/test.json.gz
```
