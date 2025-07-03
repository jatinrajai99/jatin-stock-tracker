# 📦 Required Libraries
import requests
from bs4 import BeautifulSoup
from twilio.rest import Client

# ✅ Twilio Credentials (replace with your actual SID & TOKEN)
account_sid = 'YOUR_TWILIO_ACCOUNT_SID'
auth_token = 'YOUR_TWILIO_AUTH_TOKEN'
client = Client(account_sid, auth_token)

# ✅ Your verified WhatsApp Number
to_whatsapp_number = 'whatsapp:+919898436842'
from_whatsapp_number = 'whatsapp:+14155238886'  # Twilio Sandbox number

# 📊 NSE Stocks to track under ₹50
stock_urls = {
    'Vodafone Idea': 'https://www.moneycontrol.com/india/stockpricequote/telecommunications/vodafoneidea/VI44',
    'Yes Bank': 'https://www.moneycontrol.com/india/stockpricequote/banks-public-sector/yesbank/YB',
    'Trident': 'https://www.moneycontrol.com/india/stockpricequote/textiles-processing/trident/TI',
    'IRB Infra': 'https://www.moneycontrol.com/india/stockpricequote/constructioncontracting-civil/irbinfrastructuredevelopers/ID06',
    'Suzlon': 'https://www.moneycontrol.com/india/stockpricequote/miscellaneous/suzlonenergy/SE'
}

# 📈 Function to fetch price
def fetch_price(url):
    try:
        res = requests.get(url)
        soup = BeautifulSoup(res.text, 'html.parser')
        price_tag = soup.find('div', {'class': 'inprice1'})
        return price_tag.text.strip()
    except:
        return 'N/A'

# 🔔 Build message
message_body = "📊 Jatin Stock Tracker – Budget Alerts\n\n"
for stock, url in stock_urls.items():
    price = fetch_price(url)
    message_body += f"✅ {stock}: ₹{price}\n"

# 🚀 Send WhatsApp Message via Twilio
client.messages.create(
    body=message_body,
    from_=from_whatsapp_number,
    to=to_whatsapp_number
)

print("✅ WhatsApp alert sent successfully!")
