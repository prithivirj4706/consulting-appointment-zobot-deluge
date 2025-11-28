# Setup Guide - Consulting Appointment Booking Zobot

Complete step-by-step guide to set up the appointment booking zobot in your Zoho SalesIQ instance.

## Prerequisites

- Zoho SalesIQ account with admin access
- Zoho SalesIQ Scripts enabled
- Google Cloud Project with Calendar API enabled
- Twilio account (for SMS/OTP)
- Zoho Mail or SendGrid account (for emails)
- Basic understanding of Deluge scripting

## Step 1: Prepare Google Calendar API

### 1.1 Create Google Cloud Project
1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project named "Zobot Appointment Booking"
3. Enable Google Calendar API:
   - Go to APIs & Services > Library
   - Search for "Google Calendar API"
   - Click "Enable"

### 1.2 Create OAuth 2.0 Credentials
1. Go to APIs & Services > Credentials
2. Click "Create Credentials" > "OAuth 2.0 Client ID"
3. Choose "Web application"
4. Add authorized redirect URI:
   ```
   https://salesiq.zoho.com/oauth/callback
   ```
5. Create and copy your Client ID and Client Secret

## Step 2: Set Up Zoho SalesIQ Connection

### 2.1 Create Connection in SalesIQ
1. Go to SalesIQ Settings > Developers > Connections
2. Click "Create Connection"
3. Select "OAuth 2.0"
4. Fill in details:
   - **Connection Name**: google_calendar_connection
   - **Authorization URL**: https://accounts.google.com/o/oauth2/v2/auth
   - **Token URL**: https://oauth2.googleapis.com/token
   - **Client ID**: Your Google Client ID
   - **Client Secret**: Your Google Client Secret
   - **Scope**: https://www.googleapis.com/auth/calendar
5. Save and authorize

## Step 3: Create Zobot in SalesIQ Scripts

### 3.1 Create Main Bot Script
1. Go to Settings > Developers > Bots
2. Click "Create New Bot" > "Create from Script"
3. Copy content from `salesiq-bot/main_bot_script.ds`
4. Paste into editor
5. Save as "Consultation Appointment Bot"

### 3.2 Customize Services
Edit the `show_services()` function to add your services:
```deluge
suggestions.add("Your Service 1");
suggestions.add("Your Service 2");
```

## Step 4: Create Plugs

### 4.1 OTP Generation Plug (Plug 1)
1. Go to Settings > Developers > Plugs
2. Click "Create New Plug"
3. Copy content from `plugs/plug_otp_generate_send.ds`
4. Set up Twilio connection in the plug:
   - Replace `TWILIO_ACCOUNT_SID` with your Twilio Account SID
   - Replace `TWILIO_AUTH_TOKEN` with your Twilio Auth Token
   - Replace `TWILIO_PHONE_NUMBER` with your Twilio phone number
5. Save as "OTP_GENERATE_SEND"

### 4.2 Google Calendar Slots Plug (Plug 2)
1. Create new Plug
2. Copy content from `plugs/plug_fetch_slots_google.ds`
3. Save as "FETCH_SLOTS_GOOGLE"
4. No modifications needed - uses OAuth connection created in Step 2

### 4.3 Booking Creation Plug (Plug 3)
1. Create new Plug
2. Copy content from `plugs/plug_create_booking.ds` (when created)
3. Configure email sending:
   - Set up Zoho Mail API key or SendGrid key
   - Update sender email address
4. Save as "CREATE_BOOKING"

## Step 5: Configure Environment Variables

Create `.env` file locally (for reference):
```env
# Google OAuth
GOOGLE_CLIENT_ID=your_client_id_here
GOOGLE_CLIENT_SECRET=your_client_secret_here

# Twilio
TWILIO_ACCOUNT_SID=your_account_sid_here
TWILIO_AUTH_TOKEN=your_auth_token_here
TWILIO_PHONE_NUMBER=+1XXXXXXXXXX

# Email Configuration
ZOHO_MAIL_API_KEY=your_zoho_mail_key
SENDER_EMAIL=appointments@yourcompany.com

# Payment (Optional)
PAYMENT_GATEWAY=razorpay
RAZORPAY_KEY_ID=your_key_id
RAZORPAY_KEY_SECRET=your_key_secret
```

## Step 6: Test the Bot

### 6.1 Enable Bot in Widget
1. Go to SalesIQ > Settings > Chat Customization
2. Enable the bot in your chat widget
3. Set it as the first responder (optional)

### 6.2 Test Flow
1. Open your website's chat widget
2. Type: "Book an appointment"
3. Follow the entire booking flow:
   - Select service
   - Provide details
   - Verify OTP
   - Select date
   - View and pick time slot
   - Confirm booking
4. Check your email for booking confirmation
5. Verify event appears in Google Calendar

## Step 7: Customize Business Rules

### 7.1 Modify Working Hours
Edit `plug_fetch_slots_google.ds`:
```deluge
start_time = date + "T09:00:00Z"; // Your start time
end_time = date + "T17:00:00Z";   // Your end time
```

### 7.2 Change Slot Duration
Pass `duration_minutes` parameter when calling the plug:
```deluge
"duration_minutes": 45  // Change from default 30 minutes
```

### 7.3 Add More Services
Update `show_services()` function in main bot script

## Troubleshooting

### OTP not sending?
- Verify Twilio credentials
- Check phone number format (+country_code format)
- Review Twilio account balance
- Check SMS rate limits

### Calendar not showing slots?
- Verify OAuth token is valid
- Check Google Calendar permissions
- Ensure calendar is not empty
- Verify API is enabled in Google Cloud

### Email not received?
- Check recipient email address
- Verify email API credentials
- Check spam folder
- Review email template

## Security Checklist

- [ ] Never commit `.env` file to GitHub
- [ ] Use secure Connections in SalesIQ for all API keys
- [ ] Enable OTP verification for all bookings
- [ ] Regularly rotate API keys and tokens
- [ ] Monitor bot usage and errors
- [ ] Use HTTPS for all API calls
- [ ] Encrypt sensitive data in storage

## Support

For issues or questions:
1. Check `TROUBLESHOOTING.md`
2. Review logs in SalesIQ Scripts
3. Test individual plugs independently
4. Verify API credentials and permissions
5. Consult Zoho SalesIQ documentation

## Next Steps

After successful setup:
1. Customize bot responses for your brand
2. Add more services/departments
3. Integrate with your CRM (Zoho CRM)
4. Set up analytics and reporting
5. Enable payment integration for premium services
6. Train your team on managing bookings

---

**Made for Zoho Internship 2025**
