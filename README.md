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
  - Find subdomains -> feed them back to the beginning of the workflow. Iterative process.
  - Linked discovery:
    - Check all links on the page to get new domains/subdomains
    - When new domains/subdomains are found, also add them to the spider list and repeat
    - Spiders to use: [hakrawler](https://github.com/hakluke/hakrawler) and [gospider](https://github.com/jaeles-project/gospider)
    - Similar tool: [subdomainizer](https://github.com/nsonaniya2010/SubDomainizer), get subdomains from javascript, also cloud info, potentially leaked keys.
    - Just for subdomains use [subscraper](https://github.com/Cillian-Collins/subscraper). It supports recursion.
  - Subdomain scraping:
    - Using Google:
      - First search for `site:twitch.tv`. Then `site:twitch.tv -www.twitch.tv`. Then `site:twitch.tv -www.twitch.tv -dev.twitch.tv`, iteratively adding the ones you've found
    - Using amass, subfinder, [github-subdomains](https://github.com/gwen001/github-subdomains), :
      - `amass -d twitch.tv`
      - `subfinder -d twitch.tv -v`
      - [github-subdomains](https://github.com/gwen001/github-subdomains): `github-subdomains -d twitch.tv` (rate limited, 5 iterations with a sleep of 6 seconds, and then wait 10 seconds)
      - [shosubgo](https://github.com/incogbyte/shosubgo): `shosubgo`: `go run main.go -d twitch.tv -s`
      - Scan cloud ranges and check if certificates match your target (masscan, nmap) Some bash scripting required
  - Subdomain bruting
    - It's important to use multiple DNS servers simultaneously
    - [massdns](https://github.com/blechschmidt/massdns)
    - Also amass uses 8 dns servers by default and also allows to set extra ones using the -rf flag
      - `amass enum -brute -d twitch.tv -src` (-src gets from where the subdomains are coming from)
      - `amass enum -brute -d twitch.tv -rf resolvers.txt -w bruteforce.list`
    
    
