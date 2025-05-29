 **সম্পূর্ণ Fastfile** (Android অ্যাপের জন্য):

* ✅ Debug build
* ✅ Release build
* ✅ Beta রিলিজ (Play Store-এর বেটা ট্র্যাকে আপলোড)
* ✅ Production রিলিজ
* ✅ Custom lane with parameter
* ✅ Error handling

সবকিছু **বাংলা ব্যাখ্যা সহ**!

---

## 📁 Fastfile (Android Version) — সম্পূর্ণ উদাহরণ

```ruby
default_platform(:android)

platform :android do

  # 🔧 সাধারণ বিল্ড (debug)
  lane :build_debug do
    gradle(
      task: "assembleDebug"
    )
  end

  # 🔧 রিলিজ বিল্ড
  lane :build_release do
    gradle(
      task: "assembleRelease"
    )
  end

  # 🚀 Beta রিলিজ (Play Store এর beta ট্র্যাকে আপলোড)
  lane :beta do
    gradle(task: "assembleRelease")
    upload_to_play_store(track: 'beta')
  end

  # 🚀 Production রিলিজ (প্রোডাকশন ট্র্যাকে আপলোড)
  lane :release do
    gradle(task: "assembleRelease")
    upload_to_play_store(track: 'production')
  end

  # ⚙️ Custom বিল্ড টাইপ লেন (parameter সহ)
  lane :build_custom do |options|
    build_type = options[:build_type] || "Debug"
    gradle(
      task: "assemble#{build_type}"
    )
  end

  # 🔁 অন্য lane কল করার উদাহরণ
  lane :full_release do
    build_release
    upload_to_play_store(track: 'production')
  end

  # 🛑 Error হলে হ্যান্ডল করা
  error do |lane, exception|
    puts "❌ ভুল হয়েছে lane: #{lane}, কারন: #{exception.message}"
  end

  # 📌 Lane শুরু হওয়ার আগে
  before_all do
    puts "⚙️ শুরু হচ্ছে Android বিল্ড প্রসেস..."
  end

  # ✅ Lane শেষ হওয়ার পর
  after_all do |lane|
    puts "✅ Lane সফলভাবে সম্পন্ন হয়েছে: #{lane}"
  end

end
```

---

## 📜 কিভাবে এই Fastfile ব্যবহার করবেন?

### 1. ✅ Debug build:

```bash
fastlane build_debug
```

### 2. ✅ Release build:

```bash
fastlane build_release
```

### 3. 🚀 Beta release (Play Store বেটা ট্র্যাকে):

```bash
fastlane beta
```

### 4. 🚀 Production release:

```bash
fastlane release
```

### 5. ⚙️ Custom build (Debug/Release/QA ইত্যাদি):

```bash
fastlane build_custom build_type:Release
```

---

## 🧩 প্রয়োজনীয় শর্ত (Requirements):

* `gradle` কাজ করতে হলে আপনার `build.gradle` ফাইল সঠিকভাবে সেটআপ করা থাকতে হবে।
* `upload_to_play_store` কাজ করতে হলে Fastlane-কে Google Play Console-এর JSON API key দিয়ে authorize করতে হবে (`supply` সেটআপ সহ)।

---

## 🔚 উপসংহার

এই Fastfile দিয়ে আপনি Android অ্যাপের সমস্ত গুরুত্বপূর্ণ বিল্ড ও ডিপ্লয়মেন্ট কাজ অটোমেট করতে পারবেন। এটি আপনার সময় বাঁচাবে এবং রিলিজ প্রক্রিয়া সহজ করে তুলবে।

🔧 আপনি চাইলে আমি আপনার `build.gradle` দেখে `Fastlane` পুরোপুরি আপনার প্রজেক্টের জন্য কাস্টমাইজ করেও দিতে পারি।

আর কোনো নির্দিষ্ট কাজ চাইলে বলুন — যেমন:

* Firebase App Distribution
* Slack notification
* Version bumping
* Code signing with Keystore
