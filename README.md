ML/DL Employee-Attrition

.اول از همه دیتاست رو دانلود کردم و یه سری از اطلاعات نامربوط که لازم نمیشود رو حذف کردم به صورت دستی
Employee-Attrition.csv

.زمانی که دیتاست رو توی وی اس کد خوندم متوجه شدم یه سری از ردیف ها به صورت NON مونده

.یه دستور کدی نوشتم که اون ردیف های خالی رو حذف کنم و دیتاست جدید درست کردم



# خوندن فایل
df = pd.read_csv("Employee-Attrition.csv")  

#  حذف ستون‌هایی که کامل خالی هستن
df = df.dropna(axis=1, how='all')
# حذف ردیف‌هایی که حداقل یک مقدار خالی دارن
df_cleaned = df.dropna()

#  ذخیره فایل تمیزشده
df_cleaned.to_csv("cleaned_file.csv", index=False)

print("✅ فایل تمیزسازی شد و ذخیره شد.")


.ردیف های خالی رو چک کردم

#خوندن دیتافریم
df.info()



.هر حروفی رو تبدیل به عدد کردم مثلا زن و مرد رو 0و1 تعریف کردم

#تبدیل حروف به عدد
le=LabelEncoder()

for col in df:
    if df [col].dtype=='object':
        if len (df[col].unique()) <=9:
            print(col)
            le.fit(df[col])
            df[col]=le.transform(df[col])



.حالت های متخلف رو چک کردم و اونای که زیاد بودن و کاربردی نداشت رو حذف کردم

#تعداد حالت های مختلف
colum_name = []
unique_value = []

for col in df :
    colum_name.append(str(col))
    unique_value.append(df[col].nunique())

table = pd.DataFrame()
table['col_name'] =colum_name
table['value']  = unique_value

table=table.sort_values('value',ascending=False)
table


.رابطه ترک شغل رو با کار چک کردم 

f,ax = plt.subplots(figsize=(5,5))
sns.countplot(x="OverTime",hue="Attrition",data=df)



.ترک شغل رو از ستون ها خارج کردم و به عنوان خروجی تعریف کردم
#جدا کردن x,y
x=df.drop('Attrition',axis=1)
y=df['Attrition']


.ورودی و خروجی تست و تمرین چک کردم
#جدا کردن test,train
x_train, x_tast, y_train, y_test =train_test_split(x,y, test_size=0.2)



.با ااگوریتم های Random forest , Logistic Regression چک کردم درصد بالای نداد و با الگوریتم Light GBM چک کردم درصد بالای داد بهم
.وحالا با دیپ لرنینگ چک کردم

