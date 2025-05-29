Fastlane DSL-এর `lane`-এর **পুরো syntax (বিন্যাস)**, বিভিন্ন ধরনের `lane` কমান্ড, এবং সেগুলোর ব্যবহার কিভাবে করতে হয় তা **বাংলায় ব্যাখ্যা সহ** দিচ্ছি।

---

## 📦 `lane` Syntax – Full Structure

```ruby
lane :lane_name do
  # lane-এর মধ্যে আপনি বিভিন্ন Fastlane action বা Ruby কোড ব্যবহার করতে পারেন
end
```

### ✅ উদাহরণ:

```ruby
lane :beta do
  gradle(task: "assembleRelease")
  upload_to_play_store(track: 'beta')
end
```

---

## 🧱 Lane Structure Explained in Bangla (বাংলায় ব্যাখ্যা)

| অংশ          | ব্যাখ্যা                                                                   |
| ------------ | -------------------------------------------------------------------------- |
| `lane`       | Fastlane DSL-এ একটি lane তৈরি করতে ব্যবহৃত হয়।                             |
| `:lane_name` | lane-এর নাম, আপনি যেকোনো নাম দিতে পারেন যেমন: `:beta`, `:release` ইত্যাদি। |
| `do ... end` | lane-এর কাজগুলো এখানে লেখা হয় (Ruby DSL ব্লক)।                             |

---

## 🧩 Lane-এর ভেতরে যেসব কাজ করা যায় (lane actions)

Fastlane-এর `lane` এর ভেতরে আপনি নিচের মতো কাজ করতে পারেন:

### ✅ Android:

```ruby
lane :build_android do
  gradle(task: "assembleRelease")
end
```

### ✅ iOS:

```ruby
lane :build_ios do
  match(type: "appstore")
  gym(scheme: "MyApp")
  deliver(force: true)
end
```

---

## 🎯 Multiple Lanes একসাথে:

```ruby
platform :android do
  lane :beta do
    gradle(task: "assembleRelease")
    upload_to_play_store(track: 'beta')
  end

  lane :release do
    gradle(task: "assembleRelease")
    upload_to_play_store(track: 'production')
  end
end
```

```ruby
platform :ios do
  lane :beta do
    match(type: "appstore")
    gym(scheme: "MyApp")
    pilot  # TestFlight-এ আপলোড
  end

  lane :release do
    match(type: "appstore")
    gym(scheme: "MyApp")
    deliver
  end
end
```

---

## 🔁 Lane-এর মধ্যে অন্য Lane কল করা

```ruby
lane :prepare do
  puts "Preparing for build..."
end

lane :full_release do
  prepare
  build_android_app
end
```

---

## ⚙️ Lane-এ parameter ব্যবহার (with parameters):

```ruby
lane :custom_build do |options|
  gradle(task: "assemble#{options[:build_type]}")
end
```

### রান করার সময়:

```bash
fastlane custom_build build_type:Release
```

---

## 🔑 Lane সংক্রান্ত গুরুত্বপূর্ণ বিষয় (বাংলায়):

| কনসেপ্ট                            | ব্যাখ্যা                                           |
| ---------------------------------- | -------------------------------------------------- |
| `platform`                         | iOS বা Android প্ল্যাটফর্ম নির্ধারণ করে।           |
| `lane`                             | নির্দিষ্ট টাস্কের গ্রুপ।                           |
| `action`                           | কাজ করার কমান্ড (যেমন `gradle`, `gym`, `deliver`)। |
| `parameters`                       | lane-এ ইনপুট পাস করা যায়।                          |
| `before_all`, `after_all`, `error` | হুক ব্যবহারের জন্য (lane শুরু বা শেষে)।            |

---

## 🧪 Default Lane:

```ruby
lane :default do
  puts "This is the default lane"
end
```

যদি আপনি কোনো lane নির্দিষ্ট না করেন:

```bash
fastlane
```

তাহলে `default` lane চালু হবে।

---

## 🔚 উপসংহার

Fastlane DSL-এর `lane` হচ্ছে সমস্ত কাজের কেন্দ্রবিন্দু। আপনি lane-এর মাধ্যমে:

* অ্যাপ বিল্ড করতে পারবেন
* টেস্ট চালাতে পারবেন
* স্টোরে আপলোড করতে পারবেন
* অন্য lane বা কাজ কল করতে পারবেন

