# Installation နှင့် Setup

ဤလမ်းညွှန်တွင် Fuel Management System (FMS) ကို Local Development Environment တွင် စတင်အသုံးပြုနိုင်ရန် Backend (`fuel-management`) နှင့် Frontend (`fuel-management-frontend-nextjs`) တို့ကို တပ်ဆင်ပြင်ဆင်ပုံ အဆင့်ဆင့် ဖော်ပြထားသည်။

ဤလမ်းညွှန်တွင် Backend၊ Database၊ Redis နှင့် MQTT Broker တို့ကို Docker Compose ဖြင့် run ပြီး Frontend ကို Node.js ဖြင့် သီးခြား run သည့်နည်းလမ်းကို အဓိကထား ရှင်းပြထားသည်။

Production Station သို့မဟုတ် Cloud Environment တွင် တပ်ဆင်အသုံးပြုမည်ဆိုပါက Repository တစ်ခုချင်းစီရှိ README နှင့် သက်ဆိုင်ရာ Deployment Configuration များကိုပါ ဖတ်ရှုရန် လိုအပ်သည်။

## 1. လိုအပ်သော Software များ

Installation မစတင်မီ အောက်ပါ Software များကို ကြိုတင်တပ်ဆင်ထားရန် လိုအပ်သည်။

| Software | အသုံးပြုပုံ |
|---|---|
| Git | Backend နှင့် Frontend Source Code များကို ရယူရန် |
| Docker Engine | Backend နှင့် သက်ဆိုင်ရာ Service များကို Container အတွင်း run ရန် |
| Docker Compose Plugin | Docker Service များကို တစ်ပြိုင်နက် စတင်/ရပ်တန့်ရန် |
| GNU Make | Backend ၏ Makefile command များကို အသုံးပြုရန် |
| Node.js 22 | Frontend Development Server ကို run ရန် |
| Corepack / pnpm | Frontend package များကို install လုပ်ရန် |

Backend ကို Docker မသုံးဘဲ Host Machine ပေါ်တွင် တိုက်ရိုက် run မည်ဆိုပါက အောက်ပါ Software များလည်း လိုအပ်သည်။

- PHP 8.2 နှင့်အထက်
- Composer 2
- Laravel အတွက် လိုအပ်သော PHP Extensions များ

## 2. Source Code ရယူခြင်း

FMS Project တွင် Backend နှင့် Frontend Repository များကို သီးခြားခွဲထားသည်။ ထို့ကြောင့် Repository နှစ်ခုလုံးကို Local Machine ပေါ်တွင် Clone လုပ်ရန် လိုအပ်သည်။

ဥပမာအားဖြင့် Project အတွက် Folder တစ်ခုတည်ဆောက်ပြီး အောက်ပါအတိုင်း Clone လုပ်နိုင်သည်။

```bash
mkdir fuel-management
cd fuel-management

git clone <backend-repository-url> fuel-management
git clone <frontend-repository-url> fuel-management-frontend-nextjs
```

`<backend-repository-url>` နှင့် `<frontend-repository-url>` နေရာတွင် Project ၏ လက်ရှိ Git Hosting မှ ရရှိသော Repository URL များကို အစားထိုးအသုံးပြုပါ။

Clone ပြီးပါက Folder Structure သည် အောက်ပါအတိုင်း ဖြစ်ရမည်။

```text
fuel-management/
├── fuel-management/
└── fuel-management-frontend-nextjs/
```

## 3. Backend Setup (Docker Compose)

### 3.1 Environment File ပြင်ဆင်ခြင်း

ပထမဦးစွာ Backend Repository ထဲသို့ ဝင်ပြီး `.env.example` ကို မိတ္တူကူး၍ `.env` ဖိုင်တစ်ခု ဖန်တီးပါ။

```bash
cd fuel-management
cp .env.example .env
```

ထို့နောက် `.env` ဖိုင်ကို ဖွင့်ပြီး Local Development အတွက် လိုအပ်သော Configuration များကို ပြင်ဆင်ပါ။

| Setting | Local Docker ဥပမာ | ရှင်းလင်းချက် |
|---|---|---|
| `APP_ENV` | `local` | Development Environment အဖြစ် သတ်မှတ်ရန် |
| `APP_DEBUG` | `true` | Local Development တွင် Error အသေးစိတ်များ ကြည့်ရှုရန် |
| `APP_URL` | `http://localhost:9000` | Backend Application URL |
| `FRONTEND_URL` | `http://localhost:3000` | Frontend Development Server URL |
| `DB_HOST` | `mysql` | Docker Compose ရှိ MySQL Service Name |
| `DB_PORT` | `3306` | MySQL Container Port |
| `DB_DATABASE` | ကိုယ်ပိုင် Database Name | အသုံးပြုမည့် Database အမည် |
| `DB_USERNAME` | ကိုယ်ပိုင်တန်ဖိုး | Database Username |
| `DB_PASSWORD` | ကိုယ်ပိုင်တန်ဖိုး | Database Password |
| `REDIS_HOST` | `redis` | Docker Compose ရှိ Redis Service Name |
| `REDIS_PORT` | `6379` | Redis Container Port |
| `MQTT_HOST` | `mosquitto` | Docker Compose ရှိ MQTT Broker Service Name |
| `MQTT_PORT` | `1883` | MQTT Broker TCP Port |
| `MQTT_USERNAME` | Broker Username | Mosquitto Configuration နှင့် ကိုက်ညီရမည် |
| `MQTT_PASSWORD` | Broker Password | Mosquitto Password File နှင့် ကိုက်ညီရမည် |

**သတိပြုရန်**

- `DB_DATABASE`, `DB_USERNAME` နှင့် `DB_PASSWORD` တို့ကို MySQL Service Configuration နှင့် ကိုက်ညီအောင် သတ်မှတ်ပါ။
- MQTT Username နှင့် Password များသည် Mosquitto ၏ Password File ထဲရှိ Credential များနှင့် ကိုက်ညီရမည်။
- Secret များကို မိမိ၏ Environment အတွက် သီးခြားလုံခြုံသော တန်ဖိုးများဖြင့် သတ်မှတ်ပါ။
- `.env` ဖိုင်ကို Git Repository ထဲသို့ Commit မလုပ်ပါနှင့်။
- Development အတွက်သာ သတ်မှတ်ထားသော Default Key သို့မဟုတ် Password များကို Production တွင် ပြန်လည်အသုံးမပြုပါနှင့်။

### 3.2 Docker Container များ စတင်ခြင်း

Environment File ပြင်ဆင်ပြီးပါက Backend Repository ၏ Root Directory မှ အောက်ပါ Command ကို run ပါ။

```bash
make up
```

ဤ Command သည် `docker-compose-local.yml` ကို အသုံးပြုပြီး အောက်ပါ Service များကို စတင်ပေးသည်။

- Laravel Backend
- MySQL Database
- Redis
- Mosquitto MQTT Broker

Docker Compose တွင် MySQL Health Check ကို စောင့်ဆိုင်းရသောကြောင့် Container များ စတင်ရန် အချိန်အနည်းငယ် ကြာနိုင်သည်။

### 3.3 Application Key နှင့် Database Migration

Container များ စတင်ပြီးပါက Backend Container အတွင်း ဝင်ရောက်၍ Laravel Application Key ကို Generate လုပ်ပြီး Database Migration များကို run ပါ။

```bash
make app-shell
php artisan key:generate
php artisan migrate
exit
```

Command တစ်ခုချင်းစီ၏ ရည်ရွယ်ချက်မှာ -

| Command | ရှင်းလင်းချက် |
|---|---|
| `make app-shell` | Backend Application Container အတွင်း ဝင်ရန် |
| `php artisan key:generate` | Laravel Application Encryption Key ကို Generate လုပ်ရန် |
| `php artisan migrate` | Database Table များကို Migration အတိုင်း တည်ဆောက်/ပြင်ဆင်ရန် |
| `exit` | Container Shell မှ ပြန်ထွက်ရန် |

Database Schema အသစ်များ ထပ်မံထည့်သွင်းထားပါကလည်း သက်ဆိုင်ရာ Migration များကို run လုပ်ရန် လိုအပ်သည်။

**Sample Data ထည့်သွင်းရာတွင် သတိပြုရန်**

Backend README အရ `make seed` သည် `migrate:fresh --seed` ကို လုပ်ဆောင်သည်။ ထို့ကြောင့် ရှိပြီးသား Database Table များကို ပြန်လည်တည်ဆောက်ပြီး Data များကို ဖျက်ပစ်နိုင်သည်။ လက်ရှိ Database Data များကို မဆုံးရှုံးစေလိုပါက `make seed` ကို မစမ်းသပ်မီ သေချာစစ်ဆေးပါ။

### 3.4 အသုံးဝင်သော Backend Commands များ

အောက်ပါ Make Commands များကို Backend Service များ စီမံခန့်ခွဲရာတွင် အသုံးပြုနိုင်သည်။

| Command | အသုံးပြုပုံ |
|---|---|
| `make status` | Docker Service များ၏ လက်ရှိအခြေအနေကို ကြည့်ရန် |
| `make logs` | Service Log များကို ကြည့်ရန် |
| `make app-shell` | Backend Container အတွင်း ဝင်ရန် |
| `make artisan migrate:status` | Migration များ၏ အခြေအနေကို စစ်ဆေးရန် |
| `make down` | Docker Compose Service များကို ရပ်တန့်ရန် |

Backend API ကို ပုံမှန်အားဖြင့် `http://localhost:9000` မှတစ်ဆင့် ဝင်ရောက်နိုင်သည်။ အမှန်တကယ် အသုံးပြုနိုင်သော URL သည် Docker Compose Port Mapping နှင့် `.env` Configuration ပေါ်တွင် မူတည်သည်။

Local Supervisor Configuration တွင် MQTT `subscribe:message` Process နှင့် Queue Worker တို့ ပါဝင်သည်။

## 4. Frontend Setup

### 4.1 Environment File ပြင်ဆင်ခြင်း

Backend Setup ပြီးပါက Frontend Repository သို့ ပြောင်းဝင်ပါ။

```bash
cd ../fuel-management-frontend-nextjs
cp .env.example .env
```

Corepack ကို Enable လုပ်ပြီး လိုအပ်သော Package များကို Install လုပ်ပါ။

```bash
corepack enable
pnpm install
```

### 4.2 Frontend Environment Variables

Frontend ၏ `.env` ဖိုင်တွင် Local Environment နှင့် ကိုက်ညီသော URL နှင့် Secret များကို သတ်မှတ်ပါ။

```dotenv
NEXT_PUBLIC_APP_TYPE=local
NEXT_PUBLIC_APP_IMAGE_URL=localhost
NEXT_PUBLIC_API_URL=http://localhost:9000/v1/api/
NEXT_PUBLIC_MQTT_BROKER_URL=ws://<mqtt-user>:<mqtt-password>@localhost:9001
NEXTAUTH_URL=http://localhost:3000/
NEXTAUTH_SECRET=<long-random-development-secret>
```

Environment Variable များ၏ အသုံးပြုပုံမှာ -

| Variable | ရှင်းလင်းချက် |
|---|---|
| `NEXT_PUBLIC_APP_TYPE` | `local` သို့မဟုတ် `cloud` ကို သတ်မှတ်ပြီး API Request အချို့၏ Base URL ရွေးချယ်ပုံကို သက်ရောက်စေသည်။ |
| `NEXT_PUBLIC_APP_IMAGE_URL` | Frontend တွင် အသုံးပြုသော Image URL ဆိုင်ရာ Configuration ဖြစ်သည်။ |
| `NEXT_PUBLIC_API_URL` | Backend API ၏ Base URL ကို သတ်မှတ်သည်။ |
| `NEXT_PUBLIC_MQTT_BROKER_URL` | MQTT Broker WebSocket URL ကို သတ်မှတ်ရန် ရည်ရွယ်ထားသော Environment Variable ဖြစ်သည်။ သို့သော် လက်ရှိ `utils/mqtt.ts` က ၎င်းကို တိုက်ရိုက်ဖတ်မဖတ်ကို သီးခြားစစ်ဆေးရန် လိုအပ်သည်။ |
| `NEXTAUTH_URL` | NextAuth အသုံးပြုမည့် Frontend URL ဖြစ်သည်။ |
| `NEXTAUTH_SECRET` | NextAuth အတွက် အသုံးပြုသော Secret ဖြစ်သည်။ |

**Security သတိပြုရန်**

`NEXT_PUBLIC_` ဖြင့် စတင်သော Environment Variable များသည် Browser Client Bundle ထဲသို့ ပါဝင်နိုင်သည်။ ထို့ကြောင့် Password သို့မဟုတ် အခြားလျှို့ဝှက် Credential များကို `NEXT_PUBLIC_` Variable များထဲတွင် ထည့်သွင်းအသုံးပြုခြင်းကို Production တွင် အထူးသတိပြုရမည်။

`NEXTAUTH_SECRET` အတွက်လည်း မျှဝေသုံးစွဲထားသော Default Value မဟုတ်ဘဲ မိမိ Environment အတွက် သီးခြား Random Secret တစ်ခု သတ်မှတ်ပါ။

### 4.3 Frontend Development Server စတင်ခြင်း

Package များ Install ပြီးပါက Frontend Development Server ကို စတင်နိုင်သည်။

```bash
pnpm dev
```

ပုံမှန်အားဖြင့် Frontend ကို အောက်ပါ URL မှတစ်ဆင့် ဝင်ရောက်နိုင်သည်။

```text
http://localhost:3000
```

Production နှင့် ပိုမိုနီးစပ်သော Local Build ကို စမ်းသပ်လိုပါက အောက်ပါ Command များကို အသုံးပြုပါ။

```bash
pnpm build
pnpm start
```

## 5. Local URL နှင့် Port Configuration စစ်ဆေးခြင်း

Frontend နှင့် Backend ကို စတင်အသုံးပြုရာတွင် သတိပြုရမည့်အချက်တစ်ခုမှာ Frontend Source Code ထဲရှိ API/MQTT URL အချို့သည် `.env.example` တွင် သတ်မှတ်ထားသော URL များနှင့် မတူညီနိုင်ခြင်း ဖြစ်သည်။

လက်ရှိ Source Code အရ အောက်ပါအချက်များကို စစ်ဆေးရန် လိုအပ်သည်။

### 5.1 Backend API Port

`fuel-management-frontend-nextjs/utils/appAxios.ts` တွင် Local Mode အတွက် Browser Request ကို အောက်ပါ URL သို့ ပို့ထားသည်။

```text
http://<current-host>:8000/v1/api/
```

သို့သော် Backend Docker Compose တွင် Port `9000:9000` ကို expose လုပ်ထားသည်။

ထို့ကြောင့် Frontend က Port `8000` သို့ Request ပို့နေပြီး Backend က Port `9000` တွင်သာ လက်ခံနေပါက API Request မအောင်မြင်နိုင်ပါ။ Port `8000` မှ `9000` သို့ Proxy သို့မဟုတ် Port Mapping မရှိပါက `NEXT_PUBLIC_API_URL` တစ်ခုတည်း ပြင်ဆင်ခြင်းဖြင့် Browser Request URL ပြောင်းလဲမည်ဟု မယူဆသင့်ပါ။

### 5.2 MQTT WebSocket Port

`fuel-management-frontend-nextjs/utils/mqtt.ts` တွင် -

- Browser ဘက်မှ WebSocket URL ကို လက်ရှိ Host ၏ Port `9002` ဖြင့် တည်ဆောက်ထားသည်။
- Server-side တွင် `localhost:9001` ကို အသုံးပြုသည်။
- Local Mosquitto Docker Compose တွင် Port `9001:9001` ကို expose လုပ်ထားသည်။

ထို့ကြောင့် Browser မှ MQTT ချိတ်ဆက်ရာတွင် Source Code က အသုံးပြုသော Port နှင့် Docker/Proxy Configuration တို့ ကိုက်ညီမှုရှိမရှိ စစ်ဆေးပါ။

### 5.3 MQTT Environment Variable

လက်ရှိ `utils/mqtt.ts` သည် `NEXT_PUBLIC_MQTT_BROKER_URL` ကို တိုက်ရိုက်ဖတ်ရှုအသုံးပြုခြင်း မရှိပါ။

ထို့ကြောင့် `.env` ထဲတွင် `NEXT_PUBLIC_MQTT_BROKER_URL` ကို သတ်မှတ်ထားရုံဖြင့် MQTT Connection URL ပြောင်းလဲသွားမည်ဟု မယူဆသင့်ပါ။ အမှန်တကယ် အသုံးပြုသော URL ကို Source Code ထဲတွင် စစ်ဆေးပါ။

### 5.4 URL/Port မကိုက်ညီမှုကို ဖြေရှင်းခြင်း

Frontend မှ Backend API သို့မဟုတ် MQTT Broker ကို ချိတ်ဆက်၍ မရပါက အောက်ပါနေရာများကို တစ်ဆင့်ချင်း စစ်ဆေးပါ။

1. Browser Developer Tools ၏ **Network** Tab တွင် Request URL နှင့် Response Status ကို စစ်ဆေးပါ။
2. **Console** Tab တွင် API သို့မဟုတ် MQTT Connection Error ရှိမရှိ ကြည့်ပါ။
3. `utils/appAxios.ts` ရှိ API Base URL ကို စစ်ဆေးပါ။
4. `utils/mqtt.ts` ရှိ MQTT WebSocket URL ကို စစ်ဆေးပါ။
5. Docker Compose Port Mapping နှင့် Reverse Proxy Configuration တို့ကို တိုက်စစ်ပါ။

## 6. Installation အောင်မြင်မှု စစ်ဆေးခြင်း

Backend နှင့် Frontend Setup ပြီးပါက အောက်ပါအဆင့်များအတိုင်း စစ်ဆေးပါ။

1. **Backend Service များ စစ်ဆေးပါ။**

   ```bash
   make status
   ```

   သို့မဟုတ် -

   ```bash
   docker compose -f docker-compose-local.yml ps
   ```

   Backend၊ MySQL၊ Redis နှင့် Mosquitto Service များ Running ဖြစ်နေကြောင်း စစ်ဆေးပါ။

2. **Backend API စစ်ဆေးပါ။**

   သတ်မှတ်ထားသော Backend API URL ကို Browser သို့မဟုတ် API Client မှ ခေါ်ဆိုပြီး Backend သို့ ချိတ်ဆက်နိုင်ကြောင်း စစ်ဆေးပါ။

3. **Frontend စစ်ဆေးပါ။**

   Browser တွင် `http://localhost:3000` ကို ဖွင့်ပြီး Frontend Application နှင့် Sign-in Page ပေါ်လာကြောင်း စစ်ဆေးပါ။

4. **Login/API ချိတ်ဆက်မှု စစ်ဆေးပါ။**

   Login သို့မဟုတ် API Request မအောင်မြင်ပါက Frontend API Base URL၊ Backend `APP_URL`၊ CORS/Sanctum Domain နှင့် Frontend URL တို့ကို တိုက်စစ်ပါ။

5. **MQTT ချိတ်ဆက်မှု စစ်ဆေးပါ။**

   MQTT လိုအပ်သော UI ကို ဖွင့်ပြီး Browser Console မှ WebSocket Connection အောင်မြင်မှု၊ Broker Credential နှင့် Port မှန်ကန်မှုတို့ကို စစ်ဆေးပါ။

## 7. Cloud နှင့် Station Installation မှတ်ချက်များ

ဤလမ်းညွှန်သည် Local Development Setup ကို အဓိကထား ရှင်းပြထားခြင်းဖြစ်သည်။ Cloud သို့မဟုတ် Station Environment တွင် Installation ပြုလုပ်ရာ၌ Configuration နှင့် လုပ်ဆောင်ရမည့်အဆင့်များ ကွာခြားနိုင်သည်။

### 7.1 Backend Deployment

Backend README တွင် ဖော်ပြထားသည့်အတိုင်း -

- Station Deployment အတွက် `.env.station` ကို အသုံးပြုသည်။
- `make install` သည် Host Provisioning လုပ်ဆောင်ချက်များကို ဆောင်ရွက်သည်။
- Cloud Deployment အတွက် `.env.example` ကို အခြေခံပြီး `APP_ENV=production` သတ်မှတ်ကာ Production Compose Configuration ကို အသုံးပြုသည်။

**သတိပြုရန်:** `make install` သည် Server/System Configuration များကို ပြောင်းလဲနိုင်သည်။ ထို့ကြောင့် Production သို့မဟုတ် Station Server တွင် မလုပ်ဆောင်မီ Backend README နှင့် Deployment Checklist ကို အပြည့်အစုံ ဖတ်ရှုပါ။

### 7.2 Frontend Deployment

Frontend README တွင် -

- Local Station အတွက် `.env.example` ကို အခြေခံအသုံးပြုရန်။
- Cloud Environment အတွက် `.env.prod` ကို အခြေခံအသုံးပြုရန်။
- Deployment တွင် `entrypoint.sh` ကို run လုပ်ရန်။

ဖော်ပြထားသည်။

လက်ရှိ Frontend Dockerfile သည် Node.js 22 နှင့် pnpm ကို အသုံးပြု၍ Application Image ကို Build လုပ်ပြီး Port `3000` ကို expose လုပ်ထားသည်။

Production Deployment မပြုမီ အောက်ပါ Configuration များကို မှန်ကန်စွာ သတ်မှတ်ထားရန် လိုအပ်သည်။

- Production Hostname
- Backend API URL
- MQTT WebSocket URL
- TLS/HTTPS နှင့် Reverse Proxy
- `NEXTAUTH_URL`
- `NEXTAUTH_SECRET`

## 8. အဓိက Source Files

Installation နှင့် Configuration ဆိုင်ရာ အချက်အလက်များကို စစ်ဆေးရန် အောက်ပါ Source Files များကို ကိုးကားနိုင်သည်။

### Backend

- `fuel-management/README.md`
- `fuel-management/Makefile`
- `fuel-management/docker-compose-local.yml`
- `fuel-management/config/mqtt.php`

### Frontend

- `fuel-management-frontend-nextjs/README.md`
- `fuel-management-frontend-nextjs/package.json`
- `fuel-management-frontend-nextjs/Dockerfile`

### Frontend API နှင့် MQTT Configuration

- `fuel-management-frontend-nextjs/utils/appAxios.ts`
- `fuel-management-frontend-nextjs/utils/mqtt.ts`

---

## 9. အနှစ်ချုပ်

FMS ကို Local Development Environment တွင် စတင်အသုံးပြုရန် Backend နှင့် သက်ဆိုင်ရာ Service များကို Docker Compose ဖြင့် စတင်ပြီး Frontend ကို Node.js/pnpm ဖြင့် သီးခြား run ရမည်။

Installation အောင်မြင်ရန် အဓိကအားဖြင့် -

- Backend၊ MySQL၊ Redis နှင့် Mosquitto Service များ Running ဖြစ်နေရမည်။
- Laravel Application Key နှင့် Database Migration များ မှန်ကန်စွာ ပြင်ဆင်ထားရမည်။
- Frontend Environment Variables များသည် Local Environment နှင့် ကိုက်ညီရမည်။
- API URL၊ MQTT WebSocket URL နှင့် Docker Port Mapping များ ကိုက်ညီရမည်။
- Login နှင့် MQTT Connection များကို Browser မှတစ်ဆင့် စမ်းသပ်အတည်ပြုရမည်။

Local Setup ပြီးပါက FMS Frontend နှင့် Backend တို့ကို ချိတ်ဆက်အသုံးပြုနိုင်မည်ဖြစ်သည်။
