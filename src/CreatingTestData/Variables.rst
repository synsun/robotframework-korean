Variables
=========

.. contents::
   :depth: 2
   :local:

Introduction
------------

..
   Variables are an integral feature of Robot Framework, and they can be
   used in most places in test data. Most commonly, they are used in
   arguments for keywords in test case tables and keyword tables, but
   also all settings allow variables in their values. A normal keyword
   name *cannot* be specified with a variable, but the BuiltIn_ keyword
   :name:`Run Keyword` can be used to get the same effect.

Variables는 Robot Framework의 핵심 특징이며, 테스트 데이터 내의 어느
곳에서나 사용가능하다. 가장 일반적으로, test case 표와 keyword 표에서
키워드 전달인자로 사용할 수 있다. 또한 모든 설정은 그 값으로 변수를
사용할 수 있다. 일반 키워드 이름은 변수를 가지고 기술할 수 *없지만*,
BuiltIn_ 키워드 :name:`Run Keyword` 는 동일한 효과를 얻기 위해 사용될
수 있다.

..
   Robot Framework has its own variables that can be used as scalars__, lists__
   or `dictionaries`__ using syntax `${SCALAR}`, `@{LIST}` and `&{DICT}`,
   respectively. In addition to this, `environment variables`_ can be used
   directly with syntax `%{ENV_VAR}`.

Robot Framework은 변수로 scalars__, lists__, dictionaries__ 를 가지며,
각각 `${SCALAR}`, `@{LIST}`, `&{DICT}` 구문을 사용한다. 이것외에
`environment variables`_ 는 `%{ENV_VAR}` 구문을 사용한다.

..
   Variables are useful, for example, in these cases:

변수는 아래와 같은 경우 유용하다:

..
   - When strings change often in the test data. With variables you only
     need to make these changes in one place.

- 문자열이 테스트 데이터 안에서 자주 바뀔 때. 변수를 사용하면 한
  곳에서 이러한 변화를 만들 수 있다.
  
..
   - When creating system-independent and operating-system-independent test
     data. Using variables instead of hard-coded strings eases that considerably
     (for example, `${RESOURCES}` instead of `c:\resources`, or `${HOST}`
     instead of `10.0.0.1:8080`). Because variables can be `set from the
     command line`__ when tests are started, changing system-specific
     variables is easy (for example, `--variable HOST:10.0.0.2:1234
     --variable RESOURCES:/opt/resources`). This also facilitates
     localization testing, which often involves running the same tests
     with different strings.

- 시스템 독립적이고 운영체제 독립적인 테스트 데이타를 생성할 때.
  하드코딩한 문자열 대신 변수를 사용하면 상당히 편하다.(예,
  `c:\resources` 대신에 `${RESOURCES}`, `10.0.0.1:8080` 대신에
  `${HOST}`). 변수는 테스트를 시작할 때 `명령형에서 설정`__ 할 수 있기
  때문에, 시스템 특정 변수를 쉽게 바꿀 수 있다(예, `--variable
  HOST:10.0.0.2:1234 --variable RESOURCES:/opt/resources`). 또한 종종
  다른 문자열과 동일한 테스트를 실행하여 지역화 테스팅을 용이하게
  한다.

..
   - When there is a need to have objects other than strings as arguments
     for keywords. This is not possible without variables.

- 키워드의 전달인자로 문자열보다 다른 객체를 가질 필요가 있을 때.
  이것은 변수 없이 불가능하다.
  
..
   - When different keywords, even in different test libraries, need to
     communicate. You can assign a return value from one keyword to a
     variable and pass it as an argument to another.

- 다른 키워드(심지어 다른 테스트 라이브러리)끼리 의사소통을 해야 할
  때. 하나의 키워드에서 변수에 리턴 값을 할당 하고, 다른 키워드에
  인자로 전달 할 수 있다.
  
  
..
   - When values in the test data are long or otherwise complicated. For
     example, `${URL}` is shorter than
     `http://long.domain.name:8080/path/to/service?foo=1&bar=2&zap=42`.

- 테스트 데이타에서 값이 길거나 복잡할 때. 예를 들어 `${URL}` 는
  `http://long.domain.name:8080/path/to/service?foo=1&bar=2&zap=42`
  보다 짧다.
  
..
   If a non-existent variable is used in the test data, the keyword using
   it fails. If the same syntax that is used for variables is needed as a
   literal string, it must be `escaped with a backslash`__ as in `\${NAME}`.

테스트 데이타에서 존재하지 않는 변수가 사용된 경우, 그 변수를 사용한
키워드는 실패한다. 변수에 사용되는 것과 동일한 구문이 문자그대로로
필요하다면 반드시 `\${NAME}` 처럼 `백슬래시로 이스케이프`__ 해야 한다.

__ `Scalar variables`_
__ `List variables`_
__ `Dictionary variables`_
__ `Setting variables in command line`_
__ Escaping_

Variable types
--------------

..
   Different variable types are explained in this section. How variables
   can be created is discussed in subsequent sections.

이번 섹션에서 서로 다른 변수 타입을 설명한다. 이어지는 섹션에서는
어떻게 변수를 생성할 수 있는지를 설명한다.

..
   Robot Framework variables, similarly as keywords, are
   case-insensitive, and also spaces and underscores are
   ignored. However, it is recommended to use capital letters with
   global variables (for example, `${PATH}` or `${TWO WORDS}`)
   and small letters with variables that are only available in certain
   test cases or user keywords (for example, `${my var}` or
   `${myVar}`). Much more importantly, though, cases should be used
   consistently.

키워드와 비슷하게 Robot Framework 변수는 대소문자 구분을 하지 않으며
공백과 언더스코아를 무시한다. 그러나 전역 변수(global variables)는
대문자를 사용하는 것이 좋다.(예, `${PATH}` 또는 `${TWO WORDS}`) Test
case나 user keywords내에서만 사용하는 변수는 소문자를 사용하는 것이
좋다.(예, `${my var}` 또는 `${myVar}` 하지만 더 중요한 것은 일관성을
가지는 것이다.

..
   Variable name consists of the variable type identifier (`$`, `@`, `&`, `%`),
   curly braces (`{`, `}`) and actual variable name between the braces.
   Unlike in some programming languages where similar variable syntax is
   used, curly braces are always mandatory. Variable names can basically have
   any characters between the curly braces. However, using only alphabetic
   characters from a to z, numbers, underscore and space is recommended, and
   it is even a requirement for using the `extended variable syntax`_.

변수 이름은 변수 타입 식별자(`$`, `@`, `&`, `%`), 중괄호(`{`, `}`)와
실제 변수이름(괄호내)으로 구성된다. 비슷한 변수 구문을 사용하는 다른
프로그래밍 언어와 다르게 중괄호는 반드시 필요하다. 변수 이름은
기본적으로 중괄호 사이에 임의 문자를 사용할 수 있다. 그러나 a-z 알파벳
문자, 숫자, 언더스코어, 공백을 권장한다. 심지어 이것은 `extended
variable syntax`_ 을 사용하기 위한 요구사항이다.
   
.. _scalar variable:

Scalar variables
~~~~~~~~~~~~~~~~

..
   When scalar variables are used in the test data, they are replaced
   with the value they are assigned to. While scalar variables are most
   commonly used for simple strings, you can assign any objects,
   including lists, to them. The scalar variable syntax, for example
   `${NAME}`, should be familiar to most users, as it is also used,
   for example, in shell scripts and Perl programming language.

스칼라 변수가 테스트 데이터에 사용되는 경우 할당된 값으로 대체된다.
스칼라 변수는 가장 일반적으로 간단한 문자열을 위해 사용되지만 리스트를
포함하여 어떤 객체든지 할당할 수 있다. 스칼라 변수 구문(예,
`${NAME}`)은 쉘 스크립트나 펄 프로그래밍 언어를 사용하는 대부분의
사용자에 익숙 할 것이다.
   
..
   The example below illustrates the usage of scalar variables. Assuming
   that the variables `${GREET}` and `${NAME}` are available
   and assigned to strings `Hello` and `world`, respectively,
   both the example test cases are equivalent.

아래의 예는 스칼라 변수 사용 방법을 보여준다. `${GREET}` 와 `${NAME}`
변수는 사용가능하고 각각 `Hello` 와 `world` 문자열을 할당한다. 두 예제
테스트 케이스는 동일하다.
   
.. sourcecode:: robotframework

   *** Test Cases ***
   Constants
       Log    Hello
       Log    Hello, world!!

   Variables
       Log    ${GREET}
       Log    ${GREET}, ${NAME}!!

..
   When a scalar variable is used as the only value in a test data cell,
   the scalar variable is replaced with the value it has. The value may
   be any object. When a scalar variable is used in a test data cell with
   anything else (constant strings or other variables), its value is
   first converted into a Unicode string and then catenated to whatever is in
   that cell. Converting the value into a string means that the object's
   method `__unicode__` (in Python, with `__str__` as a fallback)
   or `toString` (in Java) is called.

스칼라 변수가 테스트 데이터 셀에서 단지 값으로 사용될 경우 스칼라
변수는 그 값으로 대체된다. 값은 임이의 객체가 될 수 있다. 스칼라
변수가 테스트 데이터에서 다른 어떤 것(상수 문자열 또는 다른 변수)으로
사용되면, 그 값은 먼저 유니코드 문자열로 변환한 후 해당 셀에 잇는다.
값이 문자열로 변환한다는 것은 객체의 메소드 `__unicode__` (파이썬에서
`__str__` 로 대체) 또는 `toString` (자바) 이 호출 된다는 것을
의미한다.

..
   .. note:: Variable values are used as-is without conversions also when
	     passing arguments to keywords using the `named arguments`_
	     syntax like `argname=${var}`.

.. note:: 변수 값은 `argname=${var}` 처럼 `named arguments`_ 구문을
          사용하여 키워드에 인자를 전달한 경우 변환 없이 변수 그대로
          사용된다.
	  
..
   The example below demonstrates the difference between having a
   variable in a cell alone or with other content. First, let us assume
   that we have a variable `${STR}` set to a string `Hello,
   world!` and `${OBJ}` set to an instance of the following Java
   object:

아래의 예는 셀안에 단독으로 사용되는 변수와 다른 내용과 함께 사용되는
변수의 차이를 보여준다. 첫째, 변수 `${STR}` 에 `Hello, world!`
문자열을 설정하였다고 가정하고, `${OBJ}` 는 다음 자바 객체의
인스턴스를 설정하자:

.. sourcecode:: java

 public class MyObj {

     public String toString() {
         return "Hi, tellus!";
     }
 }

..
   With these two variables set, we then have the following test data:

이들 두 변수의 설정과 함께 다음 테스트 데이타가 주어진다:  
   
.. sourcecode:: robotframework

   *** Test Cases ***
   Objects
       KW 1    ${STR}
       KW 2    ${OBJ}
       KW 3    I said "${STR}"
       KW 4    You said "${OBJ}"

..
   Finally, when this test data is executed, different keywords receive
   the arguments as explained below:

마지막으로, 테스트 데이타가 수행되면 서로 다른 키워드는 후술하는 바와
같이 인자를 받는다:
   
- :name:`KW 1` gets a string `Hello, world!`
- :name:`KW 2` gets an object stored to variable `${OBJ}`
- :name:`KW 3` gets a string `I said "Hello, world!"`
- :name:`KW 4` gets a string `You said "Hi, tellus!"`

..
   .. Note:: Converting variables to Unicode obviously fails if the variable
	     cannot be represented as Unicode. This can happen, for example,
	     if you try to use byte sequences as arguments to keywords so that
	     you catenate the values together like `${byte1}${byte2}`.
	     A workaround is creating a variable that contains the whole value
	     and using it alone in the cell (e.g. `${bytes}`) because then
	     the value is used as-is.

.. Note:: 변수가 유니코드로 표시 할 수 없는 경우 유니코드로 변수를
          변환하면 실패한다. 이것은 예를 들어, 키워드의 인자로 바이트
          시퀀스를 사용하면 `${byte1}${byte2}` 와 같은 값을 이을 때
          문제가 발생할 수 있다. 해결 방법은 전체 값을 포함하는 변수를
          만들어 셀에 혼자 사용(예. `${bytes}`)하면 그 값이 그대로
          사용된다.
	  
.. _list variable:

List variables
~~~~~~~~~~~~~~

..
   When a variable is used as a scalar like `${EXAMPLE}`, its value will be
   used as-is. If a variable value is a list or list-like, it is also possible
   to use as a list variable like `@{EXAMPLE}`. In this case individual list
   items are passed in as arguments separately. This is easiest to explain with
   an example. Assuming that a variable `@{USER}` has value `['robot','secret']`,
   the following two test cases are equivalent:

`${EXAMPLE}` 와 같은 스칼라 변수를 사용할 때 그 값을 그대로 사용한다.
만약 변수 값이 리스트나 유사리스트이면 `@{EXAMPLE}` 같은 리스트 변수로
사용할 수 있다. 이 경우 리스트의 개별 목록은 별도의 인수로 전달된다.
예제로 설명하는 것이 가장 쉽다. 변수 `@{USER}` 는 `['robot',
'secret']` 값을 가진다면 다음 두 test cases는 동일하다:

.. sourcecode:: robotframework

   *** Test Cases ***
   Constants
       Login    robot    secret

   List Variable
       Login    @{USER}

..
   Robot Framework stores its own variables in one internal storage and allows
   using them as scalars, lists or dictionaries. Using a variable as a list
   requires its value to be a Python list or list-like object. Robot Framework
   does not allow strings to be used as lists, but other iterable objects such
   as tuples or dictionaries are accepted.

Robot Framework는 자신의 변수를 내부 저장 장치에 저장하고, 스칼라,
리스트, 사전으로 사용할 수 있다. 리스트를 변수로 사용할 경우 값은
파이썬 리스트나 유사리스트 객체이어야 한다. Robot Framework는 문자열을
리스트로 사용하는 것을 허용하지 없지만,  튜플이나 사전 같은 반복 가능
객체로 사용할 수 있다.

..
   Prior to Robot Framework 2.9, scalar and list variables were stored separately,
   but it was possible to use list variables as scalars and scalar variables as
   lists. This caused lot of confusion when there accidentally was a scalar
   variable and a list variable with same name but different value.

Robot Framework 2.9이전에는 스칼라와 리스트 변수는 별도로 저장되었지만
리스트 변수를 스칼라로 스칼라 변수를 리스트로 사용가능했다. 이것은
실수로 같은 이름의 스칼라 변수와 리스트 변수가 존재하고 , 서로 다른
값을 가질때 많은 혼동을 야기했다.

Using list variables with other data
''''''''''''''''''''''''''''''''''''

..
   It is possible to use list variables with other arguments, including
   other list variables.

다른 인자(다른 리스트 변수를 포함)와 함께 리스트 변수를 사용하는 것이
가능하다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Example
       Keyword    @{LIST}    more    args
       Keyword    ${SCALAR}    @{LIST}    constant
       Keyword    @{LIST}    @{ANOTHER}    @{ONE MORE}

..
   If a list variable is used in a cell with other data (constant strings or other
   variables), the final value will contain a string representation of the
   variable value. The end result is thus exactly the same as when using the
   variable as a scalar with other data in the same cell.

만약 리스트 변수가 다른 데이타(상수 문자열 또는 다른 변수)와 같이 한
셀에 사용된다면, 최종 값은 변수 값의 문자열 표현을 포함할 것이다.
그래서 최종 결과는 동일 셀내에 다른 데이타와 같이 스칼라를 변수로
사용했을 때와 정확히 동일하다.
   
Accessing individual list items
'''''''''''''''''''''''''''''''

..
   It is possible to access a certain value of a list variable with the syntax
   `@{NAME}[index]`, where `index` is the index of the selected value. Indices
   start from zero, negative indices can be used to access items from the end,
   and trying to access a value with too large an index causes an error.
   Indices are automatically converted to integers, and it is also possible to
   use variables as indices. List items accessed in this manner can be used
   similarly as scalar variables.

`@{NAME}[index]` 구문으로 리스트 변수의 어떤 값 접근할 수 있다. 여기서
`index` 는 선택되어진 값의 인덱스다. 인덱스는 0부터 시작하며, 음의
인덱스는 끝에서부터 항목에 접근한다. 매우 큰 값으로 값에 접근을 시도할
경우 에러를 야기한다.인덱스는 자동적으로 정수로 변환되고, 변수를
인덱스로 사용할 수 있다. 이러한 방법으로 접근한 리스트 항목은 스칼라
변수처럼 사용할 수 있다.

.. sourcecode:: robotframework

   *** Test Cases ***
   List Variable Item
       Login    @{USER}
       Title Should Be    Welcome @{USER}[0]!

   Negative Index
       Log    @{LIST}[-1]

   Index As Variable
       Log    @{LIST}[${INDEX}]

Using list variables with settings
''''''''''''''''''''''''''''''''''

..
   List variables can be used only with some of the settings__. They can
   be used in arguments to imported libraries and variable files, but
   library and variable file names themselves cannot be list
   variables. Also with setups and teardowns list variable can not be used
   as the name of the keyword, but can be used in arguments. With tag related
   settings they can be used freely. Using scalar variables is possible in
   those places where list variables are not supported.

리스트 변수는 settings__ 의 일부로 사용할 수 있다. 리스트 변수는
임포트된 라이브러리와 variable files의 인수로 사용될 수 있다. 하지만
라이브러리와 variable file 이름 자체는 리스트 변수가 될 수 없다. 또한
setups과 teardowns에서 리스트 변수는 키워드의 이름으로 사용될 수 없고
인자로 사용될 수 잇다. 태그 관련 설정에서 리스트 변수는 자유롭게
사용할 수 있다. 스칼라 변수를 사용하는 것이 가능한 곳에 리스트 변수를
사용하는 것은 지원되지 않는다.

.. sourcecode:: robotframework

   *** Settings ***
   Library         ExampleLibrary      @{LIB ARGS}    # This works
   Library         ${LIBRARY}          @{LIB ARGS}    # This works
   Library         @{NAME AND ARGS}                   # This does not work
   Suite Setup     Some Keyword        @{KW ARGS}     # This works
   Suite Setup     ${KEYWORD}          @{KW ARGS}     # This works
   Suite Setup     @{KEYWORD}                         # This does not work
   Default Tags    @{TAGS}                            # This works

__ `All available settings in test data`_

.. _dictionary variable:

Dictionary variables
~~~~~~~~~~~~~~~~~~~~

..
   As discussed above, a variable containing a list can be used as a `list
   variable`_ to pass list items to a keyword as individual arguments.
   Similarly a variable containing a Python dictionary or a dictionary-like
   object can be used as a dictionary variable like `&{EXAMPLE}`. In practice
   this means that individual items of the dictionary are passed as
   `named arguments`_ to the keyword. Assuming that a variable `&{USER}` has
   value `{'name': 'robot', 'password': 'secret'}`, the following two test cases
   are equivalent.

상술한 바와 같이 리스트를 포함하는 변수는 `list variable`_ 로 사용할
수 있으며, 리스트 함목은 키워드 키워드의 개별 인자로 전달 된다.
비슷하게 파이썬 사전이나 유사 사전 객체를 포함하는 변수는 `&{EXAMPLE}`
처럼 사전 변수로 사용할 수 있다. 실제로 이것은 사전의 개별 항목이
키워드에 `named arguments`_ 로 전달됨을 의미한다. 변수 `&{USER}` 가
`{'name': 'robot', 'password': 'secret'}` 값을 가진다고 하면, 다음 두
test cases는 동일하다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Constants
       Login    name=robot    password=secret

   Dict Variable
       Login    &{USER}

..
   Dictionary variables are new in Robot Framework 2.9.

사전 변수는 Robot Framework 2.9에 새롭게 도입되었다.

Using dictionary variables with other data
''''''''''''''''''''''''''''''''''''''''''

..
   It is possible to use dictionary variables with other arguments, including
   other dictionary variables. Because `named argument syntax`_ requires positional
   arguments to be before named argument, dictionaries can only be followed by
   named arguments or other dictionaries.

다른 인자(다른 사전 변수를 포함)를 가지는 사전 변수를 사용할 수 있다.
`named argument syntax`_ 는 named argument 앞에 positional
arguments가 필요하기 때문에, 사전 변수 다음에 named arguments 또는
다른 사전변수가 위치할 수 있다.
   
.. sourcecode:: robotframework

   *** Test Cases ***
   Example
       Keyword    &{DICT}    named=arg
       Keyword    positional    @{LIST}    &{DICT}
       Keyword    &{DICT}    &{ANOTHER}    &{ONE MORE}

..
   If a dictionary variable is used in a cell with other data (constant strings or
   other variables), the final value will contain a string representation of the
   variable value. The end result is thus exactly the same as when using the
   variable as a scalar with other data in the same cell.

만약 사전 변수가 다른 데이타(상수 문자열 또는 다른 변수들)와 함께 한
셀에 사용되는 경우, 최종 값은 변수 값의 문자열 표현을 포함할 것이다.
최종 결과는 다른 데이타와 같이 스칼라 변수를 사용했을 경우와 동일하다.
   
Accessing individual dictionary items
'''''''''''''''''''''''''''''''''''''

..
   It is possible to access a certain value of a dictionary variable
   with the syntax `&{NAME}[key]`, where `key` is the name of the
   selected value. Keys are considered to be strings, but non-strings
   keys can be used as variables. Dictionary items accessed in this
   manner can be used similarly as scalar variables:

`&{NAME}[key]` 구문으로 사전 변수의 값에 접근할 수 있다. 여기서 `key`
는 선택된 값의 이름이다. 키는 문자열로 간주되지만 비문자열 키는 변수로
사용할 수 있다. 이러한 방식으로 사전 항목은 스칼라 변수와 유사하게
사용할 수 있다:

.. sourcecode:: robotframework

   *** Test Cases ***
   Dict Variable Item
       Login    &{USER}
       Title Should Be    Welcome &{USER}[name]!

   Variable Key
       Log Many    &{DICT}[${KEY}]    &{DICT}[${42}]

Using dictionary variables with settings
''''''''''''''''''''''''''''''''''''''''

..
   Dictionary variables cannot generally be used with settings. The only exception
   are imports, setups and teardowns where dictionaries can be used as arguments.

사전 변수는 일반적으로 설정에 사용할 수 없다. 유일한 예외는 임포트와
Setups과 teardowns이다. Setups과 teardowns에서 사전 변수는 전달인자로
사용할 수 있다.

.. sourcecode:: robotframework

   *** Settings ***
   Library        ExampleLibrary    &{LIB ARGS}
   Suite Setup    Some Keyword      &{KW ARGS}     named=arg

.. _environment variable:

Environment variables
~~~~~~~~~~~~~~~~~~~~~

..
   Robot Framework allows using environment variables in the test
   data using the syntax `%{ENV_VAR_NAME}`. They are limited to string
   values.

Robot Framework는 테스트 데이타에서 `%{ENV_VAR_NAME}` 문법을 사용해서
환경변수를 사용할 수 있다.

..
   Environment variables set in the operating system before the test execution are
   available during it, and it is possible to create new ones with the keyword
   :name:`Set Environment Variable` or delete existing ones with the
   keyword :name:`Delete Environment Variable`, both available in the
   OperatingSystem_ library. Because environment variables are global,
   environment variables set in one test case can be used in other test
   cases executed after it. However, changes to environment variables are
   not effective after the test execution.

테스트 실행하기 전에 운영 체제에서 설정 한 환경 변수는 테스트 수행
중에 사용가능하다. :name:`Set Environment Variable` 키워드를 사용해서
새로운 변수를 생설할 수도 있고 :name:`Delete Environment Vairable`
키워드를 사용하여 삭제 할 수도 있다. 이 두 키워드는 OperatingSystem_
라이브러리에서 사용가능하다. 환경 변수는 전역적으로 접근 가능하기
때문에 하나의 test case에서 설정한 환경 변수는 그 후에 다른 test
cases에서 사용할 수 있다. 그러나, 환경 변수에 대한 변경 사항은 테스트
실행 후에 효과를 미치지 않는다.
   
.. sourcecode:: robotframework

   *** Test Cases ***
   Env Variables
       Log    Current user: %{USER}
       Run    %{JAVA_HOME}${/}javac

Java system properties
~~~~~~~~~~~~~~~~~~~~~~

..
   When running tests with Jython, it is possible to access `Java system properties`__
   using same syntax as `environment variables`_. If an environment variable and a
   system property with same name exist, the environment variable will be used.

Jython으로 테스트를 실행 했을 때 `environment variables`_ 와 동일한
문법을 사용하여 `자바 시스템 속성`__ 을 접근할 수 있다. 환경 변수와
시스템 속성에 동일한 이름이 존재한다면 환경 변수가 사용된다.

.. sourcecode:: robotframework

   *** Test Cases ***
   System Properties
       Log    %{user.name} running tests on %{os.name}

__ http://docs.oracle.com/javase/tutorial/essential/environment/sysprop.html

Creating variables
------------------

..
   Variables can spring into existence from different sources.

변수는 서로 다른 소스에서 다른 파일에 존재할 수 있다.

Variable table
~~~~~~~~~~~~~~

..
   The most common source for variables are Variable tables in `test case
   files`_ and `resource files`_. Variable tables are convenient, because they
   allow creating variables in the same place as the rest of the test
   data, and the needed syntax is very simple. Their main disadvantages are
   that values are always strings and they cannot be created dynamically.
   If either of these is a problem, `variable files`_ can be used instead.

변수에 대한 가장 일반적인 소스는 `test case files`_ 과 `resource
files`_ 의 Variable 표이다. Variable 표는 테스트 데이타의 나머지와
동일한 곳에 변수를 생성하며 필요한 구문이 매우 간단하기 때문에
편리하다. 주요 단점은 값은 항상 문자열이고 동적으로 생성할 수 없다는
것이다. 이들 중 하나가 문제가 된다면 대신 `variable files`_ 를 사용할
수 있다.


Creating scalar variables
'''''''''''''''''''''''''

..
   The simplest possible variable assignment is setting a string into a
   scalar variable. This is done by giving the variable name (including
   `${}`) in the first column of the Variable table and the value in
   the second one. If the second column is empty, an empty string is set
   as a value. Also an already defined variable can be used in the value.

가장 간단한 변수 할당은 스칼라 변수에 문자열을 설정하는 것이다. 이는
Variable 표의 첫번째 열에 변수 이름(`${}` 포함)을 두번째 열에는 값을
제공함으로써 이루어진다. 만약 두번째 열이 비어있는 경우 빈 문자열
값으로 설정된다. 또한 이미 정의된 변수는 값으로 사용할 수 있다.
   
.. sourcecode:: robotframework

   *** Variables ***
   ${NAME}         Robot Framework
   ${VERSION}      2.0
   ${ROBOT}        ${NAME} ${VERSION}

..
   It is also possible, but not obligatory,
   to use the equals sign `=` after the variable name to make assigning
   variables slightly more explicit.

의무는 아니지만 변수 이름 뒤의 등호(`=`)를 사용하여 변수에 할당한다는
것을 약간 더 명시화 할 수 있다.
   
.. sourcecode:: robotframework

   *** Variables ***
   ${NAME} =       Robot Framework
   ${VERSION} =    2.0

..
   If a scalar variable has a long value, it can be split to multiple columns and
   rows__. By default cells are catenated together using a space, but this
   can be changed by having `SEPARATOR=<sep>` in the first cell.

스칼라 변수가 긴 값을 가지면 여러 열과 `행`__ 으로 분리 될 수 있다.
기본적으로 셀들은 공백을 사용하여 이어지지만 첫번째 셀에
`SEPARATOR=<sep>` 를 사용하여 바꿀 수 있다.
   
.. sourcecode:: robotframework

   *** Variables ***
   ${EXAMPLE}      This value is joined    together with a space
   ${MULTILINE}    SEPARATOR=\n    First line
   ...             Second line     Third line

..
   Joining long values like above is a new feature in Robot Framework 2.9.
   Creating a scalar variable with multiple values was a syntax error in
   Robot Framework 2.8 and with earlier versions it created a variable with
   a list value.

위와 같이 긴 값을 합치는 것은 Robot Framework 2.9의 새로운 기능이다.
여러 값을 가지는 스칼라 변수를 생성하는 것은 Robot Framework 2.8에서는
문법 오류를 발생시키고 더 이전 버전은 리스트 값을 가지는 변수를
생성한다.
   
__ `Dividing test data to several rows`_

Creating list variables
'''''''''''''''''''''''

..
   Creating list variables is as easy as creating scalar variables. Again, the
   variable name is in the first column of the Variable table and
   values in the subsequent columns. A list variable can have any number
   of values, starting from zero, and if many values are needed, they
   can be `split into several rows`__.

리스트 변수를 생성하는 것은 스칼라 변수를 생성하는 것만큼 쉽다. 또
Variable 표의 첫번째 열에는 변수이름이, 이어지는 열에는 값들이
존재한다. 리스트 변수는 0에서 부터 임의 갯수의 값을 가질수 있고 많은
값이 필요한 경우 `여러 행으로 분할 할 수`__ 있다.
   

__ `Dividing test data to several rows`_

.. sourcecode:: robotframework

   *** Variables ***
   @{NAMES}        Matti       Teppo
   @{NAMES2}       @{NAMES}    Seppo
   @{NOTHING}
   @{MANY}         one         two      three      four
   ...             five        six      seven

Creating dictionary variables
'''''''''''''''''''''''''''''

..
   Dictionary variables can be created in the variable table similarly as
   list variables. The difference is that items need to be created using
   `name=value` syntax or existing dictionary variables. If there are multiple
   items with same name, the last value has precedence. If a name contains
   an equal sign, it can be escaped__ with a backslash like `\=`.

사전 변수는 Variable 표에 리스트 변수와 유사하게 생성할 수 있다.
차이는 `name=value` 문법을 사용하거나 이미 존재하는 사전 변수를
사용하여 항목을 생성한다는 점이다. 이름이 같은 여러 항목이 있는 경우,
마지막 값은 우선 순위가 있다. 이름이 등호를 포함하는 경우 `\=` 처럼
백슬래시로 `이스케이프`__ 해야 한다.

.. sourcecode:: robotframework

   *** Variables ***
   &{USER 1}       name=Matti    address=xxx         phone=123
   &{USER 2}       name=Teppo    address=yyy         phone=456
   &{MANY}         first=1       second=${2}         ${3}=third
   &{EVEN MORE}    &{MANY}       first=override      empty=
   ...             =empty        key\=here=value

..
   Dictionary variables created in variable table have two extra properties
   compared to normal Python dictionaries. First of all, values of these
   dictionaries can be accessed like attributes, which means that it is possible
   to use `extended variable syntax`_ like `${VAR.key}`. This only works if the
   key is a valid attribute name and does not match any normal attribute
   Python dictionaries have. For example, individual value `&{USER}[name]` can
   also be accessed like `${USER.name}` (notice that `$` is needed in this
   context), but `&{MANY}[${3}]` does not work as `${MANY.3}`.

Variable 표에 생성한 사전 변수는 일반 파이썬 사전과 비교하여 두가지
여분의 속성을 가진다. 우선, 이 사전의 값은 속성처럼 접근할 수 있다. 즉
`{VAR.key}` 같이 `확정 변수 문법 <extended variable syntax_>`__ 을
사용할 수 있다. 이는 키가 유효한 속성이름이고, 파이썬 사전이 가지는
어떤 일반 속성과 일치하지 않는 경우에만 동작한다. 예를 들어 개별 값
`&{USER}[name]` 는 `${USER.name}` 처럼 접근할 수 있지만(`$` 에 주의)
`&{MANY}[${3}]` 은 `${MANY.3}` 처럼 동작하지 않는다.

..
   Another special property of dictionaries created in the variable table is
   that they are ordered. This means that if these dictionaries are iterated,
   their items always come in the order they are defined. This can be useful
   if dictionaries are used as `list variables`_ with `for loops`_ or otherwise.
   When a dictionary is used as a list variable, the actual value contains
   dictionary keys. For example, `@{MANY}` variable would have value `['first',
   'second', 3]`.

Variable 표에 생성한 사전의 또 다른 특별한 속성은 정렬(orderd)이다.
이것은 이 사전이 반복되는 경우 항목은 항상 정의된 순서대로 오는 것을
의미한다. 사전이 `for loops` 에서 `리스트 변수 <list variables_>`__
처럼 사용하는 경우 유용한다. 사전이 리스트 변수로 사용될 때 실제 값은
사전 키를 포함한다. 즉 `@{MANY}` 변수는 `['first', 'second', 3]` 값을
가진다.

__ Escaping_

Variable file
~~~~~~~~~~~~~

..
   Variable files are the most powerful mechanism for creating different
   kind of variables. It is possible to assign variables to any object
   using them, and they also enable creating variables dynamically. The
   variable file syntax and taking variable files into use is explained
   in section `Resource and variable files`_.

Variable files는 다른 종류의 변수를 생성하는 가장 강력한 메커니즘이다.
이것을 사용하여 변수에 임의의 객체를 할당할 수 있고, 또한 동적으로
변수를 생성할 수 있다. Variable file 문법과 variable files을 사용하기
위한 방법은 `Resource and variable files`_ 섹션에서 설명한다.
   
Setting variables in command line
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Variables can be set from the command line either individually with
   the :option:`--variable (-v)` option or using a variable file with the
   :option:`--variablefile (-V)` option. Variables set from the command line
   are globally available for all executed test data files, and they also
   override possible variables with the same names in the Variable table and in
   variable files imported in the test data.

변수는 명령행에서 개별적으로 :option:`--variable (-v)` 옵션을
사용하거나 variable file과 함께 :option:`--variablefile (-V)` 옵션을
사용하여 설정할 수 있다. 명령행에서 설정한 변수는 모든 테스트 데이타
파일이 실행되는 동안 전역적으로 접근가능하고, Variable 표와 테스트
데이타에서 임포트된 variable files 안의 동일 이름의 변수를 재정의
한다.
   

..
   The syntax for setting individual variables is :option:`--variable
   name:value`, where `name` is the name of the variable without
   `${}` and `value` is its value. Several variables can be
   set by using this option several times. Only scalar variables can be
   set using this syntax and they can only get string values. Many
   special characters are difficult to represent in the
   command line, but they can be escaped__ with the :option:`--escape`
   option.

개별 변수 설정을 위한 문법은 :option:`--variable name:value` 이다.
여기서 `name`은 `${}` 를 제외한 변수의 이름이고 `value` 는 값이다. 이
옵션을 여러번 사용해서 여러 변수를 설정할 수 있다. 스칼라 변수만 이
문법을 사용하여 설정할 수 있고 단지 문자열 값을 얻을 수 있다. 대부분의
특수 문자는 명령형에서 표현하기 어렵다. 하지만 그런 문자는
:option:`--escape` 옵션으로 `이스케이프`__ 할 수 있다.

__ `Escaping complicated characters`_

.. sourcecode:: bash

   --variable EXAMPLE:value
   --variable HOST:localhost:7272 --variable USER:robot
   --variable ESCAPED:Qquotes_and_spacesQ --escape quot:Q --escape space:_

..
   In the examples above, variables are set so that

위의 예제에서 변수를 설정하면

..
   - `${EXAMPLE}` gets the value `value`
   - `${HOST}` and `${USER}` get the values
     `localhost:7272` and `robot`
   - `${ESCAPED}` gets the value `"quotes and spaces"`

- `${EXAMPLE}` 은 `value` 값을 얻는다.
- `${HOST}` 와 `${USER}` 는 `localhost:7272` 와 `robot` 값을 얻는다.
- `${ESCAPED}` 는 `"quotes and spaces"` 값을 얻는다.

  
..
   The basic syntax for taking `variable files`_ into use from the command line
   is :option:`--variablefile path/to/variables.py`, and `Taking variable files into
   use`_ section has more details. What variables actually are created depends on
   what variables there are in the referenced variable file.

명령행 `variable files`_ 을 사용하는 구문은 :option:`--variablefile
path/to/variables.py` 이며, 자세한 내용은 `variable files를 사용하는
방법 <Taking variable files into use_>`__ 섹션에서 설명한다. 어떤
변수가 실제로 생성되는 경우, 참조된 variable files의 변수에 따라
달라진다.


..
   If both variable files and individual variables are given from the command line,
   the latter have `higher priority`__.

Variable files과 개별 변수 모두 명령행에서 주어진 경우 후자가 `더 높은
우선 순위`__ 를 가진다.

__ `Variable priorities and scopes`_

Return values from keywords
~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Return values from keywords can also be set into variables. This
   allows communication between different keywords even in different test
   libraries.

키워드의 반환 값은 변수로 설정할 수 있다. 이것을 이용하면 키워드간
통신을 할 수 있다. 키워드가 다른 테스트라이브러리에 존재한다고 해도 가능하다.


..
   Variables set in this manner are otherwise similar to any other
   variables, but they are available only in the `local scope`_
   where they are created. Thus it is not possible, for example, to set
   a variable like this in one test case and use it in another. This is
   because, in general, automated test cases should not depend on each
   other, and accidentally setting a variable that is used elsewhere
   could cause hard-to-debug errors. If there is a genuine need for
   setting a variable in one test case and using it in another, it is
   possible to use BuiltIn_ keywords as explained in the next section.

이런 방법으로 설정한 변수는 다른 변수와 비슷하지만, 만들어진 `지역
범위 <local scope_>`__ 에서만 사용할 수 있다. 따라서 하나의 test
case에서 이와 같이 설정한 변수는 다른 test case에서 사용할 수 없다.
일반적으로 이런 이유로 자동화된 test cases는 서로 의존하지 않는다.
부주의하게 다른 곳에서 사용하는 변수를 설정하면 디버그 하기 어려운
에러를 만든다. 만약 한 test case에서 설정한 변수를 다른 test case에서
사용하기를 진짜 원한다면, 다음 섹션에서 설명한 BuiltIn_ 키워드를
사용할 수 있다.

Assigning scalar variables
''''''''''''''''''''''''''

..
   Any value returned by a keyword can be assigned to a `scalar variable`_.
   As illustrated by the example below, the required syntax is very simple.

키워드에서 반환된 값을 `스칼라 변수 <scalar variable_>`__ 에 할당할 수
있다. 이것은 매우 간단한 문법을 요구하고, 아래 예제에서 설명한다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Returning
       ${x} =    Get X    an argument
       Log    We got ${x}!

..
   In the above example the value returned by the :name:`Get X` keyword
   is first set into the variable `${x}` and then used by the :name:`Log`
   keyword. Having the equals sign `=` after the variable name is
   not obligatory, but it makes the assignment more explicit. Creating
   local variables like this works both in test case and user keyword level.

위의 예제에서 :name:`Get X` 키워드에 의해 반환된 값은 변수 `${x}` 를
설정하고 :name:`Log` 키워드에 의해 사용된다. 변수 이름뒤의 등호 `=` 는
필수는 아니지만 값을 할당한다는 의미를 더 명시적으로 보여준다. 이와
같이 생성된 지역 변수는 test case와 user keyword에서 잘 동작한다.

..
   Notice that although a value is assigned to a scalar variable, it can
   be used as a `list variable`_ if it has a list-like value and as a `dictionary
   variable`_ if it has a dictionary-like value.

값이 스칼라 변수에 할당되어 있어도, 유사 리스트 값을 가진다면 `리스트
변수 <list variable_>`__ 로, 유사 사전 값을 가진다면 `사전 변수
<dictionary variable_>`_ 로 사용할 수 있음에 유의하라.

.. sourcecode:: robotframework

   *** Test Cases ***
   Example
       ${list} =    Create List    first    second    third
       Length Should Be    ${list}    3
       Log Many    @{list}

Assigning list variables
''''''''''''''''''''''''

..
   If a keyword returns a list or any list-like object, it is possible to
   assign it to a `list variable`_.

키워드가 리스트 또는 유사 리스트 객체를 반환하면 `리스트 변수 <list
variable_>`__ 에 할당 할 수 있다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Example
       @{list} =    Create List    first    second    third
       Length Should Be    ${list}    3
       Log Many    @{list}

..
   Because all Robot Framework variables are stored in same namespace, there is
   not much difference between assigning a value to a scalar variable or a list
   variable. This can be seen by comparing the last two examples above. The main
   differences are that when creating a list variable, Robot Framework
   automatically verifies that the value is a list or list-like, and the stored
   variable value will be a new list created from the return value. When
   assigning to a scalar variable, the return value is not verified and the
   stored value will be the exact same object that was returned.

모든 Robot Framework 변수는 동일 네임스페이스(namespace)에
저장되기때문에 스칼라 변수나 리스트 변수에 값을 할당하는 것은 별로
차이가 없다. 이것은 위의 마지막 두 예제를 비교하여 볼 수 있다. 주요
차이점은 리스트 변수를 만들 때 Robot Framework는 자동으로 값이
리스트나 유사 리스트인지를 검증하고, 저장된 변수값은 리턴 값으로
생성된 새로운 리스트라는 것이다. 스칼라 변수를 할당 할때 리턴 값은
검증하지 않고 저장된 값은 리턴된 것과 정확히 같은 객체가 될 것이다.

Assigning dictionary variables
''''''''''''''''''''''''''''''

..
   If a keyword returns a dictionary or any dictionary-like object, it is possible
   to assign it to a `dictionary variable`_.

키워드가 사전이나 유사 사전 객체를 리턴하면 `사전 변수 <dictionary
variable_>`__ 에 할당 할 수 있다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Example
       &{dict} =    Create Dictionary    first=1    second=${2}    ${3}=third
       Length Should Be    ${dict}    3
       Do Something    &{dict}
       Log    ${dict.first}

..
   Because all Robot Framework variables are stored in same namespace, it would
   also be possible to assign a dictionary into a scalar variable and use it
   later as a dictionary when needed. There are, however, some actual benefits
   in creating a dictionary variable explicitly. First of all, Robot Framework
   verifies that the returned value is a dictionary or dictionary-like similarly
   as it verifies that list variables can only get a list-like value.
   Another benefit is that Robot Framework converts the value into a special
   dictionary it uses also when `creating dictionary variables`_ in the variable
   table. These dictionaries are sortable and their values can be accessed using
   attribute access like `${dict.first}` in the above example.

모든 Robot Framework 변수는 동일한 네임스페이스에 저장되기 때문에
사전을 스칼라 변수에 할당하여 후에 사전이 필요한 경우 사용가능하다.
그러나 명시적으로 사전 변수를 생성하는 것은 몇가지 이득이 있다. 첫째로
Robot Framework는 리턴된 값이 이전의 리스트 변수처럼 사전이나 유사
사전인지 검증한다. 다른 이득은 Robot Framework는 Variable 표에서 `사전
변수 생성 <creating dictionary variables_>`__ 할때 값을 특수한
사전으로 변환한다. 이 사전은 저장가능하고 값은 위의 예제처럼
`${dict.first}` 처럼 속성 접근을 사용하여 접근할 수 있다.


Assigning multiple variables
''''''''''''''''''''''''''''

..
   If a keyword returns a list or a list-like object, it is possible to assign
   individual values into multiple scalar variables or into scalar variables and
   a list variable.

키워드가 리스트나 유사 리스트 객체를 리턴하면 개별 값을 여러개의
스칼라 변수나 스칼라 변수와 리스트 변수에 할당할 수 있다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Assign Multiple
       ${a}    ${b}    ${c} =    Get Three
       ${first}    @{rest} =    Get Three
       @{before}    ${last} =    Get Three
       ${begin}    @{middle}    ${end} =    Get Three

..
   Assuming that the keyword :name:`Get Three` returns a list `[1, 2, 3]`,
   the following variables are created:

:name:`Get Three` 키워드는 리스트 `[1, 2, 3]` 리턴한다고 하면 다음과
      같은 변수가 만들어진다:

..
   - `${a}`, `${b}` and `${c}` with values `1`, `2`, and `3`, respectively.
   - `${first}` with value `1`, and `@{rest}` with value `[2, 3]`.
   - `@{before}` with value `[1, 2]` and `${last}` with value `3`.
   - `${begin}` with value `1`, `@{middle}` with value `[2]` and ${end} with
     value `3`.

- 각각 `1`, `2`, `3` 값을 가지는 `${a}`, `${b}`, `${c}`.
- `1` 값을 가지는 `${first}` 와 `[2, 3]` 값을 가지는 `@{rest}`.
- `[1, 2]` 값을 가지는 `@{before}` 와 `3` 값을 가지는 `${last}`.
- `1` 값을 가지는 `${begin}`, `[2]` 값을 가지는 `@{middle}`, `3` 값을 가지는 `${end}`.

..
   It is an error if the returned list has more or less values than there are
   scalar variables to assign. Additionally, only one list variable is allowed
   and dictionary variables can only be assigned alone.

반환된 리스트가 할당하기위한 스칼라 변수들보다 많거나 적은 값을 가지면
오류를 발생한다. 추가적으로 단지 하나의 리스트 변수만 허용되고 사전
변수 또한 하나만 할당 할 수 있다.

The support for assigning multiple variables was slightly changed in
Robot Framework 2.9. Prior to it a list variable was only allowed as
the last assigned variable, but nowadays it can be used anywhere.
Additionally, it was possible to return more values than scalar variables.
In that case the last scalar variable was magically turned into a list
containing the extra values.

Robot Framework 2.9에서 여러 변수를 할당하기 위한 지원이 약간
변경되었다. 이전 버전에서는 리스트 변수는 마지막 변수로만 할당 할 수
있었다. 지금은 어느 곳에서든 사용할 수 있다. 게다가 스칼라 변수들 보다
더 많은 값을 반환할 수 있다. 이 경우 마지막 스칼라 변수는 마술처럼
여분의 값을 포함하는 리스트가 된다.


Using :name:`Set Test/Suite/Global Variable` keywords
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   The BuiltIn_ library has keywords :name:`Set Test Variable`,
   :name:`Set Suite Variable` and :name:`Set Global Variable` which can
   be used for setting variables dynamically during the test
   execution. If a variable already exists within the new scope, its
   value will be overwritten, and otherwise a new variable is created.

BuiltIn_ 라이브러리에 테스트를 실행하는 동안 동적으로 변수를 설정할 수
있는 :name:`Set Test Variable`, :name:`Set Suite Variable`, :name:`Set
Global Variable` 키워드가 있다. 새로운 범위내에 변수가 이미 존재한다면
그 값은 덮어 쓰기되고 그렇지 않으면 새로운 변수가 생성된다.

..
   Variables set with :name:`Set Test Variable` keyword are available
   everywhere within the scope of the currently executed test case. For
   example, if you set a variable in a user keyword, it is available both
   in the test case level and also in all other user keywords used in the
   current test. Other test cases will not see variables set with this
   keyword.

:name:`Set Test Variable` 키워드를 사용하여 변수를 설정하면 현재 실행
중인 test case 범위 내 어디에서나 사용할 수있다. 예를 들어 한 user
keyword에서 변수를 설정하면 이것은 test case 레벨안에서나 현재
테스트내에서 사용된 다른 모든 user keywords내에서도 사용할 수 있다.
하지만 다른 test cases는 이 키워드로 설정한 변수를 볼 수 없다.

..
   Variables set with :name:`Set Suite Variable` keyword are available
   everywhere within the scope of the currently executed test
   suite. Setting variables with this keyword thus has the same effect as
   creating them using the `Variable table`_ in the test data file or
   importing them from `variable files`_. Other test suites, including
   possible child test suites, will not see variables set with this
   keyword.

:name:`Set Suite Variable` 키워드를 사용하여 변수를 설정하면 현재 실행
중인 test suite 범위 내 어디에서나 사용할 수 있다. 이 키워드로 변수를
설정하면 테스트 데이타 파일에서 `Variable table`_ 을 사용하거나
`variable files`_ 로 부터 임포팅하여 변수를 생성하는 것과 동일한
효과를 갖는다. 가능한 자식 test suites를 포함한 다른 test suites는 이
키워드로 설정한 변수를 볼 수 없다.

..
   Variables set with :name:`Set Global Variable` keyword are globally
   available in all test cases and suites executed after setting
   them. Setting variables with this keyword thus has the same effect as
   `creating from the command line`__ using the options :option:`--variable` or
   :option:`--variablefile`. Because this keyword can change variables
   everywhere, it should be used with care.

:name:`Set Global Variable` 키워드를 사용하여 변수를 설정하면 이후
실행되는 모든 test cases와 suites에서 전역적으로 사용할 수 있다. 이
키워드로 변수를 설정하면 :option:`--variable` 또는
:option:`--variablefile` 옵션을 사용하여 `명령행에서 생성`__ 한 것과
동일한 효과를 가진다. 이 키워드는 모든 곳에서 변수를 변경할 수 있기
때문에 주의하여 사용해야 한다.

..
   .. note:: :name:`Set Test/Suite/Global Variable` keywords set named
	     variables directly into `test, suite or global variable scope`__
	     and return nothing. On the other hand, another BuiltIn_ keyword
	     :name:`Set Variable` sets local variables using `return values`__.

.. note:: :name:`Set Test/Suite/Global Variable` 키워드는 직접 `test,
          suite, global 변수 범위의`__ 변수를 설정하고 아무것도
          반환하지 않는다. 반면에 다른 BuiltIn_ 키워드 :name:`Set
          Variable` 은 지역 변수를 설정하고 `값을 리턴한다`__.

		
__ `Setting variables in command line`_
__ `Variable scopes`_
__ `Return values from keywords`_

.. _built-in variable:

Built-in variables
------------------

..
   Robot Framework provides some built-in variables that are available
   automatically.

Robot Framework는 자동적으로 사용가능한 몇몇 내장(built-in) 변수를 제공한다.   
   
Operating-system variables
~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Built-in variables related to the operating system ease making the test data
   operating-system-agnostic.

운영 체제와 관련된 내장 변수는 운영 체제에
독립적인(operating-system-agnostic) 테스틀 데이타를 만들기 쉽게한다.

..
   .. table:: Available operating-system-related built-in variables
      :class: tabular

      +------------+------------------------------------------------------------------+
      |  Variable  |                      Explanation                                 |
      +============+==================================================================+
      | ${CURDIR}  | An absolute path to the directory where the test data            |
      |            | file is located. This variable is case-sensitive.                |
      +------------+------------------------------------------------------------------+
      | ${TEMPDIR} | An absolute path to the system temporary directory. In UNIX-like |
      |            | systems this is typically :file:`/tmp`, and in Windows           |
      |            | :file:`c:\\Documents and Settings\\<user>\\Local Settings\\Temp`.|
      +------------+------------------------------------------------------------------+
      | ${EXECDIR} | An absolute path to the directory where test execution was       |
      |            | started from.                                                    |
      +------------+------------------------------------------------------------------+
      | ${/}       | The system directory path separator. `/` in UNIX-like            |
      |            | systems and :codesc:`\\` in Windows.                             |
      +------------+------------------------------------------------------------------+
      | ${:}       | The system path element separator. `:` in UNIX-like              |
      |            | systems and `;` in Windows.                                      |
      +------------+------------------------------------------------------------------+
      | ${\\n}     | The system line separator. :codesc:`\\n` in UNIX-like systems and|
      |            | :codesc:`\\r\\n` in Windows. New in version 2.7.5.               |
      +------------+------------------------------------------------------------------+

.. table:: Available operating-system-related built-in variables
   :class: tabular

   +------------+------------------------------------------------------------------+
   |  Variable  |                      Explanation                                 |
   +============+==================================================================+
   | ${CURDIR}  | 테스트 데이타 파일이 위치한 디렉토리의 절대 경로.                |
   |            | 이 변수는 대소문자를 구분한다.                                   |
   +------------+------------------------------------------------------------------+
   | ${TEMPDIR} | 시스템 임시 디렉토리에 대한 절대 경로.                           |
   |            | 유닉스 계열 :file:`/tmp`, 윈도우즈는                             |
   |            | :file:`c:\\Documents and Settings\\<user>\\Local Settings\\Temp`.|
   +------------+------------------------------------------------------------------+
   | ${EXECDIR} | 테스트 수행이 시작된 디렉토리에 대한 절대 경로                   |
   +------------+------------------------------------------------------------------+
   | ${/}       | 시스템 디렉토리 경로 구분자.                                     |
   |            | 유닉스 계열 `/`, 윈도우즈 :codesc:`\\`                           |
   +------------+------------------------------------------------------------------+
   | ${:}       | 시스템 경로 요소 구분자.                                         |
   |            | 유닉스 계열 `:`, 윈도우즈 `;`                                    |
   +------------+------------------------------------------------------------------+
   | ${\\n}     | 시스템 라인 구분다.                                              |
   |            | 유닉스 계열 :codesc:`\\n`, 윈도우즈 :codesc:`\\r\\n`.            |
   +------------+------------------------------------------------------------------+

   
.. sourcecode:: robotframework

   *** Test Cases ***
   Example
       Create Binary File    ${CURDIR}${/}input.data    Some text here${\n}on two lines
       Set Environment Variable    CLASSPATH    ${TEMPDIR}${:}${CURDIR}${/}foo.jar

Number variables
~~~~~~~~~~~~~~~~

..
   The variable syntax can be used for creating both integers and
   floating point numbers, as illustrated in the example below. This is
   useful when a keyword expects to get an actual number, and not a
   string that just looks like a number, as an argument.

아래 예와 같이 변수 문법은 정수와 부동 소수점 숫자를 생성할 수 있다.
이것은 키워드가 인수로 단지 숫자처럼 보이는 문자가 아니라 실제 숫자를
얻을 것으로 예상 할 때 유용하다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Example 1A
       Connect    example.com    80       # Connect gets two strings as arguments

   Example 1B
       Connect    example.com    ${80}    # Connect gets a string and an integer

   Example 2
       Do X    ${3.14}    ${-1e-4}        # Do X gets floating point numbers 3.14 and -0.0001

..
   It is possible to create integers also from binary, octal, and
   hexadecimal values using `0b`, `0o` and `0x` prefixes, respectively.
   The syntax is case insensitive.

`0b`, `0o`, `0x` 접두어를 사용하는 이진수, 8진수, 16진수로 부터 정수를
생성할 수 있다. 이 문법은 대소문자를 구분하지 않는다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Example
       Should Be Equal    ${0b1011}    ${11}
       Should Be Equal    ${0o10}      ${8}
       Should Be Equal    ${0xff}      ${255}
       Should Be Equal    ${0B1010}    ${0XA}

Boolean and None/null variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Also Boolean values and Python `None` and Java `null` can
   be created using the variable syntax similarly as numbers.

불린(boolean) 값과 파이썬 `None`, 자바 `null` 은 숫자와 유사한 변수
문법을 사용하여 생성할 수 있다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Boolean
       Set Status    ${true}               # Set Status gets Boolean true as an argument
       Create Y    something   ${false}    # Create Y gets a string and Boolean false

   None
       Do XYZ    ${None}                   # Do XYZ gets Python None as an argument

   Null
       ${ret} =    Get Value    arg        # Checking that Get Value returns Java null
       Should Be Equal    ${ret}    ${null}

..
   These variables are case-insensitive, so for example `${True}` and
   `${true}` are equivalent. Additionally, `${None}` and
   `${null}` are synonyms, because when running tests on the Jython
   interpreter, Jython automatically converts `None` and
   `null` to the correct format when necessary.

이 변수는 대소문자를 구분하지 않으므로 `${True}` 와 `${true}` 는
동일하다. 추가적으로 `${None}` 과 `${null}` 는 동의어(synonyms)이고
Jython 인터프리터에서 테스트를 수행할 때 Jython은 필요할 때 자동적으로
`None` 과 `null` 을 올바른 형식으로 변환한다.

Space and empty variables
~~~~~~~~~~~~~~~~~~~~~~~~~

..
   It is possible to create spaces and empty strings using variables
   `${SPACE}` and `${EMPTY}`, respectively. These variables are
   useful, for example, when there would otherwise be a need to `escape
   spaces or empty cells`__ with a backslash. If more than one space is
   needed, it is possible to use the `extended variable syntax`_ like
   `${SPACE * 5}`.  In the following example, :name:`Should Be
   Equal` keyword gets identical arguments but those using variables are
   easier to understand than those using backslashes.

각각 `${SPACE}` 와 `${EMPTY}` 변수를 사용하여 공백과 빈 문자열을 만들
수 있다. 이 변수는 예를 들어 백슬래시로 `공백 또는 빈 셀을
이스케이프`__ 할 필요가 있을 때 유용하다. 하나 이상의 공백이 필요하면
`${SPACE * 5}` 처럼 `확장 변수 문법 <extended variable syntax_>`__ 을
사용할 수 있다. 다음 예에서 :name:`Should Be Equal` 키워드는 두 경우
모두 동일한 전달인자를 얻을 수 있지만, 백슬래시를 사용하는 것보다 이
변수를 사용하는 것이 더 이해하기 쉽다.

.. sourcecode:: robotframework

   *** Test Cases ***
   One Space
       Should Be Equal    ${SPACE}          \ \

   Four Spaces
       Should Be Equal    ${SPACE * 4}      \ \ \ \ \

   Ten Spaces
       Should Be Equal    ${SPACE * 10}     \ \ \ \ \ \ \ \ \ \ \

   Quoted Space
       Should Be Equal    "${SPACE}"        " "

   Quoted Spaces
       Should Be Equal    "${SPACE * 2}"    " \ "

   Empty
       Should Be Equal    ${EMPTY}          \

..
   There is also an empty `list variable`_ `@{EMPTY}` and an empty `dictionary
   variable`_ `&{EMPTY}`. Because they have no content, they basically
   vanish when used somewhere in the test data. They are useful, for example,
   with `test templates`_ when the `template keyword is used without
   arguments`__ or when overriding list or dictionary variables in different
   scopes. Modifying the value of `@{EMPTY}` or `&{EMPTY}` is not possible.

또한 빈 `리스트 변수 <list variable_>`__ `@{EMPTY}` 와 빈 `사전 변수
<dictionary variable_>`__ `&{EMPTY}` 가 있다. 이 변수들은 내용이 없기
때문에 테스트 데이타 어딘가에 사용될때 기본적으로 사라진다. 예를 들어
`테스트 템플릿 <test templates_>`__ 에서 `템플릿 키워드가 전달인자
없이 사용될 때`__ 나 다른 범위의 리스트나 사전 변수가 재정의 될 때
유용하다. `@{EMPTY}` 나 `&{EMPTY}` 의 값을 변경하는 것은 불가능하다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Template
       [Template]    Some keyword
       @{EMPTY}

   Override
       Set Global Variable    @{LIST}    @{EMPTY}
       Set Suite Variable     &{DICT}    &{EMPTY}

..
   .. note:: `@{EMPTY}` is new in Robot Framework 2.7.4 and `&{EMPTY}` in
	     Robot Framework 2.9.

.. note:: `@{EMPTY}` 는 Robot Framework 2.7.4에서 `&{EMPTY}` 는
          2.9에서 새로 도입되었다.
   
__ Escaping_
__ https://groups.google.com/group/robotframework-users/browse_thread/thread/ccc9e1cd77870437/4577836fe946e7d5?lnk=gst&q=templates#4577836fe946e7d5

Automatic variables
~~~~~~~~~~~~~~~~~~~

..
   Some automatic variables can also be used in the test data. These
   variables can have different values during the test execution and some
   of them are not even available all the time. Altering the value of
   these variables does not affect the original values, but some values
   can be changed dynamically using keywords from the `BuiltIn`_ library.

일부 자동 변수를 테스트 데이타에서 사용할 수 있다. 이 변수는 테스트가
수행되는 동안 다른 값을 가진다. 그중 일부는 항상 사용할 수 있는 것은
아니다. 이러한 변수의 값을 변경하는 것은 원래의 값에 영향을 주지
않지만 일부 값은 `BuiltIn`_ 라이브러리의 키워드를 사용하여 동적으로
변경할 수 있다.

..
   .. table:: Available automatic variables
      :class: tabular

      +------------------------+-------------------------------------------------------+------------+
      |        Variable        |                    Explanation                        | Available  |
      +========================+=======================================================+============+
      | ${TEST NAME}           | The name of the current test case.                    | Test case  |
      +------------------------+-------------------------------------------------------+------------+
      | @{TEST TAGS}           | Contains the tags of the current test case in         | Test case  |
      |                        | alphabetical order. Can be modified dynamically using |            |
      |                        | :name:`Set Tags` and :name:`Remove Tags` keywords.    |            |
      +------------------------+-------------------------------------------------------+------------+
      | ${TEST DOCUMENTATION}  | The documentation of the current test case. Can be set| Test case  |
      |                        | dynamically using using :name:`Set Test Documentation`|            |
      |                        | keyword. New in Robot Framework 2.7.                  |            |
      +------------------------+-------------------------------------------------------+------------+
      | ${TEST STATUS}         | The status of the current test case, either PASS or   | `Test      |
      |                        | FAIL.                                                 | teardown`_ |
      +------------------------+-------------------------------------------------------+------------+
      | ${TEST MESSAGE}        | The message of the current test case.                 | `Test      |
      |                        |                                                       | teardown`_ |
      +------------------------+-------------------------------------------------------+------------+
      | ${PREV TEST NAME}      | The name of the previous test case, or an empty string| Everywhere |
      |                        | if no tests have been executed yet.                   |            |
      +------------------------+-------------------------------------------------------+------------+
      | ${PREV TEST STATUS}    | The status of the previous test case: either PASS,    | Everywhere |
      |                        | FAIL, or an empty string when no tests have been      |            |
      |                        | executed.                                             |            |
      +------------------------+-------------------------------------------------------+------------+
      | ${PREV TEST MESSAGE}   | The possible error message of the previous test case. | Everywhere |
      +------------------------+-------------------------------------------------------+------------+
      | ${SUITE NAME}          | The full name of the current test suite.              | Everywhere |
      +------------------------+-------------------------------------------------------+------------+
      | ${SUITE SOURCE}        | An absolute path to the suite file or directory.      | Everywhere |
      +------------------------+-------------------------------------------------------+------------+
      | ${SUITE DOCUMENTATION} | The documentation of the current test suite. Can be   | Everywhere |
      |                        | set dynamically using using :name:`Set Suite          |            |
      |                        | Documentation` keyword. New in Robot Framework 2.7.   |            |
      +------------------------+-------------------------------------------------------+------------+
      | &{SUITE METADATA}      | The free metadata of the current test suite. Can be   | Everywhere |
      |                        | set using :name:`Set Suite Metadata` keyword.         |            |
      |                        | New in Robot Framework 2.7.4.                         |            |
      +------------------------+-------------------------------------------------------+------------+
      | ${SUITE STATUS}        | The status of the current test suite, either PASS or  | `Suite     |
      |                        | FAIL.                                                 | teardown`_ |
      +------------------------+-------------------------------------------------------+------------+
      | ${SUITE MESSAGE}       | The full message of the current test suite, including | `Suite     |
      |                        | statistics.                                           | teardown`_ |
      +------------------------+-------------------------------------------------------+------------+
      | ${KEYWORD STATUS}      | The status of the current keyword, either PASS or     | `User      |
      |                        | FAIL. New in Robot Framework 2.7                      | keyword    |
      |                        |                                                       | teardown`_ |
      +------------------------+-------------------------------------------------------+------------+
      | ${KEYWORD MESSAGE}     | The possible error message of the current keyword.    | `User      |
      |                        | New in Robot Framework 2.7.                           | keyword    |
      |                        |                                                       | teardown`_ |
      +------------------------+-------------------------------------------------------+------------+
      | ${LOG LEVEL}           | Current `log level`_. New in Robot Framework 2.8.     | Everywhere |
      +------------------------+-------------------------------------------------------+------------+
      | ${OUTPUT FILE}         | An absolute path to the `output file`_.               | Everywhere |
      +------------------------+-------------------------------------------------------+------------+
      | ${LOG FILE}            | An absolute path to the `log file`_ or string NONE    | Everywhere |
      |                        | when no log file is created.                          |            |
      +------------------------+-------------------------------------------------------+------------+
      | ${REPORT FILE}         | An absolute path to the `report file`_ or string NONE | Everywhere |
      |                        | when no report is created.                            |            |
      +------------------------+-------------------------------------------------------+------------+
      | ${DEBUG FILE}          | An absolute path to the `debug file`_ or string NONE  | Everywhere |
      |                        | when no debug file is created.                        |            |
      +------------------------+-------------------------------------------------------+------------+
      | ${OUTPUT DIR}          | An absolute path to the `output directory`_.          | Everywhere |
      +------------------------+-------------------------------------------------------+------------+

.. table:: Available automatic variables
   :class: tabular

   +------------------------+-------------------------------------------------------+------------+
   |        Variable        |                    Explanation                        | Available  |
   +========================+=======================================================+============+
   | ${TEST NAME}           | 현재 실행중인 test case 이름.                         | Test case  |
   +------------------------+-------------------------------------------------------+------------+
   | @{TEST TAGS}           | 알파벳 순서로 현재 실행중인 test case의 태그를 포함.  | Test case  |
   |                        | :name:`Set Tags` 과 :name:`Remove Tags` 키워드로      |            |
   |                        | 동적으로 변경 가능.                                   |            |
   +------------------------+-------------------------------------------------------+------------+
   | ${TEST DOCUMENTATION}  | 현재 실행중인 test case의 문서.                       | Test case  |
   |                        | :name:`Set Test Documentation` 키워드로 동적으로 설정 |            |
   |                        | 가능. Robot Framework 2.7에서 도입.                   |            |
   +------------------------+-------------------------------------------------------+------------+
   | ${TEST STATUS}         | 현재 실행중인 test case의 상태.                       | `Test      |
   |                        | PASS 또는 FAIL.                                       | teardown`_ |
   +------------------------+-------------------------------------------------------+------------+
   | ${TEST MESSAGE}        | 현재 실행중인 test case의 메시지.                     | `Test      |
   |                        |                                                       | teardown`_ |
   +------------------------+-------------------------------------------------------+------------+
   | ${PREV TEST NAME}      | 이전 test case 이름 또는 테스트가 실행전이라면        | Everywhere |
   |                        | 공백 문자열.                                          |            |
   +------------------------+-------------------------------------------------------+------------+
   | ${PREV TEST STATUS}    | 이전 test case의 상태: PASS, FAIL 또는 테스트가 실행  | Everywhere |
   |                        | 전이라면 공백 문자열.                                 |            |
   +------------------------+-------------------------------------------------------+------------+
   | ${PREV TEST MESSAGE}   | 이전 test case의 가능한 에러 메시지.                  | Everywhere |
   +------------------------+-------------------------------------------------------+------------+
   | ${SUITE NAME}          | 현재 실행중인 test suite의 완전한 이름.               | Everywhere |
   +------------------------+-------------------------------------------------------+------------+
   | ${SUITE SOURCE}        | suite 파일 또는 디렉토리의 절대 경로.                 | Everywhere |
   +------------------------+-------------------------------------------------------+------------+
   | ${SUITE DOCUMENTATION} | 현재 실행중인 test suite의 문서.                      | Everywhere |
   |                        | :name:`Set Suite Documentation` 키워드로 동적 설정    |            |
   |                        | 가능. Robot Framework 2.7 도입.                       |            |
   +------------------------+-------------------------------------------------------+------------+
   | &{SUITE METADATA}      | 현재 실행중인 test suite의 메타 데이타.               | Everywhere |
   |                        | :name:`Set Suite Metadata` 키워드로 동적 설정 가능.   |            |
   |                        | Robot Framework 2.7.4 도입.                           |            |
   +------------------------+-------------------------------------------------------+------------+
   | ${SUITE STATUS}        | 현재 실행중인 test suite 상태.                        | `Suite     |
   |                        | PASS 또는 FAIL.                                       | teardown`_ |
   +------------------------+-------------------------------------------------------+------------+
   | ${SUITE MESSAGE}       | 현재 실행중인 test suite의 통계를 포함한 전체 메시지. | `Suite     |
   |                        |                                                       | teardown`_ |
   +------------------------+-------------------------------------------------------+------------+
   | ${KEYWORD STATUS}      | 현재 실행중인 키워드의 상태.                          | `User      |
   |                        | PASS 또는 FAIL.                                       | keyword    |
   |                        | Robot Framework 2.7.4 도입.                           | teardown`_ |
   +------------------------+-------------------------------------------------------+------------+
   | ${KEYWORD MESSAGE}     | 현재 실행중인 키워드의 가능한 에러 메시지.            | `User      |
   |                        | Robot Framework 2.7 도입.                             | keyword    |
   |                        |                                                       | teardown`_ |
   +------------------------+-------------------------------------------------------+------------+
   | ${LOG LEVEL}           | 현재 `log level`_. Robot Framework 2.8 도입.          | Everywhere |
   +------------------------+-------------------------------------------------------+------------+
   | ${OUTPUT FILE}         | `output file`_ 절대 경로.                             | Everywhere |
   +------------------------+-------------------------------------------------------+------------+
   | ${LOG FILE}            | `log file`_ 절대 경로 또는 NONE 문자열(로그 파일이    | Everywhere |
   |                        | 생성되지 않았을 때)                                   |            |
   +------------------------+-------------------------------------------------------+------------+
   | ${REPORT FILE}         | `report file`_ 절대 경로 또는 NONE 문자열(레포트      | Everywhere |
   |                        | 파일이 생성되지 않았을 때).                           |            |
   +------------------------+-------------------------------------------------------+------------+
   | ${DEBUG FILE}          | `debug file`_ 절대 경로 또는 string 문자열(디버그     | Everywhere |
   |                        | 파일이 생성되지 않았을 때).                           |            |
   +------------------------+-------------------------------------------------------+------------+
   | ${OUTPUT DIR}          | `output directory`_ 절대 경로.                        | Everywhere |
   +------------------------+-------------------------------------------------------+------------+

..
   Suite related variables `${SUITE SOURCE}`, `${SUITE NAME}`,
   `${SUITE DOCUMENTATION}` and `&{SUITE METADATA}` are
   available already when test libraries and variable files are imported,
   except to Robot Framework 2.8 and 2.8.1 where this support was broken.
   Possible variables in these automatic variables are not yet resolved
   at the import time, though.

Suite 관련 변수 `${SUITE SOURCE}`, `${SUITE NAME}`, `${SUITE
DOCUMENTATION}`, `&{SUITE METADATA}` 는 테스트 라이브러리와 variable
files이 임포트될 때 이미 사용할 수 있다.(Robot Framework 2.8, 2.8.1은
지원하지 않음) 이러한 자동 변수에 가능한 변수는 아직 임포스시에
해결되지 않는다.

Variable priorities and scopes
------------------------------

..
   Variables coming from different sources have different priorities and
   are available in different scopes.

서로 다른 소스에서 오는 변수는 다른 우선 순위를 가지고 다른 범위에서
사용할 수 있다.

Variable priorities
~~~~~~~~~~~~~~~~~~~

..
   *Variables from the command line*

      Variables `set in the command line`__ have the highest priority of all
      variables that can be set before the actual test execution starts. They
      override possible variables created in Variable tables in test case
      files, as well as in resource and variable files imported in the
      test data.

      Individually set variables (:option:`--variable` option) override the
      variables set using `variable files`_ (:option:`--variablefile` option).
      If you specify same individual variable multiple times, the one specified
      last will override earlier ones. This allows setting default values for
      variables in a `start-up script`_ and overriding them from the command line.
      Notice, though, that if multiple variable files have same variables, the
      ones in the file specified first have the highest priority.

*명령행에서의 변수*

   `명령행 설정`__ 변수는 실제 테스트 수행이 시작되기 전에 설정할 수
   있는 모든 변수중에 가장 높은 우선순위를 가진다. 그 변수는 test case
   files의 Variable 표에서 생성된 가능한 모든 변수 뿐만 아니라 아니라
   테스트 데이타에서 임포트 된 resource와 variable files 변수 또한
   재정의한다.

   개별적으로 설정 변수(:option:`--variable` 옵션)는 `variable files`_
   (:option:`--variablefile` 옵션)을 사용하여 설정한 변수를 재정의
   한다. 이 같은 개별 변수를 여러번 지정한다면 마지막에 지정한 것이
   이전의 것을 재정의 하게 된다. 이를 사용하면 `시작 스크립트
   <start-up script_>`__ 의 변수에 기본 값을 설정하고 명령행에서
   재정의 할 수 있다. 여러 변수 파일이 동일한 변수를 가질 경우 처음
   지정한 파일에 있는 것이 가장 높은 우선순위를 가진다.
   
__ `Setting variables in command line`_

..
   *Variable table in a test case file*

      Variables created using the `Variable table`_ in a test case file
      are available for all the test cases in that file. These variables
      override possible variables with same names in imported resource and
      variable files.

      Variables created in the variable tables are available in all other tables
      in the file where they are created. This means that they can be used also
      in the Setting table, for example, for importing more variables from
      resource and variable files.

*Test case 파일내의 Variable 표*

   Test case 파일내의 `Variable table`_ 를 사용하여 생성한 변수는 같은
   파일안의 모든 test cases에서 사용 할 수 있다. 이런 변수는 임포트된
   resource와 variable 파일에 동일한 이름의 변수가 있다면 재정의한다.

   Variable 표에 생성한 변수는 생성된 파일안의 모든 다른 표에서 사용할
   수 있다. 이것은 예를 들어 Setting 표에서 resource와 variable 파일로
   부터 더 많은 변수를 임포트 하기 위해 사용할 수 있다.
      
..
   *Imported resource and variable files*

      Variables imported from the `resource and variable files`_ have the
      lowest priority of all variables created in the test data.
      Variables from resource files and variable files have the same
      priority. If several resource and/or variable file have same
      variables, the ones in the file imported first are taken into use.

      If a resource file imports resource files or variable files,
      variables in its own Variable table have a higher priority than
      variables it imports. All these variables are available for files that
      import this resource file.

      Note that variables imported from resource and variable files are not
      available in the Variable table of the file that imports them. This
      is due to the Variable table being processed before the Setting table
      where the resource files and variable files are imported.

*임포트된 resource와 variable 파일*

   `resource and variable files`_ 에서 임포트된 변수는 테스트
   데이타에서 생성된 모든 변수 중 가장 낮은 우선순위를 가진다.
   Resource 파일과 variable 파일의 변수는 동일한 우선순위를 가진다.
   하나의 resource 파일이 여러 resource 파일 또는 variable 파일을
   임포트 하면 자신의 Variable 표안의 변수는 임포트된 변수보다 더 높은
   우선순위를 가진다. 이 resource 파일을 임포트한 파일들은 이 모든
   변수를 사용할 수 있다.

   Resource와 variable 파일에서 임포트한 변수는 임포트하는 파일의
   Variable 표에서 사용할 수 없다. 왜냐하면 Variable 표는 resource
   파일과 variable 파일이 임포트되는 Setting 표 전에 처리되기
   때문이다.
      
..
   *Variables set during test execution*

      Variables set during the test execution either using `return values
      from keywords`_ or `using Set Test/Suite/Global Variable keywords`_
      always override possible existing
      variables in the scope where they are set. In a sense they thus
      have the highest priority, but on the other hand they do not affect
      variables outside the scope they are defined.

*테스트 실행중의 변수 설정*

   테스트 실행 중 `키워드 반환값 <return values from keywords_>`__
   이나 `Set Test/Suite/Global Variable 키워드 <using Set
   Test/Suite/Global Variable keywords_>`__ 를 사용하여 변수를
   설정하는 것은 항상 변수가 설정되는 범위안의 변수를 재정의 한다.
   어떤 의미에서 해당 변수는 가장 높은 우선 순위를 가지지만, 다른 한편
   정의 되어지는 범위 외부의 변수에 대해서는 영향을 미치지 않는다.

..
   *Built-in variables*

      `Built-in variables`_ like `${TEMPDIR}` and `${TEST_NAME}`
      have the highest priority of all variables. They cannot be overridden
      using Variable table or from command line, but even they can be reset during
      the test execution. An exception to this rule are `number variables`_, which
      are resolved dynamically if no variable is found otherwise. They can thus be
      overridden, but that is generally a bad idea. Additionally `${CURDIR}`
      is special because it is replaced already during the test data processing time.

*내장(Built-in) 변수*

   `${TEMPDIR}` 과 `${TEST_NAME}` 같은 `내장 변수 <Built-in
   variables_>`__ 는 모든 변수들 중에서 가장 높은 우선 순위를 가진다.
   이 변수는 Variable 표나 명령행에서 뿐만아니라 심지어 테스트 실행 중
   초기화 될때 조차도재정의 할 수 없다. 이 규칙의 단 하나의 예외는
   `숫자 변수 <number variables_>`__ 이다. 해당 변수는 찾을 수 없다면
   동적으로 해결된다. 그래서 재정의 할수도 있지만 일반적으로 나쁜
   아이디어다. 게다가 `${CURDIR}` 는 테스트 데이타 처리 시간 동안 이미
   대체되기 때문에 특별하다.

Variable scopes
~~~~~~~~~~~~~~~

..
   Depending on where and how they are created, variables can have a
   global, test suite, test case or local scope.

생성되는 장소와 방법에 따라 변수는 전역(golbal), test suite, test case
또는 지역(local) 범위를 가진다.

Global scope
''''''''''''

..
   Global variables are available everywhere in the test data. These
   variables are normally `set from the command line`__ with the
   :option:`--variable` and :option:`--variablefile` options, but it is also
   possible to create new global variables or change the existing ones
   with the BuiltIn_ keyword :name:`Set Global Variable` anywhere in
   the test data. Additionally also `built-in variables`_ are global.

전역 변수는 테스트 데이타의 어디에서나 사용할 수 있다. 이런 변수는
보통 :option:`--variable` 와 :option:`--variablefile` 옵션으로
`명령행에서 설정`__ 하지만 테스트 데이타의 어디에서나 BuiltIn_ 키워드
:name:`Set Global Vairable` 로 새로운 전역 변수를 생성하거나 이미
존재하는 것을 변경할 수 있다. 그리고 `내장 변수 <built-in
variables_>`__ 또한 전역이다.

..
   It is recommended to use capital letters with all global variables.

모든 전역 변수는 대문자를 사용을 권장한다.

Test suite scope
''''''''''''''''

..
   Variables with the test suite scope are available anywhere in the
   test suite where they are defined or imported. They can be created
   in Variable tables, imported from `resource and variable files`_,
   or set during the test execution using the BuiltIn_ keyword
   :name:`Set Suite Variable`.

Test suite 범위를 가지는 변수는 정의되거나 임포트된 test suite
어디에서나 사용할 수 있다. 이런 변수는 Variable 표나 `resource와
variable 파일 <resource and variable files_>`__ 에서 임포트되거나
테스트를 실행하는 동안 BuiltIn_ 키워드 :name:`Set Suite Variable` 를
사용하여 생성할 수 있다.
      
..
   The test suite scope *is not recursive*, which means that variables
   available in a higher-level test suite *are not available* in
   lower-level suites. If necessary, `resource and variable files`_ can
   be used for sharing variables.

Test suite 범위는 *재귀적이지 않다*. 이는 higher-level test suite에서
사용할 수 있는 변수는 lower-level suites에서 *사용할 수 없다* 는 것을
의미한다. 필요한 경우 `resource와 variable 파일 <resource and variable
files_>`__ 을 변수 공유에 사용할 수 있다.

..
   Since these variables can be considered global in the test suite where
   they are used, it is recommended to use capital letters also with them.

이러한 변수는 사용되어지는 test suite에서 전역으로 간주되기 때문에
대문자로 사용을 권장한다.

Test case scope
'''''''''''''''

..
   Variables with the test case scope are visible in a test case and in
   all user keywords the test uses. Initially there are no variables in
   this scope, but it is possible to create them by using the BuiltIn_
   keyword :name:`Set Test Variable` anywhere in a test case.

Test case 범위를 가지는 변수는 test case 및 테스트가 사용하는 모든
user keywords에서 볼수 있다. 우선 이 범위 내에 변수는 없지만 test case
어딘가에 BuiltIn_ 키워드 :name:`Set Test Variable` 을 사용하여 변수를
생성할 수 있다.
   
..
   Also variables in the test case scope are to some extend global. It is
   thus generally recommended to use capital letters with them too.

또한 test case 범위의 변수는 어느 정도 전역으로 확장한다. 그래서
일반적으로 대문자를 사용을 권장한다.

Local scope
'''''''''''

..
   Test cases and user keywords have a local variable scope that is not
   seen by other tests or keywords. Local variables can be created using
   `return values`__ from executed keywords and user keywords also get
   them as arguments__.

Test cases와 user keywords는 다른 테스트나 키워드에서는 보이지 않는
지역 변수 범위를 가진다. 지역 변수는 실행된 키워드의 `리턴값`__ 을
사용하여 생성할 수 있으며, 또한 user keywords의 `전달인자`__ 로 얻을
수 있다.

..
   It is recommended to use lower-case letters with local variables.

지역 변수는 소문자 사용을 권장한다.
   
..
   .. note:: Prior to Robot Framework 2.9 variables in the local scope
	     `leaked to lower level user keywords`__. This was never an
	     intended feature, and variables should be set or passed
	     explicitly also with earlier versions.

.. note:: Robot Framework 2.9 이전의 지역 범위의 변수는 `더 낮은
          수준의 user keywords에 유출되었다`__. 이것은 결코 의도된
          기능이 아니였다. 그래서 변수는 이전 버전에서도 명시적으로
          설정하거나 전달되어야 한다.
	     
__ `Setting variables in command line`_
__ `Return values from keywords`_
__ `User keyword arguments`_
__ https://github.com/robotframework/robotframework/issues/532

Advanced variable features
--------------------------

Extended variable syntax
~~~~~~~~~~~~~~~~~~~~~~~~

..
   Extended variable syntax allows accessing attributes of an object assigned
   to a variable (for example, `${object.attribute}`) and even calling
   its methods (for example, `${obj.getName()}`). It works both with
   scalar and list variables, but is mainly useful with the former

확장 변수 문법(extended variable syntax)은 변수에 할당된 객체의
속성으로 접근(예, `${object.attribute}`)할 수 있고 심지어 메소드를
호출(예, `${obj.getName()}`)할 수도 있다. 이는 스칼라 및 리스트 변수
모두 동작하지만 주로 전자에서 사용한다.

..
   Extended variable syntax is a powerful feature, but it should
   be used with care. Accessing attributes is normally not a problem, on
   the contrary, because one variable containing an object with several
   attributes is often better than having several variables. On the
   other hand, calling methods, especially when they are used with
   arguments, can make the test data pretty complicated to understand.
   If that happens, it is recommended to move the code into a test library.

확장 변수 문법은 강력하지만 주의해서 사용해야 한다. 일반적으로 속성에
접근하는 것은 문제가 되지 않는다. 반대로 여러 속성을 가지는 객체를
포함하는 하나의 변수가 여러 변수를 가지는 것보다 종종 낫다. 특히
전달인자와 함께 사용하는 메소드 호출은 테스트 데이타를 이해하는 것을
꽤 복잡하게 만들 수 있다. 그렇게 되면 코드를 테스트 라이브러리로
이동할 것을 권장한다.

..
   The most common usages of extended variable syntax are illustrated
   in the example below. First assume that we have the following `variable file`_
   and test case:

확장 변수 문법의 가장 일반적인 용도는 아래 예에서 설명한다. 먼저
`variable file`_ 과 test case가 아래와 같이 있다고 가정한다:
   
.. sourcecode:: python

   class MyObject:

       def __init__(self, name):
           self.name = name

       def eat(self, what):
           return '%s eats %s' % (self.name, what)

       def __str__(self):
           return self.name

   OBJECT = MyObject('Robot')
   DICTIONARY = {1: 'one', 2: 'two', 3: 'three'}

.. sourcecode:: robotframework

   *** Test Cases ***
   Example
       KW 1    ${OBJECT.name}
       KW 2    ${OBJECT.eat('Cucumber')}
       KW 3    ${DICTIONARY[2]}

..
   When this test data is executed, the keywords get the arguments as
   explained below:

이 테스트 데이타가 실행될 때 키워드는 아래 설명처럼 전달인자를 얻는다:
   
- :name:`KW 1` gets string `Robot`
- :name:`KW 2` gets string `Robot eats Cucumber`
- :name:`KW 3` gets string `two`
     
  
..
   The extended variable syntax is evaluated in the following order:

확장 변수 문법은 다음 순서로 평가된다:
   
..
   1. The variable is searched using the full variable name. The extended
      variable syntax is evaluated only if no matching variable
      is found.

1. 변수는 전체 변수 이름으로 검색된다. 일치하는 변수가 발견되지 않는
   경우에만 확장 변수 문법이 평가된다.

..
   2. The name of the base variable is created. The body of the name
      consists of all the characters after the opening `{` until
      the first occurrence of a character that is not an alphanumeric character
      or a space. For example, base variables of `${OBJECT.name}`
      and `${DICTIONARY[2]}`) are `OBJECT` and `DICTIONARY`,
      respectively.

2. 기본 변수의 이름이 생성된다. 이름의 몸체는 `{` 이후 알파벳 문자나
   공백이 아닌 문자를 처음 만날때까지의 모든 문자로 구성한다. 예를
   들어 `${OBJECT.name}` 와 `${DICTIONARY[2]}` 기본 변수는 각각
   `OBJECT` 와 `DICTIONARY` 이다.
   
..
   3. A variable matching the body is searched. If there is no match, an
      exception is raised and the test case fails.

3. 몸체와 일치하는 변수가 검색된다. 일치하는 것이 없는 경우 예외가
   발생하고 test case는 실패한다.
      
..
   4. The expression inside the curly brackets is evaluated as a Python
      expression, so that the base variable name is replaced with its
      value. If the evaluation fails because of an invalid syntax or that
      the queried attribute does not exist, an exception is raised and
      the test fails.

4. 중괄호 안의 표현식은 파이썬 표현식으로 평가된다. 그래서 기본 변수
   이름은 그 값으로 대체된다. 잘못된 문법 또는 쿼리된 속성이 존재한지
   않아 평가가 실패한다면 예외가 발생하고 테스트는 실패한다.
      
..
   5. The whole extended variable is replaced with the value returned
      from the evaluation.

5. 전체 확장 변수는 평가에서 반환된 값으로 대체된다.
   
..
   If the object that is used is implemented with Java, the extended
   variable syntax allows you to access attributes using so-called bean
   properties. In essence, this means that if you have an object with the
   `getName`  method set into a variable `${OBJ}`, then the
   syntax `${OBJ.name}` is equivalent to but clearer than
   `${OBJ.getName()}`. The Python object used in the previous example
   could thus be replaced with the following Java implementation:

사용되는 객체를 자바로 구현하는 경우, 확장 변수 문법은 소위 콩
속성(bean properies)를 사용하여 속성에 접근할 수 있다. 본질적으로
`getName` 메소드를 가지는 객체를 변수 `${OBJ}` 설정하면 `${OBJ.name}`
문법은 `${OBJ.getName()}` 과 동일하지만 더 분명하다. 따라서 이전의
예제에서 사용된 파이썬 객체는 다음 자바 구현체로 대체 될 수 있다:

.. sourcecode:: java

 public class MyObject:

     private String name;

     public MyObject(String name) {
         name = name;
     }

     public String getName() {
         return name;
     }

     public String eat(String what) {
         return name + " eats " + what;
     }

     public String toString() {
         return name;
     }
 }

..
   Many standard Python objects, including strings and numbers, have
   methods that can be used with the extended variable syntax either
   explicitly or implicitly. Sometimes this can be really useful and
   reduce the need for setting temporary variables, but it is also easy
   to overuse it and create really cryptic test data. Following examples
   show few pretty good usages.

문자열과 숫자를 포함하여 많은 표준 파이썬 객체는 명시적 또는
암시적으로 확장 변수 문법과 함께 사용할 수 있는 메소드를 가진다.
때때로 이것은 매우 유용하여 임시 변수를 설정하기 위한 필요성을
감소시킨다. 그러나 이는 남용하기 쉬워 매우 애매한 테스트 데이타를
생성할 수 있다. 다음 예는 꽤 괜찮은 사용법을 보여준다.

.. sourcecode:: robotframework

   *** Test Cases ***
   String
       ${string} =    Set Variable    abc
       Log    ${string.upper()}      # Logs 'ABC'
       Log    ${string * 2}          # Logs 'abcabc'

   Number
       ${number} =    Set Variable    ${-2}
       Log    ${number * 10}         # Logs -20
       Log    ${number.__abs__()}    # Logs 2

..
   Note that even though `abs(number)` is recommended over
   `number.__abs__()` in normal Python code, using
   `${abs(number)}` does not work. This is because the variable name
   must be in the beginning of the extended syntax. Using `__xxx__`
   methods in the test data like this is already a bit questionable, and
   it is normally better to move this kind of logic into test libraries.

비록 보통의 파이썬 코드에서는 `number.__abs__()` 보다 `abs(number)` 를
권장하지만 `${abs(number)}` 는 동작하지 않는다. 변수 이름은 반드시
확장 문법의 시작 부분에 있어야 하기 때문이다. 테스트 데이타에서
`__xxx__` 메소드 사용은 이미 약간 의심스럽고, 일반적으로 이런 종류의
로직은 테스트 라이브러리로 옮기는 것이 더 낫다.

..
   Extended variable syntax works also in `list variable`_ context.
   If, for example, an object assigned to a variable `${EXTENDED}` has
   an attribute `attribute` that contains a list as a value, it can be
   used as a list variable `@{EXTENDED.attribute}`.

확장 변수 문법은 `리스트 변수 <list variable_>`__ 문맥에서도 동작한다.
예를 들어 `${EXTENDED}` 변수에 할당된 객체가 리스트 값을 포함하는
`attribute` 속성을 가지는 경우 리스트 변수 `@{EXTENDED.attribute}` 로
사용할 수 있다.

Extended variable assignment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Starting from Robot Framework 2.7, it is possible to set attributes of
   objects stored to scalar variables using `keyword return values`__ and
   a variation of the `extended variable syntax`_. Assuming we have
   variable `${OBJECT}` from the previous examples, attributes could
   be set to it like in the example below.

Robot Framework 2.7부터 `키워드 리턴 값`__ 과 `확장 변수 문법
<extended variable syntax_>`__ 의 변형을 사용하여 스칼라 변수에 저장된
객체의 속성을 설정할 수 있다. 이전 예제에서 `${OBJECT}` 변수를
가진다면 속성은 아래 예처럼 설정할 수 있다.

__ `Return values from keywords`_

.. sourcecode:: robotframework

   *** Test Cases ***
   Example
       ${OBJECT.name} =    Set Variable    New name
       ${OBJECT.new_attr} =    Set Variable    New attribute

..
   The extended variable assignment syntax is evaluated using the
   following rules:

확장 변수 할당 문법은 다음 순서로 평가된다:

..
   1. The assigned variable must be a scalar variable and have at least
      one dot. Otherwise the extended assignment syntax is not used and
      the variable is assigned normally.

1. 할당된 변수는 반드시 스칼라 변수이어야 하며 적어도 점을 하나 가지고
   있어야 한다. 그렇지 않으면 확장 할당 문법은 사용되지 않고, 변수는
   일반적으로 할당된다.
   
..
   2. If there exists a variable with the full name
      (e.g. `${OBJECT.name}` in the example above) that variable
      will be assigned a new value and the extended syntax is not used.

2. 전체 이름을 가지는 변수(예. 위의 예에서 `${OBJECT.name}`)가
   존재하면, 변수는 새로운 값으로 할당하고 확장 문법은 사용하지 않는다.
      
..
   3. The name of the base variable is created. The body of the name
      consists of all the characters between the opening `${` and
      the last dot, for example, `OBJECT` in `${OBJECT.name}`
      and `foo.bar` in `${foo.bar.zap}`. As the second example
      illustrates, the base name may contain normal extended variable
      syntax.

3. 기본 변수의 이름이 생성된다. 이름의 몸체는 `${` 시작과 마지막
   점사이의 모든 문자로 구성된다. 예를 들면 `${OBJECT.name}` 의
   `OBJECT` 와 `${foo.bar.zap}` 에서 `foo.bar` 이다. 두번째 예에서
   보여준 것 처럼 기본 이름(`foo.bar`)은 보통 확장 변수 문법을 포함 할
   수 있다.
      
..
   4. The name of the attribute to set is created by taking all the
      characters between the last dot and the closing `}`, for
      example, `name` in `${OBJECT.name}`. If the name does not
      start with a letter or underscore and contain only these characters
      and numbers, the attribute is considered invalid and the extended
      syntax is not used. A new variable with the full name is created
      instead.

4. 설정하기 위한 속성의 이름은 마지막 점과 닫는 `}` 사이의 모든 문자로
   생성된다. 예를 들어 `${OBJECT.name}` 의 `name`. 이름이 문자 또는
   언더스코어로 시작하지 않고 단지 이런 문자와 숫자를 포함하지 않으면
   속성은 유효하지 않은 것으로 간주되고 확장 문법은 사용되지 않는다.
   대신 전체 이름을 가지는 새로운 변수가 생성된다.

..
   5. A variable matching the base name is searched. If no variable is
      found, the extended syntax is not used and, instead, a new variable
      is created using the full variable name.

5. 기본 이름이 일치하는 변수가 검색된다. 변수가 찾을 수 없다면, 확장
   문법은 사용되지 않고 대신 새로운 변수가 전체 변수이름을 사용하여
   생성된다.
   
..
   6. If the found variable is a string or a number, the extended syntax
      is ignored and a new variable created using the full name. This is
      done because you cannot add new attributes to Python strings or
      numbers, and this way the new syntax is also less
      backwards-incompatible.

6. 찾은 변수가 문자열 또는 숫자이면 확장 문법은 무시되고 새로운 변수가
   전체 이름을 사용하여 생성된다. 이렇게 하는 이유는 파이썬 문자열
   또는 숫자에 새로운 속성을 추가할 수 없기 때문이다. 이와 같은 새로운
   문법이 그나마 하위비호환성이 덜하다.
   
..
   7. If all the previous rules match, the attribute is set to the base
      variable. If setting fails for any reason, an exception is raised
      and the test fails.

7. 모든 이전 규칙이 일치하는 경우 속성은 기본 변수로 설정된다. 설정이
   어떤 이유로 실패 할 경우 예외가 발생하고 테스트가 실패한다.
   
..
   .. note:: Unlike when assigning variables normally using `return
	     values from keywords`_, changes to variables done using the
	     extended assign syntax are not limited to the current
	     scope. Because no new variable is created but instead the
	     state of an existing variable is changed, all tests and
	     keywords that see that variable will also see the changes.

.. note:: 일반적으로 `키워드의 반환 값 <return values from
          keywords_>`__ 을 사용하여 변수를 할당 할 때와는 달리, 확장
          할당 구문을 사용하여 변수를 변경하는 것은 현재 범위에
          한정되지 않는다. 새로운 변수가 생성되지 않고 대신 기존
          변수의 상태가 변경되기 때문에, 그 변수를 참고하는 모든
          테스트 및 키워드는 변경 내용을 볼 수 있다.
	  
Variables inside variables
~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Variables are allowed also inside variables, and when this syntax is
   used, variables are resolved from the inside out. For example, if you
   have a variable `${var${x}}`, then `${x}` is resolved
   first. If it has the value `name`, the final value is then the
   value of the variable `${varname}`. There can be several nested
   variables, but resolving the outermost fails, if any of them does not
   exist.

변수는 내부 변수를 허용하고 이 문법이 사용될 때, 변수 안팎에서
해석된다. 예를 들어 변수 `${var${x}}` 가 있다면 `${x}` 가 먼저
해석된다. `${x}` 가 `name` 값을 가진다면 마지막 값은 변수 `${varname}`
의 값이 된다. 변수가 여러번 중첩될 수 있지만 그 중 어느 하나가
존재하지 않을 경우 가장 바깥쪽 해석은 실패한다.

..
   In the example below, :name:`Do X` gets the value `${JOHN HOME}`
   or `${JANE HOME}`, depending on if :name:`Get Name` returns
   `john` or `jane`. If it returns something else, resolving
   `${${name} HOME}` fails.

아래 예에서 :name:`Do X` 는 :name:`Get Name` 이 `john` 또는 `jane` 을
리턴하는 것에 따라 `${JOHN HOME}` 또는 `${JANNE HOME}` 값을 얻는다.
만약 그 밖의 다른 값을 리턴하면 `${${name} HOME}` 해석은 실패한다.

.. sourcecode:: robotframework

   *** Variables ***
   ${JOHN HOME}    /home/john
   ${JANE HOME}    /home/jane

   *** Test Cases ***
   Example
       ${name} =    Get Name
       Do X    ${${name} HOME}
