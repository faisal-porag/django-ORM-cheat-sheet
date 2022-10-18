# Django-ORM-Queryset

#### ORM Sample Filter Example
Return only the records where the firstname is 'Emil':
```shell
mydata = Members.objects.filter(firstname='Emil').values()
```
#### SQL Example
```shell
SELECT * FROM members WHERE firstname = 'Emil';
```

---

#### ORM Sample Filter Example 2
Return only the records where the firstname is 'Emil':
```shell
mydata = Members.objects.filter(firstname='Emil').values(id, firstname, lastname)
```
#### SQL Example 2
```shell
SELECT id, firstname, lastname FROM members WHERE firstname = 'Emil';
```

---

#### ORM `AND` Example
Return records where lastname is "Refsnes" and id is 2:
```shell
mydata = Members.objects.filter(lastname='Refsnes', id=2).values()
```
#### SQL `AND` Example
```shell
SELECT * FROM members WHERE lastname='Refsnes' AND id = 2;
```

---

#### ORM `OR` Example
Return records where firstname is either "Emil" or Tobias":
```shell
mydata = Members.objects.filter(firstname='Emil').values() | Members.objects.filter(firstname='Tobias').values()
```

```shell
from django.db.models import Q
Members.objects.filter(Q(income__gte=5000) | Q(income__isnull=True))
```


#### SQL `OR` Example
```shell
SELECT * FROM members WHERE firstname = 'Emil' OR firstname = 'Tobias';
```

---

### Field Lookups Reference
A list of all field look up keywords:

Keyword	| Description
--- | ---
contains |	Contains the phrase
icontains |	Same as contains, but case-insensitive
date |	Matches a date
day	| Matches a date (day of month, 1-31) (for dates)
endswith |	Ends with
iendswith |	Same as endswidth, but case-insensitive
exact |	An exact match
iexact |	Same as exact, but case-insensitive
in |	Matches one of the values
isnull |	Matches NULL values
gt |	Greater than
gte |	Greater than, or equal to
hour |	Matches an hour (for datetimes)
lt |	Less than
lte |	Less than, or equal to
minute |	Matches a minute (for datetimes)
month |	Matches a month (for dates)
quarter |	Matches a quarter of the year (1-4) (for dates)
range |	Match between
regex |	Matches a regular expression
iregex |	Same as regex, but case-insensitive
second |	Matches a second (for datetimes)
startswith |	Starts with
istartswith |	Same as startswith, but case-insensitive
time |	Matches a time (for datetimes)
week |	Matches a week number (1-53) (for dates)
week_day |	Matches a day of week (1-7) 1 is sunday
iso_week_day |	Matches a ISO 8601 day of week (1-7) 1 is monday
year |	Matches a year (for dates)
iso_year |	Matches an ISO 8601 year (for dates)


#### ORM Example 
Return the records where firstname starts with the letter 'L':
```shell
mydata = Members.objects.filter(firstname__startswith='L').values()
```
#### SQL EXAMPE
```shell
SELECT * FROM members WHERE firstname LIKE 'L%';
```
---

#### ORM Example `(IN)`
```shell
mydata = Members..objects.filter(pk__in=[1, 4, 7])
```
#### SQL `IN` EXAMPE
```shell
SELECT * FROM members WHERE id in (1, 4, 7);
```
---

#### Date range example 
```shell
from datetime import date, timedelta
startdate = date.today()
enddate = startdate + timedelta(days=6)
Sample.objects.filter(date__range=[startdate, enddate])
```

#### GET data from last 90 days - ORM Example
```shell
last_90_days = timezone.now() - timedelta(days=90)

mydata = Members.objects.filter(
    initiated_at__gte=last_90_days
).order_by('-initiated_at').values('firstname')
```

---

### UPDATE ORM EXAMPLE 

#### Update all the headlines with pub_date in 2007.
```shell
News.objects.filter(pub_date__year=2007).update(headline='Everything is the same')
```

```shell
User.objects.filter(last_login=None).update(is_active=False)
```

#### Order by ASC (Order the the result alphabetically by firstname)
```shell
mydata = Members.objects.all().order_by('firstname').values()
```
#### Order by DESC (Order the the result firstname descending)
```shell
mydata = Members.objects.all().order_by('-firstname').values()
```
---

#### Example of Order the the result first by lastname ascending, then descending on id:
```shell
mydata = Members.objects.all().order_by('lastname', '-id').values()
```
##### In SQL, the above statement would be written like this:
```shell
SELECT * FROM members ORDER BY lastname ASC, id DESC;
```
---

#### JOIN QUERY ORM EXAMPLE MODEL
```shell
class Question(models.Model):
      title = models.CharField(max_length=70)
      details = models.TextField()

class Answer(models.Model):
     question = models.ForeignKey('Question')
     details = models.TextField()
```      

#### JOIN QUERY
```shell
answers = Answer.objects.filter(question_id=1).select_related()
```

#### SUM ORM FORMAT
```shell
from django.db.models import Sum

ItemPrice.objects.aggregate(Sum('price'))
# returns {'price__sum': 1000} for example
```

---

#### AND, OR CONDITION in ORM 
You can combine multiple comma-separated `Q` objects to `.get()`, which will act like parenthesized `AND` clauses.

```shell
User.objects.get(
  Q(income__gte=5000) | Q(income=0),
  Q(email__endswith='gmail.com') | Q(email__endswith='yahoo.com')
)
```

#### RAW SQL 
```shell
SELECT * FROM users
WHERE (income >= 5000 OR income = 0)
AND 
(email LIKE '%gmail.com' OR email LIKE '%yahoo.com');
```

#### NOTE 
If you combine `Q` clauses with keyword arguments, just make sure that the `Q` argument comes first:

```shell
User.objects.get(
  Q(income__gte=5000) | Q(income=0),
  email__endswith='gmail.com'
)
```
---

#### Avg, Max, Min, Sum, Count

```shell
>>> from django.db.models import Avg, Max, Min, Sum, Count  
>>> Student.objects.all().aggregate(Avg('id'))  
# {'id__avg': 5.5}  
>>> Student.objects.all().aggregate(Min('id'))    
# {'id__min': 1}  
>>> Student.objects.all().aggregate(Max('id'))   
# {'id__max': 10}  
>>> Student.objects.all().aggregate(Sum('id'))   
# {'id__sum': 55}  
```
---

#### ‘__isnull’ is used to filter the null values in the Django ORM. It accepts True or False.

```shell
>>> User.objects.filter(age__isnull=True).values('id','age')
<QuerySet []>
>>> User.objects.filter(age__isnull=False).values('id','age')
<QuerySet [{'id': 1, 'age': 20}, {'id': 2, 'age': 25}, {'id': 3, 'age': 30}]>
```


#### The exists() method is used to check the result of the query. Returns True if the queryset contains any results, and False if not.

```shell
>>> User.objects.filter(age__isnull=True).values('id','age').exists()
False
>>> User.objects.filter(age__isnull=False).values('id','age').exists()
True
```
---

#### Excludes objects from the queryset which match with the lookup parameters.

```shell
>>> User.objects.exclude(id=1)
<QuerySet [<User: User object (2)>, <User: User object (3)>]>
```


---

#### Simple or fancy UPSERT in PostgreSQL with Django >>>> Here's the basic version in "pure Django ORM":

```shell
if UserModel.objects.filter(hash=hash_).exists():
    UserModel.objects.filter(hash=hash_).update(
        count=F('count') + 1,
        modified_at=timezone.now()
    )
else:
    UserModel.objects.create(
        hash=hash_,
        symbol=symbol,
        debugid=debugid,
        filename=filename,
        code_file=code_file or None,
        code_id=code_id or None,
    )
```
---

#### with `select_related()`

```shell
entry = Entry.objects.select_related("blog").first()
# SELECT "blog_entry"."id", ... "blog_blog"."id", ...
# FROM "blog_entry"
# INNER JOIN "blog_blog" ...;

blog = entry.blog
# no query is run because we JOINed with the blog table above
```

--- 

#### `CASE` and conditional logic 

```shell
entries = Entry.objects.annotate(
    coolness=Case(
        When(rating=5, then=Value("super cool")),
        When(rating=4, then=Value("pretty cool")),
        default=Value("not cool"),
    )
)
# SELECT ...
# CASE
#   WHEN "blog_entry"."rating" = 5 THEN 'super cool'
#   WHEN "blog_entry"."rating" = 4 THEN 'pretty cool'
#   ELSE 'not cool'
# END AS "coolness"
# FROM "blog_entry" LIMIT 5;

[f"Entry {e.pk} is {e.coolness}" for e in entries[:5]]
# ['Entry 4137 is super cool',
#  'Entry 4138 is not cool',
#  'Entry 4139 is not cool',
#  'Entry 4140 is pretty cool',
#  'Entry 4141 is not cool']
```
---

#### Select data from table using Django ORM

```shell
queryset = ModelName.objects.all()
print(queryset.query)
print(list(queryset))
```
---

#### Our first index
###### We will now introduce a simple index with one field. Sure this field iscode0 :

```shell
class Student(models.Model):
    first_name = models.CharField(max_length=100)
    last_name = models.CharField(max_length=100)
    code0 = models.CharField(max_length=100, blank=True)
    code1 = models.CharField(max_length=100, blank=True)
    code2 = models.CharField(max_length=100, blank=True)
    code3 = models.CharField(max_length=100, blank=True)
    code4 = models.CharField(max_length=100, blank=True)

    class Meta:
        indexes = [models.Index(fields=['code0', ]), ]
```




--- 

#### DJANGO ORM EXAMPLES 

###### CREATE MODEL

```shell
class Author(models.Model):
    nickname = models.CharField(max_length=20, null=True, blank=True)
    firstname = models.CharField(max_length=20)
    lastname = models.CharField(max_length=40)
    birth_date = models.DateField()
class Book(models.Model):
    author = models.ForeignKey(Author, related_name="books", on_delete=models.CASCADE)
    title = models.CharField(unique=True, max_length=100)
    category = models.CharField(max_length=50)
    published = models.DateField()
    price = models.DecimalField(decimal_places=2, max_digits=6)
    rating = models.IntegerField()
```

###### range

```shell
start_date = datetime.date(2021, 1, 1)
end_date = datetime.date(2021, 1, 3)
books = Book.objects.filter(published__range=(start_date, end_date))
```

##### F expressions

```shell
from django.db.models import F
# Query books published by authors under 30 years old (this is not exactly true because years vary in length)
books = Book.objects.filter(published__lte=F("author__birth_date") + datetime.timedelta(days=365*30))
```
```shell
from django.db.models import F
book = Book.objects.get(title="Crime and Punishment")
book.update(rating=F("rating") + 1)
```

##### Annotation

```shell
from django.db.models import F, Value as V
from django.db.models.functions import Concat
author = Author.objects.annotate(full_name=Concat(F("firstname"), V(" "), F("lastname")))
```
`or` 

```shell
from django.db.models import F
from django.db.models.functions import Coalesce
books = Author.objects.annotate(known_as=Coalesce(F("nickname"), F("firstname")))
```

















