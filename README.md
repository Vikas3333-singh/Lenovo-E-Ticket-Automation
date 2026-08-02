# Lenovo E-Ticket Automation System for Hardware Support Management

> Python + Selenium automation tool for Lenovo support ticketing — single and bulk ticket creation, auto Excel logging with case IDs and statuses, and open/closed case monitoring.

---

## Problem

Raising Lenovo support tickets was fully manual — engineers logged into the portal, filled forms one by one, then copied case IDs and statuses back into an Excel tracker by hand. For bulk hardware issues across multiple devices, this took significant time and was prone to copy-paste errors and missed updates.

---

## Solution

TicketForge automates the full Lenovo ticketing lifecycle:

- **Single ticket mode** — fill one input, script handles the rest on the portal
- **Bulk ticket mode** — reads a list of devices from Excel, creates tickets for each automatically
- **Auto Excel update** — writes case IDs and ticket statuses back to the tracker on creation
- **Case monitoring** — checks open and closed case statuses and updates Excel accordingly

---

## Features

| Feature | Detail |
|---------|--------|
| Single ticket creation | Automates form fill and submission on Lenovo Support portal |
| Bulk ticket creation | Reads device list from Excel, loops through and creates all tickets |
| Auto case ID logging | Captures case ID from portal after submission, writes to Excel |
| Status tracking | Monitors open/closed status per case, updates tracker automatically |
| Error handling | Flags failed submissions with reason in Excel for manual review |

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Language | Python 3.x |
| Browser Automation | Selenium WebDriver |
| Excel Read/Write | `openpyxl` |
| Browser Driver | ChromeDriver / GeckoDriver |
| Portal | Lenovo Support Web Portal |

---

## Project Structure

```
ticketforge/
├── main.py                  # Entry point — single or bulk mode
├── ticket_creator.py        # Selenium logic — form fill and submission
├── case_monitor.py          # Status checker — open/closed case tracking
├── excel_handler.py         # openpyxl — read input, write case IDs and statuses
├── config.py                # Portal URL, credentials, field mappings, file paths
├── data/
│   └── devices.xlsx         # Input: device list | Output: case IDs + statuses
├── logs/
│   └── ticketforge.log      # Run log — successes, failures, timestamps
├── requirements.txt
└── README.md
```

---

## How It Works

### Single Ticket Mode

```bash
python main.py --mode single
```

1. Prompts for device serial number and issue details
2. Selenium opens Lenovo Support portal, logs in, fills the form
3. Submits ticket, captures case ID from confirmation page
4. Writes case ID + status to `devices.xlsx`

---

### Bulk Ticket Mode

```bash
python main.py --mode bulk --input data/devices.xlsx
```

Input Excel format:

| Serial No | Device Model | Issue | Case ID | Status |
|-----------|-------------|-------|---------|--------|
| PF2X1234 | ThinkPad X1 | Battery not charging | — | — |
| PF2X5678 | ThinkPad T14 | Keyboard failure | — | — |

1. Script reads all rows where Case ID is empty
2. Loops through each device — opens portal, fills form, submits
3. Captures case ID and writes back to the same row
4. Status set to `Open` on creation

---

### Case Monitoring

```bash
python main.py --mode monitor
```

1. Reads all rows in Excel where Status is `Open`
2. Selenium checks each case on the portal
3. Updates Status column — `Open`, `Closed`, `In Progress`
4. Timestamps the last check in the tracker

---

## Excel Tracker (Output)

| Serial No | Device Model | Issue | Case ID | Status | Last Checked |
|-----------|-------------|-------|---------|--------|--------------|
| PF2X1234 | ThinkPad X1 | Battery not charging | LEN-882341 | Closed | 15-Jan-2025 |
| PF2X5678 | ThinkPad T14 | Keyboard failure | LEN-882356 | Open | 15-Jan-2025 |
| PF2X9012 | ThinkPad E14 | Display issue | FAILED | Form error | 15-Jan-2025 |

---

## Setup

```bash
# Clone
git clone https://github.com/your-username/ticketforge.git
cd ticketforge

# Virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Configure
# Edit config.py — add portal credentials, file paths, ChromeDriver path
```

---

## Configuration (`config.py`)

```python
PORTAL_URL      = "https://support.lenovo.com"
PORTAL_USER     = "your-email@company.com"
PORTAL_PASSWORD = "your-password"

EXCEL_PATH      = "data/devices.xlsx"
CHROMEDRIVER    = "drivers/chromedriver.exe"   # or path to geckodriver

LOG_PATH        = "logs/ticketforge.log"
RETRY_ATTEMPTS  = 3       # Retries per ticket on failure
DELAY_BETWEEN   = 5       # Seconds between bulk submissions
```

---

## Requirements

```
selenium
openpyxl
webdriver-manager
```

---

## Impact

- Eliminated manual form-filling on Lenovo portal for single and bulk ticket creation
- Case IDs and statuses auto-logged — no copy-paste errors in Excel tracker
- Open case monitoring runs on demand — IT team has real-time visibility without logging into the portal manually
- Consistent tracking and reporting across the full support lifecycle

---

## Author

**Vikas Singh**
System Analyst | IT Operations | Endpoint Management
[LinkedIn](#) · [GitHub](#)

---

## License

MIT
