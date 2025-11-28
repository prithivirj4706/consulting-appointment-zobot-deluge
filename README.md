# Consulting Appointment Booking Zobot

A fully functional appointment booking chatbot built with **Zoho SalesIQ**, **Deluge scripting**, and **Google Calendar API** integration. Designed for hospitals, clinics, consulting services, and appointment-based businesses.

## Features

- Appointment booking with service selection
- Collect visitor details (name, email, phone)
- OTP-based phone verification
- Real-time Google Calendar availability
- Reschedule/cancel appointments
- Payment integration (Razorpay/Stripe)
- OAuth 2.0 authentication
- Email confirmations

## Built For

- **Platform**: Zoho SalesIQ
- **Language**: Deluge
- **Third-party Integration**: Google Calendar API
- **SMS Provider**: Twilio / AWS SNS
- **Email**: Zoho Mail / SendGrid
- **Payment**: Razorpay / Stripe

## Project Structure

```
.
├── salesiq-bot/
│   ├── main_bot_script.ds
│   ├── booking_flow.ds
│   └── reschedule_flow.ds
├── plugs/
│   ├── plug_otp_generate_send.ds
│   ├── plug_fetch_slots_google.ds
│   ├── plug_create_booking.ds
│   └── plug_payment_integration.ds
├── oauth/
│   ├── google_oauth_setup.md
│   └── environment_variables.md
├── docs/
│   ├── SETUP.md
│   ├── API_INTEGRATION.md
│   └── BOT_FLOW.md
└── README.md
```

## Quick Start

### Prerequisites
- Zoho SalesIQ account
- Google Calendar API credentials
- Twilio account (for SMS)
- Payment gateway account (Razorpay/Stripe)

### Step 1: Create Zobot
1. Log into Zoho SalesIQ
2. Go to Settings → Developers → Bots → Create New Bot
3. Copy content from `salesiq-bot/main_bot_script.ds`
4. Save as "Consultation Appointment Bot"

### Step 2: Set Up OAuth
1. Create Google OAuth 2.0 credentials
2. Add redirect URI: `https://salesiq.zoho.com/oauth/callback`
3. Store Client ID and Secret in SalesIQ Connection

### Step 3: Create Plugs
1. Go to Settings → Developers → Plugs
2. Create 3 custom plugs from files in `plugs/` folder
3. Map them in the bot script

### Step 4: Configure Environment
Set up environment variables:
```
GOOGLE_CLIENT_ID=your_client_id
GOOGLE_CLIENT_SECRET=your_client_secret
TWILIO_ACCOUNT_SID=your_sid
TWILIO_AUTH_TOKEN=your_token
PAYMENT_GATEWAY=razorpay
```

## Bot Flow

### Booking an Appointment
1. User selects service type (carousel)
2. Provide name, email, phone
3. Receive OTP and verify
4. Select preferred date
5. View available time slots
6. Pick time slot
7. Create event in Google Calendar
8. Receive confirmation email
9. Optional: Make payment

### Reschedule/Cancel
1. Verify email
2. Fetch upcoming appointments
3. Select appointment
4. Choose: reschedule or cancel
5. Update/delete from Google Calendar

## API Integrations

**Google Calendar API**
- Endpoint: `https://www.googleapis.com/calendar/v3/`
- Methods: LIST, CREATE, UPDATE, DELETE events
- Auth: OAuth 2.0

**Twilio SMS API**
- Endpoint: `https://api.twilio.com/2010-04-01/`
- Methods: Send SMS
- Auth: Account SID + Auth Token

**Razorpay API**
- Endpoint: `https://api.razorpay.com/v1/`
- Methods: CREATE orders
- Auth: Basic Auth

## Key Deluge Scripts

**main_bot_script.ds**
- Context handler: Bot greeting + main menu
- Message handler: Route to booking/reschedule/cancel

**booking_flow.ds**
- Service selection carousel
- Collect user details
- OTP verification
- Fetch and display time slots
- Create booking event

**reschedule_flow.ds**
- Fetch user appointments
- Update or cancel bookings

## Plug Scripts

**plug_otp_generate_send.ds** (Plug 1)
- Generate 6-digit OTP
- Send via Twilio SMS

**plug_fetch_slots_google.ds** (Plug 2)
- Query Google Calendar
- Return available slots

**plug_create_booking.ds** (Plug 3)
- Create Google Calendar event
- Send confirmation email
- Generate payment link

## OAuth 2.0 Flow

1. Bot requests calendar access
2. Redirect to Google consent screen
3. User authorizes bot
4. Google returns authorization code
5. Exchange code for access token
6. Token stored in Zoho vault
7. Auto-refresh on expiry

## Payment Integration

After slot selection:
1. Create order with Razorpay
2. Generate payment URL
3. Display "Pay Now" button
4. On success: confirm booking

## Security Features

- OTP-based phone verification
- OAuth 2.0 for Google Calendar
- Email verification for reschedule
- Secure data storage in Zoho
- No hardcoded credentials

## Brownie Points Implemented

✅ Payment Integration (Razorpay)
✅ OAuth 2.0 Authentication
✅ AI Triage (symptom-based department suggestion)
✅ Multiple plugs for modularity
✅ Complete error handling

## Testing

1. Log into SalesIQ
2. Open chat widget
3. Type: "Book an appointment"
4. Follow flow and verify each step

For plug testing:
```deluge
plug_result = zoho.salesiq.plug.execute("OTP_GENERATE_SEND", {"phone": "+91xxxxxxxxxx"});
info plug_result;
```

## Deployment

1. Test thoroughly in your account
2. Create production Google OAuth credentials
3. Enable bot in live chat widget
4. Monitor SalesIQ dashboard
5. Track metrics: booking success, dropoff points

## Troubleshooting

**OTP not sending?**
- Check Twilio credentials
- Verify phone format: +country_code
- Check rate limits

**No calendar slots?**
- Verify OAuth token
- Check Google Calendar permissions
- Ensure calendar shared with bot

**Email not sending?**
- Verify Zoho Mail API key
- Check recipient email
- Review templates

## Contributing

Contributions welcome! Please:
1. Fork repository
2. Create feature branch: `git checkout -b feature/your-feature`
3. Commit: `git commit -m 'Add feature'`
4. Push: `git push origin feature/your-feature`
5. Submit Pull Request

## License

MIT License - See LICENSE file for details

## Support

For issues or questions:
- Create GitHub Issue
- Check troubleshooting docs
- Review Zoho SalesIQ documentation

## Acknowledgments

- Built with Zoho SalesIQ Scripts (Deluge)
- Google Calendar API
- Zoho Internship Program 2025
- Community contributors

---

**Made with ❤️ for Zoho Internship 2025**
