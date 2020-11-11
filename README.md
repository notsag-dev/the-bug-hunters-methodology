# the-bug-hunters-methodology

## Get seed/root domains
- Check scope of the bug bounty program
- (If it corresponds) get acquisitions from Crunchbase
- Manually get IP blocks and ASN ids from bgp.he.net
- Do ASN recon using:
  - ASN lookup: `python asnlookup.py -o {{orgname}}
  - Metabigor: `echo "tesla" | metabigor net --org -v`
  - From the obtained ASNs, check them using Amass (check also metabigor for this): `amass intel -asn {{asnid}}`
- Do reverse whois:
  - whoxy.com: Lets you query who has owned a certain domain name in the past. It also contains other domains owned by the same companies (API key needed)
  - Use [domlink](https://github.com/vysecurity/DomLink) to automate queries to whoxy.com (API key needed)
- Use Builtwith to get technologies and also other related domains/subdomains through shared tracking codes that are occasionally reused
- Use Google to search for the copyright sentence: `"Twitch Interactive, Inc" inurl:twitch`
- Search the domain on Shodan
- Find subdomains:
  - Linked discovery:
    - Check all links on the page to get new domains/subdomains. Spider. Burp suite pro. I believe this can be done using Zap
    - When new domains/subdomains are found, also add them to the spider list and repeat
