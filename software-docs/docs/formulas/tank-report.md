# Tank Report

## 1. အကြောင်းအရာအကျဉ်း (Overview)

Tank Report သည် သတ်မှတ်ထားသော ရက်စွဲအတွင်း Tank တစ်ခုချင်းစီ၏ ဆီလက်ကျန်၊ ဆီလက်ခံရရှိမှုနှင့် ဆီရောင်းချမှုများကို စုစည်းဖော်ပြပေးသော Report ဖြစ်သည်။

ဤ Report တွင် Tank History၊ Fuel Delivery နှင့် Sale Records များမှ အချက်အလက်များကို ရယူပြီး Tank ၏ လက်ကျန်ဆီပမာဏနှင့် ကွာဟချက်ကို တွက်ချက်သည်။

တွက်ချက်မှုများတွင် အသုံးပြုသော ဆီပမာဏအားလုံးသည် Liter ဖြစ်သည်။

## 2. Tank Balance တွက်ချက်ပုံ (Balance Calculation)

### 2.1 Opening Balance

Opening Balance ဆိုသည်မှာ ရွေးချယ်ထားသော ရက်စွဲအတွင်း ပထမဆုံး Tank History Detail ၏ `opening_balance` ဖြစ်သည်။

```text
Opening Balance = First Tank History Detail's Opening Balance
```

ဤပမာဏသည် သတ်မှတ်ကာလအတွင်း Tank ၏ စတင်လက်ကျန်ဆီပမာဏကို ဖော်ပြသည်။

### 2.2 Received

Received ဆိုသည်မှာ သတ်မှတ်ထားသော ရက်စွဲအတွင်း Tank သို့ လက်ခံရရှိခဲ့သော ဆီပမာဏအားလုံး၏ စုစုပေါင်းဖြစ်သည်။

```text
Received = Sum of receive_balance
```

Fuel Delivery Records များရှိ `receive_balance` ကို အသုံးပြု၍ တွက်ချက်သည်။

### 2.3 Sales

Sales ဆိုသည်မှာ သတ်မှတ်ကာလအတွင်း Tank နှင့် သက်ဆိုင်သော အရောင်းမှတ်တမ်းများ၏ `sale_liter` စုစုပေါင်းဖြစ်သည်။

```text
Sales = Sum of sale_liter
```

Code တွင် Tank အတွက် Sale Liters အားလုံးကို Sales အဖြစ် အသုံးပြုထားသည်။

### 2.4 Expected Closing

Expected Closing ဆိုသည်မှာ ဆီလက်ခံရရှိမှုနှင့် ဆီအသုံးပြုမှုများကို ထည့်သွင်းတွက်ချက်ပြီးနောက် Tank ထဲတွင် ကျန်ရှိသင့်သော ဆီပမာဏဖြစ်သည်။

```text
Expected Closing = Opening + Received
                 - Sales - Pump Tests - Other Usage
```

ဤနေရာတွင် -

- **Opening** – စတင်လက်ကျန်ဆီပမာဏ။
- **Received** – လက်ခံရရှိသော ဆီပမာဏ။
- **Sales** – ရောင်းချခဲ့သော ဆီပမာဏ။
- **Pump Tests** – Pump စမ်းသပ်ရာတွင် အသုံးပြုခဲ့သော ဆီပမာဏ။
- **Other Usage** – အခြားအသုံးပြုမှုအဖြစ် သတ်မှတ်ထားသော ဆီပမာဏ။

### ဥပမာ

အောက်ပါအချက်အလက်များရှိသည်ဟု ယူဆပါက -

| အချက်အလက် | ပမာဏ |
|---|---:|
| Opening | 5,000 L |
| Received | 10,000 L |
| Sales | 3,000 L |
| Pump Tests | 100 L |
| Other Usage | 50 L |

Expected Closing ကို တွက်ချက်ပါက -

```text
Expected Closing = 5,000 + 10,000
                 - 3,000 - 100 - 50

                 = 11,850 L
```

ထို့ကြောင့် စာရင်းတွက်ချက်မှုအရ Tank ထဲတွင် `11,850 L` ကျန်ရှိသင့်သည်။

## 3. Actual Closing

Actual Closing ဆိုသည်မှာ ရွေးချယ်ထားသော ရက်စွဲအတွင်း နောက်ဆုံး Tank History Detail ၏ `closing_balance` ဖြစ်သည်။

```text
Actual Closing = Last Tank History Detail's Closing Balance
```

ဤတန်ဖိုးသည် Tank History တွင် မှတ်တမ်းတင်ထားသော အမှန်တကယ် လက်ကျန်ပမာဏကို ကိုယ်စားပြုသည်။

## 4. Different Balance တွက်ချက်ပုံ

Different Balance သည် Opening Balance နှင့် Received ပမာဏတို့ကို ပေါင်းပြီး Actual Closing ကို နုတ်ထားသော တန်ဖိုးဖြစ်သည်။

```text
Different Balance = Opening + Received - Actual Closing
```

ဤတန်ဖိုးသည် သတ်မှတ်ကာလအတွင်း လက်ခံရရှိသော ဆီပမာဏနှင့် Actual Closing တို့ကို နှိုင်းယှဉ်ဖော်ပြခြင်းဖြစ်သည်။

## 5. Gain / Loss တွက်ချက်ပုံ

Gain/Loss သည် Expected Closing နှင့် Actual Closing ကြားရှိ ကွာခြားချက်ကို ဖော်ပြသည်။

```text
Gain/Loss = Actual Closing - Opening - Received
          + Sales + Pump Tests + Other Usage
```

အဓိပ္ပာယ်မှာ -

- **Positive Value (+)** – စာရင်းတွက်ချက်မှုအရ မျှော်မှန်းထားသော လက်ကျန်ထက် ပိုနေခြင်း။
- **Negative Value (-)** – စာရင်းတွက်ချက်မှုအရ မျှော်မှန်းထားသော လက်ကျန်ထက် လျော့နည်းနေခြင်း။
- **Zero (0)** – Expected Closing နှင့် Actual Closing တူညီခြင်း။

### ဥပမာ

အထက်ပါ ဥပမာအတိုင်း Expected Closing သည် `11,850 L` ဖြစ်ပြီး Actual Closing သည် `11,800 L` ဖြစ်ပါက -

```text
Gain/Loss = 11,800 - 5,000 - 10,000
          + 3,000 + 100 + 50

          = -50 L
```
            
ထို့ကြောင့် စာရင်းတွက်ချက်မှုအရ Tank တွင် `50 L` လျော့နည်းနေသည်။

## 6. Pump Tests နှင့် Other Usage သတ်မှတ်ပုံ

Code တွင် Sale Records များကို အမျိုးအစားခွဲရာ၌ အောက်ပါ ID များကို အသုံးပြုထားသည်။

| အမျိုးအစား | သတ်မှတ်ချက် |
|---|---|
| Pump Tests | Vehicle Type ID `28` နှင့် ကိုက်ညီသော Sale Liters |
| Other Usage | Payment ID `10` နှင့် ကိုက်ညီသော Sale Liters |
| Sales | Tank အတွက် Sale Liters အားလုံး |

**သတိပြုရန်**

Sales တွင် Sale Liters အားလုံး ပါဝင်နေပြီး Pump Tests နှင့် Other Usage ကိုလည်း သီးခြားခွဲထုတ်ထားသည်။

ထို့ကြောင့် Pump Tests သို့မဟုတ် Other Usage သည် Sales ထဲတွင် ပါဝင်နေပါက Expected Closing တွက်ချက်ရာတွင် နှစ်ကြိမ်နုတ်မိနိုင်သည်။

အမျိုးအစားများ ထပ်တူပါဝင်ခြင်းရှိ၊ မရှိကို Station Configuration နှင့် Sale Records များအပေါ် မူတည်၍ စစ်ဆေးရန် လိုအပ်သည်။

## 7. Unit Conversion

Tank Report တွင် ဆီပမာဏများကို Liter ဖြင့် အခြေခံတွက်ချက်ထားသည်။

Frontend တွင် Gallon ကို ရွေးချယ်ထားပါက Liter ပမာဏကို `4.546` ဖြင့် စား၍ Gallon အဖြစ် ပြောင်းလဲဖော်ပြသည်။

```text
Gallons = Liters / 4.546
```

ဥပမာ -

```text
10,000 Liters / 4.546
≈ 2,199.736 Gallons
```

ဤပြောင်းလဲမှုသည် UI တွင် ဖော်ပြရန်အတွက်သာဖြစ်ပြီး မူရင်း Liter တန်ဖိုးကို ပြောင်းလဲခြင်းမဟုတ်ပါ။

## 8. အရေးကြီးသော မှတ်ချက်များ (Important Notes)
    
- Opening Balance ကို ကာလအတွင်း ပထမဆုံး Tank History Detail မှ ရယူသည်။
- Actual Closing ကို ကာလအတွင်း နောက်ဆုံး Tank History Detail မှ ရယူသည်။
- Received ကို Fuel Delivery Records ရှိ `receive_balance` များမှ တွက်ချက်သည်။
- Sales ကို Tank အတွက် Sale Records ရှိ `sale_liter` များမှ တွက်ချက်သည်။
- Pump Tests နှင့် Other Usage ကို သတ်မှတ်ထားသော ID များအလိုက် ခွဲခြားသည်။
- Expected Closing နှင့် Actual Closing တူညီပါက Gain/Loss သည် `0` ဖြစ်သည်။
- Positive Gain/Loss သည် လက်ကျန်ပိုနေခြင်း၊ Negative Gain/Loss သည် လက်ကျန်လျော့နည်းနေခြင်းကို ဖော်ပြသည်။
- Gallon ဖြင့် ပြသပါက Liter ကို `4.546` ဖြင့် စား၍ ပြောင်းလဲဖော်ပြသည်။