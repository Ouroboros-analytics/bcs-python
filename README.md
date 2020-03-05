# BCS-Python

## Getting started:

Your credentials for login are pulled from the environment variables `BCS_USER` (your email) and `BCS_PASS` (your password). You'll need to set them in your workspace.

In bash/zsh:

```bash
export BCS_USER="my.email@email.com"
export BCS_PASS="mypassword123"
```

In Windows Command Prompt:

```cmd
set BCS_USER="my.email@email.com"
set BCS_PASS="january1st1970"
```

## Using the Wrapper:

### Basic Usage

With your environment variables set, your code will look something like this:

```python
>>> from bcswrapper import Bootcampspot

>>> bcs = Bootcampspot()

>>> print(bcs)
[{'courseName':'UT-MUNICH-UXUI-12-2042-U-C-FT','courseId':1234, 'enrollmentId': 123456}]

>>> bcs.user
{'id': 42,
 'userName': 'my.email@email.com',
 'firstName': 'John',
 'lastName': 'Doe',
 'email': 'my.email@email.com',
 'githubUserName': 'MyPasswordIsMyBirthday',
...
```

### Getting Fancier

Setting the course will set your enrollment. Your `courseId` is the most specific identifier for a course so it's a good place to start.

```python
# Setting the course will set your enrollment
>>> bcs.course = 1234

>>> bcs.enrollment
123456
```

But how do I find out my courseId? _Well since you asked_:

```python
# After class construction, call bcs.courses at any time to get a list of records
# with courseId, enrollmentId and courseName

>>> bcs.courses

[{'courseName':'UT-MUNICH-UXUI-12-2042-U-C-MW','courseId':1234, 'enrollmentId': 123456},
{'courseName':'UT-MUNICH-UXUI-12-2042-U-C-TTH','courseId':2345, 'enrollmentId': 234567}]
```

This is also tied, _temporarily_, to the `__repr__` function for the `Bootcampspot` class so printing your instance will give you the same functionality.

Don't worry about messing it up either! If, at any time, you enter an invalid `courseId` or `enrollmentId`, the accompanying error message will give you a list of valid ids to work with.

If you haven't set your course or you want to use a different course, each method that requires a property as a parameter will take the course/enrollment/name as a keyword argument.

```python
# Setting the course isn't permanent
>>> bcs.course = 1234

>>> bcs.grades(courseId=2345)
{'0: Prework':{
    'Abe Lincoln': 'A',
    'Clifford the Dog': 'Unsubmitted',
    'Jane Doe': 'Incomplete'},
'1: Hello, World':{
    'Abe Lincoln': 'A+',
    'Clifford the Dog': 'B',
    'Jane Doe': 'Incomplete'}}

# Without that kwarg the method will default to bcs.course
>>> bcs.grades()
{'0: Prework':{
    'Anya Novak': 'A',
    'Ken Burns': 'Unsubmitted',
    'Sterling Archer': 'Incomplete'},
'1: Hello, World':{
    'Anya Novak': 'A+',
    'Ken Burns': 'B',
    'Sterling Archer': 'Incomplete'}}
```

To get attendance, call the attendance method. Like the grades method, it will use your set `courseId` as a default. The resulting data structure (as are all the outputs) is designed to be table friendly so it can be called directly into a DataFrame instance.

```python
>>> import pandas as pd

>>> df = pd.DataFrame(bcs.attendance())

# Insert table here
```

**@TODO**:

- sessions: all calendar sessions **prototyped**
- session_closest: closest session to right now **prototyped**
- session_details: info on a given session (Also all student info?!) **prototyped**
- students: info on students for a courseId
- weekly_feedback
- assignments
- tests: this was designed to be test driven but I got a little carried away last night
