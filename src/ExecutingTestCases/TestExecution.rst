Test execution
==============

This section describes how the test suite structure created from the parsed
test data is executed, how to continue executing a test case after failures,
and how to stop the whole test execution gracefully.

이번 섹션에서는 어떻게 파싱한 테스트 데이터로 만들어진 테스트 스위트 구조가 구동되는지,
어떻게 테스트 케이스 실패 후 계속해서 수행하는지, 어떻게 전체 테스트 수행을 중단시키는지 살펴본다.

.. contents::
   :depth: 2
   :local:

Execution flow
--------------

Executed suites and tests
~~~~~~~~~~~~~~~~~~~~~~~~~

Test cases are always executed within a test suite. A test suite
created from a `test case file`_ has tests directly, whereas suites
created from directories__ have child test suites which either have
tests or their own child suites. By default all the tests in an
executed suite are run, but it is possible to `select tests`__ using
options :option:`--test`, :option:`--suite`, :option:`--include` and
:option:`--exclude`. Suites containing no tests are ignored.

테스트 케이스는 항상 테스트 스위트 안에서 수행된다.
`test case file`_ 에 의해 생성된 테스트 스위트는 직접 테스트를 갖는다.
반면에 directories__ 로 부터 생성된 스위트는 테스트 혹은 그것의 하위 스위트를 갖는 하위 테스트 스위트를 갖는다.
디폴트로 수행된 스위트의 모든 테스트는 작동하긴 한다.
하지만 :option:`--test`, :option:`--suite`, :option:`--include`, :option:`--exclude` 를 이용해서 `select tests`__ 할 수도 있다.
테스트를 포함하지 않은 스위트는 무시된다.

The execution starts from the top-level test suite. If the suite has
tests they are executed one-by-one, and if it has suites they are
executed recursively in depth-first order. When an individual test
case is executed, the keywords it contains are run in a
sequence. Normally the execution of the current test ends if any
of the keywords fails, but it is also possible to
`continue after failures`__. The exact `execution order`_ and how
possible `setups and teardowns`_ affect the execution are discussed
in the following sections.


수행은 상위 레벨 테스트 스위트부터 시작한다.
만일 스위트을 가진 경우 하나 하나 테스트를 수행하고, 스위트가 또 스위트를 가진 경우 재귀적으로 깊이 우선 순서로 수행하게 된다.
각각의 테스트 케이스가 수행될 때, 테스트에 포함된 키워드들이 순서대로 동작한다.
일반적으로 어떤 키워드가 실패한 경우, 현재 테스트의 수행이 종료된다. 하지만 `continue after failures`__ 도 가능하다.
정확한 `execution order`_ 그리고 어떻게 `setups and teardowns`_ 가 수행에 영향을 줄지는 다음 섹션에서 논의한다.

__ `Test suite directories`_
__ `Selecting test cases`_
__ `Continue on failure`_
__ `Test suite directories`_
__ `Selecting test cases`_
__ `Continue on failure`_


Setups and teardowns
~~~~~~~~~~~~~~~~~~~~

Setups and teardowns can be used on `test suite`__, `test case`__ and
`user keyword`__ levels.

__ `Test setup and teardown`_
__ `Suite setup and teardown`_
__ `User keyword teardown`_

Suite setup
'''''''''''

If a test suite has a setup, it is executed before its tests and child
suites. If the suite setup passes, test execution continues
normally. If it fails, all the test cases the suite and its child
suites contain are marked failed. The tests and possible suite setups
and teardowns in the child test suites are not executed.

만약 테스트 스위트가 setup을 가지고 있다면, 테스트와 하위 스위트 전에 수행된다.
만약 스위트 setup이 통과하면, 테스트 수행은 계속될 것이다.
만약 실패한다면, 모든 테스트 케이스 스위트와 이것의 하위 스위트는 실패로 표시될 것이다.
하위 테스트 스위트의 테스트와 스위트 setup과 teardown은 수행되지 않는다.

Suite setups are often used for setting up the test environment.
Because tests are not run if the suite setup fails, it is easy to use
suite setups for verifying that the environment is in state in which the
tests can be executed.

suite setup은 항상 테스트 환경 셋업을 위해 사용된다.
왜냐하면 테스트는 suite setup이 실패하면 작동하지 않기 때문에, 환경이 테스트 수행할 수 있는 상태인지 확인하기 용이하다.

Suite teardown
''''''''''''''

If a test suite has a teardown, it is executed after all its test
cases and child suites. Suite teardowns are executed regardless of the
test status and even if the matching suite setup fails. If the suite
teardown fails, all tests in the suite are marked failed afterwards in
reports and logs.

만일 테스트 스위트가 teardown을 가지고 있다면, 모든 테스트 케이스와 하위 스위트 수행 후 수행된다.
suite teardown은 테스트가 실패하더라도, 상태에 상관없이 수행된다.
만일 suite teardown이 실패한다면, 스위트의 모든 테스트는 리포트와 로그에 실패했다고 표기될 것이다.

Suite teardowns are mostly used for cleaning up the test environment
after the execution. To ensure that all these tasks are done, `all the
keywords used in the teardown are executed`__ even if some of them
fail.

__ `Continue on failure`_

suite teardown은 대개 테스트 수행 후 환경을 깨끗이 하는데에 사용된다.
모든 동작이 끝났음을 확인하기 위하여 `all the keywords used in the teardown are executed`__ 비록 그것들 중 일부가 실패했을 지라도.

__ `Continue on failure`_

Test setup
''''''''''

Possible test setup is executed before the keywords of the test case.
If the setup fails, the keywords are not executed. The main use
for test setups is setting up the environment for that particular test
case.

test setup은 테스트 케이스의 키워드 전에 동작한다.
만일 setup이 실패한다면, 키워드는 수행하지 않게된다.
test setup의 주요 사용은 특정 테스트 케이스의 수행 환경을 구성하는 것이다.

Test teardown
'''''''''''''

Possible test teardown is executed after the test case has been
executed. It is executed regardless of the test status and also
if test setup has failed.

test teardown은 테스트 케이스 수행 후에 수행된다.
테스트 상태에 상관 없이, test setup이 실패할지라도 수행된다.

Similarly as suite teardown, test teardowns are used mainly for
cleanup activities. Also they are executed fully even if some of their
keywords fail.

suite teardown과 비슷하게, test teardown은 주로 그동안의 동작했던 것을 정리하기 위해 사용한다.
키워드가 실패하더라도 그것들은 모두 수행이 될 것이다.

Keyword teardown
''''''''''''''''

`User keywords`_ cannot have setups, but they can have teardowns that work
exactly like other teardowns. Keyword teardowns are run after the keyword is
executed otherwise, regardless the status, and they are executed fully even
if some of their keywords fail.

`User keywords`_ 는 setup을 가질 수 없다. 하지만 그것들은 다른 teardwon 처럼 teardown을 가질 수 있다.
키워드 teardown은 키워드가 수행된 다음에 수행된다.
상태와 상관없이, 그것들은 키워드가 실패할지라도 모두 수행될 것이다.

Execution order
~~~~~~~~~~~~~~~

Test cases in a test suite are executed in the same order as they are defined
in the test case file. Test suites inside a higher level test suite are
executed in case-insensitive alphabetical order based on the file or directory
name. If multiple files and/or directories are given from the command line,
they are executed in the order they are given.

테스트 스위트의 테스트 케이스는 테스트 케이스 파일에서 정의된것과 같은 순서대로 수행된다.
상위 레벨의 테스트 스위트 내부의 테스트 스위트는 대소문자 구분을 하고, 파일이나 디렉토리 이름의 순서대로 수행된다.
만일 여러개의 파일이나 디렉토리가 command line에서 주어진다면, 주어진 순서대로 수행될 것이다.

If there is a need to use certain test suite execution order inside a
directory, it is possible to add prefixes like :file:`01` and
:file:`02` into file and directory names. Such prefixes are not
included in the generated test suite name if they are separated from
the base name of the suite with two underscores

만일 디렉토리 내부의 테스트 스위트 수행 순서를 사용할 필요가 있다면, 파일과 디렉토리 이름에 :file:`01` 와:file:`02`
같이 말머리를 추가할 수 있다::

   01__my_suite.html -> My Suite
   02__another_suite.html -> Another Suite

If the alphabetical ordering of test suites inside suites is
problematic, a good workaround is giving them separately in the
required order. This easily leads to overly long start-up commands,
but `argument files`_ allow listing files nicely one file per line.

스위트 안의 테스트 스위트를 만일 알파벳 순서대로 나열하는것에 문제가 있다면, 좋은 해결책은 필수적인 순서로 별도로 수행하는 것이 좋다.
긴 수동 명령으로 유도할 수 있다. 하지만 `argument files`_ 는 라인당 한 파일만 나열할 수 있도록 한다.

It is also possible to `randomize the execution order`__ using
the :option:`--randomize` option.

__ `Randomizing execution order`_

:option:`--randomize` 옵션을 이용하여 `randomize the execution order`__ 할 수 있다.

__ `Randomizing execution order`_

Passing execution
~~~~~~~~~~~~~~~~~

Typically test cases, setups and teardowns are considered passed if
all keywords they contain are executed and none of them fail. From
Robot Framework 2.8 onwards, it is also possible to use BuiltIn_ keywords
:name:`Pass Execution` and :name:`Pass Execution If` to stop execution with
PASS status and skip the remaining keywords.

일반적으로 테스트 케이스, setup, teardown은 모든 키워드가 실패 없이 수행된다면 성공으로 간주된다.
로봇 프레임워크 2.8부터, BuiltIn_ 키워드 :name:`Pass Execution` 와 :name:`Pass Execution If` 를 PASS 상태로 설정 후
남은 키워드는 스킵하고 수행을 중지하는 데에 이용할 수 있다.

How :name:`Pass Execution` and :name:`Pass Execution If` behave
in different situations is explained below:

어떻게 :name:`Pass Execution` 와 :name:`Pass Execution If` 가 다른 상황에서 수행하는지 아래에서 설명한다.

- When used in any `setup or teardown`__ (suite, test or keyword), these
  keywords pass that setup or teardown. Possible teardowns of the started
  keywords are executed. Test execution or statuses are not affected otherwise.

- `setup or teardown`__ (스위트, 테스트, 키워드)에서 사용될 때, 이 키워드들은 setup이나 teardown을 통과한다.
  시작된 키워드의 teardown은 수행된다. 테스트 수행이나 상태는 영향을 받지 않는다.

- When used in a test case outside setup or teardown, the keywords pass that
  particular test case. Possible test and keyword teardowns are executed.

- setup 이나 teardown 외의 테스트 케이스에서 사용될 때, 키워드는 특정 테스트 케이스르 통과한다.
  테스트와 키워드 teardown이 수행된다.

- Possible `continuable failures`__ that occur before these keyword are used,
  as well as failures in teardowns executed afterwards, will fail the execution.

- `continuable failures`__ 는 키워드가 사용되기 전에 발생한 것과 후에 teardown에서 실패한 것은 수행 실패한다.

- It is mandatory to give an explanation message
  why execution was interrupted, and it is also possible to
  modify test case tags. For more details, and usage examples, see the
  `documentation of these keywords`__.

- 왜 수행이 방해받았는지 설명 메시지를 의무적으로 제공해야한다. 그리고 테스트 케이스 태그로 표기하는 것도 가능하다.
  자세한 사용예는, `documentation of these keywords`__ 를 참고하라.

Passing execution in the middle of a test, setup or teardown should be
used with care. In the worst case it leads to tests that skip all the
parts that could actually uncover problems in the tested application.
In cases where execution cannot continue do to external factors,
it is often safer to fail the test case and make it `non-critical`__.

__ `Setups and teardowns`_
__ `Continue on failure`_
__ `BuiltIn`_
__ `Setting criticality`_

테스트, setup, teardown 의 중간에 수행 성공하는 것은 주의가 필요하다.
최악의 경우에는 테스트된 어플에서 문제를 발견하지 못하고 모든 부분을 스킵하게 될 수 있다.
수행이 외부적인 요인에 의해 지속되지 못한 경우, 테스트 케이스를 실패시키고 `non-critical`__ 로 만드는 것이 더 안전하다.

__ `Setups and teardowns`_
__ `Continue on failure`_
__ `BuiltIn`_
__ `Setting criticality`_

Continue on failure
-------------------

Normally test cases are stopped immediately when any of their keywords
fail. This behavior shortens test execution time and prevents
subsequent keywords hanging or otherwise causing problems if the
system under test is in unstable state. This has the drawback that often
subsequent keywords would give more information about the state of the
system. Hence Robot Framework offers several features to continue after
failures.

일반적으로 테스트 케이스는 키워드가 실패했을 때 즉시 멈춘다. 이 동작은 테스트 수행 시간을 줄여주고 연속된 키워드의 실패 혹은
테스트가 수행되는 시스템이 불안정한 경우 문제를 야기하는 것을 막아준다.
종종 연속된 키워드가 시스템의 상태에 대한 많은 정보를 가지고 있다면 문제가 되기도 한다.
그러므로 로봇 프레임워크는 실패 이후 계속 수행에 대한 몇몇 특징을 갖는다.

:name:`Run Keyword And Ignore Error` and :name:`Run Keyword And Expect Error` keywords
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

BuiltIn_ keywords :name:`Run Keyword And Ignore Error` and :name:`Run
Keyword And Expect Error` handle failures so that test execution is not
terminated immediately. Though, using these keywords for this purpose
often adds extra complexity to test cases, so the following features are
worth considering to make continuing after failures easier.

BuiltIn_ 키워드 :name:`Run Keyword And Ignore Error` 와 :name:`Run Keyword And Expect Error` 는 테스트 수행이 즉시 종료되지
못하도록 실패를 다룬다. 그럼에도, 이런 목적으로 키워드를 사용하는 것은 테스트 케이스에 복잡성을 키운다.
다음의 특징은 실패 이후에 지속 수행하는 것을 쉽게 도와준다.

Special failures from keywords
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

`Library keywords`_ report failures using exceptions, and it is
possible to use special exceptions to tell the core framework that
execution can continue regardless the failure. How these exceptions
can be created is explained in the `test library API chapter`__.

`Library keywords`_ 예외처리를 이용하여 실패를 리포트한다. 실패 여부와 상관없이 수행을 지속할 수 있는지 핵심 프레임워크를 언급하기
위하여 특별한 예외처리를 사용한다. 어떻게 이 예외가 생성되는지 `test library API chapter`__ 에서 설명한다.

When a test ends and there has been one or more continuable failure,
the test will be marked failed. If there are more than one failure,
all of them will be enumerated in the final error message

테스트가 끝나고 하나 이상의 연속적인 실패가 있는 경우, 테스트는 실패로 표기될 것이다.
만일 하나 이상의 실패가 있는경우, 모두는 최종 에러 메시지에 나열될 것이다::

  Several failures occurred:

  1) First error message.

  2) Second error message ...

  몇몇 실패들이 발생한다:

  1) 첫번째 에러 메시지.

  2) 두번째 에러 메시지 ...

Test execution ends also if a normal failure occurs after continuable
failures. Also in that case all the failures will be listed in the
final error message.

연속적인 실패 이후 일반적인 실패가 발생한 경우 테스트 수행이 끝난다.
케이스에서 모든 실패는 최종 실패 메시지에 나열될 것이다.

The return value from failed keywords, possibly assigned to a
variable, is always the Python `None`.

__ `Continuing test execution despite of failures`_

실패한 키워드로부터의 리턴 값은, 변수에 할당되고, 항상 파이썬의 `None` 값이다.

__ `Continuing test execution despite of failures`_

:name:`Run Keyword And Continue On Failure` keyword
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

BuiltIn_ keyword :name:`Run Keyword And Continue On Failure` allows
converting any failure into a continuable failure. These failures are
handled by the framework exactly the same way as continuable failures
originating from library keywords.

BuiltIn_ 키워드 :name:`Run Keyword And Continue On Failure` 는 실패를 지속적인 실패로 변환할 수 있게 한다.
실패는 프레임워크에 의해 라이브러리 키워드에 근거한 지속적인 실패와 완전히 같게 다루어진다.

Execution continues on teardowns automatically
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To make it sure that all the cleanup activities are taken care of, the
continue on failure mode is automatically on in `test and suite
teardowns`__. In practice this means that in teardowns all the
keywords in all levels are always executed.

__ `Setups and teardowns`_

모든 클린업 활동은 주의를 기울여야 한다. 계속해서 실패하는 경우 자동으로 `test and suite teardowns`__ 을 수행해야 한다.
실제로 teardown의 모든 레벨의 모든 키워드는 항상 수행된다.

__ `Setups and teardowns`_

All top-level keywords are executed when tests have templates
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When using `test templates`_, all the data rows are always executed to
make it sure that all the different combinations are tested. In this
usage continuing is limited to the top-level keywords, and inside them
the execution ends normally if there are non-continuable failures.

`test templates`_ 을 사용할 때, 모든 데이터 열은 항상 모든 다른 결합이 테스트 된다는 것을 확실히 하기 위하여 수행된다.
이런 사용은 top-level 키워드에게는 제한적이다. 그리고 지속적이지 않은 실패가 생겼을 때 내부에서 수행 종료된다.

Stopping test execution gracefully
----------------------------------

Sometimes there is a need to stop the test execution before all the tests
have finished, but so that logs and reports are created. Different ways how
to accomplish this are explained below. In all these cases the remaining
test cases are marked failed.

때때로 모든 테스트가 끝나기 전에 테스트 수행을 멈춰야 할 때가 있다. 하지만 로그와 리포트는 생성된다.
어떻게 이것들을 수행할 수 있는지는 아래에서 설명한다. 이런 케이스에 남은 테스트 케이스들은 실패로 표기된다.

Starting from Robot Framework 2.9 the tests that are automatically failed get
`robot-exit` tag and the generated report will include `NOT robot-exit`
`combined tag pattern`__ to easily see those tests that were not skipped. Note
that the test in which the exit happened does not get the `robot-exit` tag.

__ `Generating combined tag statistics`_

로봇 프레임워크 2.9부터 테스트는 자동적으로 실패한다. ★
`robot-exit` 태그와 생성된 리포트는 `NOT robot-exit` 를 포함하고 스킵되지 않은 테스트를 쉽게 보기 위해 `combined tag pattern`__ 한다.

__ `Generating combined tag statistics`_

Pressing `Ctrl-C`
~~~~~~~~~~~~~~~~~

The execution is stopped when `Ctrl-C` is pressed in the console
where the tests are running. When running the tests on Python, the
execution is stopped immediately, but with Jython it ends only after
the currently executing keyword ends.

테스트가 진행중일 때 콘솔 창에서 `Ctrl-C` 를 누르면 동작은 멈춘다.
Python 기반으로 테스트가 진행 중일 때, 동작은 즉시 멈추지만, Jython 기반인 경우 수행중인 키워드가 끝난 뒤에 종료된다.

If `Ctrl-C` is pressed again, the execution ends immediately and
reports and logs are not created.

만일 `Ctrl-C` 가 다시한번 눌린다면, 수행은 즉시 멈추고 리포트와 로그도 생성되지 않는다.

Using signals
~~~~~~~~~~~~~

On Unix-like machines it is possible to terminate test execution
using signals `INT` and `TERM`. These signals can be sent
from the command line using ``kill`` command, and sending signals can
also be easily automated.

Unix와 유사한 머신에서 테스트 수행을 종료하기 위해서 시그널 `INT` 와 `TERM` 를 사용한다.
이 시그널은 command line에서 ``kill`` 을 수행해서 보낼 수 있다. 그리고 보내진 신호는 쉽게 자동화된다.

Signals have the same limitation on Jython as pressing `Ctrl-C`.
Similarly also the second signal stops the execution forcefully.

Jython 에서 시그널은 `Ctrl-C` 을 누르는 것에 있어 같은 제한이 있다.
비슷하게 두번째 신호는 수행을 강제로 종료시킨다.

Using keywords
~~~~~~~~~~~~~~

The execution can be stopped also by the executed keywords. There is a
separate :name:`Fatal Error` BuiltIn_ keyword for this purpose, and
custom keywords can use `fatal exceptions`__ when they fail.

__ `Stopping test execution`_

수행되는 키워드를 이용하여 수행은 정지할 수 있다. 이것을 위해 :name:`Fatal Error` 라는 BuiltIn_ 키워드를 사용할 수 있다.
커스텀 키워드 `fatal exceptions`__ 도 실패했을 때 사용할 수 있다.

__ `Stopping test execution`_

Stopping when first test case fails
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If option :option:`--exitonfailure` is used, test execution stops
immediately if any `critical test`_ fails. Also the remaining tests
are marked as failed.

:option:`--exitonfailure` 옵션이 사용된다면, `critical test`_ 가 실패 하자마자 테스트 수행은 즉시 멈춘다.
또한 남아있는 테스트는 실패한것으로 표시된다.

Stopping on parsing or execution error
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Robot Framework separates *failures* caused by failing keywords from *errors*
caused by, for example, invalid settings or failed test library imports.
By default these errors are reported as `test execution errors`__, but errors
themselves do not fail tests or affect execution otherwise. If
:option:`--exitonerror` option is used, however, all such errors are considered
fatal and execution stopped so that remaining tests are marked failed. With
parsing errors encountered before execution even starts, this means that no
tests are actually run.

.. note:: :option:`--exitonerror` is new in Robot Framework 2.8.6.

__ `Errors and warnings during execution`_

로봇 프레임워크는  *errors* 에 의해 실패한 키워드와 *failures* 에 의해 실패한 키워드를 구분한다.
예를들어 유효하지 않은 설정이나 테스트 라이브러리 임포트에 실패했을 때 발생한다.
디폴트로 이 에러는 `test execution errors`__ 로 보고된다. 하지만 에러자체는 테스트를 실패하게 하거나 수행에 영향을 주지 않는다.
하지만, 만약 옵션 :option:`--exitonerror` 가 사용된다면, 그런 모든 에러는 치명적인 것으로 여겨지고,
수행은 멈춰서 실패한 것으로 표기될 것이다.
파싱 에러는 수행 시작 전에 마주할 수 있다. 즉 아무런 테스트도 실제로 수행되지 않는 다는 것이다.

.. note:: :option:`--exitonerror` 는 로봇 프레임워크 2.8.6 부터 지원된다.

__ `Errors and warnings during execution`_

Handling teardowns
~~~~~~~~~~~~~~~~~~

By default teardowns of the tests and suites that have been started are
executed even if the test execution is stopped using one of the methods
above. This allows clean-up activities to be run regardless how execution
ends.

시작된 테스트와 스위트의 디폴트 teardown은 위의 방법중 하나로 정지되었다고 하더라도 수행된다.
이것이 어떻게 수행이 종료되었던 상관 없이 clean-up 되는 활동이다.

It is also possible to skip teardowns when execution is stopped by using
:option:`--skipteardownonexit` option. This can be useful if, for example,
clean-up tasks take a lot of time.

:option:`--skipteardownonexit` 옵션을 이용하여 수행이 정지되었을 때 teardown도 스킵할 수 있다.
예를들어 clean-up 태스크가 많은 시간을 필요로 한다면 유용하다.
