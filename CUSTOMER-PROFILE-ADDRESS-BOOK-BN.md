# ধাপ ২৯ — Customer Profile ও Saved Address

## নতুন সুবিধা
- ফোন OTP দিয়ে লগইন করা গ্রাহকের নাম ও যাচাইকৃত ফোন নম্বর `customerProfiles/{uid}`-এ সংরক্ষণ।
- একই UID-এর অধীনে সর্বোচ্চ ৩০টি saved delivery address লোড করা।
- ঠিকানা যোগ, নির্বাচন করে checkout-এ বসানো এবং মুছে ফেলা।
- Firestore Rules-এ গ্রাহককে শুধু নিজের profile ও address subcollection-এ প্রবেশাধিকার দেওয়া।
- Admin claim-যুক্ত অ্যাকাউন্ট customer profile path ব্যবহার করতে পারবে না; Admin-এর জন্য আলাদা admin workflow থাকবে।

## Firestore Data Structure
- `customerProfiles/{uid}`: `name`, `phone`, `updatedAt`
- `customerProfiles/{uid}/addresses/{addressId}`: `label`, `address`, `recipient`, `phone`, `createdAt`

## সেটআপ
1. Firebase Console → Authentication → Phone provider সক্রিয় রাখুন।
2. `firestore.rules` ফাইলের সম্পূর্ণ নিয়ম Firebase Console → Firestore Database → Rules-এ Publish করুন।
3. Android Studio-তে Gradle Sync করুন এবং APK Build করুন।
4. Phone OTP দিয়ে লগইন করে Profile খুলুন; একটি Home/Office ঠিকানা যোগ করুন।
5. ঠিকানা থেকে “এই ঠিকানা ব্যবহার” নির্বাচন করে Checkout-এ prefill হয়েছে কিনা পরীক্ষা করুন।

## নিরাপত্তা ও সীমাবদ্ধতা
- Rules-এ `request.auth.token.phone_number` ব্যবহার করে profile phone-কে Firebase Phone Auth token-এর নম্বরের সঙ্গে মিলানো হয়।
- ঠিকানার ফোন নম্বর delivery contact হতে পারে, তাই সেটি profile phone থেকে আলাদা হতে পারে।
- ঠিকানার সর্বোচ্চ ৩০টি entry UI/bridge-এ লোড সীমা; production-এ rate limiting ও abuse protection যোগ করুন।
- গ্রাহক অ্যাকাউন্ট মুছে ফেলা, OTP নম্বর পরিবর্তন, ঠিকানা edit, default address sync এবং account recovery এখনো নেই।
- বাস্তব Firebase Rules Emulator/Android device test এবং Gradle build এখনো করতে হবে।
