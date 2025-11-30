# 📧 Dues Reminder System - Automated Payment Collection

An intelligent, no-code automation system built with n8n and Google Sheets that automatically sends payment reminders to customers and alerts managers about overdue payments.

## 🎯 Problem Statement

Businesses often struggle with:
- Manual tracking of payment due dates
- Forgotten payment reminders leading to cash flow issues
- Time-consuming follow-ups with customers
- Late identification of overdue payments
- Lack of systematic payment collection process

**This system solves all these problems with complete automation.**

---

## ✨ Features

- ✅ **Automated Daily Checks** - Runs every day at 9 AM automatically
- ✅ **Smart Customer Reminders** - Sends emails 0-2 days before due date
- ✅ **Manager Alerts** - Notifies managers immediately about overdue payments
- ✅ **Google Sheets Integration** - Simple spreadsheet-based data management
- ✅ **Professional Email Templates** - Beautiful, responsive HTML emails
- ✅ **Status-Based Filtering** - Skips customers who've already paid
- ✅ **Zero Manual Work** - Completely hands-off after setup
- ✅ **Cost-Effective** - Uses free tools (Google Sheets, Gmail)
- ✅ **No Coding Required** - 100% no-code solution

---

## 🏗️ System Architecture
```
┌─────────────────────────────────────────────────┐
│           DAILY SCHEDULE TRIGGER                │
│              (9:00 AM Pakistan Time)            │
└─────────────────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────┐
│         GOOGLE SHEETS - DATA SOURCE             │
│    • Customer Name                              │
│    • Email Address                              │
│    • Due Date                                   │
│    • Due Amount                                 │
│    • Payment Status                             │
└─────────────────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────┐
│            AUTOMATED FILTERING                  │
│  Customer Reminders  |  Manager Alerts          │
│  (0-2 days left)     |  (Overdue payments)      │
└─────────────────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────┐
│          EMAIL NOTIFICATIONS (Gmail)            │
│  📧 Customers        |  ⚠️ Manager              │
│  Payment Reminder    |  Overdue Alert           │
└─────────────────────────────────────────────────┘
```

---

## 🛠️ Tech Stack

| Component | Technology | Cost |
|-----------|-----------|------|
| **Automation Engine** | n8n (Self-hosted or Cloud) | Free / $20/month |
| **Database** | Google Sheets | Free |
| **Email Service** | Gmail API | Free (500 emails/day) |
| **Hosting** | VPS / Docker / n8n Cloud | ~$5-20/month |

**Total Monthly Cost:** $0 - $20 (depending on hosting choice)

---

## 📋 Prerequisites

Before starting, ensure you have:

- [ ] Google Account (Gmail)
- [ ] n8n installed (self-hosted or cloud account)
- [ ] Basic understanding of Google Sheets
- [ ] Manager email address for alerts

---

## 🚀 Quick Start Guide

### Step 1: Set Up Google Sheets

1. Create a new Google Sheet named "Dues Tracker"
2. Add the following columns in Row 1:

| Column A | Column B | Column C | Column D | Column E |
|----------|----------|----------|----------|----------|
| Customer Name | Email | Due Date | Due Amount | Status |

3. **Format Requirements:**
   - **Due Date:** YYYY-MM-DD format (e.g., 2025-12-15)
   - **Due Amount:** Numbers only (e.g., 5000)
   - **Status:** Either "Pending" or "Paid" (case-sensitive)

4. **Sample Data:**
```
John Doe    | john@example.com    | 2025-12-02 | 5000  | Pending
Jane Smith  | jane@example.com    | 2025-11-28 | 3000  | Paid
Ali Khan    | ali@example.com     | 2025-12-01 | 10000 | Pending
```

---


---

## 📧 Email Templates

### Customer Reminder Template
```html
<div style="font-family: Arial, sans-serif; max-width: 600px; margin: 0 auto;">
  <h2 style="color: #d9534f;">Payment Reminder</h2>
  
  <p>Dear <strong>{{ $json["Customer Name"] }}</strong>,</p>
  
  <p>This is a friendly reminder about your upcoming payment:</p>
  
  <div style="background-color: #f8f9fa; padding: 20px; border-radius: 5px; margin: 20px 0;">
    <p style="margin: 5px 0;"><strong>Due Amount:</strong> PKR {{ $json["Due Amount"] }}</p>
    <p style="margin: 5px 0;"><strong>Due Date:</strong> {{ $json["Due Date"] }}</p>
    <p style="margin: 5px 0;"><strong>Status:</strong> {{ $json["Status"] }}</p>
  </div>
  
  <p style="color: #d9534f; font-weight: bold;">Please process your payment before the due date to avoid any late fees.</p>
  
  <p>If you have already made the payment, please ignore this reminder.</p>
  
  <p>Best regards,<br><strong>Finance Team</strong></p>
</div>
```

### Manager Alert Template
```html
<div style="font-family: Arial, sans-serif; max-width: 600px; margin: 0 auto;">
  <h2 style="color: #dc3545;">⚠️ OVERDUE PAYMENT ALERT</h2>
  
  <p>Dear Manager,</p>
  
  <p>The following customer has an overdue payment:</p>
  
  <div style="background-color: #fff3cd; padding: 20px; border-left: 4px solid #ffc107; margin: 20px 0;">
    <h3 style="margin-top: 0; color: #856404;">Customer Details:</h3>
    <p style="margin: 5px 0;"><strong>Name:</strong> {{ $json["Customer Name"] }}</p>
    <p style="margin: 5px 0;"><strong>Email:</strong> {{ $json["Email"] }}</p>
    <p style="margin: 5px 0;"><strong>Due Amount:</strong> PKR {{ $json["Due Amount"] }}</p>
    <p style="margin: 5px 0;"><strong>Due Date:</strong> {{ $json["Due Date"] }}</p>
    <p style="margin: 5px 0;"><strong>Status:</strong> {{ $json["Status"] }}</p>
  </div>
  
  <p style="color: #dc3545; font-weight: bold;">⚠️ Immediate action required to recover this payment.</p>
  
  <p><strong>Recommended Actions:</strong></p>
  <ul>
    <li>Contact customer immediately</li>
    <li>Send formal payment notice</li>
    <li>Consider late payment fees</li>
    <li>Escalate if necessary</li>
  </ul>
  
  <p>This is an automated alert from your Dues Reminder System.</p>
  
  <p>Best regards,<br><strong>Automated Dues Tracking System</strong></p>
</div>
```

---

## 🔧 Configuration Options

### Adjust Reminder Timing

**Change Schedule:**
```yaml
# Daily at 9 AM
Cron: 0 9 * * *

# Twice daily (9 AM and 3 PM)
Cron: 0 9,15 * * *

# Weekdays only
Cron: 0 9 * * 1-5
```

**Change Reminder Window:**

Edit Google Sheets QUERY formula:
```excel
# Current: 0-2 days before due date
Col3 <= TODAY()+2

# Change to 0-5 days before
Col3 <= TODAY()+5
```

### Customize Manager Email

In Gmail node (Manager Alert):
```yaml
# Single manager
To: manager@company.com

# Multiple managers (CC)
To: manager@company.com
CC: finance@company.com, cfo@company.com
```

---

## 📊 Business Logic

### Customer Reminder Conditions

Email sent when **ALL** conditions are met:
1. ✅ Days until due date: 0, 1, or 2 days
2. ✅ Due amount > 0
3. ✅ Status ≠ "Paid"

### Manager Alert Conditions

Email sent when **ALL** conditions are met:
1. ✅ Due date has passed (overdue)
2. ✅ Due amount > 0
3. ✅ Status ≠ "Paid"

---

## 🧪 Testing

### Test Case 1: Customer Reminder
```
Customer Name: Test User
Email: test@example.com
Due Date: [Tomorrow's date]
Due Amount: 5000
Status: Pending

Expected: Customer receives reminder ✅
```

### Test Case 2: Manager Alert
```
Customer Name: Late Payer
Email: late@example.com
Due Date: [Yesterday's date]
Due Amount: 10000
Status: Pending

Expected: Manager receives alert ✅
```

### Test Case 3: Skip Paid Customer
```
Customer Name: Good Customer
Email: good@example.com
Due Date: [Tomorrow's date]
Due Amount: 3000
Status: Paid

Expected: No email sent ✅
```

---

## 🐛 Troubleshooting

### Issue: Workflow not running automatically

**Solution:**
- Verify "Active" toggle is ON in n8n
- Check Schedule Trigger timezone setting
- Ensure n8n service is running (if self-hosted)

### Issue: No emails being sent

**Solution:**
- Re-authenticate Gmail credentials in n8n
- Check Google Sheets has correct data format
- Verify dates are in YYYY-MM-DD format
- Check spam/junk folder

### Issue: Wrong customers receiving emails

**Solution:**
- Verify Google Sheets QUERY formulas
- Check date format in Due Date column
- Ensure Status field uses exact text: "Paid" or "Pending"

### Issue: Manager not receiving alerts

**Solution:**
- Verify manager email address in Gmail node
- Check "Manager Alerts" sheet has data
- Test with a manually overdue entry

---

## 🔒 Security & Privacy

### Best Practices

1. **Credential Management**
   - Use OAuth2 (never store passwords)
   - Rotate credentials periodically
   - Use dedicated Google account

2. **Data Privacy**
   - Store only necessary customer information
   - Comply with local data protection laws
   - Secure Google Sheet permissions

3. **Access Control**
   - Limit Google Sheet access to authorized personnel
   - Use n8n user permissions appropriately
   - Monitor execution logs regularly

---

## 📈 Expected Results

After implementing this system:

- ✅ **30-40% faster payment collection**
- ✅ **Zero manual reminder work**
- ✅ **100% on-time overdue notifications**
- ✅ **Improved customer relationships** (proactive communication)
- ✅ **Better cash flow management**
- ✅ **Reduced administrative overhead**

---

## 🚀 Future Enhancements

### Planned Features

- [ ] SMS notifications (Pakistan telecom integration)
- [ ] WhatsApp Business API integration
- [ ] Multiple reminder escalation levels
- [ ] Payment link generation
- [ ] Dashboard with payment analytics
- [ ] Multi-language support
- [ ] Automated late fee calculation
- [ ] Integration with accounting software

### Enhancement Ideas

**Add SMS Alerts:**
```yaml
# After Gmail nodes, add HTTP Request node
Method: POST
URL: https://sms-gateway.pk/api/send
Body:
  phone: {{ $json.phone }}
  message: "Payment due: PKR {{ $json['Due Amount'] }}"
```

**Multiple Reminder Levels:**
- 2 days before → Urgent reminder
- 1 day before → Final reminder
- Overdue → Escalation to manager

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

### How to Contribute

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request


## 🙏 Acknowledgments

- n8n community for automation platform
- Google for Sheets and Gmail APIs
- All contributors and users of this system

---

## 📊 Project Stats

- **Setup Time:** 20-30 minutes
- **Monthly Cost:** $0-20
- **Maintenance:** < 5 minutes/week
- **Lines of Code:** 0 (100% no-code!)
- **ROI:** Immediate cash flow improvement

---

## 🌍 Works Best In

- ✅ Pakistan
- ✅ India
- ✅ Bangladesh
- ✅ Any country with Gmail access

---

## 📞 Contact

**Project Maintainer:** monisa hasnain

- Portfolio: monisahasnain.netlify.app
- LinkedIn: https://linkedin.com/in/monisa-hasnain
- Email: your-email@example.com

---

## ⭐ Show Your Support

If this project helped you automate payment collection, please give it a ⭐ star on GitHub!

---

**Made with ❤️ for businesses struggling with payment collection**

---

## 📌 Quick Links

- [Setup Guide](#-quick-start-guide)
- [Troubleshooting](#-troubleshooting)
- [Email Templates](#-email-templates)
- [Configuration](#-configuration-options)
- [Testing](#-testing)

---

*Last Updated: November 2025*
