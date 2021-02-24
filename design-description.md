# Design description of single-user job offer comparison app


##Requirement design description

1.When the app is started, the user is presented with the main menu, which allows the user to (1) enter or edit current job details, (2) enter job offers, (3) adjust the comparison settings, or (4) compare job offers (disabled if no job offers were entered yet). 

Actually, this is the System GUI-specific class. Theoritically, it is supposed to be MainMenue class with 4 attributes:(1) enter or edit current job details, (2) enter job offers, (3) adjust the comparison settings, or (4) compare job offers (disabled if no job offers were entered yet). User can click these four button to realize the function they want. In here, a SingleUser class has been created as an entry point.It is shown as follow:

![](https://github.com/tyang373/CS6300UML_pics/blob/main/SingleUser%20Class.jpg?raw=true)

The attributes I set is the String `Name` of user(maybe unnecessary), a boolean `Hasjob` for determine the status that whether the user has a current job. This attribute will be used for define whether funciton(4)above will be enabled, in case there is no current job.

For the opreation: 

- The user can use 'Compare job offers' class to send job compare request. 
- The user can select different functions, edit and get the final report result.
- The result will be achieved via 'Return'class associated with 'Compare Job Offers' class, the String array `Title` and `Company` comparison   info will be returned.
- 'SingleUser'class can has 'currentJob' class, the person limit is 1, the currentjob number is an integer no less than zero.
- 'SingleUser'class can has 'Joboffers' class, the person limit is 1, the joboffers number is an integer no less than one.

Tips: function (4) will be enabled in these two conditions:
- at least two job offers, in case there is no current job
- at least one job offer, in case there is a current job



2.When choosing to enter current job details, a user will:

a. Be shown a user interface to enter (if it is the first time) or edit all of the details of
their current job, which consist of:
 i. Title
 ii. Company
 iii. Location (entered as city and state)
 iv. Cost of living in the location (expressed as an index )
 v. Yearly salary
 vi. Yearly bonus
 vii. Allowed weekly telework days (expressed as the number of days per
week allowed for remote work, inclusively between 0 and 5)
 viii. Retirement benefits (as percentage matched)
 ix. Leave time (vacation days and holiday and/or sick leave, as a single
overall number of days)
b. Be able to either save the job details or cancel and exit without saving, returning
in both cases to the main menu.


In this part,  a subclass 'CurrentJob' has been created, it is shown as follows:

![](https://github.com/tyang373/CS6300UML_pics/blob/main/CurrentJob%20subclass.jpg?raw=true)

- It inherits all the attributes and method or operations of its superclass 'Jobs'. For example： `title`, `company` and `location` of attributes, `save or cancel` of operations.

The 'Jobs' class is shown as follows:

![](https://github.com/tyang373/CS6300UML_pics/blob/main/Job%20Class.jpg?raw=true)

- The only attribute I set for this subclass is a boolean `IsFirstTime`, if it is true, the user should enter the current job information.If it is false, the user can edit the current job information.


3.When choosing to enter job offers, a user will:
a. Be shown a user interface to enter all of the details of the offer, which are the
same ones listed above for the current job.
b. Be able to either save the job offer details or cancel.
c. Be able to (1) enter another offer, (2) return to the main menu, or (3) compare the
offer (if they saved it) with the current job details (if present).

In this part, I define 'job offers' as subclass of 'jobs' class is due to that it has the same ones listed for the current job. It is shown as follows:

![](https://github.com/tyang373/CS6300UML_pics/blob/main/JobOffer%20subclass.jpg?raw=true)

- It inherits all the attributes and method or operations of its superclass 'Jobs'. For example： `title`, `company` and `location` of attributes, `save or cancel` of operations.
- The specific attribute is a boolean `IsSaved`. If it is true, it means this job offer has been saved, the compare job offers class could be call to compare the job offer with current job offers.
- It also has another function `Enter another offer`.


4.When adjusting the comparison settings, the user can assign integer weights to:
 a. Yearly salary
 b. Yearly bonus
 c. Allowed weekly telework days
 d. Retirement benefits
 e. Leave time
If no weights are assigned, all factors are considered equal.

In this part, I consider the comparison settings as a method incoporated into the 'Compare Job Offers' class. It is shown as follows:

![](https://github.com/tyang373/CS6300UML_pics/blob/main/CompareJobOffer%20class.jpg?raw=true)

-when user wants to request for the job comparison, the first step is to do comparsion setting to assign the integer weights for the listed attribute above.



5.When choosing to compare job offers, a user will:
 a. Be shown a list of job offers, displayed as Title and Company, ranked from best
to worst (see below for details), and including the current job (if present), clearly
indicated.
 b. Select two jobs to compare and trigger the comparison.
 c. Be shown a table comparing the two jobs, displaying, for each job:
 i. Title
 ii. Company
 iii. Location
 iv. Yearly salary adjusted for cost of living
 v. Yearly bonus adjusted for cost of living
 vi. Allowed weekly telework days
 vii. Retirement benefits (as percentage matched)
 viii. Leave time
 d. Be offered to perform another comparison or go back to the main menu.

In this part, the class 'Compare Job Offers' has been created. It has the list attribute `Joblist`, which will be display String array `Title` and `Company`. It is shown as follows:

![](https://github.com/tyang373/CS6300UML_pics/blob/main/CompareJobOffer%20class.jpg?raw=true).

When the user requests for comparing job offers. Several steps will be operated:

- The 'Compare Job class' will make use of 'Jobs'class to pick up the necessary attributes.
- User will be asked to assign weight value for the attributes required.
- Then it ranks the joboffers from best to worst and display for user.
- Then user select two job offers (current job may include) for comparsion.
- The result will be returned to the user.


6.When ranking jobs, a job’s score is computed as the weighted sum of:
AYS + AYB + (RBP * AYS) + (LT * AYS / 260) - ((260 - 52 * RWT) * (AYS / 260) / 8)
where:

- AYS = yearly salary adjusted for cost of living
- AYB = yearly bonus adjusted for cost of living
- RBP = retirement benefits percentage
- LT = leave time
- RWT = telework days per week

The rationale for the RWT subformula is:
a. value of an employee hour = (AYS / 260) / 8
b. commute hours per year (assuming a 1-hour/day commute) =1 * (260 - 52 * RWT)
c. therefore travel-time cost = (260 - 52 * RWT) * (AYS / 260) / 8

For example, if the weights are 2 for the yearly salary, 2 for the retirement benefits, and
1 for all other factors, the score would be computed as:
2/7 * AYS + 1/7 * AYB + 2/7 * (RBP * AYS) + 1/7 * (LT * AYS / 260) - 1/7 * ((260 - 52 *RWT) * (AYS / 260) / 8)

In this part, rank will be consider as a method for calling in the 'Compare Job Offers'class.Just follow this rules to calculate the temporary attribute `score` for ranking from best to worst.

7.The user interface must be intuitive and responsive.
8.For simplicity, you may assume there is a single system running the app (no
communication or saving between devices is necessary).

For 7 and 8 requirements, they will be considered.

The whole UML of this app is as follows:

![](https://github.com/tyang373/CS6300UML_pics/blob/main/design.jpg?raw=true)