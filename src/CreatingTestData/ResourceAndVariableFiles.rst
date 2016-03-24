Resource and variable files
===========================

..    User keywords and variables in `test case files`_ and `test suite
    initialization files`_ can only be used in files where they are
    created, but *resource files* provide a mechanism for sharing them. Since
    the resource file structure is very close to test case files, it is
    easy to create them.

`test case files`_ 와 `test suite initialization files`_ 내의 사용자
키워드와 변수는 생성되어진 파일 내에서만 사용가능하다. 하지만
*리소스파일(resource files)* 은 공유할 수 있는 메커니즘을 제공한다.
리소스 파일 구조는 테스트 케이스 파일과 비슷하기 때문에 쉽게 만들 수
있다.

..
   *Variable files* provide a powerful mechanism for creating and sharing
   variables. For example, they allow values other than strings and
   enable creating variables dynamically. Their flexibility comes from
   the fact that they are created using Python code, which also makes
   them somewhat more complicated than `Variable tables`_.

*변수 파일(Varialbe files)* 은 변수를 생성하고 공유하기 위한 강력한
메카니즘을 제공한다. 예를 들어 변수파일은 문자열 이외의 값을 허용하고
동적으로 변수를 생성 할 수 있다. 파이썬 코드를 사용하여 생성하기
때문에 좀 더 유연하다. 하지만 그렇기 때문에 `Variable tables`_ 보다는
좀 더 복잡하다.

.. contents::
   :depth: 2
   :local:

Resource files
--------------

Taking resource files into use
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Resource files are imported using the :setting:`Resource` setting in the
   Settings table. The path to the resource file is given in the cell
   after the setting name.

리소스파일은 설정표(Setting table)에서 :setting:`Resource` 설정을 통해
읽어들일 수 있다. 리소스파일의 경로는 설정이름(Resource) 다음 셀에
주어진다.

..
   If the path is given in an absolute format, it is used directly. In other
   cases, the resource file is first searched relatively to the directory
   where the importing file is located. If the file is not found there,
   it is then searched from the directories in Python's `module search path`_.
   The path can contain variables, and it is recommended to use them to make paths
   system-independent (for example, :file:`${RESOURCES}/login_resources.html` or
   :file:`${RESOURCE_PATH}`). Additionally, slashes (`/`) in the path
   are automatically changed to backslashes (:codesc:`\\`) on Windows.
   
리소스 파일은 만약 경로가 절대경로 형식으로 주어진다면 바로 사용된다.
절대경로가 아닌 경우, 처음에는 읽혀진 파일의 디렉토리 위치에 대해서
상대적인 위치로 검색한다. 만약 파일이 거기서 발견되지 않는다면 파이썬
`module search path`_ 내의 디렉토리들을 검색한다. 경로는 변수를 포함할
수 있다. 이 경우 시스템 독립적인 경로를 만들 수 있어 권장된다(예,
:file:`${RESOURCES}/login_resources.html` 또는
:file:`${RESOURCE_PATH}`). 추가적으로 경로 내의 슬래쉬('/')는
윈도우즈에서는 자동적으로 두개의 백슬래쉬(:codesc:`\\`)로 변경된다.

.. sourcecode:: robotframework

   *** Settings ***
   Resource    myresources.html
   Resource    ../data/resources.html
   Resource    ${RESOURCES}/common.tsv

..
   The user keywords and variables defined in a resource file are
   available in the file that takes that resource file into
   use. Similarly available are also all keywords and variables from the
   libraries, resource files and variable files imported by the said
   resource file.

리소스 파일에 정의된 사용자 키워드와 변수는 파일안에서 리소스파일을
읽어들이면 사용가능하다. 리소스파일에서 읽혀진 라이러리리와 리소스파일
및 변수파일 내의 모든 키워드와 변수도 사용가능하다.

Resource file structure
~~~~~~~~~~~~~~~~~~~~~~~

..
   The higher-level structure of resource files is the same as that of
   test case files otherwise, but, of course, they cannot contain Test
   Case tables. Additionally, the Setting table in resource files can
   contain only import settings (:setting:`Library`, :setting:`Resource`,
   :setting:`Variables`) and :setting:`Documentation`. The Variable table and
   Keyword table are used exactly the same way as in test case files.

좀 더 높은 수준의 구조로 이루어진 리소스파일은 테스트 케이스 파일과
동일하지만 테스트 케이스 표를 포함하지는 않는다. 추가적으로
리소스파일의 설정표는 단지 문서화(:setting:`Documentation`)와 설정을
임포트 하는 것(:setting:`Library`, :setting:`Resource`,
:setting:`Variables`)을 포함한다.

..
   If several resource files have a user keyword with the same name, they
   must be used so that the `keyword name is prefixed with the resource
   file name`__ without the extension (for example, :name:`myresources.Some
   Keyword` and :name:`common.Some Keyword`). Moreover, if several resource
   files contain the same variable, the one that is imported first is
   taken into use.

..
   __ `Handling keywords with same names`_

만약 여러개의 리소스 파일이 동일한 이름의 사용자 키워드를 가진다면,
키워드는 반드시 `접두어로 확장자 없는 리소스 파일이름`__ 을 가져야
한다. (예, :name:`myresources.Some Keyword`, :name:`common.Some
Keyword`) 게다가, 만약 여러 리소스파일이 동일한 이름의 변수를 가진다면
첫번째로 임포트된 것을 사용한다.


__ `Handling keywords with same names`_

Documenting resource files
~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Keywords created in a resource file can be documented__ using
   :setting:`[Documentation]` setting. The resource file itself can have
   :setting:`Documentation` in the Setting table similarly as
   `test suites`__.

리소스파일에 생성된 키워드는 :setting:`[Documentation]` 을 사용하여
`문서화`__ 할 수 있다. 리소스파일 자체는 `테스트 스위트`__ 와 비슷한게
설정표에 :setting:`Documentation` 할 수 있다.

Both Libdoc_ and RIDE_ use these documentations, and they
are naturally available for anyone opening resource files.  The
first line of the documentation of a keyword is logged when it is run,
but otherwise resource file documentations are ignored during the test
execution.

Libdoc_ 과 RIDE_ 둘다 이 문서화를 사용하고, 누구나 리소스파일을 열어서
이용가능하다. 키워드 문서화의 첫째줄은 키워드가 수행될때 로그로 남지만
리소스파일의 문서화는 테스트가 수행되는 동안 무시된다.

__ `User keyword name and documentation`_
__ `Test suite name and documentation`_

Example resource file
~~~~~~~~~~~~~~~~~~~~~

.. sourcecode:: robotframework

   *** Settings ***
   Documentation     An example resource file
   Library           Selenium2Library
   Resource          ${RESOURCES}/common.robot

   *** Variables ***
   ${HOST}           localhost:7272
   ${LOGIN URL}      http://${HOST}/
   ${WELCOME URL}    http://${HOST}/welcome.html
   ${BROWSER}        Firefox

   *** Keywords ***
   Open Login Page
       [Documentation]    Opens browser to login page
       Open Browser    ${LOGIN URL}    ${BROWSER}
       Title Should Be    Login Page

   Input Name
       [Arguments]    ${name}
       Input Text    username_field    ${name}

   Input Password
       [Arguments]    ${password}
       Input Text    password_field    ${password}

Variable files
--------------

..
   Variable files contain variables_ that can be used in the test
   data. Variables can also be created using variable tables or set from
   the command line, but variable files allow creating them dynamically
   and their variables can contain any objects.

변수파일은 테스트 데이타안에 사용할 수 있는 variables_ 를 포함한다.
변수는 또한 변수표를 사용하여 생성될 수 있고, 명령행에서 설정할 수
있다. 그러나 변수파일은 동적으로 변수를 생성할 수 있고 변수는 임의의
객체를 포함 할 수 있다.

..
   Variable files are typically implemented as Python modules and there are
   two different approaches for creating variables:

보통 변수 파일은 파이썬 모듈로 구현되어지고 변수를 생성하기 위해서는
두가지 다른 접근법이 있다:

..
   `Creating variables directly`_
      Variables are specified as module attributes. In simple cases, the
      syntax is so simple that no real programming is needed. For example,
      `MY_VAR = 'my value'` creates a variable
      `${MY_VAR}` with the specified text as the value.

`Creating variables directly`_
   변수는 모듈 속성으로 기술된다. 가장 간단한 경우로 구문은 매우
   간단하고 실제 프로그래밍은 필요하지 않다. 예를 들어 `MY_VAR = 'my
   value'` 는 값으로 기술된 문자를 가지는 변수 `${MY_VAR}` 를
   생성한다.
   
..
   `Getting variables from a special function`_
      Variable files can have a special `get_variables`
      (or `getVariables`) method that returns variables as a mapping.
      Because the method can take arguments this approach is very flexible.

`Getting variables from a special function`_
   변수파일은 매핑된 변수들을 리턴하는 특별한 `get_variables`(또는
   `getVariables`) 메소드를 가진다. 해당 메소드는 전달인자를 가질수
   있어 이 방법은 매우 유연하다.

..
   Alternatively variable files can be implemented as `Python or Java classes`__
   that the framework will instantiate. Also in this case it is possible to create
   variables as attributes or get them from a special method.

다른 대안으로 변수파일은 `파이썬이나 자바 클래스`__ 로 구현할 수 있고
프레임워크는 인스턴스화할 것이다. 이 경우 속성(attributes)을 변수로
생성하거나 특별한 메소드로 변수를 얻을 수 있다.


__ `Implementing variable file as Python or Java class`_

Taking variable files into use
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Setting table
'''''''''''''

..
   All test data files can import variables using the
   :setting:`Variables` setting in the Setting table, in the same way as
   `resource files are imported`__ using the :setting:`Resource`
   setting. Similarly to resource files, the path to the imported
   variable file is considered relative to the directory where the
   importing file is, and if not found, it is searched from the
   directories in the `module search path`_. The path can also contain variables,
   and slashes are converted to backslashes on Windows. If an `argument file takes
   arguments`__, they are specified in the cells after the path and also they
   can contain variables.

모든 테스트 데이타 파일은 설정표안에 :setting:`Variables` 를 설정하여
변수를 임포트 할 수 있다. :setting:`Resource` 설정으로 `리소스파일
임포트`__ 도 동일한 방법으로 임포트 가능하다. 리소스파일과 비슷하게
임포트된 변수파일의 경로는 임포트된 파일이 존재하는 디렉토리에 대하여
상대적으로 고려한다. 만약 찾을 수 없다면 `module search path`_ 내의
디렉토리를 검색한다. 경로는 변수와 슬래쉬(`/`, 윈도우즈에서는 `\\` 로
치환됨)를 포함할 수 있다. 만약 `전달인자 파일이 전달인자를 가진다`__
면 경로 다음 셀에 기술해야 한다. 전달인자는 변수를 포함할 수 있다.


__ `Taking resource files into use`_
__ `Getting variables from a special function`_

.. sourcecode:: robotframework

   *** Settings ***
   Variables    myvariables.py
   Variables    ../data/variables.py
   Variables    ${RESOURCES}/common.py
   Variables    taking_arguments.py    arg1    ${ARG2}

All variables from a variable file are available in the test data file
that imports it. If several variable files are imported and they
contain a variable with the same name, the one in the earliest imported file is
taken into use. Additionally, variables created in Variable tables and
set from the command line override variables from variable files.

변수파일을 임포트한 테스트 데이타 파일은 변수파일의 모든 변수를 사용할
수 있다. 만약 여러 변수파일이 임포트 되고 그 파일들이 동일한 이름의
변수를 포함한다면 가장 먼저 임포트된 파일안의 변수를 사용한다.
추가적으로 변수표에서 생성된 변수는 명령행라인에서 변수파일을 전달하여
오버라이드할 수 있다.

Command line
''''''''''''

..
   Another way to take variable files into use is using the command line option
   :option:`--variablefile`. Variable files are referenced using a path to them,
   and possible arguments are joined to the path with a colon (`:`)::

변수파일을 사용하는 다른 방법은 명령행 라인 옵션
:option:`--variablefile` 을 사용하는 것이다. 변수파일은 경로를
사용하여 참조할 수 있다. 콜론(`:`)을 경로뒤에 붙여서 가능한 변수를
선언할 수 있다::

   --variablefile myvariables.py
   --variablefile path/variables.py
   --variablefile /absolute/path/common.py
   --variablefile taking_arguments.py:arg1:arg2

..
   Starting from Robot Framework 2.8.2, variable files taken into use from the
   command line are also searched from the `module search path`_ similarly as
   variable files imported in the Setting table.

로봇프레임워크 2.8.2부터 명령행라인에 사용된 변수파일은 설정표에
임포트된 변수파일과 비슷하게 `module search path`_ 에서 찾는다.

..
   If a variable file is given as an absolute Windows path, the colon after the
   drive letter is not considered a separator::

만약 변수파일이 윈도우즈 절대경로로 주어진다면 드리이브 문자뒤의
콜론은 전달인자 분리자로 인식되지 않는다:;

   --variablefile C:\path\variables.py

..
   Starting from Robot Framework 2.8.7, it is also possible to use a semicolon
   (`;`) as an argument separator. This is useful if variable file arguments
   themselves contain colons, but requires surrounding the whole value with
   quotes on UNIX-like operating systems::

로봇프레임워크 2.8.7부터 전달인자 분리자로 세미콜론(`;`)을 사용할 수
있다. 변수파일 전달인자 내에 콜론이 포함된 경우 유용하다. 그러나
UNIX-like 운영 체제는 전체를 인용부호(")로 감싸야 한다::
  
   --variablefile "myvariables.py;argument:with:colons"
   --variablefile C:\path\variables.py;D:\data.xls

..
   Variables in these variable files are globally available in all test data
   files, similarly as `individual variables`__ set with the
   :option:`--variable` option. If both :option:`--variablefile` and
   :option:`--variable` options are used and there are variables with same
   names, those that are set individually with
   :option:`--variable` option take precedence.

변수파일내의 변수는 모든 테스트 데이타 파일안에서 :option:`--variable`
옵션으로 설정하는 `개별 변수들`__ 처럼 전역적으로 사용가능하다. 만약
:option:`--variablefile` 과 :option:`--variable` 옵션이 둘다
사용되어지고, 동일한 이름의 변수가 존재 한다면 우선적으로
:option:`--variable` 로 선언된 것이 개별적으로 설정된다.
	
__ `Setting variables in command line`_

Creating variables directly
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Basic syntax
''''''''''''

..
   When variable files are taken into use, they are imported as Python
   modules and all their global attributes that do not start with an
   underscore (`_`) are considered to be variables. Because variable
   names are case-insensitive, both lower- and upper-case names are
   possible, but in general, capital letters are recommended for global
   variables and attributes.

변수파일을 사용할때 파이썬 모듈로 임포트되고 언더스코어(`_`)로 시작하지 않는
모든 전역 속성은 변수로 취급된다. 변수이름은 대소문자 구분이 없기
때문에 소문자, 대문자 이름이 모두 가능하다. 하지만 일반적으로 전역
변수와 속성은 대문자사용이 권장된다.

.. sourcecode:: python

   VARIABLE = "An example string"
   ANOTHER_VARIABLE = "This is pretty easy!"
   INTEGER = 42
   STRINGS = ["one", "two", "kolme", "four"]
   NUMBERS = [1, INTEGER, 3.14]
   MAPPING = {"one": 1, "two": 2, "three": 3}

..
   In the example above, variables `${VARIABLE}`, `${ANOTHER VARIABLE}`, and
   so on, are created. The first two variables are strings, the third one is
   an integer, then there are two lists, and the final value is a dictionary.
   All these variables can be used as a `scalar variable`_, lists and the
   dictionary also a `list variable`_ like `@{STRINGS}` (in the dictionary's case
   that variable would only contain keys), and the dictionary also as a
   `dictionary variable`_ like `&{MAPPING}`.

위의 예제에서 변수 `${VARIABLE}`, `${ANOTHER VARIABLE}`, 등등이
생성된다. 처음 두 변수는 문자열이다. 세번째는 정수다. 네번째,
다섯번째는 리스트고 마지막 값은 사전이다. 이 모든 변수는 `scalar
variable`_ , 리스트로 사용할 수 있다. 사전형은 `@{STRINGS}` 같은 `list
variable`_ 로 사용가능하고(사전의 경우 변수는 단지 키를 포함한다.) ,
`&{MAPPING}` 같은 `dictionary variable`_ 로도 사용가능하다.

..
   To make creating a list variable or a dictionary variable more explicit,
   it is possible to prefix the variable name with `LIST__` or `DICT__`,
   respectively:

더욱 명시적으로 리스트 변수나 사전 변수를 만들기 위하여 변수이름 앞에
각각 `LIST__` 또는 `DICT__` 접두어를 사용할 수 있다.

.. sourcecode:: python

   from collections import OrderedDict

   LIST__ANIMALS = ["cat", "dog"]
   DICT__FINNISH = OrderedDict([("cat", "kissa"), ("dog", "koira")])

..
   These prefixes will not be part of the final variable name, but they cause
   Robot Framework to validate that the value actually is list-like or
   dictionary-like. With dictionaries the actual stored value is also turned
   into a special dictionary that is used also when `creating dictionary
   variables`_ in the Variable table. Values of these dictionaries are accessible
   as attributes like `${FINNISH.cat}`. These dictionaries are also ordered, but
   preserving the source order requires also the original dictionary to be
   ordered.

이런 접두어는 변수이름에 포함되지 않고 로봇프레임워크가 값이 실제
리스트형 또는 사전형 인지 검사하는데 사용한다. 사전에서 실제 저장하는
값은 특별한 사전으로 변환된다. 이 사전은 변수표안에서 `creating
dictionary variables`_ 할때 사용되어지는 것과 같다. 이런 사전의 값은
`${FINNISH.cat}` 처럼 속성으로 접근 가능하다. 이런 사전은 또한
정렬(ordered)되어진다. 원본 순서를 유지되기 위해서는 원본 사전이
정렬되어 있어야 한다.

..
   The variables in both the examples above could be created also using the
   Variable table below.

위의 예제 모두 아래 변수표를 사용하여 생성될 수도 있다.

.. sourcecode:: robotframework

   *** Variables ***
   ${VARIABLE}            An example string
   ${ANOTHER VARIABLE}    This is pretty easy!
   ${INTEGER}             ${42}
   @{STRINGS}             one          two           kolme         four
   @{NUMBERS}             ${1}         ${INTEGER}    ${3.14}
   &{MAPPING}             one=${1}     two=${2}      three=${3}
   @{ANIMALS}             cat          dog
   &{FINNISH}             cat=kissa    dog=koira

..
   .. note:: Variables are not replaced in strings got from variable files.
	     For example, `VAR = "an ${example}"` would create
	     variable `${VAR}` with a literal string value
	     `an ${example}` regardless would variable `${example}`
	     exist or not.

.. note:: 변수는 변수파일로 부터 얻어진 문자열로 치환되지 않는다. 예를
          들어, `VAR = "an ${example}"` 은 `${example}` 변수의
          존재유무에 상관없이 문자 그대로의 값 `an ${exaple}` 을
          값으로 가지는 `${VAR}` 변수를 생성한다.

   

Using objects as values
'''''''''''''''''''''''

..
   Variables in variable files are not limited to having only strings or
   other base types as values like variable tables. Instead, their
   variables can contain any objects. In the example below, the variable
   `${MAPPING}` contains a Java Hashtable with two values (this
   example works only when running tests on Jython).

변수파일내의 변수는 변수표처럼 문자열 또는 다른 기본 유형의 값으로
제한되지 않는다. 변수파일내의 변수는 임의의 객체를 포함 할 수 있다.
아래 예제에서 변수 `${MAPPING}` 은 두개의 값을 가지는 Java Hashtable을
가진다.(이 예제는 Jython에서만 동작한다.)

.. sourcecode:: python

    from java.util import Hashtable

    MAPPING = Hashtable()
    MAPPING.put("one", 1)
    MAPPING.put("two", 2)

..
   The second example creates `${MAPPING}` as a Python dictionary
   and also has two variables created from a custom object implemented in
   the same file.

두번째 예제는 파이썬 사전으로 `${MAPPING}` 변수를 생성하였다. 또 다른
두 변수는 동일 파일에서 사용자 객체로 생성되었다.

.. sourcecode:: python

    MAPPING = {'one': 1, 'two': 2}

    class MyObject:
        def __init__(self, name):
            self.name = name

    OBJ1 = MyObject('John')
    OBJ2 = MyObject('Jane')

Creating variables dynamically
''''''''''''''''''''''''''''''
..
   Because variable files are created using a real programming language,
   they can have dynamic logic for setting variables.


변수 파일은 프로그래밍 언어를 사용하여 생성되기 때문에 동적인 로직을
통해 변수를 설정할 수 있다.

.. sourcecode:: python

   import os
   import random
   import time

   USER = os.getlogin()                # current login name
   RANDOM_INT = random.randint(0, 10)  # random integer in range [0,10]
   CURRENT_TIME = time.asctime()       # timestamp like 'Thu Apr  6 12:45:21 2006'
   if time.localtime()[3] > 12:
       AFTERNOON = True
   else:
       AFTERNOON = False


..
   The example above uses standard Python libraries to set different
   variables, but you can use your own code to construct the values. The
   example below illustrates the concept, but similarly, your code could
   read the data from a database, from an external file or even ask it from
   the user.

위의 예제에서 표준 파이썬 라이브러리가 다른 변수를 설정하기 위해
사용되지만 자시만의 코드를 사용하여 그 값을 구성할 수도 있다. 아래
예제는 개념을 묘사한다. 이와 비슷하게 데이타베이스나 외부 파일 또는
사용자로부터 입력받은 데이타를 읽는 코드를 사용할 수도 있다.

.. sourcecode:: python

    import math

    def get_area(diameter):
        radius = diameter / 2
        area = math.pi * radius * radius
        return area

    AREA1 = get_area(1)
    AREA2 = get_area(2)

Selecting which variables to include
''''''''''''''''''''''''''''''''''''

..
   When Robot Framework processes variable files, all their attributes
   that do not start with an underscore are expected to be
   variables. This means that even functions or classes created in the
   variable file or imported from elsewhere are considered variables. For
   example, the last example would contain the variables `${math}`
   and `${get_area}` in addition to `${AREA1}` and
   `${AREA2}`.

로봇프레임워크는 변수파일을 처리할때 언더스코어로 시작하지 않는
파일의 모든 속성은 변수가 될 것으로 기대한다. 이것이 의미하는 바는
변수파일안에 생성된 함수나 클래스 또는 다른 곳에에 임포든 된 것들 조차
변수로 고려된다는 것이다. 예를 들어, 마지막 예제는 `${math}`,
`${get_area}` 및 `${AREA1}`, `${AREA2}` 를 변수로 가진다.

..
   Normally the extra variables do not cause problems, but they
   could override some other variables and cause hard-to-debug
   errors. One possibility to ignore other attributes is prefixing them
   with an underscore:

보통 여분의 변수는 문제를 발생시키니는 않지만 다른 변수를 오버라이드
할 수도 있고 디버깅하기 어려운 에러를 야기할 수도 있다. 무시해야 할
속성은 앞에 언더스코어를 붙여 사용하자:

.. sourcecode:: python

    import math as _math

    def _get_area(diameter):
        radius = diameter / 2.0
        area = _math.pi * radius * radius
        return area

    AREA1 = _get_area(1)
    AREA2 = _get_area(2)

..
   If there is a large number of other attributes, instead of prefixing
   them all, it is often easier to use a special attribute
   `__all__` and give it a list of attribute names to be processed
   as variables.

무시해야 할 속성이 많은 경우, 언더스코어를 붙이는 대신 특별한 속성
`__all__` 을 사용하는 것이 더 쉽다. 이것은 변수로 처리해야 하는
속성이름의 목록을 제공한다.

.. sourcecode:: python

    import math

    __all__ = ['AREA1', 'AREA2']

    def get_area(diameter):
        radius = diameter / 2.0
        area = math.pi * radius * radius
        return area

    AREA1 = get_area(1)
    AREA2 = get_area(2)

..
   .. Note:: The `__all__` attribute is also, and originally, used
	     by Python to decide which attributes to import
	     when using the syntax `from modulename import *`.

.. note:: `__all__` 속성은 원래 파이썬에서 `from modulname import *`
          문법을 사용하여 임포트해야 할 속성을 결정할때 사용한다.

Getting variables from a special function
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   An alternative approach for getting variables is having a special
   `get_variables` function (also camelCase syntax
   `getVariables` is possible) in a variable file. If such a function
   exists, Robot Framework calls it and expects to receive variables as
   a Python dictionary or a Java `Map` with variable names as keys
   and variable values as values. Created variables can be used as scalars,
   lists, and dictionaries exactly like when `creating variables directly`_,
   and it is possible to use `LIST__` and `DICT__` prefixes to make creating
   list and dictionary variables more explicit. The example below is functionally
   identical to the first `creating variables directly`_ example.

변수를 가져오는 다른 방법은 변수 파일에 특별한 `get_variables`
함수(camelCase 문법 `getVariables` 도 가능)를 만들면 된다. 만약 함수가
존재한다면 로봇프레임워크는 그 함수를 호출하여 키로 변수 이름을 가지고
값으로 변수 값을 가지는 파이썬 사전 또는 자바 `Map` 을 변수로 받기를
기대한다. 생성된 변수는 `creating variables directly`_ 때 처럼 스칼라,
리스트, 사전으로 사용할 수 있다. 더욱 명시적으로 리스트와 사전 변수를
생성하기 위해 `LIST__` 와 `DICT__` 접두어를 사용할 수 있다. 아래
예제에서 첫번째 `creating variables directly`_ 예제와 기능적으로
동일하다.

.. sourcecode:: python

    def get_variables():
        variables = {"VARIABLE ": "An example string",
                     "ANOTHER VARIABLE": "This is pretty easy!",
                     "INTEGER": 42,
                     "STRINGS": ["one", "two", "kolme", "four"],
                     "NUMBERS": [1, 42, 3.14],
                     "MAPPING": {"one": 1, "two": 2, "three": 3}}
        return variables

..
   `get_variables` can also take arguments, which facilitates changing
   what variables actually are created. Arguments to the function are set just
   as any other arguments for a Python function. When `taking variable files
   into use`_ in the test data, arguments are specified in cells after the path
   to the variable file, and in the command line they are separated from the
   path with a colon or a semicolon.

`get_variables` 는 전달인자를 가질 수 있다. 이것은 실제 생성될 어떤
변수를 바꾸는 것을 용이하게 한다. 함수에 대한 전달인자는 파이썬 함수를
위한 임의의 다른 전달인자와 같이 설정된다. 테스트 데이터 내에서
`변수파일을 사용`__ 했을 때, 전달인자는 변수파일 경로뒤의 셀에
명기해야 한다. 명령행에서는 콜론 또는 세미콜론을 사용하여 경로와 구분한다.

__ `taking variable files into use`_

..
   The dummy example below shows how to use arguments with variable files. In a
   more realistic example, the argument could be a path to an external text file
   or database where to read variables from.

아래의 더미 예제는 변수파일에 전달인자를 사용하는 것을 보여준다. 좀 더
현실적인 예제에서 전달인자로 외부의 텍스트 파일이거나 데이터베이스에
대한 경로 일 수도 있다.

.. sourcecode:: python

    variables1 = {'scalar': 'Scalar variable',
                  'LIST__list': ['List','variable']}
    variables2 = {'scalar' : 'Some other value',
                  'LIST__list': ['Some','other','value'],
                  'extra': 'variables1 does not have this at all'}

    def get_variables(arg):
        if arg == 'one':
            return variables1
        else:
            return variables2

Implementing variable file as Python or Java class
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Starting from Robot Framework 2.7, it is possible to implement variables files
   as Python or Java classes.

로봇프레임워크 2.7이후로 파이썬 또는 자바 클래스로 변수 파일을 구현할 수 있다.

Implementation
''''''''''''''

..
   Because variable files are always imported using a file system path, creating
   them as classes has some restrictions:

  ..
     - Python classes must have the same name as the module they are located.
     - Java classes must live in the default package.
     - Paths to Java classes must end with either :file:`.java` or :file:`.class`.
       The class file must exists in both cases.

변수파일은 항상 파일 시스템 경로를 사용하여 임포트되어지기 때문에
클래스로 생성할 경우 몇가지 제한을 가진다:


  - 파이썬 클래스는 반드시 자신이 위치하는 모듈과 같은 이름을 가져야 한다.
  - 자바 클래스는 반드시 기본 패키지 않에 있어야 한다.
  - 자바 클래스에 대한 경로는 반드시 :file:`java` 또는 file:`class` 로
    끝나야 하고, 두 경우 모두 클래스 파일이 존재해야 한다.
    
..
   Regardless the implementation language, the framework will create an instance
   of the class using no arguments and variables will be gotten from the instance.
   Similarly as with modules, variables can be defined as attributes directly
   in the instance or gotten from a special `get_variables`
   (or `getVariables`) method.

구현 언어에 관계없이 프레임워크는 전달인자 없는 것을 사용하여 클래스의
인스턴스를 생성할 것이다. 변수는 해당 인스턴스로부터 얻을 수 있게
된다. 모듈과 비슷하게 변수는 직접적으로 인스턴스내의 속성들로 정의
되거나 `get_variables`(or `getVariables`) 메소드로 부터 얻을 수 있다.

..
   When variables are defined directly in an instance, all attributes containing
   callable values are ignored to avoid creating variables from possible methods
   the instance has. If you would actually need callable variables, you need
   to use other approaches to create variable files.

변수가 인스턴스에서 직접 정의 될때 호출 가능한 값을 포함하는 모든
속성은 인스턴스가 가지는 가능한 메소드들로 부터 변수를 생성하는 것을
피하기 위해 무시된다. 만약 호출가능한 변수가 필요하다면 변수 파일을
생성하기 위한 다른 방법을 사용해야 한다.

Examples
''''''''

..
   The first examples create variables from attributes using both Python and Java.
   Both of them create variables `${VARIABLE}` and `@{LIST}` from class
   attributes and `${ANOTHER VARIABLE}` from an instance attribute.

첫번째 예제는 파이썬과 자바를 사용하여 속성으로 부터 변수를 생성한다.
클래스 속성에서 `${VARIABLE}` 와 `@{LIST}` 를 생성하고 인스턴스
속성에서 `${ANOTHER VARIABLE}` 을 생성한다.

.. sourcecode:: python

    class StaticPythonExample(object):
        variable = 'value'
        LIST__list = [1, 2, 3]
        _not_variable = 'starts with an underscore'

        def __init__(self):
            self.another_variable = 'another value'

.. sourcecode:: java

    public class StaticJavaExample {
        public static String variable = "value";
        public static String[] LIST__list = {1, 2, 3};
        private String notVariable = "is private";
        public String anotherVariable;

        public StaticJavaExample() {
            anotherVariable = "another value";
        }
    }

..
   The second examples utilizes dynamic approach for getting variables. Both of
   them create only one variable `${DYNAMIC VARIABLE}`.

두번째 예제는 변수를 얻기 위해 동적인 방법을 사용한다. 단 하나의 변수
`${DYNAMIC VARIABLE}` 을 생성한다.

.. sourcecode:: python

    class DynamicPythonExample(object):

        def get_variables(self, *args):
            return {'dynamic variable': ' '.join(args)}

.. sourcecode:: java

    import java.util.Map;
    import java.util.HashMap;

    public class DynamicJavaExample {

        public Map<String, String> getVariables(String arg1, String arg2) {
            HashMap<String, String> variables = new HashMap<String, String>();
            variables.put("dynamic variable", arg1 + " " + arg2);
            return variables;
        }
    }

Variable file as YAML
~~~~~~~~~~~~~~~~~~~~~

..
   Variable files can also be implemented as `YAML <http://yaml.org>`_ files.
   YAML is a data serialization language with a simple and human-friendly syntax.
   The following example demonstrates a simple YAML file:

변수 파일은 `YAML <http://yaml.org>`_ 파일로 구현할 수 있다. YAML은
데이타 직렬화 언어로 간단하고 인간 친화적인 구문을 가진다. 다음 예제는
간단한 YAML 파일을 보여준다:

.. sourcecode:: yaml

    string:   Hello, world!
    integer:  42
    list:
      - one
      - two
    dict:
      one: yksi
      two: kaksi

..
   .. note:: Using YAML files with Robot Framework requires `PyYAML
	     <http://pyyaml.org>`_ module to be installed. If you have
	     pip_ installed, you can install it simply by running
	     `pip install pyyaml`.

	     YAML support is new in Robot Framework 2.9. Starting from
	     version 2.9.2, the `standalone JAR distribution`_ has
	     PyYAML included by default.

.. note:: 로봇프레임워크에서 YAML 파일을 사용하기위해 `PyYAML
          <http://pyyaml.org>`_ 모듈을 설치해야 한다. `pip install
          pyyaml` 명령어로 간단히 설치 가능하다.

	  YAML은 로봇프레임워크 2.9에서 새롭게 도입되었다. 2.9.2에서
	  부터 `standalone JAR distribution`_ 은 기본적으로 PyYAML을
	  포함한다.

..
   YAML variable files can be used exactly like normal variable files
   from the command line using :option:`--variablefile` option, in the settings
   table using :setting:`Variables` setting, and dynamically using the
   :name:`Import Variables` keyword. The only thing to remember is that paths to
   YAML files must always end with :file:`.yaml` extension.

YAML 변수 파일은 보통의 변수파일과 동일하게 사용할 수 있다. 명령행
라인의 :option:`--variablefile` 옵션을 사용할 수 있고, 설정표에서
:setting:`Variables` 설정을 사용할 수 있고, 동적으로 :name:`Import
Variables` 키워드를 사용할 수 있다. 기억해야 할 것은 YAML 파일에 대한
경로는 반드시 :file:`.yaml` 확장자를 가져야 한다는 것 뿐이다.

..
   If the above YAML file is imported, it will create exactly the same
   variables as the following variable table:

만약 위의 YAML 파일이 임포트 된다면 아래 변수표와 동일한 변수를 생성할
것이다.

.. sourcecode:: robotframework

   *** Variables ***
   ${STRING}     Hello, world!
   ${INTEGER}    ${42}
   @{LIST}       one         two
   &{DICT}       one=yksi    two=kaksi

..
   YAML files used as variable files must always be mappings in the top level.
   As the above example demonstrates, keys and values in the mapping become
   variable names and values, respectively. Variable values can be any data
   types supported by YAML syntax. If names or values contain non-ASCII
   characters, YAML variables files must be UTF-8 encoded.

변수 파일로 사용된 YAML 파일은 반드시 항상 최고 수준에서 매핑되어야
한다. 위에 제시한 예제에서 처럼 매핑에서의 키와 값은 각각 변수 이름과
값이 된다. 변수 값은 YAML 구문에서 지원하는 임의의 데이타 형이 될 수
있다. 만약 이름 또는 값이 non-ASCII 문자를 포함한다면 YAML 변수 파일은
반드시 UTF-8로 인코딩되어야 한다.

..
   Mappings used as values are automatically converted to special dictionaries
   that are used also when `creating dictionary variables`_ in the variable table.
   Values of these dictionaries are accessible as attributes like `${DICT.one}`.
   These dictionaries are also ordered, but with YAML files the original source
   order is unfortunately not preserved.

값으로 사용된 매핑은 자동적으로 특별한 사전으로 변환된다. 변수표에서
`사전 변수생성`__ 할때 사용할 수 있다. 사전의 값은 `${DICT.one}` 처럼
속성으로 접근가능 하다. 이런 사전은 또한 정렬되어진다. 하지만 YAML 파일의
원본 소스 순서는 보존되지 않는다.

__ `creating dictionary variables`_ 
