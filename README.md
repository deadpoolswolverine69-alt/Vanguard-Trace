> 👶 **Look guys, I know almost nothing about coding, I am jus a baby.** This is my first script, with ideas I seen in other source codes an compiled this project. I would love for feedback like "this looks great, or stick to your day job" or you can improve this in anyway, I am all ears. Again, I jus started this and thought out the box outside of generic. Also Ive only started learning about scripts an coding a few weeks ago. So please be nice. 🙏

---


# 🛰️ Vanguard-Trace

Vanguard-Trace is a lightweight, high-speed Python Open-Source Intelligence (OSINT) footprint analyzer. Built to be completely passive and white-hat, it allows individuals to audit their own digital exposure by running deep network queries, public PGP cryptographic ledger verification, and domain security infrastructure checks.

## ⚡ Features
- **Infrastructure Auditing:** Resolves active Mail Exchange (MX) routing hosts and evaluates DMARC anti-spoofing policies.
- **Cryptographic Recon:** Interrogates public OpenPGP registries for active public key blocks.
- **Identity Registry Mapping:** Verifies account creation indicators across public API endpoints like GitHub, Gravatar, and Keybase.
- **Permutation Generator:** Translates base handles into structural variations to catch historical tracking footprints.

## 🚀 Installation & Usage

```bash
# Clone the project repository
git clone https://github.com
cd Vanguard-Trace

# Install required network resolution dependencies
pip install httpx dnspython

# Execute the interrogation pipeline
python vanguard-trace.py target_email@example.com
```
