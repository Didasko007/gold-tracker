import json
import os
import sys
from datetime import datetime, timezone
from urllib.request import Request, urlopen

API_KEY = os.environ.get("GOLD_API_KEY")

if not API_KEY:
    print("ERROR: GOLD_API_KEY secret is not set.")
    sys.exit(1)

url = "https://www.goldapi.io/api/XAU/USD"

request = Request(
    url,
    headers={
        "x-access-token": API_KEY,
        "Content-Type": "application/json",
        "User-Agent": "gold-tracker/1.0",
    },
    method="GET",
)

try:
    with urlopen(request, timeout=20) as response:
        data = json.loads(response.read().decode("utf-8"))
except Exception as exc:
    print(f"ERROR: Could not retrieve GoldAPI price: {exc}")
    sys.exit(1)

if "price" not in data:
    print("ERROR: GoldAPI response did not contain 'price'.")
    print(json.dumps(data, indent=2))
    sys.exit(1)

output = {
    "symbol": "XAU/USD",
    "price": data.get("price"),
    "bid": data.get("bid"),
    "ask": data.get("ask"),
    "ch": data.get("ch"),
    "chp": data.get("chp"),
    "timestamp": data.get("timestamp"),
    "source_updated_utc": datetime.now(timezone.utc).isoformat(),
}

with open("price.json", "w", encoding="utf-8") as f:
    json.dump(output, f, indent=2)
    f.write("\n")

print(json.dumps(output, indent=2))
