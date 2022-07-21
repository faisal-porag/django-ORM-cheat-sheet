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
#### SQL `OR` Example
```shell
SELECT * FROM members WHERE   firstname = 'Emil' OR firstname = 'Tobias';
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































