# Fuel Management Formulas

## 1. အကြောင်းအရာအကျဉ်း (Overview)

ဤစာမျက်နှာတွင် Fuel Management System အတွင်း အသုံးပြုသော အခြေခံတွက်ချက်မှုများ (Calculation Formulas) ကို စုစည်းဖော်ပြထားသည်။

အဓိကအားဖြင့် ဆီရောင်းချမှုပမာဏ၊ အရောင်းတန်ဖိုး၊ Unit Conversion၊ Dispenser Totalizer၊ Tank Balance၊ Inventory Gain/Loss၊ Fuel Delivery နှင့် Daily Sales တို့၏ တွက်ချက်ပုံများ ပါဝင်သည်။

ဆီပမာဏများကို အခြေခံအားဖြင့် Liter ဖြင့် တွက်ချက်ထားသည်။

## 2. Sale Quantity & Preset Formulas

### 2.1 Sale Quantity Calculation

Cashier မှ ကြိုတင်သတ်မှတ်ထားသော ငွေပမာဏ (Preset Amount) နှင့် တစ်လီတာဈေးနှုန်း (Sale Price) ကို အသုံးပြု၍ ရောင်းချမည့် ဆီပမာဏကို တွက်ချက်သည်။

```text
sale_liter = round(presetAmount / salePrice)
```

- `presetAmount` – Cashier မှ ထည့်သွင်းသော သို့မဟုတ် ကြိုတင်သတ်မှတ်ထားသော ငွေပမာဏ။
- `salePrice` – ဆီတစ်လီတာ၏ ရောင်းဈေးနှုန်း။
- `sale_liter` – တွက်ချက်ရရှိသော ဆီပမာဏဖြစ်ပြီး အနီးစပ်ဆုံး Liter အပြည့်သို့ Round လုပ်ထားသည်။

### 2.2 Total Transaction Price

ရောင်းချပြီးသော ဆီပမာဏနှင့် တစ်လီတာဈေးနှုန်းကို မြှောက်၍ စုစုပေါင်းအရောင်းတန်ဖိုးကို တွက်ချက်သည်။

```text
total_price = sale_liter × sale_price
```

## 3. Unit Conversion

System တွင် Liter နှင့် Gallon အကြား ပြောင်းလဲတွက်ချက်ရာ၌ Imperial Gallon Conversion ကို အသုံးပြုထားသည်။

```text
Liter = Gallon × 4.546

Gallon = Liter / 4.546
```

ဥပမာ -

- `10 Gallons = 45.46 Liters`
- `100 Liters ≈ 21.998 Gallons`

## 4. Dispenser & Pump Report Formulas

Dispenser နှင့် Pump Report များတွင် Totalizer Reading နှစ်ခုကြားရှိ ကွာခြားချက်ကို အသုံးပြု၍ ရောင်းချခဲ့သော ဆီပမာဏကို တွက်ချက်သည်။

```text
difference = closing_totalizer - opening_totalizer
```

- `opening_totalizer` – ဆီမရောင်းချမီ စက်၏ Totalizer Reading။
- `closing_totalizer` – ဆီရောင်းချပြီးနောက် စက်၏ Totalizer Reading။
- `difference` – Totalizer Reading နှစ်ခုကြားရှိ ကွာခြားချက်။

## 5. Tank Report Formulas

Tank Report တွင် Tank အတွင်းရှိ ဆီလက်ကျန်ပမာဏ၊ ဆီအဝင်၊ ဆီအထွက်နှင့် Inventory ကွာဟချက်များကို တွက်ချက်သည်။

### 5.1 Calculated Tank Balance (Closing Stock)

သတ်မှတ်ကာလအတွင်း ဆီအဝင်နှင့် ဆီအသုံးပြုမှုများအပေါ် မူတည်၍ မျှော်မှန်းလက်ကျန်ကို တွက်ချက်သည်။

```text
closing = opening + received - sale - pump_test - other
```

- `opening` – စတင်လက်ကျန်ဆီပမာဏ။
- `received` – လက်ခံရရှိသော ဆီပမာဏ။
- `sale` – ရောင်းချသော ဆီပမာဏ။
- `pump_test` – Pump Test အတွက် အသုံးပြုသော ဆီပမာဏ။
- `other` – အခြားအသုံးပြုမှုအတွက် ဆီပမာဏ။

### 5.2 Tank Stock Variance (Difference)

Opening Balance နှင့် Received ပမာဏတို့မှ Actual Closing Balance ကို နုတ်၍ လက်ကျန်ကွာဟချက်ကို တွက်ချက်သည်။

```text
difference = opening + received - closing
```

### 5.3 Inventory Gain / Loss

Tank ၏ အမှန်တကယ်လက်ကျန်နှင့် ဆီအဝင်၊ အထွက်မှတ်တမ်းများကို နှိုင်းယှဉ်၍ Gain/Loss ကို တွက်ချက်သည်။

```text
gain/loss = closing - opening - received
          + sale + pump_test + other
```

- **Positive Value (+)** – စာရင်းတွက်ချက်မှုအရ ဆီပိုနေခြင်း (Gain)။
- **Negative Value (-)** – စာရင်းတွက်ချက်မှုအရ ဆီလျော့နေခြင်း (Loss)။
- **Zero (0)** – စာရင်းတွက်ချက်မှုအရ လက်ကျန်ကိုက်ညီခြင်း။

### 5.4 Operational Inventory Adjustment

Tank အတွင်းရှိ လက်ကျန်ပမာဏ ပြောင်းလဲမှုကို အခြေခံ၍ Inventory Adjustment ကို တွက်ချက်သည်။

```text
adjustment = closing - opening - received + sale
```

## 6. Fuel In Report Formulas

Fuel In Report တွင် ဆီပို့ဆောင်သူဘက်မှ ပေးပို့သောပမာဏနှင့် ဆီဆိုင်ဘက်မှ လက်ခံရရှိသောပမာဏတို့၏ ကွာခြားချက်ကို တွက်ချက်သည်။

### 6.1 Delivery Gain / Loss

```text
gain/loss = receive_balance - send_balance
```

- `receive_balance` – ဆီဆိုင်ဘက်မှ အမှန်တကယ် လက်ခံရရှိသော ဆီပမာဏ။
- `send_balance` – ဆီပို့ဆောင်သူဘက်မှ ပေးပို့သော ဆီပမာဏ။

ရလဒ်သည် Positive ဖြစ်ပါက ဆီပိုလက်ခံရရှိခြင်း၊ Negative ဖြစ်ပါက ဆီလျော့နည်းလက်ခံရရှိခြင်းကို ဖော်ပြသည်။

## 7. Daily Sales Report Formulas

Daily Sales Report တွင် သတ်မှတ်ထားသော ရက်စွဲအတွင်း Tank ၏ ဆီအဝင်၊ အထွက်နှင့် လက်ကျန်ပမာဏများကို တွက်ချက်သည်။

### 7.1 Closing Balance

```text
closing = opening + received - sale - pump_test - other
```

သတ်မှတ်ကာလ၏ Opening Balance နှင့် Received ပမာဏတို့ကို ပေါင်းပြီး Sales၊ Pump Test နှင့် Other Usage ပမာဏများကို နုတ်၍ Closing Balance ကို တွက်ချက်သည်။

### 7.2 Gain / Loss

```text
gain/loss = closing - opening - received
          + sale + pump_test + other
```

ဤ Formula ကို အသုံးပြု၍ သတ်မှတ်ကာလအတွင်း ဆီလက်ကျန်၏ Gain/Loss ကို တွက်ချက်နိုင်သည်။

## 8. အရေးကြီးသော မှတ်ချက်များ (Important Notes)

- System ၏ အခြေခံဆီသိုလှောင်ယူနစ်မှာ Liter ဖြစ်သည်။
- Gallon သည် UI နှင့် Report များတွင် ပြသရန်အတွက် အသုံးပြုသော ယူနစ်ဖြစ်သည်။
- Preset Amount မှ တွက်ချက်ရရှိသော `sale_liter` ကို အနီးစပ်ဆုံး Liter အပြည့်သို့ Round လုပ်ထားသည်။
- Totalizer Reading များကို အသုံးပြု၍ အမှန်တကယ်ရောင်းချသော ဆီပမာဏကို စစ်ဆေးနိုင်သည်။
- Tank Balance နှင့် Inventory Gain/Loss တွက်ချက်မှုများသည် အသုံးပြုသော Opening၊ Received၊ Sales နှင့် Closing Records များအပေါ် မူတည်သည်။
- Fuel Delivery Gain/Loss သည် ပေးပို့သည့်ပမာဏနှင့် လက်ခံရရှိသည့်ပမာဏကြား ကွာခြားချက်ကို ဖော်ပြသည်။
- Report တစ်ခုချင်းစီ၏ တွက်ချက်မှုသည် သက်ဆိုင်ရာ API နှင့် Business Logic အပေါ် မူတည်နိုင်သည်။

