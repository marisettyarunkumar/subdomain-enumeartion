# subdomain-enumeartion
1. What is Subdomain Enumeration

Subdomain enumeration is the process of discovering valid subdomains belonging to a target domain (for example: portswigger.com → blog.portswigger.com, academy.portswigger.com).

This is critical in penetration testing, bug bounty, and attack surface mapping because:

Each subdomain may host different applications with unique vulnerabilities.

Some subdomains are forgotten or unmonitored, making them weak points.

Attackers often exploit staging, development, or old applications via subdomains.
. Tools in Detail
Assetfinder

Created by Tomnomnom.

Pulls subdomains from multiple public data sources such as crt.sh, certspotter, Censys.

Quick way to gather an initial list of subdomains.

Advantage: fast, lightweight, no API keys needed.

Command example:

assetfinder --subs-only example.com


Output: a list of discovered subdomains.

Subfinder

From ProjectDiscovery.

Queries a wider set of APIs and providers (Shodan, VirusTotal, Certspotter, etc.).

More comprehensive than Assetfinder.

Requires some API keys (optional) for maximum results.

Command example:

subfinder -d example.com -o subfinder_results.txt


Output: more extensive list of subdomains, including those missed by Assetfinder.

Alterx

From ProjectDiscovery.

Alterx does not discover subdomains directly.

It takes existing subdomains and expands them using wordlists and templates.

Example: Given dev.example.com, Alterx might generate staging.dev.example.com, test.dev.example.com.

Helps uncover hidden or non-indexed subdomains.

Command example:

cat subfinder_results.txt | alterx -l words.txt > alterx_candidates.txt


Output: candidate subdomains that you can later test or resolve.
Example Workflow with Commands

Step 1: Discover with Assetfinder

assetfinder --subs-only example.com > data/assetfinder.txt


Finds subdomains from public sources and saves results to data/assetfinder.txt.

Step 2: Discover with Subfinder

subfinder -d example.com -silent -o data/subfinder.txt


Queries APIs and datasets for more thorough results. The option -silent removes banners for clean output.

Step 3: Aggregate and Clean

cat data/assetfinder.txt data/subfinder.txt | sort -u > data/all_subs.txt


Combines both outputs and removes duplicates.

Step 4: Expand with Alterx

cat data/all_subs.txt | alterx -l words.txt > data/alterx.txt


Takes known subdomains and generates variations using a wordlist (for example, dev, staging, test).

Step 5: Optional Validation

cat data/alterx.txt | httpx -silent -status-code -title -ip -o data/httpx_alive.txt


Checks which subdomains are alive and records HTTP status codes, titles, and IP addresses.
