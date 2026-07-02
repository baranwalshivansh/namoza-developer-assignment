 # Task 1 – GTM Event Schema Design

## Objective

The goal of this tracking plan is to measure how users interact with the OrthoNow website and identify where potential patients drop off during the appointment booking process. The implementation uses Google Tag Manager (GTM) to capture events and Google Analytics 4 (GA4) to analyse user behaviour and conversions.

---

# GTM Event Schema

| Event Name | Trigger Type | Key Parameters | Purpose |
|------------|--------------|----------------|---------|
| clinic_page_view | Page View | clinic_name, city, page_title | Measure clinic page traffic |
| booking_step_complete | Custom Event | step_number, step_name, clinic_location | Track booking progress |
| booking_completed | Form Success | booking_id, clinic_location, specialty | Measure completed appointments |
| call_now_click | Click Trigger | phone_number, page_name | Track phone enquiries |
| whatsapp_click | Click Trigger | page_name, button_position | Measure WhatsApp engagement |
| patient_guide_download | Form Submission | patient_name, phone, guide_name | Lead generation tracking |
| blog_scroll_50 | Scroll Depth | page_title, scroll_percent | Content engagement |
| blog_scroll_90 | Scroll Depth | page_title, scroll_percent | Highly engaged readers |
| consultation_form_submit | Form Submit | patient_name, phone, source | Consultation lead tracking |

---

# Booking Funnel Tracking

The booking form contains three steps. Since GTM cannot automatically identify progress in a multi-step form, the website should trigger a `dataLayer.push()` event whenever a user successfully completes a step.

These events are captured by GTM and sent to GA4 for Funnel Exploration.

---

## Step 1 – Select Location & Specialty

```json
{
  "event": "booking_step_complete",
  "step_number": 1,
  "step_name": "location_specialty_selected",
  "clinic_location": "Bengaluru - Indiranagar",
  "specialty": "Orthopaedics"
}
```

---

## Step 2 – Enter Patient Details

```json
{
  "event": "booking_step_complete",
  "step_number": 2,
  "step_name": "patient_details_entered",
  "patient_name": "Rahul Sharma",
  "phone": "9876543210",
  "preferred_date": "2026-07-10"
}
```

---

## Step 3 – Booking Confirmed

```json
{
  "event": "booking_step_complete",
  "step_number": 3,
  "step_name": "booking_confirmed",
  "booking_status": "success",
  "booking_id": "ORT123456",
  "clinic_location": "Bengaluru - Indiranagar"
}
```

---

# GTM Implementation

The frontend developer should add the above `dataLayer.push()` events after each successful booking step.

Google Tag Manager listens for these custom events using Custom Event Triggers and forwards the data to Google Analytics 4.

This allows the marketing team to monitor every stage of the booking journey without changing website code whenever tracking requirements change.

---

# GA4 Funnel Exploration

I would create a Funnel Exploration report in GA4 using the following sequence.

**Step 1**
- booking_step_complete
- step_number = 1

↓

**Step 2**
- booking_step_complete
- step_number = 2

↓

**Step 3**
- booking_step_complete
- step_number = 3

This report helps answer questions such as:

- How many users started the booking process?
- Which step has the highest drop-off?
- What percentage of users completed the booking?

These insights can be used to improve the booking experience and increase appointment conversions.

---

# Google Ads Conversion Recommendation

The event that should be imported into Google Ads is:

**booking_completed**

### Reason

Although users may click the Call Now or WhatsApp buttons, the final business goal is a successfully booked consultation.

Optimising campaigns for completed bookings helps Google Ads focus on high-quality leads rather than intermediate actions.

---

# Tracking Flow

```
Website

↓

dataLayer.push()

↓

Google Tag Manager (GTM)

↓

Google Analytics 4 (GA4)

↓

Google Ads Conversion Tracking
```

---

# Conclusion

This tracking setup provides complete visibility into the patient booking journey. By combining GTM, GA4 and Google Ads, the marketing team can measure campaign performance, identify booking drop-offs and optimise advertising based on actual appointment bookings instead of simple clicks.