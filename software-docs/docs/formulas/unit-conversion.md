# Fuel Unit Conversion

## 1. အကြောင်းအရာအကျဉ်း (Overview)

Fuel Management System တွင် ဆီပမာဏများကို အခြေခံအားဖြင့် **Liter** ဖြင့် သိမ်းဆည်းသည်။

အသုံးပြုသူက UI တွင် Gallon ဖြင့် ကြည့်ရှုလိုပါက သို့မဟုတ် Gallon ဖြင့် ပမာဏထည့်သွင်းပါက Liter နှင့် Gallon ကြား ပြောင်းလဲတွက်ချက်ပေးသည်။

System တွင် **Imperial Gallon** အချိုးကို အသုံးပြုထားသည်။

```text
1 Gallon = 4.546 Liters
```

## 2. Conversion Formula

Liter နှင့် Gallon ကြား ပြောင်းလဲရာတွင် အောက်ပါ Formula နှစ်ခုကို အသုံးပြုသည်။

### 2.1 Liter မှ Gallon သို့ ပြောင်းခြင်း

Liter ပမာဏကို `4.546` ဖြင့် စားခြင်းဖြင့် Gallon ရရှိသည်။

```text
Gallons = Liters / 4.546
```

**ဥပမာ**

ဆီပမာဏ `100 Liters` ရှိပါက -

```text
Gallons = 100 / 4.546
        ≈ 22.0 Gallons
```

### 2.2 Gallon မှ Liter သို့ ပြောင်းခြင်း

Gallon ပမာဏကို `4.546` ဖြင့် မြှောက်ခြင်းဖြင့် Liter ရရှိသည်။

```text
Liters = Gallons × 4.546
```

**ဥပမာ**

ဆီပမာဏ `10 Gallons` ရှိပါက -

```text
Liters = 10 × 4.546
       = 45.46 Liters
```

## 3. Backend Conversion Logic

Backend ရှိ Conversion Helper သည် ပမာဏကို Unit ပြောင်းလဲခြင်းမပြုမီ Input တန်ဖိုးကို ဒဿမ ၃ နေရာအထိ အရင် Round လုပ်သည်။

ထို့နောက် သက်ဆိုင်ရာ Conversion Formula ကို အသုံးပြု၍ ပြောင်းလဲတွက်ချက်သည်။

ဥပမာ -

Input တန်ဖိုးသည် `10.12356` ဖြစ်ပါက Conversion မလုပ်မီ -

```text
Rounded Input = 10.124
```

ထိုတန်ဖိုးကို အသုံးပြုပြီး Unit Conversion ဆက်လက်လုပ်ဆောင်သည်။

## 4. Report နှင့် UI တွင် ဖော်ပြပုံ

Report များတွင် အသုံးပြုသူရွေးချယ်ထားသော Unit အလိုက် ဆီပမာဏကို ဖော်ပြပေးသည်။

- **Liter Mode** – သိမ်းဆည်းထားသော Liter တန်ဖိုးကို ဖော်ပြသည်။
- **Gallon Mode** – Liter တန်ဖိုးကို `4.546` ဖြင့် စား၍ Gallon အဖြစ် ဖော်ပြသည်။
- Gallon ဖြင့် ဖော်ပြသော Report တန်ဖိုးများကို အများအားဖြင့် ဒဿမ ၃ နေရာအထိ ပြသသည်။

## 5. အရေးကြီးသော မှတ်ချက်များ (Important Notes)

- System ၏ အခြေခံသိုလှောင်ယူနစ်သည် Liter ဖြစ်သည်။
- Gallon Conversion အတွက် Imperial Gallon အချိုး `1 Gallon = 4.546 Liters` ကို အသုံးပြုသည်။
- Backend Conversion Helper သည် Input ကို အရင်ဆုံး ဒဿမ ၃ နေရာအထိ Round လုပ်သည်။
- Report များတွင် Gallon ပြသရာ၌ Liter တန်ဖိုးကို `4.546` ဖြင့် စားသည်။
- Gallon တန်ဖိုးသည် UI နှင့် Report များတွင် ပြသရန်အတွက်သာ ဖြစ်ပြီး မူရင်းသိမ်းဆည်းထားသော Liter တန်ဖိုးကို အစားမထိုးပါ။

**အဓိကအားဖြင့်** System သည် ဆီပမာဏကို Liter ဖြင့်သာ အခြေခံသိမ်းဆည်းထားပြီး Gallon သည် အသုံးပြုသူကြည့်ရှုရန်နှင့် ထည့်သွင်းရန်အတွက် ပြောင်းလဲအသုံးပြုသော Unit ဖြစ်သည်။