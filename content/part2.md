# Project Base Tutorial part 2

سنقوم في هذا الجزء باكمال المشروع السّابق من خلال تغطية بعض المفاهيم الإضافيّة و التحسينات الممكنه كالتالي.

- تحسين هيكلة البيانات ان وجد.
- اضافة Validation لاختبار البيانات المدخلة من قبل المستخدمين.
- تشفير كلمة المرور في قاعدة البيانات والتحقق منها.
- اضافة تسجيل دخول للمستخدم عن طريق استخدام JWT.


----------

- تحسينات هيكلة البيانات.

في ملف schema.prisma ستجد الكود التّالي،
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

هل تجد أي تحسينات ممكنه على الهيكلة؟ بالطبع توجد العديد من التحسينات الممكنة التي لم نقم بالتركيز عليها سابقاً، لذلك دعونا نخوض في هذه التحسينات مباشرةً وهي كالآتي. 

    - خانة email يجب أن تكون فريدة وغير متكرره، بحيث لا يستطيع شخصان لهم نفس البريد الالكتروني التسجيل في التطبيق.
    model User{
      id Int @id @default(autoincrement())
      name String
      email String @unique
      password String
      tasks Task[]
    }
    
    - تغيير نوع id للمستخدمين والمهام بحيث يكون عشوائي و معقّد أكثر (لا يتم تبؤه بسهوله) باستخدام uuid.
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
      id String @id @default(uuid())
      name String
      email String @unique
      password String
      tasks Task[]
    }
    model Task{
      id String @id @default(uuid())
      title String 
      isCompleted Boolean @default(false)
      user User @relation(fields: [userId], references: [id])
      userId String
    }
    



- اضافة أنواع من المستخدمين لنتمكّن لاحقاً من تطبيق شروط على بعض العمليّات بحيث لا تتم سوى لنوع محدد من المستخدمين ( ملاحظة: هذه الخطوة غير ضروريّة تماماً لكن نريد فقط هنا استخدام مفهوم enum لتحديد Roles للمستخدمين)

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
      id String @id @default(uuid())
      name String
      email String @unique
      password String
      role Role @default(User)
      tasks Task[]
    }
    model Task{
      id String @id @default(uuid())
      title String 
      isCompleted Boolean @default(false)
      user User @relation(fields: [userId], references: [id])
      userId String
    }
    
    enum Role{
      User
      Admin
    }

لإختبار التحديثات التي تمّت اضافتها على هيكلة البيانات في schema.prisma يجب علينا أولاً رفع هذه التحديثات على قاعدة البيانات كالتالي.

    npx prisma db push
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1673792347614_Screen+Shot+1444-06-22+at+5.19.01+PM.png)


لأننا قمنا بتحديثات تعتبر نوعاً ما جوهرية، قامت prisma باظهار تحديث عن احتمالية خسارة بعض البيانات. كمثال، قمنا سابقاً بتحديد خانة البريد بأنها فريدة وغير متكررة. لذلك ان وجد تكرار قد يتم خسارة أحد البيانات التي وجدها.
سنقوم باختيار yes بحيث نتجاهل الانذارات ونتابع عمليّة رفع التحديثات على قاعدة البيانات.

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1673792534647_Screen+Shot+1444-06-22+at+5.22.09+PM.png)


قاعدة البيانات بعد التحديث:

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1673792554341_Screen+Shot+1444-06-22+at+5.22.28+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1673792581398_Screen+Shot+1444-06-22+at+5.22.57+PM.png)


الآن، لنقم بتشغيل البرنامج بعد تطبيق التحديثات. 

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1673792734457_Screen+Shot+1444-06-22+at+5.25.30+PM.png)


نلاحظ تواجد بعض المشاكل/ الأخطاء. وجميع هذه الأخطاء متواجدة في ملف task.controller.ts. فكما تنص، أننا قمنا سباقاً بتحويل id القادم من الطلب من string الى int لأنه كان سابقاً رقم، لكن الآن أصبح string لذلك، لم نعد بحاجة الى التحويل ونستطيع حذف العمليّة بحيث يكون ملف عمليّات المهام كالتّالي. 
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
                    userId: req.params.id //User id
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
                    id: req.params.id, //Task id
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
                    id: req.params.id, //Task id
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

لنقم الآن باعادة تشغيل الخادم/ البرنامج

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1673792919802_Screen+Shot+1444-06-22+at+5.28.35+PM.png)


وهكذا، قمنا بتحديث قاعدة البيانات وتصحيح الأوامر البرمجيّة السابقة بنجاح.
add new user

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1673792971794_Screen+Shot+1444-06-22+at+5.29.25+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1673793031872_Screen+Shot+1444-06-22+at+5.30.24+PM.png)


add new task by the new user

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1673793285093_Screen+Shot+1444-06-22+at+5.34.38+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1673793324853_Screen+Shot+1444-06-22+at+5.35.20+PM.png)


get all task created by the new user

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1673793394903_Screen+Shot+1444-06-22+at+5.36.29+PM.png)




----------



- اضافة Validation لاختبار البيانات المدخلة من قبل المستخدمين.

سنقوم في هذا الجزء باستخدام zod للتحقق من مدخلات المستخدمين. لذلك دعونا نقوم باتبّاع الخطوات التالية لاضافة zod على المشروع.


    - تنزيل مكتبة zod
    npm i zod

بعد ذلك، سنقوم باتبّاع البعض من تعليمات مقالة zod الرسميّة للتعرّف على zod وطريقة استخدامه.

    - انشاء مجلّد zodSchema من ثمّ انشاء ملف لنختبر zod من خلالة كالتالي.


![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1673963516489_Screen+Shot+1444-06-24+at+4.51.48+PM.png)



    - انشاء أوّل schema باستخدام zod.

test.schema.ts

    import {z} from 'zod';
    const text = z.string();
    console.log(text.parse("test"));
    console.log(text.parse(33));

قمنا أعلاه بانشاء ثابت من النّوع string، والذي يعني ان هذه الهيكلة التّباعة للمتغيّر text لا تستقبل سوى نص. من ثمّ قمنا بتمرير واختبار نوعين، النّوع الأول نص والذي من المفترض أن يكون صحيح و النوع الآخر رقم والذي من المفترض ان يعيد Error كما سنلاحظ في النتيجة التالية.

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1673964286420_Screen+Shot+1444-06-24+at+5.04.38+PM.png)


كم لاحظنا، قام بتمرير القيمة الأولى بنجاح لأنها توافق النّوع وقت الاختبار. بينما لم يتوافق النّوع الآخر لذلك ظهرت لنا مشكلة. 

نستطيع ايضاً انشاء object وتحديد أنواع المدخلات بداخله كالتالي.
test.schema.ts

    import {z} from 'zod';
    const user = z.object({
        name: z.string().min(2),
        age: z.number(),
        email: z.string().email()
    })
    let user1 = {
        name: "sara",
        age: 22,
        email: "sara@test.com"
    }
    let user2 = {
        name: "t",
        age: 10,
        email: "t@test.com"
    }
    let user3 = {
        name: "rami",
        age: 29,
        email: "test.com"
    }
    console.log(user.parse(user1));
    console.log(user.parse(user2));
    console.log(user.parse(user3));
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1673964719912_Screen+Shot+1444-06-24+at+5.11.53+PM+2.png)


نلاحظ ظهور خطأ في user2 بسبب كون الاسم أقل من حرفين. 

test.schema.ts

    import {z} from 'zod';
    const user = z.object({
        name: z.string().min(2),
        age: z.number(),
        email: z.string().email()
    })
    let user1 = {
        name: "sara",
        age: 22,
        email: "sara@test.com"
    }
    let user2 = {
        name: "t",
        age: 10,
        email: "t@test.com"
    }
    let user3 = {
        name: "rami",
        age: 29,
        email: "test.com"
    }
    console.log(user.parse(user1));
    //console.log(user.parse(user2));
    console.log(user.parse(user3));
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1673964825561_Screen+Shot+1444-06-24+at+5.13.39+PM+2.png)


نلاحظ أعلاه ايضاً ان الاختبار على user3 لم ينجح بسبب مخالفة email الشرط في الهيكلة، وهو أن يكون بصيغة بريد الكتروني صحيح. 


> ملاحظة:  كل مدخل تمّ تعريفة في  zod object يعتبر تلقائيّاً متطلّب الزامي وليس اختياري، لذلك ان اردت اضافة خيار اختياري وتحديد هيكلته المتوقّعة قم باستخدام دالّة .optional()

بما أننا تعرّفنا على zod، سنقوم بحذف ملف test.schema.ts لعدم حاجتنا له.

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674031450563_Screen+Shot+1444-06-25+at+11.44.06+AM.png)


اصبح مجلّد zodSchema فارغ. 



    - اختبار البيانات المدخلة لعمليّات المهام

لنبدأ بالعمل الحقيقي، وهو اختبار البيانات المستقبلة من request body & params. ولاختبار هذه المدخلات سنقوم أولاً بانشاء ملف task.schema.ts بحيث يحوي هيكلة البيانات المتوقّعة.

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674031781952_Screen+Shot+1444-06-25+at+11.49.38+AM.png)




    - اضافة اختبار البيانات المدخلة لعمليّة اضافة مهمّة جديدة.

في عمليّة انشاء مهمّة جديدة، سنقوم باستقبال البينات من body فقط. لذلك سنقوم باختبار هذه البيانات كالتّالي.
task.schema.ts

    import {z} from 'zod';
    const createSchema = z.object({
        body: z.object({
            title: z.string().min(2),
            userId: z.string().uuid()
        })
    });

أعلاه، هو ابسط شكل ممكن للهيكلة، لكن قد نحتاج الى اضافة مفاهيم أخرى. أولها هو عمل export للهيكلة لنستطيع استخدامها خارج الملف لاحقاً. ثانياً، سنقوم باضافة رسالة مخصصة لكل مدخل بحيث تُعرض وقت ظهور خطأ في نوع البيانات المدخلة invalid datatype او نسيان ادخالها required error.

task.schema.ts

    import {z} from 'zod';
    export const createSchema = z.object({
        body: z.object({
            title: z.string({
                required_error: "Title is required",
                invalid_type_error: "Title must be string"
            }).min(2, "Title must consist of 2 or more letters"),
            userId: z.string({
                required_error: "userId is required",
                invalid_type_error: "userId must be string"
            }).uuid()
        })
    });



    - اضافة اختبار البيانات المدخلة لعمليّة تحديث مهمّة سابقة.

task.schema.ts

    import {z} from 'zod';
    export const createSchema = z.object({
        body: z.object({
            title: z.string({
                required_error: "Title is required",
                invalid_type_error: "Title must be string"
            }).min(2, "Title must consist of 2 or more letters"),
            userId: z.string({
                required_error: "userId is required",
                invalid_type_error: "userId must be string"
            }).uuid()
        })
    });
    export const updateSchema = z.object({
        body: z.object({
            title: z.string({
                required_error: "Title is required",
                invalid_type_error: "Title must be string"
            }).min(2, "Title must consist of 2 or more letters"),
            userId: z.string({
                required_error: "userId is required",
                invalid_type_error: "userId must be string"
            }).uuid(),
            isCompleted: z.boolean().optional()
        }),
        params: z.object({
            id: z.string({
                required_error: "id is required",
                invalid_type_error: "id must be string"
            }).uuid()
        })
    });


    - اضافة اختبار البيانات المدخلة لعمليّة حذف مهمّة سابقة.

task.schema.ts

    import {z} from 'zod';
    export const createSchema = z.object({
        body: z.object({
            title: z.string({
                required_error: "Title is required",
                invalid_type_error: "Title must be string"
            }).min(2, "Title must consist of 2 or more letters"),
            userId: z.string({
                required_error: "userId is required",
                invalid_type_error: "userId must be string"
            }).uuid()
        })
    });
    export const updateSchema = z.object({
        body: z.object({
            title: z.string({
                required_error: "Title is required",
                invalid_type_error: "Title must be string"
            }).min(2, "Title must consist of 2 or more letters"),
            userId: z.string({
                required_error: "userId is required",
                invalid_type_error: "userId must be string"
            }).uuid(),
            isCompleted: z.boolean().optional()
        }),
        params: z.object({
            id: z.string({
                required_error: "id is required",
                invalid_type_error: "id must be string"
            }).uuid()
        })
    });
    export const deleteSchema = z.object({
        body: z.object({
            userId: z.string({
                required_error: "userId is required",
                invalid_type_error: "userId must be string"
            }).uuid()
        }),
        params: z.object({
            id: z.string({
                required_error: "id is required",
                invalid_type_error: "id must be string"
            }).uuid()
        })
    });


> ملاحظة: سنقوم بتجنّب اختبار عرض جميع المهام الآن لأنه لا يوجد سوى التحقق من userId في params  والذي سنقوم باستبداله لاحقاً بحيث نقوم بتخزين userId مع تسجيل دخول المستخدم من ثمّ تخزينه واستخدامه دائماً دون الحاجة لتمريره في params.


    - اختبار بيانات اضافة المستخدمين (registration)

سنقوم باختبار بيانات المستخدمين المدخلة من خلال انشاء ملف user.schema.ts من ثمّ تعريف z.object ليحوي الهيكلة الصحيحة/المتوقعة من المستخدم لتتم العمليّة بنجاح كالتالي. 


        - انشاء ملف user.schema.ts
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674034183899_Screen+Shot+1444-06-25+at+12.29.40+PM.png)

        - كتابة الهيكلة المتوقّعة

user.schema.ts

    import {z} from 'zod';
    export const registrationSchrma = z.object({
        body: z.object({
            name: z.string({
                required_error: "Name is required",
                invalid_type_error: "Name must be string"
            }).min(2, "Name must consist of 2 or more letters"),
            email: z.string({
                required_error: "Email is required",
                invalid_type_error: "Email must be string"
            }).email("Please, insert the correct email format"),
            password: z.string({
                required_error: "Password is required",
                invalid_type_error: "Password must be string"
            }).min(8, "Your pasword must be at least 8 characters")
        })
    });



    - انشاء middleware لاختبار الهيكلة المرفقة 
    للاستفادة من جميع الاختبارات التي قمنا ببنائها، سنقوم بانشاء middleware تقوم باتقبال الهيكلة المرسلة واختبارها على البيانات من ثمّ تمريرها على controller المناسب اذا نجح الاختبار. و طباعة خطأ في حالة فشل الاختبار. 
    و ذلك سيتم من خلال انشاء مجلّد جديد ليحوي جميع middlewares الخاصّة بنا بالاسم middleware وبداخلة ملف validate.ts  الذي سنكتب فيه دالّة الاختبار الخاصّة بنا،
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674035337998_Screen+Shot+1444-06-25+at+12.48.53+PM.png)


validate.ts

    import { Request, Response, NextFunction } from "express";
    import { AnyZodObject, ZodError } from "zod";
    const validate = (schema: AnyZodObject) =>
     (req: Request,res: Response,next:NextFunction)=>{
         try{
             schema.parse({
                 body: req.body,
                 params: req.params
             });
             next();
         }catch(e){
             let zodError = e as ZodError;
             console.log(zodError.message);
             return res.status(400).json({msg: zodError.message});
         }
     }
    
    
     export default validate;


    - اختبار التحقق من البيانات من خلال استدعاء schema في router واختبارها كالتالي،
        - اختبار اضافة مهمّة جديدة. 

task.router.ts

    import express from 'express';
    import {create, findAllByUser, update, deleteById} from '../controller/task.controller';
    import { createSchema } from '../zodSchema/task.schema';
    import validate from '../middleware/validate';
    const router = express.Router();
    router.get('/:id',findAllByUser);
    router.post('/',validate(createSchema),create);
    router.put('/:id',update);
    router.delete('/:id',deleteById);
    
    export default router;



            - اختبار باستخدام معلومات صحيحة.
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674037797012_Screen+Shot+1444-06-25+at+1.29.51+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674037839576_Screen+Shot+1444-06-25+at+1.30.34+PM.png)

        
            - اختبار باستخدام معلومات خاطئة.
                - اعطاء uuid خاطئ
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674038289135_Screen+Shot+1444-06-25+at+1.38.05+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674038258626_Screen+Shot+1444-06-25+at+1.37.28+PM.png)


نلاحظ أن رسالة الخطأ غير واضحة، ولا تعبّر عن المشكلة بالتحديد. لذلك سنقوم بالتعديل عليها في ملف validate.ts بحيث تصبح قابلة للقراءة أكثر.

في النّوع ZodError الدّالة message التي تعيد لنا رسالة بجميع الأخطاء التي وجدها zod في البيانات. كما نلاحظ أيضاً ان عدنا الى تعريف ZodError انه تتواجد العديد من الدوال والخصائص التي تعرّفة. مثل خاصيّة isEmpty التي تعيد لنا false في حالة تواجدت أي مشكلة في البيانات. ومنها ايضاً خاصيّة errors التي تعيد لنا مصفوفة تحوي جميع الأخطاء. تماماً كخاصيّة issues. لذلك سنقوم باستخدام issues او errors لاظهار مصفوفة تحوي جميع الأخطاء من ثمّ طباعة رسالة أوّل خطأ واظهاره. ( وهكذا الى ان تنتهي جميع الأخطاء)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674038931404_Screen+Shot+1444-06-25+at+1.48.46+PM.png)


validate.ts

    import { Request, Response, NextFunction } from "express";
    import { AnyZodObject, ZodError } from "zod";
    const validate = (schema: AnyZodObject) =>
     (req: Request,res: Response,next:NextFunction)=>{
         try{
             schema.parse({
                 body: req.body,
                 params: req.params
             });
             next();
         }catch(e){
             let zodError = e as ZodError;
            //  console.log(zodError.issues[0].message);
             return res.status(400).json({msg: zodError.errors[0].message});
         }
     }
    
    
     export default validate;
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674039301696_Screen+Shot+1444-06-25+at+1.54.55+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674039543668_Screen+Shot+1444-06-25+at+1.58.58+PM.png)


بهذا الشكل أصبحت رسالة الخطأ قابلة للقراءة أكثر. 



        - اختبار تحديث مهمّة. 

task.router.ts

    import express from 'express';
    import {create, findAllByUser, update, deleteById} from '../controller/task.controller';
    import { createSchema, updateSchema } from '../zodSchema/task.schema';
    import validate from '../middleware/validate';
    const router = express.Router();
    router.get('/:id',findAllByUser);
    router.post('/',validate(createSchema),create);
    router.put('/:id',validate(updateSchema),update);
    router.delete('/:id',deleteById);
    
    export default router;



            - اختبار صحيح.
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674042639022_Screen+Shot+1444-06-25+at+2.50.32+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674042621219_Screen+Shot+1444-06-25+at+2.50.16+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674042699929_Screen+Shot+1444-06-25+at+2.51.36+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674042720491_Screen+Shot+1444-06-25+at+2.51.54+PM.png)

            - اختبار خاطئ.
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674043005725_Screen+Shot+1444-06-25+at+2.56.40+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674043031860_Screen+Shot+1444-06-25+at+2.57.07+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674043061600_Screen+Shot+1444-06-25+at+2.57.35+PM.png)



        - اختبار حذف مهمّة. 

task.router.ts

    import express from 'express';
    import {create, findAllByUser, update, deleteById} from '../controller/task.controller';
    import { createSchema, updateSchema, deleteSchema } from '../zodSchema/task.schema';
    import validate from '../middleware/validate';
    const router = express.Router();
    router.get('/:id',findAllByUser);
    router.post('/',validate(createSchema),create);
    router.put('/:id',validate(updateSchema),update);
    router.delete('/:id',validate(deleteSchema),deleteById);
    
    export default router;


            - اختبار صحيح.
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674043303556_Screen+Shot+1444-06-25+at+3.01.40+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674043325363_Screen+Shot+1444-06-25+at+3.01.59+PM.png)

            - اختبار خاطئ.
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674043267556_Screen+Shot+1444-06-25+at+3.01.01+PM.png)


أعلاه، لم يكن uuid في params صحيح، لذلك ظهرت مشكلة.



    - اضافة تحقق من تسجيل مستخدم جديد

user.router.ts

    import express from 'express';
    import {create} from '../controller/user.controller';
    import { registrationSchrma } from '../zodSchema/user.schema';
    import validate from '../middleware/validate';
    const router = express.Router();
    router.post('/',validate(registrationSchrma),create);
    
    export default router;


        - اختبار صحيح.
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674043728570_Screen+Shot+1444-06-25+at+3.08.44+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674043784058_Screen+Shot+1444-06-25+at+3.09.36+PM.png)


 

        - اختبار خاطئ.
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674043684219_Screen+Shot+1444-06-25+at+3.07.55+PM.png)



----------


- تشفير الرمز السرّي في قاعدة البيانات

كما نلاحظ في قاعدة البيانات، يتم تخزين كلمة المرور بشكل مباشر دون تشفيرها. وهذا يعني أن أي شخص يصل الى البيانات في قاعدة البيانات بشكل غير مصرّح او اي عضو من فريق التطوير لدية وصول الى قاعدة البيانات يمكنه استغلال البيانات، و هذا خاطئ، يجب علينا حماية بيانات المستخدمين دائماً، ومن أحد الطرق لحماية بياناتهم هي تشفير كلمة المرور في قاعدة البيانات حتّى لا يستطيع أحد استغلال حسابات المستخدمين والدخول عليها.

لهذا السبب سنقوم باستخدام مكتبة argon2 والتي هي من أفضل المكتبات لتشفير كلمات المرور، و قد فازت في مسابقة PHC: Password Hashing Competition. 

سنقوم أولاً باختبار المكتبة في مشروع جديد من ثمّ اضافتها في مشروعنا ToDo App. من خلال اتبّاع الخطوات التالية. 


- انشاء مشروع جديد
- عمل npm init -y لاستقبال مكتبات من npm
- تنزيل مكتبة argon2 
- اتبّاع تعلميات المكتبة في عمليّة التشفير hash و التحقق.

بعد اتبّاع جميع الخطوات السّابقة، ظهرت معنا النتيجة التالية وهي عمليّة تشفير واختبار كلمة المرور

app.ts in new project (test argon2)

    import * as argon2 from 'argon2';
    const myPassword: string = 'testmypassword';
    const test = async (password :string)=>{
        const hash = await argon2.hash(password);
        console.log(hash);
        console.log(await argon2.verify(hash, 'notmypassword')); //false
        console.log(await argon2.verify(hash, password)); //true
    }
    test(myPassword);

output

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674118485792_Screen+Shot+1444-06-26+at+11.54.41+AM.png)


الآن، بعد أن أخذنا نظره على طريقة استخدام argon2، سنقوم بتطبيق ما تعلّمناه في المشروع الخاص بنا من خلال اتبّاع الخطوات التالية. 

- تنزيل مكتبة argon2.
    npm i argon2
- استدعاء المكتبة في ملف user.controller.ts لتشفير البيانات وقت تسجيل المستخدم. 

user.controller.ts

    import {Request, Response} from 'express';
    import prisma from '../config/db';
    import * as argon2 from 'argon2';
    
    export const create = async (req:Request, res:Response) =>{
        const hash = await argon2.hash(req.body.password);
        try{
            const user = await prisma.user.create({
                data:{
                    name:req.body.name,
                    email: req.body.email,
                    password: hash
                }
            });
            if(user){
                res.status(200).json({msg:"user created!"})
            }
        }catch(e){
            res.status(500).json({msg:`Error: ${e}`});
        }
    }


- اختبار عمليّة اضافة مستخدم و تشفير رمز المرور الخاص به. 
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674126998329_Screen+Shot+1444-06-26+at+2.16.33+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674127030486_Screen+Shot+1444-06-26+at+2.17.05+PM.png)


بهذا الشكل انتهينا من تشفير كلمة مرور المستخدم.


- اضافة login controller و التحقق من المستخدم وكلمة المرور، ان كانت كلمة المرور صحيحة نرسل رسالة ترحيب، امّا ان كانت خاطئة سنقوم بارسال رسالة خطأ.

user.controller.ts

    import {Request, Response} from 'express';
    import prisma from '../config/db';
    import * as argon2 from 'argon2';
    
    export const create = async (req:Request, res:Response) =>{
        const hash = await argon2.hash(req.body.password);
        try{
            const user = await prisma.user.create({
                data:{
                    name:req.body.name,
                    email: req.body.email,
                    password: hash
                }
            });
            if(user){
                res.status(200).json({msg:"user created!"})
            }
        }catch(e){
            res.status(500).json({msg:`Error: ${e}`});
        }
    }
    
    export const login = async (req:Request, res:Response)=>{
        const user = await prisma.user.findUnique({
            where:{
                email: req.body.email
            }
        });
        if(!user){
            return res.status(400).json({Error:"Wrong email adress"});
        }else if(!await argon2.verify(user.password, req.body.password)){
            return res.status(400).json({Error:"Wrong password"});
        }
        return res.status(200).json({message:`Hello ${user.name}`});
    }


- اضافة route لعمليّة تسجيل الدخول.

user.route.ts

    import express from 'express';
    import {create , login} from '../controller/user.controller';
    import { registrationSchrma } from '../zodSchema/user.schema';
    import validate from '../middleware/validate';
    const router = express.Router();
    router.post('/',validate(registrationSchrma),create);
    router.post('/login', login);
    
    export default router;


> ملاحظة، تمّ تحديث registrationSchrma الى registrationSchema في الكود بعد هذه النقطة.


- اختبار عمليّة تسجيل الدخول.
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674128428481_Screen+Shot+1444-06-26+at+2.40.25+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674128444010_Screen+Shot+1444-06-26+at+2.40.39+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674128461830_Screen+Shot+1444-06-26+at+2.40.57+PM.png)



> ملاحظة: name لا نحتاج اليه، لذلك لا مشكلة من استقباله لكن لن نتعامل معه فقط.



----------



- اضافة تسجيل دخول للمستخدم عن طريق استخدام JWT.

يساعدنا JWT على التحقق من كل مستخدم يحاول الوصول الى عمليّة معيّنة endpoint في التطبيق. بحيث يقوم كل مستخدم بارسال token يثبت اهليتهم وصلاحيتهم للقيام بعمليّة معيّنة. و ينقسم JWT الى ثلاثة أقسام أساسيّة كالتالي. 


    JWT = Header.Payload.Signature

بحيث يتم فصل كل جزء عن الآخر بنقطة (.) كما يحوي كل جزء على معلومات معيّنة تجدها في هذا المصدر.


![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674132658264_Screen+Shot+1444-06-26+at+3.50.53+PM.png)


لذلك. سنقوم باستخدام مفهوم JWT لمساعدتنا في التحقق من هويّة المستخدمين وصلاحياتهم وقت استقبال الطلبات requests. و ذلك ستم بعد اتباعنا للخطوات التالية و معرفة طريقة استخدام مكتبة jsonwebtoken لانشاء و التحقق من JWT.

- انشاء مشروع جديد لاختبار مفهوم JWT.
- استخدام الأمر npm init -y.
- تنزيل مكتبة jsonwebtoken و  @types/jsonwebtoken لدعم لغة Typescript.
- استدعاء و استخدام المكتبة بناءً على المصدر الخاص بها documentation. 

app.ts

    import * as jwt from 'jsonwebtoken';
    const secret = 'mySecret';
    interface User{
        name: string,
        role: string
    }
    let userPayload : User = {
        name: 'sara',
        role: 'admin'
    }
    let enToken = jwt.sign(userPayload,secret,{expiresIn:'5h'});
    let deToken = jwt.verify(enToken,secret);
    let readableDeToken = deToken as User;
    console.log(enToken);
    console.log(deToken);
    console.log(readableDeToken.name);
    console.log(readableDeToken.role);
> ملاحظة: احتجنا استخدام as User ليقوم compiler بافتراض أن deToken هو من نوع User لنستطيع قراءة بيانات name and role منه.

output

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674135196440_Screen+Shot+1444-06-26+at+4.33.11+PM.png)



بعد أن قمنا بتطبيق مفهوم JWT ورأينا طريقة الانشاء و التحقق من Tokens، سنقوم الآن بتطبيق المفهوم على التطبيق الخاص بنا من خلال اتبّاع التالي.

- تنزيل مكتبة jsonwebtoken و types/jsonwebtoken.

Terminal

    npm i jsonwebtoken
    npm i -D @types/jsonwebtoken


- استدعاء مكتبة jsonwebtoken في ملف user.controller.ts و انشاء Token بعد تسجيل دخول المستخدم و ارساله له. 

user.controller.ts

    import {Request, Response} from 'express';
    import prisma from '../config/db';
    import * as argon2 from 'argon2';
    import * as jwt from 'jsonwebtoken';
    
    export const create = async (req:Request, res:Response) =>{
        const hash = await argon2.hash(req.body.password);
        try{
            const user = await prisma.user.create({
                data:{
                    name:req.body.name,
                    email: req.body.email,
                    password: hash
                }
            });
            if(user){
                res.status(200).json({msg:"user created!"})
            }
        }catch(e){
            res.status(500).json({msg:`Error: ${e}`});
        }
    }
    
    export const login = async (req:Request, res:Response)=>{
        const user = await prisma.user.findUnique({
            where:{
                email: req.body.email
            }
        });
        if(!user){
            return res.status(400).json({Error:"Wrong email adress"});
        }else if(!await argon2.verify(user.password, req.body.password)){
            return res.status(400).json({Error:"Wrong password"});
        }
        const token = jwt.sign({
            id: user.id,
            name: user.name,
            role: user.role
        }, "secret", {
            expiresIn: '5h'
        });
        return res.status(200).json({
            message:`Hello ${user.name}`,
            token: token
        });
    }

لأن معلومة secret تعتبر سريّة و حساسة، سنقوم باضافتها الى ملف .env واستدعاؤها في ملف user.controller.ts.
.env

    # Environment variables declared in this file are automatically made available to Prisma.
    # See the documentation for more detail: https://pris.ly/d/prisma-schema#accessing-environment-variables-from-the-schema
    # Prisma supports the native connection string format for PostgreSQL, MySQL, SQLite, SQL Server, MongoDB and CockroachDB.
    # See the documentation for all the connection string options: https://pris.ly/d/connection-strings
    DATABASE_URL="mysql://root@localhost:3306/ToDo"
    PORT= 3000
    JWT_SECRET= MY_API_KEY_SECRET

user.controller.ts

    import {Request, Response} from 'express';
    import prisma from '../config/db';
    import * as argon2 from 'argon2';
    import * as jwt from 'jsonwebtoken';
    
    export const create = async (req:Request, res:Response) =>{
        const hash = await argon2.hash(req.body.password);
        try{
            const user = await prisma.user.create({
                data:{
                    name:req.body.name,
                    email: req.body.email,
                    password: hash
                }
            });
            if(user){
                res.status(200).json({msg:"user created!"})
            }
        }catch(e){
            res.status(500).json({msg:`Error: ${e}`});
        }
    }
    
    export const login = async (req:Request, res:Response)=>{
        const user = await prisma.user.findUnique({
            where:{
                email: req.body.email
            }
        });
        if(!user){
            return res.status(400).json({Error:"Wrong email adress"});
        }else if(!await argon2.verify(user.password, req.body.password)){
            return res.status(400).json({Error:"Wrong password"});
        }
        const token = jwt.sign({
            id: user.id,
            name: user.name,
            role: user.role
        }, process.env.JWT_SECRET as string, {
            expiresIn: '5h'
        });
        return res.status(200).json({
            message:`Hello ${user.name}`,
            token: token
        });
    }
    


- اختبار انشاء token
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674139543433_Screen+Shot+1444-06-26+at+5.45.39+PM.png)

- انشاء Middleware للتحقق من Token و حفظه لاستخدامه لاحقاً عوضاً عن التحقق أكثر من مرّه للطلب الواحد.
    - انشاء ملف auth.ts في مجلّد middleware.
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674140288882_Screen+Shot+1444-06-26+at+5.58.05+PM.png)

    - انشاء أوامر middleware للتحقق من token.

auth.ts

    import { Request, Response, NextFunction } from "express";
    import * as jwt from 'jsonwebtoken';
    import { string } from "zod";
    interface User{
        id: string,
        name: string,
        role: string
    }
    const auth = (req:Request, res:Response, next:NextFunction)=>{
        try{
            const token = req.headers.authorization;
            if(!token){
                return res.status(403).json({message: "You are not authorized, please provide a valid token."});
            }
            const user = jwt.verify(token,process.env.JWT_SECRET as string) as User;
            res.locals.user = user;
            next();
        }catch(e){
            return  res.status(403).json({message: "You are not authorized, please provide a valid token."})
        }
    }
    export default auth;


- استخدام middleware في أحد routes للمهام. 

task.route.ts

    import express from 'express';
    import {create, findAllByUser, update, deleteById} from '../controller/task.controller';
    import { createSchema, updateSchema, deleteSchema } from '../zodSchema/task.schema';
    import validate from '../middleware/validate';
    import auth from '../middleware/auth';
    const router = express.Router();
    router.get('/:id',findAllByUser);
    router.post('/',auth,validate(createSchema),create);
    router.put('/:id',validate(updateSchema),update);
    router.delete('/:id',validate(deleteSchema),deleteById);
    
    export default router;


- ايضاً سنقوم باضافتها لبقيّة المسارات التي تحتاج الى تسجيل دخول.

task.route.ts

    import express from 'express';
    import {create, findAllByUser, update, deleteById} from '../controller/task.controller';
    import { createSchema, updateSchema, deleteSchema } from '../zodSchema/task.schema';
    import validate from '../middleware/validate';
    import auth from '../middleware/auth';
    const router = express.Router();
    router.get('/:id',auth,findAllByUser);
    router.post('/',auth,validate(createSchema),create);
    router.put('/:id',auth,validate(updateSchema),update);
    router.delete('/:id',auth,validate(deleteSchema),deleteById);
    
    export default router;


- اختبار مهمّة اضافة task جديدة
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674141073629_Screen+Shot+1444-06-26+at+6.11.08+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674141059758_Screen+Shot+1444-06-26+at+6.10.56+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674141128969_Screen+Shot+1444-06-26+at+6.12.01+PM.png)


في ملف auth.ts قمنا بتخزين معلومات المستخدم باستخدام res.locals، وهذا يعني أننا نستطيع ايجاد id الخاص بالمستخدم بمجرد تسجيل دخوله دون الحاجه لطلبه ادخال userId مع كل عمليّة يقوم بها على مهامّة. لذلك سنقوم بتحديث task.controller.ts بحيث نزيل أي userId لم نعد بحاجة اليه بعد الآن مع ازالته ايضاً من task.schema.ts لأننا لم نعد نطالب به.

task.controller.ts

    import {Request, Response} from 'express';
    import prisma from '../config/db';
    export const create = async (req:Request, res:Response) =>{
        try{
            const task = await prisma.task.create({
                data:{
                    title:req.body.title,
                    userId: res.locals.user.id
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
                    userId: res.locals.user.id //User id
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
                    id: req.params.id, //Task id
                    userId: res.locals.user.id
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
                    id: req.params.id, //Task id
                    userId: res.locals.user.id
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

task.schema.ts

    import {z} from 'zod';
    export const createSchema = z.object({
        body: z.object({
            title: z.string({
                required_error: "Title is required",
                invalid_type_error: "Title must be string"
            }).min(2, "Title must consist of 2 or more letters")
        })
    });
    export const updateSchema = z.object({
        body: z.object({
            title: z.string({
                required_error: "Title is required",
                invalid_type_error: "Title must be string"
            }).min(2, "Title must consist of 2 or more letters"),
            isCompleted: z.boolean().optional()
        }),
        params: z.object({
            id: z.string({
                required_error: "id is required",
                invalid_type_error: "id must be string"
            }).uuid()
        })
    });
    export const deleteSchema = z.object({
        params: z.object({
            id: z.string({
                required_error: "id is required",
                invalid_type_error: "id must be string"
            }).uuid()
        })
    });

ايضاً، لأننا لم نعد بحاجة الى userId في findAllByUser parameter سنقوم بازالته من ملف task.route.ts كالتالي.
task.route.ts

    import express from 'express';
    import {create, findAllByUser, update, deleteById} from '../controller/task.controller';
    import { createSchema, updateSchema, deleteSchema } from '../zodSchema/task.schema';
    import validate from '../middleware/validate';
    import auth from '../middleware/auth';
    const router = express.Router();
    router.get('/',auth,findAllByUser);
    router.post('/',auth,validate(createSchema),create);
    router.put('/:id',auth,validate(updateSchema),update);
    router.delete('/:id',auth,validate(deleteSchema),deleteById);
    
    export default router;


- اختبار عمليّات المهام لأحد المستخدمين. 
    - اضافة مهمّة.
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674141758471_Screen+Shot+1444-06-26+at+6.22.35+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674141796450_Screen+Shot+1444-06-26+at+6.23.13+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674141825776_Screen+Shot+1444-06-26+at+6.23.40+PM.png)



- عرض جميع المهام
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674142250056_Screen+Shot+1444-06-26+at+6.30.45+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674142261664_Screen+Shot+1444-06-26+at+6.30.56+PM.png)

- تحديث مهمّة 
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674142317421_Screen+Shot+1444-06-26+at+6.31.52+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674142332924_Screen+Shot+1444-06-26+at+6.32.08+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674142347433_Screen+Shot+1444-06-26+at+6.32.22+PM.png)

- حذف مهمّة
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674142364504_Screen+Shot+1444-06-26+at+6.32.40+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674142375955_Screen+Shot+1444-06-26+at+6.32.52+PM.png)

![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674142413767_Screen+Shot+1444-06-26+at+6.33.29+PM.png)



- اختبار عرض مهام بدون ارسال Token
![](https://paper-attachments.dropboxusercontent.com/s_139F4A99BBF351D312D5617A5D0CC5DA85C61F08FE2EB506B38663A38F0E7BF8_1674142538930_Screen+Shot+1444-06-26+at+6.35.36+PM.png)




----------


- اضافة cors على المشروع من خلال عمل التالي.
    - تنزيل مكتبة cors  و types/cors
    - استخدام cors في ملف app.ts

app.ts

    import express,{Application,Request,Response} from 'express';
    import userRouter from './routes/user.route';
    import taskRouter from './routes/task.route';
    import cors from 'cors';
    import * as dotenv from 'dotenv';
    dotenv.config();
    const app:Application = express();
    const port = process.env.PORT || 3000;
    app.use(cors());
    app.use(express.json());
    app.use('/users',userRouter);
    app.use('/tasks',taskRouter);
    app.listen(port, ()=>{
        console.log(`Application running on port ${port}`)
    });
    


[https://github.com/dev-SAFCSP/ToDo](رابط GitHub للمشروع)
