# Google Cloud "Fortified Web App" Project

This project demonstrates my ability to secure a cloud environment using Google Cloud Platform (GCP). The scenario involved hardening a new, insecurely-deployed web server by applying best practices in networking, IAM, and monitoring.

## Skills Demonstrated
* **Network Security:** VPC Firewall configuration, restricting traffic by IP, and using Google's Identity-Aware Proxy (IAP).
* **IAM & Identity:** Applying the Principle of Least Privilege by creating and assigning a custom, no-permission Service Account.
* **Logging & Monitoring:** Creating log-based metrics from Cloud Logging and building an alerting policy in Cloud Monitoring to detect threats.

---

## The Process: From "Insecure" to "Secure"

### 1. Initial "Insecure" State
The project started with a basic Ubuntu VM on Compute Engine with several critical vulnerabilities:
* **Overly-permissive Network:** An ingress firewall rule allowed SSH (port 22) from any IP address on the internet (`0.0.0.0/0`), making it a prime target for brute-force attacks.
* **Over-privileged Identity:** The VM was running with the default Compute Engine service account, which has broad "Editor" permissions across the entire project.

**"Before" Screenshot:**
*(Insert your first screenshot here: the "allow-all-ssh" firewall rule)*

### 2. Security Remediation Steps

I applied a defense-in-depth strategy to secure the VM.

#### Step 1: Network Hardening (VPC Firewall)
I immediately deleted the insecure `allow-all-ssh` rule. I replaced it with a rule that *only* allows SSH from Google's Identity-Aware Proxy (IAP) service (`35.235.240.0/20`). This removes the server's SSH port from the public internet entirely, while still allowing authenticated administrators to connect via the GCP console.

**"After" Screenshot (Network):**
*(Insert your second screenshot here: the *new* firewall rules list showing the IAP rule)*

#### Step 2: Applying Principle of Least Privilege (IAM)
I created a new, custom Service Account named `web-server-sa` with *zero* roles or permissions. I then stopped the VM, attached this new service account, and restarted it. The web server can still function, but if it is compromised, the attacker has no permissions to move laterally or damage other GCP resources.

**"After" Screenshot (IAM Role):**
*(Insert your third screenshot here: the `web-server-sa` showing "No roles assigned")*

**"After" Screenshot (VM Attachment):**
*(Insert your fourth screenshot here: the VM Details page showing the new `web-server-sa` is attached)*

#### Step 3: Proactive Monitoring & Alerting
Finally, I set up a detection system.
1.  I created a **Log-based Metric** in Cloud Logging to count any "LOGIN_STATE_FAILURE" events.
2.  I used this metric in Cloud Monitoring to build an **Alerting Policy**. This policy is configured to send me an email immediately if even one failed SSH login is detected.

**"After" Screenshot (Log Metric):**
*(Insert your fifth screenshot here: the "Log-based Metrics" page showing your new metric)*

**"After" Screenshot (Alerting Policy):**
*(Insert your sixth screenshot here: the "Alerting" page showing your active policy)*

## Conclusion
This project successfully hardened a vulnerable web server by securing its network perimeter, applying the principle of least privilege, and enabling proactive monitoring.
