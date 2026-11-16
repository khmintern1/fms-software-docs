# Fuel Sales Formulas

## 1. အကြောင်းအရာအကျဉ်း (Overview)

Fuel Sales Formulas သည် ဆီရောင်းချမှုတစ်ခုတွင် အသုံးပြုသော အရောင်းပမာဏ၊ အရောင်းတန်ဖိုးနှင့် Discount တွက်ချက်ပုံများကို ရှင်းပြထားသော စာမျက်နှာဖြစ်သည်။

System တွင် အရောင်းပမာဏအားလုံးကို အခြေခံအားဖြင့် Liter ဖြင့် သိမ်းဆည်းသည်။

## 2. အရောင်းပမာဏ တွက်ချက်ပုံ (Sale Volume Calculation)

Dispenser မှရရှိသော Totalizer Reading ကို အသုံးပြု၍ ရောင်းချခဲ့သော ဆီပမာဏကို တွက်ချက်သည်။

```text
Sale Liters = Closing Totalizer - Opening Totalizer
```

- **Opening Totalizer** – အရောင်းမစတင်မီ Dispenser ၏ စုစုပေါင်း Meter Reading။
- **Closing Totalizer** – အရောင်းပြီးဆုံးချိန် Dispenser ၏ စုစုပေါင်း Meter Reading။
- **Sale Liters** – ထိုအရောင်းအတွက် အသုံးပြုခဲ့သော ဆီပမာဏ။

**ဥပမာ**

Opening Totalizer သည် `15,000 L` ဖြစ်ပြီး Closing Totalizer သည် `15,025 L` ဖြစ်ပါက -

```text
Sale Liters = 15,025 - 15,000
            = 25 L
```

ထို့ကြောင့် ဆီ `25 L` ရောင်းချခဲ့သည်ဟု တွက်ချက်နိုင်သည်။

**သတိပြုရန်**

Totalizer သည် အရောင်းတစ်ခုချင်းစီ၏ ပမာဏကို တိုက်ရိုက်ပြသခြင်းမဟုတ်ဘဲ စုစုပေါင်း Meter Reading ကို ပြသခြင်းဖြစ်သည်။ Meter Reset သို့မဟုတ် Reading ပြန်စတင်ခြင်း ဖြစ်ပေါ်ပါက Opening နှင့် Closing Reading များကို ပြန်လည်စစ်ဆေးရန် လိုအပ်သည်။

## 3. အရောင်းတန်ဖိုး တွက်ချက်ပုံ (Sales Amount Calculation)

အရောင်းတန်ဖိုးကို ရောင်းချခဲ့သော ဆီပမာဏနှင့် တစ်လီတာဈေးနှုန်းကို မြှောက်၍ တွက်ချက်သည်။

### 3.1 Gross Amount

Discount မနုတ်ရသေးသော မူလအရောင်းတန်ဖိုး ဖြစ်သည်။

```text
Gross Amount = Sale Liters × Unit Sale Price
```

**ဥပမာ**

- Sale Liters = `25 L`
- Unit Sale Price = `3,000`

```text
Gross Amount = 25 × 3,000
             = 75,000
```

### 3.2 Fixed Discount

Discount ကို သတ်မှတ်ထားသော ငွေပမာဏအတိုင်း နုတ်ယူခြင်းဖြစ်သည်။

```text
Net Amount = Gross Amount - Discount Amount
```

**ဥပမာ**

- Gross Amount = `75,000`
- Discount Amount = `5,000`

```text
Net Amount = 75,000 - 5,000
           = 70,000
```

### 3.3 Percentage Discount

Discount ကို ရာခိုင်နှုန်းဖြင့် သတ်မှတ်ထားပါက Gross Amount ပေါ်တွင် တွက်ချက်သည်။

```text
Net Amount = Gross Amount × (1 - Discount Percent / 100)
```

**ဥပမာ**

- Gross Amount = `75,000`
- Discount Percent = `10%`

```text
Net Amount = 75,000 × (1 - 10 / 100)
           = 67,500
```

ထို့ကြောင့် Discount `10%` ပေးပြီးနောက် ပေးချေရမည့်ငွေမှာ `67,500` ဖြစ်သည်။

## 4. Voucher တွင် ဖော်ပြသော အရောင်းတန်ဖိုး

Voucher UI တွင် အရောင်းတန်ဖိုးကို ဖော်ပြရာ၌ အောက်ပါ Logic ကို အသုံးပြုသည်။

- `grand_total` ရှိပါက `grand_total` ကို ဖော်ပြသည်။
- `grand_total` မရှိပါက `total_price` ကို ဖော်ပြသည်။

ငွေကြေးတန်ဖိုး၏ Decimal Rounding သည် Payment Process နှင့် UI Formatting ပေါ်မူတည်၍ ကွာခြားနိုင်သည်။

## 5. Preset Amount ဖြင့် အရောင်းပြုလုပ်ခြင်း

Preset Sale ဆိုသည်မှာ Customer က ဆီပမာဏ သို့မဟုတ် ပေးချေမည့်ငွေပမာဏကို ကြိုတင်သတ်မှတ်ပြီး အရောင်းပြုလုပ်ခြင်းဖြစ်သည်။

ပေးချေမည့်ငွေပမာဏကို ကြိုတင်သတ်မှတ်ထားပါက ခန့်မှန်းရောင်းချနိုင်မည့် ဆီပမာဏကို အောက်ပါအတိုင်း တွက်ချက်နိုင်သည်။

```text
Estimated Liters = Preset Amount / Unit Sale Price
```

**ဥပမာ**

- Preset Amount = `30,000`
- Unit Sale Price = `3,000`

```text
Estimated Liters = 30,000 / 3,000
                 = 10 L
```

ဤရလဒ်သည် ခန့်မှန်းဆီပမာဏသာ ဖြစ်သည်။

API လုပ်ဆောင်ချက်အလိုက် တွက်ချက်ထားသော Liter တန်ဖိုးကို Round လုပ်၍ သိမ်းဆည်းခြင်း သို့မဟုတ် Nozzle မှရရှိသော Actual Reading ကို အသုံးပြုခြင်းတို့ ဖြစ်နိုင်သည်။

ထို့ကြောင့် Estimated Liters သည် အမှန်တကယ် ရောင်းချခဲ့သော ဆီပမာဏနှင့် မတူနိုင်ပါ။

**အရေးကြီးချက်**

Actual Sale ပမာဏနှင့် Report များတွင် အသုံးပြုသော အရောင်းပမာဏသည် `sale_liter` ဖြစ်သည်။ Preset Amount မှ တွက်ချက်ထားသော Estimated Liters ကို Actual Sale ပမာဏအဖြစ် တိုက်ရိုက်မယူဆသင့်ပါ။

## 6. အရေးကြီးသော မှတ်ချက်များ (Important Notes)

- System တွင် အရောင်းပမာဏကို Liter ဖြင့် သိမ်းဆည်းသည်။
- Sale Liters ကို Totalizer Reading ကွာခြားချက်မှ တွက်ချက်နိုင်သည်။
- Gross Amount သည် Discount မနုတ်ရသေးသော အရောင်းတန်ဖိုးဖြစ်သည်။
- Fixed Discount သည် သတ်မှတ်ထားသော ငွေပမာဏကို နုတ်ယူခြင်းဖြစ်သည်။
- Percentage Discount သည် Gross Amount ပေါ်တွင် ရာခိုင်နှုန်းအလိုက် တွက်ချက်ခြင်းဖြစ်သည်။
- Preset Amount မှရရှိသော Estimated Liters သည် ခန့်မှန်းတန်ဖိုးသာ ဖြစ်သည်။
- အရောင်းစာရင်းနှင့် Report များတွင် အသုံးပြုသော အရောင်းပမာဏမှာ `sale_liter` ဖြစ်သည်။