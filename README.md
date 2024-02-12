# Django_Procedure

*How to add multiple apps in a single project.*   
*Here are the steps to follow :-*

## PHASE1: Project creation 
**Step1 :** In Terminal  
```
django-admin startproject django1
```
It is used to create a project with *folder name* and *project name* as **django1**  

**Step2 :** 
```
cd django1
```
We changed the current dirctory to *django1*  

**Step3 :** 
```
django-admin startapp factorial1  
```
We created an app with *app name* as **factorial1**  

**Step4 :** 
```
python manage.py runserver  
```
Run the Django development server  
If everything is okay with your Project, Django will start running the server at localhost port 8000 (127.0. 0.1:8000), and then you have to navigate to that link in your browser.  
  
If you are doing multiple apps in same project you might get *Page Not Found Error*, You can ignore it.  

## PHASE2: Creating urls 
**Step5 :**  
5a) In factorial1, create folder templates  
5b) In factorial1/templates, create folder factorial1   
5c) In factorial1/templates/html, create file index.html  
  
**Step6:**  In index.html write a program which include 'Hello World' and {{param1}}  
```
<body>
    <p>Hello World</p>
    <p>{{param1}}</p>
</body>
```

**Step7 :** Go to django1/settings.py add 
```
INSTALLED_APPS = [...,"factorial1", ]
```

**Step8 :** Go to factorial1/views.py
```
def home(request):
    return render(request,'factorial1/index.html',{'param1':"hello world"})
```

**Step9 :** Create urls.py in factorial1 and add
```
from django.urls import path
from factorial1.views import home
urlpatterns = [path('', home),]
```
This is not mandatory to create urls.py for each apps. you can add all apps path in urls.py in main project.  
It's an option that most Django developer seem to take advantage of because it helps keep your code organized - i,e; the urls relevant to a specific app live in that app's folder.  

**Step10 :** In django1/urls.py  
There will be  two parts :  
10a) After ```from django.urls import path```  
import include- It is used for including the content of a file into your current program.  
```
from django.urls import include
```
10b) Inside urlpatterns  add
```
path("factorial1" ,include("factorial1.urls")),
```

**Step11 :** In Terminal run,  
```
python manage.py runserver
```  
You will get output as   
Hello World   
hello world  
In Browser,
We should make changes in the server at localhost port 8000 i.e; **127.0.0.1:8000/factorial1**

## PHASE3: Logic to be implemented in views.py 
**Step12 :**
Something like this  
```
from django.shortcuts import render
def home(request):
    result=1
    n1=5
    for i in range(1,n1+1,1):
        result=result*i
    return render(request,'factorial1/index.html',{'param1':result,'param2':n1})
    
```
Also make changes in index.html  
```
<body>
    <p>Hello World</p>
    <p>The factorial of {{param1}} is {{param2}}</p>
</body>
```  
We should get output as *The factorial of 5 is 120*  

## PHASE4: add forms to take input from the user ##
 We can modify the code using forms  
 **Step13 :** create forms.py in the factorial1 folder  
```
from django import forms
class inputform(forms.Form):
    name=forms.CharField(max_length=10)
    input=forms.IntegerField(min_value=$1,max_value=$2,label="$3")
```
  *$ may be any numbers for min and max values and characters for the label*  

**Step14 :** in index.html
```
<body>
    <h1>Factorial Program</h1>
    <form method="POST">
    {% csrf_token %}
    {{form.as_p}}    
    <button type="submit">find factorial</button>
    </form>
    {% if param1 %}
    <h2>Factorial of {{param2}} is {{param1}}</h2>
    {% endif %}
</body>
```
*we can use (p, ul, table)  where  p-paragraph, ul-unordered list, table-table*  
   
**Step15 :** in factorial/views.py, 
```
from django.shortcuts import render
from factorial1.forms import inputform
def home(request):
    if request.method=="POST":
        form1=inputform(request.POST)
        if form1.is_valid():
            data=form1.cleaned_data
            n1=data.get("input")
            result=fact(n1)
            return render(request,"factorial1/index.html",{'param1':result, 'param2':n1, 'form':form1})
    else:
        form1=inputform()  
    return render(request,"factorial1/index.html",{'param1':result, 'param2':n1, 'form':form1})
```
```
def fact(n1):  
    result=1
    for i in range(1,n1+1,1):
        result=result*i
    return result
```
                                           

## Phase5 is not complete. We will make the modifications 

## PHASE5: 
**Step 16:** make DEBUG = False in settings.py  
**Step17:** make   
```ALLOWED_HOSTS = ['*']
```
**Step 18(optional):** 
```
DATABASES = {  
    'default': {  
        'ENGINE': 'django.db.backends.sqlite3',  
        'NAME': BASE_DIR / 'db.sqlite3',  
    }  
}
```
**Step 19 :** STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')  
**Step 20:** write python manage.py collectstatic in terminal  
(Optional , only used if we are using database operations)  
**Step 21:** type "python manage.py makemigrations" in terminal  
**Step 22:** type  "python manage.py migrate in terminal" in terminal  
**Step 23:**  python manage.py runserver  
