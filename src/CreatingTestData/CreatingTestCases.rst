Creating test cases
===================

..
   This section describes the overall test case syntax. Organizing test
   cases into `test suites`_ using `test case files`_ and `test suite
   directories`_ is discussed in the next section.

이번 섹션에서는, 전반적인 테스트 케이스 문법을 다룬다. `test suites`_
를 `test case files`_ 와 `test suite directories`_ 를 이용해서
구성하는 것은 다음 섹션에서 다룬다.

.. contents::
   :depth: 2
   :local:

Test case syntax
----------------

Basic syntax
~~~~~~~~~~~~

..
   Test cases are constructed in test case tables from the available
   keywords. Keywords can be imported from `test libraries`_ or `resource
   files`_, or created in the `keyword table`_ of the test case file
   itself.

테스트 케이스는 사용가능한 키워드로 작성된 테스트 케이스 표로
구성된다. 키워드는 `test libraries`_ 혹은 `resource files`_ 에서
임포트 되거나, 테스트 케이스 파일의 `keyword table`_ 에서 생성된다.

.. _keyword table: `user keywords`_

..
   The first column in the test case table contains test case names. A
   test case starts from the row with something in this column and
   continues to the next test case name or to the end of the table. It is
   an error to have something between the table headers and the first
   test.

테스트 케이스 표의 첫번째 열은 테스트 케이스 이름을 포함한다. 테스트
케이스는 이 열의 어느 행에서 시작하고, 다음 테스트 케이스 이름이나
표의 끝까지 이어진다. 테이블 헤더와 첫번째 테스트 사이에는 아무것도 없어야 한다.

.. The second column normally has keyword names. An exception to this
   rule is `setting variables from keyword return values`_, when the
   second and possibly also the subsequent columns contain variable
   names and a keyword name is located after them. In either case,
   columns after the keyword name contain possible arguments to the
   specified keyword.

두번째 열은 일반적으로 키워드 이름을 갖는다. 이것에 대한 예외는
`키워드 리턴 값으로 부터 변수를 설정하는 것 <User keyword return
values_>`__ 이다. 이것은 두번째 혹은 연속된 열이 변수명을 포함하고,
키워드 이름이 변수명뒤에 있을 때이다. 또 다른 경우는 키워드 이름
이후의 열들이 키워드에 대한 가능한 전달인자를 포함하는 것이다.


.. _example-tests:

예제:

.. sourcecode:: robotframework

   *** Test Cases ***
   Valid Login
       Open Login Page
       Input Username    demo
       Input Password    mode
       Submit Credentials
       Welcome Page Should Be Open

   Setting Variables
       Do Something    first argument    second argument
       ${value} =    Get Some Value
       Should Be Equal    ${value}    Expected value

Settings in the Test Case table
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Test cases can also have their own settings. Setting names are always
   in the second column, where keywords normally are, and their values
   are in the subsequent columns. Setting names have square brackets around
   them to distinguish them from keywords. The available settings are listed
   below and explained later in this section.

테스트 케이스는 고유의 설정을 가진다. 설정 이름은 항상 두번째 열(보통
키워드가 위치하는)에 위치하고, 값은 다음 열에 있다. 설정 이름은
키워드와 구분짓기 위해서 대괄호로 묶여있다. 이용가능한 설정은 아래에
나열되어있고, 이어서 설명한다.

`[Documentation]`:setting:
    `test case documentation`_ 을 기술하기 위해 사용.

`[Tags]`:setting:
    `tagging test cases`_ 를 위해 사용.

`[Setup]`:setting:, `[Teardown]`:setting:
  `test setup and teardown`_ 명기.

`[Template]`:setting:
   `template keyword`_ 명기를 위해 사용. 테스트는 단지 키워드에 대한 전달인자로 사용할 데이타만 포함한다.

`[Timeout]`:setting:
   `test case timeout`_ 설정을 위해 사용. Timeouts_ 은 별도의 섹션에서 설명한다.


..
   `[Documentation]`:setting:
       Used for specifying a `test case documentation`_.

   `[Tags]`:setting:
       Used for `tagging test cases`_.

   `[Setup]`:setting:, `[Teardown]`:setting:
      Specify `test setup and teardown`_.

   `[Template]`:setting:
      Specifies the `template keyword`_ to use. The test itself will
      contain only data to use as arguments to that keyword.

   `[Timeout]`:setting:
      Used for setting a `test case timeout`_. Timeouts_ are discussed in
      their own section.

..
   Example test case with settings:

설정 관련 테스트 케이스 예제:

.. sourcecode:: robotframework

   *** Test Cases ***
   Test With Settings
       [Documentation]    Another dummy test
       [Tags]    dummy    owner-johndoe
       Log    Hello, world!

Test case related settings in the Setting table
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   The Setting table can have the following test case related
   settings. These settings are mainly default values for the
   test case specific settings listed earlier.

설정 표는 다음의 설정과 관련된 테스트 케이스를 갖는다. 이 설정은 주로
이전에 지정된 기본값이다.

`Force Tags`:setting:, `Default Tags`:setting:
   The forced and default values for tags_.

`Test Setup`:setting:, `Test Teardown`:setting:
   The default values for `test setup and teardown`_.

`Test Template`:setting:
   The default `template keyword`_ to use.

`Test Timeout`:setting:
   The default value for `test case timeout`_. Timeouts_ are discussed in
   their own section.

   
Using arguments
---------------

..
   The earlier examples have already demonstrated keywords taking
   different arguments, and this section discusses this important
   functionality more thoroughly. How to actually implement `user
   keywords`__ and `library keywords`__ with different arguments is
   discussed in separate sections.

이전의 예제에서는 다른 전달인자를 가지는 키워드를 설명했고, 이번
섹션에서는 더욱 자세하게 중요한 기능에 대하여 논의한다. 다른
전달인자를 가지는 `user keywords`__ 와 `library keywords`__ 를 실제로
구현하는 방법에 대하여 섹션을 나누어 다룬다.

..
   Keywords can accept zero or more arguments, and some arguments may
   have default values. What arguments a keyword accepts depends on its
   implementation, and typically the best place to search this
   information is keyword's documentation. In the examples in this
   section the documentation is expected to be generated using the
   Libdoc_ tool, but the same information is available on
   documentation generated by generic documentation tools such as
   ``javadoc``.

키워드는 0개 이상의 전달인자를 가질 수 있다. 몇 몇 전달인자는 기본
값을 가질지도 모른다. 키워드가 어떤 전달인자를 받을지는 구현에
의존적이다. 일반적으로 이러한 정보를 검색하기 가장 좋은 곳은 키워드
문서(documentation)이다. 이 섹션의 예에서 문서는 Libdoc_ 툴을 사용해서
생성되는 것을 기본으로 한다. 동일한 정보가 ``javadoc`` 과 같은 일반
문서 도구를 사용해서 생성할 수도 있다.

__ `User keyword arguments`_
__ `Keyword arguments`_


Mandatory arguments
~~~~~~~~~~~~~~~~~~~

..
   Most keywords have a certain number of arguments that must always be
   given.  In the keyword documentation this is denoted by specifying the
   argument names separated with a comma like `first, second,
   third`. The argument names actually do not matter in this case, except
   that they should explain what the argument does, but it is important
   to have exactly the same number of arguments as specified in the
   documentation. Using too few or too many arguments will result in an
   error.

대부분의 키워드들은 항상 주어지는 일정 수의 전달인자를 갖는다. 키워드
문서에서 `first, second, third` 와 같이 콤마를 이용해 구분된
전달인자의 이름을 구분하여 표시한다. 전달인자 이름은 전달인자가 무엇을
하는지 설명하는 한다는 것을 제외하고는 사실 이 경우에는 문제가 되지
않는다. 하지만 문서에는 정확히 동일한 수의 전달인자를 명기하는 것은
중요하다. 너무 적거나 많은 수의 전달인자를 사용하면 에러가 발생한다.

..
   The test below uses keywords :name:`Create Directory` and :name:`Copy
   File` from the OperatingSystem_ library. Their arguments are
   specified as `path` and `source, destination`, which means
   that they take one and two arguments, respectively. The last keyword,
   :name:`No Operation` from BuiltIn_, takes no arguments.

아래 테스트에서는 OperatingSystem_ 라이브러리의 :name:`Create
Directory` 와 :name:`Copy File` 키워드를 사용한다. 해당 키워드의
전달인자는 `path` 와 `source, destination` 으로 적었다. 이것은 하나
혹은 두개의 전달인자를 각각 갖는다는 것을 의미한다. 마지막 BuiltIn_ 의
:name:`No Operation` 키워드는 전달인자를 갖지 않는다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Example
       Create Directory    ${TEMPDIR}/stuff
       Copy File    ${CURDIR}/file.txt    ${TEMPDIR}/stuff
       No Operation

Default values
~~~~~~~~~~~~~~

..
   Arguments often have default values which can either be given or
   not. In the documentation the default value is typically separated
   from the argument name with an equal sign like `name=default
   value`, but with keywords implemented using Java there may be
   `multiple implementations`__ of the same keyword with different
   arguments instead. It is possible that all the arguments have default
   values, but there cannot be any positional arguments after arguments
   with default values.

전달인자는 종종 기본 값을 가진다. 기본값은 주어지거나 그렇지 않을 수
있다. 문서에서 `name=default value` 와 같이 동등 부호를 통해 전달인자
이름과 기본 값을 구분한다. 하지만 자바를 이용해서 구현한 키워드에는
서로 다른 전달인자를 가지는 동일 키워드의 `여러 구현체`__ 가 존재할
지도 모른다. 모든 전달인자가 기본 값을 가질 수 있다. 하지만 기본 값을
가지는 전달 인자 뒤에는 임의의 위치 전달인자(positional arguments)가
존재할 수 없다.

__ `Default values with Java`_

..
   Using default values is illustrated by the example below that uses
   :name:`Create File` keyword which has arguments `path, content=,
   encoding=UTF-8`. Trying to use it without any arguments or more than
   three arguments would not work.

아래의 예제는 `path, content=, encoding=UTF-8` 전달인자를 갖는
:name:`Create File` 키워드에서 기본 값을 사용하는 것을 다뤘다.
전달인자 없이 혹은 3개 이상의 전달인자를 사용하려고 한다면 정상 동작
하지 않을 것이다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Example
       Create File    ${TEMPDIR}/empty.txt
       Create File    ${TEMPDIR}/utf-8.txt         Hyvä esimerkki
       Create File    ${TEMPDIR}/iso-8859-1.txt    Hyvä esimerkki    ISO-8859-1

.. _varargs:

Variable number of arguments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   It is also possible that a keyword accepts any number of arguments.
   These so called *varargs* can be combined with mandatory arguments
   and arguments with default values, but they are always given after
   them. In the documentation they have an asterisk before the argument
   name like `*varargs`.

키워드는 임의 갯수의 전달인자를 가질 수 있다. 이것은 *varargs* 라고
불리고, 필수 전달인자와 기본값을 가지는 전달인자와 조합하여 사용할 수
있다. 하지만 항상 이 두가지 변수 뒤에 있어야 한다. 문서에서 `*varargs`
와 같이 전달인자 이름 앞에 별표를 붙인다.

..
   For example, :name:`Remove Files` and :name:`Join Paths` keywords from
   the OperatingSystem_ library have arguments `*paths` and `base, *parts`,
   respectively. The former can be used with any number of arguments, but
   the latter requires at least one argument.

예를들어 OperatingSystem_ 라이브러리의 :name:`Remove Files` 와
:name:`Join Paths` 키워드는 각각 `*paths` 와 `base, *parts` 를
전달인자로 갖는다. :name:`Remove Files` 는 몇 개의 전달인자를 가지거나
상관 없지만, :name:`Join Paths` 는 최소 한개의 전달인자를 가져야 한다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Example
       Remove Files    ${TEMPDIR}/f1.txt    ${TEMPDIR}/f2.txt    ${TEMPDIR}/f3.txt
       @{paths} =    Join Paths    ${TEMPDIR}    f1.txt    f2.txt    f3.txt    f4.txt

.. _Named argument syntax:

Named arguments
~~~~~~~~~~~~~~~

..
   The named argument syntax makes using arguments with `default values`_ more
   flexible, and allows explicitly  what a certain argument value means.
   Technically named arguments work exactly like `keyword arguments`__ in Python.

명명된 전달인자(named argument) 문법은 `기본값 <default values_>`__ 을
갖는 전달인자 사용에 대한 활용도를 높인다. 그리고 특정 전달인자의 값이
갖는 의미를 명시적으로 표시한다. 기술적으로 명명된 전달인자는 파이썬의
`keyword arguments`__ 처럼 동작한다.

__ http://docs.python.org/2/tutorial/controlflow.html#keyword-arguments

Basic syntax
''''''''''''

..
   It is possible to name an argument given to a keyword by prefixing the value
   with the name of the argument like `arg=value`. This is especially
   useful when multiple arguments have default values, as it is
   possible to name only some the arguments and let others use their defaults.
   For example, if a keyword accepts arguments `arg1=a, arg2=b, arg3=c`,
   and it is called with one argument `arg3=override`, arguments
   `arg1` and `arg2` get their default values, but `arg3`
   gets value `override`. If this sounds complicated, the `named arguments
   example`_ below hopefully makes it more clear.

`arg=value` 와 같이 전달인자 이름을 값의 접두어로 사용하여 키워드
전달인자에 이름을 지어 줄 수 있다. 여러개의 전달인자가 기본 값을 가질
때, 그 중 몇몇 전달인자만 이름을 갖게 하고 나머지는 기본 값을 사용할
수 있어 특히 유용하다. 예를들어, 만약 키워드가 `arg1=a, arg2=b,
arg3=c` 와 같이 전달인자를 받는다면, `arg3=override` 전달인자 하나만
으로 호출 될 경우, `arg1` 와 `arg2` 는 기본 값을 받지만 `arg3`는
`override` 값을 받는다. 만약 이러한 것이 좀 어렵게 들린다면, 아래
`명명된 전달인자 예제 <named arguments example_>`__ 가 이해를 도울
것이다.

..
   The named argument syntax is both case and space sensitive. The former
   means that if you have an argument `arg`, you must use it like
   `arg=value`, and neither `Arg=value` nor `ARG=value`
   works.  The latter means that spaces are not allowed before the `=`
   sign, and possible spaces after it are considered part of the given value.

명명된 전달인자 문법은 대소문자와 공백을 구분한다. 만약 전달인자
`arg` 에 어떤 값을 주어주면 `arg=value` 가 되지만 `Arg=value` 와
`ARG=value` 는 별개의 것이다. `=` 앞에는 공백을 허용하지 않는다.
뒤에만 공백을 허용하고 주어진 값의 일부로 여긴다.

..
   When the named argument syntax is used with `user keywords`_, the argument
   names must be given without the `${}` decoration. For example, user
   keyword with arguments `${arg1}=first, ${arg2}=second` must be used
   like `arg2=override`.

명명된 전달인자 문법이 `user keywords`_ 와 함께 사용할 때, 전달인자
이름은 `${}` 표식 없이 작성되어야 한다. 예를들어, `${arg1}=first,
${arg2}=second` 전달인자를 갖는 user keyword는 반드시 `arg2=override`
와 같이 사용해야 한다.

..
   Using normal positional arguments after named arguments like, for example,
   `| Keyword | arg=value | positional |`, does not work.
   Starting from Robot Framework 2.8 this causes an explicit error.
   The relative order of the named arguments does not matter.

`| Keyword | arg=value | positional |` 와 같이 명명된 전달인자 뒤에
위치 전달인자가 오는 경우에는 정상 동작 하지 않는다. Robot Framework
2.8부터 이것은 명백한 에러를 발생시킨다. 명명된 전달인자의 상대적인
순서는 상관 없다.

..
   .. note:: Prior to Robot Framework 2.8 it was not possible to name arguments
	     that did not have a default value.

.. note:: Robot Framework 2.8 이전에는 명명된 전달인자는 기본 값을
           갖을 수 없었다.

Named arguments with variables
''''''''''''''''''''''''''''''

..
   It is possible to use `variables`_ in both named argument names and values.
   If the value is a single `scalar variable`_, it is passed to the keyword as-is.
   This allows using any objects, not only strings, as values also when using
   the named argument syntax. For example, calling a keyword like `arg=${object}`
   will pass the variable `${object}` to the keyword without converting it to
   a string.

`변수 <variables_>`__ 를 명명된 전달인자 이름과 값에서 사용할 수 있다.
만일 값이 단일 `스칼라 변수 <scalar variable_>`__ 라면, 키워드에
그대로 전달 된다. 명명된 전달인자 문법을 사용할 때, 문자열 뿐만 아니라
어떤 객체든지 값으로 사용할 수 있다. 예를 들어, `arg=${object}` 같이
키워드를 호출하는 것은 변수 `${object}` 를 스트링으로 변환하지 않고
전달하는 것이다.

..
   If variables are used in named argument names, variables are resolved before
   matching them against argument names. This is a new feature in Robot Framework
   2.8.6.

만일 변수가 명명된 전달인자 이름에서 사용된다면, 전달인자 이름과
매칭되기 전에 변수는 해석되어져야 한다. 이것은 Robot Framework 2.8.6의
새로운 기능이다.


..
   The named argument syntax requires the equal sign to be written literally
   in the keyword call. This means that variable alone can never trigger the
   named argument syntax, not even if it has a value like `foo=bar`. This is
   important to remember especially when wrapping keywords into other keywords.
   If, for example, a keyword takes a `variable number of arguments`_ like
   `@{args}` and passes all of them to another keyword using the same `@{args}`
   syntax, possible `named=arg` syntax used in the calling side is not recognized.
   This is illustrated by the example below.

몀명된 전달인자 문법은 키워드를 호출 할 때 등호(`=`)를 사용해야 한다.
이것은 `foo=bar` 와 같은 값을 가지지 않는다면, 변수 혼자서는 명명된
전달인자 문법을 동작하게 할 수 없다는 것을 의미한다. 이것은 특히
키워드안에서 키워드를 래핑(wrapping) 할 때 중요하다. 예를 들어,
키워드가 `@{args}` 와 같이 `가변 인자 <variable number of
arguments_>`__ 를 가지고, `@{args}` 구문을 사용하여 가변 인자를 다른
키워드에 전달한다면, 호출부에서 사용된 `named=arg` 문법은 인지되지
않을 것이다. 아래에 그 예가 있다.

..
   .. sourcecode:: robotframework

      *** Test Cases ***
      Example
	  Run Program    shell=True    # This will not come as a named argument to Run Process

      *** Keywords ***
      Run Program
	  [Arguments]    @{args}
	  Run Process    program.py    @{args}    # Named arguments are not recognized from inside @{args}

.. sourcecode:: robotframework

   *** Test Cases ***
   Example
       Run Program    shell=True    # 이것은 Run Process에 명명된 전달인자로 전달되지 않는다.

   *** Keywords ***
   Run Program
       [Arguments]    @{args}
       Run Process    program.py    @{args}    # 명명된 인자는 내부의 @{args}에서 인식되지 않는다.

   
..
   If keyword needs to accept and pass forward any named arguments, it must be
   changed to accept `free keyword arguments`_. See `kwargs examples`_ for
   a wrapper keyword version that can pass both positional and named arguments
   forward.

만약 키워드가 명명된 전달인자를 받거나 전달해야 한다면, `free keyword
arguments`_ 를 받도록 변경해야 한다. 위치 인자와 명명된 인자를 둘다
전달할 수 있는 랩퍼 키워드 버전은 `kwargs examples`_ 을 참고하라.


Escaping named arguments syntax
'''''''''''''''''''''''''''''''

..
   The named argument syntax is used only when the part of the argument
   before the equal sign matches one of the keyword's arguments. It is possible
   that there is a positional argument with a literal value like `foo=quux`,
   and also an unrelated argument with name `foo`. In this case the argument
   `foo` either incorrectly gets the value `quux` or, more likely,
   there is a syntax error.

명명된 전달인자 문법은 등호 앞의 전달인자 부분이 키워드의 전달인자 중
하나와 일치 할 때만 사용된다. `foo=quux` 와 같은 문자그래로의 값과
함께 위치 인자 문자 그리고 `foo` 와 같이 연결고리 없는 전달인자를
가지는 것이 가능하다. 이런 경우 전달인자 `foo` 는 부정확하게 `quux`
값을 가지거나, 문법 에러를 일으킬 가능성이 있다.

..
   In these rare cases where there are accidental matches, it is possible to
   use the backslash character to escape__ the syntax like `foo\=quux`.
   Now the argument will get a literal value `foo=quux`. Note that escaping
   is not needed if there are no arguments with name `foo`, but because it
   makes the situation more explicit, it may nevertheless be a good idea.

의도하지 않은 매칭 문제가 발생하는 이런 드문 경우에는, 백슬래시 문자를
`foo\=quux` 와 같이 `이스케이프`__ 목적으로 사용할 수 있다. 이제
전달인자는 `foo=quux` 와 같이 문자그대로의 값을 가진다. `foo` 라는
이름을 가진 전달인자가 없다면 이스케이핑할 필요는 없다. 하지만 이렇게
사용하는 것이 상황을 더 명확하게하기 때문에 추천한다.

__ Escaping_

Where named arguments are supported
'''''''''''''''''''''''''''''''''''

..
   As already explained, the named argument syntax works with keywords. In
   addition to that, it also works when `importing libraries`_.

이미 설명했듯이, 명명된 전달인자 문법은 키워드와 함께 잘 동작한다.
게다가 `라이브러리 임포팅 <importing libraries_>`__ 에서도 또한 잘
동작한다.

..
   Naming arguments is supported by `user keywords`_ and by most `test libraries`_.
   The only exception are Java based libraries that use the `static library API`_.
   Library documentation generated with Libdoc_ has a note does the library
   support named arguments or not.

명명된 전달인자는 `user keywords`_ 와 `test libraries`_ 에서 지원한다.
유일하게 예외적으로 자바를 기반으로 한 라이브러리는 `static library
API`_ 를 사용한다. Libdoc_ 를 이용해서 만들어진 라이브러리 문서는
라이브러리가 명명된 전달인자를 지원 하는지, 안하는지를 기록한다.

..
   .. note:: Prior to Robot Framework 2.8 named argument syntax did not work
	     with test libraries using the `dynamic library API`_.

.. note:: Robot Framework 2.8 이전에는 `dynamic library API`_ 를
          이용한 테스트 라이브러리에서는 명명된 전달인자 문법을
          지원하지 않았다.

Named arguments example
'''''''''''''''''''''''

..
   The following example demonstrates using the named arguments syntax with
   library keywords, user keywords, and when importing the Telnet_ test library.

다음 예제는 Telnet_ 테스트 라이브러리를 임포팅 할 때, 라이브러리
키워드, 사용자 키워드와 함께 명명된 전달인자 문법 사용법을 보여준다.

.. sourcecode:: robotframework

   *** Settings ***
   Library    Telnet    prompt=$    default_log_level=DEBUG

   *** Test Cases ***
   Example
       Open connection    10.0.0.42    port=${PORT}    alias=example
       List files    options=-lh
       List files    path=/tmp    options=-l

   *** Keywords ***
   List files
       [Arguments]    ${path}=.    ${options}=
       List files    options=-lh
       Execute command    ls ${options} ${path}

Free keyword arguments
~~~~~~~~~~~~~~~~~~~~~~

..
   Robot Framework 2.8 added support for `Python style free keyword arguments`__
   (`**kwargs`). What this means is that keywords can receive all arguments that
   use the `name=value` syntax and do not match any other arguments as kwargs.

Robot Framework 2.8에 `파이썬 형식의 키워드 인수`__ (`**kwargs`)
지원이 추가되었다. 이것은 키워드가 `name=value` 문법을 사용하며, 다른
어떤 전달인자와 매치되지 않는 모든 전달인자를 키워드
전달인자(kwargs)로 받을 수 있다는 의미이다.

..
   Free keyword arguments support variables similarly as `named arguments
   <Named arguments with variables_>`__. In practice that means that variables
   can be used both in names and values, but the escape sign must always be
   visible literally. For example, both `foo=${bar}` and `${foo}=${bar}` are
   valid, as long as the variables that are used exist. An extra limitation is
   that free keyword argument names must always be strings. Support for variables
   in names is a new feature in Robot Framework 2.8.6, prior to that possible
   variables were left un-resolved.

키워드 전달인자는 `명명된 전달인자 <Named arguments with
variables_>`__ 와 유사하게 변수를 지원한다. 이것은 실제로 변수는
이름과 값으로 사용될 수 있지만 이스케이프 표시는 항상 문자그대로로서
보여져야 한다는 것을 의미한다. 예를 들어, 사용되어진 변수들이 존재하는
한 `foo=${bar}` 와 `${foo}=${bar}` 는 유효하다. 추가적인 제한은 키워드
전달인자 이름은 항상 문자열 이어야 한다는 것이다. 이름 안에서의 변수
지원은 Robot Framework 2.8.6 부터 지원된 새로운 기능이다. 2.8.6 이전
버전에서는 변수는 해석되지 않은 채 남겨진다.

..
   Initially free keyword arguments only worked with Python based libraries, but
   Robot Framework 2.8.2 extended the support to the `dynamic library API`_
   and Robot Framework 2.8.3 extended it further to Java based libraries and to
   the `remote library interface`_. Finally, user keywords got `kwargs support
   <Kwargs with user keywords_>`__ in Robot Framework 2.9. In other words,
   all keywords can nowadays support kwargs.

초기에 키워드 전달인자는 파이썬 기반의 라이브러리에서만 작동하였다.
하지만 Robot Framework 2.8.2 부터는 `dynamic library API`_ 를
지원하도록 확장하였고, Robot Framework 2.8.3 부터는 자바 기반의
라이브러리와 `remote library interface`_ 를 지원하도록 더 확장하였다.
Robot Framework 2.9 부터는 `키워드 전달인자 지원 <Kwargs with user
keywords_>`__ 한다. 즉, 이제 모든 키워드가 키워드 전달인자를 지원한다.

__ http://docs.python.org/2/tutorial/controlflow.html#keyword-arguments

Kwargs examples
'''''''''''''''

..
   As the first example of using kwargs, let's take a look at
   :name:`Run Process` keyword in the Process_ library. It has a signature
   `command, *arguments, **configuration`, which means that it takes the command
   to execute (`command`), its arguments as `variable number of arguments`_
   (`*arguments`) and finally optional configuration parameters as free keyword
   arguments (`**configuration`). The example below also shows that variables
   work with free keyword arguments exactly like when `using the named argument
   syntax`__.

kwargs를 사용한 첫번째 예제처럼, Process_ 라이브러리의 :name:`Run
Process` 키워드를 살펴보자. `command, *arguments, **configuration`
시그너처는 각각 수행할 명령어(`command`), `가변인자 <variable number
of arguments_>`__ 를 가지는 전달인자(`*arguments`), 선택적인 환경설정
매개변수를 가지는 키워드 전달인자(`**configuration`)를 의미한다. 아래
예제는 키워드 전달인자에 변수를 사용하는 것이 `명명된 전달인자 문법을
사용`__ 하는 것과 비슷하다는 것을 보여준다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Using Kwargs
       Run Process    program.py    arg1    arg2    cwd=/home/user
       Run Process    program.py    argument    shell=True    env=${ENVIRON}

..
   See `Free keyword arguments (**kwargs)`_ section under `Creating test
   libraries`_ for more information about using the kwargs syntax in
   your custom test libraries.

사용자 정의 테스트 라이브러리에 kwargs 문법을 사용하기 위해서 더 많은
정보가 필요 한다면 `Creating test libraries`_ 아래의 `Free keyword
arguments (**kwargs)`_ 섹션을 참고하라.

..
   As the second example, let's create a wrapper `user keyword`_ for running the
   `program.py` in the above example. The wrapper keyword :name:`Run Program`
   accepts any number of arguments and kwargs, and passes them forward for
   :name:`Run Process` along with the name of the command to execute.

두번째 예제처럼, 위의 예제에서 `program.py` 를 수행하기 위해서 래퍼
`user keyword`_ 를 생성한다. 래퍼 키워드 :name:`Run Program` 는 임의
갯수의 전달인자와 kwargs를 받아서, 수행하려는 명령어의 이름과 함께
:name:`Run Process` 에 전달한다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Using Kwargs
       Run Program    arg1    arg2    cwd=/home/user
       Run Program    argument    shell=True    env=${ENVIRON}

   *** Keywords ***
   Run Program
       [Arguments]    @{arguments}    &{configuration}
       Run Process    program.py    @{arguments}    &{configuration}

__ `Named arguments with variables`_

Arguments embedded to keyword names
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   A totally different approach to specify arguments is embedding them
   into keyword names. This syntax is supported by both `test library keywords`__
   and `user keywords`__.

명시된 전달인자에 접근하는 전체적으로 다른 방법은 키워드 이름에 인자를
끼워넣는 것이다. 이 문법은 `test library keywords`__ 와 `user
keywords`__ 에서 지원된다.

__ `Embedding arguments into keyword names`_
__ `Embedding arguments into keyword name`_

Failures
--------

When test case fails
~~~~~~~~~~~~~~~~~~~~

..
   A test case fails if any of the keyword it uses fails. Normally this means that
   execution of that test case is stopped, possible `test teardown`_ is executed,
   and then execution continues from the next test case. It is also possible to
   use special `continuable failures`__ if stopping test execution is not desired.

만일 테스트 케이스에서 사용중인 키워드가 실패한다면, 테스트 케이스도
실패한다. 일반적으로 이런 경우 테스트 케이스의 수행이 멈추고 `test
teardown`_ 를 수행하고, 다음 테스트 케이스를 이어 수행한다. 테스트
수행을 멈추고 싶지 않다면, 특별히 `실패 해도 계속 수행 하기`__ 를 사용
할 수 있다.


Error messages
~~~~~~~~~~~~~~

..
   The error message assigned to a failed test case is got directly from the
   failed keyword. Often the error message is created by the keyword itself, but
   some keywords allow configuring them.

실패한 테스트 케이스의 에러 메시지는 실패한 키워드에서 직접 가져온다.
대부분 에러 메시지를 키워드가 직접 생성하지만, 몇몇 키워드는 설정 할
수도 있다.


..
   In some circumstances, for example when continuable failures are used,
   a test case can fail multiple times. In that case the final error message
   is got by combining the individual errors. Very long error messages are
   automatically cut from the middle to keep reports_ easier to read. Full
   error messages are always visible in log_ file as a message of the failed
   keyword.

어떤 상황, 예를 들어 실패 하더라도 계속 수행하도록 하는 경우에는,
테스트 케이스가 여러번 실패할 수도 있다. 그런 경우 각각의 에러를
결합하여 최종 에러 메시지를 출력한다. 매우 긴 에러메시지의 경우
중간에서 자동적으로 잘라서 reports_ 를 읽기 쉽게 만든다. 전체 에러
메시지는 항상 log_ 파일에서 실패한 키워드의 메시지로 확인할 수 있다.

..
   By default error messages are normal text, but
   starting from Robot Framework 2.8 they can `contain HTML formatting`__. This
   is enabled by starting the error message with marker string `*HTML*`.
   This marker will be removed from the final error message shown in reports
   and logs. Using HTML in a custom message is shown in the second example below.

기본 에러 메시지는 평문이지만, Robot Framework 2.8 부터는 `HTML 형식을
포함`__ 할 수 있다. 이는 에러 메시지를 `*HTML*` 로 시작하면 설정할 수
있다. 이 표시는 레포트와 로그에서 최종 에러 메시지가 보일 때에는
제거된다. HTML을 사용자 정의 메시지에서 사용할 때는 아래 두번째
예제처럼 보인다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Normal Error
       Fail    This is a rather boring example...

   HTML Error
       ${number} =    Get Number
       Should Be Equal    ${number}    42    *HTML* Number is not my <b>MAGIC</b> number.

__ `Continue on failure`_
__ `HTML in error messages`_

Test case name and documentation
--------------------------------

..
   The test case name comes directly from the Test Case table: it is
   exactly what is entered into the test case column. Test cases in one
   test suite should have unique names.  Pertaining to this, you can also
   use the `automatic variable`_ `${TEST_NAME}` within the test
   itself to refer to the test name. It is available whenever a test is
   being executed, including all user keywords, as well as the test setup
   and the test teardown.

Test case 이름은 Test Case 표에서 직접 가져온다. 이것은 test case 열에
입력된 것이다. 한 test suite의 test cases는 독자적인 이름을 가진다.
이에 관해서, 테스트에서 테스트 이름을 언급하기 위해서 `자동 변수
<automatic variable_>`__ `${TEST_NAME}` 를 사용할 수 있다. 이 변수는
모든 사용자 키워드를 포함하여 테스트가 수행할 때마다, test setup과
test teardown에서 사용할 수 있다.

..
   The :setting:`[Documentation]` setting allows you to set a free
   documentation for a test case. That text is shown in the command line
   output, as well as the resulting test logs and test reports.
   It is possible to use simple `HTML formatting`_ in documentation and
   variables_ can be used to make the documentation dynamic.

:setting:`[Documentation]` 설정은 test case를 위한 자유로운 문서
작성을 지원한다. 문서는 명령행 결과에서 보여지며, 테스트 로그와 테스트
레포트에서도 확인할 수 있다. 간단한 `HTML 형식 <HTML formatting_>`__
를 사용할 수 있으며, `변수 <variables_>`__ 를 사용하여 문서를 좀 더
동적으로 만들 수 있다.

..
   If documentation is split into multiple columns, cells in one row are
   concatenated together with spaces. This is mainly be useful when using
   the `HTML format`_ and columns are narrow. If documentation is `split
   into multiple rows`__, the created documentation lines themselves are
   `concatenated using newlines`__. Newlines are not added if a line
   already ends with a newline or an `escaping backslash`__.

만약 문서가 여러개의 열로 나눈다면 한 줄의 셀들은 공백으로 연결된다.
이것은 `HTML 형식 <HTML format_>`__ 사용할 때와 열이 좁을 때 유용하다.
만약 문서가 `여러 줄로 나뉜게`__ 된다면, 생성된 문서 라인들은 `개행
문자을 사용하여 이어붙일`__ 것이다. 이미 라인이 개행 문자 혹은
`이스케이핑 백슬래시`__ 로 끝났다면 새로운 줄은 추가되지 않는다.

__ `Dividing test data to several rows`_
__ `Newlines in test data`_
__ `Escaping`_

.. sourcecode:: robotframework

   *** Test Cases ***
   Simple
       [Documentation]    Simple documentation
       No Operation

   Formatting
       [Documentation]    *This is bold*, _this is italic_  and here is a link: http://robotframework.org
       No Operation

   Variables
       [Documentation]    Executed at ${HOST} by ${USER}
       No Operation

   Splitting
       [Documentation]    This documentation    is split    into multiple columns
       No Operation

   Many lines
       [Documentation]    Here we have
       ...                an automatic newline
       No Operation

..
   It is important that test cases have clear and descriptive names, and
   in that case they normally do not need any documentation. If the logic
   of the test case needs documenting, it is often a sign that keywords
   in the test case need better names and they are to be enhanced,
   instead of adding extra documentation. Finally, metadata, such as the
   environment and user information in the last example above, is often
   better specified using tags_.


테스트 케이스가 명확하고 서술적인 이름을 가지는 것은 중요하다. 이런
경우에는 추가적인 문서를 필요로 하지 않는다. 만일 테스트 케이스의
로직이 문서를 필요로 한다면, 테스트 케이스의 키워드에 문서를 추가하는
것 보다, 더 나은 이름을 지어줄 필요가 있다. 마지막으로 위의 마지막
예제에서 환경(`${HOST}`)과 사용자 정보(`${USER}`)로 메타데이타를
사용했는데, 이러한 것이 종종 `태그 <tags_>`__ 를 사용하는 것보다 더
낫다.

.. _test case tags:

Tagging test cases
------------------

..
   Using tags in Robot Framework is a simple, yet powerful mechanism for
   classifying test cases. Tags are free text and they can be used at
   least for the following purposes:

Robot Framework에서 태그는 test case를 분류할 때 단순하지만 강력한
메카니즘을 가진다. 태그는 어떤 문자이든 상관없고, 아래와 같은 목적으로
사용한다:

..
   - Tags are shown in test reports_, logs_ and, of course, in the test
     data, so they provide metadata to test cases.
   - Statistics__ about test cases (total, passed, failed  are
     automatically collected based on tags).
   - With tags, you can `include or exclude`__ test cases to be executed.
   - With tags, you can specify which test cases are considered `critical`_.

- 태그는 reports_, logs_, 테스트 데이터에서 볼 수 있고, 테스트 케이스에
  메타 데이터를 제공한다.
- 테스트 케이스에 관련된 `통계`__ (total, passed, failed 는 태그에
  기반 해서 자동적으로 수집된다)
- 태그를 사용해서, 수행할 테스트 케이스를 `포함하거나 제외`__ 할 수
  있다.
- 태그를 이용해서, `critical`_ 로 고려되어야 한다면 명시 할 수 있다.

__ `Configuring statistics`_
__ `By tag names`_

..
   In this section it is only explained how to set tags for test
   cases, and different ways to do it are listed below. These
   approaches can naturally be used together.

이번 섹션에서는 테스트 케이스에 어떻게 태그를 설정하는지 설명하고,
다른 방법을 아래에 나열할 것이다. 이런 방법은 함께 사용할 수도 있다.

..
   `Force Tags`:setting: in the Setting table
      All test cases in a test case file with this setting always get
      specified tags. If it is used in the `test suite initialization file`,
      all test cases in sub test suites get these tags.

`Force Tags`:setting: 설정 표에서
   Test case 파일의 모든 test cases는 이 설정에 명기된 태그를 가진다. 만일
   `test suite initialization file` 에서 사용된다면, 하위 test suites에 
   포함된 모든 test case는 이 태그를 가진다.

..
   `Default Tags`:setting: in the Setting table
      Test cases that do not have a :setting:`[Tags]` setting of their own
      get these tags. Default tags are not supported in test suite initialization
      files.

`Default Tags`:setting: 설정 표에서
   :setting:`[Tags]` 설정에 태그를 갖지 않는 Test cases는 모두 이
   태그를 가진다. Default tags는 `test suite initialization file` 을
   지원하지 않는다.

..
   `[Tags]`:setting: in the Test Case table
      A test case always gets these tags. Additionally, it does not get the
      possible tags specified with :setting:`Default Tags`, so it is possible
      to override the :setting:`Default Tags` by using empty value. It is
      also possible to use value `NONE` to override default tags.

`[Tags]`:setting: 테스트 케이스 표에서
   Test case는 항상 태그를 갖는다. 추가적으로 :setting:`Default Tags`
   에 명기된 태그를 가지지 않으려면 :setting:`Default Tags` 에 빈
   값으로 재정의해야 한다. 이것은 또한 default tags를 재정의 하기 위해
   `NONEE` 값을 사용할 수 있다.

..
   `--settag`:option: command line option
      All executed test cases get tags set with this option in addition
      to tags they got elsewhere.

`--settag`:option: 명령행 옵션
   수행된 모든 test cases  이 옵션에 의해 설정된 태그를 갖는다.

..
   `Set Tags`:name:, `Remove Tags`:name:, `Fail`:name: and `Pass Execution`:name: keywords
	 These BuiltIn_ keywords can be used to manipulate tags dynamically
	 during the test execution.

`Set Tags`:name:, `Remove Tags`:name:, `Fail`:name: 과 `Pass   Execution`:name: 키워드
   BuiltIn_ 키워드는 테스트 수행중 동적으로 태그를 조종하기 위해
   사용될 수 있다.

..
   Tags are free text, but they are normalized so that they are converted
   to lowercase and all spaces are removed. If a test case gets the same tag
   several times, other occurrences than the first one are removed. Tags
   can be created using variables, assuming that those variables exist.

태그는 아무 문자나 사용할 수 있다. 태그는 소문자로 변환하고 공백을
제거하여 일반화한다. 만약 test case가 같은 태그를 여러번 갖는다면,
첫번째 것을 제외한 나머지는 제거된다. 변수가 존재한다는 가정하에,
변수를 이용해서 tag를 생성할 수 있다.

.. sourcecode:: robotframework

   *** Settings ***
   Force Tags      req-42
   Default Tags    owner-john    smoke

   *** Variables ***
   ${HOST}         10.0.1.42

   *** Test Cases ***
   No own tags
       [Documentation]    This test has tags owner-john, smoke and req-42.
       No Operation

   With own tags
       [Documentation]    This test has tags not_ready, owner-mrx and req-42.
       [Tags]    owner-mrx    not_ready
       No Operation

   Own tags with variables
       [Documentation]    This test has tags host-10.0.1.42 and req-42.
       [Tags]    host-${HOST}
       No Operation

   Empty own tags
       [Documentation]    This test has only tag req-42.
       [Tags]
       No Operation

   Set Tags and Remove Tags Keywords
       [Documentation]    This test has tags mytag and owner-john.
       Set Tags    mytag
       Remove Tags    smoke    req-*

Reserved tags
~~~~~~~~~~~~~

..
   Users are generally free to use whatever tags that work in their context.
   There are, however, certain tags that have a predefined meaning for Robot
   Framework itself, and using them for other purposes can have unexpected
   results. All special tags Robot Framework has and will have in the future
   have a `robot-` prefix. To avoid problems, users should thus not use any
   tag with a `robot-` prefix unless actually activating the special functionality.

사용자는 자신의 문맥 안에서 작동 할 어떤 태그든지 자유롭게 사용할 수
있다. 그러나 Robot Framework 자체에서 미리 정의한 의미가 있는 태그를
의도하지 않은대로 사용하는 것은 예상치 못한 결과를 발생시킬 수 있다.
Robot Framework가 지원하는 모든 특별한 태그는 앞으로 `robot-` 접두어를
가진다. 문제를 예방하기 위해서, 사용자는 특별한 기능이 활성화 하지
않는한 `robot-` 접두어를 갖는 태그를 사용하지 않는 것이 좋다.

..
   At the time of writing, the only special tag is `robot-exit` that is
   automatically added to tests when `stopping test execution gracefully`_.
   More usages are likely to be added in the future, though.

현재 유일한 특수 태그는 `robot-exit` 로 `테스트 수행을 우아하게 중지
<stopping test execution gracefully_>`__ 할 때, 자동으로 테스트에
추가된다. 앞으로 더 많은 사용법이 추가될 것이다.

Test setup and teardown
-----------------------

..
   Robot Framework has similar test setup and teardown functionality as many
   other test automation frameworks. In short, a test setup is something
   that is executed before a test case, and a test teardown is executed
   after a test case. In Robot Framework setups and teardowns are just
   normal keywords with possible arguments.

Robot Framework는 다른 테스트 자동화 프레임워크와 유사하게 test
setup과 teardown 기능을 가진다. 요컨데 test setup은 test case 전에
수행되는 것이고, test teardown은 test case 후에 수행되는 것이다. Robot
Framework에서 setup과 teardown은 단지 전달인자 가지는 일반 키워드이다.

..
   Setup and teardown are always a single keyword. If they need to take care
   of multiple separate tasks, it is possible to create higher-level `user
   keywords`_ for that purpose. An alternative solution is executing multiple
   keywords using the BuiltIn_ keyword :name:`Run Keywords`.

setup과 teardown은 항상 단일 키워드이다. 만약 복수의 독립된 동작
수행을 필요로한다면, 원하는대로 higher-level `user keywords`_ 을
생성할 수 있다. 다른 방법은 BuiltIn_ 키워드 :name:`Run Keywords` 를
이용하여 복수의 키워드를 수행하는 것이다.

..
   The test teardown is special in two ways. First of all, it is executed also
   when a test case fails, so it can be used for clean-up activities that must be
   done regardless of the test case status. In addition, all the keywords in the
   teardown are also executed even if one of them fails. This `continue on failure`_
   functionality can be used also with normal keywords, but inside teardowns it is
   on by default.

test teardown은 두가지 면에서 특별하다. 첫번째로, test case가 실패했을
때에 수행된다. 그래서 test case의 상태 [#]_ 에 관계 없이 clean-up
동작을 수행한다. 게다가 teardown의 모든 키워드는 그 중 하나의 키워드가
실패할 때 조차도 모든 키워드를 수행한다. `실패해도 계속 수행하는
<continue on failure_>`__ 기능은 일반 키워드와 사용할 수 있다. 하지만
teardown에서는 기본적으로 그렇게 동작한다.

.. [#] PASS, FAIL
..
   The easiest way to specify a setup or a teardown for test cases in a
   test case file is using the :setting:`Test Setup` and :setting:`Test
   Teardown` settings in the Setting table. Individual test cases can
   also have their own setup or teardown. They are defined with the
   :setting:`[Setup]` or :setting:`[Teardown]` settings in the test case
   table and they override possible :setting:`Test Setup` and
   :setting:`Test Teardown` settings. Having no keyword after a
   :setting:`[Setup]` or :setting:`[Teardown]` setting means having no
   setup or teardown. It is also possible to use value `NONE` to indicate that
   a test has no setup/teardown.

Test case 파일에서 test case의 setup과 teardown을 명백히 하는 가장
쉬운 방법은 설정 표에서 :setting:`Test Setup` 과 :setting:`Test
Teardown` 설정을 사용하는 것이다. 각각의 test case는 setup과
teardown을 갖는다. Setup과 teardown은 Test case 표의
:setting:`[Setup]` 과 :setting:`[Teardown]` 설정에 정의하고,
:setting:`Test Setup` 과 :setting:`Test Teardown` 설정에서 재정의 할
수 있다. :setting:`[Setup]` 과 :setting:`[Teardown]` 설정 뒤에 아무런
키워드를 넣지 않는 것은 setup과 teardown이 필요 없다는 뜻이다.
테스트에 setup과 teardown이 없다는 것을 `NONE` 값으로 표현할 수도
있다.

.. sourcecode:: robotframework

   *** Settings ***
   Test Setup       Open Application    App A
   Test Teardown    Close Application

   *** Test Cases ***
   Default values
       [Documentation]    Setup and teardown from setting table
       Do Something

   Overridden setup
       [Documentation]    Own setup, teardown from setting table
       [Setup]    Open Application    App B
       Do Something

   No teardown
       [Documentation]    Default setup, no teardown at all
       Do Something
       [Teardown]

   No teardown 2
       [Documentation]    Setup and teardown can be disabled also with special value NONE
       Do Something
       [Teardown]    NONE

   Using variables
       [Documentation]    Setup and teardown specified using variables
       [Setup]    ${SETUP}
       Do Something
       [Teardown]    ${TEARDOWN}

..
   The name of the keyword to be executed as a setup or a teardown can be a
   variable. This facilitates having different setups or teardowns in
   different environments by giving the keyword name as a variable from
   the command line.

setup 혹은 teardown으로 수행될 키워드의 이름은 변수가 될 수 있다.
이것은 다른 환경에서, 명령행으로부터 키워드 이름을 변수로
전달하는 방법을 통해 다른 setup과 teardown을 가질 수 있도록 한다.

..
   .. note:: `Test suites can have a setup and teardown of their
	      own`__. A suite setup is executed before any test cases or sub test
	      suites in that test suite, and similarly a suite teardown is
	      executed after them.

.. note:: `Test suites은 자신만의 setup과 teardown을 가질 수 있다`__.
           suite setup은 다른 test cases나 sub test suites가 수행되기
           전에 수행되고, 유사하게 suite teardown은 그 후에 수행된다.

__  `Suite setup and teardown`_

Test templates
--------------

..
   Test templates convert normal `keyword-driven`_ test cases into
   `data-driven`_ tests. Whereas the body of a keyword-driven test case
   is constructed from keywords and their possible arguments, test cases with
   template contain only the arguments for the template keyword.
   Instead of repeating the same keyword multiple times per test and/or with all
   tests in a file, it is possible to use it only per test or just once per file.

테스트 템플릿은 일반 `keyword-driven`_ 테스트 케이스를 `data-driven`_
테스트로 바꿔준다. 템플릿의 테스트 케이스는 템플릿 키워드의 전달인자만
포함된 반면에 keyword-drive 테스트 케이스의 본문은 키워드와 전달인자로
구성되어있다. 한 테스트 안에서 혹은 파일의 모든 테스트에서 같은
키워드를 여러번 반복하는 것 대신에, 테스트나 파일별로 한번만 사용할 수
있다.

..
   Template keywords can accept both normal positional and named arguments, as
   well as arguments embedded to the keyword name. Unlike with other settings,
   it is not possible to define a template using a variable.

템플릿 키워드는 전달인자가 키워드 이름에 전달되는 것 처럼, 일반 위치
전달인자나 명명된 전달인자를 받을 수 있다. 다른 설정들과는 다르게
변수를 사용하여 템플릿을 정의할 수 없다.

Basic usage
~~~~~~~~~~~

..
   How a keyword accepting normal positional arguments can be used as a template
   is illustrated by the following example test cases. These two tests are
   functionally fully identical.

템플릿에서 어떻게 키워드가 일반 위치 전달인자를 받을 수 있는가는
아래의 예제에서 보여준다. 이 두 테스트들은 기능적으로 완벽히 동일하다.

.. sourcecode:: robotframework

   *** Test Cases **
   Normal test case
       Example keyword    first argument    second argument

   Templated test case
       [Template]    Example keyword
       first argument    second argument

..
   As the example illustrates, it is possible to specify the
   template for an individual test case using the :setting:`[Template]`
   setting. An alternative approach is using the :setting:`Test Template`
   setting in the Setting table, in which case the template is applied
   for all test cases in that test case file. The :setting:`[Template]`
   setting overrides the possible template set in the Setting table, and
   an empty value for :setting:`[Template]` means that the test has no
   template even when :setting:`Test Template` is used. It is also possible
   to use value `NONE` to indicate that a test has no template.

예제에서 다루었듯이, :setting:`[Template]` 설정을 사용하는 각각의 test
case에 템플릿을 명시할 수 있다. 다른 방법은 설정 표에서 :setting:`Test
Template` 를 사용하는 것이다. 이 경우 test case 템플릿은 test case
파일 안의 모든 test cases에서 적용될 것이다. :setting:`[Template]`
설정은 설정 표에서 설정한 것을 재정의 한다. :setting:`[Template]` 에서
빈 값인 경우, :setting:`Test Template` 가 사용되었을 지라도 테스트가
템플릿이 없음을 의미한다. 템플릿이 없는 경우에는 `NONE` 값을 사용할
수도 있다.

..
   If a templated test case has multiple data rows in its body, the template
   is applied for all the rows one by one. This
   means that the same keyword is executed multiple times, once with data
   on each row. Templated tests are also special so that all the rounds
   are executed even if one or more of them fails. It is possible to use this
   kind of `continue on failure`_ mode with normal tests too, but with
   the templated tests the mode is on automatically.

만일 템플릿이 적용된 테스트 케이스가 본문에 여러줄의 데이터를 가지고
있다면, 템플릿은 모든 줄마다 적용이 될 것이다. 이것은 같은 키워드가 매
줄마다, 여러번 동작하는 것을 의미한다. 템플릿이 적용된 테스트 케이스는
특별한데, 하나 이상 실패했더라도 매번 수행된다. 일반 테스트와
마찬가지로 `실패해도 계속 수행 <continue on failure_>`__ 모드를 사용할
수 있다. 하지만 템플릿이 적용된 테스트는 이 모드가 자동 설정 된다.

.. sourcecode:: robotframework

   *** Settings ***
   Test Template    Example keyword

   *** Test Cases ***
   Templated test case
       first round 1     first round 2
       second round 1    second round 2
       third round 1     third round 2

..
   Using arguments with `default values`_ or `varargs`_, as well as using
   `named arguments`_ and `free keyword arguments`_, work with templates
   exactly like they work otherwise. Using variables_ in arguments is also
   supported normally.

`기본 값 <default values_>`__ 혹은 `varargs`_ 의 전달인자를 사용하는
것과 `명명된 전달인자 <named arguments_>`__ 와 `키워드 전달인자 <free
keyword arguments_>`__ 을 사용하는 것은 템플릿과 잘 동작한다.
전달인자에 variables_ 를 사용하는 것 또한 지원된다.

Templates with embedded arguments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Starting from Robot Framework 2.8.2, templates support a variation of
   the `embedded argument syntax`_. With templates this syntax works so
   that if the template keyword has variables in its name, they are considered
   placeholders for arguments and replaced with the actual arguments
   used with the template. The resulting keyword is then used without positional
   arguments. This is best illustrated with an example:

Robot Framework 2.8.2 부터, 템플릿이 `임베디드 전달인자 문법 <embedded
argument syntax_>`__ 의 변형을 지원한다. 템플릿에서도 이 문법은
동작한다. 만약 템플릿 키워드가 이름안에 변수를 가진다(임베디드
전달인자)면, 변수는 전달인자를 위한 표시자로 여겨지고 템플릿에서
사용하는 실제 전달인자로 대체된다. 그 결과 키워드는 위치 전달인자 없이
사용된다. 아래 예제에서 이를 잘 보여준다.:

.. sourcecode:: robotframework

   *** Test Cases ***
   Normal test case with embedded arguments
       The result of 1 + 1 should be 2
       The result of 1 + 2 should be 3

   Template with embedded arguments
       [Template]    The result of ${calculation} should be ${expected}
       1 + 1    2
       1 + 2    3

   *** Keywords ***
   The result of ${calculation} should be ${expected}
       ${result} =    Calculate    ${calculation}
       Should Be Equal    ${result}     ${expected}

..
   When embedded arguments are used with templates, the number of arguments in
   the template keyword name must match the number of arguments it is used with.
   The argument names do not need to match the arguments of the original keyword,
   though, and it is also possible to use different arguments altogether:

임베디드 전달인자가 템플릿에서 사용될 때, 템플릿 키워드 이름의
전달인자의 수는 이것이 사용되는 전달인자의 수와 일치해야 한다.
전달인자 이름은 원래의 키워드 전달인자와 일치할 필요는 없다. 그리고
전적으로 다른 전달인자를 사용할 수도 있다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Different argument names
       [Template]    The result of ${foo} should be ${bar}
       1 + 1    2
       1 + 2    3

   Only some arguments
       [Template]    The result of ${calculation} should be 3
       1 + 2
       4 - 1

   New arguments
       [Template]    The ${meaning} of ${life} should be 42
       result    21 * 2

..
   The main benefit of using embedded arguments with templates is that
   argument names are specified explicitly. When using normal arguments,
   the same effect can be achieved by naming the columns that contain
   arguments. This is illustrated by the `data-driven style`_ example in
   the next section.

템플릿에서 임베디드 전달인자를 사용하는 주요 장점은 전달인자 이름이
명백히 구분된다는 것이다. 일반 전달인자를 사용할 때, 전달인자를
포함하여 열에 이름을 붙여줌으로써 같은 효과를 얻을 수 있다. 다음
섹션에 있는 `data-driven style`_ 예제에서 설명한다.

Templates with for loops
~~~~~~~~~~~~~~~~~~~~~~~~

..
   If templates are used with `for loops`_, the template is applied for
   all the steps inside the loop. The continue on failure mode is in use
   also in this case, which means that all the steps are executed with
   all the looped elements even if there are failures.

만일 템플릿에서 `for loops`_ 를 사용한다면, 템플릿은 루프 내의 모든
단계에 적용이 된다. 실패 해도 계속 수행하는 모드는 이번 케이스에서도
마찬가지이다. 즉, 모든 반복되는 요소중에 비록 실패가 존재하더라도 모든
단계는 수행된다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Template and for
       [Template]    Example keyword
       :FOR    ${item}    IN    @{ITEMS}
       \    ${item}    2nd arg
       :FOR    ${index}    IN RANGE    42
       \    1st arg    ${index}

Different test case styles
--------------------------

..
   There are several different ways in which test cases may be written. Test
   cases that describe some kind of *workflow* may be written either in
   keyword-driven or behavior-driven style. Data-driven style can be used to test
   the same workflow with varying input data.

Test cases가 작성되는데에는 여러가지 방법들이 있다. Test cases은
keyword-driven 혹은 behavior-driven 스타일의 *workflow* 를 가질 수
있다. Data-driven 스타일은 다양한 입력 데이타를 갖는 동일 워크플로우를
테스트하는데 사용할 수 있다.

Keyword-driven style
~~~~~~~~~~~~~~~~~~~~

..
   Workflow tests, such as the :name:`Valid Login` test described
   earlier_, are constructed from several keywords and their possible
   arguments. Their normal structure is that first the system is taken
   into the initial state (:name:`Open Login Page` in the :name:`Valid
   Login` example), then something is done to the system (:name:`Input
   Name`, :name:`Input Password`, :name:`Submit Credentials`), and
   finally it is verified that the system behaved as expected
   (:name:`Welcome Page Should Be Open`).

`앞서 <earlier_>`__ 설명한 :name:`Valid Login` 테스트와 같은
워크플로우 테스트는 여러 키워드와 가능한 전달인자로 구성되어 있다.
일반적인 구조에서, 첫번째 시스템은 초기 상태를 확인한다. (:name:`Valid
Login` 예제에서 :name:`Open Login Page`) 그리고 시스템에 무엇인가를
수행한다. (:name:`Input Name`, :name:`Input Password`, :name:`Submit
Credentials`) 마지막으로 시스템에서 예상대로 동작했는지 검증한다.
(:name:`Welcome Page Should Be Open`)

.. _earlier: example-tests_

Data-driven style
~~~~~~~~~~~~~~~~~

..
   Another style to write test cases is the *data-driven* approach where
   test cases use only one higher-level keyword, normally created as a
   `user keyword`_, that hides the actual test workflow. These tests are
   very useful when there is a need to test the same scenario with
   different input and/or output data. It would be possible to repeat the
   same keyword with every test, but the `test template`_ functionality
   allows specifying the keyword to use only once.

테스트 케이스를 작성하는 다른 스타일은 *data-driven* 접근 방식이다.
여기서 테스트 케이스는 오직 하나의 higher-level 키워드만 사용한다.
이것은 보통 `user keyword`_ 로 생성되고 실제 워크플로우는 숨긴다. 같은
시나리오를 다른 입력과 출력 데이터로 시험하는 테스트는 매우 유용하다.
이 경우 매 테스트마다 같은 키워드를 반복해야한다. 하지만 `test
template`_ 기능을 사용하면 키워드를 한번만 사용하면 된다.

.. sourcecode:: robotframework

   *** Settings ***
   Test Template    Login with invalid credentials should fail

   *** Test Cases ***                USERNAME         PASSWORD
   Invalid User Name                 invalid          ${VALID PASSWORD}
   Invalid Password                  ${VALID USER}    invalid
   Invalid User Name and Password    invalid          invalid
   Empty User Name                   ${EMPTY}         ${VALID PASSWORD}
   Empty Password                    ${VALID USER}    ${EMPTY}
   Empty User Name and Password      ${EMPTY}         ${EMPTY}

..
   .. tip:: Naming columns like in the example above makes tests easier to
	    understand. This is possible because on the header row other
	    cells except the first one `are ignored`__.

.. tip:: 위의 예처럼 이름을 적는 열(USERNAME, PASSWORD)은 테스트를
         이해하기 쉽게 도와준다. 이것은 헤더 열에서 첫번째 셀을 제외한
         다른 셀을 `무시하기`__ 때문에 가능하다.

..
   The above example has six separate tests, one for each invalid
   user/password combination, and the example below illustrates how to
   have only one test with all the combinations. When using `test
   templates`_, all the rounds in a test are executed even if there are
   failures, so there is no real functional difference between these two
   styles. In the above example separate combinations are named so it is
   easier to see what they test, but having potentially large number of
   these tests may mess-up statistics. Which style to use depends on the
   context and personal preferences.

위의 예제는 별도의 여섯가지 테스트를 가진다. 유효하지 않은
사용자/패스워드 조합을 하나씩을 수행한다. 아래의 예제에서는 모든
조합을 단하나의 테스트에서 어떻게 수행할 수 있는지를 보여준다. `test
templates`_ 을 사용할 때, 실패한 것이 있을지라도 모든 회차를 수행한다.
그래서 두 스타일 사이에 기능적인 차이는 없다. 위의 예제에서, 분리된
조합은 각각 이름지어져서 무엇을 테스트하는지 보기 쉽게 만든다. 하지만
잠재적으로 많은 수 이런 테스트는 통계를 망칠 수도 있다. 어떤 스타일을
사용할지는 문맥과 개인적 선호도에 의존한다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Invalid Password
       [Template]    Login with invalid credentials should fail
       invalid          ${VALID PASSWORD}
       ${VALID USER}    invalid
       invalid          whatever
       ${EMPTY}         ${VALID PASSWORD}
       ${VALID USER}    ${EMPTY}
       ${EMPTY}         ${EMPTY}

__ `Ignored data`_

Behavior-driven style
~~~~~~~~~~~~~~~~~~~~~

..
   It is also possible to write test cases as requirements that also non-technical
   project stakeholders must understand. These *executable requirements* are a
   corner stone of a process commonly called `Acceptance Test Driven Development`__
   (ATDD) or `Specification by Example`__.

요구사항대로 test case를 작성하려면 비기술적인 프로젝트 이해관계자들을
이해해야만 한다. 이런 *실행가능한 요구사항(executable requirements)*
은 `Acceptance Test Driven Development`__ (ATDD) 혹은 `Specification
by Example`__ 라고 불리는 프로세스의 주춧돌이 될 것이다.

..
   One way to write these requirements/tests is *Given-When-Then* style
   popularized by `Behavior Driven Development`__ (BDD). When writing test cases in
   this style, the initial state is usually expressed with a keyword starting with
   word :name:`Given`, the actions are described with keyword starting with
   :name:`When` and the expectations with a keyword starting with :name:`Then`.
   Keyword starting with :name:`And` or :name:`But` may be used if a step has more
   than one action.

이런 요구사항/테스트를 작성하는 방법은 `Behavior Driven Development`__
(BDD)로 알려진 *Given-When-Then* 스타일을 사용하는 것이다. 이 스타일로
test case를 작성할 때, 초기 상태는 일반적으로 :name:`Given` 로
시작하는 이름을 가진 키워드로 기술하고, 동작은 :name:`When` 로
시작하는 이름을 가진 키워드로 기술한다. 그리고 예측 결과에 대한 설명은
:name:`Then` 로 시작하는 키워드로 기술한다. :name:`And` 혹은
:name:`But` 로 시작하는 키워드는 하나 이상의 동작이 필요할 때
사용한다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Valid Login
       Given login page is open
       When valid username and password are inserted
       and credentials are submitted
       Then welcome page should be open

__ http://testobsessed.com/2008/12/08/acceptance-test-driven-development-atdd-an-overview
__ http://en.wikipedia.org/wiki/Specification_by_example
__ http://en.wikipedia.org/wiki/Behavior_Driven_Development

Ignoring :name:`Given/When/Then/And/But` prefixes
'''''''''''''''''''''''''''''''''''''''''''''''''

..
   Prefixes :name:`Given`, :name:`When`, :name:`Then`, :name:`And` and :name:`But`
   are dropped when matching keywords are searched, if no match with the full name
   is found. This works for both user keywords and library keywords. For example,
   :name:`Given login page is open` in the above example can be implemented as
   user keyword either with or without the word :name:`Given`. Ignoring prefixes
   also allows using the same keyword with different prefixes. For example
   :name:`Welcome page should be open` could also used as :name:`And welcome page
   should be open`.

접두어 :name:`Given`, :name:`When`, :name:`Then`, :name:`And`,
:name:`But` 은 키워드 검색을 할 때 만약 전체 이름으로 일치하는 것이
없다면 버린다. 이것은 user keyword와 라이브러리 키워드에 모두
적용된다. 예를들어 위의 예제에서 :name:`Given login page is open` 는
:name:`Given` 단어와 함께 혹은 제외하고 user keyword로 사용된다.
무시되는 접두어는 도일 키워드에 다른 접두어 사용을 허용한다. 예를 들어
:name:`Welcome page should be open`는 또한 :name:`And welcome page
should be open` 처럼 사용할 수 있다.

..
   .. note:: Ignoring :name:`But` prefix is new in Robot Framework 2.8.7.

.. note:: Robot Framework 2.8.7 부터 :name:`But` 접두어는 무시된다.

Embedding data to keywords
''''''''''''''''''''''''''

..
   When writing concrete examples it is useful to be able pass actual data to
   keyword implementations. User keywords support this by allowing `embedding
   arguments into keyword name`_.

구체적인 예를 작성할 때 실제 데이터를 키워드로 전달할 수 있는 것은
매우 유용하다. User keywords는 `키워드 이름안의 임베딩 전달인자
<embedding arguments into keyword name_>`__ 를 통해 이를 지원한다.
