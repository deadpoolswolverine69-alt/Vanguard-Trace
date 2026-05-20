#!/usr/bin/env python3
import sys
import hashlib
import json
import csv
import time
import httpx

# Professional-grade headers to make your PC look like a standard web browser
HEADERS = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/124.0.0.0 Safari/537.36",
    "Accept": "application/json, text/plain, */*",
    "Accept-Language": "en-US,en;q=0.9",
}

class VanguardTraceEngine:
    def __init__(self):
        """Initializes an error-resistant, browser-cloaked network client."""
        self.client = httpx.Client(headers=HEADERS, timeout=12.0, follow_redirects=True)

    def get_md5_hash(self, email: str) -> str:
        """Converts an email into an MD5 hash required by global profile registries."""
        return hashlib.md5(email.strip().lower().encode('utf-8')).hexdigest()

    def scan_profile_registry(self, email: str) -> dict:
        """Scrapes global forum and blogging registries for public real names."""
        email_hash = self.get_md5_hash(email)
        url = f"https://gravatar.com{email_hash}.json"
        
        try:
            response = self.client.get(url)
            if response.status_code == 200:
                data = response.json()
                entry = data.get("entry", [{}])[0]
                
                # Extracting specific biographical fields safely
                name_details = entry.get("name", {})
                full_name = name_details.get("formatted", entry.get("displayName", "Hidden Name"))
                
                return {
                    "status": "Profile Found",
                    "extracted_name": full_name,
                    "username": entry.get("preferredUsername", "Not set"),
                    "location": entry.get("currentLocation", "Not specified"),
                    "url": entry.get("profileUrl", "")
                }
            return {"status": "No Profile", "extracted_name": "None", "username": "None", "location": "None", "url": "None"}
        except Exception:
            return {"status": "Error/Skipped", "extracted_name": "None", "username": "None", "location": "None", "url": "None"}

    def scan_breach_footprint(self, email: str) -> str:
        """Checks if the email exists in historical public data leak dumps."""
        url = f"https://breachdirectory.org{email}"
        try:
            response = self.client.get(url)
            if response.status_code == 200:
                return "EXPOSED (Potential historical name leak in raw files)"
            elif response.status_code == 404:
                return "CLEAN (No public leak footprints detected)"
            return f"UNAVAILABLE (Response Code {response.status_code})"
        except Exception:
            return "Network Timeout"

    def execute_pipeline(self, target_email: str):
        """Executes the core scanning logic and formats the output screens cleanly."""
        print("\n" + "="*55)
        print(f"[*] VANGUARD TRACE ACTIVE - AUDITING: {target_email}")
        print("="*55)

        # Module 1: Profile Registry Check
        print("[*] Accessing global profile registries...")
        profile = self.scan_profile_registry(target_email)
        
        print(f"    -> Registry Status: {profile['status']}")
        print(f"    -> Real Name Given: {profile['extracted_name']}")
        print(f"    -> System Alias   : {profile['username']}")
        print(f"    -> Stated Location: {profile['location']}")
        
        print("-" * 55)

        # Module 2: Corporate Leak Check
        print("[*] Syncing with historical data breach catalogs...")
        leak_status = self.scan_breach_footprint(target_email)
        print(f"    -> Leak Status    : {leak_status}")
        print("="*55)

        # Module 3: Automatic Case Export Engine
        self.export_case_file(target_email, profile, leak_status)

    def export_case_file(self, email: str, profile: dict, leak: str):
        """Automatically saves findings to a clean CSV case spreadsheet on your PC."""
        filename = "vanguard_trace_log.csv"
        file_exists = False
        
        try:
            with open(filename, "r") as f:
                file_exists = True
        except FileNotFoundError:
            pass

        with open(filename, "a", newline="", encoding="utf-8") as csv_file:
            fields = ["Target Email", "Registry Status", "Extracted Name", "Username Alias", "Location", "Leak History", "Timestamp"]
            writer = csv.DictWriter(csv_file, fieldnames=fields)
            
            if not file_exists:
                writer.writeheader()

            writer.writerow({
                "Target Email": email,
                "Registry Status": profile["status"],
                "Extracted Name": profile["extracted_name"],
                "Username Alias": profile["username"],
                "Location": profile["location"],
                "Leak History": leak,
                "Timestamp": time.strftime("%Y-%m-%d %H:%M:%S")
            })
        print(f"[+] Case successfully logged to local file: '{filename}'\n")

if __name__ == "__main__":
    # Check if user provided an email via command line arguments
    if len(sys.argv) > 1:
        target = sys.argv[1]
    else:
        # Fallback test address if you run the script directly with no arguments
        target = "example_test_account@gmail.com"
        print("[!] No email argument found. Running basic system test on placeholder address.")

    hunter = VanguardTraceEngine()
    hunter.execute_pipeline(target)
