# Fuel Management System (FMS)

## 2.1 Project Introduction (စီမံကိန်းမိတ်ဆက်)

Fuel Management System (FMS) သည် ဆီဆိုင်လုပ်ငန်းများကို စီမံခန့်ခွဲရန် တည်ဆောက်ထားသော Web-based Application တစ်ခုဖြစ်သည်။

ဤစနစ်တွင် Next.js Web Application နှင့် Laravel API တို့ ပါဝင်ပြီး ဆီဆိုင်၏ Configuration များ၊ ဆီလက်ကျန်စာရင်းများ၊ Dispenser နှင့် Nozzle မှတ်တမ်းများ၊ အရောင်းလုပ်ငန်းများ၊ Customer Credit နှင့် လုပ်ငန်းဆိုင်ရာ Report များကို စီမံခန့်ခွဲနိုင်သည်။

ထို့အပြင် Local Station နှင့် Cloud Deployment ပုံစံနှစ်မျိုးလုံးကို ပံ့ပိုးပေးပြီး လိုအပ်ပါက MQTT နှင့် ATG Integration များကိုလည်း ချိတ်ဆက်အသုံးပြုနိုင်သည်။

ဤ Project တွင် Application Repository နှစ်ခု ပါဝင်သည်။

| Component | Repository Directory | အဓိကတာဝန် |
|---|---|---|
| Backend API | `fuel-management/` | Laravel API၊ လုပ်ငန်းဆိုင်ရာ Data များနှင့် သတ်မှတ်ပြင်ဆင်ထားသော Service၊ Device များနှင့် Integration လုပ်ဆောင်ခြင်း။ |
| Web Application | `fuel-management-frontend-nextjs/` | ဆီဆိုင်ဝန်ထမ်းများနှင့် Administrator များ အသုံးပြုရန် Next.js Interface ကို ပံ့ပိုးပေးခြင်း။ |

## 2.2 Project Purpose (စီမံကိန်း၏ ရည်ရွယ်ချက်)

FMS ၏ အဓိကရည်ရွယ်ချက်မှာ ဆီဆိုင်လုပ်ငန်းများနှင့် သက်ဆိုင်သော မှတ်တမ်းများကို စနစ်တစ်ခုတည်းတွင် စုစည်းစီမံခန့်ခွဲနိုင်ရန် ဖြစ်သည်။

ဤစနစ်မှတစ်ဆင့် ဝန်ထမ်းများသည် အောက်ပါလုပ်ငန်းများကို မှတ်တမ်းတင်ခြင်းနှင့် ပြန်လည်စစ်ဆေးခြင်းတို့ ပြုလုပ်နိုင်သည်။

- အရောင်းမှတ်တမ်းများ
- ဆီပို့ဆောင်ဖြည့်တင်းမှု မှတ်တမ်းများ
- ဆီဈေးနှုန်းများ
- ဆီဆိုင်သုံး စက်ပစ္စည်းများ
- ဆီသိုလှောင်ကန်ဆိုင်ရာ အချက်အလက်များ

Dashboard နှင့် Report များသည် လုပ်ငန်းဆိုင်ရာ အချက်အလက်များကို လွယ်ကူစွာ ပြန်လည်ကြည့်ရှုစစ်ဆေးနိုင်ရန် အထောက်အကူပြုသည်။

ထို့အပြင် MQTT နှင့် ATG Integration များကို သက်ဆိုင်ရာ Service များနှင့် ချိတ်ဆက်ပြင်ဆင်ထားပါက Device များမှ ရရှိသော အချက်အလက်များကို ကိုယ်တိုင်ပြန်လည်ထည့်သွင်းရသည့် လုပ်ငန်းများကို လျှော့ချနိုင်သည်။

မှတ်တမ်းများကို စနစ်တကျ စုစည်းထားခြင်းနှင့် Device များမှ ရရှိသော Data များကို ချိတ်ဆက်အသုံးပြုခြင်းတို့ကြောင့် ထပ်ခါတလဲလဲ ကိုယ်တိုင် Data ထည့်သွင်းရမှုကို လျှော့ချနိုင်ပြီး လုပ်ငန်းဆိုင်ရာ အချက်အလက်များတွင် မကိုက်ညီမှုများ ရှိ၊ မရှိ စစ်ဆေးရာ၌လည်း အထောက်အကူဖြစ်စေသည်။

သို့သော် Repository တွင် အချိန်ကုန်သက်သာမှု သို့မဟုတ် အမှားလျော့နည်းမှုအတွက် တိကျသော ကိန်းဂဏန်းပစ်မှတ်များကို သတ်မှတ်ထားခြင်း မရှိပါ။

## 2.3 Project Objectives (စီမံကိန်း၏ ရည်မှန်းချက်များ)

FMS ၏ အဓိကရည်မှန်းချက်များမှာ အောက်ပါအတိုင်းဖြစ်သည်။

- ဆီဆိုင်၊ ဆီအမျိုးအစား၊ စက်ပစ္စည်း၊ အရောင်းနှင့် အခြားလုပ်ငန်းဆိုင်ရာ မှတ်တမ်းများကို တစ်နေရာတည်းတွင် စုစည်းစီမံခန့်ခွဲရန်။
- Local Station နှင့် Cloud-based Management ပုံစံနှစ်မျိုးလုံးကို ပံ့ပိုးရန်။
- အရောင်း၊ ဆီလက်ကျန်နှင့် ဆီဆိုင်လုပ်ငန်းဆောင်ရွက်မှုများကို ပြန်လည်သုံးသပ်နိုင်ရန် Dashboard နှင့် Report များ ပံ့ပိုးရန်။
- သက်ဆိုင်ရာ Service များကို Configure လုပ်ထားပါက ATG Tank Data နှင့် MQTT Device Messaging များကို အသုံးပြုနိုင်ရန်။
- Authentication စနစ်နှင့် Configure လုပ်နိုင်သော Role နှင့် Permission များမှတစ်ဆင့် ခွင့်ပြုထားသူများကိုသာ စနစ်အသုံးပြုခွင့်ပေးရန်။
- ရရှိနိုင်သော Integration များကို အသုံးပြုခြင်းဖြင့် ထပ်ခါတလဲလဲ ကိုယ်တိုင် Data ထည့်သွင်းရမှုကို လျှော့ချရန်။

## 2.4 Scope of the Project (စီမံကိန်း၏ လုပ်ဆောင်မှုနယ်ပယ်)

### In Scope (စီမံကိန်းတွင် ပါဝင်သော လုပ်ဆောင်ချက်များ)

FMS တွင် အောက်ပါလုပ်ဆောင်ချက်များ ပါဝင်သည်။

- User Authentication နှင့် User၊ Role၊ Permission များကို စီမံခန့်ခွဲခြင်း။
- Station၊ Shop၊ Tank၊ Fuel Type၊ Stock Price၊ Dispenser နှင့် Nozzle များကို စီမံခန့်ခွဲခြင်း။
- Sales၊ Credit Sales၊ Station Sales၊ Fuel Delivery၊ Price Change နှင့် Transaction History များကို မှတ်တမ်းတင်စီမံခန့်ခွဲခြင်း။
- Local Deployment တွင် Daily Operating Session ဆိုင်ရာ လုပ်ဆောင်ချက်များကို ပံ့ပိုးခြင်း။
- Local နှင့် Cloud Deployment နှစ်မျိုးလုံးအတွက် Dashboard နှင့် လုပ်ငန်းဆိုင်ရာ Report များ ပံ့ပိုးခြင်း။
- လိုအပ်သော External Service များနှင့် Equipment များကို Configure လုပ်ထားပါက MQTT Messaging နှင့် ATG Data Operations များကို ပံ့ပိုးခြင်း။
- Local Station နှင့် Cloud Deployment Configuration များကို ပံ့ပိုးခြင်း။

### Out of Scope (စီမံကိန်းတွင် မပါဝင်သော လုပ်ဆောင်ချက်များ)

အောက်ပါလုပ်ဆောင်ချက်များသည် လက်ရှိ Project ၏ သတ်မှတ်ထားသော လုပ်ဆောင်မှုနယ်ပယ်အတွင်း မပါဝင်ပါ။

- **Native Mobile Application** — လက်ရှိ Repository တွင် Android သို့မဟုတ် iOS အတွက် သီးခြား Mobile Application မပါဝင်ဘဲ Web Application ကိုသာ ပံ့ပိုးထားသည်။
- **Third-party Service နှင့် Hardware များကို ပံ့ပိုးပေးခြင်း** — MQTT Broker၊ ATG Service သို့မဟုတ် ဆီဆိုင်သုံး Hardware များကို FMS က ကိုယ်တိုင်ပေးအပ်ခြင်း သို့မဟုတ် လည်ပတ်ထိန်းသိမ်းပေးခြင်း မဟုတ်ပါ။ သက်ဆိုင်ရာ Service များကို Configure လုပ်ထားပါက FMS မှ ချိတ်ဆက်အသုံးပြုနိုင်သည်။
- **အာမခံထားသော System Performance** — စနစ်၏ အမြဲတမ်းအသုံးပြုနိုင်မှု (Uptime)၊ Response Time၊ Data Retention နှင့် Recovery ဆိုင်ရာ အာမခံချက်များကို အထွေထွေအနေဖြင့် သတ်မှတ်မထားပါ။ ၎င်းတို့အတွက် Deployment တစ်ခုချင်းစီအလိုက် သီးခြားပစ်မှတ်များ သတ်မှတ်ရန် လိုအပ်သည်။
- **သတ်မှတ်ထားသော Payment Processing နယ်ပယ်ထက် ကျော်လွန်သော လုပ်ဆောင်ချက်များ** — လက်ရှိ Application တွင် ကိုယ်စားပြုထားသော Payment နှင့် လုပ်ငန်းဆိုင်ရာ မှတ်တမ်းများထက် ကျော်လွန်သည့် Payment Provider Processing လုပ်ဆောင်ချက်များ မပါဝင်ပါ။

အသုံးပြုသူတစ်ဦးချင်းစီ ရရှိနိုင်သော Feature များသည် အသုံးပြုနေသော Deployment Mode နှင့် သတ်မှတ်ပေးထားသည့် Permission များအပေါ် မူတည်သည်။

## 2.5 Target Users & User Roles (ရည်ရွယ်ထားသော အသုံးပြုသူများနှင့် Role များ)

အောက်တွင် ဖော်ပြထားသော Role Name များသည် Backend ရှိ Role Seeder တွင် သတ်မှတ်ထားသော အမည်များဖြစ်သည်။

Permission Matrix တွင် Seeder မှ သတ်မှတ်ပေးထားသော CRUD Permission များကို ဖော်ပြထားသည်။

- **Create** — အသစ်ဖန်တီးခွင့်
- **Read** — ကြည့်ရှုခွင့်
- **Update** — ပြင်ဆင်ခွင့်
- **Delete** — ဖျက်ပစ်ခွင့်

သို့သော် Function တစ်ခုချင်းစီကို အမှန်တကယ် အသုံးပြုနိုင်ခြင်းသည် Application Logic နှင့် Deployment Configuration များအပေါ်လည်း မူတည်နိုင်သည်။

| Role | သတ်မှတ်ပေးထားသော Permission | ရှင်းလင်းချက် |
|---|---|---|
| `admin` | Create, Read, Update, Delete | CRUD Permission အပြည့်အစုံ ရရှိသည်။ |
| `installer` | Create, Read, Update, Delete | CRUD Permission အပြည့်အစုံ ရရှိသည်။ |
| `manager` | Create, Read, Update | Delete Permission ကို Seeder မှ သတ်မှတ်မပေးထားပါ။ |
| `cashier` | Create, Read | Update နှင့် Delete Permission များကို Seeder မှ သတ်မှတ်မပေးထားပါ။ |
| `pprd` | Read | ဖတ်ရှုခွင့်သာ သတ်မှတ်ပေးထားသည်။ |
| `kyawsan` | Read | ဖတ်ရှုခွင့်သာ သတ်မှတ်ပေးထားသည်။ |

ဤ Role Name များသည် Code ထဲတွင် အမှန်တကယ် သတ်မှတ်ထားသော အမည်များဖြစ်ပြီး ယေဘုယျအလုပ်အကိုင်အမည်များကို ဆိုလိုခြင်း မဟုတ်ပါ။

Administrator များသည် Application မှတစ်ဆင့် Role နှင့် Permission များကို စီမံခန့်ခွဲနိုင်သည်။

## 2.6 Key Features (အဓိကလုပ်ဆောင်ချက်များ)

FMS ၏ အဓိက Feature များမှာ အောက်ပါအတိုင်းဖြစ်သည်။

- User Sign-in၊ Card Authentication Support နှင့် Role/Permission Management။
- Local Station နှင့် Cloud Operation Mode များ။
- Fuel Station၊ Shop၊ Tank၊ Fuel Type၊ Stock Price၊ Dispenser နှင့် Nozzle Management။
- Sales၊ Credit Sales၊ Fuel Delivery၊ Price Change နှင့် Transaction History Management။
- Daily Sales Dashboard နှင့် Local/Cloud Operational Report များ။
- Configure လုပ်ထားပါက ATG Tank Data Operations။
- Configure လုပ်ထားပါက ဆီဆိုင်သုံး Device များနှင့် MQTT Messaging။
- Cloud Feature Set တွင် Customer၊ Company၊ Member၊ Payment၊ Discount နှင့် Vehicle Type Management။

---

# System Architecture (စနစ်၏ ဖွဲ့စည်းပုံ)

FMS သည် Web Client နှင့် Backend API ကို အခြေခံထားပြီး Supporting Data Service များနှင့် Messaging Service များကိုလည်း အသုံးပြုနိုင်သော စနစ်တစ်ခုဖြစ်သည်။

ပုံမှန် Deployment တစ်ခုတွင် Frontend သည် Laravel API သို့ Request များ ပေးပို့သည်။ Backend သည် Application Data များကို ဖတ်ရှုခြင်း၊ သိမ်းဆည်းခြင်းနှင့် ပြင်ဆင်ခြင်းတို့ကို လုပ်ဆောင်ပြီး Configure လုပ်ထားသော Station Integration များနှင့်လည်း ချိတ်ဆက်နိုင်သည်။

MQTT ကို Configure လုပ်ထားပါက Real-time Communication အတွက် အသုံးပြုနိုင်ပြီး ATG Endpoint များမှတစ်ဆင့် ဆီသိုလှောင်ကန်အတွင်းရှိ ဆီပမာဏဆိုင်ရာ တိုင်းတာချက်များကို ရယူနိုင်သည်။

### Typical Data Flow (ပုံမှန် Data စီးဆင်းပုံ)

1. ဆီဆိုင်ဝန်ထမ်းသည် Next.js Web Application ကို အသုံးပြုသည်။
2. Web Application သည် လိုအပ်သော Request များကို Laravel API သို့ ပေးပို့သည်။
3. Laravel API သည် MySQL နှင့် Redis တို့ကို အသုံးပြု၍ လိုအပ်သော Data များကို ရယူခြင်း၊ သိမ်းဆည်းခြင်းနှင့် စီမံခန့်ခွဲခြင်းတို့ လုပ်ဆောင်သည်။
4. MQTT Broker၊ ဆီဆိုင်သုံး Device များနှင့် ATG Service များကို Configure လုပ်ထားပါက Backend သည် ၎င်းတို့နှင့် ဆက်သွယ်နိုင်သည်။
5. ရရှိလာသော Data များကို Application ၏ သက်ဆိုင်ရာ လုပ်ဆောင်ချက်များတွင် အသုံးပြုနိုင်သည်။

အသုံးပြုနိုင်သော Service များနှင့် ၎င်းတို့၏ Network ချိတ်ဆက်ပုံသည် Deployment Configuration အပေါ် မူတည်သည်။

---

# Technology Stack (အသုံးပြုထားသော နည်းပညာများ)

| အပိုင်း | နည်းပညာ |
|---|---|
| Backend | PHP 8.2 သို့မဟုတ် နောက်ပိုင်း Version၊ Laravel 11 |
| Frontend | Next.js 15၊ React 19၊ TypeScript |
| Database | MySQL |
| Cache နှင့် Queue | Redis၊ Laravel Horizon |
| Device Messaging | MQTT |
| Deployment | Docker Compose၊ Local Station နှင့် Cloud Configuration များ |


# Deployment Modes

## 1. Local Station Deployment

Local Station Deployment တွင် Backend API နှင့် လိုအပ်သော Service များကို ဆိုင်အတွင်းရှိ Server ပေါ်တွင် တင်ဆင်ထားပြီး Station ၏ LAN (Local Area Network) မှတစ်ဆင့် အသုံးပြုနိုင်သည်။

Frontend သည် ထို Server ပေါ်ရှိ Laravel API သို့ ချိတ်ဆက်ကာ အောက်ပါလုပ်ငန်းများကို လုပ်ဆောင်နိုင်သည်။

- User Login
- ဆီစာရင်း စီမံခန့်ခွဲခြင်း
- Dispenser နှင့် Nozzle စီမံခန့်ခွဲခြင်း
- အရောင်းမှတ်တမ်းများ ကြည့်ရှုခြင်းနှင့် ပြင်ဆင်ခြင်း
- အခြားလုပ်ငန်းဆိုင်ရာ မှတ်တမ်းများ စီမံခန့်ခွဲခြင်း

Backend ၏ Docker Compose ပတ်ဝန်းကျင်တွင် MySQL နှင့် Redis ကဲ့သို့သော Supporting Service များကိုလည်း အသုံးပြုနိုင်သည်။

### Network နှင့် Local Services

Station အတွင်းရှိ Network Service များအတွက် အောက်ပါအစိတ်အပိုင်းများကို ပြင်ဆင်အသုံးပြုနိုင်သည်။

- **Pi-hole** — Local DNS/DHCP အတွက် အသုံးပြုသည်။
- **Caddy** — Local Hostname များအတွက် HTTPS Reverse Proxy အဖြစ် အသုံးပြုသည်။

ဥပမာအားဖြင့် `fms.lan` ကို Backend API သို့၊ `pos.lan` ကို Frontend/POS သို့ ညွှန်ပေးနိုင်သည်။

Client ကိရိယာများကို မှန်ကန်သော Local DNS နှင့် Network သို့ ချိတ်ဆက်ထားရမည်။ ထို့အပြင် Caddy ၏ Internal TLS Certificate ကို Client ကိရိယာများတွင် ယုံကြည်စိတ်ချရသော Certificate အဖြစ် ထည့်သွင်းရန် လိုအပ်နိုင်သည်။

### Server နှင့် Device Configuration

Installer သည် အောက်ပါအချက်များကို ပြင်ဆင်ပေးသည်။

- Server IP Address
- DHCP Range
- Docker Container များ
- အခြား Station Service များ

ထို့ကြောင့် Deployment မလုပ်မီ Server IP၊ DHCP Range နှင့် အမြဲတမ်း IP Address အသုံးပြုမည့် Pump၊ POS၊ Printer ကဲ့သို့သော ကိရိယာများကို ကြိုတင်စီစဉ်ထားရန် လိုအပ်သည်။

### MQTT နှင့် ATG Integration

Dispenser သို့မဟုတ် အခြား Device များနှင့် အလိုအလျောက် စာတိုပေးပို့ခြင်းနှင့် လက်ခံခြင်းတို့ လုပ်ဆောင်ရန် လိုအပ်ပါက MQTT Broker ကို ချိတ်ဆက်ပြီး Device များ၏ Topic နှင့် Message ပုံစံများကို ကိုက်ညီအောင် Configure လုပ်ရသည်။

ATG Tank Reading များကို အသုံးပြုရန်လည်း သက်ဆိုင်ရာ ATG Service နှင့် ချိတ်ဆက်ထားရမည်။

**သတိပြုရန်။** MQTT နှင့် ATG တို့သည် Station တိုင်းတွင် အလိုအလျောက် အသုံးပြုနိုင်သော Service များ မဟုတ်ပါ။ သက်ဆိုင်ရာ Service၊ Hardware၊ Network နှင့် Configuration များ အဆင်သင့်ဖြစ်မှသာ လုပ်ဆောင်နိုင်မည်ဖြစ်သည်။

အသေးစိတ် Configuration လမ်းညွှန်များကို အောက်ပါဖိုင်များတွင် ကြည့်ရှုနိုင်သည်။

- `fuel-management/README.md` — Provisioning နှင့် Installation
- `fuel-management/NETWORK.md` — LAN၊ DNS၊ DHCP နှင့် Proxy Configuration
- `fuel-management/MQTT.md` — MQTT Topic နှင့် Message Configuration

---

## 2. Cloud Deployment

Cloud Deployment တွင် Backend API၊ Database၊ Cache နှင့် Queue Service များကို Cloud Server သို့မဟုတ် သက်ဆိုင်ရာ Hosting ပတ်ဝန်းကျင်တွင် တင်ဆင်ထားသည်။

အသုံးပြုသူများသည် Network မှတစ်ဆင့် Frontend ကို ဝင်ရောက်အသုံးပြုပြီး Frontend မှ Backend API သို့ ချိတ်ဆက်ကာ လုပ်ငန်းဆိုင်ရာ အချက်အလက်များကို ရယူခြင်းနှင့် ပြင်ဆင်ခြင်းတို့ လုပ်ဆောင်နိုင်သည်။

### Frontend Configuration

Frontend ၏ Environment Variables များတွင် အသုံးပြုမည့် Setting များကို သတ်မှတ်ရသည်။

- API URL
- Application URL
- Authentication ဆိုင်ရာ Setting များ
- လိုအပ်ပါက MQTT Broker Address

သတ်မှတ်ထားသော URL များနှင့် Authentication Configuration များသည် Cloud Deployment ပတ်ဝန်းကျင်နှင့် ကိုက်ညီမှုရှိရမည်။

Frontend Repository ၏ လမ်းညွှန်ချက်အရ Cloud Deployment အတွက် `.env.prod` ကို အသုံးပြုရသည်။

### Backend Configuration

Backend Setup လမ်းညွှန်အရ Cloud Deployment အတွက် `.env.example` ဖိုင်ကို `.env` အဖြစ် ကူးယူပြီး Production Environment နှင့် သက်ဆိုင်သော Setting များကို ပြင်ဆင်ရသည်။

အဓိကသတ်မှတ်ရမည့် Setting များမှာ အောက်ပါအတိုင်းဖြစ်သည်။

- `APP_ENV=production`
- Database Connection Configuration
- Redis Connection Configuration
- Application URL
- အခြား Production Service များနှင့် သက်ဆိုင်သော Setting များ

ဤ Configuration များကို သတ်မှတ်ပြီးနောက် Installer သည် Production Docker Compose Configuration ကို ရွေးချယ်အသုံးပြုသည်။

### Deployment မလုပ်မီ စစ်ဆေးရန်

Cloud Deployment မလုပ်မီ အောက်ပါအချက်များကို စစ်ဆေးရမည်။

1. Frontend API URL နှင့် Backend Application URL ကို မှန်ကန်စွာ သတ်မှတ်ထားခြင်း။
2. Database နှင့် Redis တို့ကို Backend မှ ချိတ်ဆက်အသုံးပြုနိုင်ခြင်း။
3. လိုအပ်သော Environment Variables နှင့် Secret များကို မှန်ကန်စွာ သတ်မှတ်ထားခြင်း။
4. Cloud Server မှ လိုအပ်သော Service များသို့ လုံခြုံစွာ ချိတ်ဆက်နိုင်ခြင်း။
5. Production Environment အတွက် လိုအပ်သော Configuration များ ပြည့်စုံခြင်း။

### MQTT နှင့် ATG Integration

Cloud Deployment ဖြစ်ခြင်းတစ်ခုတည်းကြောင့် MQTT သို့မဟုတ် ATG Integration များ အလိုအလျောက် ပါဝင်လာမည် မဟုတ်ပါ။

ထို Integration များကို အသုံးပြုရန် လိုအပ်ပါက Cloud Server မှ သက်ဆိုင်ရာ MQTT Broker သို့မဟုတ် ATG Service ကို ရောက်ရှိချိတ်ဆက်နိုင်မှု ရှိရမည်။ ထို့အပြင် လိုအပ်သော Hardware၊ Network နှင့် သီးခြား Configuration များကိုလည်း ပြင်ဆင်ထားရန် လိုအပ်သည်။

---

## 3. Local Station နှင့် Cloud Deployment ကွာခြားချက်

| အချက် | Local Station | Cloud |
|---|---|---|
| Backend API | Station အတွင်းရှိ Server | Cloud Server သို့မဟုတ် Hosting |
| Network Access | Station ၏ LAN | သက်ဆိုင်ရာ Network မှတစ်ဆင့် |
| DNS/Proxy | Pi-hole နှင့် Caddy ကို အသုံးပြုနိုင်သည် | Cloud Environment အလိုက် သတ်မှတ်သည် |
| Database/Redis | Local Docker Compose တွင် အသုံးပြုနိုင်သည် | Cloud သို့မဟုတ် သက်ဆိုင်ရာ Server တွင် စီစဉ်သည် |
| Configuration | Station IP၊ DHCP Range နှင့် Device များ | Production URL၊ Database နှင့် Secret များ |
| MQTT/ATG | သီးခြား Service နှင့် Configuration လိုအပ်သည် | သီးခြား Service နှင့် Configuration လိုအပ်သည် |

**အနှစ်ချုပ်။** Local Station Deployment သည် ဆိုင်အတွင်းရှိ Local Network နှင့် Server ကို အခြေခံထားပြီး Cloud Deployment သည် Cloud Server နှင့် Production Environment ကို အခြေခံထားသည်။ Deployment Mode နှစ်မျိုးစလုံးတွင် MQTT နှင့် ATG ကဲ့သို့သော Integration များကို အသုံးပြုလိုပါက သက်ဆိုင်ရာ Service၊ Hardware၊ Network နှင့် Configuration များကို သီးခြားစီ ပြင်ဆင်ထားရန် လိုအပ်သည်။
## Repository Guide

- `fuel-management/README.md`: backend setup,
  deployment, Make commands, and environment configuration.
- `fuel-management/NETWORK.md`: local station
  networking and service setup.
- `fuel-management/MQTT.md`: MQTT integration
  notes.
- `fuel-management-frontend-nextjs/README.md`:
  frontend setup and environment configuration.

## Documentation Scope

This overview introduces the system and its major components. Detailed
installation, configuration, API, deployment, and operational procedures
should be documented in their own pages as the MkDocs site grows.

