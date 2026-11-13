# ধাপ ২৬ — Customer Order History ও Tracking

## নতুন সুবিধা
- Customer `আমার অর্ডার` থেকে নিজের Firebase order list দেখতে পারবেন।
- প্রতিটি অর্ডারে Order ID, পণ্যের পরিমাণ, মোট মূল্য, delivery address এবং বর্তমান status দেখা যাবে।
- Admin status পরিবর্তন করলে customer refresh করে নতুন status দেখতে পারবেন।
- Customer query Firebase anonymous user-এর UID দিয়ে সীমাবদ্ধ; অন্য customer-এর order পড়তে পারবেন না।

## Firebase Rules
এই ZIP-এর `firestore.rules` Firebase Console → Firestore Database → Rules-এ Publish করতে হবে। Rules-এ customer নিজের `customerUid`-এর order read করতে পারে; create কেবল নিজের UID ও Pending status দিয়ে; update/delete কেবল Admin custom claim-এ।

## সীমাবদ্ধতা
- Customer identity Firebase Anonymous Authentication-এর persistent UID-নির্ভর; app data clear বা uninstall হলে anonymous account হারাতে পারে। Cross-device order history-এর জন্য পরবর্তী ধাপে phone OTP/account linking দরকার।
- Status tracking এখন Admin-এর manually updated status; live courier GPS tracking নয়।
- APK build/device test এই ZIP তৈরির সময় করা হয়নি। `google-services.json` আপনার Firebase project থেকে `app/google-services.json`-এ দিতে হবে; এটি public GitHub-এ commit করবেন না।
