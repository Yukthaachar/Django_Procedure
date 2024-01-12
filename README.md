# Django_Procedure

how to add multiple apps in a single project. 
Here are the steps to follow :-

## PHASE1: Project creation 
Step1 : django-admin startproject django1
Step2 : cd django1
Step3 : django-admin startapp factorial1
Step4 : python manage.py runserver
check if project is created by going here http://127.0.0.1:8000/

## PHASE2: Creating urls 
Step5 : Create factorial1/templates/factorial1 folder    (appname/templates/appname)
Step6:  Create index.html file with 'Hello World' and {{param1}}

```
<body>
    <p>Hello World</p>
    <p>{{param1}}</p>
</body>
```

Step7 : Go to django1/settings.py add 
```
INSTALLED_APPS = [...,"factorial1", ]
```

Step8 : Go to views.py
```
def home(request):
    return render(request,'factorial1/index.html',{'param1':”hello world”})
```

Step9 : Create urls.py in factorial1 and add
```
from django.urls import path
from factorial1.views import home
urlpatterns = [path('', home),]
```

Step10 : In django1/urls.py
```
from django.urls import include
urlpatterns = [
path("" ,include("factorial1.urls"))
]
```

Step11 : python manage.py runserver
You will get output as 
Hello World 
hello world

## PHASE3: Logic to be implemented in views.py 
Step12 :
Something like this
```
from django.shortcuts import render
def home(request):
    result=1
    n1=5
    for i in range(1,n1+1,1):
        result=result*i
    return render(request,'fact1/index.html',{'param1':result,'param2':n1})
    
```

## PHASE4: add forms to take input from the user ##
 We can modify the code using forms
 Step13 : create forms.py in the factorial1 folder
 ```
 from django import forms
 class inputform(forms.Form):
      name=forms.CharField(max_Length=10)
      input=forms.IntegerField(min_value=$1,max_value=$2,lable="$3")  
  ```
  $ may be any numbers for min and max values and characters for the label

Step14 : in index.html
```
<body>
<h1>Factorial Program</h1>
<form method="POST">
{% csrf_token %}
{{form.as_p}}     # we can use (p, ul, table)  where   p-paragraph, ul-unordered list, table-table
<button type="submit">find factorial</button>
</form>
{% if param1 %}
<h2>Factorial of {{param2}} is {{param1}}</h2>
{% endif %}
</body>
```

Step15 : in factorial/views.py, 
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
       return render(request,"factorial1/index.html",{'param1':result,'param2':n1,
                                                          'form':form1})
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
                                           

## we will make the modifications 

## PHASE5: 
Step 16: make DEBUG = False in settings.py
Step17: make 
```ALLOWED_HOSTS = ['*']```
Step 17(optional):   DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
Step 18 : STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
Step 19: write python manage.py collectstatic in terminal
(Optional , only used if we are using database operations)
Step 20: type "python manage.py makemigrations" in terminal
Step 21:type  "python manage.py migrate in terminal" in terminal
Step 22:  python manage.py runserver
