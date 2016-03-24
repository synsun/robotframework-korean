Advanced features
=================

.. contents::
   :depth: 2
   :local:

Handling keywords with same names
---------------------------------

..
   Keywords that are used with Robot Framework are either `library
   keywords`_ or `user keywords`_. The former come from `standard
   libraries`_ or `external libraries`_, and the latter are either
   created in the same file where they are used or then imported from
   `resource files`_. When many keywords are in use, it is quite common
   that some of them have the same name, and this section describes how to
   handle possible conflicts in these situations.

Robot Framework에서 사용되는 키워드는 `library keywords`_ 이거나 `user
keywords`_ 이다. 전자는 `standard libraries`_ 또는 `external libraries`_
로 제공되고, 후자는 키워드를 사용하는 동일 파일에 만들거나 `resource files`_
로 부터 읽어온다. 많은 키워드를 사용할 때 동일 이름을 가지는 경우는 아주 흔하다.
그래서 이번 섹션에서는 이러한 상황에서 충돌을 다루는 방법을 기술한다.

Keyword scopes
~~~~~~~~~~~~~~

..
   When only a keyword name is used and there are several keywords with
   that name, Robot Framework attempts to determine which keyword has the
   highest priority based on its scope. The keyword's scope is determined
   on the basis of how the keyword in question is created:

단지 하나의 키워드 이름이 사용되어지고 그 이름과 함께 여러 키워드가 존재할때,
Robot Framework는 어떤 키워드가 해당 범위(scope)를 기준으로 가장 높은 우선순위를
가지는지 결정하려고 시도한다. 해당 키워드의 범위는 어떻게 키워드가 생성되었는지에 대한
물음으로 부터 결정되어진다.

..
   1. Created as a user keyword in the same file where it is used. These
      keywords have the highest priority and they are always used, even
      if there are other keywords with the same name elsewhere.

1. 키워드가 사용되어지는 파일안에 사용자 키워드 생성. 이런 키워드는
   가장 높은 우선순위를 가지며, 다른 곳에 동일한 이름을 가지는
   키워드가 존재하더라도 항상 먼저 사용되어진다.

..
   2. Created in a resource file and imported either directly or
      indirectly from another resource file. This is the second-highest
      priority.

2. 리소스 파일안에 생성하고 다른 리소스 파일로 부터 직접 또는
   간접적으로 읽혀진 경우. 이 경우 두번째 높은 우선순위를 가진다.


..
   3. Created in an external test library. These keywords are used, if
      there are no user keywords with the same name. However, if there is
      a keyword with the same name in the standard library, a warning is
      displayed.

3. 외부 테스트 라이브러리안에 생성. 만약 동일한 이름을 가지는 유저
   키워드가 없다면 이 키워드가 사용된다. 그러나 표준 라이브러리에 같은
   이름을 가지는 키워드가 존재한다면 경고가 출력된다.
   
..
   4. Created in a standard library. These keywords have the lowest
      priority.

4. 표준라이브러리안에서 생성. 이 키워드는 가장 낮은 우선순위를 가진다.

Specifying a keyword explicitly
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Scopes alone are not a sufficient solution, because there can be
   keywords with the same name in several libraries or resources, and
   thus, they provide a mechanism to use only the keyword of the
   highest priority. In such cases, it is possible to use *the full name
   of the keyword*, where the keyword name is prefixed with the name of
   the resource or library and a dot is a delimiter.

여러 라이브러리나 리소스안에 동일 이름을 가지는 키워드가 존재할 수
있기 때문에 범위 만으로는 충분한 해결책은 아니다. 그래서 가장 높은
우선순위를 가지는 유일한 키워드를 사용할 수 있도록 매카니즘을
제공한다. 이런 경우 리소스나 라이브러리의 이름을
접두어(.구분자사용)로 사용하여 *전체 이름의 키워드* 를 사용할 수 있다.

..
   :name:`LibraryName.Keyword Name`. For example, the keyword :name:`Run`
   from the OperatingSystem_ library could be used as
   :name:`OperatingSystem.Run`, even if there was another :name:`Run`
   keyword somewhere else. If the library is in a module or package, the
   full module or package name must be used (for example,
   :name:`com.company.Library.Some Keyword`). If a custom name is given
   to a library using the `WITH NAME syntax`_, the specified name must be
   used also in the full keyword name.

긴 형식의 라이브러리 키워드는 :name:`LibraryName.Keyword Name` 이다.
에를 들어 OperatingSystem_ 라이브러리의 키워드 :name:`Run` 은 비록 다른
곳에 :name:`Run` 키워드가 존재할지라도 :name:`OperatingSystem.Run` 으로
사용할 수 있다. 만약 라이브러리가 모듈이나 패키지 안에 있다면 전체
모듈이나 패키지 이름을 사용할 수 있다.(예.
:name:`com.company.Library.Some Keyword`). 만약 사용자 정의 이름이
`WITH NAME syntax`_ 를 사용하여 라이브러에 주어진다면, 그 명기된
이름이 전체 키워드 이름안에 사용되어야 한다.

..
   Resource files are specified in the full keyword name, similarly as
   library names. The name of the resource is derived from the basename
   of the resource file without the file extension. For example, the
   keyword :name:`Example` in a resource file :file:`myresources.html` can
   be used as :name:`myresources.Example`. Note that this syntax does not
   work, if several resource files have the same basename. In such
   cases, either the files or the keywords must be renamed. The full name
   of the keyword is case-, space- and underscore-insensitive, similarly
   as normal keyword names.

리소스 파일도 라이브러리 이름과 비슷하게 전체 키워드 이름을 기술한다.
리소스 이름은 확장자가 없는 파일이름이다. 예를 들어, resource 파일
:file:`myresources.html` 내의 :name:`Example` 키워드는
:name:`myresources.Example` 로 사용할 수 있다. 만약 여러 리소스 파일이
동일한 기본이름을 가진다면 이러한 문법이 적용되지 않는다. 그런 경우
파일 이나 키워드를 개명해야 한다. 키워드의 전체이름은 보통의 키워드
이름과 같이 대소문자, 공백, 언더스코어(_)를 구분하지 않는다.

Specifying explicit priority between libraries and resources
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   If there are multiple conflicts between keywords, specifying all the keywords
   in the long format can be quite a lot work. Using the long format also makes it
   impossible to create dynamic test cases or user keywords that work differently
   depending on which libraries or resources are available. A solution to both of
   these problems is specifying the keyword priorities explicitly using the keyword
   :name:`Set Library Search Order` from the BuiltIn_ library.

키워드 사이에 충돌이 많은 경우 긴 형식의 키워드를 기술하면 잘 동작할
수 있다. 또한 긴 형식을 사용하면 동적인 테스트 케이스 및 라이브러니나
리소스에 의존하여 다르게 동작하는 사용자 키워드를 사용하는 것을
불가능하게 만들 수 있다. 이러한 두가지 문제점의 해결책은 명시적으로
BuiltIn_ 라이브러리의 :name:`Set Library Search Order` 의 키워드를
사용하여 키워드 우선순위를 명기하는 것이다.

 ..
    .. note:: Although the keyword has the word *library* in its name, it works
	      also with resource files. As discussed above, keywords in resources
	      always have higher priority than keywords in libraries, though.

 .. note:: 비록 키워드가 이름내에 *library* 단어를 가지더라도, 그
           키워드는 리소스 파일과 동작한다. 위에서 논의한 바와 같이 리소스
           파일안의 키워드는 항상 라이브러리안의 키워드보다 더 높은 우선순위를
           가진다.
           

..
   The :name:`Set Library Search Order` accepts an ordered list or libraries and
   resources as arguments. When a keyword name in the test data matches multiple
   keywords, the first library or resource containing the keyword is selected and
   that keyword implementation used. If the keyword is not found from any of the
   specified libraries or resources, execution fails for conflict the same way as
   when the search order is not set.

:name:`Set Libaray Search Order` 는 정렬된 리스트, 라이브러리,
리소스를 전달인자로 받는다. 테스트 데이타안의 키워드 이름이 여러번
일치할 경우, 키워드를 포함하는 첫번째 라이브러리나 리소스를 선택하여
그 구현체를 사용한다. 만약 키워드가 기술된 라이브러리나 리소스로부터
찾을 수 없다면 검색순서가 설정되지 않은 것과 같은 방식으로 실패한다.

..
   For more information and examples, see the documentation of the keyword.

상세한 정보와 예제는 키워드 문서를 참고하라.

Timeouts
--------

..
   Keywords may be problematic in situations where they take
   exceptionally long to execute or just hang endlessly. Robot Framework
   allows you to set timeouts both for `test cases`_ and `user
   keywords`_, and if a test or keyword is not finished within the
   specified time, the keyword that is currently being executed is
   forcefully stopped. Stopping keywords in this manner may leave the
   library or system under test to an unstable state, and timeouts are
   recommended only when there is no safer option available. In general,
   libraries should be implemented so that keywords cannot hang or that
   they have their own timeout mechanism, if necessary.

키워드는 예외적으로 수행에 오랜 시간이 소요되거나 단지 끝없이
정지(hang)된 상태인 상황에서는 문제가 될 수 있다. 로봇프레임워크는
`test cases`_ 와 `user keywords`_ 에 시간제한(timeout)을 설정할 수
있다. 만약 테스트나 키워드가 정해진 시간안에 끝나지 않는다면, 현재
수행되고 있는 키워드는 강제적으로 정지된다. 이러한 방법으로 정지된
키워드는 라이브러리나 불안정한 상태의 SUT를 건너뛴다. 가능한 안전
옵션이 없는 경우에만 시간 제한을 권장한다. 일반적으로 라이브러리는
필요하다면 키워드가 정지(hang)되지 않거나 스스로 시간 제한 메커니즘을
가지도록 구현해야 한다.


Test case timeout
~~~~~~~~~~~~~~~~~

..
   The test case timeout can be set either by using the :setting:`Test
   Timeout` setting in the Setting table or the :setting:`[Timeout]`
   setting in the Test Case table. :setting:`Test Timeout` in the Setting
   table defines a default test timeout value for all the test cases in
   the test suite, whereas :setting:`[Timeout]` in the Test Case table
   applies a timeout to an individual test case and overrides the
   possible default value.

테스트 케이스 타임아웃은 설정표(Setting table)에서 :setting:`Test
Timeout` 을 사용하거나 테스트 케이스 표안에 :setting:`[Timeout]`
값으로 설정할 수 있다. 설정표에서 :setting:`Test Timeout` 은 테스트
스위트안의 모든 테스트 케이스에 대한 디폴트 타임아웃 값을 정의한다.
반면 테스트 케이스 표의 :setting:`[Timeout]` 은 개별적인 테스트
케이스의 타임아웃 값을 적용하고 기본 값을 덮어 쓴다.(override)

..
   Using an empty :setting:`[Timeout]` means that the test has no
   timeout even when :setting:`Test Timeout` is used. It is also possible
   to use value `NONE` for this purpose.

:setting:`Test Timeout` 이 사용되더라도, :setting:`[Timeout]` 에
`NONE` 값을 사용하여 테스트가 시간제한이 없게 할 수 있다.

..
   Regardless of where the test timeout is defined, the first cell after
   the setting name contains the duration of the timeout. The duration
   must be given in Robot Framework's `time format`_, that is,
   either directly in seconds or in a format like `1 minute
   30 seconds`. It must be noted that there is always some overhead by the
   framework, and timeouts shorter than one second are thus not
   recommended.

테스트 타임아웃이 정의됨에도 불구하고, 설정이름 후의 첫번째 셀은
타임아웃의 기간을 포함한다. 기간은 로봇프레임워크의 `time format`_
으로 주어진다. 직접적으로 초로 적든지 `1 minute 30 secondes` 같은
형태를 사용할 수있다. 항상 프레임워크에 의한 오버헤드가 있으므로, 1초
보다 짧은 타임아웃은 추천하지 않는다.

..
   The default error message displayed when a test timeout occurs is
   `Test timeout <time> exceeded`. It is also possible to use custom
   error messages, and these messages are written into the cells
   after the timeout duration. The message can be split into multiple
   cells, similarly as documentations. Both the timeout value and the
   error message may contain variables.

테스트 타임아웃이 발생할 때 출력되는 에러 메시지는 `Test timeout
<time> exceed` 이다. 사용자 에러 메시지 또한 사용가능하며 이런
메시지는 타임아웃 기간 뒤의 셀에 적혀진다. 메시지는
문서화(documentation)처럼 여러 셀에 나누어 적을 수 있다. 타임아웃 값과
에러 메시지는 모두 변수를 포함할 수 있다.

..
   If there is a timeout, the keyword running is stopped at the
   expiration of the timeout and the test case fails. However, keywords
   executed as `test teardown`_ are not interrupted if a test timeout
   occurs, because they are normally engaged in important clean-up
   activities. If necessary, it is possible to interrupt also these
   keywords with `user keyword timeouts`_.

타임아웃이 있는 경우, 키워드은 타임아웃이 만료되면 실행을 중지하고
테스트 케이스는 실패한다. 그러나 `test teardown`_ 은 일반적으로 중요한
클린업(clean-up)활동을 수행하기 때문에 테스트 타임아웃이 발생하더라도
방해받지 않는다.. 필요하다면 `user keyword timeouts`_ 을 사용하여
이러한 키워드를 방해할 수 있다.

.. sourcecode:: robotframework

   *** Settings ***
   Test Timeout    2 minutes

   *** Test Cases ***
   Default Timeout
       [Documentation]    Timeout from the Setting table is used
       Some Keyword    argument

   Override
       [Documentation]    Override default, use 10 seconds timeout
       [Timeout]    10
       Some Keyword    argument

   Custom Message
       [Documentation]    Override default and use custom message
       [Timeout]    1min 10s    This is my custom error
       Some Keyword    argument

   Variables
       [Documentation]    It is possible to use variables too
       [Timeout]    ${TIMEOUT}
       Some Keyword    argument

   No Timeout
       [Documentation]    Empty timeout means no timeout even when Test Timeout has been used
       [Timeout]
       Some Keyword    argument

   No Timeout 2
       [Documentation]    Disabling timeout with NONE works too and is more explicit.
       [Timeout]    NONE
       Some Keyword    argument

User keyword timeout
~~~~~~~~~~~~~~~~~~~~

..
   A timeout can be set for a user keyword using the :setting:`[Timeout]`
   setting in the Keyword table. The syntax for setting it, including how
   timeout values and possible custom messages are given, is
   identical to the syntax used with `test case timeouts`_. If no custom
   message is provided, the default error message `Keyword timeout
   <time> exceeded` is used if a timeout occurs.

사용자 키워드 타임아웃은 키워드 표에서 :setting:`[Timeout]` 값을
설정할 수 있다. 타임아웃값과 사용자 메시지를 포함하여 설정하는 문법은
`test case timeouts`_ 와 동일하다. 만약 사용자 메시지가 제공되지
않는다면, 타임아웃이 발생했을 때 기본 에러 메시지 `Keyword timeout
<time> exceeded` 가 사용된다.

.. sourcecode:: robotframework

   *** Keywords ***
   Timed Keyword
       [Documentation]    Set only the timeout value and not the custom message.
       [Timeout]    1 minute 42 seconds
       Do Something
       Do Something Else

   Timed-out Wrapper
       [Arguments]    @{args}
       [Documentation]    This keyword is a wrapper that adds a timeout to another keyword.
       [Timeout]    2 minutes    Original Keyword didn't finish in 2 minutes
       Original Keyword    @{args}

..
   A user keyword timeout is applicable during the execution of that user
   keyword. If the total time of the whole keyword is longer than the
   timeout value, the currently executed keyword is stopped. User keyword
   timeouts are applicable also during a test case teardown, whereas test
   timeouts are not.

사용자 키워드 타임아웃은 해당 사용자 키워드가 실행하는 동한
적용가능하다. 만약 전체 키워드의 총 시간이 타임아웃 값보다 긴 경우
현재 실행된 키워드는 중지된다. 유저 키워드 타임아웃은 테스트 케이스
teardown에도 적용가능하다. 반면에 테스트 타임아웃은 불가하다.

..
   If both the test case and some of its keywords (or several nested
   keywords) have a timeout, the active timeout is the one with the least
   time left.

만약 테스트 케이스와 테스트 케이스내 키워드 중 일부(또는 여러번 중첩된
키워드) 모두 타임아웃을 가진다면, 액티브 타임아웃은 가장 작게 남겨진
시간이다.

..
   .. warning:: Using timeouts might slow down test execution when using Python 2.5
		elsewhere than on Windows. Prior to Robot Framework 2.7 timeouts
		slowed down execution with all Python versions on all platforms.

.. warning:: 윈도우즈 이외의 다른 곳에서 Python 2.5를 사용할 경우
             타임아웃 사용은 테스트 실행 속도를 느리게 할지도 모른다.
             로봇프레임워크 2.7이전 버전의 타임아웃은 모든 플래폼의
             모든 버전의 파이썬에서 수행 시간을 느리게 한다.

.. _for loop:

For loops
---------

..
   Repeating same actions several times is quite a common need in test
   automation. With Robot Framework, test libraries can have any kind of
   loop constructs, and most of the time loops should be implemented in
   them. Robot Framework also has its own for loop syntax, which is
   useful, for example, when there is a need to repeat keywords from
   different libraries.

동일 액션을 여러번 반복하는 것은 테스트 자동화에서 아주 공통적으로
요구된다. 테스트 라이브러리는 다양한 루프 구조를 가질 수 있으며,
대부분의 타임 루프는 그것 안에서 구현되어져야 한다. 로봇프레임워크는
자신만의 루프 문법을 가지며, 다른 라이브러리로 부터 반복 키워드가
필요한 경우 유용하다.


..
   For loops can be used with both test cases and user keywords. Except for
   really simple cases, user keywords are better, because they hide the
   complexity introduced by for loops. The basic for loop syntax,
   `FOR item IN sequence`, is derived from Python, but similar
   syntax is possible also in shell scripts or Perl.

For 루프는 테스트 케이스와 사용자 키워드와 함께 사용할 수 있다. 매우
간단한 경우를 제외하고, 사용자 키워드를 사용하는 것이 FOR 루프에 의한
복잡성를 감추기 때문에 더 낫다. 기본적인 FOR 루프 문법 즉 `FOR item IN
sequence` 는 파이썬에서 파생했다. 하지만 또한 Shell Script와 Perl의
문법과도 비슷하다.

Normal for loop
~~~~~~~~~~~~~~~

..
   In a normal for loop, one variable is assigned from a list of values,
   one value per iteration. The syntax starts with `:FOR`, where
   colon is required to separate the syntax from normal keywords. The
   next cell contains the loop variable, the subsequent cell must have
   `IN`, and the final cells contain values over which to iterate.
   These values can contain variables_, including `list variables`_.

일반적인 FOR 루프에서, 루프 변수는 반복시 마다 리스트의 값에서 한 값이
할당된다. 문법은 `:FOR` 로 시작하며, 콜론(:)이 일반 키워드와 구분하기
위해 사용된다. 다음 셀은 루프 변수를 포함한다. 이어지는 셀은 반드시
`IN` 을 가져야 한다. 그리고 마지막 셀들은 반복가능한 값들을 포함한다.
이러한 값들은 `list variables`_ 를 포함한 variables_ 를 포함한다.


..
   The keywords used in the for loop are on the following rows and they must
   be indented one cell to the right. When using the `plain text format`_,
   the indented cells must be `escaped with a backslash`__, but with other
   data formats the cells can be just left empty. The for loop ends
   when the indentation returns back to normal or the table ends.

..
   __ `dividing test data to several rows`_

FOR 루프에서 사용된 키워드는 다음 행에 있어야 하고 반드시 오른쪽으로
한 셀을 들여써야 한다. `plain text format`_ 을 사용할 때 들여쓴 셀은
`escaped with a backslash`__ 되어야 한다. 하지만 다른 데이타
형식에서는 해당 셀은 공백이다. 들여쓰기가 정상으로 바뀌거나 표가
끝날때 FOR 루프도 끝난다.

__ `dividing test data to several rows`_

.. sourcecode:: robotframework

   *** Test Cases ***
   Example 1
       :FOR    ${animal}    IN    cat    dog
       \    Log    ${animal}
       \    Log    2nd keyword
       Log    Outside loop

   Example 2
       :FOR    ${var}    IN    one    two
       ...     ${3}    four    ${last}
       \    Log    ${var}

..
   The for loop in :name:`Example 1` above is executed twice, so that first
   the loop variable `${animal}` has the value `cat` and then
   `dog`. The loop consists of two :name:`Log` keywords. In the
   second example, loop values are `split into two rows`__ and the
   loop is run altogether five times.

..
   __ `escaping`_

위의 :name:`Example 1` 에서 FOR 루프는 두번 수행된다. 루프변수
`${animal}` 은 첫번째 `cat` 값을 가지고 두번째 `dog` 를 가진다. 루프는
두개의 :name:`Log` 키워드로 구성된다. 두번째 예제에서 루프 변수는 두
행으로 나눠진다.(`split into two rows`__ ) 그리고 루프는 다섯번
수행된다.

__ `escaping`_


..
   It is often convenient to use for loops with `list variables`_. This is
   illustrated by the example below, where `@{ELEMENTS}` contains
   an arbitrarily long list of elements and keyword :name:`Start Element` is
   used with all of them one by one.

`list variables`_ 를 사용하여 FOR 루프를 사용하면 편리하다. 아래
예제에서 `@{ELEMENTS}` 는 임의의 긴 리스트 요소를 가진다. :name:`Start
Element` 키워드는 차례대로 그것을 사용한다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Example
       :FOR    ${element}    IN    @{ELEMENTS}
       \    Start Element  ${element}


Nested for loops
~~~~~~~~~~~~~~~~

..
   Having nested for loops is not supported directly, but it is possible to use
   a user keyword inside a for loop and have another for loop there.

중첩된 FOR 루프는 직접적으로 지원되지 않는다. 하지만 이것은 내부에 FOR
루프를 가지는 사용자 키워드를 FOR 루프에서 사용함으로써 가능한다.

.. sourcecode:: robotframework

   *** Keywords ***
   Handle Table
       [Arguments]    @{table}
       :FOR    ${row}    IN    @{table}
       \    Handle Row    @{row}

   Handle Row
       [Arguments]    @{row}
       :FOR    ${cell}    IN    @{row}
       \    Handle Cell    ${cell}


Using several loop variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   It is also possible to use several loop variables. The syntax is the
   same as with the normal for loop, but all loop variables are listed in
   the cells between `:FOR` and `IN`. There can be any number of loop
   variables, but the number of values must be evenly dividable by the number of
   variables.

여러 루프 변수를 사용할 수도 있다. 구문은 정상 루프와 동일하다. 모든
루프 변수는 `:FOR` 와 `IN` 사이의 셀에 나열해야 한다. 임의 갯수의 루프
변수가 존재할 수 있지만 값의 개수는 변수의 수에 의해 균등하게 분할
가능해야 한다.

..
   If there are lot of values to iterate, it is often convenient to organize
   them below the loop variables, as in the first loop of the example below:

반복하는 값이 많으면 아래의 예에서와 같이 루프 변수 아래에 값들을 적으면 편리하다:

.. sourcecode:: robotframework

   *** Test Cases ***
   Three loop variables
       :FOR    ${index}    ${english}    ${finnish}    IN
       ...     1           cat           kissa
       ...     2           dog           koira
       ...     3           horse         hevonen
       \    Add to dictionary    ${english}    ${finnish}    ${index}
       :FOR    ${name}    ${id}    IN    @{EMPLOYERS}
       \    Create    ${name}    ${id}


For-in-range loop
~~~~~~~~~~~~~~~~~

..
   Earlier for loops always iterated over a sequence, and this is also the most
   common use case. Sometimes it is still convenient to have a for loop
   that is executed a certain number of times, and Robot Framework has a
   special `FOR index IN RANGE limit` syntax for this purpose. This
   syntax is derived from the similar Python idiom.

이전의 FOR 루프는 항상 시퀀스 반복을 사용하였으며 가장 많이 사용한다.
때때로 특정 횟수로 실행되는 FOR 루프에서는 편리하다. 그래서
로봇프레임워크는 이러한 목적으로 특별한 `FOR index IN RANGE limit`
구문을 가진다. 이 구문은 유사 파이썬 이디엄에서 파생되었다.

..
   Similarly as other for loops, the for-in-range loop starts with
   `:FOR` and the loop variable is in the next cell. In this format
   there can be only one loop variable and it contains the current loop
   index. The next cell must contain `IN RANGE` and the subsequent
   cells loop limits.

다른 FOR 루프와 유사하게 for-in-range 루프는 `:FOR` 로 시작하고, 다음
셀에 루프 변수가 온다. 이 형태에서 단지 하나의 루프 변수가 존재할 수
있으며 그것은 현재의 루프 인덱스를 포함한다. 다음 셀은 반드시 `IN
RANGE` 를 포함해야 하며 이후 셀들은 루프를 제한한다.

..
   In the simplest case, only the upper limit of the loop is
   specified. In this case, loop indexes start from zero and increase by one
   until, but excluding, the limit. It is also possible to give both the
   start and end limits. Then indexes start from the start limit, but
   increase similarly as in the simple case. Finally, it is possible to give
   also the step value that specifies the increment to use. If the step
   is negative, it is used as decrement.

가장 간단한 경우는 단지 루프의 상한값을 기술한다. 이 경우 루프
인덱스는 0에서 시작하여 상한값(자신은 제외)까지 하나씩 증가한다.
시작과 상한값만 주는 것도 가능한다. 그러면 인덱스는 시작 값으로
시작하고, 간단한 경우와 처럼 증가한다. 마지막으로 증가를 사용하여
기술할때 증분값을 사용할 수 있다. 증분값이 음수면 감소로 사용된다.

..
   It is possible to use simple arithmetics such as addition and subtraction
   with the range limits. This is especially useful when the limits are
   specified with variables.

범위 제한에 더하기나 빼기 같은 간단한 산술이 가능한다. 이것은 범위 제한이
변수와 함께 기술 될때 특히 유용하다.

..
   Starting from Robot Framework 2.8.7, it is possible to use float values for
   lower limit, upper limit and step.

로봇프레임워크 2.8.7이후에는 하한값, 상한값, 증분값에 부동 소수점 값 사용이 가능하다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Only upper limit
       [Documentation]    Loops over values from 0 to 9
       :FOR    ${index}    IN RANGE    10
       \    Log    ${index}

   Start and end
       [Documentation]  Loops over values from 1 to 10
       :FOR    ${index}    IN RANGE    1    11
       \    Log    ${index}

   Also step given
       [Documentation]  Loops over values 5, 15, and 25
       :FOR    ${index}    IN RANGE    5    26    10
       \    Log    ${index}

   Negative step
       [Documentation]  Loops over values 13, 3, and -7
       :FOR    ${index}    IN RANGE    13    -13    -10
       \    Log    ${index}

   Arithmetics
       [Documentation]  Arithmetics with variable
       :FOR    ${index}    IN RANGE    ${var}+1
       \    Log    ${index}

   Float parameters
       [Documentation]  Loops over values 3.14, 4.34, and 5.34
       :FOR    ${index}    IN RANGE    3.14    6.09    1.2
       \    Log    ${index}


For-in-enumerate loop
~~~~~~~~~~~~~~~~~~~~~

..
   Sometimes it is useful to loop over a list and also keep track of your location
   inside the list.  Robot Framework has a special
   `FOR index ... IN ENUMERATE ...` syntax for this situation.
   This syntax is derived from the
   `Python built-in function <https://docs.python.org/2/library/functions.html#enumerate>`_.

때때로 리스트와 리스트내의 인덱스를 사용하여 루프를 구성하는 것은
유용하다. 로봇프레임워크은 이런 상황을 위해 특별한 `FOR index ... IN
ENUMERATE ...` 구문을 가진다. 이 구문은 `Python built-in function
<https://docs.python.org/2/library/functions.html#enumerate>`_ 에서
파생되었다.

..
   For-in-enumerate loops work just like regular for loops,
   except the cell after its loop variables must say `IN ENUMERATE`,
   and they must have an additional index variable before any other loop-variables.
   That index variable has a value of `0` for the first iteration, `1` for the
   second, etc.

For-in-enumerate 루프는 일반적인 FOR 루프와 비슷하다. 다만 루프 변수 이후에 `IN ENUMERATE` 가 
사용되고, 루프 변수는 다른 루프변수 전에 추가적인 인덱스 변수를 가진다. 인덱스 변수는 첫번째 반복에 `0` 
값을 가지고, 두번째에 `1` 을 가진다.

..
   For example, the following two test cases do the same thing:

예를 들어 다음 두가지 테스트 케이스는 동일하다:

.. sourcecode:: robotframework

   *** Variables ***
   @{LIST}         a    b    c

   *** Test Cases ***
   Manage index manually
       ${index} =    Set Variable    -1
       : FOR    ${item}    IN    @{LIST}
       \    ${index} =    Evaluate    ${index} + 1
       \    My Keyword    ${index}    ${item}

   For-in-enumerate
       : FOR    ${index}    ${item}    IN ENUMERATE    @{LIST}
       \    My Keyword    ${index}    ${item}

..
   Just like with regular for loops, you can loop over multiple values per loop
   iteration as long as the number of values in your list is evenly divisible by
   the number of loop-variables (excluding the first, index variable).

일반적인 FOR 루프와 비슷하게, 루프 당 루프 변수 수 만큼 리스트의 값을 균등하게 나누어
할당 할 수 있다.(첫번째 인덱스 변수는 제외)

.. sourcecode:: robotframework

   *** Test Case ***
   For-in-enumerate with two values per iteration
       :FOR    ${index}    ${english}    ${finnish}    IN ENUMERATE
       ...    cat      kissa
       ...    dog      koira
       ...    horse    hevonen
       \    Add to dictionary    ${english}    ${finnish}    ${index}

..
   For-in-enumerate loops are new in Robot Framework 2.9.

For-in-enumerate 루프는 로봇프레임워크 2.9에서 새롭게 도입되었다.

For-in-zip loop
~~~~~~~~~~~~~~~

..
   Some tests build up several related lists, then loop over them together.
   Robot Framework has a shortcut for this case: `FOR ... IN ZIP ...`, which
   is derived from the
   `Python built-in zip function <https://docs.python.org/2/library/functions.html#zip>`_.

일부 테스트는 여러 관련된 리스트를 구축하고 루프에서 함께 사용한다.
로봇프레임워크는 `FOR ... IN ZIP ...` 구문을 사용한다. 
이것은 `Python built-in zip function <https://docs.python.org/2/library/functions.html#zip>`_ 에서 파생했다.

..
   This may be easiest to show with an example:

예제를 보는 것이 가장 이해하기 쉽다:

.. sourcecode:: robotframework

   *** Variables ***
   @{NUMBERS}      ${1}    ${2}    ${5}
   @{NAMES}        one     two     five

   *** Test Cases ***
   Iterate over two lists manually
       ${length}=    Get Length    ${NUMBERS}
       : FOR    ${idx}    IN RANGE    ${length}
       \    Number Should Be Named    ${NUMBERS}[${idx}]    ${NAMES}[${idx}]

   For-in-zip
       : FOR    ${number}    ${name}    IN ZIP    ${NUMBERS}    ${NAMES}
       \    Number Should Be Named    ${number}    ${name}

..
   Similarly as for-in-range and for-in-enumerate loops, for-in-zip loops require
   the cell after the loop variables to read `IN ZIP`.

for-in-range 와 for-in-enumerate 루프와 비슷하게 for-in-zip 루프는 루프 변수 뒤의
셀에 `IN ZIP` 을 사용한다.

..
   Values used with for-in-zip loops must be lists or list-like objects, and
   there must be same number of loop variables as lists to loop over. Looping
   will stop when the shortest list is exhausted.

for-in-zip 루프에서 사용되는 값은 반드시 리스트나 리스트 형태의 객체여야 하고, 
루프변수의 개수와 동일해야 한다. 루프는 가장 짧은 리스트를 모두 소진했을 때 멈춘다.

..
   Note that any lists used with for-in-zip should usually be given as `scalar
   variables`_ like `${list}`. A `list variable`_ only works if its items
   themselves are lists.

for-in-zip에 사용된 리스트는 일반적으로 `${list}` 와 같이 `scalar variables`_ 로 주어진다.
`list variable`_ 은 해당 항목 자체가 리스트인 경우에만 동작한다.

..
   For-in-zip loops are new in Robot Framework 2.9.

For-in-zip 루프는 로봇프레임워크 2.9에서 새롭게 도입되었다.


Exiting for loop
~~~~~~~~~~~~~~~~

..
   Normally for loops are executed until all the loop values have been iterated
   or a keyword used inside the loop fails. If there is a need to exit the loop
   earlier,  BuiltIn_ keywords :name:`Exit For Loop` and :name:`Exit For Loop If`
   can be used to accomplish that. They works similarly as `break`
   statement in Python, Java, and many other programming languages.

보통 FOR 루프는 모든 루프 값이 반복되거나 루프 내에 사용된 키워드가 실패할때까지 실행된다.
만약 루프에서 일찍 종료하기를 원한다면, BuiltIn_ 키워드 :name:`Exit For Loop` 와
:name:`Exit For Loop If` 를 사용하라. 이 키워드는 파이썬, 자바  그외 다른 많은 프로그램 언어에서
사용되는 `break` 문과 유사하게 동작한다.

..
   :name:`Exit For Loop` and :name:`Exit For Loop If` keywords can be used
   directly inside a for loop or in a keyword that the loop uses. In both cases
   test execution continues after the loop. It is an error to use these keywords
   outside a for loop.

:name:`Exit For Loop` 와 :name:`Exit For Loop If` 키워드는 FOR 루프 안에서나 루프를 
사용하는 키워드 내에서 직접적으로 사용할 수 있다. 두 경우 모두 루프 이후에 계속해서 테스트를 수행한다.
FOR 루프 밖에서 이 키워드를 사용하면 에러가 발생한다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Exit Example
       ${text} =    Set Variable    ${EMPTY}
       :FOR    ${var}    IN    one    two
       \    Run Keyword If    '${var}' == 'two'    Exit For Loop
       \    ${text} =    Set Variable    ${text}${var}
       Should Be Equal    ${text}    one

..
   In the above example it would be possible to use :name:`Exit For Loop If`
   instead of using :name:`Exit For Loop` with :name:`Run Keyword If`.
   For more information about these keywords, including more usage examples,
   see their documentation in the BuiltIn_ library.

위의 예제에서 :name:`Run Keyword If` 와 name:`Exit For Loop` 대신 
:name:`Exit For Loop If` 를 사용할 수 있다. 더 많은 예제와 키워드에 대한 상세한 정보는 
BuiltIn_ 라이브러리의 문서를 참고하라.

..
   .. note:: :name:`Exit For Loop If` keyword was added in Robot Framework 2.8.

.. note:: :name:`Exit For Loop If` 키워드는 로봇프레임워크 2.8에 추가되었다.

Continuing for loop
~~~~~~~~~~~~~~~~~~~

..
   In addition to exiting a for loop prematurely, it is also possible to
   continue to the next iteration of the loop before all keywords have been
   executed. This can be done using BuiltIn_ keywords :name:`Continue For Loop`
   and :name:`Continue For Loop If`, that work like `continue` statement
   in many programming languages.

조기에 FOR 루프를 종료하는 것외에 모든 키워드가 수행되기 전에 루프의 다음 반복을 계속할 수도 있다.
이것은 다른 프로그래밍 언어의 `continue` 문처럼 동작하고,
BuiltIn_ 키워드 :name:`Continue For Loop` 와 :name:`Continue For Loop If` 를 사용한다.

..
   :name:`Continue For Loop` and :name:`Continue For Loop If` keywords can be used
   directly inside a for loop or in a keyword that the loop uses. In both cases
   rest of the keywords in that iteration are skipped and execution continues
   from the next iteration. If these keywords are used on the last iteration,
   execution continues after the loop. It is an error to use these keywords
   outside a for loop.

:name:`Continue For Loop` 와 :name:`Continue For Loop If` 키워드는 FOR 루프내에서 
또는 루프를 사용하는 키워드에서 직접적으로 사용될 수 있다. 두 경우 모두 반복내의 나머지 
키워드를 건너뛰고 다음 반복을 계속해서 수행한다. 만약 이 키워드가 마지막 반복에 사용된다면 
루프문 밖의 구문을 계속해서 수행한다. 루프 밖에서 이 키워드를 사용하면 에러가 발생한다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Continue Example
       ${text} =    Set Variable    ${EMPTY}
       :FOR    ${var}    IN    one    two    three
       \    Continue For Loop If    '${var}' == 'two'
       \    ${text} =    Set Variable    ${text}${var}
       Should Be Equal    ${text}    onethree

..
   For more information about these keywords, including usage examples, see their
   documentation in the BuiltIn_ library.

이 키워드에 대한 더 많은 정보 및 사용 예제는 BuiltIn_ 라이브러리 내의 문서를 참고하라.

..
   .. note::  Both :name:`Continue For Loop` and :name:`Continue For Loop If`
	      were added in Robot Framework 2.8.

.. note::  :name:`Continue For Loop` 와 :name:`Continue For Loop If` 은 
           로봇프레임워크 2.8에서 추가되었다.


Removing unnecessary keywords from outputs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   For loops with multiple iterations often create lots of output and
   considerably increase the size of the generated output_ and log_ files.
   Starting from Robot Framework 2.7, it is possible to `remove unnecessary
   keywords`__ from the outputs using :option:`--RemoveKeywords FOR` command line
   option.

..
   __ `Removing and flattening keywords`_

여러번 반복을 수행하는 FOR 루프는 많은 출력문을 생성하여 output_ 과 log_ 파일의 크기를 상당히 크게 만든다.
로봇프레임워크 2.7이후부터 명령행 라인의 :option:`--RemoveKeywords FOR` 을 사용하여 `remove unnecessary
keywords`__ 할 수 있다.

__ `Removing and flattening keywords`_

Repeating single keyword
~~~~~~~~~~~~~~~~~~~~~~~~

..
   For loops can be excessive in situations where there is only a need to
   repeat a single keyword. In these cases it is often easier to use
   BuiltIn_ keyword :name:`Repeat Keyword`.  This keyword takes a
   keyword and how many times to repeat it as arguments. The times to
   repeat the keyword can have an optional postfix `times` or `x`
   to make the syntax easier to read.

단지 하나의 키워드를 반복하는 상황에서 FOR 루프는 과하다. 이런 경우 BuiltIn_ 키워드 
:name:`Repeat Keyword` 를 사용하는 것이 더 쉽다. 이 키워드는 전달인자로 반복횟수와 
반복할 키워드를 받는다. 키워드의 반복횟수 구문은 좀 더 쉽게 읽히기 위해서 선택적으로 
접미어 `times` 나 `x` 를 가질 수 있다. 

.. sourcecode:: robotframework

   *** Test Cases ***
   Example
       Repeat Keyword    5    Some Keyword    arg1    arg2
       Repeat Keyword    42 times    My Keyword
       Repeat Keyword    ${var}    Another Keyword    argument

Conditional execution
---------------------

..
   In general, it is not recommended to have conditional logic in test
   cases, or even in user keywords, because it can make them hard to
   understand and maintain. Instead, this kind of logic should be in test
   libraries, where it can be implemented using natural programming
   language constructs. However, some conditional logic can be useful at
   times, and even though Robot Framework does not have an actual if/else
   construct, there are several ways to get the same effect.

일반적으로 테스트 케이스나 사용자 키워드에 조건 논리를 가지는 것은 추천되지 않는다.
이러한 것은 이해하기도 힘들고 유지보수 하기도 어렵게 만든다. 대신 이런 종류의 논리는
테스트 라이브러리 안에 존재해야 한다. 라이브러리는 프로그래밍 언어의 고유 문법으로 이런 
논리를 구현 할 수 있다. 그러나 어떤 조건식은 때로는 유용하다. 비록 로봇프레임워크가 
실질적인 if/else 구문을 가지고 있지는 않지만, 동일한 효과를 얻기위해 다양한 방법이 존재한다.

..
   - The name of the keyword used as a setup or a teardown of both `test
     cases`__ and `test suites`__ can be specified using a
     variable. This facilitates changing them, for example, from
     the command line.

- `test cases`__ 와 `test suites`__ 양쪽 모두의 setup 또는 teardown에 사용되는
  키워드 이름은 변수를 사용하여 기술 할 수 있다. 이렇게 하면 커맨드 라인에서 변수를 변경할 수 있다.

..
   - The BuiltIn_ keyword :name:`Run Keyword` takes a keyword to actually
     execute as an argument, and it can thus be a variable. The value of
     the variable can, for example, be got dynamically from an earlier
     keyword or given from the command line.
  
- BuiltIn_ 키워드 :name:`Run Keyword` 는 전달인자로 키워드를 받아서 실행한다. 그래서 
  경우에 따라서 전달인자는 변수가 될 수 있다. 변수의 값은 이전 키워드에서 동적으로 얻거나 
  명령행 라인에서 주어질 수 있다.  

..
   - The BuiltIn_ keywords :name:`Run Keyword If` and :name:`Run Keyword
     Unless` execute a named keyword only if a certain expression is
     true or false, respectively. They are ideally suited to creating
     simple if/else constructs. For an example, see the documentation of
     the former.
  
- BuiltIn_ 키워드 :name:`Run Keyword If` 와 :name:`Run Keyword Unless` 는 
  주어진 식이 각각 참 또는 거짓일 때만 주어진 키워드를 실행한다. 그리고 이상적으로 
  간단한 if/else 구조를 만드는데 적당하다.

..
   - Another BuiltIn_ keyword, :name:`Set Variable If`, can be used to set
     variables dynamically based on a given expression.
  
- 다른 BuiltIn_ 키워드 :name:`Set Variable If` 는 동적으로 주어진 식에 기초하여 
  변수에 값을 설정하는데 사용할 수 있다.

..
   - There are several BuiltIn_ keywords that allow executing a named
     keyword only if a test case or test suite has failed or passed.
  
- 여러 BuiltIn_ 키워드는 테스트 케이스나 테스트 스위트가 실패하거나 성공할 경우에만 
  키워드를 수행한다.   

..
   __ `Test setup and teardown`_
   __ `Suite setup and teardown`_

__ `Test setup and teardown`_
__ `Suite setup and teardown`_

Parallel execution of keywords
------------------------------

..
   When parallel execution is needed, it must be implemented in test library
   level so that the library executes the code on background. Typically this
   means that the library needs a keyword like :name:`Start Something` that
   starts the execution and returns immediately, and another keyword like
   :name:`Get Results From Something` that waits until the result is available
   and returns it. See OperatingSystem_ library keywords :name:`Start Process`
   and :name:`Read Process Output` for an example.


병령 실행이 필요한 경우에는 반드시 테스트 라이브러리 수준에서 구현되어야 하면 라이브러리는 
백그라운드로 코드를 수행한다. 전형적으로 이러한 라이브러리가 의미하는 바는 수행 시작하고
즉시 리턴하는 :name:`Start Something` 과 같은 키워드를 필요로 한다. 다른 키워드
name:`Get Results From Something` 는 결과가 유효할때까지 기다렸다가 그것을 리턴한다.
예로, OperatingSystem_ 라이브러리 키워드 :name:`Start Process` 와 :name:`Read Process Ouput`
을 참고하라.
