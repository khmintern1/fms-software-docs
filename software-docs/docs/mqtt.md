# MQTT Integration

ဤစာမျက်နှာတွင် Fuel Management System (FMS) အတွင်း MQTT ကို အသုံးပြု၍ Dispenser/Device များ၊ Laravel Backend နှင့် Next.js Frontend တို့အကြား Real-time message များ ဖလှယ်ပုံကို ရှင်းပြထားသည်။

MQTT သည် စက်ပစ္စည်းများမှ ပေးပို့သော အချက်အလက်များကို Backend က လက်ခံဆောင်ရွက်ရန်နှင့် လိုအပ်သည့် command များကို စက်ပစ္စည်းများထံ ပြန်လည်ပေးပို့ရန် အသုံးပြုသည်။ Frontend သည်လည်း MQTT WebSocket မှတစ်ဆင့် message များကို တိုက်ရိုက်လက်ခံပြီး ဆီဖြည့်နေစဉ်အခြေအနေများကို အချိန်နှင့်တစ်ပြေးညီ ဖော်ပြပေးသည်။

## 1. MQTT အလုပ်လုပ်ပုံ

FMS ၏ MQTT ဆက်သွယ်မှုတွင် အဓိကပါဝင်သည့် အစိတ်အပိုင်းများမှာ -

- **Dispenser / Device** — ဆီဖြည့်ခြင်းနှင့်ပတ်သက်သည့် data များကို MQTT မှတစ်ဆင့် ပေးပို့သည်။
- **Mosquitto Broker** — Device၊ Backend နှင့် Frontend တို့အကြား message များကို လက်ခံပြီး သက်ဆိုင်ရာ subscriber များထံ ဖြန့်ဝေပေးသည်။
- **Laravel Backend** — Device မှရောက်လာသော message များကို လက်ခံစစ်ဆေးပြီး သက်ဆိုင်ရာ business logic များကို ဆောင်ရွက်သည်။ လိုအပ်ပါက command နှင့် acknowledgement များကို ပြန်ပို့သည်။
- **Next.js Frontend** — Browser မှ MQTT WebSocket ကို တိုက်ရိုက်ချိတ်ဆက်ပြီး ဆီဖြည့်မှုအခြေအနေများကို real-time ပြသသည်။

### Message Flow

```text
Dispenser / Device
       |
       | MQTT publish
       v
Mosquitto Broker
       |
       +------------------------------+
       |                              |
       v                              v
Laravel Backend                Next.js Frontend
Subscribe: detpos/#             Subscribe: detpos/#
       |                              |
       v                              v
MessageReceived Event          Real-time UI Update
       |
       v
HandleMessage Listener
       |
       v
Laravel Queue Jobs
       |
       v
Database / Business Logic
       |
       v
Publish Command / ACK / Save Response
       |
       +----------> Mosquitto Broker
                         |
                         v
                  Dispenser / Device
```

**အရေးကြီးချက်:** Frontend နှင့် Backend တို့သည် Broker ကို မတူညီသော connection နည်းလမ်းများဖြင့် ချိတ်ဆက်ထားသည်။ Frontend သည် Browser မှ MQTT WebSocket ကို အသုံးပြုပြီး Backend သည် MQTT TCP client ကို အသုံးပြုသည်။

## 2. MQTT Broker နှင့် Configuration

### 2.1 Local Environment

Local Docker environment တွင် `fuel-management/docker-compose-local.yml` မှ Eclipse Mosquitto 2 ကို MQTT Broker အဖြစ် အသုံးပြုထားသည်။

Mosquitto ၏ configuration file သည် `fuel-management/docker/mosquitto/mosquitto.conf` ဖြစ်သည်။ ၎င်းတွင် အောက်ပါ port များကို သတ်မှတ်ထားသည်။

| Connection အမျိုးအစား | Port | အသုံးပြုပုံ |
|---|---:|---|
| MQTT TCP | `1883` | Laravel Backend နှင့် MQTT TCP ချိတ်ဆက်ရန် |
| MQTT WebSocket | `9001` | Browser Frontend မှ MQTT ချိတ်ဆက်ရန် |

Broker တွင် anonymous access ကို ပိတ်ထားပြီး username/password ပါဝင်သော password file ကို အသုံးပြုထားသည်။ Docker Compose မှ host port `1883` ကို broker port `1883` သို့လည်းကောင်း၊ host port `9001` ကို broker port `9001` သို့လည်းကောင်း forward လုပ်ထားသည်။

### 2.2 Backend Environment Variables

Backend MQTT configuration ကို `fuel-management/config/mqtt.php` မှ ဖတ်ယူသည်။ အဓိကအသုံးပြုသည့် Environment Variables များမှာ -

| Variable | Default Value | ရှင်းလင်းချက် |
|---|---|---|
| `MQTT_ENABLED` | `true` | MQTT connection ကို ဖွင့်/ပိတ်ရန် |
| `MQTT_HOST` | `127.0.0.1` | အဓိက MQTT Broker ၏ host |
| `MQTT_PORT` | `1883` | အဓိက Broker ၏ TCP port |
| `MQTT_USERNAME` | `detpos` | Broker login username |
| `MQTT_PASSWORD` | Config ထဲရှိ default value | Broker login password |
| `MQTT_CLIENT_ID` | `mqtt-` + unique ID | MQTT client တစ်ခုချင်းစီအတွက် သတ်မှတ်သော ID |
| `MQTT_KEEP_ALIVE` | `60` | Connection အသက်ရှင်နေမှုကို စစ်ဆေးသည့် interval (စက္ကန့်) |
| `MQTT_TWO_HOST` | `127.0.0.1` | ဒုတိယ Broker host |
| `MQTT_TWO_PORT` | `1884` | ဒုတိယ Broker TCP port |

**Security သတိပြုရန်:** Source code ထဲတွင် development အတွက် default credential များ ပါဝင်နေသည်။ Production environment တွင် သီးခြားလုံခြုံသော username/password များကို Environment Variables မှ သတ်မှတ်အသုံးပြုသင့်သည်။

### 2.3 Backend Broker ရွေးချယ်ပုံ

`HasMqtt::getClient()` သည် MQTT client ရယူရာတွင် အောက်ပါအတိုင်း လုပ်ဆောင်သည်။

1. ပထမ Broker ကို socket connection ဖြင့် အများဆုံး ၃ ကြိမ် စမ်းသပ်သည်။
2. ပထမ Broker သို့ ချိတ်ဆက်၍ မရပါက ဒုတိယ Broker ကို စမ်းသပ်သည်။
3. Broker နှစ်ခုလုံးကို မရောက်နိုင်ပါက ပထမ Broker configuration ဖြင့် client ဖန်တီးရန် ဆက်လက်ကြိုးစားသည်။
4. Client ဖန်တီးရာတွင် exception ဖြစ်ပါက `NullMqttClient` သို့ fallback လုပ်သည်။
5. `MQTT_ENABLED=false` ဖြစ်နေပါကလည်း `NullMqttClient` ကို ပြန်ပေးသည်။

### 2.4 Port နှင့် Deployment ပတ်သက်၍ သတိပြုရန်

Frontend ရှိ `utils/mqtt.ts` အရ -

- Browser ဘက်တွင် လက်ရှိ host ကို အသုံးပြုပြီး WebSocket port `9002` သို့ ချိတ်ဆက်ရန် သတ်မှတ်ထားသည်။
- Server-side တွင် `ws://localhost:9001` ကို အသုံးပြုသည်။
- Local Docker Compose တွင် WebSocket port `9001` ကိုသာ expose လုပ်ထားသည်။

ထို့ကြောင့် Browser ဘက်မှ `9002` ကို အသုံးပြုမည်ဆိုပါက deployment environment တွင် port `9002` ကို Broker ၏ WebSocket port သို့ proxy သို့မဟုတ် forward လုပ်ပေးထားရန် လိုအပ်သည်။ Port mapping မကိုက်ညီပါက Browser မှ MQTT ချိတ်ဆက်မှု မအောင်မြင်နိုင်ပါ။

### 2.5 Local နှင့် Production Environment ကွာခြားချက်

လက်ရှိ source configuration အရ -

- `docker-compose-local.yml` တွင် Mosquitto service နှင့် MQTT environment variables များ ပါဝင်သည်။
- `docker-compose-prod.yml` တွင် Mosquitto service မပါဝင်ပါ။
- Production Supervisor configuration တွင်လည်း `subscribe:message` process ကို မတွေ့ရပါ။

ထို့ကြောင့် Production environment တွင် MQTT Broker နှင့် subscriber process များကို သီးခြား server သို့မဟုတ် service အဖြစ် deploy/configure လုပ်ထားခြင်းရှိမရှိ စစ်ဆေးရန် လိုအပ်သည်။

## 3. MQTT Topics နှင့် Payload များ

FMS တွင် အသုံးပြုသော MQTT Topic များသည် `detpos/` ဖြင့် စတင်သည်။

- `{dispenser}` သည် Dispenser နံပါတ်ကို ကိုယ်စားပြုသည်။
- Payload အတွင်းရှိ `01` ကဲ့သို့ နံပါတ်သည် Nozzle နံပါတ်ကို ကိုယ်စားပြုသည်။
- Payload format များသည် Device Firmware Protocol အပေါ် မူတည်သည်။

အောက်ပါဇယားတွင် အဓိက Topic များ၊ ဥပမာ Payload များနှင့် အသုံးပြုပုံကို ဖော်ပြထားသည်။

| လုပ်ဆောင်ချက် | Topic | ဥပမာ Payload | အသုံးပြုပုံ |
|---|---|---|---|
| Permit | `detpos/device/permit/{dispenser}` | `01permit` | Device မှ Permit request ပေးပို့သည်။ Backend က request ကို စစ်ဆေးပြီး Auto/Semi approval အခြေအနေအရ Sale စတင်ဖန်တီးနိုင်သည်။ |
| Approve Command | `detpos/local_server/{dispenser}` | `01appro` | Backend က Nozzle `01` အတွက် Approve command ကို Dispenser ထံ ပို့သည်။ |
| Preset | `detpos/local_server/preset` | `01P0010000` သို့မဟုတ် `01L0003012` | Nozzle အလိုက် Preset Amount သို့မဟုတ် Liter ကို သတ်မှတ်ရန် command ပို့သည်။ Frontend ကလည်း ဤ Topic မှ Nozzle နံပါတ်ကို ဖတ်ယူသည်။ |
| Live Data | `detpos/device/livedata/{dispenser}` | `01L3.000P9960U23323` | ဆီဖြည့်နေစဉ် Liter နှင့် Price ကဲ့သို့သော live data များကို ပေးပို့သည်။ Frontend က real-time ပြသပြီး Backend က Sale state ကို `livedata` အဖြစ် သတ်မှတ်သည်။ |
| Final | `detpos/device/Final/{dispenser}` | `01S3320L3.012P10000T1234.567A123456U23323` | ဆီဖြည့်ခြင်းပြီးဆုံးသည့်အခါ နောက်ဆုံး Sale data နှင့် Totalizer data ကို ပေးပို့သည်။ Backend က Sale ကို update လုပ်ပြီး Frontend က Nozzle ၏ live state ကို ဖယ်ရှားသည်။ |
| Initialize | `detpos/device/initialize/{dispenser}` | `01S3320L0P0T1234.567A123456U23323` | Device ၏ Totalizer data ကို အခြေခံ၍ Sale ကို initialize/update လုပ်သည်။ Backend က ACK ပြန်ပို့သည်။ |
| ACK / Save Response | `detpos/local_server` | `01/ACK`, `01/D1S1` | `ACK` သည် acknowledgement ဖြစ်သည်။ `D1S1` ရောက်ရှိပါက Frontend က Save အောင်မြင်ကြောင်း notification ပြသည်။ |
| Price Request | `detpos/device/pricereq` | `01P3200` | Device မှ Price request ပေးပို့သည်။ Backend က သက်ဆိုင်ရာ Nozzle/Fuel unit price ကို ရယူပြီး Local Server သို့ ပို့သည်။ |
| Price Update | `detpos/local_server/price` | `013200` | Backend က Device ဘက်သို့ Price update ပို့သည်။ |
| Polling (Optional) | `detpos/device/polling/#` | — | Polling subscription code ကို လက်ရှိတွင် comment ပိတ်ထားသည်။ ထို့ကြောင့် အလုပ်လုပ်နေသော feature အဖြစ် မယူဆသင့်ပါ။ |

**Payload သတိပြုရန်:** `01P0010000` ကဲ့သို့ Payload များသည် Device Firmware Protocol အတိုင်း သတ်မှတ်ထားခြင်းဖြစ်သည်။ Payload ကို ကိုယ်တိုင်တည်ဆောက်ခြင်း သို့မဟုတ် ပြင်ဆင်ခြင်းမပြုမီ သက်ဆိုင်ရာ Device Protocol ကို စစ်ဆေးပါ။

## 4. Laravel Backend Implementation

Backend ၏ အဓိကတာဝန်မှာ Broker မှ message များကို လက်ခံခြင်း၊ Topic အလိုက် ခွဲခြားခြင်း၊ သက်ဆိုင်ရာ Queue Job များကို လုပ်ဆောင်စေခြင်းနှင့် Device ထံ command/response များ ပြန်ပို့ခြင်း ဖြစ်သည်။

### 4.1 MQTT Subscribe နှင့် Event Dispatch

အဓိက source file:

`fuel-management/app/Console/Commands/SubscribeMessage.php`

`subscribe:message` Artisan command သည် MQTT Broker မှ message များကို subscribe လုပ်ရန် အသုံးပြုသည်။

လုပ်ဆောင်ပုံမှာ -

1. Command စတင်သောအခါ System သည် Local/Cloud mode ကို စစ်ဆေးသည်။
2. Local mode ဖြစ်ပါက `detpos/#` Topic အောက်ရှိ message အားလုံးကို subscribe လုပ်သည်။
3. Message တစ်ခု ရောက်လာသောအခါ `MessageReceived` event ကို dispatch လုပ်သည်။
4. `supervisord-local.conf` မှ ဤ command ကို background process အဖြစ် run စေပြီး ရပ်တန့်သွားပါက ပြန်လည်စတင်စေသည်။

Polling Topic subscription ကို source code ထဲတွင် comment ပိတ်ထားသည်။

### 4.2 Topic ခွဲခြမ်းခြင်းနှင့် Queue Job Dispatch

အဓိက source file:

`fuel-management/app/Listeners/HandleMessage.php`

`HandleMessage` Listener သည် ရောက်လာသော MQTT message ကို လက်ခံပြီး -

1. Topic ကို `/` ဖြင့် အပိုင်းလိုက် ခွဲခြားသည်။
2. Payload ကို `HasMqtt::splitMessage()` ဖြင့် parse လုပ်သည်။
3. Topic ၏ သတ်မှတ်ထားသော အပိုင်း (`[2]`) ကို အခြေခံ၍ သက်ဆိုင်ရာ Queue Job ကို dispatch လုပ်သည်။

| Topic Segment | Queue Job | အဓိကတာဝန် |
|---|---|---|
| `permit` | `ProcessPermit` | Nozzle၊ User နှင့် Station Expiry တို့ကို စစ်ဆေးပြီး Auto/Semi approval အတွက် Sale စတင်ဖန်တီးခြင်း |
| `livedata` | `ProcessLivedata` | Nozzle ၏ live data ကို cache ထဲတွင် သိမ်းဆည်းပြီး Sale state ကို `livedata` အဖြစ် ပြောင်းလဲခြင်း |
| `Final` | `ProcessFinal` | ရှိပြီးသား live Sale ကို Device မှရောက်လာသော Final data ဖြင့် update လုပ်ခြင်း |
| `initialize` | `ProcessInitialize` | ACK ပြန်ပို့ပြီး Totalizer ကွာခြားချက်ကို အခြေခံ၍ Sale ကို initialize လုပ်ခြင်း |
| `pricereq` | `ProcessPriceChange` | Unit Price ကို ပြင်ဆင်ပြီး `detpos/local_server/price` Topic သို့ publish လုပ်ခြင်း |

`initialize` နှင့် `Final` message များကို ကိုင်တွယ်ရာတွင် `validateMessage()` သည် ယခင် Totalizer တန်ဖိုးနှင့် လက်ရှိ Totalizer တန်ဖိုးကို နှိုင်းယှဉ်စစ်ဆေးသည်။ လက်ရှိတန်ဖိုးသည် ယခင်တန်ဖိုးထက် လျော့နည်းနေပါက `ProcessDiscordMessage` ကို dispatch လုပ်နိုင်သည်။

### 4.3 Command Publish နှင့် Sale Lifecycle

အဓိက source file:

`fuel-management/app/Services/Local/SaleService.php`

Sale အသစ်ဖန်တီးသည့်အခါ `SaleService` သည် လိုအပ်သလို `appro` သို့မဟုတ် `preset` command ကို MQTT မှတစ်ဆင့် Device ထံ ပို့သည်။

Sale ကို update လုပ်ပြီးသည့်အခါ -

- `D1S1` ကို publish လုပ်ပါက Save အောင်မြင်ကြောင်း response အဖြစ် အသုံးပြုသည်။
- `ACK` ကို publish လုပ်ပါက acknowledgement/fallback response အဖြစ် အသုံးပြုသည်။
- Response ပို့ပြီးနောက် သက်ဆိုင်ရာ cache ကို ရှင်းလင်းသည်။

`ProcessInitialize` နှင့် `ProcessFinal` တို့သည် မူရင်း MQTT message ကို `sales.mqtt_message` field ထဲတွင် သိမ်းဆည်းသည်။

### 4.4 HasMqtt Trait

`fuel-management/app/Traits/HasMqtt.php` တွင် MQTT နှင့်သက်ဆိုင်သော common functions များကို စုစည်းထားသည်။ အဓိကအားဖြင့် -

- `splitTopic()` — MQTT Topic ကို အပိုင်းလိုက် ခွဲခြားရန်။
- `splitMessage()` — Payload ကို parse လုပ်ရန်။
- Totalizer validation — ယခင်နှင့် လက်ရှိ Totalizer တန်ဖိုးများကို စစ်ဆေးရန်။
- Broker/client handling — MQTT Broker နှင့် Client ကို ရယူအသုံးပြုရန်။

Job၊ Service နှင့် Repository များသည် `HasMqtt` ကို အသုံးပြု၍ MQTT message များကို publish လုပ်နိုင်သည်။

## 5. Next.js Frontend Implementation

Frontend သည် Browser မှ MQTT WebSocket ကို တိုက်ရိုက်ချိတ်ဆက်ပြီး Device မှ ပေးပို့လာသော live data များကို လက်ခံပြသသည်။

### 5.1 Broker URL သတ်မှတ်ပုံ

အဓိက source file:

`fuel-management-frontend-nextjs/utils/mqtt.ts`

`getMqttOptions()` သည် Environment အပေါ်မူတည်၍ WebSocket URL ကို သတ်မှတ်ပေးသည်။

- Browser တွင် လက်ရှိ Page ၏ hostname ကို အသုံးပြုသည်။
- HTTPS ဖြစ်ပါက `wss:` ကို အသုံးပြုသည်။
- HTTP ဖြစ်ပါက `ws:` ကို အသုံးပြုသည်။
- Server-side တွင် `ws://localhost:9001` ကို အသုံးပြုသည်။

**Security သတိပြုရန်:** လက်ရှိ implementation တွင် MQTT credential များကို URL ထဲတွင် ထည့်သွင်းထားသည်။ ထို့ကြောင့် Browser သို့ ပေးပို့သော credential များကို Client ဘက်မှ မြင်နိုင်သည်။ Production တွင် credential များကို မည်သို့ကာကွယ်ထားသည်ကို သီးခြားစစ်ဆေးသင့်သည်။

### 5.2 MqttProvider

အဓိက source file:

`fuel-management-frontend-nextjs/components/providers/MqttProvider.tsx`

`MqttProvider` သည် Frontend အတွင်း MQTT connection ကို မျှဝေအသုံးပြုနိုင်ရန် React Context အဖြစ် စီမံပေးသည်။

၎င်း၏ အဓိကလုပ်ဆောင်ချက်များမှာ -

1. `mqtt` package ကို အသုံးပြု၍ MQTT client တစ်ခု ဖန်တီးသည်။
2. `detpos/#` Topic ကို subscribe လုပ်သည်။
3. React Context မှတစ်ဆင့် `client`, `publish()` နှင့် `isConnected` တို့ကို အခြား component များထံ ပေးသည်။
4. `initialize`, `Final` နှင့် ACK/Save response များ ရောက်လာသောအခါ Sales query များကို invalidate လုပ်သည်။
5. `D1S1` ရောက်လာပါက Nozzle Save အောင်မြင်ကြောင်း Toast notification ပြသည်။
6. Connection error သို့မဟုတ် close ဖြစ်ပါက `isConnected` ကို `false` ပြောင်းသည်။

### 5.3 Live Fueling Display

အဓိက source file:

`fuel-management-frontend-nextjs/hooks/useMqttFueling.ts`

`useMqttFueling` Hook သည် Broker URL ရရှိပါက MQTT client နှင့် ချိတ်ဆက်ပြီး `detpos/#` Topic ကို subscribe လုပ်သည်။

Topic အလိုက် Frontend တွင် အောက်ပါအတိုင်း ကိုင်တွယ်သည်။

- **`permit` နှင့် `preset`** — သက်ဆိုင်ရာ Nozzle entry ကို ထည့်သွင်းခြင်း သို့မဟုတ် update လုပ်ခြင်း။
- **`livedata`** — Nozzle နံပါတ်၊ Liter နှင့် Price ကို ဖတ်ယူပြီး ဆီဖြည့်နေသည့်အခြေအနေကို real-time ပြသခြင်း။
- **`Final`** — ဆီဖြည့်ခြင်းပြီးဆုံးသည့်အခါ သက်ဆိုင်ရာ Nozzle entry ကို ဖယ်ရှားခြင်း။
- **Hook cleanup** — Hook ကို unmount လုပ်သည့်အခါ သို့မဟုတ် Broker URL ပြောင်းလဲသည့်အခါ MQTT client ကို ပိတ်ခြင်း။

`MqttProvider` အပြင် Installer Dispensers နှင့် Device Control UI များတွင်လည်း MQTT client ကို အသုံးပြုထားသည်။ ထို့ကြောင့် MQTT ဆိုင်ရာ code ပြင်ဆင်သည့်အခါ Component တစ်ခုတည်းကိုသာ စစ်ဆေးခြင်းမပြုဘဲ Frontend အတွင်းရှိ MQTT အသုံးပြုမှုအားလုံးကို စစ်ဆေးရန် လိုအပ်သည်။

## 6. MQTT Connection စစ်ဆေးခြင်း

MQTT connection မအောင်မြင်ပါက အောက်ပါအချက်များကို အစဉ်လိုက် စစ်ဆေးနိုင်သည်။

1. **Broker Host စစ်ဆေးပါ။** Backend container မှ MQTT Broker host သို့ ရောက်ရှိနိုင်ခြင်းရှိမရှိ စစ်ဆေးပါ။ Docker network အတွင်း `localhost` သည် Backend container ကိုယ်တိုင်ကိုသာ ရည်ညွှန်းသည်။ Compose default အရ `MQTT_HOST=mosquitto` ကို အသုံးပြုထားသည်။
2. **Port စစ်ဆေးပါ။** MQTT TCP port `1883` နှင့် WebSocket port တို့ မှန်ကန်စွာ သတ်မှတ်ထားခြင်းရှိမရှိ စစ်ဆေးပါ။ Frontend Browser URL တွင် `9002` ကို အသုံးပြုထားသော်လည်း Local Mosquitto Compose တွင် `9001` ကို map လုပ်ထားသည်။
3. **Credential စစ်ဆေးပါ။** Mosquitto `password_file` ထဲရှိ username/password နှင့် Backend/Frontend configuration တို့ ကိုက်ညီမှုရှိမရှိ စစ်ဆေးပါ။
4. **Subscriber Process စစ်ဆေးပါ။** Local Backend တွင် Supervisor ၏ `laravel-subscribe` process အလုပ်လုပ်နေခြင်းရှိမရှိ စစ်ဆေးပါ။
5. **Browser Console စစ်ဆေးပါ။** Developer Console ထဲရှိ MQTT connection error၊ subscribe error၊ `isConnected` state နှင့် WebSocket handshake result တို့ကို ကြည့်ပါ။
6. **Test Message စစ်ဆေးပါ။** Test message ပို့ရန်လိုအပ်ပါက Device Firmware Protocol နှင့် ကိုက်ညီသော Payload ကိုသာ အသုံးပြုပြီး သက်ဆိုင်ရာ Local/Test environment တွင်သာ စမ်းသပ်ပါ။

## 7. အဓိက Source Files

MQTT လုပ်ဆောင်ချက်များကို စစ်ဆေးခြင်း၊ ပြင်ဆင်ခြင်းနှင့် Debug လုပ်ခြင်းတို့အတွက် အောက်ပါ source files များသည် အဓိကကျသည်။

### Backend

- `fuel-management/config/mqtt.php`
- `fuel-management/app/Traits/HasMqtt.php`
- `fuel-management/app/Console/Commands/SubscribeMessage.php`
- `fuel-management/app/Listeners/HandleMessage.php`
- `fuel-management/app/Jobs/Local/ProcessPermit.php`
- `fuel-management/app/Jobs/Local/ProcessLivedata.php`
- `fuel-management/app/Jobs/Local/ProcessFinal.php`
- `fuel-management/app/Jobs/Local/ProcessInitialize.php`
- `fuel-management/app/Jobs/Local/ProcessPriceChange.php`
- `fuel-management/app/Services/Local/SaleService.php`

### Frontend

- `fuel-management-frontend-nextjs/utils/mqtt.ts`
- `fuel-management-frontend-nextjs/components/providers/MqttProvider.tsx`
- `fuel-management-frontend-nextjs/hooks/useMqttFueling.ts`

### Docker / Broker

- `fuel-management/docker-compose-local.yml`
- `fuel-management/docker/mosquitto/mosquitto.conf`

---

## 8. အနှစ်ချုပ်

FMS တွင် MQTT ကို အသုံးပြု၍ Dispenser/Device များမှ ဆီဖြည့်မှုဆိုင်ရာ message များကို Mosquitto Broker မှတစ်ဆင့် Laravel Backend နှင့် Next.js Frontend ထံ ပို့ဆောင်သည်။

- **Backend** သည် message များကို လက်ခံစစ်ဆေးပြီး Queue Job များမှတစ်ဆင့် Sale နှင့် Totalizer ဆိုင်ရာ လုပ်ဆောင်ချက်များကို ဆောင်ရွက်သည်။
- **Frontend** သည် MQTT WebSocket မှ live data များကို လက်ခံပြီး ဆီဖြည့်နေသည့်အခြေအနေကို real-time ပြသသည်။
- **Broker** သည် Device၊ Backend နှင့် Frontend တို့အကြား message များကို ဖြန့်ဝေပေးသည်။

Deployment ပြုလုပ်ရာတွင် Broker host၊ TCP/WebSocket port၊ credential နှင့် subscriber process များကို Environment အလိုက် မှန်ကန်စွာ သတ်မှတ်ထားရန် အရေးကြီးသည်။
