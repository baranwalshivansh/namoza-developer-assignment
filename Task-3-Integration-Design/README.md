# Task 3 – Integration Design

This setup is meant to connect the OrthoNow website with HubSpot CRM and WhatsApp (Karix) so that every consultation request is properly stored and the patient gets a quick confirmation message.

---

## How the flow works

When a user submits the consultation form on the landing page:

1. The form first sends a `dataLayer` event to Google Tag Manager.
2. GTM forwards this event to Google Analytics 4 for tracking.
3. At the same time, the form data is sent to a backend API.

From the backend:

- The system first checks HubSpot using the phone number.
- If the contact already exists, it just updates the details.
- If it doesn’t exist, a new contact is created.

After this step, the backend triggers a WhatsApp message through the Karix API.

---

## HubSpot side

We store basic details like:

- Name  
- Phone number  
- Selected clinic  
- Source (Google Ads landing page)

The important thing here is to avoid duplicate entries. So phone number is always used to search before creating a new contact.

---

## WhatsApp message (Karix)

Once the lead is saved in HubSpot, a WhatsApp message is sent to the patient.

Something like:

> Hi, thanks for booking a consultation with OrthoNow. Our team will contact you shortly.

This message should go within 2 minutes of form submission.

---

## Possible issues

### 1. Duplicate leads
Since users may submit multiple times, duplicates can happen.

Fix:
- Always check phone number first before creating a new contact.

---

### 2. WhatsApp delay or failure
Sometimes the API may fail or respond slowly.

Fix:
- Retry the request if it fails
- Keep logs for monitoring
- Add a small retry queue for pending messages

---

## Tracking (GTM + GA4 + Ads)

We use GTM to capture the form submission event:

```
consultation_form_submit
```

This is sent to:
- GA4 for funnel tracking
- Google Ads as a conversion event

So marketing teams can see which campaigns are actually bringing real bookings.

---

## Summary

Overall, the idea is simple:

- Capture lead on website  
- Store it properly in CRM  
- Send instant WhatsApp confirmation  
- Track everything in GA4 and Google Ads  

This keeps both marketing and operations in sync without manual work.