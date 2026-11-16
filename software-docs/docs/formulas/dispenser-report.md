# Dispenser Report

## 1. အကြောင်းအရာအကျဉ်း (Overview)

Dispenser Report ဆိုသည်မှာ သတ်မှတ်ထားသော ရက်စွဲအတွင်း ဆီပန့်တစ်ခုချင်းစီနှင့် Nozzle တစ်ခုချင်းစီမှ ရောင်းချခဲ့သော ဆီပမာဏများကို စုစည်းဖော်ပြပေးသော Report ဖြစ်သည်။

ဤ Report တွင် ဆီပန့်၏ စတင်ဖတ်ရှုချိန်နှင့် နောက်ဆုံးဖတ်ရှုချိန်ရှိ Totalizer ပမာဏများအပြင် အမှန်တကယ် ရောင်းချထားသော ဆီပမာဏကိုလည်း ဖော်ပြထားသည်။

**Totalizer** ဆိုသည်မှာ Dispenser စက်မှတစ်ဆင့် ဆီထုတ်ပေးခဲ့သော စုစုပေါင်းပမာဏကို မှတ်တမ်းတင်ထားသည့် Meter Reading ဖြစ်သည်။

ဥပမာ - Dispenser ၏ Totalizer သည် ဆီမရောင်းချမီ `10,000 L` ရှိပြီး ရောင်းချပြီးနောက် `10,500 L` ဖြစ်လာပါက ထိုအချိန်အတွင်း ဆီထုတ်ပေးခဲ့သော ပမာဏမှာ `500 L` ဖြစ်သည်။

## 2. Totalizer နှင့် Sales တွက်ချက်ပုံ

Dispenser Report တွင် Totalizer Reading နှင့် Sale Records များကို အသုံးပြု၍ အောက်ပါအချက်များကို တွက်ချက်သည်။

| အချက်အလက် | ရှင်းလင်းချက် |
|---|---|
| Opening Totalizer | ပထမဆုံး Sale မတိုင်မီရှိခဲ့သော Totalizer ပမာဏ။ |
| Closing Totalizer | နောက်ဆုံး Sale ပြုလုပ်ပြီးနောက်ရှိသော Totalizer ပမာဏ။ |
| Totalizer Difference | Opening နှင့် Closing Totalizer ကြားရှိ ကွာခြားချက်။ |
| Reported Sale Liters | သတ်မှတ်ကာလအတွင်း Sale Records များမှ ရောင်းချခဲ့သော ဆီပမာဏ စုစုပေါင်း။ |

**တွက်ချက်ပုံ**

```text
Opening Totalizer = First Sale Device Totalizer
                  - First Sale Liters

Closing Totalizer = Last Sale Device Totalizer

Totalizer Difference = Closing Totalizer
                     - Opening Totalizer

Reported Sale Liters = Sum of Sale Liters
```

ဥပမာ -

Dispenser တစ်ခုတွင် အောက်ပါ Reading များရှိသည်ဟု ယူဆပါက -

| အချက်အလက် | ပမာဏ |
|---|---:|
| Opening Totalizer | 10,000 L |
| Closing Totalizer | 10,500 L |
| Totalizer Difference | 500 L |
| Reported Sale Liters | 500 L |

ထို့ကြောင့် Totalizer အရ ဆီ `500 L` ထုတ်ပေးထားပြီး Sale Records များတွင်လည်း `500 L` ရောင်းချထားကြောင်း တွေ့ရသည်။

## 3. Local နှင့် Cloud Report တွင် တွက်ချက်ပုံ

Dispenser Report တွင် Local နှင့် Cloud အလိုက် အသုံးပြုသော Data နှင့် ဖော်ပြပုံ အနည်းငယ်ကွာခြားသည်။

### 3.1 Local Sale Summary

Local Sale Summary တွင် `device_totalizer_liter` တန်ဖိုးသည် `0` မဟုတ်သော Sale Records များကိုသာ အသုံးပြုသည်။

ထို့နောက် Sale Records များကို ရက်စွဲအလိုက် စီစဉ်ပြီး -

- ပထမဆုံး Sale Record မှ Opening Totalizer ကို တွက်ချက်သည်။
- နောက်ဆုံး Sale Record မှ Closing Totalizer ကို ရယူသည်။
- Opening နှင့် Closing Totalizer တို့၏ ကွာခြားချက်ကို တွက်ချက်သည်။

**သတိပြုရန် -** ပထမဆုံး Sale Record မတိုင်မီရှိခဲ့သော ယခင် Totalizer Reading ကို မရရှိနိုင်ပါက Opening Totalizer သည် `0` ဖြစ်နိုင်သည်။

### 3.2 Cloud Pump Report

Cloud Pump Report တွင် Dispenser နှင့် Nozzle တစ်ခုချင်းစီအလိုက် Sale Records များကို စုစည်းပြီး အောက်ပါအချက်များကို Subtotal အဖြစ် ဖော်ပြသည်။

| အချက်အလက် | ရှင်းလင်းချက် |
|---|---|
| Difference Totalizer (`difference_totalizer_liter`) | Totalizer Reading များ၏ ကွာခြားချက်။ |
| Sale Liters (`sale_liter`) | ပုံမှန်ရောင်းချမှုမှ ရရှိသော ဆီပမာဏ။ |
| Pump Test (`pump_test`) | Pump စမ်းသပ်ရာတွင် အသုံးပြုသော ဆီပမာဏ။ |
| Other (`other`) | Other အမျိုးအစားအဖြစ် သတ်မှတ်ထားသော Sale ပမာဏ။ |

ဤတန်ဖိုးများကို Dispenser နှင့် Nozzle အလိုက် စုစည်းဖော်ပြထားသောကြောင့် မည်သည့် Pump နှင့် Nozzle မှ ဆီဘယ်လောက်ထုတ်ပေးခဲ့သည်ကို စစ်ဆေးနိုင်သည်။

## 4. Totalizer နှင့် Sale Records ကိုက်ညီမှု စစ်ဆေးခြင်း (Reconciliation Check)

Dispenser မှ ထုတ်ပေးခဲ့သော ဆီပမာဏနှင့် System ထဲတွင် ရောင်းချမှုအဖြစ် မှတ်တမ်းတင်ထားသော ဆီပမာဏတို့ ကိုက်ညီမှုရှိ၊ မရှိ စစ်ဆေးရန် လိုအပ်သည်။

ဤအတွက် Totalizer Difference နှင့် Reported Sale Liters တို့ကို နှိုင်းယှဉ်သည်။

**တွက်ချက်ပုံ**

```text
Reconciliation Difference = Totalizer Difference
                           - Reported Sale Liters
```

ဥပမာ -

| အချက်အလက် | ပမာဏ |
|---|---:|
| Totalizer Difference | 500 L |
| Reported Sale Liters | 500 L |
| Reconciliation Difference | 0 L |

Reconciliation Difference သည် `0` ဖြစ်ပါက Totalizer မှရရှိသော ဆီပမာဏနှင့် Sale Records မှရရှိသော ဆီပမာဏတို့ ကိုက်ညီနေသည်။

`0` မဟုတ်ပါက အောက်ပါအချက်များကို စစ်ဆေးရန် လိုအပ်သည်။

1. **Date Range** – ရွေးချယ်ထားသော ရက်စွဲအပိုင်းအခြား မှန်ကန်မှုရှိ၊ မရှိ။
2. **Dispenser / Nozzle Mapping** – Sale Records များနှင့် Dispenser၊ Nozzle ချိတ်ဆက်မှု မှန်ကန်မှုရှိ၊ မရှိ။
3. **Missing / Reset Totalizer** – Totalizer Reading ပျောက်ဆုံးခြင်း သို့မဟုတ် Meter Reset ဖြစ်ခြင်း ရှိ၊ မရှိ။
4. **Voided / Modified Sales** – ပယ်ဖျက်ထားသော သို့မဟုတ် ပြင်ဆင်ထားသော Sale Records များ ရှိ၊ မရှိ။
5. **Pump Test / Other Categories** – Pump Test နှင့် Other ပမာဏများကို Totalizer နှင့် Sale စာရင်း နှိုင်းယှဉ်ရာတွင် မှန်ကန်စွာ ထည့်သွင်းထားခြင်း ရှိ၊ မရှိ။

## 5. Unit Conversion

Dispenser Report တွင် ဆီပမာဏများကို အခြေခံအားဖြင့် Liter ဖြင့် တွက်ချက်ထားသည်။

UI တွင် Gallon Mode ကို ရွေးချယ်ထားပါက Liter ပမာဏများကို Gallon သို့ ပြောင်းလဲဖော်ပြပေးသည်။

```text
Gallons = Liters / 4.546
```

ဥပမာ -

`100 Liters` ကို Gallon သို့ ပြောင်းလဲပါက -

```text
Gallons = 100 / 4.546
        ≈ 21.998 Gallons
```

ဤပြောင်းလဲမှုသည် Report တွင် ပြသရန်အတွက်သာဖြစ်ပြီး Database ထဲရှိ မူရင်း Liter တန်ဖိုးကို ပြောင်းလဲခြင်းမဟုတ်ပါ။

## 6. အရေးကြီးသော မှတ်ချက်များ (Important Notes)

- Dispenser Report သည် Dispenser နှင့် Nozzle အလိုက် ရောင်းချမှုများကို စုစည်းဖော်ပြသည်။
- Totalizer သည် Dispenser မှ ထုတ်ပေးခဲ့သော စုစုပေါင်းဆီပမာဏကို ဖော်ပြသည်။
- Sale Liters သည် System ထဲတွင် ရောင်းချမှုအဖြစ် မှတ်တမ်းတင်ထားသော ဆီပမာဏဖြစ်သည်။
- Local Sale Summary တွင် `device_totalizer_liter` သုညမဟုတ်သော Records များကိုသာ အသုံးပြုသည်။
- Cloud Pump Report တွင် Sale၊ Pump Test နှင့် Other ပမာဏများကို သီးခြားဖော်ပြနိုင်သည်။
- Totalizer Reading နှင့် Sale Records များ မကိုက်ညီပါက သက်ဆိုင်ရာ Records များကို ပြန်လည်စစ်ဆေးရန် လိုအပ်သည်။
- Gallon Mode ကို ရွေးချယ်ထားပါက Liter ပမာဏကို `4.546` ဖြင့်စား၍ ပြောင်းလဲဖော်ပြသည်။