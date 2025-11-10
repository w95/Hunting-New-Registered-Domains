# Hunting-Newly-Registered-Domains
Hunting Newly Registered Domains

The `hnrd.py` is a Python utility for finding and analysing potential phishing domains used in phishing campaigns targeting your customers. This utility is written in Python 3.10+ and is based on the analysis of the features below by consuming a free daily list provided by the [Whoisds](https://whoisds.com/newly-registered-domains) site.

## Features

* Download a free list from [Whoisds](https://whoisds.com/newly-registered-domains)
* Bitsquatting, hyphenation & domain permutation is used for the transformation of the given keyword
* Generated words are searched/matched against the list
* Retrieve DNS Record(s) Information
* Retrieve IP2ASN Information
* Retrieve WHOIS Information
* [Retrieve Reverse WHOIS (by Name) Information](https://domainbigdata.com)
* [Retrieve Certificates](https://crt.sh)
* Retrieve VirusTotal Information
* Check domains against [QUAD9](https://quad9.net) service 
* Calculate Shannon Entropy Information
* Calculate Levenshtein Ratio Distance

## External Services Used

This tool queries the following external services to gather intelligence about domains:

| # | Service | Type | Status | Requires Auth | Purpose |
|---|---------|------|--------|---------------|---------|
| 1 | [WhoisDS](https://whoisds.com) | HTTP | ✅ Working | ❌ No | Downloads newly registered domain lists |
| 2 | DNS Resolvers | DNS | ✅ Working | ❌ No | Resolves A, MX, NS, AAAA, SOA records |
| 3 | WHOIS Servers | WHOIS | ⚠️ Rate Limited | ❌ No | Gets domain registration details |
| 4 | IPWhois/RDAP | RDAP/WHOIS | ✅ Working | ❌ No | Converts IP addresses to ASN, CIDR, country |
| 5 | [DomainBigData](http://domainbigdata.com) | Web Scraping | ⚠️ May Block | ❌ No | Finds other domains by same registrant |
| 6 | [crt.sh](https://crt.sh) | API | ⚠️ Sometimes Slow | ❌ No | Finds SSL/TLS certificates and subdomains |
| 7 | [VirusTotal](https://www.virustotal.com) | API | ⚠️ Requires Key | ✅ **Yes (API Key)** | Gets malware detections, URLs, samples, PDNS |
| 8 | [QUAD9](https://quad9.net) | DNS | ✅ Working | ❌ No | Checks if domain is blocked by security filters |

**Note:** VirusTotal requires a valid API key. Get one at [virustotal.com](https://www.virustotal.com/) and update line 334 in `hnrd.py`.

## Requirements

**Python Version:** 3.10+

### Dependencies

Install dependencies using:
```bash
pip3.10 install -r requirements.txt
```

**Updated Dependencies (2025):**
* beautifulsoup4==4.14.2
* bs4==0.0.2
* certifi==2025.10.5
* chardet==5.2.0
* colorama==0.4.6
* dnspython==2.8.0
* future==1.0.0
* html5lib==1.1
* idna==3.11
* ipaddr==2.2.0
* ipwhois==1.3.0
* python-Levenshtein==0.27.3
* python-whois==0.9.6
* requests==2.32.5
* requests-file==3.0.1
* six==1.17.0
* termcolor==3.2.0
* tldextract==5.3.0
* urllib3==2.5.0
* webencodings==0.5.1

## Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/Hunting-New-Registered-Domains.git
cd Hunting-New-Registered-Domains
```

2. Install dependencies:
```bash
pip3.10 install -r requirements.txt
```

3. (Optional) Add your VirusTotal API key in `hnrd.py` line 334

## Usage

```
usage: hnrd.py [-h] [-d DFILE] [-f DATE] [-t DATE_END]
               (-s SEARCH | -S SEARCH_FILE | -r REGEX) [-v]

hunting newly registered domains

options:
  -h, --help      show this help message and exit
  -d DFILE        File containing new domain names
  -f DATE         date [format: year-month-date]
  -t DATE_END     Ending date (get domain names since date to ending date)
                  [format: year-month-date or "yesterday"]
  -s SEARCH       Search a keyword
  -S SEARCH_FILE  File to read list of keywords from (One word per line)
  -r REGEX        Regex to be matched
  -v              show program's version number and exit
```

## Examples

### Example 1: Search for a specific keyword on a specific date

```bash
python3.10 hnrd.py -f 2025-11-04 -s paypal
```

### Example 2: Search with an existing domain file

```bash
python3.10 hnrd.py -d domains.txt -s microsoft
```

### Example 3: Search multiple keywords from a file

```bash
# Create keywords.txt with one keyword per line
echo -e "paypal\nmicrosoft\napple\namazon" > keywords.txt
python3.10 hnrd.py -f 2025-11-04 -S keywords.txt
```

### Example 4: Use regex pattern matching

```bash
python3.10 hnrd.py -f 2025-11-04 -r "^(paypal|bank|secure)"
```

### Example 5: Download and search date range

```bash
# Search from specific date to yesterday
python3.10 hnrd.py -f 2025-11-01 -t yesterday -s google
```

## Sample Output

```bash
python3.10 hnrd.py -f 2025-11-04 -s paypal
```

```
[*]-Retrieving DNS Record(s) Information
  \_ paypal-required-action.com
    \_ A 162.219.251.133
    \_ SOA ns19.hosterbox.com
    \_ NS ns19.hosterbox.com,ns20.hosterbox.com
    \_ MX paypal-required-action.com
  \_ paypal-resolvedbillingstatement.com
    \_ A 74.220.199.6
[*]-Retrieving IP2ASN Information
  \_ 162.219.251.133
    \_ asn_registry arin
    \_ asn_country_code US
    \_ asn_date 2013-08-21
    \_ asn_cidr 162.219.251.0/24
    \_ asn 33494
    \_ asn_description IHNET - IHNetworks, LLC, US
  \_ 74.220.199.6
    \_ asn_registry arin
    \_ asn_country_code US
    \_ asn_date 2007-01-09
    \_ asn_cidr 74.220.192.0/19
    \_ asn 46606
    \_ asn_description UNIFIEDLAYER-AS-1 - Unified Layer, US
[*]-Retrieving WHOIS Information
  \_ paypal-required-action.com
    \_ Created Date 2018-03-29 19:11:32
    \_ Updated Date [datetime.datetime(2018, 3, 29, 19, 11, 32), datetime.datetime(2018, 3, 29, 19, 11, 41)]
    \_ Expiration Date [datetime.datetime(2019, 3, 29, 19, 11, 32), datetime.datetime(2019, 3, 29, 20, 11, 32)]
    \_ DateDiff 3
    \_ Name mario pichardo
    \_ Email domain-abuse@psi-usa.info,pichardomario44@gmail.com
    \_ Registrar PSI-USA, Inc. dba Domain Robot
  \_ paypal-resolvedbillingstatement.com
    \_ Created Date 2018-03-29 23:45:13
    \_ Updated Date 2018-03-29 23:45:14
    \_ Expiration Date 2019-03-29 23:45:13
    \_ DateDiff 2
    \_ Name DOMAIN PRIVACY SERVICE FBO REGISTRANT
    \_ Email abuse@bluehost.com,WHOIS@BLUEHOST.COM
    \_ Registrar FastDomain Inc.
[*]-Retrieving Reverse WHOIS (by Name) Information [Source https://domainbigdata.com]
  \_ mario pichardo
    \_ 3 domain(s) have been created in the past
  \_ DOMAIN PRIVACY SERVICE FBO REGISTRANT
    \_ 200 domain(s) have been created in the past
[*]-Retrieving Certficates [Source https://crt.sh]
  \_ paypal-resolvedbillingstatement.com
    \_ No CERT found
  \_ paypal-required-action.com
    \_ not_after 2018-06-28T23:59:59
    \_ min_entry_timestamp 2018-03-30T07:07:18.128
    \_ min_cert_id 370495406
    \_ issuer_ca_id 12922
    \_ name_value mail.paypal-required-action.com
    \_ issuer_name C=US, ST=TX, L=Houston, O="cPanel, Inc.", CN="cPanel, Inc. Certification Authority"
    \_ not_before 2018-03-30T00:00:00
    \_ not_after 2018-06-28T23:59:59
    \_ min_entry_timestamp 2018-03-30T07:07:18.128
    \_ min_cert_id 370495406
    \_ issuer_ca_id 12922
    \_ name_value www.paypal-required-action.com
    \_ issuer_name C=US, ST=TX, L=Houston, O="cPanel, Inc.", CN="cPanel, Inc. Certification Authority"
    \_ not_before 2018-03-30T00:00:00
[*]-Retrieving VirusTotal Information
  \_ paypal-required-action.com
    \_ Detected URLs
      \_ http://paypal-required-action.com/signin/?country.x=&amp;locale.x=en_EN 10 / 68 2018-03-30 13:04:22
      \_ http://paypal-required-action.com/signin/?country.x=&locale.x=it_IT 10 / 67 2018-03-30 12:39:00
      \_ http://paypal-required-action.com/signin/?country.x=&amp;locale.x=it_IT 10 / 67 2018-03-30 12:37:59
      \_ http://paypal-required-action.com/signin 9 / 67 2018-03-30 12:22:42
      \_ http://paypal-required-action.com/signin/ 8 / 68 2018-03-30 10:30:01
      \_ https://paypal-required-action.com/ 1 / 67 2018-03-30 08:03:02
    \_ Detected Download Samples
      \_ 2018-03-30 13:12:15 2 / 59 84d698d294b28a3ea1413c162e23f28e42a7a6c49669004e67dcf01867b5e7f4
      \_ 2018-03-30 12:46:13 2 / 59 91b9a986026cc24bd46a3a9c868606b47164554f87c8f03e2f9725bfc29b52fb
      \_ 2018-03-30 12:45:19 2 / 59 3aab8ffed0e0aec6f2551170c72f8fb4bb4a82891efdb16df14b25fd96dee52e
    \_ categories
      \_ dynamic content
    \_ Subdomains
      \_ www.paypal-required-action.com
      \_ mail.paypal-required-action.com
    \_ Resolutions (PDNS)
      \_ 2018-03-30 00:00:00 162.219.251.133
  \_ paypal-resolvedbillingstatement.com
    \_ Domain not found
[*]-Check domains against QUAD9 service
  \_ paypal-required-action.com
    \_ Blocked
  \_ paypal-resolvedbillingstatement.com
    \_ Not Blocked
[*]-Calculate Shannon Entropy Information
  \_ paypal-required-action.com 3.97909789113
  \_ paypal-resolvedbillingstatement.com 4.05757515968
[*]-Calculate Levenshtein Ratio
  \_ paypal-required-action vs paypal 0.428571428571
  \_ paypal-resolvedbillingstatement vs paypal 0.324324324324
```

## Understanding the Output

### DNS Records
Shows A, MX, NS, AAAA, and SOA records for discovered domains.

### IP2ASN Information
Provides Autonomous System Number (ASN), CIDR range, country code, and organization for resolved IPs.

### WHOIS Information
- **Creation Date**: When the domain was registered
- **DateDiff**: Days since domain creation (useful for finding very new domains)
- **Registrant**: Domain owner information
- **Registrar**: Company that registered the domain

### QUAD9 Check
- **Blocked**: Domain is flagged as malicious by QUAD9 DNS security service
- **Not Blocked**: Domain is not currently flagged

### Shannon Entropy
Measures randomness in domain names:
- **< 3.5**: Low entropy (normal-looking domain)
- **3.5 - 4.0**: Medium entropy (potentially suspicious)
- **> 4.0**: High entropy (likely random/generated domain)

### Levenshtein Ratio
Measures similarity between searched keyword and found domain:
- **> 0.8**: Very similar (high phishing risk)
- **0.4 - 0.8**: Moderately similar
- **< 0.4**: Low similarity

## Domain Transformation Techniques

The tool uses several techniques to find potential phishing domains:

1. **Bitsquatting**: Flips single bits in characters (e.g., `paypal` → `payqal`)
2. **Hyphenation**: Adds hyphens between characters (e.g., `paypal` → `pay-pal`, `p-aypal`)
3. **Subdomain**: Adds dots to create subdomains (e.g., `paypal` → `pay.pal`)

## Troubleshooting

### VirusTotal API Errors
If you see "Invalid API response", get a free API key from [virustotal.com](https://www.virustotal.com/) and update line 334 in `hnrd.py`.

### Rate Limiting
Some services (WHOIS, DomainBigData) may rate-limit requests. This is normal for large domain lists. The tool continues processing other checks.

### Empty Results
If you get no results, the searched keyword might not have any newly registered domains on that date. Try:
- Different keywords
- Different dates
- Using regex patterns for broader matching

## Scheduled Scanning

You can set up automated daily scans using `cron.sh`:

```bash
# Edit cron.sh with your keywords
./cron.sh
```

Or set up a cron job:

```bash
# Add to crontab (run daily at 9 AM)
0 9 * * * cd /path/to/Hunting-New-Registered-Domains && python3.10 hnrd.py -f $(date +\%Y-\%m-\%d) -s paypal >> logs/scan.log 2>&1
```

## Updates (2025)

- ✅ Updated to Python 3.10+
- ✅ All dependencies upgraded to latest versions
- ✅ Fixed dnspython 2.x compatibility (`resolver.query()` → `resolver.resolve()`)
- ✅ Improved error handling for VirusTotal API
- ✅ Enhanced documentation with external services table

## Similar Projects

* [**dnstwist**](https://github.com/elceef/dnstwist) - Domain name permutation engine for detecting phishing
* [**phishing catcher**](https://github.com/x0rz/phishing_catcher) - Catch possible phishing domains in near real time

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is open source. Please check the repository for license details.

## Disclaimer

This tool is for educational and security research purposes only. Users are responsible for complying with applicable laws and the terms of service of external APIs and services used by this tool.
