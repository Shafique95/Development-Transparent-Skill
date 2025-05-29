**Flutter iOS অ্যাপ** এর জন্য **Fastlane Fastfile** :

* ✅ Build IPA
* ✅ TestFlight (Beta) এ আপলোড
* ✅ App Store (Production) এ রিলিজ
* ✅ Version bump
* ✅ Code signing (using `match`)
* ✅ Error handling
* ✅ Flutter integration

সবকিছু **বাংলা ব্যাখ্যা সহ**।

---

## 📁 Flutter iOS App — Fastfile (Full Example)

```ruby
default_platform(:ios)

platform :ios do

  # ✅ Flutter Build for iOS (Release Mode)
  lane :build_flutter_ios do
    sh("flutter clean")
    sh("flutter build ios --release --no-codesign")
    gym(scheme: "Runner")  # Flutter iOS scheme usually is Runner
  end

  # 🚀 Upload to TestFlight (Beta)
  lane :beta do
    match(type: "appstore")  # Certificate & profile sync
    build_flutter_ios        # Flutter iOS build
    upload_to_testflight(skip_waiting_for_build_processing: true)
  end

  # 🚀 Upload to App Store (Production Release)
  lane :release do
    match(type: "appstore")
    build_flutter_ios
    deliver(force: true)
  end

  # 🔖 Version bump (iOS only)
  lane :version_bump do |options|
    increment_build_number(xcodeproj: "ios/Runner.xcodeproj")
    increment_version_number(
      version_number: options[:version]
    )
  end

  # 🛑 Error handling
  error do |lane, exception|
    puts "❌ ভুল হয়েছে lane: #{lane}, কারণ: #{exception.message}"
  end

  # ⚙️ আগে যা হবে
  before_all do
    puts "🚀 Flutter iOS build শুরু হচ্ছে..."
  end

  # ✅ পরে যা হবে
  after_all do |lane|
    puts "✅ সফলভাবে সম্পন্ন হয়েছে lane: #{lane}"
  end

end
```

---

## 📦 ফোল্ডার স্ট্রাকচার কেমন হবে?

আপনার Flutter প্রজেক্টের ভিতরে `ios` ফোল্ডারে গিয়ে Fastlane সেটআপ করুন:

```bash
cd ios
fastlane init
```

এরপর উপরের `Fastfile` টি `ios/fastlane/Fastfile` এ বসান।

---

## 🔑 প্রয়োজনীয় টুলস ও শর্ত

| টুল                    | কাজ                                          |
| ---------------------- | -------------------------------------------- |
| `flutter`              | Flutter CLI                                  |
| `gym`                  | iOS অ্যাপ বিল্ড করার জন্য                    |
| `match`                | iOS কোড সাইনিং (cert, provisioning profiles) |
| `deliver`              | App Store Connect এ রিলিজ করার জন্য          |
| `upload_to_testflight` | TestFlight এ আপলোড করার জন্য                 |
| Xcode                  | iOS বিল্ড ও সাইনিং                           |
| App Store Connect API  | অ্যাক্সেস টোকেন / JSON key                   |

---

## 🧪 কিভাবে চালাবেন?

### ✅ Build Only:

```bash
fastlane build_flutter_ios
```

### 🚀 TestFlight (Beta):

```bash
fastlane beta
```

### 🚀 App Store (Production):

```bash
fastlane release
```

### 🔖 Version Bump:

```bash
fastlane version_bump version:1.2.3
```

---

## ✅ Bonus: Match সেটআপ (code signing automation)

আপনার iOS অ্যাপের সাইনিং সহজ করতে `match` ব্যবহৃত হয়।

1. GitHub/GitLab এ একটি প্রাইভেট রিপোজিটরি তৈরি করুন।
2. Fastlane `match` দিয়ে সেটআপ করুন:

```bash
fastlane match init
```

3. ফাস্টফাইলে `match(type: "appstore")` ব্যবহার করুন।

---

## 🧩 অন্যান্য integration (প্রয়োজনে যুক্ত করা যাবে)

* 🔔 Slack notification
* 📦 Firebase App Distribution
* 📊 Crashlytics
* 🔐 Keystore/token secrets management

---

## 🔚 উপসংহার

এই `Fastfile` দিয়ে আপনি Flutter iOS অ্যাপের:

* Build
* Versioning
* TestFlight বেটা আপলোড
* App Store রিলিজ

সবকিছু **স্বয়ংক্রিয়ভাবে** করতে পারবেন।


