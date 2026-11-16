# Fuel Delivery Report

## 1. အကြောင်းအရာအကျဉ်း (Overview)

Fuel Delivery Report ဆိုသည်မှာ ဆီဆိုင်သို့ ပို့ဆောင်လာသော လောင်စာဆီပမာဏနှင့် အမှန်တကယ် လက်ခံရရှိသော ဆီပမာဏများကို ဖော်ပြပေးသော Report ဖြစ်သည်။

ဤ Report တွင် `fuel_ins` မှတ်တမ်းများကို အသုံးပြုပြီး Tank တစ်ခုချင်းစီနှင့် ရက်စွဲအလိုက် ဆီလက်ခံရရှိမှုများကို စုစည်းဖော်ပြထားသည်။

အဓိကအားဖြင့် အောက်ပါအချက်များ ပါဝင်သည်။

| အချက်အလက် | ရှင်းလင်းချက် |
|---|---|
| Sent Volume | ဆီပို့ဆောင်သူဘက်မှ ပေးပို့သော ဆီပမာဏ။ |
| Received Volume | ဆီဆိုင်ဘက်မှ အမှန်တကယ် လက်ခံရရှိသော ဆီပမာဏ။ |
| Delivery Variance | ပို့ပေးသောပမာဏနှင့် လက်ခံရရှိသောပမာဏကြား ကွာခြားချက်။ |
| Calculated Closing | Opening၊ Received၊ Delivery Variance နှင့် Sales တို့ကို အသုံးပြု၍ တွက်ချက်ထားသော နောက်ဆုံးလက်ကျန်။ |
| Different Balance | Opening၊ Received နှင့် Calculated Closing တို့ကြားရှိ ကွာခြားချက်။ |

## 2. ဆီလက်ခံရရှိမှု တွက်ချက်ပုံ (Calculation Logic)

Fuel Delivery Report တွင် ဆီလက်ခံရရှိသောပမာဏကို `fuel_ins` မှတ်တမ်းများရှိ `receive_balance` မှ ရယူသည်။

`receive_balance` တန်ဖိုးမရှိသော Delivery Records များကို Tank Report ၏ Received Total တွင် ထည့်သွင်းတွက်ချက်ခြင်းမပြုပါ။

### 2.1 Received Volume

သတ်မှတ်ထားသော ရက်စွဲအတွင်း လက်ခံရရှိခဲ့သော ဆီပမာဏများကို ပေါင်း၍ တွက်ချက်သည်။

```text
Received Volume = Sum of receive_balance
```

### 2.2 Delivery Variance (Gain / Loss)

ဆီပို့ဆောင်သူဘက်မှ ပေးပို့သောပမာဏနှင့် ဆီဆိုင်ဘက်မှ လက်ခံရရှိသောပမာဏကို နှိုင်းယှဉ်၍ ကွာခြားချက်ကို တွက်ချက်သည်။

```text
Delivery Variance = Received - Sent
```

- Positive Value (+) – ပေးပို့သောပမာဏထက် ပိုမိုလက်ခံရရှိခြင်း။
- Negative Value (-) – ပေးပို့သောပမာဏထက် လျော့နည်းလက်ခံရရှိခြင်း။
- Zero (0) – ပေးပို့သောပမာဏနှင့် လက်ခံရရှိသောပမာဏ တူညီခြင်း။

## 3. Closing Balance တွက်ချက်ပုံ

Report တွင် Opening Balance၊ Received Volume၊ Delivery Variance နှင့် Sales တို့ကို အသုံးပြု၍ Calculated Closing Balance ကို တွက်ချက်သည်။

```text
Calculated Closing = Opening + Received
                   + Delivery Variance - Sales
```

အဓိပ္ပာယ်မှာ -

- Opening – စတင်လက်ကျန်ဆီပမာဏ။
- Received – ဆီဆိုင်မှ လက်ခံရရှိသော ဆီပမာဏ။
- Delivery Variance – ဆီပို့ဆောင်မှုတွင် ဖြစ်ပေါ်သော အပို သို့မဟုတ် အလျော့ပမာဏ။
- Sales – သတ်မှတ်ကာလအတွင်း ရောင်းချခဲ့သော ဆီပမာဏ။

### ဥပမာ

ဆီပို့ဆောင်သူက `10,000 L` ပေးပို့ပြီး ဆီဆိုင်က `9,950 L` သာ လက်ခံရရှိသည်ဟု ယူဆပါက -

**Delivery Variance**

```text
Delivery Variance = 9,950 - 10,000
                  = -50 L
```

ထို့ကြောင့် ဆီပို့ဆောင်မှုတွင် `50 L` လျော့နည်းနေသည်။

**Calculated Closing ဥပမာ**

Opening Balance သည် `5,000 L` ဖြစ်ပြီး Sales သည် `2,000 L` ဖြစ်ပါက -

```text
Calculated Closing = 5,000 + 9,950
                   + (-50) - 2,000

                   = 12,900 L
```

## 4. Different Balance တွက်ချက်ပုံ

Different Balance သည် Opening Balance နှင့် Received Volume တို့ကို ပေါင်းပြီး Calculated Closing Balance ကို နုတ်ထားသော တန်ဖိုးဖြစ်သည်။

```text
Different Balance = Opening + Received
                  - Calculated Closing
```

ဤ Formula ကို အသုံးပြုရာတွင် Calculated Closing ကို ပြန်လည်နှိုင်းယှဉ်တွက်ချက်ထားခြင်းဖြစ်သည်။

**သတိပြုရန် -** Different Balance သည် Actual Tank Closing နှင့် နှိုင်းယှဉ်ထားသော ကွာခြားချက်မဟုတ်ပါ။ Actual Tank Closing ကို Tank History သို့မဟုတ် ATG Records များမှ ရယူနိုင်သည့် Tank Report နှင့် မတူညီပါ။

## 5. အရေးကြီးသော တွက်ချက်မှုဆိုင်ရာ မှတ်ချက်များ (Important Notes)

- Fuel Delivery Records များကို `fuel_ins` မှ ရယူသည်။
- `receive_balance` မရှိသော Records များကို Tank Report ၏ Received Total တွင် ထည့်သွင်းမတွက်ပါ။
- Delivery Variance ကို `receive_balance - send_balance` ဖြင့် တွက်ချက်သည်။
- Calculated Closing တွင် Delivery Variance ကို ထပ်မံပေါင်းထည့်ထားသည်။
- ထို့ကြောင့် Formula ကို ရိုးရှင်းအောင် ပြန်ရေးပါက အောက်ပါအတိုင်း ဖြစ်သည်။

```text
Calculated Closing = Opening + 2 × Received
                   - Sent - Sales
```

ဤအချက်သည် Report ၏ လက်ရှိ Calculation Logic ကို နားလည်ရန် အရေးကြီးသည်။ Delivery Variance ကို ထပ်ပေါင်းထားခြင်းကြောင့် ပုံမှန် Inventory Balance Formula နှင့် မတူညီနိုင်ပါ။

ထို့ကြောင့် Fuel Delivery Report မှ Calculated Closing သည် Tank History သို့မဟုတ် ATG မှ ရရှိသော Actual Closing နှင့် တူညီမည်ဟု မသတ်မှတ်နိုင်ပါ။

## 6. Unit Conversion

Fuel Delivery Report တွင် ဆီပမာဏများကို အခြေခံအားဖြင့် Liter ဖြင့် တွက်ချက်ထားသည်။

UI တွင် Gallon Mode ကို ရွေးချယ်ထားပါက Liter ပမာဏကို Gallon သို့ ပြောင်းလဲဖော်ပြသည်။

```text
Gallons = Liters / 4.546
```

ဥပမာ -

`10,000 Liters` ကို Gallon သို့ ပြောင်းလဲပါက -

```text
Gallons = 10,000 / 4.546
        ≈ 2,199.736 Gallons
```

ဤပြောင်းလဲမှုသည် UI တွင် ဖော်ပြရန်အတွက်သာဖြစ်ပြီး မူရင်း Liter တန်ဖိုးကို ပြောင်းလဲခြင်းမဟုတ်ပါ။