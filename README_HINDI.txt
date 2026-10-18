SHAKTI CRM - ANDROID APP (बिना Chrome के असली ऐप)
==================================================
ये क्या है: एक असली Android ऐप (APK), जो अपने आइकन से खुलता है, Chrome के बिना चलता है।
ऐप आपकी लाइव वेबसाइट (Vercel) को ही खोलता है, इसलिए वेबसाइट में जो भी बदलाव करेंगे वो ऐप में अपने आप आ जाएगा। हर बार नया APK नहीं बनाना पड़ेगा।
ऐप बंद होने पर भी: नई लीड, चैट और कॉल की रिंग आएगी (Firebase के ज़रिए, WhatsApp की तरह)।

सिर्फ़ एक बार करना है (लगभग 30 मिनट)
------------------------------------------
स्टेप 1 - Firebase (फ्री)
  1. console.firebase.google.com -> Add project (कोई भी नाम, Analytics बंद कर सकते हैं)
  2. Project में "Add app" -> Android -> Package name बिल्कुल यही लिखें:  in.shakticrm.app   -> Register
  3. google-services.json डाउनलोड करें (इसे खोलकर पूरा टेक्स्ट कॉपी कर लें)
  4. Project settings -> Service accounts -> "Generate new private key" -> एक JSON फ़ाइल डाउनलोड होगी (ये दूसरी फ़ाइल है)

स्टेप 2 - वेबसाइट (Vercel)
  1. ShaktiCRM_v175.zip को वैसे ही Vercel पर डालें जैसे हर बार डालते हैं।
  2. Vercel -> Settings -> Environment Variables में नया वैरिएबल जोड़ें:
       नाम:  FIREBASE_SERVICE_ACCOUNT
       वैल्यू: स्टेप 1 के (4) वाली JSON फ़ाइल का पूरा टेक्स्ट
  3. Redeploy करें। फिर  https://आपकी-साइट/api/push  खोलें, उसमें "androidApp":true दिखना चाहिए।
  (कोई नया SQL नहीं चलाना है।)

स्टेप 3 - APK बनवाना (GitHub, फ्री)
  1. github.com पर नई Repository बनाएँ (Public रखें, ताकि डाउनलोड लिंक सबके लिए खुले। इसमें कोई पासवर्ड या CRM कोड नहीं है)।
  2. इस kit का पूरा सामान उसमें अपलोड करें। ध्यान दें:
       - PRIVATE_DO_NOT_UPLOAD नाम का फ़ोल्डर अपलोड नहीं करना है।
       - .github नाम का छिपा फ़ोल्डर भी जाना ज़रूरी है। अगर ड्रैग करने पर न जाए तो:
         Add file -> Create new file -> नाम में लिखें  .github/workflows/build-apk.yml  -> उस फ़ाइल का टेक्स्ट चिपकाएँ।
  3. SITE_URL.txt खोलकर उसमें अपनी वेबसाइट का पता लिखें (https:// से शुरू), जैसे https://shakticrm.vercel.app
  4. Repository -> Settings -> Secrets and variables -> Actions -> New repository secret (3 बार):
       GOOGLE_SERVICES_JSON = स्टेप 1 (3) वाली google-services.json का पूरा टेक्स्ट
       KEYSTORE_B64         = PRIVATE_DO_NOT_UPLOAD/KEYSTORE_B64.txt का पूरा टेक्स्ट
       KEYSTORE_PASSWORD    = PRIVATE_DO_NOT_UPLOAD/KEYSTORE_PASSWORD.txt का टेक्स्ट
     (KEYSTORE वाली फ़ाइलें ऐप की "चाबी" हैं। इन्हें संभालकर रखें। इसी से आगे के अपडेट पुराने ऐप के ऊपर इंस्टॉल होंगे।)
  5. Actions टैब -> "Build Shakti CRM Android APK" -> Run workflow। लगभग 8-10 मिनट लगेंगे।
  6. Repository के दाईं तरफ़ Releases -> सबसे नया -> ShaktiCRM.apk  <- यही आपका डाउनलोड लिंक है।
     (हमेशा सबसे नया वाला लिंक: github.com/आपका-नाम/रिपॉ-नाम/releases/latest/download/ShaktiCRM.apk)

एजेंट/एडमिन के फ़ोन पर इंस्टॉल
--------------------------------
  1. ऊपर का लिंक फ़ोन में खोलें -> ShaktiCRM.apk डाउनलोड -> "Install unknown apps" की अनुमति दें -> Install
  2. ऐप खोलकर लॉगिन करें। Notifications और Microphone, दोनों में Allow दबाएँ।
  3. पुराना Chrome वाला आइकन हटा दें, वरना हर अलर्ट दो बार आएगा।
  4. फ़ोन Settings -> Apps -> Shakti CRM -> Battery -> "Unrestricted" (Xiaomi/Oppo/Vivo/Realme में "Autostart" भी चालू करें)।
     ये न करने पर कई फ़ोन ऐप को बंद करके अलर्ट रोक देते हैं।

ईमानदारी से: क्या पक्का है और क्या नहीं
-----------------------------------------
  * मैंने ये kit यहाँ बनाया है, पर यहाँ इंटरनेट और Android टूल नहीं है, इसलिए असली APK बनाकर फ़ोन पर चलाकर नहीं देखा।
    पुश भेजने वाला सर्वर कोड मैंने नकली डेटा से जाँचा है। पहली बार का बिल्ड कहीं अटक सकता है। Actions में जो लाल error दिखे वो मुझे भेज दें, मैं ठीक कर दूँगा।
  * कॉल की रिंग अभी "ऊपर से आने वाली नोटिफिकेशन + रिंग की आवाज़" जैसी है (करीब 4 सेकंड)। WhatsApp की तरह पूरी स्क्रीन पर लगातार बजने वाली कॉल
    के लिए अलग से नेटिव कोड चाहिए। वो अगला चरण हो सकता है।
  * Invoice/PDF/CSV "Download" दबाने पर फ़ोन की Share विंडो खुलेगी (Save / WhatsApp / Drive)। सीधा Downloads फ़ोल्डर में नहीं जाएगा।
    Invoice का Print बटन ऐप में काम नहीं भी कर सकता। टेस्ट करके बताइए।
  * Play Store पर डालना इस kit में नहीं है। ये सीधे APK लिंक से बाँटने के लिए है। iPhone के लिए नहीं है।
  * कॉल लॉग (फ़ोन की साधारण कॉल अपने आप लिखना) इसमें नहीं है। अभी जो CRM के अंदर वाली कॉल और tel: वाली कॉल है वही चलेगी।
