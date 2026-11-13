# Firebase সংযোগ — Medicine & Surgical Products

এই প্যাকেজে Firebase Android plugin, Authentication এবং Cloud Firestore dependency প্রস্তুত করা হয়েছে। **এটি এখনো সম্পূর্ণ Firebase-connected অ্যাপ নয়**: আপনার Firebase project-এর `google-services.json` ছাড়া Google Services plugin দিয়ে APK build হবে না; আর Admin UI-তে Firebase login/data operation যুক্ত করার জন্য পরের কোড পরিবর্তনও প্রয়োজন।

## ১) Firebase project তৈরি
1. https://console.firebase.google.com/ খুলে একটি project তৈরি করুন।
2. Android app যোগ করুন; package name হুবহু `com.medicinesurgical.store` দিন।
3. Firebase Console থেকে `google-services.json` ডাউনলোড করুন।
4. ফাইলটি এই project-এর `app/google-services.json` স্থানে রাখুন (ZIP root-এ নয়)। ফাইলটি public repository-তে commit করবেন না।
5. Firebase Console → Authentication → Sign-in method থেকে Email/Password চালু করুন।
6. Firestore Database তৈরি করুন। Production mode বেছে নিন।

## ২) Admin নিরাপত্তা — অত্যন্ত গুরুত্বপূর্ণ
শুধু Firebase-এ sign-in হওয়া মানেই Admin নয়। Admin custom claim (`admin: true`) অবশ্যই trusted server/Admin SDK বা Cloud Functions দিয়ে দিতে হবে। Android app থেকে নিজে নিজে custom claim সেট করা যাবে না। প্রথম Admin claim দেওয়ার আগে আপনার পরিচয় যাচাই করে trusted environment ব্যবহার করুন; service-account key কখনো APK বা GitHub repository-তে রাখবেন না।

এই repository-র `firestore.rules` একটি নিরাপদ baseline: Admin custom claim ছাড়া কেউ product/order/customer collection পড়তে বা লিখতে পারবে না। Customer-facing public product catalogue চালু করার সময় আলাদা public-read rule ও field validation পরিকল্পনা করে যোগ করতে হবে।

## ৩) Firestore Rules প্রকাশ
Firebase Console → Firestore Database → Rules-এ `firestore.rules`-এর সম্পূর্ণ content বসিয়ে Publish করুন। Rules publish না করে production data রাখবেন না।

## ৪) Build
`app/google-services.json` যোগ করার পর Android Studio-তে Gradle Sync করুন। এরপর Build → Build APK(s) চালান। JSON ফাইল না থাকলে build ব্যর্থ হওয়া স্বাভাবিক।

## বর্তমান অবস্থা
- [x] Firebase Gradle plugin
- [x] Firebase Auth ও Firestore dependencies
- [x] Internet permission (আগের project-এ ছিল)
- [x] Admin-claim ভিত্তিক Firestore security rules baseline
- [ ] Firebase project তৈরি ও `google-services.json` যোগ
- [ ] Trusted server থেকে প্রথম Admin custom claim দেওয়া
- [ ] WebView Admin UI-তে Auth/Firestore operation যুক্ত করা ও end-to-end পরীক্ষা

কোনো বাস্তব গ্রাহক/অর্ডারের তথ্য ব্যবহারের আগে security rules, Admin provisioning এবং prescription/medicine compliance পরীক্ষা করুন।
