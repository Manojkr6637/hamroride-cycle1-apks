# HamroRide Cycle 1 — Client install / क्लाइन्ट इन्स्टल

Closed-beta **sideload**. These are **not** Play Store production builds. Bike taxi in **Kathmandu Valley** only. Cash. No tempo / cab / parcel / iOS / consumer web.

**Download (HTTPS):** [GitHub Release v1.0.0](https://github.com/Manojkr6637/hamroride-cycle1-apks/releases/latest)

| App | Android package | APK |
| --- | --- | --- |
| Customer / यात्रु | `np.hamroride.customer` | [hamroride-customer-1.0.0.apk](https://github.com/Manojkr6637/hamroride-cycle1-apks/releases/download/v1.0.0/hamroride-customer-1.0.0.apk) |
| Captain / क्याप्टेन | `np.hamroride.captain` | [hamroride-captain-1.0.0.apk](https://github.com/Manojkr6637/hamroride-cycle1-apks/releases/download/v1.0.0/hamroride-captain-1.0.0.apk) |

API (HTTPS): `https://hamroride-api-production.up.railway.app`  
AAB files are on the same Release (Play **Internal** later — not public production).

Needs **Android 7.0+** (API 24). Typical 2 GB Valley phones are OK.

---

## English

### 1. Download

On the phone Chrome/browser, open the **Customer** or **Captain** APK link above. Save the file. You do **not** need Android Studio or a PC after this.

### 2. Install unknown apps (Unknown sources)

The Play Store is not used. Android will block the APK until you allow this browser (or Files) to install unknown apps.

**Typical path (names vary by brand):**

1. Open the downloaded `.apk`.
2. If Android says *For your security, your phone is not allowed to install unknown apps from this source* → tap **Settings**.
3. Turn **Allow from this source** **ON** for Chrome / Files / your browser.
4. Go back and tap **Install**.
5. Xiaomi / Oppo / Vivo: also check **Security** → **Install unknown apps** (or **USB installation** / **Install via USB** off is fine; this is a file install).

Uninstall any old debug build of the same package first if Install says *conflict* / *signature mismatch*.

### 3. Permissions

**Customer (`HamroRide`)**

- **Internet** — quotes, book, OTP.
- The map library may also ask for **Location**. Cycle 1 does not need GPS (you drag pins). Deny is OK; Allow is OK.

**Captain (`HamroRide Captain`)**

- **Location** (precise) while Online and during a trip.
- **Location — allow all the time** / background: the app declares it for the **active-trip** ping only. If the phone asks, allow it so the passenger map can move. If you deny background, you can still demo with the **stand chips** (Thamel / New Road / Baneshwor).
- **Internet**.

### 4. Test numbers (do not use a random real SIM yet)

| Role | Type in the app | Full MSISDN | Notes |
| --- | --- | --- | --- |
| Passenger | `9800000001` | `9779800000001` | Book Bike |
| Captain (Thamel) | `9800000002` | `9779800000002` | KYC **ACTIVE**, plate `BA 1 PA 1234` |
| Captain (New Road) | `9800000003` | `9779800000003` | KYC **ACTIVE**, plate `BA 2 PA 5678` |

A passenger number stays passenger. Use the captain numbers in the **Captain** app.

### 5. OTP how-to (no SMS vendor)

Cycle 1 uses **fake SMS**. The app **never** shows the 6-digit code.

1. In the app: language (नेपाली default) → phone → **Request OTP**.
2. Read the code **one** of these ways (staging API):
   - Browser on the phone or PC:  
     `https://hamroride-api-production.up.railway.app/v1/dev/otp?msisdn=9779800000001`  
     (change the number: `…002` captain Thamel, `…003` New Road). Copy the `otp` field.
   - Operator PC: `railway logs --service hamroride-api --lines 50` and look for `[fake-sms] msisdn=... otp=......`
3. Type the **6 digits**. Login OTP is **not** the ride PIN (ride PIN is **4 digits** after a captain accepts).

If inspect returns **404**, the host was flipped to production env — ask the operator; do not guess codes.

### 6. Two-phone demo

Two packages: both can sit on **one** phone, but two phones is the real demo.

1. **Captain phone:** install Captain APK → OTP `9800000002` → KYC should already be ACTIVE → stand **Thamel** → **Online**. Heartbeat dies in **20 seconds** if you leave Online off.
2. **Customer phone:** install Customer APK → OTP `9800000001` → map **Thamel → New Road** → fare about **NPR 51.45** (bike only) → **Book Bike**.
3. Customer shows **Searching** (no fake ETA). Captain offer card (~1 s). **Accept**.
4. Customer: **4-digit ride PIN**, helmet waiting, Share. Captain: type PIN, tick **two helmets**, **Start trip**.
5. Optional: either app **SOS**. Desk case is queued (`policeAutoDial` is off). Ops inbox is internal, not a public website.
6. Captain **Complete drop** → **Cash received**. Both see receipt: **HamroRide is not the vehicle owner**. Both rate 1–5 stars.
7. Honest **NoCaptain:** captain **Offline** (or wait >20 s), customer Books again with a **fresh** quote.

Share URL (no login): `https://hamroride-api-production.up.railway.app/t/{publicId}` — this trip only; no home address, no full phone, no ride PIN.

Optional captain **Daily pass NPR 25** (cash) → 0% take that Nepal calendar day; GMV still on the receipt. Without a pass, take is ~8% (capped at 10%).

### 7. Limits (Cycle 1)

- **Valley bbox** only. Bike **`BIKE_STD`** only. **Cash** first (wallet stub, no eSewa live).
- Not Play production. Not iOS. Not consumer web Book Ride. No ads. No tempo/cab/parcel.
- OTP is fake / inspect URL — not a real SMS aggregator.
- API sessions live **in memory**. A Railway restart drops passenger logins; seed captains are re-activated on boot. Request a **new OTP**. Go **Online** again before Book.
- Presence TTL **20 s**. Quote TTL **12 min**. Matching max **4 km**.
- SOS pages an **ops queue**; it does **not** auto-dial 100.
- Crash reporting (Firebase Crashlytics) is **not** in these APKs (would have blocked sideload). Crashes will not appear in a Firebase console.
- Legal: `legal.licence_issued=false`. Receipts must not say government-approved.

---

## नेपाली

### १. डाउनलोड

फोनको ब्राउजरबाट माथिको **Customer** वा **Captain** APK लिंक खोल्नुहोस्। फाइल सेभ गर्नुहोस्। यसपछि Android Studio वा PC चाहिँदैन।

### २. अज्ञात एप इन्स्टल (Unknown apps)

Play Store होइन। पहिलो पटक इन्स्टल गर्दा फोनले रोक्न सक्छ।

1. डाउनलोड भएको `.apk` खोल्नुहोस्।
2. *Unknown apps* / *अज्ञात स्रोत* भने **Settings** थिच्नुहोस्।
3. Chrome / Files का लागि **Allow from this source** अन गर्नुहोस्।
4. फर्केर **Install** थिच्नुहोस्।
5. Xiaomi / Oppo / Vivo: **Security → Install unknown apps** पनि हेर्नुहोस्।

पुरानो debug APK भए पहिले अनइन्स्टल गर्नुहोस् (हस्ताक्षर बाझियो भने)।

### ३. अनुमति

**यात्रु एप:** इन्टरनेट। नक्सा लाइब्रेरीले लोकेसन पनि सोध्न सक्छ — Cycle 1 मा GPS अनिवार्य छैन (पिन तान्नुहोस्); Deny पनि हुन्छ।

**क्याप्टेन एप:** लोकेसन (सटीक)। ट्रिप चलिरहेका बेला ब्याकग्राउन्ड लोकेसन सोधे अनुमति दिनुहोस्। नदिए पनि स्ट्यान्ड चिप (ठमेल / नयाँ सडक) ले डेमो चल्छ।

### ४. टेस्ट नम्बर

| भूमिका | एपमा टाइप | नोट |
| --- | --- | --- |
| यात्रु | `9800000001` | बुक बाइक |
| क्याप्टेन (ठमेल) | `9800000002` | प्लेट `BA 1 PA 1234`, KYC ACTIVE |
| क्याप्टेन (नयाँ सडक) | `9800000003` | प्लेट `BA 2 PA 5678` |

क्याप्टेन एपमा यात्रु नम्बरले काम गर्दैन।

### ५. OTP कसरी पढ्ने

एसएमएस आउँदैन। एपले ६ अङ्क देखाउँदैन।

1. एपमा फोन हालेर OTP माग्नुहोस्।
2. ब्राउजर:  
   `https://hamroride-api-production.up.railway.app/v1/dev/otp?msisdn=9779800000001`  
   (`otp` फिल्ड कपी गर्नुहोस्। क्याप्टेनका लागि `…0002` / `…0003`।)
3. **६ अङ्क** हाल्नुहोस्। राइड PIN भने **४ अङ्क** हो (क्याप्टेन Accept पछि यात्रु स्क्रिनमा)।

### ६. दुई-फोन डेमो

1. क्याप्टेन: OTP `9800000002` → ठमेल → **Online**।
2. यात्रु: OTP `9800000001` → ठमेल → नयाँ सडक, भाडा करिब **NPR ५१.४५** → **Book Bike**।
3. यात्रु: Searching। क्याप्टेन: अफर **Accept**।
4. यात्रुको ४-अङ्क PIN + दुई हेलमेट → **Start**।
5. चाहे **SOS** (डेस्क केस; १०० आफैं लाग्दैन)।
6. Complete drop → नगद आयो → रसिद (**HamroRide गाडीको मालिक होइन**) → दुवैतिर स्टार।
7. क्याप्टेन Offline → फेरि Book = इमानदार **NoCaptain** (नयाँ quote)।

### ७. सीमा

काठमाडौं उपत्यका, बाइक मात्र, नगद, Play प्रोडक्सन होइन, iOS/वेब/टेम्पो/क्याब/पार्सल छैन। API रिस्टार्ट भए नयाँ OTP। Online नगरी Book नगर्नुहोस् (२० सेकेन्डमा heartbeat सकिन्छ)।
