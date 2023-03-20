# Project Base Tutorial part 1


### مقدّمة

سنقوم في هذه المقالة باتبّاع نمط Code along، والذي يعني أننا سنتعلّم على بناء APIs باستخدام Prisma/Express/Typescript/MySQL وغيرها من التقنيات لبناء مشروع ToDo بسيط. 
كما سنفتتح بناء المشروع بكتابة خطّة او تحديد المتطلّبات اللّازمة لبنائه.



### الفكرة

في مشروع ToDo بشكل بسيط نريد أن يكون لدينا مستخدم قادر على كتابة مجموعة من المهام الخاصّة به و تحديد ما ان أتمّها ام لا. كما نريد ايضاً اضافة أكثر من مستخدم بحيث يقوم كل مستخدم بتسجيل الدخول وإضافة مهامّة الخاصّة. 
ولا ننسى أن نعطي جميع المستخدمين القدرة على اضافة، تحديث، حذف و قراءة جميع المهام الخاصّة بهم. 



### التخطيط

في بداية أي مشروع، يجب علينا تحديد وقت مخصص فقط لعمليّة التخطيط. لتحديد ما هي العناصر التي سنقوم بحفظها في قاعدة البيانات، ما هي العمليّات التي يستطيع كل مستخدم القيام بها، هل لدينا أنواع من المستخدمين او هو مستخدم واحد فقط. ما هي العلاقات بين الجداول في قاعدة البيانات.

كل هذه التساؤلات يجب علينا الرد عليها، قد لا تجيب عنها جميعها لعدم وضوح الصورة في البداية، لكن خذ بعين الاعتبار أن بناء الأساس الصحيح وتحديد هيكلة قاعدة البيانات الصحيحة ستساعدك لاحقاً على اضافة خصائص جديدة بكل سهولة. 

أفضّل دائماً البدء بتحديد هيكلة قاعدة البيانات في المشروع، لأنه كما ذكرت سابقاً. تحديد الهيكلة الصحيحة سيساعد في تطوّر المشروع لاحقاً بدون تعقيد. و بما أننا سنقوم ببناء برنامج تحديد مهام ToDo سنقوم بتخزين المهام التي سيدونها المستخدم. 

(١) ما هي البيانات ( الجداول/Tables )
قم بالتفكير، ما هي البيانات التي سنحتاج الى تخزينها في قاعدة البيانات؟ تذّكر أن المشروع هو عبارة عن ToDo. 
سنقوم بتخزين التّالي:

- المهام Tasks
- معلومات المستخدمين Users

هل يوجد معلومات إضافيّة أساسيّة يجب تخزينها؟ الى الآن لا أفكّر في معلومات أخرى. لذلك سنبقى على ماقمنا بتحديده سابقاً. 

(٢) ما هي البيانات ( الحقول/Fields )

- جدول المستخدمين Users
    - اسم المستخدم name
    - بريد الالكتروني email
    - الرمز السّري password
    
- جدول المهام Tasks
    - العنوان Title
    - هل تمّ اتمامها isCompleted

نستطيع أيضاً اضافة بيانات أخرى مثل تاريخ ميلاد المستخدم و تاريخ انشاء و اتمام المهمّة و غيرها من المعلومات. لكن الى الآن لا نحتاج أي معلومات اضافيّة. لذا سنبقى على البيانات الأساسيّة الحاليّة.

(٣) ما هي العلاقات
هل توجد علاقة بين جدول المهام Tasks و جدول المستخدمين Users؟ 
نعم، كل مستخدم سيقوم بكتابة مجموعة من المهام. وكل مهمّة، هي خاصّة في مستخدم واحد فقط. بالتّالي العلاقة هنا هي 1:m والتي تنص على أن الجدول الذي يحوي العلاقة m سيضاف اليه خاصيّة جديدة وهي المعرّف الخاص في الجدول ذو العلاقة ١. وهذا يعني أننا سنقوم باضافة userId في جدول Tasks بحيث يشير الى المستخدم الذي قام بانشاء هذه المهمّة. بهذا الشكل ستحوي قاعدة البيانات على البيانات التالية.

- جدول المستخدمين Users
    - اسم المستخدم name
    - بريد الالكتروني email
    - الرمز السّري password
    
- جدول المهام Tasks
    - العنوان Title
    - هل تمّ اتمامها isCompleted
    - من هو المستخدم الذي أنشأها userId


بما أننا انتهينا الآن من تحديد هيكلة قاعدة البيانات، سننتقل مباشرتاً الى انشاء مشروع جديد للبدء في تطوير البرنامج. 




## تطوير مشروع ToDo

في هذا القسم، سنقوم بكتابة الأوامر البرمجيّة وتطوير المشروع بالاضافة الى تدوين كل جزء اضافي نقوم به وشرحه. 

### الخطوات

- انشاء مشروع جديد
![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1672926615216_Screen+Shot+1444-06-12+at+4.50.09+PM.png)

- انشاء الملفّات و المجلّدات الأساسيّة
![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1672926660241_Screen+Shot+1444-06-12+at+4.50.54+PM.png)

- تهيئة المشروع لاستقبال المكتبات من NPM
    npm init -y 
![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1672926717019_Screen+Shot+1444-06-12+at+4.51.51+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1672926748416_Screen+Shot+1444-06-12+at+4.52.24+PM.png)

- اضافة ملف tsconfig لاعدادات لغة Typescript
    tsc --init
![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1672926799150_Screen+Shot+1444-06-12+at+4.53.15+PM.png)

- التحديث على ملف tsconfig 
    "rootDir": "./src", 
    "outDir": "./dist",


- تنزيل المكتبات الأساسيّة 
    npm i express


- تنزيل المكتبات التطويريّة
    npm i -D typescript ts-node @types/node @types/express prisma


- ملف package.json بعد تنزيل المكتبات السّابقة ( الأساسيّة و التطويريّة)
    {
      "name": "todoapp",
      "version": "1.0.0",
      "description": "",
      "main": "index.js",
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
      },
      "keywords": [],
      "author": "",
      "license": "ISC",
      "dependencies": {
        "express": "^4.18.2"
      },
      "devDependencies": {
        "@types/express": "^4.17.15",
        "@types/node": "^18.11.18",
        "prisma": "^4.8.0",
        "ts-node": "^10.9.1",
        "typescript": "^4.9.4"
      }
    }
    


- اعداد ملف app.ts للتعامل مع Express
    import express,{Application, Request, Response} from 'express';
    const app:Application = express();
    const port = 3000;
    app.get('/',(req:Request, res:Response) =>{
        res.status(200).json({msg:"Hello Visitor"})
    });
    app.listen(port, ()=>{
        console.log(`Application running on port ${port}`)
    });
    


- تشغيل البرنامج و اختبار أوّل مسار قمنا ببنائه
    ts-node src/app.ts
![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1672927710678_Screen+Shot+1444-06-12+at+5.08.23+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1672927738774_Screen+Shot+1444-06-12+at+5.08.54+PM.png)


أتممنا الآن عمليّة استخدام Express في المشروع بنجاح، ما نحتاج اليه الآن هو البدء في انشاء قاعدة البيانات والرّبط بها. 


- انشاء قاعدة بيانات 

قمت بانشاء قاعدة بيانات من خلال استخدام MySQL CLI بالأمر التّالي في Terminal

    > mysql -u <username> -p <password>
    > create database TODo;
    > show databases;
![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1672927896021_Screen+Shot+1444-06-12+at+5.11.32+PM.png)

- فتح قاعدة البيانات واستعراضها من خلال TablePlus
![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1672927955767_Screen+Shot+1444-06-12+at+5.12.29+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1672927975928_Screen+Shot+1444-06-12+at+5.12.50+PM.png)


كما نلاحظ، لا توجد أي بيانات لأننا لم نقم بحفظ أي معلومات في قاعدة البيانات بل أنشأناها فقط لنستطيع ربطها مع المشروع لاحقاً.


- لنقم بإغلاق الخادم Server من خلال control + c في mac.


- تهيئة Prisma وانشاء Schema الخاصّة في قاعدة البيانات. 
    - انشاء ملف schema.prisma
    npx prisma init
    هذا الأمر سيقوم بانشاء ملف schema.prisma لكن مع الاعدادات التلقائيّة، وهي ربطه بقاعدة بيانات من النّوع sqlpostgres. لكن نستطيع اضافة خيار كما تنص عليه المقاله هنا، بحيث يكون datasource هو mysql وهي قاعدة البيانات التي سنقوم باستخدامها كالتالي.
    npx prisma init --datasource-provider mysql
![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673172921590_Screen+Shot+1444-06-15+at+1.15.15+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673173172287_Screen+Shot+1444-06-15+at+1.19.28+PM.png)


كما نلاحظ، تمّ ايضاً انشاء ملف .env و اللذي يحوي رابط قاعدة البيانات المتوافق مع صيغة mysql كما قمنا بتحديده في datasource. و ملف .gitignore ليتم تجاهل ملف .env لعدم رفقه بالخطأ على Github.
.env 

    # Environment variables declared in this file are automatically made available to Prisma.
    # See the documentation for more detail: https://pris.ly/d/prisma-schema#accessing-environment-variables-from-the-schema
    # Prisma supports the native connection string format for PostgreSQL, MySQL, SQLite, SQL Server, MongoDB and CockroachDB.
    # See the documentation for all the connection string options: https://pris.ly/d/connection-strings
    DATABASE_URL="mysql://johndoe:randompassword@localhost:3306/mydb"

schema.prisma

    // This is your Prisma schema file,
    // learn more about it in the docs: https://pris.ly/d/prisma-schema
    generator client {
      provider = "prisma-client-js"
    }
    datasource db {
      provider = "mysql"
      url      = env("DATABASE_URL")
    }
    

بهذا الشكل، تمّ انشاء ملف schema.prisma بداخل مجلّد prisma لتحديد هيكلة البيانات. وكما تلاحظ في الصورة من مخرجات الأمر السّابق، قام باخبارانا بما هي الخطوات القادمة و أولها هو تحديث ملف .env لتحديد رابط قاعدة البيانات التي نريد الرّبط به من ثمّ تحديث قاعدة البيانات لتحوي على الهيكلة و أخيراً تحديث prisma client حتّى نستطيع الوصول الى دوال قاعدة البيانات مثل create و findmany وغيرها من الدوّال التي تزودنا بها prisma client.
لكن قبل عمل كل هذا، سنقوم أولاً بتحديد الهيكلة التي نريد قاعدة البيانات أن تتبناها من خلال التحديث على ملف schema.prisma من ثمّ العودة للخطوات التي تمّ ذكرها في مخرجات الأمر السّابق.


- تحديد هيكلة البيانات schema.prisma

schema.prisma

    // This is your Prisma schema file,
    // learn more about it in the docs: https://pris.ly/d/prisma-schema
    generator client {
      provider = "prisma-client-js"
    }
    datasource db {
      provider = "mysql"
      url      = env("DATABASE_URL")
    }
    model User{
      id Int @id @default(autoincrement())
      name String
      email String 
      password String
      tasks Task[]
    }
    model Task{
      id Int @id @default(autoincrement())
      title String 
      isCompleted Boolean @default(false)
      user User @relation(fields: [userId], references: [id])
      userId Int
    }


> ملاحظة: تمّت تسمية model و fields بناءً على قواعد التسمية في Prisma هنا.

بعد أن قمنا الآن بالانتهاء من تحديد هيكلة الجداول و العلاقات فيما بينها، سنقوم باتبّاع الخطوات التي تمّ ذكرها سابقاً.


- الدخول على ملف .env و التحديث على رابط الاتصال بقاعدة البيانات

ملف .env الحالي

    # Environment variables declared in this file are automatically made available to Prisma.
    # See the documentation for more detail: https://pris.ly/d/prisma-schema#accessing-environment-variables-from-the-schema
    # Prisma supports the native connection string format for PostgreSQL, MySQL, SQLite, SQL Server, MongoDB and CockroachDB.
    # See the documentation for all the connection string options: https://pris.ly/d/connection-strings
    DATABASE_URL="mysql://johndoe:randompassword@localhost:3306/mydb"

ملف .env بعد تحديث الرّابط

    # Environment variables declared in this file are automatically made available to Prisma.
    # See the documentation for more detail: https://pris.ly/d/prisma-schema#accessing-environment-variables-from-the-schema
    # Prisma supports the native connection string format for PostgreSQL, MySQL, SQLite, SQL Server, MongoDB and CockroachDB.
    # See the documentation for all the connection string options: https://pris.ly/d/connection-strings
    DATABASE_URL="mysql://root@localhost:3306/ToDo"

اسم المستخدم لدي هو root, كما أنه لا توجد كلمة مرور. و اسم قاعدة البيانات في جهازي هو ToDo كما انشأناها سابقاً. 


- استخدام الأمر `npx prisma db push` لتحديث قاعدة البيانات ليحوي الهيكلة التي قمنا ببنائها والتحقق من الربط مع قاعدة البيانات.


> ملاحظة: في مخرجات الأمر السابق ذكر لنا استخدام الأمر npx prisma db pull لجلب هيكلة البيانات من قاعدة البيانات. لكن في حالتنا الآن لدينا العكس، فالهيكلة متواجدة في ملف schema.prisma وليست في قاعدة البيانات. لذلك سنقوم بنقل الهيكلة من المشروع الى قاعدة البيانات باستخدام الأمر npx prisma db push.



    npx prisma db push
![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673175541815_Screen+Shot+1444-06-15+at+1.58.58+PM.png)


كما نلاحظ من المخرجات، تمّ ايضاً عمل generate لتحديث prisma client. و ان لم يوجد prisma client او لم يتم تنزيل المكتبة، سيتم تنزيلها تلقائيّاً كما سنلاحظ لاحقاً في ملف package.json.

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673175559881_Screen+Shot+1444-06-15+at+1.59.16+PM.png)



- التحقق من قاعدة البيانات ان كانت تحوي الهيكلة ذاتها التي في ملف schema.prisma
![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673176595673_Screen+Shot+1444-06-15+at+2.16.31+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673176626226_Screen+Shot+1444-06-15+at+2.17.01+PM.png)



- انشاء مستخدم جديد في ملف app.ts 

app.ts

    import express,{Application, Request, Response} from 'express';
    const app:Application = express();
    const port = 3000;
    import { PrismaClient } from '@prisma/client'; //importing prisma client
    const prisma = new PrismaClient();
    app.use(express.json()); //ability to read json data format from request object
    app.post('/users',async (req:Request, res:Response) =>{
        try{ //try, if the function works return sucess message
            const user = await prisma.user.create({
                data:{
                    name:req.body.name,
                    email: req.body.email,
                    password: req.body.password
                }
            });
            if(user){
                res.status(200).json({msg:"user created!"})
            }
        }catch(e){ //if there is an error, catch it and print it in the response msg
            res.status(500).json({msg:`Error: ${e}`});
        }
    });
    app.get('/',(req:Request, res:Response) =>{
        res.status(200).json({msg:"Hello Visitor"})
    });
    app.listen(port, ()=>{
        console.log(`Application running on port ${port}`)
    });
    
![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673178367283_Screen+Shot+1444-06-15+at+2.45.59+PM.png)



- اختبار عمليّة اضافة المستخدم

لاختبار عمليّة اضافة مستخدم، يجب علينا تشغيل البرنامج باستخدام الأمر التالي

    ts-node src/app.ts
![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673178681389_Screen+Shot+1444-06-15+at+2.51.14+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673178744478_Screen+Shot+1444-06-15+at+2.52.20+PM.png)



- انشاء مهمّة جديدة في ملف app.ts

app.ts

    import express,{Application, Request, Response} from 'express';
    const app:Application = express();
    const port = 3000;
    import { PrismaClient } from '@prisma/client';
    const prisma = new PrismaClient();
    app.use(express.json());
    app.post('/users',async (req:Request, res:Response) =>{
        try{
            const user = await prisma.user.create({
                data:{
                    name:req.body.name,
                    email: req.body.email,
                    password: req.body.password
                }
            });
            if(user){
                res.status(200).json({msg:"user created!"})
            }
        }catch(e){
            res.status(500).json({msg:`Error: ${e}`});
        }
    });
    app.post('/tasks',async (req:Request, res:Response) =>{
        try{
            const task = await prisma.task.create({
                data:{
                    title:req.body.title,
                    userId: req.body.userId
                }
            });
            if(task){
                res.status(200).json({msg:"task created!"})
            }
        }catch(e){
            res.status(500).json({msg:`Error: ${e}`});
        }
    });
    app.get('/',(req:Request, res:Response) =>{
        res.status(200).json({msg:"Hello Visitor"})
    });
    app.listen(port, ()=>{
        console.log(`Application running on port ${port}`)
    });
    


![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673178863114_Screen+Shot+1444-06-15+at+2.54.19+PM.png)



- اختبار عمليّة اضافة مهمة

تذّكر ان تعيد تشغيل الخادم server قبل اختبار التحديث الجديد الذي قمت باضافته (اضافة مهمّة)

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673179285520_Screen+Shot+1444-06-15+at+3.01.21+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673179305160_Screen+Shot+1444-06-15+at+3.01.39+PM.png)


بعد أن تحققنا من هيكلة قاعدة البيانات وأننا قمنا فعليّاً بتخزين بيانات مستخدمين و مهام، مع اشارة المهمّة للمستخدم الذي قام بانشائها. نستطيع الآن المضي قدماً في تقسيم الملفّات باتبّاع نمط MVC من ثمّ اكمال عمليّات CRUD على البيانات لاحقاً.



- انشاء مجلّد Controller واضافة ملف user.controller and task.controller
![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673183016493_Screen+Shot+1444-06-15+at+4.03.32+PM.png)

- اضافة عمليّات المستخدمين في ملف user.controller.ts


    - نقل عمليّة اضافة المستخدم الى ملف user.controller.ts

user.controller.ts

    import {Request, Response} from 'express';
    import { PrismaClient } from '@prisma/client';
    const prisma = new PrismaClient();
    export const create = async (req:Request, res:Response) =>{
        try{
            const user = await prisma.user.create({
                data:{
                    name:req.body.name,
                    email: req.body.email,
                    password: req.body.password
                }
            });
            if(user){
                res.status(200).json({msg:"user created!"})
            }
        }catch(e){
            res.status(500).json({msg:`Error: ${e}`});
        }
    }
    
    


    - اختبار عمليّة اضافة المستخدم من خلال استدعاء دالّة create من ملف user.controller.ts

app.ts

    import express,{Application, Request, Response} from 'express';
    const app:Application = express();
    const port = 3000;
    import {create} from './controller/user.controller';
    import { PrismaClient } from '@prisma/client';
    const prisma = new PrismaClient();
    app.use(express.json());
    app.post('/users',create);
    app.post('/tasks',async (req:Request, res:Response) =>{
        try{
            const task = await prisma.task.create({
                data:{
                    title:req.body.title,
                    userId: req.body.userId
                }
            });
            if(task){
                res.status(200).json({msg:"task created!"})
            }
        }catch(e){
            res.status(500).json({msg:`Error: ${e}`});
        }
    });
    app.get('/',(req:Request, res:Response) =>{
        res.status(200).json({msg:"Hello Visitor"})
    });
    app.listen(port, ()=>{
        console.log(`Application running on port ${port}`)
    });
    
![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673183242142_Screen+Shot+1444-06-15+at+4.07.18+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673183372711_Screen+Shot+1444-06-15+at+4.09.22+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673183429504_Screen+Shot+1444-06-15+at+4.10.24+PM.png)




- اضافة عمليّات المهام في ملف task.controller.ts
    - نقل عمليّة اضافة مهمة الى ملف task.controller.ts

task.controller.ts

    import {Request, Response} from 'express';
    import { PrismaClient } from '@prisma/client';
    const prisma = new PrismaClient();
    export const create = async (req:Request, res:Response) =>{
        try{
            const task = await prisma.task.create({
                data:{
                    title:req.body.title,
                    userId: req.body.userId
                }
            });
            if(task){
                res.status(200).json({msg:"task created!"})
            }
        }catch(e){
            res.status(500).json({msg:`Error: ${e}`});
        }
    }

 
 

    - اختبار عمليّة اضافة مهمّة من خلال استدعاء دالّة create من ملف task.controller.ts

app.ts

    import express,{Application, Request, Response} from 'express';
    const app:Application = express();
    const port = 3000;
    import {create} from './controller/user.controller';
    import {create as createTask} from './controller/task.controller';
    import { PrismaClient } from '@prisma/client';
    const prisma = new PrismaClient();
    app.use(express.json());
    app.post('/users',create);
    app.post('/tasks',createTask);
    app.get('/',(req:Request, res:Response) =>{
        res.status(200).json({msg:"Hello Visitor"})
    });
    app.listen(port, ()=>{
        console.log(`Application running on port ${port}`)
    });


> ملاحظة: قمنا بتغيير اسم create الى createTask لتشابهها مع دالّة انشاء مستخدم جديد.


![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673184139727_Screen+Shot+1444-06-15+at+4.22.16+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673184288575_Screen+Shot+1444-06-15+at+4.24.44+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673184307459_Screen+Shot+1444-06-15+at+4.25.02+PM.png)




- انشاء مجلّد routes ليحوي المسارات الخاصّة في كلاً من user و task
![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673184706648_Screen+Shot+1444-06-15+at+4.31.42+PM.png)

- اضافة مسارات المستخدم user

user.route.ts

    import express from 'express';
    import {create} from '../controller/user.controller';
    const router = express.Router();
    router.post('/users',create);
    
    export default router;


app.ts (old)

    import express,{Application, Request, Response} from 'express';
    const app:Application = express();
    const port = 3000;
    import {create} from './controller/user.controller';
    import {create as createTask} from './controller/task.controller';
    import { PrismaClient } from '@prisma/client';
    const prisma = new PrismaClient();
    app.use(express.json());
    app.post('/users',create);
    app.post('/tasks',createTask);
    app.get('/',(req:Request, res:Response) =>{
        res.status(200).json({msg:"Hello Visitor"})
    });
    app.listen(port, ()=>{
        console.log(`Application running on port ${port}`)
    });

أعلاه، هو ملف app.ts الحالي. سنقوم الآن بحذف جميع الأجزاء التي لا نحتاج اليها والتي تمّ استخراجها في ملفّات خارجيّة حتى لا يتكوّن تكرار للكود، بحيث تظهر النتيجة التالية:
app.ts (new)

    import express,{Application, Request, Response} from 'express';
    const app:Application = express();
    const port = 3000;
    import {create as createTask} from './controller/task.controller';
    
    app.use(express.json());
    app.post('/tasks',createTask);
    app.get('/',(req:Request, res:Response) =>{
        res.status(200).json({msg:"Hello Visitor"})
    });
    app.listen(port, ()=>{
        console.log(`Application running on port ${port}`)
    });
    


- اضافة مسارات المهام task

task.route.ts

    import express from 'express';
    import {create} from '../controller/task.controller';
    const router = express.Router();
    router.post('/tasks',create);
    
    export default router;


app.ts (old)

    import express,{Application, Request, Response} from 'express';
    const app:Application = express();
    const port = 3000;
    import {create as createTask} from './controller/task.controller';
    
    app.use(express.json());
    app.post('/tasks',createTask);
    app.get('/',(req:Request, res:Response) =>{
        res.status(200).json({msg:"Hello Visitor"})
    });
    app.listen(port, ()=>{
        console.log(`Application running on port ${port}`)
    });
    

app.ts (new)

    import express,{Application, Request, Response} from 'express';
    const app:Application = express();
    const port = 3000;
    app.use(express.json());
    
    app.get('/',(req:Request, res:Response) =>{
        res.status(200).json({msg:"Hello Visitor"})
    });
    app.listen(port, ()=>{
        console.log(`Application running on port ${port}`)
    });
    

سنقوم بحذف المسار `/` لعدم حاجتنا له حاليّاً، مع حذف استدعاؤنا لنوع Request and Response بحيث يكون آخر اصدار لملف app.ts هو التالي.
app.ts

    import express,{Application} from 'express';
    const app:Application = express();
    const port = 3000;
    app.use(express.json());
    
    app.listen(port, ()=>{
        console.log(`Application running on port ${port}`)
    });



- اعادة ضبط المسارات في ملف app.ts

سنقوم باستدعاء router الذي قمنا ببنائه في ملف مسار المستخدمين و المهام لايتم التحقق من أي طلب يأتي من قبل المستخدمين من ثمّ توجيهه نحو الملف (المسار) المطابق من ثمّ تنفيذ العمليّة المطلوبة بشكل صحيح. 

app.ts

    import express,{Application} from 'express';
    import userRouter from './routes/user.route';
    import taskRouter from './routes/task.route';
    const app:Application = express();
    const port = 3000;
    app.use(express.json());
    app.use('/',userRouter);
    app.use('/',taskRouter);
    app.listen(port, ()=>{
        console.log(`Application running on port ${port}`)
    });
    


- اختبار المشروع و التأكد من خلوة من الأخطاء
![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673186070300_Screen+Shot+1444-06-15+at+4.54.25+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673186157868_Screen+Shot+1444-06-15+at+4.55.50+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673186137214_Screen+Shot+1444-06-15+at+4.55.33+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673186173502_Screen+Shot+1444-06-15+at+4.56.07+PM.png)

- تحسين عمليّة routing

نلاحظ من ملف app.ts أن هنالك مسارين وهما كالتالي.

    import express,{Application} from 'express';
    import userRouter from './routes/user.route';
    import taskRouter from './routes/task.route';
    const app:Application = express();
    const port = 3000;
    app.use(express.json());
    app.use('/',userRouter);
    app.use('/',taskRouter);
    app.listen(port, ()=>{
        console.log(`Application running on port ${port}`)
    });
    

جميعهم مسار `/` والذي يعني، ان اتى طلب يحوي المسار `/` في اسم الرّابط، قم بتوجيهه لقسم مسارات المستخدمين.
كمثال، المسار  `users/`  يحوي `/` في بدايته، لذلك سيتم مباشرتاً توجيهه الى مسارات المستخدمين. من ثمّ التحقق من المسارات الموجودة في ملف user.route.ts وهو مسار `users` من النّوع get. ان تطابقت المسارات سيقوم بتشغيل العمليّة التي يحويها، وهذه هي العمليّة الصحيحة. لكن ما لم ننتبه له هو ان اتى مسار من نوع get بالاسم `tasks/` على التطبيق. 

الترتيب الموضّح في ملف app.ts هو ان يتم التحقق من المسار ان كان متواجد في users اولاً عند تواجد `/` في بداية اسم المسار. ان لم يتواجد، قم باختبار مسارات المهام task.route.ts. ،هذا يعني أن المسار `tasks/` سيتم اختبار توافقه مع جميع مسارات users بسبب توافق أول خانة وهي احتوائه على `/` بداية اسم المسار. ان لم يتوافق المسار المطلوب مع أي مسار في قسم users سيتم تحويله الى مسارات المهام بسبب توافقه ايضاً مع أول خانة وهي `/`. 

كما نلاحظ من الحالة الثانية، سيتم استنزاف وقت في البحث عن المسار بسبب تشابههم، بينما نستطيع تحسين الكود حتّى يتم تحويل الطلب على المسارات الصحيحة بشكل مباشر دون الخوض في البحث عن التطابق لجميع المسارات المتوفرة. و ذلك من خلال تحديد أن أي مسار يبدأ بالاسم users سيتم توجيهه لمسارات المستخدمين، بينما أي مسار يبدأ بالاسم tasks يتم توجيهه الى مسارات المهام كالتّالي. 

app.ts

    import express,{Application} from 'express';
    import userRouter from './routes/user.route';
    import taskRouter from './routes/task.route';
    const app:Application = express();
    const port = 3000;
    app.use(express.json());
    app.use('/users',userRouter);
    app.use('/tasks',taskRouter);
    app.listen(port, ()=>{
        console.log(`Application running on port ${port}`)
    });
    

بهذا الشكل سنحل المشكلة، لكن يجب علينا تحديث المسارات في ملف user.route and task.route،

user.route.ts

    import express from 'express';
    import {create} from '../controller/user.controller';
    const router = express.Router();
    router.post('/',create);
    
    export default router;


> ملاحظة: قمنا بازالة كلمة users لأننا اضفناها في ملف app.ts, ان لم نقم بازالتها سيكون رابط اضافة مستخدم كالتالي  `users/users/`. لأنه اولاً سيقراء users في ملف app.ts من ثمّ سيحوّله الى ملف user.route.ts من ثمّ سيقراء users مرّة أخرى ليستطيع تنفيذ عمليّة الاضافة. لكن الآن سيقراء users في ملف app.ts من ثمّ يقراء / في ملف user.route.ts. لذلك ستبقى عمليّة الاضافة بنفس مسمّى الرابط الذي كنا نستقبله سابقاً.

task.route.ts

    import express from 'express';
    import {create} from '../controller/task.controller';
    const router = express.Router();
    router.post('/',create);
    
    export default router;


> ملاحظة: تستطيع ايضاً انشاء ملف في مجلّد routes بالاسم index.route.ts ليحوي جميع المسارات بحيث يتبقى فقط app.use('/',indexRoutes) في ملف app.ts.


- اختبار المسارات مرّة أخرى
![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673187593657_Screen+Shot+1444-06-15+at+5.19.49+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673187581828_Screen+Shot+1444-06-15+at+5.19.37+PM.png)



- انشاء مجلّد config 

نلاحظ أنه في كلاً من user.controller و task.controller تكرار لانشاء prisma client. بينما نحن بحاجة الى انشاء prisma client واحد فقط واستخدامة مع كل models المتواجدة في المشروع، لذلك عوضاً عن تكرارنا للكود. سنقوم باستخراج الجزء المتكرر من الكود في ملف خارجي واستدعاؤه لدى حاجتنا له. و سنقوم بذلك من خلال انشاء ملف بالاسم db.ts في مجلّد جديد بالاسم config ليحوي prisma client

user.controller.ts

    import {Request, Response} from 'express';
    import { PrismaClient } from '@prisma/client';
    const prisma = new PrismaClient();
    export const create = async (req:Request, res:Response) =>{
        try{
            const user = await prisma.user.create({
                data:{
                    name:req.body.name,
                    email: req.body.email,
                    password: req.body.password
                }
            });
            if(user){
                res.status(200).json({msg:"user created!"})
            }
        }catch(e){
            res.status(500).json({msg:`Error: ${e}`});
        }
    }
    

task.controller.ts

    import {Request, Response} from 'express';
    import { PrismaClient } from '@prisma/client';
    const prisma = new PrismaClient();
    export const create = async (req:Request, res:Response) =>{
        try{
            const task = await prisma.task.create({
                data:{
                    title:req.body.title,
                    userId: req.body.userId
                }
            });
            if(task){
                res.status(200).json({msg:"task created!"})
            }
        }catch(e){
            res.status(500).json({msg:`Error: ${e}`});
        }
    }
    

 
كما ذكرنا، سنقوم باستخدام prisma client في الملفان أعلاه الى ملف جديد بالاسم db.ts.

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673254275611_Screen+Shot+1444-06-16+at+11.51.09+AM.png)


الآن، سنقوم بانشاء prisma client في ملف db.ts
db.ts

    import { PrismaClient } from '@prisma/client';
    const prisma = new PrismaClient();
    
    export default prisma;

الآن، نستطيع استخدام prisma client الذي انشأناه في ملف db.ts في أي ملف آخر بمجرّد استدعاؤنا له، لذلك سنقوم بحذف prisma client المنشئة في ملف user.controller.ts و task.controller.ts بحيث تكون لدينا النتيجة التاليّة:

user.controller.ts

    import {Request, Response} from 'express';
    import prisma from '../config/db';
    
    export const create = async (req:Request, res:Response) =>{
        try{
            const user = await prisma.user.create({
                data:{
                    name:req.body.name,
                    email: req.body.email,
                    password: req.body.password
                }
            });
            if(user){
                res.status(200).json({msg:"user created!"})
            }
        }catch(e){
            res.status(500).json({msg:`Error: ${e}`});
        }
    }
    

task.controller.ts

    import {Request, Response} from 'express';
    import prisma from '../config/db';
    
    export const create = async (req:Request, res:Response) =>{
        try{
            const task = await prisma.task.create({
                data:{
                    title:req.body.title,
                    userId: req.body.userId
                }
            });
            if(task){
                res.status(200).json({msg:"task created!"})
            }
        }catch(e){
            res.status(500).json({msg:`Error: ${e}`});
        }
    }
    



> ملاحظة: تستطيع اضافة خيارات الى prisma client بحيث تجعله يقوم بطباعة جميع الاستعلامات queries التي ارسلها الى قاعدة البيانات من خلال query، او طباعة errors بشكل جمالي من خلال استخدام pretty وغيرها من الخيارات. وكمثال عليها الصورة التالية.
![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673268542766_Screen+Shot+1444-06-16+at+3.48.56+PM.png)




- اضافة nodemon

سابقاً، عندما نقوم بأي تحديث على المشروع. كان يجب علينا اعادة تشغيل الخادم server. مما يستغرق بعض الوقت، كما قد ننسى فترات اعادة تشغيله ونظن أننا لم نقم بالتحديث او ان هنالك مشكلة في الكود بينما المشكلة فقط هي نسياننا اعادة تشغيل الخادم. لذلك، سنتعرّف هنا على مكتبة nodemon، وهي مكتبة تساعدنا في مرحلة التطوير. بحيث تكون مسؤوله عن مراقبة التحديثات على المشروع واعادة تشغيل الخادم مع كل تحديث يتم عملة.


    - تنزيل مكتبة nodemon
    npm i -D nodemon

بعد تنزيل المكتبة، سنقوم باستخدامها لتشغيل ملف app.ts من خلال كتابة script في ملف package.json
package.json

    {
      "name": "todoapp",
      "version": "1.0.0",
      "description": "",
      "main": "index.js",
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "dev": "nodemon src/app.ts"
      },
      "keywords": [],
      "author": "",
      "license": "ISC",
      "dependencies": {
        "@prisma/client": "^4.8.0",
        "express": "^4.18.2"
      },
      "devDependencies": {
        "@types/express": "^4.17.15",
        "@types/node": "^18.11.18",
        "nodemon": "^2.0.20",
        "prisma": "^4.8.0",
        "ts-node": "^10.9.1",
        "typescript": "^4.9.4"
      }
    }
    
![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673260177925_Screen+Shot+1444-06-16+at+1.29.31+PM.png)


الآن، يمكننا تشغيل الملف باستخدام nodemon عن طريقة استخدام الأمر npm run dev


    - تشغيل الخادم
    npm run dev
![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673260281931_Screen+Shot+1444-06-16+at+1.31.16+PM.png)


بهذا الشكل، اصبح الخادم يعمل على المنفذ 3000، كما أنه يستجيب لأي تحديث على المشروع من خلال اعادة تشغيل الخادم.


- استخدام ملف env. لتخزين معلومات أخرى

يستخدم عادتاً ملف .env لتخزين المعلومات الحساسة او التابعة لبيئة العمل الخاصّة بك، كما لاحظنا في رابط قاعدة البيانات المستخدم من قبل prisma. 

هل تظن ان هنالك بيانات نحتاج الى تخزينها في ملف .env؟ 

رقم port في ملف app.ts قد يعتبر من البيانات الحسّاسة و التي لا نريد مشاركتها عند رفع ملفّات المشروع على gihtub او مشاركته بأي وسيله. لذلك، سنقوم بتخزين رقم port في ملف .env، لكن قبل ذلك يجب علينا تنزيل مكتبة dotenv من npm لتسمح لنا بتخزين البيانات في الملف واستخدامه خارجها. 


> ملاحظة: تقوم prisma باستخدام طريقة مختلفة في تخزين معلومات رابط قاعدة البيانات، لذلك لا تحتاج الى مكتبة dotenv.


    - تنزيل مكتبة dotenv
    npm i dotenv


    - تخزين قيمة PORT في ملف .env
    # Environment variables declared in this file are automatically made available to Prisma.
    # See the documentation for more detail: https://pris.ly/d/prisma-schema#accessing-environment-variables-from-the-schema
    # Prisma supports the native connection string format for PostgreSQL, MySQL, SQLite, SQL Server, MongoDB and CockroachDB.
    # See the documentation for all the connection string options: https://pris.ly/d/connection-strings
    DATABASE_URL="mysql://root@localhost:3306/ToDo"
    PORT= 3000


    - استدعاء مكتبة dotenv في ملف app.ts
    - استدعاء قيمة PORT في ملف app.ts 

app.ts

    import express,{Application} from 'express';
    import userRouter from './routes/user.route';
    import taskRouter from './routes/task.route';
    import * as dotenv from 'dotenv';
    dotenv.config()
    const app:Application = express();
    const port = process.env.PORT;
    app.use(express.json());
    app.use('/users',userRouter);
    app.use('/tasks',taskRouter);
    app.listen(port, ()=>{
        console.log(`Application running on port ${port}`)
    });

النتيجة بعد تشغيل البرنامج 

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673261101465_Screen+Shot+1444-06-16+at+1.44.56+PM.png)



اضافة قيمة أخرى في حالة لم توجد قيمة للمنفذ او فشل البرنامج من استخدام قيمة PORT المتواجد في ملف .env
app.ts

    import express,{Application} from 'express';
    import userRouter from './routes/user.route';
    import taskRouter from './routes/task.route';
    import * as dotenv from 'dotenv';
    dotenv.config()
    const app:Application = express();
    const port = process.env.PORT || 3000;
    app.use(express.json());
    app.use('/users',userRouter);
    app.use('/tasks',taskRouter);
    app.listen(port, ()=>{
        console.log(`Application running on port ${port}`)
    });



- انشاء بقيّة عمليّات المستخدمين والمهام
    - المستخدمين 
        - اضافة مستخدم

لا نحتاج الى الآن اضافة أي عمليّة أخرى، فقط قابليّة المستخدم على انشاء حساب (اضافة مستخدم)


    - المهام
        - اضافة مهمّة من قبل مستخدم Create
        - عرض جميع المهام لكل مستخدم Read
        - تحديث بيانات أحد المهام من قبل المستخدم Update
        - حذف مهمّة من قبل المستخدم Delete
    أعلاه، العمليّات الخاصّة بالمهام، وكما تلاحظون. لم نقم سوى بعمليّة واحدة وهي انشاء مهمّة. بالتّالي سنقوم باضافة بقيّة العمليّات على المهام.

READ
task.controller.ts

    import {Request, Response} from 'express';
    import prisma from '../config/db';
    export const create = async (req:Request, res:Response) =>{
        try{
            const task = await prisma.task.create({
                data:{
                    title:req.body.title,
                    userId: req.body.userId
                }
            });
            if(task){
                res.status(200).json({msg:"task created!"})
            }
        }catch(e){
            res.status(500).json({msg:`Error: ${e}`});
        }
    }
    export const findAllByUser = async (req:Request, res:Response) =>{
        try{
            const tasks = await prisma.task.findMany({
                where: {
                    userId: parseInt(req.params.id) //User id
                }
            });
            res.status(200).json(tasks)
        }catch(e){
            res.status(500).json({msg:`Error: ${e}`});
        }
    }
    
    

task.route.ts

    import express from 'express';
    import {create, findAllByUser} from '../controller/task.controller';
    const router = express.Router();
    router.get('/:id',findAllByUser);
    router.post('/',create);
    
    export default router;

test the operation

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673268686209_Screen+Shot+1444-06-16+at+3.51.22+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673268717891_Screen+Shot+1444-06-16+at+3.51.53+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673268738264_Screen+Shot+1444-06-16+at+3.52.12+PM.png)


UPDATE
task.controller.ts

    import {Request, Response} from 'express';
    import prisma from '../config/db';
    export const create = async (req:Request, res:Response) =>{
        try{
            const task = await prisma.task.create({
                data:{
                    title:req.body.title,
                    userId: req.body.userId
                }
            });
            if(task){
                res.status(200).json({msg:"task created!"})
            }
        }catch(e){
            res.status(500).json({msg:`Error: ${e}`});
        }
    }
    export const findAllByUser = async (req:Request, res:Response) =>{
        try{
            const tasks = await prisma.task.findMany({
                where: {
                    userId: parseInt(req.params.id) //User id
                }
            });
            res.status(200).json(tasks)
        }catch(e){
            res.status(500).json({msg:`Error: ${e}`});
        }
    }
    export const update = async (req:Request, res:Response) =>{
        try{
            const updatedTasks = await prisma.task.updateMany({
                where:{
                    id: parseInt(req.params.id), //Task id
                    userId: req.body.userId
                },
                data:{
                    title: req.body.title,
                    isCompleted: req.body.isCompleted
                }
            });
            if(updatedTasks.count == 0){
                throw("No Task Updated");
            }
            res.status(200).json({msg:"task updated!"});
            
        }catch(e){
            res.status(500).json({msg:`Error: ${e}`});
        }
    }


> ملاحظة: لتحديث مهمّة ما، يجب علينا التحقق من ان كان userid الذي يحاول تحديث المهمّة نفس قيمة userid الخاص بمنشئ المهمّة، لاثبات ان الشخص الذي انشأها هو الشخص الذي يريد تحديثها. 
> و بما أن Prisma لا تدعم ان نقوم بالتحقق من خانة ليست unique في where بداخل عمليّة التحديث (userId). يتبقى لنا خياران. 
> ١. البحث عن المهمّة باستخدام find مع تحديد todo id and userid لعمل فرز على البيانات من ثمّ عمل update  لها بعد التحقق من أن id للمستخدم متماثل. 
> ٢. استخدام دالّة updateMany لعمل فرز على مجموعة من المهام والمخرج سيكون مهمّة واحدة مطابقة للشروط.
> (مصدر)

task.route.ts

    import express from 'express';
    import {create, findAllByUser, update} from '../controller/task.controller';
    const router = express.Router();
    router.get('/:id',findAllByUser);
    router.post('/',create);
    router.put('/:id',update);
    
    export default router;

test the operation

![current database values](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673271906010_Screen+Shot+1444-06-16+at+4.44.58+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673272004330_Screen+Shot+1444-06-16+at+4.46.37+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673272024517_Screen+Shot+1444-06-16+at+4.47.00+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673272042402_Screen+Shot+1444-06-16+at+4.47.17+PM.png)



DELETE
task.controller.ts

    import {Request, Response} from 'express';
    import prisma from '../config/db';
    export const create = async (req:Request, res:Response) =>{
        try{
            const task = await prisma.task.create({
                data:{
                    title:req.body.title,
                    userId: req.body.userId
                }
            });
            if(task){
                res.status(200).json({msg:"task created!"})
            }
        }catch(e){
            res.status(500).json({msg:`Error: ${e}`});
        }
    }
    export const findAllByUser = async (req:Request, res:Response) =>{
        try{
            const tasks = await prisma.task.findMany({
                where: {
                    userId: parseInt(req.params.id) //User id
                }
            });
            res.status(200).json(tasks)
        }catch(e){
            res.status(500).json({msg:`Error: ${e}`});
        }
    }
    export const update = async (req:Request, res:Response) =>{
        try{
            const updatedTasks = await prisma.task.updateMany({
                where:{
                    id: parseInt(req.params.id), //Task id
                    userId: req.body.userId
                },
                data:{
                    title: req.body.title,
                    isCompleted: req.body.isCompleted
                }
            });
            if(updatedTasks.count == 0){
                throw("No Task Updated");
            }
            res.status(200).json({msg:"task updated!"});
            
        }catch(e){
            res.status(500).json({msg:`Error: ${e}`});
        }
    }
    export const deleteById = async (req:Request, res:Response) =>{
        try{
            const deletedTasks = await prisma.task.deleteMany({
                where:{
                    id: parseInt(req.params.id), //Task id
                    userId: req.body.userId
                }
            });
            if(deletedTasks.count == 0){
                throw("No Task Deleted");
            }
            res.status(200).json({msg:"task deleted!"});
            
        }catch(e){
            res.status(500).json({msg:`Error: ${e}`});
        }
    }

task.route.ts

    import express from 'express';
    import {create, findAllByUser, update, deleteById} from '../controller/task.controller';
    const router = express.Router();
    router.get('/:id',findAllByUser);
    router.post('/',create);
    router.put('/:id',update);
    router.delete('/:id',deleteById);
    
    export default router;


> ملاحظة: من الممكن استخدام return قبل res.send للخروج من الدّالة نهائيّاً و عدم تنفيذ أي كود بعد return بعكس استخدام res.send والذي يقوم باعادة response للمستخدم من ثمّ تنفيذ الكود الذي يليه.

test the operation

![current database](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673272042402_Screen+Shot+1444-06-16+at+4.47.17+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673272304262_Screen+Shot+1444-06-16+at+4.51.39+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673272333254_Screen+Shot+1444-06-16+at+4.52.08+PM.png)

![database after deletion](https://paper-attachments.dropboxusercontent.com/s_7D2AC24AFD12594130E7FF64CA47356C0FCD2C9065863FB787E40EA169726487_1673272405147_Screen+Shot+1444-06-16+at+4.53.17+PM.png)



و هكذا، قمنا بالانتهاء من بناء تطبيق ToDo بسيط يقوم باستقبال طلبات من المستخدمين والاستعلام من قاعدة البيانات بناءً عليها. كما تعلّمنا طريقة استخدام ايطار العمل Express باستخدام لغة Typescript مع الرّبط بقاعدة البيانات من النّوع SQL باستخدام Prisma ORM.


مصادر

رابط الكود على GitHub 
