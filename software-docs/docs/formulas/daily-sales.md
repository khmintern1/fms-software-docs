# Daily Sales Report

## 1. အကြောင်းအရာအကျဉ်း (Overview)

Daily Sales Report ဆိုသည်မှာ သတ်မှတ်ထားသော ရက်တစ်ရက်အတွင်း ဆီဆိုင်တွင် ဆီဘယ်လောက်လက်ခံရရှိခဲ့သည်၊ ဘယ်လောက်ရောင်းချခဲ့သည်နှင့် နောက်ဆုံးဆီဘယ်လောက်ကျန်ရှိသည်ကို ဖော်ပြပေးသော Report ဖြစ်သည်။

ဤ Report ကို Tank နှင့် Fuel Type တစ်ခုချင်းစီအလိုက် ခွဲခြားကြည့်ရှုနိုင်သည်။

Report တွင် အဓိကအားဖြင့် အောက်ပါအချက်များကို ဖော်ပြသည်။

| အချက်အလက် | ရှင်းလင်းချက် |
|---|---|
| Opening Balance | သတ်မှတ်ရက်၏ အစတွင် Tank ထဲ၌ ရှိနေသော ဆီလက်ကျန်ပမာဏ။ |
| Received | ထိုနေ့အတွင်း ဆီထပ်မံလက်ခံရရှိသော ပမာဏ။ |
| Sales | ထိုနေ့အတွင်း ရောင်းချခဲ့သော ဆီပမာဏ။ |
| Adjustment | Tank အတွင်း ဆီပမာဏကို အတိုးအလျှော့ ပြုလုပ်ထားသော ပမာဏ။ |
| Closing Balance | ထိုနေ့၏ နောက်ဆုံးဆီလက်ကျန်ပမာဏ။ |

ဆီပမာဏအားလုံးကို Liter ဖြင့် တွက်ချက်ထားသည်။ Report တွင် ပြသမည့် ရလဒ်များသည် ရွေးချယ်ထားသော ရက်စွဲအတွင်းရှိ Sale Records နှင့် Tank History Records များအပေါ် မူတည်သည်။

## 2. Report တွင် အသုံးပြုသော Data များ

Daily Sales Report တွင် အချက်အလက်များကို သက်ဆိုင်ရာ Records များမှ ရယူထားသည်။

| အချက်အလက် | Data ရယူသည့်နေရာ | ရယူပုံ |
|---|---|---|
| Closing Balance | Tank History | နောက်ဆုံးမှတ်တမ်း၏ Closing Balance ကို ယူသည်။ |
| Received | Fuel Delivery | သတ်မှတ်ရက်အတွင်း လက်ခံရရှိသော ဆီပမာဏများကို ပေါင်းသည်။ |
| Sales | Sale Records | `sale_liter` တန်ဖိုးများကို ပေါင်းသည်။ |
| Adjustment | Tank History | `tank_adjust_balance` တန်ဖိုးများကို ပေါင်းသည်။ |

ဤနေရာတွင် Tank History သည် Tank အတွင်းရှိ ဆီလက်ကျန်နှင့် ပြောင်းလဲမှုများကို မှတ်တမ်းတင်ထားသော Data ဖြစ်သည်။

Fuel Delivery သည် ဆီဆိုင်သို့ ဆီပို့ဆောင်လက်ခံထားသော မှတ်တမ်းဖြစ်ပြီး Sale Records သည် Customer များထံ ရောင်းချထားသော ဆီစာရင်းဖြစ်သည်။

## 3. Cloud Daily Report တွင် Opening နှင့် Closing Balance ရယူပုံ

Cloud Daily Report တွင် Opening Balance နှင့် Closing Balance ကို Tank History Records များမှ ရယူသည်။

- **Opening Balance (`opened_liter`)** – ရွေးချယ်ထားသော ကာလအတွင်း ပထမဆုံး Tank History Record ၏ Opening Balance ကို အသုံးပြုသည်။
- **Closing Balance (`closed_liter`)** – ရွေးချယ်ထားသော ကာလအတွင်း နောက်ဆုံး Tank History Record ၏ Closing Balance ကို အသုံးပြုသည်။
- **Adjusted Amount (`adjusted_liter`)** – သက်ဆိုင်ရာ Tank History Records များရှိ `tank_adjust_balance` တန်ဖိုးများကို ပေါင်း၍ ရယူသည်။

**သတိပြုရန်**

Adjusted Amount ကို Sales နှင့် Fuel Delivery ပမာဏများမှ တွက်ချက်ထားခြင်းမဟုတ်ပါ။ Tank History Records ထဲတွင် သိမ်းဆည်းထားသော Adjustment ပမာဏများကိုသာ ပေါင်းထားခြင်းဖြစ်သည်။

## 4. သီးခြားဖော်ပြသော အရောင်းအမျိုးအစားများ

Cloud Daily Report တွင် ပုံမှန်ဆီရောင်းချမှုအပြင် အချို့သော အရောင်းအမျိုးအစားများကိုလည်း သီးခြားခွဲခြားဖော်ပြထားသည်။

| အမျိုးအစား | ခွဲခြားသတ်မှတ်ပုံ |
|---|---|
| Pump Test | Vehicle Type ID သည် `28` ဖြစ်သော Sale Records များ။ |
| Other | Payment ID သည် `10` ဖြစ်သော Sale Records များ။ |

**Pump Test** ဆိုသည်မှာ Dispenser သို့မဟုတ် Pump ကို စမ်းသပ်ရာတွင် အသုံးပြုသော ဆီပမာဏဖြစ်သည်။

**Other** ဆိုသည်မှာ Payment ID `10` ဖြင့် သတ်မှတ်ထားသော အခြားအရောင်းအမျိုးအစားဖြစ်သည်။

ဤအမျိုးအစားများကို ပုံမှန် Sales Total နှင့် သီးခြားခွဲ၍ ဖော်ပြနိုင်သည်။

## 5. Grand Total နှင့် Unit Conversion

### 5.1 Grand Total

Grand Total ဆိုသည်မှာ Fuel Type သို့မဟုတ် Tank တစ်ခုချင်းစီ၏ Subtotal များကို ပေါင်းထားသော စုစုပေါင်းတန်ဖိုးဖြစ်သည်။

ဥပမာ -

| Fuel Type | Sales |
|---|---:|
| Diesel | 1,000 L |
| Petrol 92 | 500 L |
| Petrol 95 | 300 L |
| **Grand Total** | **1,800 L** |

Grand Total တွက်ချက်ရာတွင် Liter နှင့် Gallon ကဲ့သို့ ယူနစ်မတူသော ပမာဏများကို ရောနှောပေါင်းခြင်း မပြုလုပ်ပါ။

### 5.2 Gallon Conversion

Report တွင် Gallon ဖြင့် ကြည့်ရှုလိုပါက Liter ပမာဏကို `4.546` ဖြင့် စား၍ ပြောင်းလဲဖော်ပြသည်။

```text
Gallons = Liters / 4.546
```

ဥပမာ -

ဆီပမာဏ `100 Liters` ရှိပါက -

```text
Gallons = 100 / 4.546
        ≈ 21.998 Gallons
```

ထို့ကြောင့် `100 Liters` သည် `21.998 Gallons` ခန့်နှင့် ညီမျှသည်။

## 6. အရေးကြီးသော မှတ်ချက်များ (Important Notes)

- Report တွင် ရလဒ်များကို ရွေးချယ်ထားသော ရက်စွဲအလိုက် ပြသသည်။
- Opening နှင့် Closing Balance များကို Cloud Report တွင် Tank History Records များမှ ရယူသည်။
- Tank History Records မရှိပါက Opening သို့မဟုတ် Closing Balance ကို `0` အဖြစ် ပြသနိုင်သည်။
- Adjustment ပမာဏကို `tank_adjust_balance` မှ ရယူသည်။
- Pump Test နှင့် Other အမျိုးအစားများကို သတ်မှတ်ထားသော ID များအပေါ် မူတည်၍ ခွဲခြားသည်။
- Grand Total ကို သက်ဆိုင်ရာ Fuel Type သို့မဟုတ် Tank Subtotal များမှ တွက်ချက်သည်။
- Gallon ဖြင့် ပြသသော်လည်း မူရင်းဆီပမာဏကို Liter ဖြင့်သာ အခြေခံထားသည်။