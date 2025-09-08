# subdomain-enumeration
1. What is Subdomain Enumeration

Subdomain enumeration is the process of discovering valid subdomains belonging to a target domain (for example: portswigger.com → blog.portswigger.com, academy.portswigger.com).

This is critical in penetration testing, bug bounty, and attack surface mapping because:

Each subdomain may host different applications with unique vulnerabilities.

Some subdomains are forgotten or unmonitored, making them weak points.

Attackers often exploit staging, development, or old applications via subdomains.
. Tools in Detail
Assetfinder

Step 1: Discover Subdomains with Assetfinder

Purpose: Quickly gather an initial subdomain list from public sources like crt.sh and Certspotter.

Command:

assetfinder --subs-only example.com > ~/subdomains/data/assetfinder.txt


--subs-only ensures only subdomains are returned.

Advantage: Fast, lightweight, no API keys needed.

Step 2: Discover More Subdomains with Subfinder

Purpose: Query multiple APIs (Shodan, VirusTotal, Certspotter) to find subdomains missed by Assetfinder.

Command:

subfinder -d example.com -silent -o ~/subdomains/data/subfinder.txt


-silent removes banners for clean output.

Output: more comprehensive list.

Optional: Add API keys in ~/.config/subfinder/config.yaml for better results.

Step 3: Aggregate and Clean

Combine and deduplicate results from Assetfinder and Subfinder:

cat ~/subdomains/data/assetfinder.txt ~/subdomains/data/subfinder.txt | sort -u > ~/subdomains/data/all_subs.txt


sort -u removes duplicates.

all_subs.txt is your master list of discovered subdomains.

Step 4: Expand Subdomains with Alterx

Purpose: Generate hidden or non-indexed subdomains using known ones and a wordlist.

Command:

cat ~/subdomains/data/all_subs.txt | alterx -l words.txt > ~/subdomains/data/alterx.txt


words.txt contains common prefixes: dev, staging, test, admin.

Output: Candidate subdomains for further testing.

Note: Alterx does not discover new subdomains directly; it generates permutations from existing ones.

Step 5: Optional Validation

If you want to check which subdomains are alive, you can use tools like httpx later. But with just Assetfinder, Subfinder, and Alterx, your workflow ends with alterx.txt for candidate subdomains.
