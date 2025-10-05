# oracleCloudSetup
Instructions on how to host webpage on Oracle Cloud

# Oracle Cloud Ubuntu Instance Setup Guide

## 1. Choosing the Shape
- **E2.Micro** is a free-tier eligible shape.
- **Specialty:** Required for free-tier micro instances.
- **Processor choice:**
  - **AMD (x86)** → safest for Docker, Python, and full compatibility.
  - **Ampere (ARM)** → cheaper, works with ARM-compatible software, but some Docker images may not run.

**Recommendation:** Pick **AMD** for maximum compatibility with Docker and Python.

---

## 2. Networking / Public IP
- When creating a new instance:
  1. Select **Create new public subnet**.
  2. Make sure **Assign a public IPv4 address** is set to **Yes**.
- After creation, check the instance details page for the **Public IP**.

---

## 3. Accessing the Instance
**Cloud Shell (recommended if port 22 is blocked):**
1. Open **Cloud Shell** in the OCI Console.
2. Upload your private key if not already present:
   ```bash
   nano mykey.key
   chmod 600 mykey.key
   ```
   - Paste the private key content into `mykey.key`.
3. SSH into the instance:
   ```bash
   ssh -i mykey.key ubuntu@<your_public_ip>
   ```
4. Type `yes` if asked to confirm the server fingerprint.

**Optional:** Use **OCI Bastion** for managed SSH sessions without exposing port 22.

---

## 4. Minimizing Resource Usage
- **Stop the instance** to preserve OS, data, and installed software while halting compute charges:
  - Console: Click **Stop** on the instance page.
  - OCI CLI:
    ```bash
    oci compute instance action --instance-id <instance_OCID> --action STOP
    ```
- **Charges while stopped:**
  - Storage (boot and block volumes) continues.
  - Public IP (if reserved) continues.
- **Optional safety measures:**
  - Create a **boot volume backup**.
  - Create a **custom image** for quick redeployment.
- Restart anytime using **Start** in the console or OCI CLI.

---

## 5. Notes
- Active SSH/Cloud Shell sessions will **disconnect** when the instance is stopped.
- No additional steps are needed beyond stopping the instance to save compute costs.
- Cloud Shell and Bastion are preferred methods for accessing instances when port 22 is blocked.

---

**TL;DR:**
- Pick **AMD (x86)** for Ubuntu + Docker + Python compatibility.
- Use **public subnet + assign public IPv4** for external access.
- Connect via **Cloud Shell** if SSH is blocked.
- **Stop** the instance when idle to save costs; your data and OS remain intact.

