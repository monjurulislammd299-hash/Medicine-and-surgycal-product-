# ধাপ ২৮ — Firebase Phone OTP Login

এই ধাপে গ্রাহক বাংলাদেশি মোবাইল নম্বরে SMS OTP যাচাই করে লগইন করবেন। আগে Anonymous Firebase session-এ তৈরি অর্ডার থাকলে OTP credential link করার মাধ্যমে একই Firebase UID বজায় রাখার চেষ্টা করা হয়; ফলে সেই অর্ডারগুলোও একই গ্রাহক দেখতে পারবেন।

## Firebase Console সেটআপ
1. Firebase Console → Authentication → Sign-in method → **Phone** Enable করুন।
2. Authentication-এর SMS region policy-তে Bangladesh (+880) অনুমোদিত আছে নিশ্চিত করুন।
3. Project settings → Your apps → Android app-এ debug এবং release signing certificate-এর **SHA-1 ও SHA-256** যোগ করুন।
4. নতুন `google-services.json` ডাউনলোড করে `app/` ফোল্ডারের পুরোনো ফাইল প্রতিস্থাপন করুন।
5. Authentication-এ test phone numbers যোগ করে প্রথমে test করুন। বাস্তব SMS-এর জন্য Firebase-এর quota/billing ও SMS delivery policy প্রযোজ্য হতে পারে।

## কোডে কী আছে
- `FirebaseWebBridge.kt`: `startPhoneOtp()` SMS পাঠায় এবং `verifyPhoneOtp()` কোড যাচাই করে।
- Anonymous user থাকলে phone credential `linkWithCredential()` দিয়ে একই UID-তে যুক্ত হয়।
- অর্ডার তালিকা এখন Anonymous অথবা Phone-authenticated UID দিয়ে লোড হয়।
- Login UI-তে OTP যাচাই না হলে verified profile সংরক্ষণ হয় না।
- Checkout-এ verified phone number read-only হিসেবে ব্যবহৃত হয়।

## Build ও পরীক্ষা
1. Android Studio-তে Gradle Sync করুন।
2. ফোনে APK ইনস্টল করে +8801XXXXXXXXX ফরম্যাটে নম্বর দিয়ে OTP পাঠান।
3. OTP যাচাই করে একটি COD অর্ডার দিন।
4. একই অ্যাকাউন্টে `আমার অর্ডার` খুলে অর্ডারটি দেখুন।
5. অ্যাপের ডেটা মুছে ফেলার আগে নিশ্চিত করুন যে phone credential সফলভাবে account-এ link হয়েছে; তারপর পুনরায় লগইন করে অর্ডার history পরীক্ষা করুন।

## গুরুত্বপূর্ণ সীমাবদ্ধতা
- একই ফোন নম্বর আগে অন্য Firebase account-এ যুক্ত থাকলে credential collision হতে পারে; এই সংস্করণে স্বয়ংক্রিয়ভাবে দুই account-এর order merge করা হয় না।
- Firebase Phone Auth-এর Android app verification, SHA fingerprint, SMS region policy, quota এবং delivery বাস্তব ডিভাইসে পরীক্ষা করতে হবে।
- OTP হলো ফোন নম্বরের নিয়ন্ত্রণ যাচাই; এটি একা পরিচয়, চিকিৎসা-পরামর্শ বা prescription বৈধতার প্রমাণ নয়।
- Release APK-র signing SHA fingerprint আলাদাভাবে Firebase-এ যোগ করতে হবে।
